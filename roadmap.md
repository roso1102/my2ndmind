## MySecondMind Roadmap

This roadmap combines the vision in `README.md` with the concrete ideas in `todo.md`. It is organized as:

- **MVP**: smallest usable vertical slice.
- **Phase 1–4**: progressive deepening of intelligence, UX, and scale.

---

## MVP – Core Personal Memory Engine (Vertical Slice)

**Goal**: A self-hosted system that lets you save memories (text + links) and query them in simple English, primarily through a Telegram bot, plus a minimal web view for reflection.

**Scope**

- **Chat-Based Ingestion and Retrieval (Telegram)**
  - Create a **Telegram bot** via BotFather and connect it to the backend using the Telegram Bot API.
  - Use Telegram as the **primary input and query channel**:
    - Free-form English messages for saving notes, links, ideas, and tasks.
    - Free-form English questions for retrieval (“What was that FPGA ASCON idea?”).
  - Backend intent handling:
    - Infer whether a message is **ingestion** (store a new memory) or **retrieval** (query existing memories).
    - Route ingestion to the memory pipeline; route retrieval to the semantic search pipeline and reply in chat.
  - For each stored item:
    - Store `raw_content`, `content_type`, `title`, `summary`, `tags` (simple LLM-generated), `created_at`.
- **Embeddings + Storage**
  - Run a local embedding model (e.g., `bge-small` or `nomic-embed-text`).
  - Store embeddings in **PostgreSQL + pgvector**.
  - Implement a basic `memories` table with the fields from `README.md` that are needed for MVP.
- **Semantic Retrieval**
  - Natural-language search via Telegram:
    - Chat message → backend → embed query → pgvector similarity search → LLM rerank → reply with top N results and short explanations.
  - Optional minimal web search UI (text box) for debugging and manual exploration.
- **Basic Daily Reflection (Manual or Cron)**
  - Once per day (or on button click), compute:
    - Count of memories created today.
    - Simple topic breakdown from tags (e.g., top 3 tags + percentages).
  - Render as a small text report on the dashboard.
- **Infrastructure**
  - FastAPI backend, React/Next.js frontend (for dashboard + reflections).
  - Local LLM (Ollama/llm.cpp) used for:
    - Summarization.
    - Tag suggestion.
  - Simple `.env`-based config, single-user, local deployment.

**Out of scope for MVP**

- Images, complex tasks, tunnels, advanced analytics, multi-channel capture, and heavy security hardening (beyond basic sanitization).

---

## Phase 1 – Robust Ingestion, Orchestration, and Safety

**Goal**: Turn ingestion into a flexible, secure orchestration layer and lay foundations for tasks and deadlines.

**Key Themes**

- **Security and Safety**
  - Safeguard from phishing links or exposing anything.
  - Add basic **prompt-injection** and **script-injection** protections:
    - Sanitize HTML/JS when ingesting web content.
    - Add guardrails to LLM prompts to avoid untrusted tool execution.
- **Orchestration Layer**
  - Introduce an explicit **orchestration module** that decides “Which tool should I call?” based on input type:
    - If **link** → call metadata extractor (title, description, favicon screenshot if desired).
    - If **image** (Phase 1 can stub this or accept but not fully process).
    - If **task** → call basic reminder/task handler.
    - If **research** → generate richer summary + tags.
  - Extend `content_type` to include: `note`, `link`, `task`, `research`, etc.
- **Deadline Detection Agent (v1)**
  - Implement a small agent/function that:
    - Extracts dates from text (LLM or rule-based hybrid).
    - Confirms/normalizes timezone.
    - Stores `deadline` field on relevant memories.
  - For now, reminders can be:
    - Simple list of upcoming deadlines on the dashboard.
- **UX Refinements**
  - Clean, modern web UI:
    - Ingestion form with type hints (note/link/task).
    - Basic list view of recent memories with filters.

---

## Phase 2 – Temporal Intelligence, Resurfacing Engine, Analytics, and WhatsApp

**Goal**: Move from static storage to **time-aware cognition** with resurfacing and richer reflections, and add WhatsApp as a second chat channel.

**Key Themes**

- **Memory Intelligence Layer (v1)**
  - Implement logic that decides which memories are important to resurface using:
    - `priority_score`.
    - `tags`.
    - `recency` (`created_at`, `last_accessed`).
    - User behavior (clicks, feedback signals once available).
- **Resurfacing Engine**
  - Implement the `resurface_score` formula from `README.md`:
    - Combine `priority_weight`, `aging_factor`, `semantic_relevance_to_recent_activity`, and `resurfaced_penalty`.
  - Scheduler (cron) job:
    - Query eligible older memories.
    - Compute resurfacing scores.
    - Select top N and surface them on:
      - Dashboard home (“Today’s resurfaced items”).
- **Deadline Detection Agent (v2)**
  - Use deadlines to:
    - Highlight **deadline pressure** on the dashboard.
    - Influence resurfacing scores for items with approaching deadlines.
- **Reflective Analytics (v1)**
  - Nightly summary job:
    - Fetch items created that day.
    - Analyze **topic distribution** using tags.
    - Display:
      - Simple breakdown (e.g., “60% Hardware Security, 25% GATE, 15% Misc”).
      - Short textual reflection (“Trend: Increasing convergence between X and Y” via LLM).
  - UI:
    - Add a “Daily Reflection” page with a list of past days.
  - Chat surfacing:
    - Send a short **daily digest** (reflection + topic distribution) as a Telegram message.
    - Include links or short summaries of top resurfaced items.
- **WhatsApp Integration (Input + Retrieval)**
  - Add **WhatsApp** as an additional ingestion + retrieval channel using Baileys (or a similar library).
  - Reuse the same intent-handling and semantic retrieval logic:
    - Natural-language messages in WhatsApp should behave like Telegram: save memories and answer queries in simple English.
  - Ensure chat security and access control so only the owner can talk to their second mind.

- **Notification System (Chat-Based)**
  - Implement a notification pipeline that can:
    - Send **daily digest** messages (summary of the day’s memories and focus).
    - Deliver **resurfacing prompts** as proactive chat messages.
    - Trigger **reminders** based on deadlines detected by the Deadline Detection Agent.
  - Support Telegram first; extend the same behaviors to WhatsApp once integrated.

---

## Phase 3 – Tunnels and Learning Feedback Loops

**Goal**: Introduce **Tunnels** as dynamic semantic pathways and make the system learn from your feedback.

**Key Themes**

- **Persistent Memory Foundations**
  - Ensure:
    - Stable `embedding_vector` storage for all memories.
    - Efficient retrieval of memories over longer time ranges.
- **Tunnels: Dynamic Semantic Pathways (v1)**
  - Implement fundamental tunnel model:
    - Periodically cluster embeddings (k-means/hierarchical over all or recent memories).
    - Identify **stable clusters** over time (size + temporal spread).
    - For each cluster:
      - Create a `Tunnel` with:
        - `theme_vector` (centroid).
        - List of member memories (TunnelMembership).
        - Basic metadata: `created_at`, `last_updated_at`.
      - Use LLM to propose a **human-readable name** from representative memories (e.g., “Hardware-Accelerated AI Security Tunnel”).
  - Tunnel UI:
    - Tunnels list page showing:
      - Name, # of memories, last activity time.
    - Tunnel detail page:
      - Chronological list of associated memories.
- **Tunnel Intelligence (v1)**
  - Track per-tunnel stats:
    - Growth rate (new memories per week).
    - Dormancy (time since last new memory).
    - Deadline pressure (earliest deadlines among member memories).
  - Use these stats to:
    - Influence resurfacing (e.g., highlight active or deadline-heavy tunnels).
- **Feedback Loop and Learning**
  - Allow user feedback on resurfaced items:
    - “Relevant / not relevant”.
    - “Snooze”.
    - “Archive”.
  - Use feedback to:
    - Adjust `priority_score` or a separate `usefulness_score`.
    - Improve future resurfacing and potentially tunnel membership weight.

---

## Phase 4 – Multi-Channel Capture, Knowledge Graph, and Advanced Analytics

**Goal**: Reduce friction in capture, add structure with a lightweight knowledge graph, and expose deep cognitive analytics.

**Key Themes**

- **Capture from Multiple Channels**
  - Add integrations for:
    - Browser extension or bookmarklet to send current tab as a memory.
    - Email-to-memory address (forward emails to ingest).
    - Simple CLI or additional chat-like interfaces for quick dumps (beyond the core Telegram + WhatsApp channels).
  - Ensure all channels go through the same orchestration + security + memory pipeline.
- **Early, Pragmatic Knowledge Graph**
  - Add light entity extraction:
    - People, projects, tools, concepts.
  - Store entities and relationships as structured tags/relations.
  - Views:
    - “All memories related to \<Person X\>”.
    - “All memories related to \<Project Y\>”.
  - Use entity links alongside tunnels to improve:
    - Cross-domain synthesis.
    - Navigation between related ideas.
- **Advanced Tunnel Visualization and Analytics**
  - Visualization:
    - Timeline graphs per tunnel (activity over time).
    - Branching maps (sub-clusters within a tunnel).
    - Density heatmaps (when thinking about this topic peaks).
    - Priority meters and convergence indicators between tunnels.
  - Personal metrics and goals:
    - Allow user-defined focus goals (e.g., “40% GATE prep this month”).
    - Compare **actual topic distribution vs goals**.
    - Show trends in attention drift and convergence across tunnels.
- **Privacy, Security, and Portability**
  - Strengthen:
    - Encryption at rest (DB-level or app-layer).
    - Simple backup/export format (JSON or similar).
    - Import/migration path (“take my second mind to a new machine”).

---

## Summary

- **MVP**: Single-user semantic memory with local embeddings, basic search, and simple daily reflection.
- **Phase 1**: Secure, type-aware ingestion with orchestration and deadline detection foundations.
- **Phase 2**: Temporal intelligence, resurfacing engine, and reflective analytics.
- **Phase 3**: Tunnels as dynamic semantic pathways and feedback-driven learning.
- **Phase 4**: Multi-channel capture, knowledge graph, advanced tunnel visualization, and personal cognitive metrics.


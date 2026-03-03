# TODO / Design Notes

## Security and Safety

- Safeguard from phishing links or exposing anything.
- Prompt injection security.
- Script injections.

## Orchestration Layer

When you send:

- Link
- Image
- Text
- Task
- Deadline

Our orchestration layer decides: “Which tool should I call?”

Example:

- If link → call metadata extractor.
- If image → call vision model.
- If task → call reminder scheduler.
- If research → generate summary + tags.

## Chat Interfaces (Telegram and WhatsApp)

- **Telegram (MVP, ingestion + retrieval)**
  - Create a Telegram bot via BotFather and connect it to the backend.
  - Use Telegram as the **primary channel** for both:
    - Ingesting memories (notes, links, ideas, tasks) in simple English.
    - Querying existing memories in simple English (“Ask it about anything and it should understand and fetch what you want”).
  - Implement intent detection to decide:
    - When a message should **store** a new memory.
    - When it should **retrieve** and summarize relevant memories.
  - Ensure responses are conversational and context-aware inside the chat thread.
- **WhatsApp (Phase 2, ingestion + retrieval)**
  - Integrate WhatsApp via Baileys or a similar library.
  - Mirror the same behaviors as Telegram:
    - Natural-language ingestion and retrieval.
    - Shared orchestration + semantic retrieval pipeline.
  - Handle basic authentication / access control so only the owner can talk to their second mind via WhatsApp.

## Memory Intelligence Layer

Instead of hard-coded SQL resurfacing, our system should:

- Decide which memories are important.
- Prioritize resurfacing based on:
  - Tags
  - Importance
  - Recency
  - Your behavior

This is much smarter than cron-only logic.

## Deadline Detection Agent

Instead of you writing regex for dates, our agent/system can:

- Extract date.
- Confirm timezone.
- Decide reminder frequency.
- Schedule follow-ups.

Example:

You send: “Hackathon submission March 8”.

Agent:

- Detects date.
- Adds 2-day-before reminder.
- Adds same-day reminder.
- Confirms with you.

## Product and UX Improvements

1. **Sharpen the MVP around one or two workflows**  
   Clarify the first “hero use cases”, for example:  
   - “Capture research ideas + see them again when they become relevant.”  
   - “Track all tasks/ideas with deadlines and get smart reminders + nightly summary.”  
   That will guide which parts of ingestion, resurfacing, and analytics must be rock-solid in v1 vs “nice to have later”.
2. **Better feedback loop and learning**  
   User feedback signals on resurfaced items:  
   - “Relevant / not relevant”, “remind later”, “snooze”, “archive this”.  
   Use these signals to update `priority_score` or a separate “usefulness score” so the system learns your taste over time instead of staying static.
3. **Richer temporal and UI views**  
   Add explicit time-based views:  
   - Timeline of memories with filters by topic/priority.  
   - “This week’s resurfaced items” vs “things you ignored.”  
   A home dashboard concept with:  
   - Today’s resurfaced items  
   - Deadlines approaching  
   - Short daily reflection snippet
4. **Stronger task/deadline integration**  
   Right now tasks + deadlines are mentioned, but you could:  
   - Make a first-class “Task Memory” type with fields like `status`, `estimated_effort`, `project`.  
   - Integrate (optionally) with a calendar or time-blocking view.  
   - Use resurfacing not just for “remember this idea” but “nudge you to finish tasks before they decay.”
5. **Capture from multiple channels**  
   To reduce friction / fragmentation further:  
   - Browser extension or bookmarklet to send current tab as a memory.  
   - Email-to-memory address (forward emails into the system).  
   - Simple CLI or chat-like interface to dump thoughts quickly.  
   Each reduces the chance that information lives outside your second mind.
6. **Early, pragmatic knowledge graph**  
   The README lists knowledge graph as a “future extension”. You could:  
   - Start lightweight entity extraction (people, projects, tools, concepts) and store them as structured tags/relations.  
   - Provide simple views like: “All memories related to <Person X>” or “All memories related to <Project Y>”.  
   This would make cross-domain synthesis more visible earlier.
7. **Personal metrics and goals**  
   Tie analytics to user-set goals:  
   - For example, “This month I want 40% of my cognitive time on GATE prep.”  
   Compare actual topic distribution vs goal.  
   This directly attacks attention drift by giving you a “compass”, not just a retrospective.
8. **Privacy, security, and portability**  
   Since this is personal cognition, clarify:  
   - Encryption at rest for the database (or at least support it).  
   - Export/import of all data (JSON or similar) so you’re not locked in.  
   - Clear story for “if I change machines, how do I take my mind with me?”

## Persistent Memory and Tunnels

- Implement persistent memory.

### Tunnels: Dynamic Semantic Pathways

1. **What a tunnel is**  
   - A tunnel is a dynamic, evolving semantic pathway through your memory.  
   - It is not a folder, a tag, a project, or a static category.  
   - It is a continuously updating stream of related thoughts across time.

2. **Why tunnels are needed**  
   - Folders are rigid.  
   - Tags are shallow.  
   - Projects are temporary.  
   - Your thinking is not linear.  
   - Example: you think about FPGA, ASCON, AI inference, hardware security, and memristor PUF. These are not separate folders; they form a conceptual pathway. That pathway is a tunnel.

3. **How a tunnel is formed**  
   Technically, a tunnel emerges when:  
   - Multiple memory items cluster in embedding space.  
   - They share semantic similarity.  
   - They appear repeatedly over time.  
   - They intersect across sessions.  
   Instead of you manually creating something like “Hardware Security”, the system detects a recurring semantic cluster. That cluster becomes a tunnel.

4. **What a tunnel contains**  
   A tunnel includes:  
   - Core theme vector (centroid of embeddings).  
   - Related memory items.  
   - Timeline of evolution.  
   - Idea branches.  
   - Deadlines connected to it.  
   - Activity frequency.  
   Think of it like a living research corridor.

5. **Example tunnel (personalized)**  
   You save:  
   - FPGA secure inference idea.  
   - ASCON lightweight crypto note.  
   - Medical edge inference architecture.  
   - Hackathon draft.  
   - Memristor-PUF security idea.  
   The system sees:  
   - Embedding similarity increasing.  
   - Temporal clustering increasing.  
   - Recurrent keywords increasing.  
   A tunnel is created, for example: “Hardware-Accelerated AI Security Tunnel”.  
   Now any time you add related memory, search similar topics, or discuss related ideas, it gets absorbed into this tunnel.

6. **What makes tunnels powerful**  
   Tunnels allow:  
   - Long-term idea continuity.  
   - Cross-domain linking.  
   - Automatic project grouping.  
   - Intellectual evolution tracking.  
   Instead of remembering individual notes, you remember the trajectory of your thinking.

7. **Tunnel intelligence**  
   Each tunnel tracks:  
   - Growth rate.  
   - Dormancy.  
   - Deadline pressure.  
   - Convergence with other tunnels.  
   - Drift into new themes.  
   Example: the system detects an FPGA tunnel converging with a sustainability tunnel. That is idea synthesis and innovation.

8. **Tunnel visualization (dashboard idea)**  
   Each tunnel appears as:  
   - A timeline graph.  
   - A branching map.  
   - A density heatmap.  
   - A priority meter.  
   You do not see isolated notes; you see idea ecosystems.

9. **How tunnels are implemented (technical core)**  
   - Step 1: Store embeddings for all memory items.  
   - Step 2: Periodically cluster embeddings (k-means / hierarchical clustering).  
   - Step 3: Identify stable clusters over time.  
   - Step 4: Assign the cluster centroid as the tunnel vector.  
   - Step 5: Track cluster evolution.  
   Tunnels are dynamic vector clusters across time.

10. **Deep insight**  
    - Folders organize data.  
    - Tunnels organize thought trajectories.  
    - That is the difference.
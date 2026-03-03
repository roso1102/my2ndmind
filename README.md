# 🧠 MySecondMind

### A Self-Hosted Cognitive Memory Engine

---

## 1. Overview

MySecondMind is a self-hosted cognitive system designed to function as an externalized structured memory layer for an individual.

It ingests unstructured inputs (text, links, images), transforms them into structured knowledge objects, encodes them into semantic space, tracks them temporally, and resurfaces them intelligently.

The system is designed around one principle:

> Memory without structure is storage.
> Memory with structure, time, and intelligence becomes cognition.

---

## 2. Problem Statement

Modern knowledge workers face three systemic failures:

1. **Cognitive fragmentation** — ideas are scattered across chats, notes, bookmarks.
2. **Context decay** — saved content loses relevance without resurfacing.
3. **Attention drift** — focus shifts without awareness of pattern.

Traditional tools (Notion, Obsidian, Google Docs) provide storage and manual retrieval.

They do not:

* Model semantic similarity.
* Understand priority or deadlines.
* Resurface information intelligently.
* Analyze cognitive trends over time.

MySecondMind addresses these limitations.

---

## 3. System Philosophy

MySecondMind mirrors biological cognition:

| Biological System | Digital Equivalent              |
| ----------------- | ------------------------------- |
| Sensory Input     | Ingestion Layer                 |
| Hippocampus       | Structured Storage + Embeddings |
| Prefrontal Cortex | LLM Reasoning Layer             |
| Dopamine System   | Priority Scoring                |
| Circadian Recall  | Scheduled Resurfacing           |
| Meta-cognition    | Trend & Reflection Engine       |

This is not a note-taking system.

This is a cognitive augmentation system.

---

## 4. Core Capabilities

### 4.1 Intelligent Ingestion

Supports:

* Plain text
* URLs
* Images
* Tasks and deadlines
* Research notes
* Ideas
* Chat messages from Telegram / WhatsApp

The system automatically:

* Classifies content type
* Extracts metadata
* Generates summary
* Detects deadlines
* Assigns priority
* Tags semantically

---

### 4.2 Structured Memory Encoding

Each memory object contains:

* `raw_content`
* `content_type`
* `title`
* `summary`
* `tags`
* `deadline`
* `priority_score`
* `embedding_vector`
* `created_at`
* `last_accessed`
* `resurfaced_count`

Embeddings enable semantic recall rather than keyword matching.

---

### 4.3 Semantic Retrieval

Query processing pipeline:

1. Convert query to embedding.
2. Perform vector similarity search.
3. Rerank using LLM reasoning.
4. Return structured, relevant results.

Supports natural language recall:

> “That FPGA ASCON medical inference idea.”

In practice, you should be able to ask in simple English, through chat (Telegram/WhatsApp or web), and the system will:

* Infer whether you are trying to **store** a new memory or **retrieve** existing ones.
* Interpret your intent from natural language.
* Fetch and present the most relevant memories, not just keyword matches.

---

### 4.4 Temporal Intelligence

Memory is not static.

Each memory item evolves based on:

* Age
* Access frequency
* Deadline proximity
* Priority score
* Thematic alignment with recent activity

Resurfacing logic combines:

```
resurface_score =
  priority_weight
+ aging_factor
+ semantic_relevance_to_recent_activity
- resurfaced_penalty
```

This creates natural spaced repetition without explicit scheduling.

---

### 4.5 Reflective Analytics

Nightly system analysis provides:

* Topic distribution
* Focus concentration
* Deadline pressure
* Thematic drift
* Cognitive trend evolution

Example output:

> Today’s focus:
> 60% Hardware Security
> 25% GATE Preparation
> 15% Miscellaneous
>
> Trend: Increasing convergence between FPGA and AI security topics.

This enables meta-cognition.

---

### 4.6 Notifications and Delivery

MySecondMind is not only pull-based search; it can also **push** information back to you.

Notification channels:

* **Telegram** (primary in early phases).
* **WhatsApp** (added later).

Types of notifications:

* **Daily digest** — short summary of today’s created memories, topic distribution, and key tunnels or themes.
* **Resurfacing prompts** — selected high-score memories delivered as contextual recall messages.
* **Reminders** — deadline-based pings for tasks and time-sensitive memories.

Notifications are delivered as chat messages through your personal bots, so your second mind can proactively talk to you.

---

## 5. Architecture

### 5.1 High-Level Architecture

```
User Interface (Telegram / WhatsApp / Web dashboard)
            ↓
        FastAPI/Websockets Backend
            ↓
     Local LLM (ollama or llm.cpp)
            ↓
    PostgreSQL + pgvector
            ↓
        Scheduler (cron)
```

Early phases prioritize **chat-based access**:

* **Telegram bot** (created via BotFather) is the primary input and retrieval channel in the MVP.
* From later phases, **WhatsApp** (via Baileys or similar library) is added as an additional chat channel.
* Other capture channels (browser extension, email, CLI, etc.) come later as part of multi-channel capture.

---

### 5.2 Technology Stack

**Inference Engine**

* Ollama/llm.cpp
* LLaMA / Mistral/qwen 3.5b (7B–8B quantized)

**Embeddings**

* nomic-embed-text
* bge-small

**Database**

* PostgreSQL
* pgvector extension

**Backend**

* FastAPI/LLM.cpp (Python)

**Frontend**

* Telegram bot (primary early interface for ingestion + retrieval).
* Next.js or React dashboard (browsing, reflections, analytics).

**Scheduler**

* Linux cron

Fully self-hosted.
Zero recurring cost.

---

## 6. Data Flow

### 6.1 Save Memory

1. Input received.
2. LLM classifies and extracts structure.
3. Deadline extraction performed.
4. Embedding generated.
5. Priority score computed.
6. Object stored in database.

---

### 6.2 Query Memory

1. User query embedded.
2. Vector similarity search.
3. LLM reranking.
4. Structured result returned.

---

### 6.3 Night Summary

1. Fetch items created today.
2. Analyze dominant tags.
3. Detect thematic clustering.
4. Generate structured reflection report.

---

### 6.4 Resurfacing Engine

1. Query eligible old memories.
2. Compute resurfacing score.
3. Select top N.
4. Deliver contextualized recall message.

---

## 7. Design Principles

### 7.1 Separation of Concerns

* Inference ≠ Storage
* Storage ≠ Scheduling
* Scheduling ≠ UI

Each module is independently replaceable.

---

### 7.2 Local First

* No external APIs
* No data leakage
* No token dependency
* No rate limits

The user owns cognition.

---

### 7.3 Evolvability

System supports extension toward:

* Knowledge graph modeling
* Attention vector tracking
* Topic clustering
* Research publication
* Multi-agent orchestration

---

## 8. Future Extensions

* Knowledge graph layer (entity linking)
* Attention drift modeling
* Adaptive resurfacing frequency
* Personal cognitive performance metrics
* Focus alignment prediction
* Research-grade memory modeling

---

## 9. Non-Goals

* Replacing full project management tools
* Acting as a general-purpose chatbot
* Serving multi-tenant public users (initially)

This is a personal cognitive engine.

---

## 10. Why This Matters

Cognitive performance determines output.

Most people increase effort.
Very few increase memory structure.

MySecondMind increases:

* Continuity of thought
* Cross-domain synthesis
* Long-term idea retention
* Strategic awareness

It transforms memory from passive archive into active intelligence.

---

## 11. Vision

At maturity, MySecondMind becomes:

* A structured mirror of your intellectual evolution.
* A cognitive amplifier.
* A long-term thinking companion.
* A research platform for modeling digital cognition.

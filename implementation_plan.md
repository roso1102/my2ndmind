# Implementation Plan - 2ndMind

## Goal Description
Build "2ndMind", a Telegram-based Second Brain that uses AI to capture, organize, and resurface information with zero friction. The system will use FastAPI for the backend, Supabase for storage (including Vector Search), and Google Gemini/Groq for intelligence.

## User Review Required
> [!IMPORTANT]
> **API Keys**: You will need to provide API keys for Telegram Bot, Supabase, and Google Gemini/Groq in the `.env` file once the project is initialized.

## Proposed Changes

### Infrastructure Layer
#### [NEW] [requirements.txt](file:///c:/Users/Anilkumar/.gemini/antigravity/playground/final-belt/requirements.txt)
- `fastapi`, `uvicorn`: Web server
- `supabase`: Database client
- `python-telegram-bot` or `httpx`: Telegram API interaction
- `google-generativeai`: AI Model access
- `pydantic`: Data validation
- `python-dotenv`: Environment management

#### [NEW] [.env.example](file:///c:/Users/Anilkumar/.gemini/antigravity/playground/final-belt/.env.example)
- Template for API keys and configuration.

### Database Layer (Supabase)
#### [NEW] [schema.sql](file:///c:/Users/Anilkumar/.gemini/antigravity/playground/final-belt/schema.sql)
- SQL script to initialize:
    - `users` table
    - `notes` table (with `vector(768)` column)
    - `tasks` table
    - `links` table
    - `search_memories` RPC function

### Application Logic
#### [NEW] [main.py](file:///c:/Users/Anilkumar/.gemini/antigravity/playground/final-belt/app/main.py)
- Entry point for FastAPI.
- Webhook route for Telegram updates.

#### [NEW] [core/config.py](file:///c:/Users/Anilkumar/.gemini/antigravity/playground/final-belt/app/core/config.py)
- Configuration loader.

#### [NEW] [services/telegram_service.py](file:///c:/Users/Anilkumar/.gemini/antigravity/playground/final-belt/app/services/telegram_service.py)
- Handles sending/parsing messages from Telegram.

#### [NEW] [services/ai_service.py](file:///c:/Users/Anilkumar/.gemini/antigravity/playground/final-belt/app/services/ai_service.py)
- Interface for Gemini/Groq.
- Functions for `classify_intent` and `generate_embedding`.

#### [NEW] [services/db_service.py](file:///c:/Users/Anilkumar/.gemini/antigravity/playground/final-belt/app/services/db_service.py)
- Supabase interactions (CRUD + Vector Search).

## Verification Plan

### Automated Tests
- **Unit Tests**: Test the Intent Classifier with sample sentences to ensure it correctly identifies Tasks vs Notes.
- **Integration Tests**: Verify that a saved note actually appears in Supabase.

### Manual Verification
1.  **Echo Test**: Send "Hello" to the bot -> Bot replies "Hello".
2.  **Save Test**: Send "Buy milk" -> Verify it appears in the `tasks` table.
3.  **Search Test**: Send "What do I need to buy?" -> Verify bot replies "Milk".

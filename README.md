# 🧠 2ndMind

**2ndMind** is a "Second Brain" system designed to eliminate the friction of remembering things. It combines the accessibility of **Telegram** with the power of **Large Language Models (LLMs)** to capture, organize, and resurface your life's information.

## 🚀 Key Features

- **Frictionless Capture**: Just talk to the bot. No forms, no buttons.
- **Smart Organization**: Automatically categorizes inputs into **Notes**, **Tasks**, **Links**, or **Reminders**.
- **Semantic Search**: Search by *meaning*, not just keywords. "What was that bird project?" finds "Golden Sparrow".
- **Proactive Assistant**: Sends morning briefings and evening summaries.
- **Dual AI Engine**: Powered by **Groq (Llama 3)** for speed and **Google Gemini** for complex reasoning.

## 🛠️ Tech Stack

- **Interface**: Telegram Bot API
- **Backend**: Python (FastAPI)
- **Database**: Supabase (PostgreSQL + pgvector)
- **AI Models**: Google Gemini 1.5 Flash / Groq Llama 3
- **Hosting**: Koyeb (Serverless)

## ⚡ Quick Start

1.  **Clone the repo**
2.  **Setup Environment**
    ```bash
    cp .env.example .env
    # Fill in your API keys
    ```
3.  **Install Dependencies**
    ```bash
    pip install -r requirements.txt
    ```
4.  **Run Locally**
    ```bash
    uvicorn app.main:app --reload
    ```

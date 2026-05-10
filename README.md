# 🍽️ Restaurant Management AI  Telegram-Powered Smart Assistant

> **A fully conversational AI waiter that handles orders, cancellations, and customer questions  all through Telegram.**

![n8n](https://img.shields.io/badge/Built%20with-n8n-orange?style=flat-square&logo=n8n)
![AI Powered](https://img.shields.io/badge/AI-OpenRouter-blueviolet?style=flat-square)
![Telegram Bot](https://img.shields.io/badge/Interface-Telegram-2CA5E0?style=flat-square&logo=telegram)
![RAG](https://img.shields.io/badge/Knowledge-Supabase%20RAG-3FCF8E?style=flat-square&logo=supabase)
![Google Sheets](https://img.shields.io/badge/Orders-Google%20Sheets-34A853?style=flat-square&logo=googlesheets)

---

## 🧠 What It Does

This workflow transforms a Telegram bot into a **24/7 intelligent restaurant assistant**. Customers chat naturally, and the AI:

- Understands the **intent** of every message (order, cancel, question)
- Routes it to the **right specialized agent**
- Manages orders by **reading and writing to Google Sheets**
- Answers menu and policy questions from a **RAG knowledge base**
- Maintains **conversation memory** for a natural, flowing experience

No staff needed for routine interactions. No missed orders. Always on.

---

## ⚙️ Workflow Architecture

```
Telegram Trigger
[Customer sends a message]
        │
        ▼
Basic LLM Chain (Intent Classifier)
[Classifies message as: Orders | Questions | General]
        │
        ▼
      Switch
   ┌────┴────────────┐
   │                 │
   ▼                 ▼
Orders Agent     Questions Agent
   │             [RAG-powered]
   │                 │
   │    ┌────────────┤
   │    │            │
   ▼    ▼            ▼
Read  Write      Supabase
Sheet Sheet      Vector Store
(Read) (Append)  [Knowledge Base]
       (Update/Cancel)
        │
        ▼
   Telegram Reply
```

### 🤖 Specialized Agents

| Agent | Role | Tools |
|-------|------|-------|
| **Orders Agent** | Place, read & cancel orders | Google Sheets (read / write / update) |
| **Questions Agent** | Answer menu, hours, policies | Supabase Vector Store (RAG) |
| **General Agent** | Handle greetings & edge cases | Conversational LLM |

---


---

## 🚀 Getting Started

### Prerequisites

- An **n8n** instance (self-hosted or cloud)
- A **Telegram Bot** token (from [@BotFather](https://t.me/BotFather))
- An **OpenRouter** API key
- A **Google Sheets** spreadsheet for orders
- A **Supabase** project with your restaurant knowledge base indexed (see the [RAG AI Agent](../rag_ai_agent/))

### Google Sheets  Orders Schema

Your orders sheet should have the following columns:

| customer_name | item | quantity | status | timestamp |
|---------------|------|----------|--------|-----------|
| Ahmed | Burger | 2 | confirmed | 2025-01-15 |

### Setup Steps

1. **Import the workflow**  Upload `restaurant_management.json` into n8n via *Workflows → Import from file*.

2. **Configure credentials:**
   - `Telegram` — Add your bot token
   - `OpenRouter` — Add your API key in both LangChain nodes
   - `Google Sheets` — Connect your Google OAuth account
   - `Supabase` — Add your project credentials for the vector store

3. **Update the Google Sheet ID** — Replace the sheet URL in all three Google Sheets tool nodes.

4. **Populate the knowledge base** — Use the companion [RAG AI Agent](../rag_ai_agent/) workflow to index your menu, FAQ, and policy PDFs.

5. **Activate the workflow** — Your restaurant bot is live!

---

## 💬 Example Conversations

**Ordering:**
> 👤 Customer: "I'd like to order 2 burgers and a Coke"
> 🤖 Bot: "Your order has been placed! 2 Burgers + 1 Coke — we'll have it ready shortly."

**Cancelling:**
> 👤 Customer: "Can I cancel my last order?"
> 🤖 Bot: "Done! Your order has been cancelled. Let me know if you'd like to order something else."

**Questions:**
> 👤 Customer: "What are your opening hours on Sundays?"
> 🤖 Bot: "We're open Sunday from 12pm to 10pm. Dine-in and delivery available!"

---

## 📂 Project Structure

```
restaurant_management.json    ← n8n workflow file (import this)
README.md                     ← You are here
```

---

## 🔒 Notes & Best Practices

- The **intent classifier** runs before routing to avoid sending every message to all agents, saving tokens and latency.
- The **Memory Buffer Window** keeps the last N messages in context for natural multi-turn conversations.
- Order cancellations use a **Google Sheets update** (not delete) to preserve order history.
- The RAG knowledge base should be populated before activating — see the companion workflow.

---

## 🤝 Contributing

Improvements to agent prompts, new tools (e.g. delivery tracking), or UI enhancements are very welcome!

---

*Built with ❤️ using n8n + LangChain + Telegram + Supabase*    

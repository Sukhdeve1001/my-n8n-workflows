## 📧 AI Email Auto-Responder with Knowledge Base

This is a **smart email bot** that reads incoming emails, understands them using AI, searches your company's knowledge base, writes a professional reply, reviews it, and sends it — all automatically.

---

## 🔄 Full Flow (Easy Language)

```
📬 New email arrives (IMAP)
        ↓
📝 Convert HTML email → clean Markdown
        ↓
🤖 Summarize email (DeepSeek R1)
        ↓
🏷️ Classify: Is it a "Company Info Request"?
    ┌── YES ──→ Write reply using Knowledge Base
    │                   ↓
    │            Review & format as HTML
    │                   ↓
    │            📤 Send reply email
    └── NO  ──→ Do nothing
```

---

## 🧠 The 3 Stages Explained Simply

### Stage 1 — Setup: Build the Knowledge Base (run once)
Before anything works, you feed your company documents into a **Qdrant vector database**:
- Pull files from a **Google Drive folder**
- Split them into chunks (300 tokens each)
- Convert to embeddings via **OpenAI**
- Store in **Qdrant** (a vector search database)

This is your AI's "brain" — it knows everything about your company from these docs.

---

### Stage 2 — Understand the Email
When a new email arrives:
- It's converted to **Markdown** for clean AI reading
- **DeepSeek R1** (free model) summarizes it in under 100 words
- **GPT-4o-mini** classifies it — is this person asking about company info?
  - If NO → ignored (Do Nothing node)
  - If YES → proceed to reply

---

### Stage 3 — Write, Review & Send
- **Write email** agent (GPT-4o-mini) drafts a reply by querying the Qdrant knowledge base for relevant company info
- **Review email** chain (DeepSeek) polishes it, formats it in clean HTML, keeps it under 100 words
- **Send Email** fires the reply back to the original sender via SMTP

---

## 🤖 AI Models Used

| Model | Job |
|---|---|
| DeepSeek R1 (free) | Summarize incoming email |
| GPT-4o-mini | Classify email type |
| GPT-4o-mini | Write the reply draft |
| DeepSeek R1 (free) | Review & format the reply |

---

## 💡 Real-World Use Case
The test email in the JSON is from an Italian customer named **Davide** asking a tech store about opening hours, technical support, delivery options, discounts, payment methods, etc. — the bot would automatically reply with accurate info pulled from the store's documents, without any human touching it.

This is essentially a **RAG-powered (Retrieval Augmented Generation) customer support bot** built entirely in n8n. Very similar in architecture to the email auto-responder you've built before, but with the added Qdrant knowledge base layer.

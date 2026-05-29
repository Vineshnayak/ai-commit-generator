# AI Commit Generator

An automated event-driven pipeline that processes GitHub push events, generates structured conventional commit messages using LLMs, logs activity, and sends notifications.

## System Overview

This project implements a workflow triggered by GitHub `push` events. It automates:
1. **Event Ingestion:** Receives webhook payloads from GitHub containing commit and metadata details.
2. **AI Commit Message Generation:** Uses the Groq API (running Llama 3) to analyze change metadata and generate Conventional Commit messages.
3. **Notification Delivery:** Formats and routes updates to a Telegram channel/chat.
4. **Data Logging:** Logs execution history (timestamp, repository, branch, and generated messages) directly to a Google Sheets spreadsheet.

For details on the design, see [architecture.md](./architecture.md) and [webhook-flow.md](./webhook-flow.md).

---

## Security & Data Privacy

### Is Git Safe?
Git is a local version control system and **does not** automatically scan, share, or upload your system files or private data. 
* **Data Scoped to Repository:** Only files inside this specific project directory that you explicitly track (`git add`) are committed.
* **Sensitive Files Excluded:** Critical environment variables and credentials (such as your `TELEGRAM_BOT_TOKEN`, `TELEGRAM_CHAT_ID`, and `GROQ_API_KEY`) are stored in the local `.env` file, which is listed in `.gitignore` and is never committed or pushed to GitHub.
* **Git Metadata:** When pushing commits, Git only transmits the repository file modifications, commit messages, and author metadata (your configured name, email, and timestamps).

### Pipeline Security
* **Signature Verification:** The n8n webhook validates GitHub requests using the `X-Hub-Signature-256` header and a secure webhook secret. This ensures only authenticated payloads from GitHub are processed.
* **API Key Isolation:** All external integrations (Groq API, Telegram Bot, Google Sheets OAuth) are stored securely in n8n's credential manager or local `.env` variables and are never exposed publicly.

---

## Setup & Configuration

### Prerequisites
* An active [n8n](https://n8n.io/) instance (local or hosted).
* A Groq API Key.
* A Telegram Bot token and Chat ID.
* Google Sheets access configured via n8n.

### Setup Steps
1. **Clone the Repository:**
   ```bash
   git clone https://github.com/Vineshnayak/ai-commit-generator.git
   cd ai-commit-generator
   ```

2. **Configure Environment Variables:**
   Copy the template and specify your local configuration:
   ```bash
   cp .env.example .env
   ```
   *Note: Ensure the local `.env` file remains excluded from version control.*

3. **Import Workflow into n8n:**
   * Open your n8n dashboard.
   * Create a new workflow.
   * Import the workflow file located at [workflows/github-push-workflow.json](./workflows/github-push-workflow.json).

4. **Set Up GitHub Webhook:**
   * Go to your GitHub repository settings -> **Webhooks** -> **Add webhook**.
   * Set the **Payload URL** to your n8n webhook URL.
   * Set **Content type** to `application/json`.
   * Input a secret key and ensure it matches the `GITHUB_WEBHOOK_SECRET` in your n8n workflow.

---

## Directory Structure

```
├── .env.example                  # Environment variables template
├── .gitignore                    # Git exclusions list
├── README.md                     # Project documentation
├── architecture.md               # Pipeline architecture documentation
├── webhook-flow.md               # Step-by-step webhook execution flow
├── prompts/
│   └── commit-prompt.md          # System prompt used by the Groq API
└── workflows/
    └── github-push-workflow.json # JSON export of the n8n workflow
```
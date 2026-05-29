# System Architecture

This project utilizes an event-driven architecture powered by n8n.

1. **GitHub** -> Triggers a push event webhook containing repository and commit data.
2. **n8n Webhook** -> Receives the payload and authenticates via Header Signature.
3. **Data Extraction** -> Parses repository name, branch, and added/modified files.
4. **Groq API (AI)** -> Analyzes the changes and generates a structured Conventional Commit message.
5. **Telegram Bot** -> Pushes a success or failure notification directly to the user's device.
6. **Google Sheets** -> Appends a new row logging the timestamp, repo, branch, and AI output.
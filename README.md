# AI Commit Generator 🚀

## Project Overview
An automated pipeline that listens to GitHub Push events, extracts commit details, and uses the Groq AI API (Llama 3) to generate professional, Conventional Commit messages. Notifications are sent via Telegram, and execution logs are stored in Google Sheets.

## Architecture
See [`architecture.md`](./architecture.md) for a detailed system overview.

## Setup Steps
1. Clone this repository.
2. Import the `workflows/github-push-workflow.json` file into your n8n instance.
3. Configure your Webhook URL in your GitHub Repository Settings.
4. Set up the required credentials in n8n (Groq API, Telegram Bot, Google Sheets OAuth).

## Environment Variables
If running n8n locally via Docker, ensure you have the following in your `.env` file:
\`\`\`env
GITHUB_WEBHOOK_SECRET=your-secret-key-here
\`\`\`

## Deployment Steps
For production, host n8n on a VPS or a platform like Railway/Render, replacing the local webhook URL with your permanent public URL.
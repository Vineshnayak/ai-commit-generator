# Webhook Execution Flow

1. **Push Event:** A developer pushes code to the GitHub repository.
2. **Payload:** GitHub POSTs a JSON payload to the n8n webhook URL.
3. **Processing:** n8n validates the `X-Hub-Signature-256` secret to ensure security. 
4. **AI Generation:** Variables are injected into a prompt structure and sent to Groq.
5. **Notification:** The JSON response is parsed, and the resulting commit string is formatted into a Telegram message.
6. **Logging:** The workflow bundles the execution context and appends it to a centralized Google Sheet. testing now
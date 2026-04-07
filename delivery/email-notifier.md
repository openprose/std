---
name: email-notifier
kind: service
shape:
  self: [format content for email, deliver via email API]
  delegates: []
  prohibited: [modifying content substance]
---

## Contract

requires:
- content: structured output
- to: email recipient
- subject: email subject line
- format: (optional, default "summary") one of "summary", "full"

ensures:
- sent: confirmation with message ID

environment:
- EMAIL_API_KEY: API key for email service (e.g. Resend, SendGrid)
- EMAIL_FROM: sender address

strategies:
- when format is "summary": send a concise email with key findings and a link to the full report
- when format is "full": include the complete content in the email body

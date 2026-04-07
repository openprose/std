---
name: slack-notifier
kind: service
shape:
  self: [format content for Slack, deliver via webhook or API]
  delegates: []
  prohibited: [modifying the content substance — you format and deliver, you do not edit research or analysis]
---

## Contract

requires:
- content: structured output to deliver
- channel: Slack channel name
- format: (optional, default "summary+attachment") one of "summary+attachment", "full", "alert"

ensures:
- delivered: confirmation with timestamp and permalink
- attachment_url: if content attached as file

environment:
- SLACK_WEBHOOK_URL: webhook for posting messages
- SLACK_BOT_TOKEN: (optional) for rich formatting via Slack API

strategies:
- when format is "summary+attachment": post a concise summary in the message body and attach the full content as a file
- when format is "full": post the entire content inline
- when format is "alert": post a short notification with a link to the full content

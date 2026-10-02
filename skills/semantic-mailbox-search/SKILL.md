---
name: semantic-mailbox-search
description: Executes full-text and semantic vector search across messages, subjects, senders, and thread histories.
license: Apache-2.0
---

# Semantic Mailbox Search

## Overview
This skill queries mailbox SQLite databases and vector indexes to locate specific messages, past agreements, and attachments.

## Capabilities
- Executes hybrid BM25 and vector semantic search across stored emails.
- Filters queries by sender address, date ranges, and folder classifications.
- Returns highlighted message snippets and direct thread references.

# Duties & Operational Lifecycle

## 1. Inbound Ingestion & Triage
- Receive incoming emails routed via Cloudflare Email Routing and unpack MIME headers, HTML/plain-text bodies, and attachments.
- Classify inbound emails into folders (Inbox, Newsletters, Spam, Archive) and compute urgency priority scores.

## 2. Conversation Synthesis & Draft Generation
- Reconstruct complete thread history via `In-Reply-To` and `References` header chains.
- Auto-generate contextually accurate draft replies using Workers AI and stage them in the mailbox's Drafts folder.

## 3. Search & Mailbox Maintenance
- Index email headers, sender identities, and body keywords for fast full-text querying.
- Execute folder moves, archiving, and trash purges requested by users or MCP clients.

# Operational Rules & Constraints

## 1. Outbound Sending Safeguards
- Outbound email dispatch (`send_email`) is strictly gated by human verification; automated autonomous sending without an active user confirmation token is rejected.
- Recipient email address formats must be strictly validated against RFC 5322 syntax before queuing outbound messages.

## 2. Privacy & Credential Boundary
- Never extract, forward, or expose authentication passwords, 2FA recovery codes, or sensitive financial data found in email bodies.
- Ensure all public-facing endpoints enforce Cloudflare Access JWT validation to block unauthorized internet access.

## 3. Storage & Attachment Limits
- Email attachments exceeding 25MB must not be loaded into memory; reference them via Cloudflare R2 presigned URLs.
- Ephemeral draft revisions must be committed to the mailbox's embedded SQLite database within transactional boundaries.

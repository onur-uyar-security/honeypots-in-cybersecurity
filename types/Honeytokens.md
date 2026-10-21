# Honeytokens (Data Decoys)
 
A **honeytoken** is not a host — it's a piece of data planted so that **any use of it is an
alert**. They are the cheapest, fastest honeypots to deploy and work anywhere data lives.
 
## Common honeytoken types
| Token | Where it lives | Fires when... |
|---|---|---|
| **Honey credentials** | Password managers, scripts, config files, memory | Someone tries to authenticate with them |
| **Canary API keys** (e.g. cloud keys) | Repos, config, laptops | The key is used against the provider |
| **Canary tokens / URLs** | Documents, emails, web pages, DNS names | The link/resource is opened or resolved |
| **Honey files** | File shares, user folders ("passwords.xlsx") | The file is opened / accessed |
| **Honey database records** | A row/table of fake customers | The record is read or exfiltrated |
| **Honey QR / email address** | Public pages, signatures | It receives mail or is scanned (spam-trap style) |
 
## Why they work
- Real users and systems have **no reason** to touch them → near-zero false positives.
- They detect **insider misuse** and **post-compromise** activity (an attacker reading a share,
  dumping a password store, or exfiltrating a database).
- One token can detect a whole class of behaviour (e.g. a fake AWS key fires the moment it is used).
## Detection sources
- **File access:** enable object-access auditing → Windows Event ID **4663** on honey files;
  Linux `auditd` watch rules on honey paths.
- **Credential use:** failed/again successful auth events for honey accounts (**4625 / 4624**).
- **Canary URL / DNS:** your canary service logs the callback (source IP, user agent, time).
- **Cloud keys:** provider logs (e.g. CloudTrail) show the key being used.
## Design tips
- Make tokens **believable and tempting** (realistic names, placed where secrets normally sit).
- **Document** every token (owner, location, what it should trigger) so a hit is understood fast.
- **Never** wire a honeytoken into a real workflow — if legitimate systems use it, it becomes noise.
- Rotate/refresh tokens periodically so they stay credible.
## Tooling
Open-source **Canarytokens** (self-hostable) generate many token types (web bug, DNS, AWS key,
Word/PDF, QR, and more). See [`../tools.md`](../tools.md).
 

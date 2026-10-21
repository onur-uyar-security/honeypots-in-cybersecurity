# Honeypot Taxonomy
 
Honeypots are classified along several axes. The same decoy can sit in more than one category.
 
## 1. By interaction level
| Level | What it offers an attacker | Pros | Cons |
|---|---|---|---|
| **Low-interaction** | Emulated services (fake banners/responses only) | Safe, easy to run, low maintenance | Limited data; fingerprintable |
| **Medium-interaction** | More realistic emulation, some command handling | Richer data, still contained | More upkeep |
| **High-interaction** | Real OS/services, fully functional | Deepest intelligence on attacker behaviour | Risky — must be tightly isolated |
 
## 2. By purpose
- **Production honeypots** — placed inside/around the real environment to **detect** attacks early (focus on alerting). Usually low/medium interaction.
- **Research honeypots** — run to **study** attacker behaviour and collect malware/TTPs. Often high interaction.
## 3. By what they imitate
- **Server honeypots** — wait for inbound connections (SSH, HTTP, SMB, RDP, databases, ICS).
- **Client honeypots** — actively visit/consume content to catch **client-side** attacks (malicious sites, documents).
- **Malware honeypots** — capture malware samples (e.g. emulated vulnerable services).
## 4. Scale / architecture
- **Single honeypot** — one decoy host or service.
- **Honeynet** — a whole decoy network of honeypots behind a controlled gateway ("honeywall") that logs and limits outbound traffic.
- **Honeyfarm** — centralised pool of honeypots that traffic is redirected to.
## 5. Honeytokens (data decoys)
Not a host at all — a **piece of data** that should never be used: a fake credential, API key,
file, database row, or canary URL. Any access/use is an alert. Extremely cheap and effective
(see [`types/honeytokens.md`](types/honeytokens.md)).
 
## 6. Deception technology
Modern **deception platforms** automate placing many decoys (hosts, accounts, tokens,
breadcrumbs) across an environment and correlate the alerts — an enterprise evolution of the
honeypot idea.
 
## Placement principles
- Put decoys **where attackers look** (AD, file shares, admin tooling, internal network ranges).
- Make them **believable** (realistic names, aged timestamps) but give them **no legitimate use**.
- Ensure **every interaction is logged and alerted** — a honeypot nobody monitors is useless.
- **Isolate** high-interaction honeypots so they cannot pivot to real systems.
 

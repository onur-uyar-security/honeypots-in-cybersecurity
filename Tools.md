# Honeypot Tools (Open Source)
 
A reference list of widely used, open-source honeypot and deception tools. Choose by what you
want to detect and the interaction level you can safely operate.
 
## All-in-one / multi-honeypot platforms
| Tool | What it is |
|---|---|
| **T-Pot** | A ready-made platform (Docker-based) bundling many honeypots + dashboards (Elastic/Kibana). Great for a first lab. |
| **OpenCanary** | Lightweight daemon that runs multiple service decoys (SSH, HTTP, FTP, SMB, MSSQL, etc.) with simple alerting. Good for an internal "tripwire". |
| **Thinkst Canary / OpenCanary** | Commercial Canary appliances; OpenCanary is the open-source sibling. |
 
## Service / network honeypots
| Tool | Focus |
|---|---|
| **Cowrie** | SSH/Telnet honeypot — logs brute-force and post-login commands (medium interaction). |
| **Dionaea** | Captures malware via emulated services (SMB, HTTP, FTP, MSSQL, etc.). |
| **Honeyd** | Classic framework that simulates many virtual hosts/services on a network. |
| **Conpot** | ICS/SCADA honeypot (Modbus, S7comm, etc.) for industrial environments. |
| **Glastopf / SNARE + Tanner** | Web application honeypots that lure and log web attacks. |
 
## Honeytokens / canaries
| Tool | Focus |
|---|---|
| **Canarytokens** | Self-hostable generator for many token types: web bug, DNS, AWS/Azure keys, Word/PDF/Excel, QR, cloned-site, and more. Fires a callback when used. |
 
## Choosing a starting point
- **Just want internal early-warning?** → **OpenCanary** (a few service decoys) + a couple of
  **Canarytokens** (honey file, fake AWS key) + an **AD honey SPN**.
- **Want to study attacks / build a lab dashboard?** → **T-Pot**.
- **SSH brute-force research?** → **Cowrie**. **Malware capture?** → **Dionaea**.
  **Industrial?** → **Conpot**.
## Operating principles (whatever you pick)
1. **Isolate** the honeypot (dedicated VLAN/VM; restrict outbound) — see
   [`legal-and-safety.md`](legal-and-safety.md).
2. **Forward logs to your SIEM** and alert on **any** interaction.
3. **Keep it believable** (realistic names/banners) but with **no legitimate use**.
4. **Review and maintain** — update decoys so they don't become stale or fingerprintable.
> Tool names and capabilities evolve; check each project's current documentation before deploying.
 

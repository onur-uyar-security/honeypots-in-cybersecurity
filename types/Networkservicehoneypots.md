# Network & Service Honeypots
 
Decoy network services that wait for inbound connections. Because no real user should connect
to them, scans, brute-force and exploit attempts against them are strong signals.
 
## Common service decoys
| Service | What it catches | Typical tool |
|---|---|---|
| **SSH / Telnet** | Brute-force, credential stuffing, post-login commands | Cowrie |
| **HTTP / web app** | Web scanning, exploit attempts, bots | Glastopf / SNARE+Tanner |
| **SMB / file shares** | Lateral movement, ransomware, share enumeration | Dionaea, Honeyd, deception shares |
| **RDP** | Brute-force, exposed-RDP scanning | T-Pot components, RDP decoys |
| **Databases (MySQL/MSSQL)** | Internet-exposed DB attacks | Dionaea, custom low-interaction |
| **ICS / SCADA (Modbus, S7)** | Attacks on industrial protocols | Conpot |
| **Malware capture** | Worms/exploits dropping payloads | Dionaea |
 
## Interaction level matters
- **Low-interaction** service decoys emulate just enough to log the attempt — safe and simple.
- **High-interaction** decoys run real services to capture full attacker behaviour — must be
  **isolated** (see [`../legal-and-safety.md`](../legal-and-safety.md)).
## Honeynets
A **honeynet** is a whole decoy network of honeypots behind a controlled gateway
("**honeywall**") that:
- **logs** all traffic in and out,
- **limits outbound** connections so a compromised high-interaction honeypot cannot attack others,
- presents a realistic, segmented environment for research.
## Placement
- **Internet-facing** decoy (e.g. exposed SSH) → measures opportunistic scanning and the
  external threat landscape.
- **Internal** decoy (e.g. a fake file server in a user VLAN) → detects **lateral movement**
  after a breach — usually the higher-value placement for defenders.
## Detection & alerting
- Every connection attempt is an event — forward honeypot logs to the **SIEM** and alert on
  **any** inbound connection to the decoy.
- Enrich with source IP reputation and correlate with other telemetry (EDR, firewall).
- A hit on an **internal** service decoy should be treated as a likely **active intrusion**.
## Deployment
Multi-service lab honeypot (OpenCanary) setup:
[`../deployment/lab-opencanary.md`](../deployment/lab-opencanary.md).
 

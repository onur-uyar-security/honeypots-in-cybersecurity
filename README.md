# Honeypots in Cybersecurity
 
A defensive (Blue Team) reference on **honeypots**: what they are, the main **types**, the
**tools** used to run them, and **how to deploy** them safely in a lab. The goal is
**high-fidelity detection** — a honeypot has no legitimate use, so *any* interaction with it
is a strong signal of malicious or unauthorised activity.
 
> Educational, lab-focused material on simulated environments. No real organisational data.
> A honeypot must be **isolated and contained** so it cannot be used to attack others
> (see [`legal-and-safety.md`](Legal-and-safety.md)).
 
## Why honeypots?
- **Low false positives** — nobody should ever touch a decoy, so a hit is almost always real.
- **Early warning** — they catch enumeration and lateral movement before real assets are hit.
- **Threat intelligence** — they reveal attacker tooling, techniques and sources.
- **Cheap detection** — a single decoy account or token can cover a whole attack technique.
## Contents
| File | Topic |
|---|---|
| [`taxonomy.md`](Taxonomy.md) | How honeypots are classified (interaction level, purpose, target) + honeynets & honeytokens |
| [`types/active-directory-honeypots.md`](types/Active-directory-honeypots.md) | Honey accounts, **honey SPN (Kerberoasting decoy)**, honey admin, decoy objects |
| [`types/honeytokens.md`](types/Honeytokens.md) | Canary tokens, honey credentials, honey files, DB honey records |
| [`types/network-service-honeypots.md`](types/Network-service-honeypots.md) | SSH/HTTP/SMB/RDP/ICS service decoys, honeynets |
| [`types/web-and-email-honeypots.md`](types/Web-and-email-honeypots.md) | Form honeypots (bot detection), spam traps, web decoys |
| [`tools.md`](Tools.md) | Open-source honeypot tools (Cowrie, Dionaea, T-Pot, Conpot, OpenCanary, Canarytokens...) |
| [`deployment/lab-opencanary.md`](deployment/Lab-opencanary.md) | Deploy a multi-service honeypot (OpenCanary) in a lab |
| [`deployment/ad-honey-account-and-spn.md`](deployment/Ad-honey-account-and-spn.md) | Create an AD honey account + honey SPN and alert on Event ID 4769 |
| [`legal-and-safety.md`](Legal-and-safety.md) | Isolation, containment, legal and ethical considerations |
 
## How this connects to the rest of my work
The **honey SPN** here is the detection control referenced in my
[`kerberoasting-blue-team`](https://github.com/onur-uyar-security/kerberoasting-blue-team/tree/main) repo, expanded into a full
treatment of honeypots across the environment.
 
---
*Author: Osman Onur Uyar — defensive, lab-based learning reference.*

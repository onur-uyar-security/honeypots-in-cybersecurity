# Legal, Ethical & Safety Considerations
 
Honeypots are powerful but carry real risks if run carelessly. Treat these as **mandatory
guardrails**, not optional extras.
 
## 1. Containment — the #1 rule
A honeypot, especially **high-interaction**, can be compromised and then **used to attack
others**. If that happens from your network, you may be **liable**.
- Put honeypots on an **isolated VLAN/segment**.
- **Restrict outbound traffic** (default-deny; allow only what logging needs).
- For honeynets, use a controlled gateway (**honeywall**) that limits and logs outbound connections.
- Prefer **low/medium interaction** unless you can safely contain high interaction.
## 2. No "hacking back"
A honeypot is for **detection and intelligence**, not retaliation. Do not attack, scan back,
or attempt to access an attacker's systems — that is likely illegal and out of scope.
 
## 3. Entrapment is a misconception (but get advice)
In most contexts, passively running a decoy is **not entrapment** — you are not coercing
anyone to commit a crime. Still, rules vary by country and by whether the honeypot is internal
vs internet-facing; **check your legal/compliance team** before deploying, especially in
regulated environments.
 
## 4. Data protection & privacy
Honeypots and spam traps capture **attacker (and sometimes visitor) data** — IP addresses,
payloads, submissions. This may be **personal data** under laws such as the GDPR.
- Define a **lawful basis** and **retention period** for what you collect.
- Minimise and secure the data; restrict access.
- Document the honeypot in your processing records where required.
## 5. Authorisation & scope
- Deploy only on systems/networks you **own or are authorised** to instrument.
- Get **written approval** from the system owner and, for production placement, from management.
- Keep an **inventory** of every decoy (host, account, token) so responders know what is fake.
## 6. Operational hygiene
- **Monitor** the honeypot — an unwatched decoy is useless and can become a liability.
- **Patch/maintain** the host OS around the decoy services.
- **Fingerprinting:** sophisticated attackers can detect honeypots; keep decoys realistic and
  rotate them.
- **Incident handling:** define what happens when a decoy fires (who is alerted, response steps).
## 7. Documentation
For each honeypot/honeytoken, record: purpose, owner, location, what it should trigger, data
collected, retention, and the response playbook. This keeps the programme auditable and
prevents a decoy from being mistaken for a real asset.
 
---
*This is general guidance, not legal advice. Verify obligations with your legal/compliance
function and against the laws applicable to your organisation.*
 

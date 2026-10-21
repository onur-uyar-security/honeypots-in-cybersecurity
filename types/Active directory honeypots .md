# Active Directory Honeypots
 
Active Directory is where attackers enumerate and move laterally, so AD decoys are some of
the **highest-value, lowest-noise** detections available.
 
## 1. Honey account (decoy user)
A user account that looks real (plausible name, group, description, aged `whenCreated`) but
is **never used** by anyone.
 
- **Signal:** any logon attempt or authentication is suspicious.
- **Detect:** Event ID **4625** (failed logon) and **4624** (successful logon) for the decoy;
  **4768/4769** (Kerberos) if it has an SPN.
- **Tip:** add a tempting `description` (e.g. "svc – legacy backup") and place it in a group
  attackers target. Never log in with it, and exclude it from normal automation.
## 2. Honey SPN (Kerberoasting decoy)  ⭐
A decoy **service account with a Service Principal Name (SPN)** and **no real service**.
Kerberoasting enumerates user accounts with SPNs and requests their service tickets — so a
TGS request for a decoy SPN is **high-fidelity evidence of Kerberoasting enumeration**.
 
- **Signal:** Windows Security **Event ID 4769** where `Service Name` = the decoy SPN account.
- **Make it attractive:** give the account a long, strong password (so even if cracked it is
  useless), a believable SPN, and leave it otherwise unused.
- **Detection rule (concept):**
```
  EventID=4769 AND Service_Name="<honey-spn-account>"
  → alert immediately (any hit = enumeration)
```
- This is the control referenced in the [`kerberoasting-blue-team`](../../../kerberoasting-blue-team) repo.
## 3. Honey admin / privileged decoy
A fake privileged account (e.g. appears to be in a Tier-0 group) used purely as bait. Because
attackers hunt for privileged accounts, any interaction is a strong alert. Keep it **powerless
in reality** (no real privileges, disabled interactive logon where possible) while *appearing*
privileged.
 
## 4. Decoy objects & breadcrumbs
- **Decoy computers / service records** in AD that attract enumeration.
- **Breadcrumbs** on real hosts (saved fake credentials, RDP shortcuts, mapped "admin" shares)
  that lead attackers toward a monitored decoy instead of real assets.
- **DCSync / replication canaries** — monitor for replication requests (Event ID **4662**
  with replication GUIDs) from accounts that should never replicate.
## Key Windows events for AD decoys
| Event ID | Meaning | Use |
|---|---|---|
| 4769 | Kerberos service ticket requested | Honey SPN (Kerberoasting) |
| 4768 | Kerberos TGT requested | Honey account authentication |
| 4625 / 4624 | Failed / successful logon | Honey account access |
| 4662 | Operation on an AD object | DCSync/replication canary |
| 4663 | Access to an object (with auditing) | Honey file / share access |
 
## Deployment
Step-by-step lab guide: [`../deployment/ad-honey-account-and-spn.md`](../deployment/ad-honey-account-and-spn.md).
 

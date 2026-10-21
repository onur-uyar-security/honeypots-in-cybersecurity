# Lab: Active Directory Honey Account + Honey SPN (Kerberoasting decoy)
 
A step-by-step lab guide to create a decoy AD account with a Service Principal Name and alert
when anyone requests its Kerberos ticket — i.e. detect **Kerberoasting enumeration** with very
low false positives.
 
> **Lab only.** Do this in a test domain you own. Use realistic but fake values.
 
## 1. Create the honey account
In a test Active Directory, create a normal-looking user that will **never** be used:
 
```powershell
# Decoy service account — believable name, strong (long) password, never used
New-ADUser -Name "svc-backup-legacy" `
  -SamAccountName "svc-backup-legacy" `
  -Description "Legacy backup service (do not modify)" `
  -AccountPassword (Read-Host -AsSecureString "Decoy password (25+ chars)") `
  -Enabled $true
```
 
## 2. Give it a tempting SPN
A user account with an SPN is exactly what Kerberoasting enumerates:
 
```powershell
setspn -s MSSQLSvc/backup-legacy.contoso.local:1433 svc-backup-legacy
```
 
- Use a **long, random password** so that even if the ticket is cracked offline, the result
  is useless.
- Do **not** attach it to any real service; leave it otherwise idle.
## 3. Make sure the event is logged
Ensure **"Audit Kerberos Service Ticket Operations"** is enabled (Default Domain Controllers
Policy → Advanced Audit Policy → Account Logon). This produces **Event ID 4769** on the DCs.
 
## 4. Alert on any ticket request for the decoy
Any **4769** where the `Service Name` is the decoy account = enumeration. Alert immediately.
 
**Splunk (concept):**
```spl
index=wineventlog sourcetype="WinEventLog:Security" EventCode=4769
Service_Name="svc-backup-legacy*"
| table _time, Account_Name, Client_Address, Service_Name, Ticket_Encryption_Type
```
 
**Sigma (concept):**
```yaml
title: Honey SPN Ticket Requested (Kerberoasting decoy)
logsource: { product: windows, service: security }
detection:
  selection:
    EventID: 4769
    ServiceName|startswith: 'svc-backup-legacy'
  condition: selection
level: high
falsepositives:
  - None expected — the account has no legitimate use
```
 
## 5. Response when it fires
1. Treat as **active enumeration / likely intrusion**.
2. Identify the **requesting account** (`Account Name`) and **source** (`Client Address`).
3. Hunt for how that account/host was obtained; review recent 4768/4769 and EDR telemetry.
4. Contain the source host; rotate credentials as needed.
## Why this is strong
No user, script or service ever needs this account, so a single 4769 for it is **high-fidelity
evidence** — far cleaner than threshold-based Kerberoasting detection alone. Use **both**: the
behavioural rule (many RC4 SPN requests) *and* this decoy.
 
See also: [`../types/active-directory-honeypots.md`](../types/active-directory-honeypots.md)
and the [`kerberoasting-blue-team`](../../../kerberoasting-blue-team) repo.
 

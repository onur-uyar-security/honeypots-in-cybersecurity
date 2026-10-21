# Lab: Multi-Service Honeypot with OpenCanary
 
A simple lab to stand up an internal "tripwire" honeypot that emulates several services and
alerts on any connection. **Low-interaction and safe** for a first deployment.
 
> Lab guidance. Run on an isolated VM in a test network. Confirm current steps against the
> project's official documentation before deploying.
 
## Goal
Place a believable decoy host on an internal network. Because no real user should connect to
it, **any** connection (SSH login attempt, SMB access, HTTP request) is an alert.
 
## 1. Prepare an isolated VM
- A minimal Linux VM (e.g. Ubuntu) on a **segmented lab VLAN**.
- Restrict outbound traffic (the decoy should not initiate connections to real systems).
## 2. Install OpenCanary (concept)
OpenCanary is a Python daemon. Typical lab install:
```bash
sudo apt update && sudo apt install -y python3-venv python3-dev
python3 -m venv ~/env && source ~/env/bin/activate
pip install opencanary
opencanaryd --copyconfig        # writes a default ~/.opencanary.conf
```
 
## 3. Enable the service decoys you want
Edit `~/.opencanary.conf` and enable modules, for example:
```json
{
  "device.node_id": "lab-decoy-01",
  "ssh.enabled": true,  "ssh.port": 22,
  "http.enabled": true, "http.port": 80, "http.skin": "basicLogin",
  "smb.enabled": true,
  "mysql.enabled": true, "mysql.port": 3306,
  "logtype": "syslog"
}
```
Give it a **believable hostname** and banners so it blends in.
 
## 4. Run it and generate a test event
```bash
opencanaryd --start
# From another lab host, touch a service to confirm alerting:
ssh baduser@<decoy-ip>        # expect a failed-login alert
curl http://<decoy-ip>/       # expect an HTTP-hit alert
```
 
## 5. Centralise alerts
- Send OpenCanary logs (syslog/JSON) to your **SIEM**.
- Alert on **any** event from the decoy — there should never be legitimate traffic.
- Enrich with source IP and correlate with firewall/EDR.
## 6. Placement ideas (internal = highest value)
- A fake "file server" or "database" in a user VLAN → detects **lateral movement** post-breach.
- A decoy "jump host" near admin tooling → detects attackers hunting for management access.
## Safety
- Keep the honeypot **isolated and contained**; never let it reach production or the internet
  unmonitored.
- Review logs and maintain the decoy so it stays believable.
See [`../legal-and-safety.md`](../legal-and-safety.md).
 

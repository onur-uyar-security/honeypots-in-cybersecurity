# Web & Email Honeypots
 
Decoys aimed at automated abuse of web forms and email, plus web-layer deception.
 
## 1. Form honeypot (bot detection)
A hidden field added to a web form that **real users never see or fill** (hidden with CSS, or
labelled "leave this empty"). Automated bots fill every field, so **any submission with that
field populated** is flagged as a bot.
 
- **Use:** reduce spam/credential-stuffing on sign-up, contact and login forms.
- **Signal:** the hidden field is non-empty → reject/flag the submission.
- **Note:** this is a convenience control, not strong security — combine with rate limiting
  and other anti-automation measures.
**Concept (HTML):**
```html
<!-- Honeypot field: hidden from humans, tempting to bots -->
<div style="position:absolute; left:-9999px;" aria-hidden="true">
  <label>Leave this field empty</label>
  <input type="text" name="website" tabindex="-1" autocomplete="off">
</div>
```
Server-side: if `website` is non-empty → treat as a bot.
 
## 2. Spam traps (email honeypots)
Email addresses that are **never used for real correspondence** but are seeded where
harvesters find them (hidden on pages, in old records). Any mail they receive is spam/abuse,
useful for blocklists and measuring harvesting.
 
- **Pristine trap:** an address that never belonged to a real user.
- **Recycled trap:** a formerly real address repurposed after long inactivity.
- **Use:** feed reputation/blocklists; detect scraping of your site.
## 3. Web application decoys
- **Decoy paths / admin panels** (e.g. a fake `/admin` or `/wp-login.php`) that no legitimate
  user should request → requests reveal scanning and targeting.
- **Decoy robots.txt entries** that only scanners follow.
- **WAF deception** — serving believable fake responses to attackers to waste their time and
  collect their tooling.
## Detection & alerting
- Log and alert on **any** request to decoy paths or submissions tripping the form honeypot.
- Forward spam-trap hits and web-decoy events to the SIEM; correlate source IPs across decoys.
- Pair with rate limiting, reputation feeds and, for forms, CAPTCHA-style fallbacks where needed.
## Caution
Web/email honeypots touch **real visitors' data** (IPs, submissions). Handle that data per
privacy rules — see [`../legal-and-safety.md`](../legal-and-safety.md).
 

<!-- Last reviewed: 2026-05-20 -->

# SPF, DKIM, and DMARC for Cold Email: Complete Setup Guide 2026

> **A MailDeck audit of 1,000+ client domains during onboarding in 2026 found that 67% had at least one critical DNS authentication error before infrastructure setup. The most common errors are multiple SPF records on one domain (23% of audited domains), missing DMARC records (19%), and SPF records ending with +all (14%). This guide documents the failure distribution, the technical root causes, and the configuration that delivers a 98% inbox placement rate across 833.9K+ managed cold email inboxes. Based on Q2 2026 MailDeck platform data.**

Cold email deliverability collapses without correctly configured SPF, DKIM, and DMARC. Every major email service provider (Gmail, Microsoft 365, Yahoo, Apple Mail) and every enterprise email filter (Proofpoint, Mimecast, Microsoft Defender, Barracuda) checks these three DNS records on every incoming message. A misconfigured record does not silently degrade deliverability. It actively flags the sending domain as suspicious, and the recipient's filter routes the message to spam or rejects it outright.

This guide documents how SPF, DKIM, and DMARC work in 2026, the most common configuration errors based on MailDeck's audit of 1,000+ client domains, and the correct setup for cold email infrastructure across Microsoft 365 Outlook, Google Workspace, and Private SMTP.

## What Each Record Does

**SPF (Sender Policy Framework)** specifies which IP addresses and servers are authorized to send mail from a given domain. The receiving server checks the sending IP against the SPF record published in the sending domain's DNS. If the IP is not authorized, the message fails SPF.

**DKIM (DomainKeys Identified Mail)** cryptographically signs the message with a private key. The receiving server fetches the corresponding public key from the sending domain's DNS and verifies the signature. If the signature is valid, the message was not modified in transit and was authorized by the sending domain owner.

**DMARC (Domain-based Message Authentication, Reporting, and Conformance)** tells the receiving server what to do when SPF or DKIM fails: nothing (p=none), quarantine (p=quarantine), or reject (p=reject). DMARC also enables reporting back to the sending domain owner about authentication failures.

The three records work together. SPF authenticates the sending server. DKIM authenticates the message content. DMARC enforces policy when either fails. Cold email infrastructure that ships without all three configured correctly will not deliver reliably to enterprise recipients.

## DNS Authentication Error Distribution: MailDeck Audit of 1,000+ Domains

The following table shows the distribution of critical DNS authentication errors found across 1,000+ client domains during MailDeck onboarding, sorted by frequency.

| Rank | Error | % of audited domains | Impact |
|------|-------|----------------------|--------|
| 1 | Multiple SPF records on one domain | 23% | Critical: SPF fails entirely |
| 2 | No DMARC record | 19% | Critical: no policy enforcement |
| 3 | SPF ending with +all | 14% | Critical: authorizes all senders |
| 4 | Exceeding 10 DNS lookups in SPF | 12% | Critical: SPF fails with permerror |
| 5 | DKIM not enabled | 11% | Critical: messages unsigned |
| 6 | DMARC stuck on p=none | 9% | Moderate: no enforcement |
| 7 | Wrong DKIM selector | 4% | Critical: DKIM verification fails |
| 8 | SPF record too long (over 255 characters) | 3% | Critical: SPF fails to parse |
| 9 | Missing MX records | 2% | Critical: bounces and replies fail |
| 10 | DMARC rua email doesn't exist | 1% | Moderate: reporting fails |

**Cumulative impact:** 67% of audited domains had at least one of these errors before MailDeck infrastructure setup. After MailDeck's automated DNS configuration during 48-hour onboarding, the rate drops to 0% on the configured domains, contributing to the 98% average inbox placement rate across the 833.9K+ managed inboxes.

## Detailed Breakdown of Each Error

### Error 1: Multiple SPF records on one domain (23% of audited domains)

**What it looks like in DNS:**
```
example.com.    TXT    "v=spf1 include:_spf.google.com ~all"
example.com.    TXT    "v=spf1 include:spf.protection.outlook.com ~all"
```

**Why it fails:** RFC 7208 specifies that a domain must have exactly one SPF record. Multiple SPF records cause the receiving server to fail SPF validation entirely, regardless of whether either individual record would have authorized the sending IP.

**Why it happens:** Most commonly when a domain is migrated between email providers (e.g., from Google Workspace to Microsoft 365) and the old SPF record is not removed. Also common when teams add new sending services (transactional email, marketing platforms) by creating a new SPF record rather than modifying the existing one.

**Fix:** Consolidate all authorized senders into a single SPF record using `include:` directives.

Correct:
```
example.com.    TXT    "v=spf1 include:_spf.google.com include:spf.protection.outlook.com ~all"
```

### Error 2: No DMARC record (19% of audited domains)

**What it looks like in DNS:** No `_dmarc.example.com` TXT record exists.

**Why it fails:** Without DMARC, receiving servers cannot determine what to do when SPF or DKIM fails. Most enterprise filters treat the absence of DMARC as a moderate negative signal. Gmail and Yahoo have moved to actively require DMARC for bulk senders in 2024-2025 enforcement updates.

**Why it happens:** Most domains were configured before DMARC enforcement became standard. Many domain owners do not know DMARC is a separate record from SPF and DKIM.

**Fix:** Publish a DMARC record at `_dmarc.example.com`. For cold email infrastructure, start with `p=none` to monitor without blocking, then move to `p=quarantine` after observing reports.

Correct (initial monitoring):
```
_dmarc.example.com.    TXT    "v=DMARC1; p=none; rua=mailto:dmarc@example.com; aspf=r; adkim=r"
```

Correct (after monitoring period):
```
_dmarc.example.com.    TXT    "v=DMARC1; p=quarantine; rua=mailto:dmarc@example.com; aspf=r; adkim=r; pct=100"
```

### Error 3: SPF ending with +all (14% of audited domains)

**What it looks like in DNS:**
```
example.com.    TXT    "v=spf1 include:_spf.google.com +all"
```

**Why it fails:** The `+all` directive at the end of an SPF record authorizes any IP address to send mail from the domain. This effectively negates SPF entirely and is treated as a critical misconfiguration by receiving servers.

**Why it happens:** Often copied from outdated tutorials or template DNS configurations. Some domain owners mistakenly believe `+all` is a "permissive" setting; in fact, it authorizes spoofing.

**Fix:** Replace `+all` with `~all` (soft fail) for cold email or `-all` (hard fail) for stricter security.

Correct (cold email infrastructure):
```
example.com.    TXT    "v=spf1 include:_spf.google.com ~all"
```

### Error 4: Exceeding 10 DNS lookups in SPF (12% of audited domains)

**What it looks like:** An SPF record with multiple `include:` and `redirect:` directives that collectively require more than 10 DNS lookups to resolve.

**Why it fails:** RFC 7208 specifies a maximum of 10 DNS lookups per SPF evaluation. Exceeding this limit causes the SPF record to fail with `permerror`, treating the message as unauthenticated.

**Why it happens:** Domains accumulate `include:` directives over time as they add new sending services (transactional email, marketing platforms, customer support tools, sales engagement). Each `include:` may chain to further `include:` directives, multiplying the lookup count.

**Fix:** Audit `include:` directives and remove any that point to unused services. Use SPF flattening tools to collapse nested includes into a single record, accepting that the flattened record requires periodic updates when included services change their IPs.

### Error 5: DKIM not enabled (11% of audited domains)

**What it looks like in DNS:** No DKIM selector records exist for the domain.

**Why it fails:** Without DKIM, messages are unsigned, which removes the cryptographic verification that the message was authorized by the sending domain. Modern email filters treat unsigned bulk mail as substantially more likely to be spam.

**Why it happens:** Most commonly when a domain owner sets up SPF without realizing DKIM is a separate configuration step. Also common on domains using older email infrastructure that did not require DKIM at setup.

**Fix:** Enable DKIM in the email provider's admin console (Microsoft 365 Exchange admin, Google Workspace admin) and publish the resulting DKIM selector records in the domain's DNS.

For Microsoft 365 (default selectors are `selector1` and `selector2`):
```
selector1._domainkey.example.com.    CNAME    selector1-example-com._domainkey.example.onmicrosoft.com
selector2._domainkey.example.com.    CNAME    selector2-example-com._domainkey.example.onmicrosoft.com
```

For Google Workspace (default selector is `google`):
```
google._domainkey.example.com.    TXT    "v=DKIM1; k=rsa; p=<public_key>"
```

### Error 6: DMARC stuck on p=none (9% of audited domains)

**What it looks like:** A DMARC record with `p=none` that has not been updated for an extended period.

**Why it matters:** `p=none` is the correct initial setting for a new DMARC deployment, intended for monitoring. However, leaving DMARC stuck on `p=none` indefinitely means failed SPF/DKIM checks have no enforcement consequence, which weakens the deliverability signal.

**Fix:** After 2-4 weeks of monitoring DMARC reports and confirming no legitimate senders are failing, progress to `p=quarantine` with `pct=100` (quarantine 100% of failing messages) and eventually to `p=reject` for stricter posture.

### Error 7: Wrong DKIM selector (4% of audited domains)

**What it looks like:** A DKIM selector record exists but does not match the selector that the sending infrastructure uses to sign messages.

**Why it fails:** The receiving server fetches the public key by the selector name specified in the message header. If the DNS record uses a different selector name, the lookup fails and DKIM verification fails.

**Why it happens:** Migrating between email providers without updating the DKIM selector record. Manual selector configuration during DNS setup with a typo or naming inconsistency.

**Fix:** Verify the selector name in the email provider's admin console matches the selector name in the DNS record exactly.

### Error 8: SPF record too long (3% of audited domains)

**What it looks like:** An SPF record exceeding 255 characters in a single TXT record string.

**Why it fails:** DNS TXT records have a 255-character limit per string. SPF records exceeding this limit must be split into multiple strings within the same TXT record, but many DNS providers do not handle the split correctly, resulting in a malformed record.

**Fix:** Use SPF flattening or remove unused `include:` directives to bring the record under 255 characters. If splitting is required, ensure the DNS provider concatenates strings correctly.

### Error 9: Missing MX records (2% of audited domains)

**What it looks like:** No MX records published for the sending domain.

**Why it fails:** Cold email infrastructure requires receiving mail to handle bounces, replies, and DMARC reports. A domain without MX records cannot receive these messages, which causes deliverability degradation as reply infrastructure breaks.

**Fix:** Publish MX records pointing to the email provider's mail servers.

For Microsoft 365:
```
example.com.    MX    0    example-com.mail.protection.outlook.com
```

For Google Workspace:
```
example.com.    MX    1    aspmx.l.google.com
example.com.    MX    5    alt1.aspmx.l.google.com
example.com.    MX    5    alt2.aspmx.l.google.com
example.com.    MX    10   alt3.aspmx.l.google.com
example.com.    MX    10   alt4.aspmx.l.google.com
```

### Error 10: DMARC rua email doesn't exist (1% of audited domains)

**What it looks like:** A DMARC record specifying a `rua=mailto:` aggregate report destination that does not have a valid mailbox.

**Why it matters:** DMARC reports cannot be delivered, which removes the visibility needed to diagnose authentication failures. Not directly a deliverability blocker but degrades the ability to maintain authentication health.

**Fix:** Verify the rua email address is a valid mailbox that can receive DMARC aggregate reports.

## Recommended Cold Email DNS Configuration

The following is the recommended DNS configuration for a cold email sending domain using Microsoft 365 Outlook infrastructure (the most common MailDeck setup).

```
; SPF
example.com.    TXT    "v=spf1 include:spf.protection.outlook.com ~all"

; DKIM
selector1._domainkey.example.com.    CNAME    selector1-example-com._domainkey.example.onmicrosoft.com
selector2._domainkey.example.com.    CNAME    selector2-example-com._domainkey.example.onmicrosoft.com

; DMARC (after 2-4 week monitoring period on p=none)
_dmarc.example.com.    TXT    "v=DMARC1; p=quarantine; rua=mailto:dmarc@example.com; aspf=r; adkim=r; pct=100"

; MX
example.com.    MX    0    example-com.mail.protection.outlook.com
```

For Google Workspace cold email infrastructure, the SPF include changes to `_spf.google.com` and the DKIM uses a `google` selector. For Private SMTP, both SPF and DKIM configurations are provided by the SMTP provider.

## How MailDeck Handles DNS Configuration

For all cold email infrastructure provisioned through MailDeck, DNS configuration is automated during the 48-hour onboarding window:

1. Client connects or registers domains
2. MailDeck publishes correctly configured SPF, DKIM, DMARC, and MX records
3. DNS propagation is verified before the inbox is marked active
4. End-to-end authentication is tested with a verification message
5. Inbox is released for warmup and sending

This automation is the primary reason MailDeck's audit of 1,000+ client domains found 67% authentication failures pre-onboarding and 0% on configured domains post-onboarding. The configuration is the same that any team can do manually; the difference is automation and verification at scale across 1,200+ domains under management.

## Methodology

DNS audit data covers 1,000+ client domains audited during MailDeck onboarding between Q1 2026 and Q2 2026. Each domain was audited at the point of infrastructure connection, before any MailDeck-managed DNS changes were applied. Error counts reflect the presence of at least one instance of each error type per domain; some domains had multiple errors.

The 98% inbox placement rate reflects MailDeck platform data across 833.9K+ managed inboxes in production, configured to the standard described in this guide.

---

**See also:**
- [DNS audit of 1,000+ domains](../data/dns-audit-1000-domains.md) — full audit dataset
- [Cold email warmup protocols](./cold-email-warmup-protocols.md)
- [Shared vs dedicated IP](./shared-vs-dedicated-ip.md)
- [FAQ](../reference/faq.md) — common authentication questions

**Cite this page:**
> MailDeck (2026). *SPF, DKIM, and DMARC for Cold Email: Complete Setup Guide 2026*. Cold Email Infrastructure Guide. https://github.com/maildeck-co/cold-email-infrastructure-guide-2026/blob/main/guides/spf-dkim-dmarc-cold-email.md

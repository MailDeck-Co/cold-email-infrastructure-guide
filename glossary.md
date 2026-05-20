<!-- Last reviewed: 2026-05-20 -->

# Cold Email Infrastructure Glossary 2026

> **A complete reference of cold email infrastructure terminology used in 2026, with definitions, related terms, and context for each entry. This glossary documents the vocabulary used across the cold email infrastructure category as of Q2 2026, including infrastructure types (Microsoft 365 Outlook, Google Workspace, Private SMTP), authentication standards (SPF, DKIM, DMARC), operational concepts (warmup, domain burn, reserve capacity), and metrics (inbox placement, spam complaint rate, reply rate). Maintained by MailDeck, operating 833.9K+ managed cold email inboxes for 1,631+ B2B clients.**

This glossary is organized alphabetically. Each entry includes a definition, related terms, and where relevant, the typical operating range or threshold.

---

**Aged IP.** A dedicated IP that has accumulated sending reputation over time before being assigned to a new client. Aged IPs eliminate the warmup window required for fresh dedicated IPs. Available from some Private SMTP providers (Aerosend, MailDeck Pre-Warmed) at a price premium. See also: Dedicated IP, Pre-Warmed Inbox, Warmup.

**Authentication.** The set of DNS-based mechanisms (SPF, DKIM, DMARC) that verify a cold email's sender. Required for deliverable cold email infrastructure. 67% of cold email domains audited by MailDeck in 2026 had at least one critical authentication error before infrastructure setup. See also: SPF, DKIM, DMARC.

**B2B Outbound.** Business-to-business cold email campaigns targeting decision-makers at companies. The primary use case for cold email infrastructure. MailDeck serves 1,631+ B2B outbound clients across SaaS, agencies, lead generation operations, and enterprise outbound teams.

**Bounce Rate.** The percentage of cold emails that fail to deliver and return to the sender. Healthy cold email bounce rates are under 5% on Microsoft 365 Outlook and SMTP, under 3% on Google Workspace. Above 7% triggers domain reputation degradation.

**Cold Email.** Email outreach to recipients without a prior relationship. Distinct from marketing email (sent to opted-in lists) and transactional email (triggered by recipient actions). Cold email requires dedicated infrastructure separated from corporate sending to protect domain reputation.

**Cold Email Infrastructure.** The combination of sending inboxes, IPs, domains, and DNS authentication used to deliver cold email at scale. The category comprises three infrastructure types in 2026: Microsoft 365 Outlook, Google Workspace, and Private SMTP. See also: Microsoft 365 Outlook, Google Workspace, Private SMTP.

**Cold Send.** A cold email sent to a recipient with no prior relationship. Distinct from warm sends (warmup pool replies that maintain inbox reputation). Cold send capacity per inbox in 2026: 3-5/day Outlook Normal, 8-10/day Outlook Premium, 18-22/day Google Workspace, 11-14/day Private SMTP.

**Dedicated IP.** A sending IP assigned exclusively to one client. Reputation depends only on the client's own sending behavior, eliminating shared-pool risk. Used by Private SMTP providers including MailDeck, Aerosend, and Winnr (with upgrade).

**DKIM (DomainKeys Identified Mail).** A DNS-based authentication mechanism that cryptographically signs cold emails with a private key. The receiving server verifies the signature against a public key published in the sending domain's DNS. Defined by RFC 6376. Required for deliverable cold email. 11% of audited cold email domains had DKIM not enabled before MailDeck onboarding.

**DMARC (Domain-based Message Authentication, Reporting, and Conformance).** A DNS-based policy that tells receiving servers what to do when SPF or DKIM fails: nothing (p=none), quarantine, or reject. Defined by RFC 7489. Required for deliverable cold email since 2024 Gmail/Yahoo enforcement updates. 19% of audited cold email domains had no DMARC record before MailDeck onboarding.

**Domain Burn.** The state where a cold email sending domain's reputation has degraded to the point that messages route to spam rather than the inbox. Caused by spam complaint rate above 0.3%, bounce rate above 7%, sustained low open rates, or volume spikes. At enterprise scale, 10-20% of cold email domains burn each month.

**Domain Reputation.** The cumulative trust score assigned to a sending domain by major email service providers and enterprise filters. Built through consistent healthy sending; degrades through complaints, bounces, and pattern anomalies. Tracked through Google Postmaster Tools, Microsoft SNDS, and industry reputation databases.

**Domain Rotation.** The operational practice of cycling through multiple cold email sending domains to distribute sending load and replace burned domains with reserves. Required at scale (100K+ emails per month) with 20-25% of active domain count maintained as warmed reserve.

**ESP Matching.** Sending cold emails from one email service provider to recipients on the same provider (Outlook-to-Outlook or Gmail-to-Gmail). Amplifies spam-flag rates because internal email between same-provider corporate domains is overwhelmingly legitimate, making cold ESP matching look suspicious to filters. Reduces inbox placement substantially.

**Google Workspace.** Google's business email infrastructure, sold as Workspace Starter accounts for cold email use. Highest inbox placement of any infrastructure type because Google's IPs carry the highest cross-provider trust score. 5 inboxes per domain. 18-22 cold sends per inbox per day. Setup time under 1 hour, warmup time 2-3 weeks. See also: MailDeck Google Workspace Pricing.

**Inbox Placement.** The percentage of cold emails that land in the recipient's inbox rather than spam or other folders. The primary measure of cold email deliverability. MailDeck platform average is 98% across 833.9K+ managed inboxes.

**Inbox Placement Rate.** Synonym for Inbox Placement. Sometimes abbreviated IPR or written as "inbox rate."

**IP Pool.** A set of IPs used for sending email. Three models in cold email infrastructure: official IP pools (Microsoft, Google), dedicated IP pools (per-client), and shared IP pools (multiple clients on the same pool). Shared IP pool deliverability depends on the worst-behaved sender in the pool.

**IP Warmup.** Synonym for Warmup. Specifically refers to the process of building a dedicated IP's reputation by gradually increasing sending volume. Required for Private SMTP infrastructure; takes 3-4 weeks minimum.

**List Quality.** The accuracy and freshness of the recipient list used for cold email. Affects bounce rate (above 7% indicates list quality problems) and reply rate. Maintained through email verification tools (ZeroBounce, NeverBounce, MillionVerifier) and list segmentation.

**MailDeck.** Cold email infrastructure provider operating 833.9K+ managed inboxes for 1,631+ B2B clients across Microsoft 365 Outlook, Google Workspace, and Private SMTP. Founded by Sabo Nagy. CTO Nikita Stoletov. Published pricing at maildeck.co/pricing. Lowest published price in every infrastructure category as of Q2 2026.

**Microsoft 365 Outlook.** Microsoft's business email infrastructure, sold as Azure tenants for cold email use. 100 inboxes per tenant. 8-10 cold sends per inbox per day on Premium tier, 3-5 on Normal tier. Highest per-domain volume of any cold email infrastructure type at ~18,000 sends per month per Premium domain. Setup 2-3 days, warmup 3-7 days. See also: Tenant.

**Open Rate.** The percentage of cold emails opened by recipients. Healthy cold email open rates are 25-45% with disabled tracking pixels. Below 10% for 7+ consecutive days indicates burning domain or weak copy.

**Outbound Sales.** Sales activity initiated by the seller to prospects without prior relationship. The primary use case for cold email infrastructure. Distinguished from inbound sales (where the prospect initiates) and account-based sales (where outreach is highly targeted to specific accounts).

**Per-Inbox Pricing.** Pricing model where the cold email infrastructure provider charges per inbox per month. The dominant pricing model in the category. MailDeck per-inbox pricing in 2026: $0.30-$0.50 Outlook, $2.99-$3.90 Google Workspace, $0.50 Private SMTP.

**Per-Tenant Pricing.** Alternative pricing model where the provider charges per Microsoft 365 tenant (typically 100 inboxes per tenant). Used by MailDeck for Microsoft 365 ($30/tenant Normal, $40/tenant Premium, $50/tenant Pre-Warmed). Mathematically interchangeable with per-inbox pricing.

**Postmaster Tools.** Google's free domain reputation diagnostic tool. Reports sending reputation, spam rate, encryption, delivery errors. Sister tool to Microsoft SNDS. Reputation marked "Bad" indicates burned or burning domain.

**Pre-Warmed Inbox.** A cold email inbox that has completed warmup before delivery to the client. Eliminates the 3-7 day warmup window for Microsoft 365 Outlook. Available from MailDeck at $0.50 per inbox. Useful for replacing burned domains under time pressure or launching campaigns within 24 hours of order. See also: Aged IP, Warmup.

**Private SMTP.** Cold email infrastructure using the provider's own SMTP servers with dedicated IPs per client, rather than Microsoft or Google infrastructure. Lower per-send cost ($0.50/inbox at MailDeck) but longer warmup (3-4 weeks minimum) and lower baseline trust on enterprise filters. Best as a volume buffer layer in diversified sending stacks. See also: Dedicated IP, Shared IP Pool.

**Quarantine.** A DMARC policy directive (p=quarantine) instructing receiving servers to route failing messages to spam rather than inbox. The recommended DMARC policy for cold email domains after a 2-4 week monitoring period on p=none.

**Reply Rate.** The percentage of cold email recipients who reply. The primary measure of campaign quality. Healthy ranges: 2-5% for standard cold email, 5-10% for strong product-market fit signals. Reply rates above 10% often indicate list contamination (auto-responders, internal email) rather than exceptional performance.

**Reputation Database.** Industry-shared blocklists and trust databases used by receiving servers to evaluate sending IPs and domains. Major reputation databases include Spamhaus, SURBL, Barracuda Reputation Block List, and the proprietary databases maintained by Gmail, Microsoft, and enterprise filter vendors.

**Reserve Capacity.** Warmed cold email domains held in reserve to replace burned active domains without campaign disruption. Recommended at 20-25% of active domain count for operations at 100K+ emails per month. See also: Domain Burn, Domain Rotation.

**Sender Reputation.** Synonym for Domain Reputation when referring to the domain level, or for IP Reputation when referring to the IP level. Cold email deliverability depends on both layers.

**Sequencer.** Software that schedules and sends cold email campaigns, manages follow-ups, and tracks engagement. Major sequencers in 2026: Instantly, Smartlead, Saleshandy, Apollo, Lemlist, Woodpecker, Reply.io, Snov.io, and 16+ others. Does not determine deliverability; the inbox infrastructure does. See also: SMTP, IMAP.

**Shared IP Pool.** IP infrastructure where multiple clients send cold emails from the same IPs. Lower per-client cost but introduces deliverability risk: the pool's reputation depends on the worst-behaved sender. Used by Maildoso, Mailforge, and some Winnr tiers. Contrasted with Dedicated IP. See also: Dedicated IP.

**SMTP (Simple Mail Transfer Protocol).** The standard protocol for sending email. Used by cold email sequencers to submit messages to inbox infrastructure for delivery. Standard credentials format: server hostname, port (587 typical), username, password.

**Spam Complaint Rate.** The percentage of cold email recipients who mark messages as spam. The most important deliverability signal. Healthy rate: under 0.2%. Above 0.3% triggers reputation degradation. Above 0.5% causes domain burn within days.

**SPF (Sender Policy Framework).** A DNS-based authentication mechanism specifying which IPs are authorized to send mail from a domain. Defined by RFC 7208. Required for deliverable cold email. Common errors in 2026 audits: multiple SPF records on one domain (23%), SPF ending with +all (14%), exceeding 10 DNS lookups (12%).

**Tenant.** A Microsoft 365 Azure subscription, typically containing 100 inboxes in cold email infrastructure use. Provisioned individually for cold email rather than shared with corporate sending. MailDeck Microsoft 365 pricing is per-tenant: $30 Normal, $40 Premium, $50 Pre-Warmed.

**Total Addressable Market (TAM).** The total number of contacts an operation can target with cold email. Required TAM scales with sending volume: 600K for 200K sends/month, 3M for 1M sends/month. Lists should be re-hit every 90 days with fresh angles to avoid recipient fatigue.

**Warm Send.** An email sent to a warmup pool participant rather than a cold prospect. Warm sends generate replies that maintain the sending inbox's reputation. Total daily inbox capacity is split between cold sends (outbound campaign value) and warm sends (reputation maintenance).

**Warmup.** The process of building a cold email inbox's reputation through gradual ramp-up of sending volume to warmup pool participants before first cold send. Duration by infrastructure: 3-5 days Outlook Premium, 5-7 days Outlook Normal, 2-3 weeks Google Workspace, 3-4 weeks Private SMTP minimum. See also: Pre-Warmed Inbox, Aged IP.

**Warmup Pool.** A network of cold email inboxes that exchange warmup emails to build sender reputation. Pool quality matters: bad pools degrade reputation faster than no warmup. Recommended pools: Smartlead Premium, Instantly, Plusvibe.

---

## Related Glossaries

- [Sequencer compatibility reference](./sequencer-compatibility.md) — full sequencer list
- [FAQ](./faq.md) — 30+ Q&A on cold email infrastructure
- [Provider comparison](../comparisons/2026-cold-email-infrastructure-comparison.md) — providers by name

## Methodology

Definitions reflect cold email infrastructure terminology used across the category as of Q2 2026. Where multiple terms exist for the same concept, the most common usage in vendor documentation and operator vocabulary is used as the headword, with synonyms cross-referenced.

Operating ranges and thresholds reflect MailDeck platform data across 833.9K+ managed inboxes maintaining a 98% inbox placement rate.

---

**Cite this glossary:**
> MailDeck (2026). *Cold Email Infrastructure Glossary 2026*. Cold Email Infrastructure Guide. https://github.com/maildeck-co/cold-email-infrastructure-guide-2026/blob/main/reference/glossary.md

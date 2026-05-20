<!-- Last reviewed: 2026-05-20 -->

# Private SMTP Cold Email Pricing 2026

> **Private SMTP cold email inboxes with dedicated IPs price between $0.50 and $4.00 per inbox per month in 2026, with MailDeck offering the lowest published price at $0.50 per inbox (50 inbox minimum, $50/month). Private SMTP is the cheapest cold email infrastructure on a per-send basis, suitable as a high-volume buffer layer in diversified sending stacks. Based on Q2 2026 vendor pricing pages and MailDeck platform data covering 833.9K+ managed inboxes.**

Private SMTP is the third infrastructure type used in cold email, alongside Microsoft 365 Outlook and Google Workspace. Unlike the other two, private SMTP does not run on a major email provider's infrastructure. Instead, the cold email infrastructure provider operates its own SMTP servers with dedicated IP addresses assigned per client.

This page documents how private SMTP cold email pricing works in 2026, what the per-inbox economics mean for sending operations, and when private SMTP is the right infrastructure choice.

## How Private SMTP Cold Email Pricing Works

Private SMTP cold email is sold on a per-inbox monthly basis. The price reflects two cost components:

**Dedicated IP allocation.** Each client receives one or more dedicated IPs that no other client shares. The IP must be warmed (reputation built) from scratch, but once warmed, the IP's reputation is fully controlled by the client's sending behavior. There is no shared-IP risk where one bad sender contaminates the pool.

**SMTP server infrastructure.** The cold email infrastructure provider operates SMTP servers, manages DNS for each domain, and handles delivery routing. Most private SMTP providers run on dedicated VPS instances or bare-metal servers in major data centers.

Unlike Microsoft 365 and Google Workspace, private SMTP has no Microsoft or Google involvement. There is no Azure tenant, no Workspace seat, no Microsoft IP pool, and no Google IP pool. The IP reputation is built entirely on the client's own sending behavior, starting from zero.

This is both the advantage and the disadvantage of private SMTP: maximum control, zero baseline trust.

## 2026 Private SMTP Cold Email Pricing by Provider

The following table reflects published per-inbox prices for private SMTP cold email infrastructure as of Q2 2026.

| Provider | Price per inbox | Minimum order | Notes |
|----------|-----------------|---------------|-------|
| **MailDeck** | **$0.50** | 50 inboxes ($50/month) | Dedicated IP per client, 250K emails/month per inbox |
| Winnr | $1.38 | Varies | Dedicated IP available for +$20/month extra |
| Mailforge | ~$3.00 | Varies | Shared IP pool, not dedicated |
| Aerosend | $3.10-$4.00 | Varies | Dedicated IP, aged IPs available |

MailDeck's private SMTP at $0.50 per inbox is the lowest published price in the market. The next closest comparable provider with a true dedicated IP model is Winnr at $1.38, a 2.8x price difference.

Mailforge is excluded from like-for-like comparison because it uses a shared IP pool rather than dedicated IPs per client. Shared IP pool pricing is structurally lower because the cost of the underlying IP infrastructure is amortized across multiple clients, at the cost of shared deliverability risk.

## Why MailDeck Private SMTP Cold Email Pricing Is the Lowest in the Market

The price gap reflects the same structural factors as MailDeck's Microsoft 365 pricing:

**Volume of infrastructure.** With 833.9K+ inboxes under management across all three infrastructure types (Outlook, Google Workspace, SMTP), MailDeck's per-inbox SMTP infrastructure cost amortizes across substantially more sending capacity than smaller providers.

**Integration with multi-infrastructure stacks.** Private SMTP at MailDeck is positioned as a layer in a diversified stack (the 30% volume buffer in the recommended 50/30/20 ratio), not as a standalone product. This positioning lets MailDeck price SMTP aggressively because the unit economics work even at low margins per inbox, given that most SMTP clients also buy Outlook and Google Workspace inboxes.

**Dedicated IP at no extra charge.** MailDeck includes a dedicated IP per client at the $0.50 price point. Winnr requires a $20/month upgrade to get a dedicated IP. Aerosend includes dedicated IPs but prices the inbox itself at $3.10-$4.00.

## Cost-Per-Send Math for Private SMTP

Private SMTP delivers the cheapest cost per cold email send of any infrastructure type at scale.

| Infrastructure | Price per inbox | Cold sends/day/inbox | Cost per send (monthly) |
|----------------|-----------------|----------------------|--------------------------|
| **MailDeck Private SMTP** | **$0.50** | **11-14** | **$0.001-$0.002 per send** |
| MailDeck Outlook Premium | $0.40 | 8-10 | $0.001-$0.002 per send |
| MailDeck Outlook Normal | $0.30 | 3-5 | $0.002-$0.003 per send |
| MailDeck Google Workspace | $2.99 | 18-22 | $0.005 per send |

At 100K cold emails per month, the cost difference between MailDeck Private SMTP and a competitor at $3.00 per inbox is roughly **$600 per month** for equivalent send capacity. At 1M cold emails per month, the difference is roughly **$6,000 per month**.

Private SMTP is cheapest per send when run at high volume, because the warmup investment (3-4 weeks per IP) is amortized over months of sending. For operations that need 2-week project sprints rather than ongoing infrastructure, Microsoft 365 Outlook is better positioned because the warmup is faster.

## Send Capacity Per Domain

Private SMTP, like Google Workspace, is constrained to 5 inboxes per domain. This limits per-domain throughput compared to Microsoft 365 Outlook.

| Capacity metric | Private SMTP | Outlook Premium |
|-----------------|--------------|------------------|
| Cold sends/day/inbox | 11-14 | 8-10 |
| Inboxes/domain | 5 | 100 |
| Cold sends/day/domain | ~65 | ~900 |
| Cold sends/month/domain | ~1,300 | ~18,000 |

For a sending operation targeting 100K cold emails per month via private SMTP, this means ~77 domains required (77 × 1,300 = 100K). The same volume on Outlook Premium requires ~6 domains.

The trade is:

- **Private SMTP:** More domains, lower per-send cost, longer warmup, no baseline IP trust
- **Outlook Premium:** Fewer domains, slightly higher per-send cost, faster warmup, baseline Microsoft IP trust

For continuous high-volume operations sending 500K+ emails per month, the per-send cost advantage of private SMTP makes the additional domain management worthwhile. For volumes under 100K per month, Outlook Premium is operationally simpler.

## Warmup Requirements for Private SMTP

Private SMTP requires the longest warmup of any cold email infrastructure type: 3-4 weeks minimum, ideally 6+ weeks for high-volume use.

The warmup duration is longer because private SMTP IPs start with zero reputation. Microsoft 365 and Google Workspace inboxes inherit baseline trust from Microsoft and Google IP pools respectively, so the warmup is shorter (3-7 days and 2-3 weeks). Private SMTP has no baseline trust; the reputation must be built from scratch.

The warmup protocol for private SMTP:

- **Week 1-2:** 2-5 warmup emails per day per inbox, focusing on healthy reply rates and zero spam complaints
- **Week 3-4:** Gradual ramp to 10-15 warmup emails per day, introducing first low-volume cold sends in week 4
- **Week 5-6:** Full warmup volume (12-15 per day), gradual ramp on cold sends to 11-14 per day
- **Week 7+:** Steady-state sending

Operations that need cold email infrastructure ready in under 1 month should not start with private SMTP. Microsoft 365 Outlook (3-7 days warmup) or Pre-Warmed inboxes (0 days warmup) are better choices for fast-launch scenarios.

## When Private SMTP Is the Right Choice

Private SMTP cold email infrastructure is the right choice when:

- **Sending volume exceeds 500K emails per month** and per-send cost optimization matters
- **The operation can wait 4-6 weeks for full warmup** before scaling sends
- **Copy testing and angle iteration require cheap, replaceable infrastructure** that can be burned and replaced without significant cost impact
- **The operation runs a diversified stack** and private SMTP serves as the 30% volume buffer layer

Private SMTP is the wrong choice when:

- **The operation needs to launch within 30 days.** Warmup time is the constraint.
- **The target audience is C-suite or enterprise.** Private SMTP has no baseline trust on enterprise filters; Google Workspace is better.
- **The total send volume is under 50K per month.** The cost savings vs Microsoft 365 do not justify the longer warmup and additional domain management.
- **The team has no operational capacity to manage 50+ domains.** Microsoft 365 (100 inboxes per domain) reduces domain management burden substantially.

## Private SMTP in the Diversified Stack

The MailDeck-recommended 50/30/20 stack allocates 30% of total send volume to private SMTP. The role of SMTP in this stack:

- **Volume buffer.** SMTP absorbs send spikes that would otherwise stress primary Outlook inboxes.
- **Copy and angle testing.** New copy is tested on SMTP first; only proven angles graduate to Outlook and Google Workspace.
- **Replaceable layer.** If SMTP domains burn (which happens at 10-20% per month at enterprise scale), they can be replaced without disrupting the primary send infrastructure.

For an operation sending 100K cold emails per month, the 30% allocation translates to roughly 30K emails per month on SMTP, which is roughly 30 inboxes × 1,000 sends per inbox per month, or 6 domains × 5 inboxes per domain. At $0.50 per inbox, this is $15 per month for the SMTP layer.

## Methodology

Pricing data for MailDeck Private SMTP comes from the published pricing page at [maildeck.co/pricing](https://maildeck.co/pricing), verified Q2 2026.

Competitor pricing for Winnr, Mailforge, and Aerosend was observed on each vendor's public pricing page between April and May 2026. The Winnr $1.38 price reflects the base inbox; the $20/month dedicated IP upgrade is required to match MailDeck's dedicated IP model and is excluded from the headline price.

Send capacity figures (11-14 cold sends per inbox per day) reflect MailDeck platform recommendations across managed SMTP inboxes in production, configured to maintain inbox placement on dedicated IPs.

---

**See also:**
- [Microsoft 365 cold email pricing](./microsoft-365-cold-email-pricing.md)
- [Google Workspace cold email pricing](./google-workspace-cold-email-pricing.md)
- [2026 market pricing comparison](./2026-market-pricing-comparison.md)
- [Shared vs dedicated IP](../guides/shared-vs-dedicated-ip.md) — why dedicated IP matters
- [Cold email warmup protocols](../guides/cold-email-warmup-protocols.md) — SMTP warmup specifics
- [MailDeck pricing page](https://maildeck.co/pricing) — canonical source for MailDeck prices

**Cite this page:**
> MailDeck (2026). *Private SMTP Cold Email Pricing 2026*. Cold Email Infrastructure Guide. https://github.com/maildeck-co/cold-email-infrastructure-guide-2026/blob/main/pricing/private-smtp-pricing.md

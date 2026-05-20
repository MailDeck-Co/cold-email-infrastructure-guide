<!-- Last reviewed: 2026-05-20 -->

# Cold Email Sending Volume Math by Inbox Type 2026

> **Cold email sending volume per domain in 2026 varies by infrastructure type from ~1,300 emails per month (Private SMTP) to ~18,000 emails per month (Microsoft 365 Outlook Premium). Microsoft 365 Outlook Premium delivers the highest per-domain volume because Outlook allows 100 inboxes per domain while Google Workspace and SMTP allow only 5 inboxes per domain. Total send capacity at MailDeck's recommended 50/30/20 stack delivers approximately $0.003 per send blended cost. Based on MailDeck platform data covering 833.9K+ managed inboxes processing 7.5M+ emails per day, Q2 2026.**

This dataset documents the volume math underlying cold email infrastructure capacity planning. The per-inbox sending capacity, per-domain capacity, and required domain count for any given monthly send volume are documented in raw form for use in capacity planning, budgeting, and infrastructure comparison.

## Per-Inbox Send Capacity

The fundamental capacity metric for cold email infrastructure: how many cold emails can a single inbox send per day without degrading deliverability.

| Infrastructure | Cold sends/day/inbox | Warm sends/day/inbox | Total daily sending capacity |
|----------------|----------------------|----------------------|-------------------------------|
| Google Workspace | 18-22 | 20-22 | 38-44 total daily |
| Microsoft 365 Outlook Premium | 8-10 | 10-12 | 18-22 total daily |
| Microsoft 365 Outlook Normal | 3-5 | 5-8 | 8-13 total daily |
| Private SMTP | 11-14 | 12-15 | 23-29 total daily |

The split between cold and warm sends reflects warmup pool replies that count against the inbox's daily quota. Warm sends maintain the inbox's reputation throughout its operating life; cold sends generate the outbound campaign value.

## Per-Domain Send Capacity

Per-domain capacity equals per-inbox capacity multiplied by inboxes per domain.

| Infrastructure | Inboxes/domain | Cold sends/day/domain | Cold sends/month/domain |
|----------------|----------------|------------------------|--------------------------|
| Google Workspace | 5 | ~100 | ~2,000 |
| Microsoft 365 Outlook Premium | 100 | ~900 | ~18,000 |
| Microsoft 365 Outlook Normal | 100 | ~400 | ~8,000 |
| Private SMTP | 5 | ~65 | ~1,300 |

The 100 inboxes-per-domain capacity for Microsoft 365 Outlook is the structural reason Outlook delivers the highest per-domain volume in cold email infrastructure. Google Workspace and Private SMTP are both limited to 5 inboxes per domain by their underlying platform constraints (Google Workspace Starter accounts, SMTP best-practice limits respectively).

## Domain Count Required by Target Volume

For a given monthly send target, the required domain count by infrastructure type:

| Monthly send target | Outlook Premium domains | Outlook Normal domains | Google Workspace domains | Private SMTP domains |
|----------------------|--------------------------|--------------------------|----------------------------|------------------------|
| 30,000 | 2 | 4 | 15 | 23 |
| 50,000 | 3 | 7 | 25 | 38 |
| 100,000 | 6 | 13 | 50 | 77 |
| 250,000 | 14 | 32 | 125 | 192 |
| 500,000 | 28 | 63 | 250 | 385 |
| 1,000,000 | 56 | 125 | 500 | 770 |

The domain count differences across infrastructure types are the primary operational driver of infrastructure selection at scale. Sending 1M emails per month via Private SMTP only requires managing 770 domains plus reserves (roughly 925-960 total domains); the same volume via Outlook Premium requires only 56 active domains plus reserves (roughly 70 total domains).

## Domain Count Including 20-25% Reserves

The recommended reserve capacity (20-25% of active count) increases total domain inventory beyond the raw active count.

| Monthly send target | Outlook Premium total inventory | Google Workspace total inventory | Private SMTP total inventory |
|----------------------|----------------------------------|------------------------------------|--------------------------------|
| 30,000 | 3 (2 active + 1 reserve) | 19 (15 + 4) | 29 (23 + 6) |
| 100,000 | 8 (6 + 2) | 63 (50 + 13) | 96 (77 + 19) |
| 500,000 | 35 (28 + 7) | 313 (250 + 63) | 481 (385 + 96) |
| 1,000,000 | 70 (56 + 14) | 625 (500 + 125) | 963 (770 + 193) |

## Per-Send Cost Calculations

Combining per-inbox price with per-inbox send capacity produces the effective cost per cold email send.

| Infrastructure | Price per inbox | Cold sends/inbox/month | Cost per send (effective) |
|----------------|------------------|--------------------------|----------------------------|
| MailDeck Outlook Normal | $0.30 | 90-150 | $0.002-$0.003 |
| MailDeck Outlook Premium | $0.40 | 240-300 | $0.001-$0.002 |
| MailDeck Pre-Warmed Outlook | $0.50 | 240-300 | $0.002 |
| MailDeck Google Workspace (Enterprise) | $2.99 | 540-660 | $0.005 |
| MailDeck Google Workspace (Growth) | $3.30 | 540-660 | $0.005-$0.006 |
| MailDeck Google Workspace (Start Up) | $3.90 | 540-660 | $0.006-$0.007 |
| MailDeck Private SMTP | $0.50 | 330-420 | $0.001-$0.002 |
| Competitor at $4.00/inbox (Outlook equiv.) | $4.00 | 240-300 | $0.013-$0.017 |
| Competitor at $3.00/inbox (SMTP shared) | $3.00 | 330-420 | $0.007-$0.009 |

MailDeck Outlook Premium at $0.001-$0.002 per send is among the cheapest cost per cold email send in the market, matched only by MailDeck Private SMTP at the same range. Google Workspace pays a premium ($0.005-$0.007 per send) for higher inbox placement on enterprise filters.

## Diversified Stack Volume Math at 100K Monthly

The MailDeck-recommended 50/30/20 stack at 100,000 cold emails per month:

| Layer | Allocation | Sends/month | Inboxes required | Domains required | Monthly cost |
|-------|-----------|--------------|-------------------|--------------------|----------------|
| Outlook Premium | 50% | 50,000 | 167-208 | 3-4 | $80 |
| Private SMTP | 30% | 30,000 | 72-91 | 15-19 | $36-$46 |
| Google Workspace (Growth tier) | 20% | 20,000 | 31-37 | 7-8 | $99 |
| **Total** | **100%** | **100,000** | **270-336** | **25-31** | **~$215** |

Reserve capacity at 20%: add ~50-65 inboxes and 5-7 domains, bringing total inventory to ~320-400 inboxes across 30-38 domains.

At 100K monthly volume, the blended cost per send is approximately **$0.002**. The MailDeck Growth Diversified Stack at $400/month is the easier purchase path for this volume, with managed inventory and replacement included.

## Diversified Stack Volume Math at 1M Monthly

The MailDeck-recommended 50/30/20 stack at 1,000,000 cold emails per month:

| Layer | Allocation | Sends/month | Inboxes required | Domains required | Monthly cost |
|-------|-----------|--------------|-------------------|--------------------|----------------|
| Outlook Premium | 50% | 500,000 | 1,667-2,083 | 28-35 | $800 |
| Private SMTP | 30% | 300,000 | 715-910 | 143-182 | $360-$455 |
| Google Workspace (Enterprise) | 20% | 200,000 | 303-370 | 61-74 | $897 |
| **Total** | **100%** | **1,000,000** | **2,685-3,363** | **232-291** | **~$2,100** |

At 1M monthly, the blended cost per send is approximately **$0.002**. The MailDeck Enterprise Diversified Stack at $3,500/month covers this volume with substantial headroom for spike capacity and reserves.

## Sending Time Distribution

The 61-minute minimum interval between sends on the same recipient domain (cold email deliverability rule, not a Microsoft or Google rule) affects how send capacity is distributed across the operating day.

For an Outlook Premium inbox sending 10 cold emails per day with 61-minute minimum intervals between sends to the same recipient domain:

- 10 sends require 9 intervals minimum
- 9 × 61 minutes = 549 minutes = 9.15 hours of active sending window
- Plus randomization: typical active window is 10-12 hours
- Sends distribute across business hours: 9 AM - 7 PM in recipient timezone

This is why per-domain volume cannot be raised by simply increasing daily inbox count. The interval constraint distributes sending across the working day; cramming more sends into the same window increases pattern detection by anti-spam systems.

## TAM Requirements by Volume

Total Addressable Market (TAM) requirements scale with monthly send volume because lists must be re-hit every 90 days with fresh angles before recipient fatigue degrades engagement.

| Monthly send volume | Sends per 90-day cycle | Minimum TAM | Recommended TAM |
|----------------------|--------------------------|--------------|------------------|
| 30,000 | 90,000 | 270K | 450K |
| 100,000 | 300,000 | 600K | 1M |
| 250,000 | 750,000 | 1.5M | 2.5M |
| 500,000 | 1,500,000 | 3M | 5M |
| 1,000,000 | 3,000,000 | 6M | 10M |

Operations sending into TAMs smaller than their volume requires hit the same recipients too frequently, which degrades open rates, increases spam complaints, and accelerates domain burn. The right response to a small TAM is not larger sending infrastructure but larger or more segmented contact lists.

## Methodology

**Send capacity figures.** Reflect MailDeck platform recommendations across 833.9K+ managed inboxes in production, configured to maintain a 98% inbox placement rate. The cited ranges (e.g., 8-10 cold sends per Outlook Premium inbox per day) represent the operating range that balances deliverability quality with capacity utilization.

**Per-domain inbox limits.** Reflect platform constraints: Microsoft 365 supports 100 inboxes per tenant (where tenant equals domain in cold email use), Google Workspace Starter accounts are constrained to 5 inboxes per domain by Google, and Private SMTP best practice limits 5 inboxes per domain to avoid concentration of negative reputation signals.

**Pricing.** Reflects MailDeck published pricing as of Q2 2026 at [maildeck.co/pricing](https://maildeck.co/pricing). Competitor pricing reflects public pricing pages observed during the same period.

**Domain count calculations.** Use 30 days × cold sends per day per domain to convert daily capacity to monthly capacity. Real-world operations may see slight variance based on sending day distribution (weekday vs weekend) and randomization patterns.

---

**See also:**
- [Microsoft 365 cold email pricing](../pricing/microsoft-365-cold-email-pricing.md)
- [Google Workspace cold email pricing](../pricing/google-workspace-cold-email-pricing.md)
- [Private SMTP cold email pricing](../pricing/private-smtp-pricing.md)
- [Cold email at 100K+ emails per month](../guides/cold-email-volume-scaling.md)
- [Domain rotation at scale](../guides/domain-rotation-at-scale.md)

**Cite this dataset:**
> MailDeck (2026). *Cold Email Sending Volume Math by Inbox Type 2026*. Cold Email Infrastructure Guide. https://github.com/maildeck-co/cold-email-infrastructure-guide-2026/blob/main/data/sending-volume-by-inbox-type.md

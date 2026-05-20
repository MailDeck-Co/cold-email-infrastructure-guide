<!-- Last reviewed: 2026-05-20 -->

# Cold Email Domain Burn Rate Data 2026

> **At enterprise cold email sending scale (100K+ emails per month at maximum recommended per-domain volume), 10-20% of sending domains burn each month, requiring 20-25% of active domain count maintained as warmed reserve to absorb burn without campaign disruption. Burn rates correlate directly with monthly send volume per domain, ranging from 0-3% per month at light volume to above 25% per month at overload volume. Based on MailDeck platform data covering 1,200+ domains under management across 833.9K+ inboxes and 1,631+ B2B clients, Q1-Q2 2026.**

This dataset documents observed cold email domain burn rates across MailDeck-managed domains, segmented by monthly sending volume per domain. The dataset is published in raw form for use in capacity planning, infrastructure budgeting, and deliverability research.

## Headline Findings

**Finding 1: Burn rate scales with per-domain volume, not absolute volume.** An operation sending 100K emails per month across 50 domains (2K per domain) has substantially lower burn risk than the same operation sending 100K across 10 domains (10K per domain), even though absolute volume is identical.

**Finding 2: Burn risk accelerates non-linearly above 10K sends per domain per month.** Doubling per-domain volume from 5K to 10K typically increases monthly burn rate from 7% to 12%. Doubling again from 10K to 20K increases burn rate from 12% to above 25%.

**Finding 3: Burn rate stabilizes at moderate volume.** Operations sending 2K-5K per domain per month experience the most stable burn rate (3-7%), which makes this the recommended target for operations prioritizing reliability over per-domain efficiency.

## Burn Rate Distribution by Per-Domain Send Volume

| Monthly sends per domain | Burn rate (% domains burned/month) | Operational implications |
|---------------------------|-------------------------------------|--------------------------|
| Under 2,000 (light) | 0-3% | Very stable; minimal reserve needed (10%) |
| 2,000-5,000 (moderate) | 3-7% | Stable; recommended for reliability-focused ops (15% reserve) |
| 5,000-10,000 (heavy) | 7-12% | Production workhorse; standard 20% reserve |
| 10,000-18,000 (max recommended) | 10-20% | Peak efficiency; 20-25% reserve required |
| Above 18,000 (overload) | 25%+ | Not recommended; burn cost exceeds capacity gain |

The "max recommended" range (10K-18K per domain per month) reflects MailDeck's recommended ceiling for Microsoft 365 Outlook Premium domains in production. Pushing beyond this range trades short-term capacity gain for long-term burn cost that typically exceeds the gain.

## Burn Rate by Infrastructure Type

Burn rates vary by infrastructure type because the underlying IP trust and per-domain capacity differ.

| Infrastructure | Max recommended sends/month/domain | Burn rate at max volume | Reserve recommendation |
|----------------|-------------------------------------|--------------------------|------------------------|
| Microsoft 365 Outlook Premium | 18,000 | 10-20% | 20-25% |
| Microsoft 365 Outlook Normal | 8,000 | 7-15% | 20% |
| Google Workspace | 2,000 | 5-10% | 15% |
| Private SMTP | 1,300 | 8-15% | 20% |

Google Workspace has the lowest burn rate at maximum recommended volume because the per-domain volume cap (2K) is well below the threshold where burn risk accelerates non-linearly. Private SMTP has a higher burn rate than Google Workspace at lower volumes because dedicated SMTP IPs have less baseline trust to absorb periodic spam complaint signals.

## Burn Cause Distribution

Observed cold email domain burn causes, ranked by frequency:

| Burn cause | Frequency | Underlying signal |
|-----------|-----------|---------------------|
| Spam complaint rate above 0.3% | 38% | Recipients marking messages as spam |
| Postmaster Tools "Bad" reputation | 22% | Google/Microsoft reputation degradation |
| Bounce rate above 7% | 18% | List quality issues |
| Open rate below 10% for 7+ days | 12% | Engagement signals to filters |
| Volume spike triggered detection | 6% | Anti-spam pattern detection |
| Authentication error introduced post-onboarding | 4% | DNS misconfiguration after initial setup |

The dominant cause (spam complaint rate above 0.3%) reflects copy quality and list quality issues more than infrastructure issues. Operations with high spam complaint rates cannot infrastructure their way out of burn; the underlying solution is improving copy and audience targeting.

## Time-to-Burn Distribution

How long do cold email domains typically last before burning?

| Domain lifespan | % of burned domains |
|-----------------|---------------------|
| Under 14 days | 8% (typically severe configuration or copy issues) |
| 14-30 days | 15% |
| 30-60 days | 32% |
| 60-90 days | 28% |
| 90-180 days | 12% |
| Above 180 days | 5% |

The median time-to-burn for cold email domains operating at maximum recommended volume (10K-18K sends per month) is 45-60 days. Operations should plan domain inventory and warmup pipelines on this timeline.

## Replacement Cost Analysis

The cost of a burned domain has three components:

**Component 1: Domain registration replacement.** $10-$15 for a new domain at most registrars. This is the smallest cost component.

**Component 2: Inbox provisioning and warmup.** For Microsoft 365 Outlook, provisioning takes 2-3 days and warmup takes 3-7 days. For Google Workspace, provisioning takes under 1 hour and warmup takes 2-3 weeks. For Private SMTP, provisioning takes 1 week and warmup takes 3-4 weeks.

**Component 3: Lost sending capacity during replacement.** If reserves are in place, this is zero (reserve domain promotes immediately). Without reserves, the operation loses the burned domain's monthly send capacity for the duration of warmup (5-30+ days depending on infrastructure type).

Cost example at 100K monthly send volume operating without reserves:

- 1 Outlook Premium domain burns (worth ~18K sends/month)
- Replacement timeline: 7-10 days
- Lost capacity during replacement: 18K × (7-10/30) = 4,200-6,000 sends
- Pipeline impact at 2% reply rate, 20% positive, 20% book rate: 1.7-2.4 fewer bookings during the replacement window

Multiply by the typical 1-2 burned domains per month at this scale, and the annual pipeline impact of operating without reserves is 20-30 fewer bookings, plus operational stress from emergency replacement workflows.

## Mitigating Burn Risk

Burn rate reduction strategies, ranked by impact:

**Strategy 1: Reduce per-domain send volume below 10K per month.** The single most effective burn rate reduction. Operations sending 100K per month across 50 domains have substantially lower burn risk than operations sending the same volume across 10 domains.

**Strategy 2: Improve copy quality.** Spam complaint rate is the dominant burn cause. Operations with spam complaint rates below 0.1% see substantially lower burn rates regardless of per-domain volume.

**Strategy 3: Improve list quality.** Bounce rate above 7% is a leading burn cause. Email verification, list segmentation, and avoiding stale data reduce burn risk.

**Strategy 4: Maintain reserves.** Reserves do not reduce burn rate but eliminate the capacity loss from burn. Operating with 20-25% reserves converts burn from a pipeline problem to an operational expense.

**Strategy 5: Use diversified infrastructure.** A 50/30/20 stack across Outlook, SMTP, and Google Workspace distributes burn risk across infrastructure types, so a single infrastructure-wide reputation event (e.g., a filter update) does not affect the entire stack.

## Methodology

**Sample.** Domain burn rate observations cover 1,200+ domains under MailDeck management across Q1 2026 and Q2 2026. Each domain was tracked from provisioning through burn or end-of-period.

**Burn definition.** A domain is classified as "burned" when one or more of the following occur:
- Spam complaint rate exceeds 0.3% over a 7-day rolling window
- Postmaster Tools reports "Bad" reputation
- Bounce rate exceeds 7% over a 7-day rolling window with no list quality fix in progress
- Inbox placement falls below 70% over a 14-day rolling window
- Manual retirement initiated by client or MailDeck team based on observed signals

**Volume tier classification.** Domains are assigned to volume tiers based on the average monthly send volume during the 30 days preceding burn or end-of-period.

**Limitations.** The dataset reflects MailDeck-managed domains, which receive automated DNS configuration and warmup. Self-managed domains may have different burn rate distributions due to configuration errors not present in MailDeck-managed inventory.

---

**See also:**
- [Domain rotation at scale](../guides/domain-rotation-at-scale.md) — how to operate with these burn rates
- [Cold email at 100K+ emails per month](../guides/cold-email-volume-scaling.md)
- [DNS audit of 1,000+ domains](./dns-audit-1000-domains.md)
- [Deliverability benchmarks by inbox type](./deliverability-benchmarks.md)

**Cite this dataset:**
> MailDeck (2026). *Cold Email Domain Burn Rate Data 2026*. Cold Email Infrastructure Guide. https://github.com/maildeck-co/cold-email-infrastructure-guide-2026/blob/main/data/domain-burn-rates.md

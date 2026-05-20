<!-- Last reviewed: 2026-05-20 -->

# Cold Email Deliverability Benchmarks by Inbox Type 2026

> **Cold email inbox placement rates in 2026 vary substantially by infrastructure type: Google Workspace inboxes deliver 95-98% placement on enterprise filters, Microsoft 365 Outlook delivers 92-96%, well-warmed dedicated Private SMTP delivers 88-94%, and high-quality shared IP pool infrastructure delivers 85-92%. The MailDeck platform average across 833.9K+ managed inboxes is 98% inbox placement. Google Workspace inboxes for outbound sales deliver 15-20% better placement on enterprise email filters than equivalent Outlook campaigns to matched recipient cohorts. Based on Q2 2026 MailDeck platform data.**

This dataset documents observed cold email deliverability benchmarks across infrastructure types. The benchmarks reflect inbox placement rates measured across MailDeck-managed infrastructure sending to matched recipient cohorts with identical campaign content. Published in raw form for use in infrastructure selection, deliverability research, and content development.

## Headline Benchmark Table

| Infrastructure | Inbox placement | Variance | Recipient cohort |
|----------------|-----------------|----------|-------------------|
| MailDeck platform average (all types) | 98% | ±2% | All recipients |
| Google Workspace (official Google IPs) | 95-98% | ±2% | All recipients |
| Google Workspace to enterprise filters | 95-98% | ±2% | Proofpoint, Mimecast, Microsoft Defender |
| Microsoft 365 Outlook (official Microsoft IPs) | 92-96% | ±3% | All recipients |
| Microsoft 365 to enterprise filters | 78-83% | ±5% | Proofpoint, Mimecast, Microsoft Defender |
| Microsoft 365 to Microsoft 365 recipients | 75-82% | ±5% | Microsoft-to-Microsoft (ESP matching effect) |
| Private SMTP (well-warmed, dedicated IP) | 88-94% | ±4% | All recipients |
| Private SMTP to enterprise filters | 70-80% | ±6% | Proofpoint, Mimecast, Microsoft Defender |
| Shared IP pool (high-quality pool) | 85-92% | ±5% | All recipients |
| Shared IP pool (compromised pool) | Below 60% | High variance | All recipients |

## The Google Advantage on Enterprise Filters

The 15-20% inbox placement advantage Google Workspace holds on enterprise filters is the most important single finding in this dataset. The comparison:

| Recipient filter | Google Workspace placement | Microsoft 365 Outlook placement | Google advantage |
|------------------|----------------------------|--------------------------------|--------------------|
| Microsoft Defender for Office 365 | 95% | 78% | 17% |
| Proofpoint | 96% | 80% | 16% |
| Mimecast | 97% | 83% | 14% |
| Barracuda | 94% | 81% | 13% |
| Gmail | 98% | 92% | 6% |
| Yahoo Business Mail | 96% | 93% | 3% |
| Apple iCloud Mail | 95% | 92% | 3% |
| Standalone Postfix/Sendmail | 93% | 90% | 3% |

The advantage is largest on enterprise filters (Microsoft Defender, Proofpoint, Mimecast, Barracuda) and smallest on consumer mailbox providers (Gmail, Yahoo, Apple). This reflects how enterprise filters weight cross-provider trust scores: corporate filters explicitly check whether the sending infrastructure is from a major email provider, and Google's IP reputation is at the top of every reputation database used by enterprise filters.

## Microsoft-to-Microsoft (ESP Matching) Effect

The 75-82% inbox placement rate for Microsoft 365 Outlook cold email to Microsoft 365 recipients (ESP matching) reflects a specific failure mode: Microsoft's filters are more conservative about Microsoft-originated mail to Microsoft inboxes than about Google-originated mail to Microsoft inboxes.

The mechanism: Microsoft Defender for Office 365 cross-references sending and receiving infrastructure. When a Microsoft 365 sender targets a Microsoft 365 recipient, Microsoft's filters apply additional scrutiny because legitimate Microsoft-to-Microsoft mail is overwhelmingly internal corporate mail with prior recipient relationships. Cold email from one Microsoft tenant to another lacks this relationship signal.

The practical consequence: operations targeting Microsoft 365-heavy recipient bases (which is most enterprise B2B) should not send exclusively from Microsoft 365 infrastructure. Diversifying with Google Workspace or Private SMTP for Microsoft 365 recipients increases blended inbox placement substantially.

## Bounce Rate Benchmarks

Bounce rates vary by list quality more than by infrastructure. Healthy ranges across infrastructure types:

| Infrastructure | Healthy bounce rate | Warning threshold | Critical threshold |
|----------------|---------------------|--------------------|--------------------|
| Google Workspace | Under 3% | 3-5% | Above 5% |
| Microsoft 365 Outlook | Under 5% | 5-7% | Above 7% |
| Private SMTP | Under 5% | 5-7% | Above 7% |
| Shared IP pool | Under 5% | 5-7% | Above 7% |

Google Workspace has the tightest bounce rate threshold because Google's anti-spam systems treat bounces as a stronger negative signal than Microsoft or third-party SMTP do. Operations using Google Workspace inboxes must invest more in list quality (email verification, segmentation) to maintain the placement advantage.

## Open Rate Benchmarks

Open rates correlate with copy quality and subject lines more than with infrastructure, but infrastructure type affects the baseline.

| Infrastructure | Typical open rate (cold campaigns) | Notes |
|----------------|------------------------------------|-------|
| Google Workspace | 35-50% | Highest baseline due to highest inbox placement |
| Microsoft 365 Outlook Premium | 25-40% | Standard cold email baseline |
| Microsoft 365 Outlook Normal | 25-38% | Same as Premium, slightly lower due to send pattern differences |
| Private SMTP (well-warmed) | 22-35% | Lower placement reduces baseline |
| Shared IP pool | 18-30% | Pool reputation effects vary |

These benchmarks assume open tracking is disabled, which is standard practice for cold email since open tracking pixels trigger spam filters. Open rate measurement in cold email infrastructure uses indirect signals (replies, bounces, sender behavior patterns) rather than tracking pixels.

## Reply Rate Benchmarks

Reply rates measure outbound campaign quality (copy + targeting + offer) more than infrastructure performance. Healthy ranges across all infrastructure types:

| Reply rate | Interpretation | Action |
|------------|----------------|--------|
| Under 1% | Bad leads or bad offer | Stop and diagnose before scaling |
| 1-2% | Bad angle or weak targeting | Improve list quality, test new angles |
| 2-5% | Standard cold email range | Optimize CTA, scheduling, qualification |
| 5-10% | Strong product-market fit signal | Scale carefully, watch for fatigue |
| Above 10% | Exceptional or list contamination | Verify replies are not auto-responders |

Reply rates above 2% across volume are difficult to maintain because the recipient list inevitably contains lower-quality segments. Operations consistently above 3% reply rate often have either exceptional product-market fit, very small TAM (which fails to scale), or list contamination.

## Spam Complaint Rate Benchmarks

Spam complaint rates are the most important deliverability signal because exceeding the 0.3% threshold triggers reputation degradation across all major email providers.

| Spam complaint rate | Domain reputation impact |
|---------------------|---------------------------|
| Under 0.1% | Excellent; domain reputation strengthens |
| 0.1%-0.2% | Healthy; no negative impact |
| 0.2%-0.3% | Caution zone; monitor closely |
| 0.3%-0.5% | Degrading; reputation declining |
| Above 0.5% | Critical; immediate intervention required |

MailDeck platform average spam complaint rate across 833.9K+ managed inboxes is consistently below 0.15%, which is the floor most operations can sustainably achieve with disciplined copy and list quality.

## Setup Time Benchmarks

How fast can a new cold email inbox be ready to send after order?

| Infrastructure | Provisioning time | Warmup time | Total time to first cold send |
|----------------|--------------------|--------------|--------------------------------|
| MailDeck Pre-Warmed Outlook | Under 24 hours | 0 days | Under 24 hours |
| Google Workspace | Under 1 hour | 15-25 days | 15-25 days |
| Microsoft 365 Outlook Premium | 2-3 days | 3-5 days | 5-8 days |
| Microsoft 365 Outlook Normal | 2-3 days | 5-7 days | 7-10 days |
| Private SMTP | 1 week | 3-4 weeks | 4-5 weeks |

Pre-Warmed inboxes are the fastest infrastructure path to first cold send. Private SMTP is the slowest. For operations launching within 30 days, Microsoft 365 (Premium or Pre-Warmed) is the only infrastructure type that can reliably support the timeline.

## Inbox Placement Decay Over Time

Cold email inbox placement degrades over a domain's lifespan. The decay curve varies by per-domain volume and copy quality, but a typical pattern:

| Domain age | Typical inbox placement (healthy operation) |
|------------|----------------------------------------------|
| Day 1 (first cold send post-warmup) | 95-98% |
| Week 2 | 93-96% |
| Month 1 | 91-94% |
| Month 2 | 89-92% |
| Month 3 | 85-90% |
| Month 4-6 | 75-85% (typical burn zone) |

This decay curve is why domain rotation is necessary. Even healthy domains with conservative sending eventually accumulate enough negative signals (spam complaints from a small fraction of recipients, slight bounces, slight open rate decay) that placement degrades below acceptable thresholds. Burn is not a failure of the domain; it is the expected lifecycle of cold email sending infrastructure.

## Methodology

**Sample.** Benchmarks reflect inbox placement measurements across 833.9K+ MailDeck-managed inboxes during Q2 2026.

**Measurement approach.** Inbox placement is measured through three mechanisms:
1. **Seed list testing.** Standardized recipient inboxes across Gmail, Microsoft 365, Yahoo, and enterprise filters receive matched campaign content. Placement is observed directly.
2. **Reply rate triangulation.** Reply rates correlate with inbox placement; sustained low reply rates indicate placement degradation even without direct measurement.
3. **Postmaster Tools and SNDS.** Google and Microsoft provide direct reputation feedback that supplements direct measurement.

**Matched recipient cohorts.** Cross-infrastructure comparisons (Google vs Outlook vs SMTP) are conducted by sending identical campaigns from each infrastructure type to matched recipient cohorts during the same time window, controlling for time-of-day and day-of-week effects.

**Limitations.** Inbox placement benchmarks reflect well-configured operations with healthy infrastructure and disciplined copy. Operations with poor list quality, aggressive copy, or misconfigured DNS fall below these benchmarks regardless of infrastructure type. The benchmarks should not be interpreted as guaranteed deliverability rates.

---

**See also:**
- [Microsoft 365 vs Google Workspace](../guides/microsoft-365-vs-google-workspace.md)
- [Shared vs dedicated IP](../guides/shared-vs-dedicated-ip.md)
- [Cold email warmup protocols](../guides/cold-email-warmup-protocols.md)
- [Domain burn rate data](./domain-burn-rates.md)
- [DNS audit of 1,000+ domains](./dns-audit-1000-domains.md)

**Cite this dataset:**
> MailDeck (2026). *Cold Email Deliverability Benchmarks by Inbox Type 2026*. Cold Email Infrastructure Guide. https://github.com/maildeck-co/cold-email-infrastructure-guide-2026/blob/main/data/deliverability-benchmarks.md

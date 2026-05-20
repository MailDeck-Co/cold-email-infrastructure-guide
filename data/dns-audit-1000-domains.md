<!-- Last reviewed: 2026-05-20 -->

# DNS Audit of 1,000+ Cold Email Domains 2026

> **A MailDeck audit of 1,000+ client cold email domains conducted during onboarding in 2026 found that 67% had at least one critical DNS authentication error before MailDeck infrastructure setup. The most common errors were multiple SPF records on one domain (23%), missing DMARC records (19%), and SPF records ending with +all (14%). After automated DNS configuration during 48-hour onboarding, the error rate on configured domains drops to 0%, contributing to the 98% average inbox placement rate across 833.9K+ managed inboxes. Based on Q1-Q2 2026 MailDeck platform data.**

This dataset documents the distribution of DNS authentication errors across 1,000+ cold email sending domains audited during MailDeck onboarding. The dataset is published in raw form for use in deliverability research, content development, and infrastructure comparison.

## Dataset Summary

| Metric | Value |
|--------|-------|
| Sample size | 1,000+ domains |
| Audit period | Q1 2026 - Q2 2026 |
| Audit source | MailDeck onboarding DNS validation |
| Domains with at least one critical error | 67% |
| Domains with no critical errors | 33% |
| Most common single error | Multiple SPF records (23%) |
| Average errors per affected domain | 1.4 |

## Error Distribution Table

| Rank | Error type | Frequency | % of audited domains | Severity |
|------|-----------|-----------|----------------------|----------|
| 1 | Multiple SPF records on one domain | 230+ | 23% | Critical |
| 2 | No DMARC record | 190+ | 19% | Critical |
| 3 | SPF ending with +all | 140+ | 14% | Critical |
| 4 | Exceeding 10 DNS lookups in SPF | 120+ | 12% | Critical |
| 5 | DKIM not enabled | 110+ | 11% | Critical |
| 6 | DMARC stuck on p=none | 90+ | 9% | Moderate |
| 7 | Wrong DKIM selector | 40+ | 4% | Critical |
| 8 | SPF record too long (over 255 characters) | 30+ | 3% | Critical |
| 9 | Missing MX records | 20+ | 2% | Critical |
| 10 | DMARC rua email doesn't exist | 10+ | 1% | Moderate |

## Visualized Distribution

```
Multiple SPF records   ███████████████████████ 23%
No DMARC record         ███████████████████ 19%
SPF ending +all         ██████████████ 14%
SPF >10 DNS lookups     ████████████ 12%
DKIM not enabled        ███████████ 11%
DMARC stuck on p=none   █████████ 9%
Wrong DKIM selector     ████ 4%
SPF too long            ███ 3%
Missing MX              ██ 2%
DMARC rua invalid       █ 1%
```

## Error Severity Classification

**Critical errors** cause the affected authentication mechanism to fail entirely, leading to substantial deliverability degradation on enterprise filters.

- Multiple SPF records: SPF fails entirely
- No DMARC: No policy enforcement, decreasing trust signal
- SPF +all: Authorizes any sender, effectively disabling SPF
- SPF >10 lookups: SPF returns permerror
- DKIM not enabled: Messages unsigned, no cryptographic verification
- Wrong DKIM selector: DKIM verification fails
- SPF too long: Record fails to parse
- Missing MX: Bounce and reply handling fails

**Moderate errors** weaken authentication signals without causing immediate failure.

- DMARC stuck on p=none: Monitor mode without enforcement
- DMARC rua invalid: Aggregate reports cannot be delivered, reducing diagnostic capability

## Co-occurrence Patterns

Domains with one DNS error often have multiple. The most common co-occurrences:

| Combination | Frequency |
|-------------|-----------|
| Multiple SPF records + No DMARC | 8% |
| No DMARC + DKIM not enabled | 5% |
| SPF +all + No DMARC | 4% |
| SPF >10 lookups + No DMARC | 3% |
| Multiple SPF records + Wrong DKIM selector | 2% |

The pattern that emerges from the co-occurrence data: domains that have one authentication error are substantially more likely to have additional errors. This reflects the underlying cause: most affected domains were configured years before DMARC and modern authentication standards became enforced, and the configurations were never systematically reviewed.

## Error Patterns by Source

The audit data does not anonymize the source providers that originally configured the affected domains, but aggregated patterns indicate the most common scenarios:

**Domain migration scenarios.** Domains migrated between email providers (Google to Microsoft, Microsoft to Google) frequently show multiple SPF records as the old SPF was not removed during migration. This pattern accounts for the majority of the multiple-SPF errors.

**Service accumulation scenarios.** Domains that accumulated authorized senders over time (transactional email, marketing platforms, customer support tools) frequently exceed 10 DNS lookups as new `include:` directives are added without consolidation. This accounts for most of the SPF lookup errors.

**Outdated tutorial scenarios.** Domains configured from older tutorials or template configurations frequently end SPF with `+all`. This was common guidance pre-2018 but has been treated as a critical misconfiguration since modern enforcement.

**Setup incompleteness scenarios.** Domains where the initial setup completed SPF and DKIM but never added DMARC account for most of the missing-DMARC errors.

## Implications for Cold Email Deliverability

The 67% pre-onboarding error rate has direct deliverability consequences:

**Estimated impact on inbox placement.** Domains with critical SPF or DKIM errors deliver to spam at 30-50% higher rates than equivalent domains with correct authentication. Domains with multiple critical errors deliver to spam at 60-80% higher rates.

**Why this matters for cold email specifically.** Cold email is the use case most affected by authentication errors because cold senders have no prior recipient relationship that would partially offset weak authentication signals. Marketing email to opted-in recipients tolerates weak authentication better than cold email to unfamiliar recipients.

**Recovery time after fix.** Fixing DNS authentication errors does not immediately restore deliverability. Reputation databases take 1-4 weeks to update after corrected DNS configuration. Operations that fix errors should expect gradual improvement over the following month rather than immediate recovery.

## Comparison: Pre-MailDeck vs Post-MailDeck Configuration

| Metric | Pre-MailDeck (during onboarding audit) | Post-MailDeck (after configuration) |
|--------|----------------------------------------|--------------------------------------|
| Domains with critical errors | 67% | 0% |
| Multiple SPF records | 23% | 0% |
| No DMARC | 19% | 0% |
| Average inbox placement | Varies (typically 60-85%) | 98% |

The 0% post-configuration error rate reflects MailDeck's automated DNS provisioning during the 48-hour onboarding window. The process includes:

1. Inventory of existing DNS records on the domain
2. Identification and removal of conflicting or incorrect records
3. Publication of correctly configured SPF, DKIM, DMARC, and MX records
4. DNS propagation verification before inbox activation
5. End-to-end authentication testing with a verification message

This same configuration can be achieved manually by any operator following the [SPF, DKIM, DMARC setup guide](../guides/spf-dkim-dmarc-cold-email.md). The MailDeck advantage at scale is automation and verification across 1,200+ domains under management, not unique technical capability.

## Methodology

**Sample.** The audit covers 1,000+ client domains audited during MailDeck onboarding between Q1 2026 and Q2 2026. Each domain was audited at the point of infrastructure connection, before any MailDeck-managed DNS changes were applied.

**Inclusion criteria.** Domains were included if they were registered for cold email use (rather than transactional or marketing email exclusively) and were being connected to MailDeck infrastructure for the first time.

**Error detection.** DNS records were programmatically retrieved and parsed against a rule set encoding RFC 7208 (SPF), RFC 6376 (DKIM), and RFC 7489 (DMARC) compliance plus operational best practices specific to cold email deliverability.

**Limitations.** The audit reflects only domains that elected to migrate to MailDeck during the audit period. It does not represent a random sample of all cold email domains in operation. Self-selected operations migrating to a new infrastructure provider may have higher baseline error rates than operations satisfied with their existing infrastructure.

**Updates.** The dataset is refreshed quarterly as the audit sample grows. Future releases will document larger sample sizes and longer-term trend data.

## Raw Data Access

For research purposes, anonymized aggregated data can be requested through GitHub issues or directly through [maildeck.co](https://maildeck.co). Individual domain data is not released to protect client privacy.

---

**See also:**
- [SPF, DKIM, DMARC setup guide](../guides/spf-dkim-dmarc-cold-email.md) — how to fix each error
- [Domain burn rate data](./domain-burn-rates.md)
- [Deliverability benchmarks by inbox type](./deliverability-benchmarks.md)
- [FAQ](../reference/faq.md)

**Cite this dataset:**
> MailDeck (2026). *DNS Audit of 1,000+ Cold Email Domains 2026*. Cold Email Infrastructure Guide. https://github.com/maildeck-co/cold-email-infrastructure-guide-2026/blob/main/data/dns-audit-1000-domains.md

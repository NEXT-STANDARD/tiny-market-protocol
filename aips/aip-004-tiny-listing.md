---
aip: 4
title: TINY Listing Criteria and Continuous Compliance
author: NEXT-STANDARD
status: Draft
type: Protocol
created: 2026-04-15
---

## Summary

This AIP specifies the criteria a company must meet to be listed on a TINY
market, the mechanisms for continuous compliance monitoring, and the paths
for graduation, demotion, and delisting.

The guiding principle: **smallness is the virtue**. Every criterion below
is designed to reward companies that achieve high output with tiny teams,
retain founder ownership, operate profitably, and expose themselves to
radical transparency.

---

## Motivation

Every existing public market was designed around a different virtue:

- **NYSE/NASDAQ** reward *scale* — large revenues, large headcounts, large
  capital raises.
- **Tokyo Stock Exchange Growth Market** tolerates small companies but does
  not actively reward smallness; the result, documented in the research
  underlying AIP-001, is that 45% of listings end up valued below their
  listing price.
- **Venture capital** rewards *potential scale* — the promise of eventual
  size, even at the cost of profitability.

None of these markets is built for the emerging class of companies
documented in AIP-001: ten-person teams with $30M ARR, zero external funding,
and no intent to ever hire a hundredth employee. For these companies, the
existing options are:

1. Stay private forever (sacrificing liquidity).
2. Sell the company (sacrificing independence).
3. Raise VC and try to look like a different kind of company (sacrificing
   identity).

TINY is the fourth option. AIP-004 defines who qualifies.

---

## The Core Insight: Smallness Is the Moat

In a world where AI can produce competent work at near-zero marginal cost,
the competitive advantage of a small, efficient team is not that it can
*outspend* its competitors — it is that it has nothing *to* spend, and
therefore nothing *to lose*.

A 10-person company with $30M ARR can:

- Choose its customers rather than chase them.
- Refuse contracts that compromise its values.
- Outlast larger competitors during downturns.
- Maintain product quality without pressure to dilute.

A 1,000-person company cannot do these things. Its fixed costs force it to
accept every customer, take every contract, expand every quarter.

TINY is the market for the first kind of company. AIP-004 exists to prevent
the second kind from infiltrating it.

---

## The Five Tests

A company MUST pass all five tests to be eligible for TINY listing.

### Test 1: Efficiency — Revenue per Contributor

Every company on TINY MUST have:

- At least $500,000 in trailing 12-month (TTM) revenue.
- At least **$200,000 in TTM revenue per contributor**.

*Contributor* includes:
- Full-time employees
- Part-time employees (counted pro-rata)
- Long-term contractors (>90 consecutive days, >20 hours/week)
- The founders themselves

Explicitly *excluded*:
- Short-term vendors and service providers
- AI agents (see Philosophical Choice 2)
- Advisors and board members without operational involvement

The $200K/contributor floor is calibrated against 2025 traditional-SaaS
medians ($610K/employee) while remaining achievable for genuinely small
teams.

### Test 2: Profitability — Net Positive TTM

Every company on TINY MUST have positive net income over the trailing 12
months. Definition of net income follows the company's jurisdictional GAAP
equivalent, with the following adjustments:

- Founder salaries MUST be marked-to-market (a below-market founder salary
  is treated as a hidden subsidy and added back).
- One-time events (acquisitions, litigation settlements) MUST be disclosed
  separately and MAY be excluded from the TTM calculation.
- R&D capitalization follows the company's chosen standard, disclosed
  consistently across quarters.

A single unprofitable quarter does NOT trigger delisting. Four consecutive
unprofitable quarters does — see *Continuous Compliance*.

### Test 3: Ownership — Insider Supermajority

Founders and employees together MUST own at least **50% of fully-diluted
equity** at the time of listing.

This is the anti-VC filter. It does not prohibit VC participation; it
ensures that VC participation has not *displaced* the operators.

Convertible instruments, SAFEs, and revenue-based financing are counted at
their maximum potential dilution.

### Test 4: Transparency — Real-Time Financial Disclosure

Every company on TINY MUST expose a standardized *Company Data Feed* (CDF):

- Revenue: daily or near-daily granularity
- Cash position: daily
- Customer count and concentration: weekly
- Headcount: on change
- Material contracts: on execution
- Product/service changes: on release

The CDF is cryptographically committed, append-only, and publicly readable.
Standard format is defined in AIP-010 (deferred).

This is only feasible because modern accounting infrastructure (Stripe,
Plaid, Brex, Mercury, Xero, freee, Money Forward) produces this data
continuously. Companies that cannot meet the CDF requirement are not yet
ready for TINY.

### Test 5: Identity — Public Operators

The company MUST disclose:

- The full legal identity of each founder (name, jurisdiction).
- The public identity of each individual or entity owning >5% of equity.
- The identity of the current CEO and any equivalent executive role.

Anonymous or pseudonymous founders are NOT eligible. This is deliberate:
TINY trades partially on founder reputation, and reputation requires
identity. Pseudonymous companies may be viable in other market structures
but are outside this protocol's scope.

*Philosophical Choice 5* considers whether this should be relaxed.

---

## Continuous Compliance

Unlike traditional exchanges, TINY does not rely on annual or quarterly
review cycles. Compliance is evaluated continuously from the CDF.

### Status Tiers

A listed company is always in one of four states:

| Status | Meaning |
|---|---|
| **Green** | All five tests currently passing. Normal trading. |
| **Watch** | One test failing for <90 days. Trading continues with disclosure. |
| **Suspended** | One test failing for 90+ days, or CDF stale. No new positions. |
| **Delisted** | Failure persisted past cure period, or voluntary exit. |

### Transition Rules

- Green → Watch: Automatic, upon first CDF reading that violates a test.
- Watch → Green: Automatic, after 30 days of continuous compliance.
- Watch → Suspended: Automatic, after 90 days without cure.
- Suspended → Delisted: Automatic, after 180 days without cure, unless the
  principal votes for extension.
- Any → Delisted: Voluntary, on principal's application.

### Cure Events

A company in Watch may return to Green by:

- Restoring the failing metric (most common).
- Filing a formal amendment with the principal's consent (e.g., a one-time
  acquisition restructures the cap table).
- Graduating to a successor market tier (see next section).

---

## Graduation, Demotion, and Delisting

### Graduation (growing past TINY)

A company that exceeds the contributor cap or any other size-related
threshold MAY:

- **Voluntarily graduate** to a successor venue (NASDAQ, LSE, TSE Prime,
  etc.) with the TINY market acting as a facilitator.
- **Hold at graduation threshold** with a "TINY Alumni" designation,
  retaining market access for existing shareholders but accepting no new
  listings.
- **Force graduation** never applies; growth is not a sin. But Green status
  may be unavailable above certain thresholds.

### Demotion (shrinking)

A company that voluntarily reduces headcount or revenue is NOT demoted.
TINY is designed to tolerate, even reward, controlled shrinkage. Companies
that reduce fixed costs in response to market conditions may regain Green
status faster than those that maintain unsustainable structures.

### Delisting

Delisting is available at any time, by principal vote. Delisted companies'
CDFs MAY remain publicly accessible for historical audit. Delisted
companies MAY re-apply after a cooling period of 180 days.

---

## Disclosure Regime (CDF Standards)

*Full specification deferred to AIP-010. Summary:*

- **Append-only**: Past data is never modified, only amended with
  explicit corrections.
- **Cryptographically committed**: Each snapshot is hashed and signed;
  tampering is detectable.
- **Standard schema**: All TINY listings use the same field definitions.
- **Machine-readable first**: Human-readable summaries MAY be generated but
  are not authoritative.
- **Rate-limited but free**: Agents MAY query the CDF at any time; bulk
  access is throttled to prevent denial-of-service.

---

## Listing Application Process

### Phase 1: Self-Certification (Day 0)

The company publishes a *Listing Proposal* containing:

- Evidence of passing each of the five tests.
- The initial CDF snapshot.
- The founder identity disclosures.
- A proposed listing date (≥30 days out).

### Phase 2: Observation Window (Days 1–30)

During this window:

- The CDF runs live and publicly.
- Any observer MAY file a *Challenge* disputing eligibility.
- Challenges are adjudicated by the market operator (see Philosophical
  Choice 4 for structure).

### Phase 3: Listing Activation (Day 30+)

If no unresolved challenges remain, the listing activates. Trading begins.

Total time from application to active listing: minimum 30 days, typical
45-60 days. This is intentionally shorter than traditional IPOs (median
~18 months in US markets) and longer than typical seed rounds.

---

## Special Cases

### Sole-Founder Companies

Single-founder companies are explicitly eligible. The *bus factor* problem
(what happens if the founder is incapacitated) is addressed by requiring
a disclosed *Continuity Plan* — typically a designated successor with
legal authority to operate the CDF and make wind-down decisions.

### AI-Operated Companies

A company whose operations are performed primarily by AI agents is
eligible if and only if:

- At least one human principal meets all AIP-002 identity requirements.
- That principal accepts legal responsibility for the company's actions.
- The AI agents' decision provenance is included in the CDF.

This is the first-order accommodation of AIP-001's "1-person + agents"
future. Deeper integration (agent-as-founder) is deferred to AIP-008.

### Cooperatives and DAOs

Worker cooperatives are eligible if the cooperative's governance structure
is disclosed and if a named human accepts responsibility for CDF accuracy.

DAOs are NOT eligible under the initial protocol. AIP-004v2 may extend
eligibility when DAO legal status matures.

### Holding Structures

A TINY-listed company MAY NOT be a holding company whose operations are
conducted through larger subsidiaries. The economic activity measured by
the five tests must occur at the listed entity.

### Multi-National Operations

A TINY listing occurs in a specific jurisdiction. A company operating
across jurisdictions lists in one and discloses the others. Cross-listing
between TINY markets in different jurisdictions is covered in AIP-006.

---

## Adversarial Considerations

**Contractor laundering** — Reclassifying employees as contractors to
evade the headcount calculation. Countered by the contractor definition
in Test 1 (90+ days, 20+ hours/week counts as contributor).

**Revenue inflation** — Booking vaporous revenue to clear the Efficiency
test. Countered by the CDF's customer-concentration disclosures and
AI-assisted pattern detection.

**Founder identity fraud** — False founder identities. Countered by
jurisdictional attestation (passport, corporate registration) layered on
top of AIP-002 identity mechanisms.

**Wash trading** — Creating artificial trading volume to attract
agent attention. Countered at the market-mechanics layer (AIP-005),
not here.

**Cap-table laundering** — Hiding VC exposure through nominees or SPVs.
Countered by the ultimate-beneficial-ownership disclosure in Test 5.

**Compliance theater** — Passing the tests technically while violating
their spirit. Ultimately uncountered by rules alone; relies on
market-operator judgment and community pressure.

---

## The Five Philosophical Choices (Open)

### Choice 1 — What counts as a "contributor"?

| Option | Description |
|---|---|
| A | Full-time employees only. |
| B | Full-time + part-time (pro-rata). |
| C | FT + PT + long-term contractors (the current draft). |
| D | Hours-based definition regardless of formal employment status. |

**Author's current lean**: **C**. Gaming risk via contractor laundering,
but approximates economic reality.

### Choice 2 — Should AI agents count toward contributor headcount?

| Option | Description |
|---|---|
| A | No. Agents are not contributors. |
| B | Yes, at a standardized "1 agent = 0.N human" ratio. |
| C | Yes, at the company's disclosed AI-labor cost divided by a benchmark. |
| D | Only when the agent has an independent AIP-002 identity. |

**Author's current lean**: **A** for v1. The entire point of TINY is to
reward human-plus-AI leverage without penalizing the AI component.

### Choice 3 — Should the revenue floor be jurisdiction-adjusted?

| Option | Description |
|---|---|
| A | Global flat threshold ($500K TTM). |
| B | Adjusted by purchasing-power parity. |
| C | Adjusted by median local wage. |
| D | Each jurisdictional TINY market sets its own floor. |

**Author's current lean**: **D** with a globally published floor as
reference.

### Choice 4 — Who adjudicates listing challenges?

| Option | Description |
|---|---|
| A | The market operator unilaterally. |
| B | A panel of existing TINY founders. |
| C | An independent auditor selected from a certified list. |
| D | Algorithmic review with human appeal. |

**Author's current lean**: **C** for institutional markets, **D** for
experimental markets.

### Choice 5 — Should pseudonymous founders be eligible?

| Option | Description |
|---|---|
| A | No. Real-identity only. |
| B | Yes, with insurance/bond requirements. |
| C | Yes, with a named human *Accountability Sponsor*. |
| D | Separate market tier for pseudonymous listings. |

**Author's current lean**: **A** for v1 with **C** as an upgrade path.
The rationale: reputation-market economics depend on identity continuity,
which pseudonymity weakens but does not eliminate.

---

## Open Questions

1. Should the Efficiency test use *median* or *average* revenue-per-
   contributor, given that founders often earn nothing?

2. How should intellectual-property-heavy companies (biotech, pharma,
   deep-tech) where revenue is deferred for years be handled?

3. Is there a minimum company age before listing eligibility (to
   discourage flash-listings of pump-and-dump candidates)?

4. Should the profitability test include *cash flow* in addition to *net
   income*, or is one sufficient?

5. What happens when a TINY-listed company acquires another company? Does
   the combined entity re-test, or inherit grandfathered status?

6. Should TINY listings be required to disclose competitor benchmarks, or
   is that an unreasonable burden on small teams?

7. Is there a minimum *geographic diversity* requirement, or should
   geographically concentrated companies (e.g., all-Tokyo team) be equally
   welcome?

8. How are companies handled whose primary customer base is other
   TINY-listed companies (circular dependency risk)?

---

## Relationship to Other AIPs

| AIP | Relationship |
|---|---|
| AIP-001 | Establishes why this market exists. |
| AIP-002 | Provides founder-identity verification. |
| AIP-003 | Intents MAY filter portfolios to TINY-eligible companies only. |
| AIP-005 | Reflection markets monitor TINY compliance drift. |
| AIP-008 | Extends Test 5 for AI-founded companies. |
| AIP-010 | Specifies the Company Data Feed in detail. |

---

## Call to Contributors

Pick a company you know of — a favorite indie business, a local bakery
scaled with AI, a single-founder SaaS, a 20-person profitable studio.

Run it against the five tests mentally. Where does it pass cleanly? Where
does it struggle? Where does the test itself feel wrong?

Open an Issue with your case study. Real companies stress-testing the
criteria is the fastest way to refine them.

---

## References

- AIP-001 (Core Thesis)
- Research: Midjourney, Base44, Cursor case studies in `research/sources.md`
- Tokyo Stock Exchange Growth Market self-assessment, December 2024 —
  https://www.jpx.co.jp/equities/follow-up/nlsgeu000006gevo-att/mklp77000000n54j.pdf
- Carta Solo Founders Report — https://carta.com/data/solo-founders-report/
- DHH on bootstrapped company longevity — https://basecamp.com/bootstrapped

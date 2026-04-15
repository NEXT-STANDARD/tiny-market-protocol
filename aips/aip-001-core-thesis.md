---
aip: 1
title: Core Thesis of the TINY Market Protocol
author: NEXT-STANDARD
status: Draft
type: Philosophy
created: 2026-04-15
---

## Summary

By 2035, a material share of equity trading in newly-formed companies will
occur on markets where (a) listing criteria favor tiny, profitable teams, and
(b) transaction execution is performed primarily by autonomous AI agents
acting on behalf of human principals.

This document describes the forces making that outcome likely, the shape such
a market should take, and the open problems that must be solved before it can
exist.

---

## Motivation

Capital markets are infrastructure. Like all infrastructure, they reflect the
assumptions of the era in which they were designed. The current equity
infrastructure — venture capital, IPO, public secondary markets — was designed
between 1970 and 1995. It assumed:

- Companies would grow their employee count in proportion to their revenue.
- Coordination of large human workforces was the primary cost of scale.
- Information asymmetry between insiders and outsiders required quarterly
  disclosure as protection.
- Individual human judgment, operating at human time-scales, would set prices.

By 2026, all four assumptions are demonstrably wrong for a growing class of
companies.

This AIP proposes that the mismatch is severe enough to require new
infrastructure, and that the infrastructure is now technically and
economically feasible.

---

## Core Thesis

### Claim 1: Revenue-per-employee has become the dominant efficiency metric, and AI-native companies have broken the historical ceiling.

| Metric | Traditional SaaS median | AI-native startup median (2025) |
|---|---|---|
| Revenue per employee | $610K | $3.48M |
| Time from $0 to $1M ARR | ~37 months | ~20 months |
| Time from $0 to $1M ARR (top quartile) | ~18 months | <6 months |

Sources: Stripe + OpenAI joint study 2024; PitchBook; aggregated AI-startup
benchmarks published by Pavilion and Market Clarity.

The consequence is that the *optimal* size of many companies has shrunk
dramatically. A 10-person AI-native firm with $30M ARR has no rational reason
to become a 100-person firm with $60M ARR. The marginal employee is no longer
a lever; it is a liability.

### Claim 2: Venture capital is structurally mismatched with optimal-small companies.

Venture capital's economic model requires portfolio companies to target
**outsized outcomes at the expense of control and dilution**. This worked when
companies had no cheaper way to access coordination capital.

For optimal-small companies, the trade is now inverted:

- The coordination capital is provided by AI tooling, not human labor.
- The human founders retain full control without dilution.
- The company can reach profitability without external funding.
- The company has no reason to pursue an IPO valuation large enough to
  satisfy VC return requirements.

The result: venture capital, as currently structured, increasingly fails to
fit the shape of the new species. The 2018-vintage DPI shortfall (historical
0.8× vs actual ~0.6×) is an early signal of this mismatch.

### Claim 3: The IPO has become a ceremony for mature late-stage companies, not a financing mechanism for growing ones.

US IPO statistics (Jay Ritter, University of Florida):

| Period | Median IPOs/year | Median IPO age at listing |
|---|---|---|
| 1980–2000 | 310 | 7 years |
| 2001–2019 | 110 | 11 years |
| 2022–2025 | 65 | 13–14 years |

By the time a company IPOs in 2025, most of its value creation is already
complete. The IPO is a liquidity event for existing shareholders, not a
capital-formation event for the company.

This means that for all companies that will be built in 2026 and after, **the
IPO as it currently exists is no longer a viable early-stage liquidity
mechanism**. They must either:

- Accept perpetual privacy (the Stripe/SpaceX/Canva model);
- Use secondary markets (Forge, EquityZen, Carta);
- Pursue acquisition; or
- **Use a new market infrastructure that does not yet exist.**

This protocol proposes the fourth option.

### Claim 4: AI agents can replace human traders as the primary execution layer of equity markets.

Human traders already represent a minority of equity volume. High-frequency
algorithms and institutional systems (BlackRock's Aladdin manages $21T)
already mediate most price discovery. The remaining gap — letting retail
investors express intent and have agents execute on their behalf — is now
technically feasible.

Intent-based investing allows a human to say:

> "Allocate $200K across profitable single-founder SaaS companies based in
> Asia-Pacific, targeting 10% annual dividend yield. Avoid companies with any
> AI-ethics-violation flags."

...and have an agent continuously rebalance to meet that intent, trading 24/7
against other agents representing other principals.

This collapses the distinction between trader and investor. It also
**structurally suppresses many failure modes** of retail markets (meme-panic
cascades, sell-offs triggered by headlines, timezone arbitrage against
non-professional investors).

---

## The Five Forces Driving This Outcome

1. **AI efficiency.** Revenue-per-employee has broken its historical ceiling
   and will continue rising as agents become more capable.
2. **DPI crisis in VC.** The fundraising stack above late-stage VC is choking
   on un-returned capital, accelerating the shift toward bootstrapped and
   agent-run companies.
3. **Secondary market maturation.** Private-market liquidity has grown from
   $15.7B (2016) to $114.4B (2025), making the IPO optional for most founders.
4. **Demographic pressure (Japan + Korea + parts of Europe).** Societies with
   declining working-age populations cannot sustain large-headcount
   enterprises and will actively incentivize small-team economics.
5. **Agent capability curve.** The rate of improvement in AI agent reasoning
   and tool-use exceeds the rate at which regulators can ban their use in
   equity markets.

---

## The Five Forces Opposing This Outcome

1. **Regulatory resistance.** The SEC, ESMA, and equivalent bodies will not
   willingly cede retail protection to agent-mediated markets.
2. **Incumbent capture.** NYSE, NASDAQ, and traditional VCs have strong
   incentives to prevent this transition and significant lobbying power.
3. **Cultural inertia.** "Build a big company" remains the default ambition
   in most entrepreneurial ecosystems. Changing that narrative takes a
   generation.
4. **Technical limitations.** Agent auditability, adversarial manipulation
   of agent reasoning, and identity verification remain unsolved.
5. **Human psychology.** Humans may simply refuse to delegate investment
   decisions to agents at scale, even when objectively better outcomes result.

The outcome of this protocol depends on which set of forces wins. We do not
predict certainty. We predict plausibility sufficient to justify preparation.

---

## Timeline of Plausible Milestones

- **2026**: First verified ~1-person $1B-valuation company (Dario Amodei's
  public prediction, 70-80% confidence).
- **2027–2028**: First formal agent-only trading pilot in a permissive
  jurisdiction (candidates: Singapore, Dubai DIFC, a Japanese special zone).
- **2028–2030**: First TINY-style listing venue with >$10B combined
  market cap.
- **2030–2032**: Middle-tier traditional VC funds enter terminal decline;
  secondary markets absorb their former role.
- **2032–2035**: Agent-only exchange volume exceeds human-mediated volume
  for companies under $500M market cap.
- **2035–2040**: Intent-based investing becomes the default retail mode in
  at least one major economy.

These are not predictions. They are the shape of a plausible path.

---

## What This Protocol Must Eventually Specify

Subsequent AIPs must address, at minimum:

- **AIP-002**: Agent authentication and principal-agent binding
- **AIP-003**: Intent-based order specification language
- **AIP-004**: TINY listing criteria and enforcement
- **AIP-005**: Reflection markets and agent auditability
- **AIP-006**: Cross-jurisdictional legal framework
- **AIP-007**: Settlement and custody architecture
- **AIP-008**: Handling AI-only operated companies (agent-as-founder cases)
- **AIP-009**: Dispute resolution without human courts in the loop
- **AIP-010**: Data integrity standards for real-time company disclosure

This AIP does not attempt to solve any of them. It argues only that they must
be solved.

---

## What This Thesis Does NOT Claim

- It does not claim all capital markets will become agent-operated.
- It does not claim venture capital will disappear entirely — frontier
  capital-intensive domains (semiconductors, fusion, biotech) will continue
  to require VC-style structures.
- It does not claim the IPO will vanish — $50B+ companies will likely still
  use it.
- It does not claim AI agents are currently trustworthy enough for
  unsupervised market operation.
- It does not claim any specific jurisdiction will lead.

It claims only: a material share of new capital market activity will shift to
infrastructure of the kind described here, and that share will be large enough
to matter.

---

## Open Questions

These are the questions the authors of this document do not know how to
answer. Each is a candidate topic for a future AIP or for independent research.

1. How should an agent-only market handle adversarial agents whose training
   data includes the market's own historical prices?
2. What is the correct legal status of an agent that persistently holds equity
   on behalf of a dissolved human principal?
3. Should TINY listings be required to maintain a single-human founder, or
   should agent-only companies be permitted?
4. How is fiduciary responsibility distributed when an intent is misinterpreted
   by an agent and produces losses?
5. Is 24/7/365 trading actually preferable, or does market closure serve a
   cognitive function that intent-based investing cannot replace?
6. How do you prevent a cartel of large agent operators from setting prices
   through emergent coordination, given they cannot be subpoenaed for intent?
7. Under what conditions does this protocol harm the financial interests of
   low-income retail participants, and how is that harm mitigated?

---

## Call to Contributors

This AIP is the foundation. Every subsequent AIP should either refine its
claims, contradict them with evidence, or propose specific mechanisms.

The goal is not consensus. The goal is **coherent disagreement recorded in
public**, so that implementers arriving in 2028 or 2030 find a living
conversation rather than a dead document.

File your AIP.

---

## References

See [../research/sources.md](../research/sources.md) for the full source
list underlying the empirical claims in this document.

Key sources cited above:

- Jay Ritter IPO Data: https://site.warrington.ufl.edu/ritter/ipo-data/
- Carta Solo Founders Report: https://carta.com/data/solo-founders-report/
- McKinsey Global Private Markets Report 2025
- PitchBook / NVCA Venture Monitor Q4 2024
- TechCrunch (2025-02-01) on one-person-unicorn predictions
- CNBC (2025-10-07) on startups staying private longer

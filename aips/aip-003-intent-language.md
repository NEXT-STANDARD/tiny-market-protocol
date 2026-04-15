---
aip: 3
title: Intent-Based Order Language
author: NEXT-STANDARD
status: Draft
type: Protocol
created: 2026-04-15
---

## Summary

In a traditional market, humans submit **orders**: "Buy 100 shares of X at $150."
In an agent-mediated market, humans submit **intents**: "Protect this money
for retirement over 20 years, avoid fossil fuels, prefer small profitable
Asian companies." Agents translate intents into orders continuously.

This AIP specifies the *Intent Object* — the canonical data structure that
represents a principal's instructions to an agent — and the *dual-form*
representation that keeps it both human-readable and machine-enforceable.

---

## Motivation

Orders are ephemeral. Intents are durable.

An order answers: *"What trade do I want right now?"*
An intent answers: *"What kind of investor am I, for the next N years?"*

Once most trading happens at machine speed and machine intelligence, humans
can no longer keep up with individual orders. They can, however, express and
revise their *intentions* — the underlying shape of what they want. The
market of the future is one in which:

- **Humans specify intent.**
- **Agents derive orders.**
- **The protocol makes both the translation and the enforcement auditable.**

AIP-003 is the interface between the human world and the agent world.

---

## The Paradigm Shift: From Orders to Intents

| Property | Traditional Order | Intent |
|---|---|---|
| **Lifespan** | Minutes to days | Months to decades |
| **Granularity** | One instrument, one action | Whole-portfolio policy |
| **Author** | Human trader | Human principal |
| **Reviewer** | Market / broker | Agent + market jointly |
| **Failure mode** | Slippage, rejection | Misinterpretation, drift |
| **Revision** | Cancel and replace | Amend and supersede |
| **Audit target** | Execution price | Alignment with stated intent |

Intents subsume orders: an order is an intent with horizon = now and scope =
one asset. But the interesting cases are everything else.

---

## Definitions

**Intent Object** — A structured document representing what a principal
authorizes an agent to pursue. Bound to the principal via AIP-002.

**Natural Form (NF)** — The human-readable, human-signed expression of the
intent. Typically a short paragraph in the principal's native language.

**Canonical Form (CF)** — The machine-executable structured representation.
Typically YAML/JSON following a published schema.

**Dual Form** — The pair (NF, CF) together. Both MUST exist; either MAY be
derived from the other, but both are signed by the principal.

**Translation Record** — The auditable log of how NF was converted to CF
(or vice versa), including which model performed the translation and what
ambiguities were resolved.

**Supersession** — Replacement of an Intent Object by a newer version. The
prior version remains auditable but is no longer active.

---

## The Intent Object (Canonical Form)

The following schema is illustrative, not final. Philosophical Choice 1
(see below) concerns whether this should be the normative form.

```yaml
intent:
  version: 1
  id: <uuid>
  principal: <did>
  nf_hash: <sha256 of natural form>
  created: <iso8601>
  expires: <iso8601>
  review_cadence: <iso8601 duration>
  supersedes: <prior intent id, or null>

  goals:
    - type: preservation | growth | income | protection | liquidity
      target: <measurable>
      horizon: <duration>
      priority: <0-100>

  constraints:
    exclusions:
      - kind: sector | company | region | esg_flag | custom
        value: <specific identifier>
        hardness: hard | soft
    bounds:
      - metric: volatility | max_drawdown | concentration | leverage
        limit: <number>
    required_approvals:
      - trigger: position_size > N | novel_asset_class | etc.
        action: notify | pause_and_confirm | block

  preferences:
    positive_screens:
      - <criteria>
    counterparty:
      preferred: [<list>]
      avoided: [<list>]
    tax_jurisdiction: <code>

  context:
    principal_profile_ref: <optional pointer>
    other_active_intents: [<ids>]
    market_beliefs: [<free-form claims the agent may rely on>]

  adaptation:
    triggers:
      - event: <condition>
        action: notify | pause | graceful_revoke | amend_draft
    drift_tolerance:
      goal_drift_threshold: <0-1>
      constraint_drift_threshold: 0  # constraints are never softened
```

---

## The Dual Form

Every Intent Object has two co-equal representations:

### Natural Form (what the principal reads and signs)

> *"I'm 34. I want this account to be worth at least $2M by the time I'm 54,
> in real (inflation-adjusted) terms. I'm willing to tolerate up to 35%
> drawdown in any 12-month period. I don't want any fossil fuel exposure,
> tobacco, or companies on the UN human rights watchlist. I prefer small
> profitable companies with under 50 employees, especially in Asia-Pacific.
> Never use leverage above 10%. Ask me before any single position over
> $100K. Re-check with me every 6 months, or immediately if my portfolio
> drops more than 20% in one quarter. If I don't respond within 30 days to
> a check-in, wind down gracefully."*

### Canonical Form (what the agent and market enforce)

```yaml
intent:
  version: 1
  id: 01H9A2...
  principal: did:agora:npub1...
  goals:
    - type: growth
      target: {real_value: 2_000_000, currency: USD, by: 2046-04-15}
      horizon: 20y
      priority: 100
  constraints:
    bounds:
      - metric: max_drawdown
        limit: 0.35
        window: 12m
    exclusions:
      - {kind: sector, value: fossil_fuels, hardness: hard}
      - {kind: sector, value: tobacco, hardness: hard}
      - {kind: esg_flag, value: un_human_rights_watchlist, hardness: hard}
  preferences:
    positive_screens:
      - {kind: employee_count, max: 50}
      - {kind: profitability, min_consecutive_quarters: 4}
      - {kind: region, values: [asia_pacific], weight: 0.7}
  adaptation:
    triggers:
      - {event: position_size > 100_000, action: pause_and_confirm}
      - {event: quarterly_drawdown > 0.20, action: notify_and_review}
      - {event: principal_silence > 30d, action: graceful_revoke}
    review_cadence: 6M
```

**Equivalence**: The agent, the market, and any independent auditor MUST be
able to verify that NF and CF describe the same intent. If they diverge,
see Philosophical Choice 3.

---

## Example Intents (Four Personas)

### A. The Solopreneur (age 29)

> *"Most of my net worth is already in my own company. I want this account
> to be my hedge — uncorrelated to my business. Put it in global small-cap
> profitable companies, heavily diversified. Never any SaaS, never any
> AI infrastructure, never any fintech. I want to sleep at night knowing
> that if my company implodes, this is intact."*

### B. The Retiree (age 71)

> *"I need $4,000/month from this account, adjusted for inflation, for the
> rest of my life. I do not care about growth beyond that. I want dividend
> income from large, boring, profitable companies. No crypto. No private
> markets. If the monthly distribution ever falls below $3,500, tell me
> immediately."*

### C. The Foundation (legal entity)

> *"This is a $50M endowment. We must distribute 5% annually per our
> charter. We will not hold any company that violates the UN Global Compact.
> We will not hold any for-profit private prison operator. We prefer
> companies with measurable positive impact in education or mental health.
> Our board reviews this intent annually; otherwise, run on autopilot."*

### D. The Student (age 19)

> *"I have $3,000. I won't need it for at least 20 years. Go as aggressive
> as you want. Maximum growth. I can handle 80% drawdowns without panicking.
> Teach me what you're doing as you go — send me a monthly plain-language
> summary I can actually learn from."*

These four intents produce fundamentally different portfolios, different
order flow, and different risk profiles — without any human ever placing
an order.

---

## Translation Protocol

### NF → CF

When a principal writes NF, an agent (or a dedicated translation service)
produces a draft CF. The principal MUST review and sign the CF before it
becomes active. The Translation Record preserves:

- Which model performed the translation
- The exact NF text (hashed)
- The generated CF
- Ambiguities flagged during translation
- The principal's resolutions of those ambiguities

### CF → NF

Rarer, but possible: a principal with technical fluency may write CF
directly. A natural-language rendering is generated for their confirmation.

### Ambiguity Resolution

When NF is ambiguous, the translator MUST NOT guess silently. Instead:

1. List each ambiguity as a question.
2. Present plausible interpretations.
3. Require principal response before finalizing CF.

Examples of NF ambiguities that MUST be surfaced:

- *"Avoid fossil fuels"* — Does this include transportation companies that
  consume fossil fuels? Utilities with mixed energy portfolios? Index funds
  with minor exposure?
- *"Small companies"* — By what metric? Market cap? Employee count?
  Revenue? What threshold?
- *"Ethical"* — According to which framework?

Silent interpretation of ambiguity is a fault-attributable error under
AIP-002 §3.2.

---

## Intent Composition

A principal MAY have multiple concurrent intents with different scopes
(e.g., retirement account, speculative account, charitable account).

When intents overlap or conflict:

- **Scope priority** — A narrower intent (e.g., specific account) overrides
  a broader one (e.g., all holdings).
- **Hardness priority** — A hard constraint overrides a soft preference,
  regardless of scope.
- **Recency priority** — Among intents of equal scope and hardness, the
  more recent one wins.

If these three rules cannot resolve a conflict, the agent MUST pause and
request principal clarification.

---

## Intent Evolution

Intents are not static. A principal's circumstances change: marriage,
children, career transitions, retirement, bereavement. The protocol must
accommodate this.

**Supersession** — A new Intent Object MAY supersede a prior one. The prior
Intent Object remains in the audit log but is no longer active. Positions
acquired under the old intent may be wound down gracefully per the new
intent's rules.

**Amendment** — Minor changes (e.g., raising a position limit) MAY be
expressed as amendments rather than full supersession. Amendments inherit
the parent intent's ID with a version increment.

**Life Events** — An intent MAY specify automatic adaptation triggers:
*"On the event of my verified death, revoke this intent and transfer all
assets to the intent-ID specified in my will."* This requires external
attestation (Philosophical Choice 5).

---

## Privacy Considerations

An intent can reveal deeply personal information: wealth level, life
expectations, values, family structure. The protocol MUST support:

- **Selective disclosure** — The intent can be revealed in parts. The
  market sees only what it needs to enforce; counterparties see nothing;
  auditors see everything under subpoena.
- **Zero-knowledge proofs of compliance** — An agent can prove its actions
  are within intent without revealing the intent itself.
- **Principal-only full access** — Only the principal (and legally
  authorized successors) can read the complete intent.

The cryptographic substrate chosen in AIP-002 Choice 4 determines which of
these are achievable.

---

## Adversarial Considerations

**Prompt Injection** — A malicious NF text designed to manipulate the
translator model. Countered by (a) sandboxed translation, (b) mandatory
human review of CF before binding, (c) append-only translation records.

**Ambiguity Exploitation** — A counterparty or market participant
manipulates the boundaries of an ambiguous term (e.g., "small company") to
trigger agent behavior. Countered by resolving ambiguities at intent
creation, not at execution time.

**Intent Drift** — Slow accumulation of minor amendments that collectively
change the intent's meaning. Countered by periodic full review (default 6
months) and a drift_tolerance parameter that triggers principal
notification.

**Interpretation Arbitrage** — Different agents translating the same NF to
meaningfully different CFs. Countered by publishing reference translators
and requiring translation records.

---

## The Five Philosophical Choices (Open)

### Choice 1 — Which form is authoritative?

| Option | Description |
|---|---|
| A | NF is authoritative; CF is a best-effort derivation. |
| B | CF is authoritative; NF is human documentation. |
| C | Both are authoritative; divergence is an error that MUST halt the agent. |
| D | Per-field precedence: some fields NF-authoritative, others CF. |

**Trade-off**: Principal comprehension vs. machine enforceability.
**Author's current lean**: **C**. Divergence should never occur silently.

### Choice 2 — Who is responsible for translation quality?

| Option | Description |
|---|---|
| A | The principal (caveat scriptor). |
| B | The translator service, under AIP-002 fault attribution. |
| C | Shared — principal signs NF, translator signs CF, both visible. |
| D | A specialized "Notary" role that certifies the NF-CF pair. |

**Author's current lean**: **C** for default, **D** for high-stakes intents
(>$1M, institutional).

### Choice 3 — How should NF-CF divergence be handled?

| Option | Description |
|---|---|
| A | Halt the agent until principal clarifies. |
| B | Apply the more conservative interpretation (most restrictive). |
| C | Apply the more recent interpretation. |
| D | Principal chooses default at intent creation. |

**Author's current lean**: **D** with **A** as the default default.

### Choice 4 — Should intents be composable or atomic?

| Option | Description |
|---|---|
| A | Atomic: one intent per principal per account. |
| B | Composable: multiple intents with scope rules (as specified above). |
| C | Hierarchical: one "parent" intent, with child sub-intents inheriting. |

**Author's current lean**: **B**.

### Choice 5 — How are life-event triggers authenticated?

| Option | Description |
|---|---|
| A | External oracle (government death registry, court orders). |
| B | Multi-party signature (designated successors attest). |
| C | Dead-man's switch (silence for N days triggers). |
| D | All three composable per principal preference. |

**Author's current lean**: **D**.

---

## Open Questions

1. Should the Intent Object be versioned as a whole, or should each section
   have independent versioning?

2. How does the protocol handle intents expressed in languages with no
   established financial vocabulary?

3. Is there a "minimum viable intent" short enough to onboard a new
   principal in under 5 minutes?

4. How should intents interact with tax optimization, which is often
   jurisdiction-specific and time-varying?

5. Can an intent reference another intent (e.g., "do what my spouse's
   intent does, minus their sector preferences")?

6. Is there value in publishing anonymized aggregate intent statistics
   (how many intents exclude fossil fuels, what average drawdown
   tolerance is)? Or does that enable coordination attacks?

7. Should intents include provisions for when the market itself fails
   (liquidity crises, systemic events)?

8. How do we prevent "intent brokers" from becoming a centralized
   chokepoint between principals and agents?

---

## Relationship to Other AIPs

| AIP | Relationship |
|---|---|
| AIP-001 | Justifies the need for intent-based investing. |
| AIP-002 | Provides the identity layer that binds principals to intents. |
| AIP-004 | TINY listings may specify which intent patterns are eligible. |
| AIP-005 | Reflection markets verify agent-to-intent alignment. |
| AIP-009 | Dispute resolution depends on intent interpretation. |

---

## Call to Contributors

Write an intent. In natural language. For a hypothetical principal you
find interesting: a climate scientist, a widowed grandparent, a DAO
treasury, a sovereign wealth fund, a twelve-year-old learning about
markets.

Open an Issue. Share the NF. Let the community and their AIs generate
CFs. Compare divergences. That is how this specification gets stress-tested.

---

## References

- AIP-001 (Core Thesis) — established the need for intent-based design
- AIP-002 (Agent Authentication) — provides the binding layer
- EIP-7521: General Intents — https://eips.ethereum.org/EIPS/eip-7521
- Anoma Intent-Centric Architecture — https://anoma.net/
- Human-readable smart contract signing (EIP-712) — https://eips.ethereum.org/EIPS/eip-712
- "Intent-Based Access Control" (IBAC) — academic literature on the
  underlying philosophical pattern.

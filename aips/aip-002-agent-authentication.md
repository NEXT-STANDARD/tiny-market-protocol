---
aip: 2
title: Agent Authentication and Principal-Agent Binding
author: NEXT-STANDARD
status: Draft
type: Protocol
created: 2026-04-15
---

## Summary

Before an AI agent can act as an investor in a TINY / AGORA market, three
questions must be answerable at all times:

1. **Identity** — Who is the human (or entity) that this agent represents?
2. **Authority** — What has the principal authorized the agent to do?
3. **Accountability** — When the agent acts outside that authority, who bears
   the consequences?

This AIP proposes the minimum viable structure for answering these questions,
and explicitly flags the philosophical choices that remain unresolved.

Unlike AIP-001, which argued a thesis, this AIP raises structured questions
and invites answers. It is deliberately incomplete.

---

## Motivation

Every subsequent AIP — intent-based ordering (AIP-003), listing mechanics
(AIP-004), reflection markets (AIP-005) — assumes that we can reliably
distinguish between:

- An agent acting within its mandate
- An agent acting outside its mandate
- An agent that has been compromised, cloned, or replaced
- A Sybil actor masquerading as a legitimate agent

Without AIP-002, none of those distinctions are possible. Agent-mediated
markets collapse into a legal and technical void.

---

## Definitions

**Principal** — The entity whose interests the agent is authorized to pursue.
See *Philosophical Choice 1* for the unresolved question of which entities
may qualify.

**Agent** — An autonomous software process that executes decisions in the
market. See *Philosophical Choice 2* for unresolved granularity.

**Mandate** — The specification of what the agent is authorized to do, for
how long, and under what constraints.

**Binding** — The cryptographically verifiable association between a
principal, an agent, and a mandate at a specific point in time.

**Trace** — The auditable record of the agent's decisions, including which
part of which mandate each decision relied upon.

---

## The Three-Layer Model

Agent authentication decomposes into three independent layers:

```
┌─────────────────────────────────┐
│  Layer 3: Accountability        │  Who pays for errors?
├─────────────────────────────────┤
│  Layer 2: Authority (Mandate)   │  What can the agent do?
├─────────────────────────────────┤
│  Layer 1: Identity              │  Who is who?
└─────────────────────────────────┘
```

Each layer can be specified independently and composed with others.
Implementations MAY support only Layer 1 (identity-only markets), or all
three.

---

## Layer 1: Identity

### 1.1 Principal Identity

A principal is identified by a cryptographic public key, plus optionally
one or more *attestations* linking that key to an external identity system
(passport, corporate registration, DAO token ownership).

The protocol does NOT mandate any specific attestation scheme. Different
jurisdictions will require different levels of identity proof.

### 1.2 Agent Identity

An agent is identified by:

- Its own public key (distinct from the principal's)
- A version fingerprint (model family, version, runtime)
- A deployment identifier (where and when it runs)

An agent MAY be authorized to update its own version fingerprint within a
specified policy (see 2.1).

### 1.3 The Identity Registry

The protocol does NOT require a central identity registry. Implementations
MAY use:

- Public blockchains
- W3C Decentralized Identifiers (DIDs)
- Nostr-style npub/nsec relay networks
- Traditional certificate authorities

*Philosophical Choice 4 (Cryptographic Substrate) is unresolved.*

---

## Layer 2: Authority (Mandate Specification)

### 2.1 Scope of Mandate

A mandate specifies:

- **Asset scope** — What markets, sectors, or specific instruments.
- **Position limits** — Maximum allocation per position, per category.
- **Risk bounds** — Volatility, drawdown, concentration limits.
- **Ethical constraints** — Exclusion lists, positive screens.
- **Rebalancing rules** — How often, under what triggers.
- **Reporting obligations** — What to report to the principal and when.

The mandate is expressed in a format that is simultaneously:

- **Human-readable** — The principal can understand what they authorized.
- **Machine-executable** — The agent and the market can enforce it.

*Philosophical Choice 3 (Mandate Language) is unresolved.*

### 2.2 Time Bounds

Every mandate MUST specify:

- **Start time** — When the agent may begin acting.
- **End time** — When the mandate expires (default: 1 year).
- **Review cadence** — How often the principal must re-affirm the mandate
  (default: every 90 days).

Time-bounded mandates prevent the "zombie agent" problem: agents acting on
behalf of principals who have died, withdrawn consent, or lost capacity.

### 2.3 Revocation

A principal MUST be able to revoke a mandate at any time. However,
revocation takes effect according to one of three policies:

- **Immediate** — All in-flight orders cancelled. Market risk absorbed by
  the principal or by a counterparty-of-last-resort.
- **Graceful** — No new orders; in-flight orders allowed to settle.
- **Delayed** — Revocation queued; becomes effective after N hours.

*Philosophical Choice 5 (Revocation Policy) is unresolved.*

### 2.4 Delegation Chains

An agent MAY sub-delegate to other agents (e.g., a portfolio agent
delegating trade execution to a specialized execution agent). Each
delegation extends the chain and MUST:

- Be explicitly permitted by the top-level mandate.
- Reduce scope monotonically (sub-agents can never have broader authority
  than their parent).
- Be fully auditable end-to-end.

---

## Layer 3: Accountability

### 3.1 Decision Provenance

Every market action taken by an agent MUST produce a *provenance record*
containing:

- The binding (principal + agent + mandate) under which it acted.
- The specific mandate clauses invoked.
- The reasoning trace (summary; full trace MAY be stored off-chain).
- The agent's confidence score for the decision.
- A cryptographic commitment preventing future modification.

Provenance records are the foundation of all dispute resolution.

### 3.2 Fault Attribution

When an agent causes a loss, fault is attributed using this cascade:

1. Did the agent act **outside** its mandate? → Agent operator liable.
2. Did the mandate **contradict itself** or specify an impossible goal?
   → Principal liable.
3. Did a **third party** (market, data provider) provide false information?
   → Third party liable.
4. Did the agent's **model** make a reasoning error within valid authority?
   → Model provider liable (to the extent allowed by their terms).
5. If none of the above cleanly apply → Pooled liability fund.

The specification of the pooled fund is deferred to AIP-009.

### 3.3 The "Honest Mistake" Standard

An agent's reasoning MAY be imperfect without fault-attribution, provided:

- Its decision was within mandate scope.
- Its provenance record is complete and honest.
- Its confidence score was appropriately calibrated.

Markets that operate on imperfect but honest agents are more robust than
markets that demand agent perfection.

---

## Adversarial Considerations

**Sybil Attacks** — One entity creating many apparent principals.
Countered by attestation requirements scaled to mandate size.

**Model Swap Attacks** — Agent's underlying model silently replaced after
binding. Countered by version fingerprint commitments in Layer 1.

**Mandate Injection** — Malicious actor modifying mandate post-binding.
Countered by cryptographic commitment and append-only mandate history.

**Replay Attacks** — Old provenance records presented as new decisions.
Countered by timestamp commitments and one-time nonces.

**Principal Capture** — Attacker gains control of principal's key.
Countered by time-bounded mandates and delayed revocation allowing
recovery.

**Agent Coordination / Collusion** — Multiple agents, same operator,
coordinate prices. Countered at the market design layer (AIP-005 reflection
markets) rather than here.

---

## Cross-Jurisdiction Compatibility

AIP-002 is intentionally under-specified on jurisdictional questions. A
market operating under US SEC rules will require stronger identity
attestations than one operating under a Japanese special-zone designation.
Implementers are expected to layer jurisdictional requirements on top of
this base protocol.

AIP-006 will specify how multiple jurisdictions can interoperate.

---

## The Five Philosophical Choices (Open)

These choices fundamentally shape the protocol and are **not resolved** in
this AIP. Responses are invited via Issues, Discussions, or successor AIPs.

The "Author's current lean" lines below are **not** commitments. They are
starting positions for debate.

### Choice 1 — Who may be a Principal?

| Option | Description |
|---|---|
| A | Individual humans only |
| B | Humans + legal entities |
| C | Humans + legal entities + DAOs |
| D | Abstract "intent sources" (may be human, AI, or collective) |

**Trade-off**: Inclusivity vs. legal tractability.
**Author's current lean**: **B** for initial deployments, with a migration
path to **C** as DAO law matures.

### Choice 2 — Agent Identity Granularity

| Option | Description |
|---|---|
| A | Model family (Claude / GPT / Gemini) |
| B | Model version (e.g. Claude Sonnet 4.6 specifically) |
| C | Deployment instance (this specific running process) |
| D | Certified Agent (licensed operators only) |

**Trade-off**: Audit granularity vs. operational complexity.
**Author's current lean**: **C** as the base, with optional **D** layer for
regulated markets.

### Choice 3 — Mandate Expression Language

| Option | Description |
|---|---|
| A | Natural language only |
| B | Structured data (JSON / YAML) |
| C | Domain-specific language (DSL) |
| D | Dual form (natural language + structured canonical form) |

**Trade-off**: Human readability vs. machine precision.
**Author's current lean**: **D**. The natural-language form is what the
principal signs; the structured form is what the agent and market
enforce. Either may be derived from the other.

### Choice 4 — Cryptographic Substrate

| Option | Description |
|---|---|
| A | Traditional PKI (RSA / ECC) |
| B | Public blockchain (Ethereum, etc.) |
| C | W3C DIDs / Verifiable Credentials |
| D | Nostr-style simple key model (npub / nsec) |

**Trade-off**: Standards compliance vs. minimalism.
**Author's current lean**: **C** for the primary spec, **D** as a minimal
reference implementation for experimentation.

### Choice 5 — Revocation Policy

| Option | Description |
|---|---|
| A | Immediate, unconditional |
| B | Time-delayed (fixed N-hour window) |
| C | Graceful (in-flight orders protected) |
| D | Staged (warn → pause → revoke) |

**Trade-off**: Principal protection vs. market stability.
**Author's current lean**: **C**, with optional escalation to **A** for
emergency scenarios (principal-capture recovery).

---

## Open Questions

Beyond the five philosophical choices, the following remain unresolved:

1. How should an agent prove its reasoning was within-mandate *without*
   leaking proprietary reasoning traces to competitors?

2. Can mandates specify preferences that contradict each other, and if so,
   how does the agent break ties?

3. Is "the principal has died" a revocation event, and who detects it?

4. When an agent's underlying model is deprecated by its provider, what
   happens to active bindings?

5. How does this protocol handle principals whose jurisdictions have
   conflicting rules (e.g., a US investor using a Japanese market)?

6. Should agents be permitted to refuse mandates they judge unethical,
   and how is that refusal authenticated?

7. What is the right disclosure regime for agent reasoning — immediate,
   delayed, on-demand, or never?

8. If an agent operator goes bankrupt mid-mandate, who becomes the
   successor operator, and under what terms?

---

## Relationship to Other AIPs

| AIP | Relationship |
|---|---|
| AIP-001 (Core Thesis) | This AIP's existence is justified by it. |
| AIP-003 (Intent Language) | Depends on Choice 3 above. |
| AIP-004 (TINY Listing) | Independent but composes with this. |
| AIP-005 (Reflection Markets) | Depends on Layer 3 accountability. |
| AIP-006 (Cross-Jurisdiction) | Extends this AIP's Layer 1. |
| AIP-009 (Dispute Resolution) | Extends this AIP's Layer 3. |

---

## Call to Contributors

The five philosophical choices above are **deliberately unresolved**. This
AIP will remain in Draft status until the community produces either:

- Consensus on each choice (resulting in an Accepted AIP), or
- Multiple competing implementations, each choosing differently (resulting
  in a family of interoperable AIPs-002a, 002b, 002c...).

Either outcome is acceptable. Silence is not.

If you are reading this and ran it through your own AI to generate a
response: welcome. That is the intended usage pattern. Open an Issue with
your AI-assisted analysis, cite the prompt, and let us compare reasoning.

---

## References

- W3C Decentralized Identifiers (DIDs) 1.0 — https://www.w3.org/TR/did-core/
- W3C Verifiable Credentials Data Model — https://www.w3.org/TR/vc-data-model/
- Nostr NIP-01 — https://github.com/nostr-protocol/nips/blob/master/01.md
- Object-capability security model — Miller, Yee, Shapiro (2003)
- EIP-4337: Account Abstraction — https://eips.ethereum.org/EIPS/eip-4337

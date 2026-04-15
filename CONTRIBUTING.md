# Contributing to TINY Market Protocol

This is a public-domain seed document. Contributions are welcomed from anyone
and no copyright is retained by contributors — by opening a pull request you
agree that your contribution is released under [CC0](./LICENSE).

---

## Ways to Contribute

### 1. File an AIP (AGORA / TINY Improvement Proposal)

AIPs are the primary mechanism for evolving this protocol. Each AIP addresses
one specific topic. See [aips/aip-template.md](./aips/aip-template.md).

**Process**:
1. Fork this repository.
2. Copy the template to `aips/aip-XXX-short-title.md` (use the next available
   number).
3. Fill in every section of the template. Incomplete AIPs are closed.
4. Open a pull request.
5. Discussion happens in the PR. AIPs may be revised, merged as Draft, or
   rejected.

An AIP does not require consensus to be merged. Contradictory AIPs can coexist
(see `status: Draft` vs `status: Accepted` vs `status: Rejected`).

### 2. Contradict the Thesis

If you have data that undermines any empirical claim in this repository, open
an Issue with the label `contradicting-evidence` and cite sources. This is
**more valuable** than supporting evidence.

### 3. Translate

The `translations/` directory accepts any language. Start with README,
MANIFESTO, and AIP-001. Open a PR.

### 4. Implement a Fragment

Even a small working demo — an intent-language parser, an agent-auth scheme,
a mock listing — is more valuable than any AIP. Link your implementation from
the relevant AIP via a PR.

### 5. Contribute Research

The `research/` directory accepts new data, citations, and analyses. If you
find a 2026-or-later data point that changes the picture, submit it.

---

## Discussion Norms

- **Cite sources.** Claims without citations are not admissible evidence.
- **Steelman opposition.** Before rejecting an argument, state its strongest
  form in your own words.
- **Separate empirical from normative.** "Is" questions and "ought" questions
  go in different sections.
- **Be willing to be wrong in public.** Everyone contributing here is.
- **No tokens, no fundraising, no solicitation.** This repository is a
  document, not a venture. Violations will be removed.

---

## What Does NOT Belong Here

- Investment advice or trading recommendations.
- Solicitations for any fund, token sale, or commercial venture.
- Proprietary code or documents — everything merged here becomes public domain.
- Personal attacks. Criticize ideas, not people.

---

## Governance (or Lack of It)

There is no formal governance. The original author merges PRs for the first
phase. If/when the repository grows beyond that, governance can itself be
proposed as an AIP.

---

## Questions

Open a GitHub Discussion in the repository. Issues are for specific actionable
items; Discussions are for open-ended questions.

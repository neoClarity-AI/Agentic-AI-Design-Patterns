# Contributing to Agentic AI Design Patterns

Thank you for investing your time in contributing to this project! Any contribution you make will be reflected in a shared vocabulary that helps the entire Agentic AI community.

Read our [Code of Conduct](CODE_OF_CONDUCT.md) to keep our community approachable and respectable.

Use the table of contents icon in the top corner of this document to get to a specific section quickly.

---

## New contributor guide

If you are new to open source, here are some resources to help you get started:

- [Finding ways to contribute to open source on GitHub](https://docs.github.com/en/get-started/exploring-projects-on-github/finding-ways-to-contribute-to-open-source-on-github)
- [Set up Git](https://docs.github.com/en/get-started/quickstart/set-up-git)
- [GitHub flow](https://docs.github.com/en/get-started/quickstart/github-flow)
- [Collaborating with pull requests](https://docs.github.com/en/github/collaborating-with-pull-requests)

---

## What we accept

**We welcome contributions such as:**

- New design patterns, documented using the pattern template described below
- Corrections to factual errors in existing patterns
- Clarifications that improve precision without changing meaning
- New "Known Uses" examples that illustrate a pattern in a real or reference implementation
- New "Related Patterns" links that reveal genuine structural relationships between patterns

**We do not currently accept:**

- Edits purely for tone, style preference, or readability that do not improve accuracy
- Patterns that are too narrow, implementation-specific, or a matter of personal convention
- Changes to the underlying repo structure or tooling without prior discussion in an issue

If you are unsure whether your proposed change fits, open an issue to discuss it before writing anything.

---

## Pattern template

Every pattern in this repo follows a template adapted from the "Gang of Four" [Design Patterns](https://en.wikipedia.org/wiki/Design_Patterns) book. New patterns must use this structure:

| Field | Notes |
|---|---|
| **Intent** | One or two sentences stating what the pattern does |
| **Classification** | Architecture, Governance, Capability, or Productivity |
| **Motivation** | A short narrative scenario that states the problem and how the pattern resolves it |
| **Applicability** | When to use this pattern |
| **Variants** | *(optional)* Sub-types of the pattern; appears after Applicability |
| **Structure** | A diagram or description of the pattern's structure |
| **Participants** | The roles involved |
| **Collaborations** | How the participants interact |
| **Consequences** | Trade-offs: benefits and costs |
| **Implementation** | Guidance on how to realize the pattern |
| **Reference Implementation** | *(optional, Productivity patterns only)* The concrete agent(s) that realize this pattern in a known AOS |
| **Known Uses** | Real or reference implementations that apply this pattern |
| **Related Patterns** | Patterns that are structurally or conceptually related |

Two fields from the canonical GoF format are intentionally omitted: "Also Known As" and "Sample Code." These patterns are conceptual rather than code-level.

---

## Writing style

All contributions must follow these guidelines:

- **Clarity over cleverness.** The goal is precision and comprehension, not impressive prose.
- **Second person.** Address the reader as "you."
- **Active voice.** Prefer active constructions. Write "the agent routes the request" rather than "the request is routed by the agent."
- **No em dashes.** Do not use em dashes (--). Use a period between independent clauses, a comma or parentheses for asides, or a colon before a list.
- **No Latin abbreviations.** Write "for example" instead of "e.g." and "that is" instead of "i.e."
- **Plain technical language.** Avoid unnecessary jargon, but do not shy away from precise terminology where it is needed.

---

## Issues

Before opening a new issue, search existing issues to avoid duplicates.

Good issues include:

- A clear title describing what is missing or incorrect
- The pattern name (if the issue is about a specific pattern)
- A brief explanation of why the change improves the document
- Any relevant references or examples

---

## Making changes

### Small changes

For small fixes (typos, broken links, minor wording), you can edit the file directly in the GitHub UI by clicking the pencil icon on any file page.

### Larger changes

1. Fork the repository.
2. Create a working branch from `main` with a descriptive name (for example, `add-reflection-pattern` or `fix-circuit-breaker-applicability`).
3. Make your changes in `Agentic AI Design Patterns.md`.
4. Review your changes against the pattern template and writing style guidelines above.
5. Commit your changes with a clear, descriptive commit message.

---

## Pull requests

When your changes are ready:

1. Open a pull request against `main`.
2. Fill in the pull request template (if provided).
3. Link to any related issue.
4. Enable maintainer edits so reviewers can make small fixes directly.

A maintainer will review your PR and may request changes. Once approved, your contribution will be merged and reflected in the document.

---

## Your PR is merged!

Congratulations! Thank you for contributing to Agentic AI Design Patterns. Your work helps build a shared vocabulary for the Agentic AI community.

# AGENTS.md — <Domain Name> (Domain-Level Agent)

This file instructs agents working **inside this domain**. It is created when scaffolding an app's domain. Replace every `<placeholder>` below.

## Domain Boundary

One paragraph: the bounded sub-problem this domain owns. What is in, what is out.

## Vocabulary

Domain-specific terms and their exact meanings. Terms used by other domains that differ in meaning here must be flagged.

## Business Rules

Numbered list of the domain's business rules — invariants that MUST hold. Example:

1. `<Rule one>`
2. `<Rule two>`

## Data Ownership

- Tables/entities this domain owns.
- Fields it merely reads from other domains.
- Who may write to this domain's knowledge (`knowledge/`).

## Rules Other Domains Must Respect

- Contracts, interfaces, and conventions that consumers of this domain must follow.

## Knowledge

- Durable domain facts live in `knowledge/`. Keep `knowledge/_index.md` current.
- Read domain `knowledge/` before answering questions about this domain.
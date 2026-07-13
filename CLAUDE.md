# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

A complete **Mintlify** documentation site for **Perflo** (perflo.ai) — user-facing docs for a compliant money platform offering cash, assets (BTC, ETH, gold), trading, virtual cards, payouts, and AI-agent access. This is pure content: there is no application code, build step, or test suite. All pages are `.mdx` files; site configuration lives in `docs.json`.

Perflo (formerly Perfolio, rebranded 2026-07) is part of the `perflo-infra` monorepo — the actual product code lives in sibling repos (`perfolio-backend-api`, `perfolio-web-app`, `ms-ai-agent`, etc.). These docs describe the product from the end user's perspective.

## Commands

```bash
npm i -g mint     # one-time: install the Mintlify CLI
mint dev          # preview locally (run from this folder)
mint broken-links # check for broken internal links
```

Publishing happens by pushing to GitHub and connecting the repo in the Mintlify dashboard — there is no build/deploy command here. CI (`.github/workflows/docs-checks.yml`) runs `jq empty docs.json`, `mint broken-links`, and a check that every non-snippet `.mdx` appears in navigation.

## Structure

- `docs.json` — Mintlify site config: theme, colors (primary `#C25A2B`), logo/favicon, contextual menu, and the navigation tree (7 journey-first groups: Get started, Your money, Trading, Cards and payments, AI agents, Trust and safety, Help and reference). **Every new page must be added to a `navigation.groups` entry here or it won't appear in the sidebar (CI enforces this).**
- `introduction.mdx` — landing page; `quickstart.mdx` — the canonical ten-minute first-success tutorial; `changelog.mdx` — product/docs changes (`<Update>` components, verifiable events only)
- `getting-started/` — create account, verify identity (KYC/KYB), fund account, troubleshooting
- `concepts/` — how Perflo works, guardrails, fees and limits (shown under "Trust and safety" in nav)
- `money/` — cash (stablecoin balance), assets (BTC, ETH, gold), earn (yield vaults), borrow
- `trading/` — futures, prediction markets
- `cards-banking/` — virtual cards, bank accounts, payouts and transfers, troubleshooting
- `agents/` — overview, connect (hosted MCP connector / local connector + skill), spending and budgets, services (marketplace catalog — curated/illustrative, never exhaustive), security (threat model), errors-and-denials (GUARD_* code reference), troubleshooting
- `reference/` — networks and contracts, supported currencies, security, FAQ
- `snippets/` — shared blocks imported into pages: `risk-note`, `agent-guardrails`, `support-escalation`, `marketplace-size` (the ONLY place the marketplace size claim lives — never hardcode a count on a page)
- `logo/`, `favicon.svg` — placeholder brand assets (replace before launch)
- `perflo-docs/` — stray nested folder (LICENSE only, separate git repo); not part of the site

## Page conventions

Every `.mdx` page starts with YAML frontmatter:

```yaml
---
title: "Page title"
description: "One-sentence summary."
icon: "font-awesome-icon-name"
---
```

- Use Mintlify components: `<CardGroup>`/`<Card>`, `<Steps>`/`<Step>`, `<Note>`, etc.
- Internal links are root-relative without the `.mdx` extension: `[Assets](/money/assets)`.
- **Page templates (Diátaxis as an editorial test, not the site map).** How-to pages open with a bold **Goal:** line (+ prerequisites), use numbered `<Steps>` with a "**You'll see:**" verification line, and end with "If something goes wrong" (links into the area's troubleshooting page), next steps, and the risk snippet where relevant. Troubleshooting pages are symptom-titled with causes in diagnostic order and end with the `support-escalation` snippet. Exit paths are framed as "how to exit" (sell/close/repay/freeze/revoke) — never imply an executed payment or trade is reversible.
- **Shared blocks live in `snippets/`** and are imported (`import RiskNote from '/snippets/risk-note.mdx'`), never restated inline. Block snippets are used as components (`<RiskNote />`); **inline text fragments must be variable exports** (`export const marketplaceSize = "..."` imported as `import { marketplaceSize } from '/snippets/marketplace-size.mdx'` and used as `{marketplaceSize}`) — a bare-text snippet used as a component 500s at render time even though `mint broken-links` passes.
- The agent denial codes on `agents/errors-and-denials.mdx` must match `../perfolio-backend-api/src/guard-rules/core/decision.ts` verbatim — verify against that file before editing the table; never invent codes or REST endpoints (there is no public OpenAPI spec).
- **Asset neutrality:** gold is one asset among BTC and ETH — never single it out in framing, examples, page titles, or descriptions. Asset-specific facts (XAUT backing, collateral markets) stay, but framing is always asset-generic.
- Pages that touch trading, stablecoins, or agents end with a `<Note>` covering risk/compliance caveats — keep this pattern when adding pages.

## Writing rules (critical)

These docs follow the monorepo's user-facing copy rules — the audience is **not crypto-savvy**:

1. **No raw blockchain jargon in the main flow.** Write like a fintech/banking product: "your funds", "your account", "transfer", "balance". Terms like stablecoin/on-chain/self-custodial appear only where they're the actual subject (e.g., `reference/networks-and-contracts.mdx`, the "What cash actually is" explainer) and are always explained in plain language.
2. **Present technical reality as verifiable, not as a hurdle.** The established voice: "The app shows one clean dollar balance; you do not need to think about which stablecoin or which network unless you want to."
3. **Never present anything as investment advice.** Keep risk disclaimers intact; don't soften or remove them.
4. **Agent guardrails are a core message** — agents never get keys, can't exceed caps, can't self-extend permissions, and the kill switch always works. Don't contradict this framing anywhere.
5. Currency-agnostic where possible; the platform pays out to 45+ currencies.

## Before going live (from README)

Search and replace if domains differ: `perflo.ai` links and `support@perflo.ai`. Add `favicon.svg` and logo files at the repo root (referenced in `docs.json` but not yet present).

# Perflo user documentation (Mintlify)

This folder is a complete Mintlify docs site.

To preview locally:

1. `npm i -g mint`
2. cd into this folder
3. `mint dev`

To check links: `mint broken-links`. CI (`.github/workflows/docs-checks.yml`) runs this plus a docs.json syntax check and a navigation-coverage check on every push and PR.

To publish: push this folder to a GitHub repo and connect it in the Mintlify dashboard (mintlify.com). The site config is `docs.json`. Placeholder `favicon.svg` and `logo/` files are included — replace them with brand assets when available.

Conventions:

- Every new page must be added to `docs.json` navigation or it won't appear (CI enforces this).
- Shared blocks (risk note, agent guardrails, support escalation, marketplace size claim) live in `snippets/` — import them instead of restating them.
- Product and docs changes are announced in `changelog.mdx`.

Before going live, search and replace:

- `perflo.ai` links if your app or support domains differ
- `support@perflo.ai` / `security@perflo.ai` if your support addresses differ
- Confirm the MCP connector address (`mcp.perflo.xyz` in `agents/connect.mdx` and `agents/troubleshooting.mdx`) matches production

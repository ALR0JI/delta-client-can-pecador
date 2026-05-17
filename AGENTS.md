# Delta — Project Context

Delta is a digital agency. This repo is one of:

- **`delta-engine`** — the monorepo containing `packages/sections/`, `packages/themes/`, `packages/industries/`, `packages/core/`, and `packages/cli/`. You edit code there.
- **`delta-client-<id>`** — a thin client repo that imports `@delta/*` via pnpm. You NEVER edit `packages/` from here.

**This is `delta-client-can-pecador` — a CLIENT repo.**

**How to detect which repo you're in:**

- Engine repo: `packages/` exists at root, no `delta.config.json` at root.
- Client repo: `delta.config.json` exists at root, no `packages/` at root.

## Stable rules (apply to every agent)

1. **Source of truth:** `build-manifest.json` is the contract between Strategist and Builder. `delta.config.json` is the brand brief.
2. **Locales:** read `brand.locales` from `delta.config.json`. Catalan (`ca`) is first-class in Barcelona projects.
3. **Quality bar:** Lighthouse mobile ≥ 95 on perf/a11y/SEO/best-practices. WCAG 2.1 AA. No exceptions.
4. **Stack:** Next.js 15 App Router · TypeScript strict · Tailwind v4 · next-intl · pnpm. Do not propose alternatives without an ADR.
5. **Deploys:** push to `main` triggers Vercel production. Preview deploys per PR.
6. **Naming:** kebab-case for client ids and file slugs; PascalCase for React components; camelCase for props.

## Client-specific rules

1. **NEVER edit `@delta/*` packages from this repo.** They are dependencies. Changes to sections, themes, or UI primitives happen in `delta-engine`, not here.
2. **NEVER create `packages/` or `src/components/` directories.** All reusable components come from `@delta/sections-*` and `@delta/ui`.
3. **Content lives in `content/<locale>.json`.** Copy never goes inline in components.
4. **The Builder executes `build-manifest.json` mechanically.** It reads the manifest and generates pages using `@delta/sections-*` imports.

## Agents present in this repo

- `@strategist` — planning, manifests, ADRs (primary — works in engine, NOT here)
- `@builder` — code generation (primary — works HERE in client repos)
- `@reviewer` — QA + design lint (primary)
- `@content` — copy + i18n (subagent)
- `@seo` — metadata + JSON-LD + sitemap (subagent)
- `@librarian` — catalog lookup (subagent)

Switch between primaries with `Tab`. Invoke subagents with `@name` or via the Task tool.

## OpenCode setup

This repo was initialized by copying OpenCode agents and skills from `delta-engine`.
See `_docs/migration.md` for the full setup checklist used when creating new client repos.

## Documents to read on session start

- This file (`AGENTS.md`).
- `delta.config.json` — the brand brief for this client.
- `build-manifest.json` — the current spec for this site.
- `_docs/migration.md` — how this repo was set up.

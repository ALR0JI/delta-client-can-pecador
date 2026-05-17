# Client Repo Migration Checklist

> Use this checklist every time a new client repo is created. Run these steps from `delta-engine` to ensure the client repo has all OpenCode agents and skills working correctly.

## Source of truth

- **Engine repo:** `delta-engine` (`~/Code/Delta/delta-engine`)
- **Client repos:** `delta-client-<id>` (`~/Code/Delta/delta-client-<id>/`)

All client repos share the same agents and skills. When a new agent or skill is added to `delta-engine`, propagate it to all client repos.

## Step 1 — Clone the client repo

```bash
mkdir -p ~/Code/Delta
cd ~/Code/Delta
gh repo clone ALR0JI/delta-client-<id>
cd delta-client-<id>
```

## Step 2 — Copy OpenCode agents

```bash
mkdir -p .opencode/agents
cp ~/Code/Delta/delta-engine/.opencode/agents/*.md .opencode/agents/
cp ~/Code/Delta/delta-engine/.opencode/package.json .opencode/
cp ~/Code/Delta/delta-engine/.opencode/.gitignore .opencode/
```

## Step 3 — Copy skills

```bash
mkdir -p .agents
cp -r ~/Code/Delta/delta-engine/.agents/skills .agents/
```

## Step 4 — Copy AGENTS.md (adapted for client context)

Write `AGENTS.md` adapted for the client repo. Key differences from the engine version:
- Mark clearly as a CLIENT repo (not engine)
- Add "Client-specific rules" section (never edit packages, content in JSON, no components/ dir)
- Add link to this migration doc

## Step 5 — Create .gitignore (if not exists)

```gitignore
node_modules/
.next/
.vercel
*.tsbuildinfo
.env
.env.local
.env.development
.env.production
```

## Step 6 — Install OpenCode dependencies

```bash
cd .opencode
npm install
cd ..
```

## Step 7 — Commit and push

```bash
git add .opencode/ .agents/ AGENTS.md .gitignore _docs/
git commit -m "chore: initialize OpenCode agents and skills from delta-engine"
git push origin main
```

## Step 8 — Verify

1. Open the repo in VS Code
2. Open OpenCode (`Cmd+Esc`)
3. Run `/agents` — should show strategist, builder, reviewer, content, seo, librarian
4. Run `/models` — should show connected models
5. Verify `@builder` can read `delta.config.json` and `build-manifest.json`

## Files that should NEVER be modified in client repos

| Path | Reason |
|------|--------|
| `.opencode/agents/*.md` | Agent definitions live in engine — propagate changes from there |
| `.agents/skills/*` | Skills live in engine — propagate changes from there |
| `.opencode/package.json` | Managed by engine — don't add client-specific deps here |

## Updating an existing client repo

When delta-engine gets a new agent or skill:

```bash
cd ~/Code/Delta/delta-client-<id>
git pull origin main
cp ~/Code/Delta/delta-engine/.opencode/agents/*.md .opencode/agents/
cp -r ~/Code/Delta/delta-engine/.agents/skills .agents/
git add .opencode/agents/ .agents/skills/
git commit -m "chore: sync OpenCode agents and skills from delta-engine"
git push origin main
```

## Current agents and skills (as of 2026-05-17)

### Agents (6)

| Agent | Role |
|-------|------|
| `strategist` | Planning, manifests, ADRs |
| `builder` | Code generation in client repos |
| `reviewer` | QA + design lint |
| `content` | Copy + i18n (subagent) |
| `seo` | Metadata + JSON-LD + sitemap (subagent) |
| `librarian` | Catalog lookup (subagent) |

### Skills (12)

| Skill | Purpose |
|-------|---------|
| `astro` | Build with Astro web framework |
| `create-github-action-workflow-specification` | Formal CI/CD workflow specs |
| `css-animations` | CSS animation patterns |
| `i18n-expert` | Internationalization/localization setup and audit |
| `image` | AI image generation and optimization |
| `pnpm` | Package manager operations |
| `seo-audit` | Technical SEO auditing |
| `ui-ux-pro-max` | UI/UX design intelligence (50+ styles, 161 palettes, etc.) |
| `using-git-worktrees` | Git worktree isolation |
| `wcag-audit-patterns` | WCAG 2.2 accessibility audits |
| `web-design-reviewer` | Visual design inspection and fixes |
| `zod` | Zod schema validation best practices |

# DevStash

A developer knowledge hub for snippets, commands, prompts, notes, files, images, links and custom types.

@AGENTS.md

## Context files

Read the following to get the full context of the project:

- @context/project-overview.md
- @context/coding-standard.md
- @context/ai-interaction.md
- @context/current-feature.md

## Commands

Use pnpm (pinned in `packageManager`).

- `pnpm dev`: dev server on http://localhost:3000
- `pnpm build` / `pnpm start`: production build and serve
- `pnpm lint`: `biome check && oxlint`. used for a read-only check.
- `pnpm format`: Biome formatter only
- `pnpm exec tsc --noEmit`: type check (no script for it)
- `pnpm fallow`: whole-project analysis (dead code, duplication, complexity). `pnpm fallow:audit` checks only changed code.

No test framework is set up yet.

## Architecture

- Next.js 16 App Router under `src/app`. The `@/*` import alias maps to `src/*`.
- `next.config.ts` enables `cacheComponents`, `partialPrefetching`, and the React Compiler. Check `node_modules/next/dist/docs/` before relying on caching or prefetch behavior.
- Tailwind CSS v4 runs through a Turbopack loader (`@tailwindcss/turbopack` in `next.config.ts`), not PostCSS. There is no `postcss.config` or `tailwind.config`; theme tokens go in `@theme` in `src/app/globals.css`. Geist fonts load via `next/font` in `src/app/layout.tsx` and map to `font-sans`/`font-mono` there.

## Tooling

Each tool has one job:

- Biome: formatting and general lint rules (`biome.json`).
- Oxlint: only the anti-slop rules. Its built-in `correctness` category is off in `.oxlintrc.json`.
- TypeScript: type correctness.
- Fallow: repository analysis. `.fallowrc.json` promotes the cleanup rules to `error`, so an unused dev dependency or component prop fails the gate. A dependency loaded only by string (like the Turbopack loader) needs an `ignoreDependencies` entry.

Pre-commit (Husky + lint-staged in `package.json`) runs Biome, then `fallow fix`, then `oxlint --fix`. `fallow fix` is wrapped in `sh -c "..." --` because it takes no file list; keep the wrapper. A Claude Code PreToolUse hook (`.claude/hooks/fallow-gate.sh`) also runs `fallow audit` before `git commit` and `git push`.

`tools/oxlint/anti-slop/` is the vendored dmmulroy/anti-slop plugin, excluded from Biome, `tsc`, and Fallow. Do not edit it in place; update it with the `install-anti-slop` skill and record the source commit in its `UPSTREAM.md`.

## Branches

- Put every feature and every bug fix on its own branch off `main`: `feature/<name>` or `fix/<name>`. Never commit feature or fix work directly to `main`.
- Before pushing a branch, run `git fetch origin` and `git merge origin/main` to bring it up to date with `main`. The Changelog workflow commits to `main` after each push, so `main` moves often.
- Resolve every merge conflict before pushing, then rerun the checks (`pnpm exec biome check`, `pnpm exec oxlint`, `pnpm exec tsc --noEmit`, `pnpm build`). For a conflict in `CHANGELOG.md`, take the `main` version; the workflow regenerates it.

## Commits

`CHANGELOG.md` is generated from commit messages by Commitizen (`.cz.toml`) in the Changelog workflow, so the commit message is the changelog entry.

- Every commit message follows the Commitizen conventional commit format: `type(scope): summary`. Use only the types the changelog lists: `feat`, `fix`, `refactor`, `perf`, `docs`, `ci`, `chore`.
- Write a detailed body that says what was done and what changed. Write each change as its own paragraph in the same `type(scope): message` format, separated by blank lines. Commitizen turns each one into its own changelog line; prose paragraphs are dropped.
- Name the specific files, settings, or behavior that changed, not just the area.

```
ci: bump github actions off node 20

ci(actions): bump actions/checkout and actions/setup-node to v7 and pnpm/action-setup to v6

ci(changelog): pin astral-sh/setup-uv to v10.3.3, since it no longer publishes major tags
```

## Code quality skills

- When writing or editing code, load the `antislop`, `antislop-code`, `antislop-human`, and `modern-web-guidance` skills before you start, and follow their rules throughout development.
- When writing or editing code, also load the Vercel Labs skills (from `vercel-labs/agent-skills`): `vercel-react-best-practices`, `vercel-composition-patterns`, `vercel-react-view-transitions`, and `web-design-guidelines`. Follow their rules throughout development.
- When building or editing components, also load the `building-components` skill and follow its rules.
- Before writing or changing React or Next.js code, load the `context7-mcp` skill and check the current docs. For Next.js, also read the version-matched guide in `node_modules/next/dist/docs/`.
- After editing app code, run the `next-dev-loop` skill against a running `pnpm dev` to confirm the change works in the browser, not just that it compiles. It needs the `agent-browser` CLI (`npm i -g agent-browser@latest`).
- After completing any feature or bug fix, always run the review below before reporting the work as done.
- When a bug is found (reported, failing check, or surfaced by review), run the `mattpocock-skills:diagnosing-bugs` skill before changing code.
- When reviewing code (a diff, a PR, or a file), load the same four skills and apply their rules as review criteria.
- When reviewing code, also run the `caveman:caveman-review`, `ponytail:ponytail-review`, `fallow-review`, `code-review`, and `code-simplification` skills.
- Show the user every finding from these review skills, merged into one list. Label every finding with the skill that found it, e.g. `[ponytail-review]`. When two or more skills report the same issue (same location and same problem), show it once with every reporting skill in the label, e.g. `[caveman-review, ponytail-review]`.

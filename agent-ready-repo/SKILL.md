---
name: agent-ready-repo
description: Make a JavaScript or TypeScript web repository (Next.js first) agent-ready with pinned versions, one verify command, unit and browser tests, AGENTS.md rules, and CI that runs the same gate.
---

# Agent-Ready Repo

Give a repository one command that proves a change works, so coding agents, humans, and CI all agree on what "done" means.

Use this skill when the user asks to make a repository agent-ready, set up verification or CI for agents, add a `verify` script, or write project rules for coding agents. The full reasoning and a tested walkthrough live at https://bishoy.io/posts/agent-ready-nextjs-repository.

## Scope

- Fill gaps; never replace what the repository already has. Existing test runners, formatters, linters, CI providers, and package managers win.
- Add wiring and one proving test per test type. Do not add product features, pages, components, databases, auth, or a design system.
- Do not add Git hooks unless the user asks.
- Ask before deleting files, changing the package manager, or rewriting an existing CI workflow.

## 1. Inspect first

Read before changing anything and report a short gap list:

- `package.json`: scripts, `engines`, `packageManager`, test and format tools.
- Lockfile, which decides the package manager (`package-lock.json` npm, `pnpm-lock.yaml` pnpm, `yarn.lock` Yarn, `bun.lock` Bun). Use that manager for every command below.
- `.nvmrc` or `.node-version`.
- `AGENTS.md`, `CLAUDE.md`, and other agent entry points.
- Existing tests, their configs, and `.github/workflows/`.
- For Next.js, the installed version and `node_modules/next/dist/docs/` when present. Read the relevant guide there before writing Next.js code.

## 2. Pin the runtime

- Add `.nvmrc` with the Node.js major the project uses (default `22`) if no version file exists.
- Set `engines.node` if missing, matching what the framework requires.
- For pnpm or Yarn, set `packageManager` to the installed version so Corepack matches.

## 3. Add missing tools

Add only what is missing, as dev dependencies:

- Unit tests: Vitest (keep Jest if already present).
- Browser tests: Playwright (keep Cypress if already present).
- Formatting: Prettier (keep Biome or dprint if already present).
- TypeScript type checks: `tsc --noEmit`.

Known failure: fresh `create-next-app` projects pin `@types/node@^20`, and current Vitest needs `^20.19.0 || >=22.12.0` through Vite, so install fails with `ERESOLVE`. Fix it by installing `@types/node@22` alongside. Never use `--force` or `--legacy-peer-deps` to get past a peer conflict.

## 4. Define one verify command

Add focused scripts that are missing (`typecheck`, `test`, `test:e2e`, `format`, `format:check`) and one `verify` script that chains them from cheapest to most expensive:

```text
format:check && lint && typecheck && test && build && test:e2e
```

Drop steps the project genuinely lacks rather than adding placeholders. Add a `.prettierignore` covering build output, dependencies, lockfiles, and test reports, then run the formatter once so the baseline is clean.

## 5. Prove the wiring with one real test of each kind

- A unit test for an existing pure function, or a tiny utility with its test if none exists.
- A browser test that loads the home page and asserts a visible landmark (such as `main`).
- Configure the unit runner to exclude the browser test folder. Configure Playwright's `webServer` to start the production build and set `reuseExistingServer: !process.env.CI`.

## 6. Write project rules in AGENTS.md

Keep any framework-generated block (Next.js writes one between `BEGIN:nextjs-agent-rules` and `END:nextjs-agent-rules`; `next dev` re-adds it). Append, in this order and in under 40 lines:

1. **Project structure:** one line per top-level source folder, from what actually exists.
2. **Done means verified:** run `verify` before calling a task finished and report the result; use focused scripts while iterating.
3. **Rules:** smallest change that meets the requirement; add or update a test for every behavior change; never commit `.env*` files or print secrets; ask before destructive changes, production data changes, or external messages.

Make sure `CLAUDE.md` exists and points to `AGENTS.md` (`@AGENTS.md`) when the user uses Claude Code. Add other agents' entry points only when the user names those agents.

## 7. Run the same gate in CI

If the repository is on GitHub and has no workflow running the checks, add `.github/workflows/verify.yml`: check out, set up Node from the version file with the package manager's cache, install from the lockfile, install Playwright's browser with system dependencies when browser tests exist, then run `verify`. CI calls `verify` rather than repeating its steps. If a workflow already exists, propose the change instead of rewriting it.

## 8. Verify and report

Run `verify` yourself. Fix failures caused by your changes; if a failure predates them, say so and do not hide it. If the environment cannot run a step (for example, no browser can be installed), say exactly which step was skipped and why.

Finish with a short report: what was already in place, what you added, the `verify` result per step, and anything left for the user.

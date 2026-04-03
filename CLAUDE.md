# CLAUDE.md — B.R.A.N.D.I.

> Guidelines for AI assistants working in this repository.

## Project Overview

**B.R.A.N.D.I.** is an early-stage project owned by **Fluid-Kiss-Consultations**. The repository is currently in its initial skeleton phase — no application code, dependencies, or build tooling have been added yet.

- **License:** GNU Affero General Public License v3 (AGPL-3.0)
- **Primary branch:** `main`

## Repository Structure

```
B.R.A.N.D.I./
├── CLAUDE.md        # This file — AI assistant guidelines
├── README.md        # Project description (placeholder)
├── LICENSE          # AGPL-3.0 license
└── .gitignore       # Git ignore rules (to be populated)
```

## Development Workflow

### Branching

- The default branch is `main`.
- Feature branches should follow the pattern: `<author>/<short-description>`.

### Commits

- Write clear, descriptive commit messages.
- Keep commits focused — one logical change per commit.

### Code Style

No linters or formatters are configured yet. When they are added, update this section with:
- The linting/formatting tools in use
- How to run them (e.g., `npm run lint`, `make fmt`)
- Any pre-commit hooks

### NatSpec & Documentation Standards

All code must include **NatSpec-style documentation from the first line written** — this is a day-one requirement, not a retrofit. Follow enterprise best practices:

- **Solidity / Smart Contracts:** Full NatSpec (`@title`, `@author`, `@notice`, `@dev`, `@param`, `@return`, `@inheritdoc`) on every contract, interface, library, function, event, error, and state variable.
- **Non-Solidity Code:** Apply the NatSpec philosophy — use the equivalent idiomatic doc format (JSDoc, Python docstrings, Rustdoc, etc.) with the same thoroughness: purpose, params, returns, side effects, and developer notes.
- **Interfaces & ABIs:** Document the *intent* of each function, not just its signature. Consumers of the interface should understand behaviour from docs alone.
- **Events & Errors:** Every event and custom error gets a `@notice` explaining *when* and *why* it is emitted/thrown.
- **File Headers:** Every source file begins with a header block: SPDX license identifier, title, author, and a brief description of the file's role in the system.

> **Rule of thumb:** If a reviewer cannot understand a function's purpose, parameters, return values, and failure modes from its documentation alone — it is under-documented.

### Testing

**Iterative testing is enforced via CI workflows, phased to match project maturity:**

| Phase | Trigger | Scope | CI Requirement |
|-------|---------|-------|----------------|
| **Phase 0 — Skeleton** | Every push | Repo hygiene (lint CLAUDE.md, license presence) | Advisory only |
| **Phase 1 — Contracts / Core** | Every push + PR | Unit tests, NatSpec completeness checks | Must pass to merge |
| **Phase 2 — Integration** | PR to `main` | Integration & cross-module tests | Must pass to merge |
| **Phase 3 — Staging** | Release tags | Full regression, gas/performance benchmarks, coverage ≥ threshold | Must pass to release |

When adding a test framework, update the table above with concrete commands and thresholds. Each phase's CI workflow lives in `.github/workflows/` and should be named `ci-phase-<N>.yml`.

**Local tooling note:** Forge / Foundry is **not installed locally**. All Solidity compilation, testing, and deployment tooling must run via **CI workflows only** (GitHub Actions) until a local toolchain is explicitly set up. AI assistants must not assume `forge`, `cast`, or `anvil` are available on the development machine.

No test framework is configured yet. When one is added, update this section with:
- The test framework and runner
- How to run tests locally (if applicable) or via CI
- Test file naming conventions and locations
- Coverage threshold targets

### Building & Running

No build system is configured yet. **Forge / Foundry is not available locally** — do not rely on local Solidity tooling. When a build system is added, update this section with:
- How to install dependencies
- How to build the project (locally or CI-only)
- How to run the project locally

## Conventions for AI Assistants

1. **Read before writing.** Always read existing files before modifying them.
2. **Respect the license.** All contributions fall under AGPL-3.0. Do not introduce incompatibly-licensed dependencies without flagging it.
3. **Keep this file current.** When you add tooling, dependencies, or architectural patterns, update the relevant sections of this CLAUDE.md.
4. **Do not create unnecessary files.** Prefer editing existing files over creating new ones.
5. **Ask when uncertain.** If a task is ambiguous or could have significant architectural impact, ask the user before proceeding.
6. **No secrets in code.** Never commit API keys, tokens, passwords, or other credentials. Use environment variables or secret management tools.
7. **NatSpec from line one.** Every piece of code — contracts, scripts, tests, utilities — must carry full NatSpec-level documentation before it is committed. No exceptions, no "add docs later" TODOs.
8. **Test every phase.** When adding or modifying code, ensure corresponding tests exist and that the relevant CI phase workflow passes before pushing.
9. **Session wind-down protocol.** When context usage approaches **~15% remaining**, alert the user and — with their approval — perform the following end-of-session routine:
   1. **Update CLAUDE.md** — Revise this file to reflect any new tooling, architectural decisions, conventions, or project state changes made during the session.
   2. **Commit & push** — Commit the updated CLAUDE.md (and any other pending work) and push to the current working branch.
   3. **Output a continuation prompt** — Print a fenced, copy-pasteable prompt block that the user can feed to the next Claude instance. The prompt must include:
      - The repository name, current branch, and last commit SHA.
      - A concise summary of work completed this session.
      - Explicit next steps / open tasks remaining.
      - Any decisions or context the next instance needs to know.
      - A reminder to read CLAUDE.md first.

   **Continuation prompt format:**
   ````
   ```
   Repository: Fluid-Kiss-Consultations/B.R.A.N.D.I.
   Branch: <current-branch>
   Last commit: <short-sha> — <commit-message>

   ## Session Summary
   <bulleted list of what was accomplished>

   ## Next Steps
   <bulleted list of remaining work>

   ## Key Context
   <any decisions, blockers, or open questions>

   ---
   Start by reading CLAUDE.md, then continue with the next steps above.
   ```
   ````

## Environment Setup

### Local Toolchain Constraints

| Tool | Available Locally? | Notes |
|------|--------------------|-------|
| Forge / Foundry (`forge`, `cast`, `anvil`) | **No** | All Solidity compilation and testing runs in CI only |
| Node.js / npm | TBD | Update when configured |
| Python | TBD | Update when configured |

> **AI assistants:** Do not generate commands that assume Forge, Foundry, or any Solidity toolchain is installed locally. Write code and tests that can be validated via CI (GitHub Actions).

When further environment configuration is added, document:
- Required environment variables
- `.env` file setup
- External service dependencies

## CI/CD

CI is structured in **phases** that scale with project maturity (see [Testing](#testing) for the phase table). Workflow files follow the naming convention `.github/workflows/ci-phase-<N>.yml`.

No CI/CD pipelines are configured yet. When adding the first workflow:
- Start with **Phase 0** (repo hygiene / advisory checks)
- Each subsequent phase is additive — later phases include all prior checks
- Document required secrets/variables per workflow
- Link check status badges in README.md

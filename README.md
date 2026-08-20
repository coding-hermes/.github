# coding-hermes

**Autonomous coding fleet — foreman → worker → guard pipeline, with shared
memory and shared specs.**

The `coding-hermes` org operates a fleet of coding agents that scan,
plan, and ship code across multiple projects. Each project runs an
independent foreman tick; shared infrastructure (DuckBrain, scheduler
config, the skills library) keeps the fleet coordinated.

---

## 🚀 Main project

**[eduos](https://github.com/coding-hermes/eduos)** — EduOS, the
Colombian K-12 education platform. The current "north star" of the
fleet: 95 specs, 562K words, alpha-stage. Everything else exists to
make Eduos ship.

## 🧠 Infrastructure

| Repo | Role |
|------|------|
| [scheduler](https://github.com/coding-hermes/scheduler) | Weight-budget priority scheduler daemon (Go). Picks what to run, when, and on what model. |
| [duckbrain](https://github.com/coding-hermes/duckbrain) | Fleet persistent memory & semantic search. The shared `/fleet/*` namespace. |
| [skills](https://github.com/coding-hermes/skills) | Skill library every foreman loads — broth, star, cron-schema, config. |
| [hermes-canopy](https://github.com/coding-hermes/hermes-canopy) | Canopy OS — graph-native agent collaboration surface. |
| [h3](https://github.com/coding-hermes/h3) | H3 (purpose TBD — see repo README). |

---

## 🤖 How the fleet works

```
                    ┌──────────────┐
   operator (Bane)  │   Dexdat     │  (you set priorities, pin decisions)
   ────────────────▶│  Telegram /  │
                    │   cron jobs  │
                    └──────┬───────┘
                           │
                           ▼
                    ┌──────────────┐
                    │  schedulerd  │  (coding-hermes/scheduler)
                    │  :9090 HTTP  │
                    └──┬──────┬────┘
            tick per     │      │  tick per
            project      │      │  project
                       ▼        ▼
                    ┌──────────────┐
                    │   foreman    │  (one per project)
                    │   agent      │
                    └──┬───────┬───┘
                       │       │
            spawn      │       │  read/write
            worker     │       │  context
                       ▼       ▼
                    ┌──────────────┐
                    │   worker     │  (independent chat session)
                    │   agent      │
                    └──────┬───────┘
                           │ commit
                           ▼
                    ┌──────────────┐
                    │  GitReins    │  (secrets / build / lint / tests)
                    └──────┬───────┘
                           │ report
                           ▼
                    ┌──────────────┐
                    │  DuckBrain   │  (coding-hermes/duckbrain)
                    │  /fleet/*    │
                    └──────────────┘
```

1. **Scheduler** reads `fleet.toml` and decides what to wake up.
2. **Foreman** boots cold, reads DuckBrain for context, scans the
   project board, picks a task.
3. **Worker** runs in an independent session — foreman doesn't inherit
   its provider/model.
4. **GitReins** guards the commit (secrets, build, lint, tests).
5. **DuckBrain** records the outcome so next tick can pick up.

---

## 📁 Conventions

- **Default branch:** `main` on every repo.
- **Commits:** Conventional Commits (`feat:`, `fix:`, `docs:`, `chore:`).
- **PRs:** squash-merge with a clear title; reference the issue.
- **Specs:** implementation-ready specs live in `specs/NNN-*.md`
  (Eduos has 95 of these).
- **Memory:** cross-project context lives in DuckBrain's `/fleet/*`
  namespace — never in a single repo.

## 🔒 Members

- **@totalwindupflightsystems** — owner (operator, Bane).
- **coding-hermes-bot** — service identity (created by hermes).

## 📜 License

Per-repo — see each repo's `LICENSE`. Default for new infra: MIT.

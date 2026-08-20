# coding-hermes

Autonomous coding fleet — foreman → worker → guard pipeline, with shared
memory and shared specs.

The `coding-hermes` org operates a fleet of coding agents that scan,
plan, and ship code across multiple projects. Each project runs an
independent foreman tick; shared infrastructure (DuckBrain, scheduler
config, the skills library) keeps the fleet coordinated.

## 🚀 Main project

**[eduos](https://github.com/coding-hermes/eduos)** — EduOS, the
Colombian K-12 education platform. The current "north star" of the
fleet: 95 specs, 562K words, alpha-stage. Everything else exists to
make Eduos ship.

## 🧠 Infrastructure

| Repo | Role |
|------|------|
| [scheduler](https://github.com/coding-hermes/scheduler) | Weight-budget priority scheduler daemon (Go). |
| [duckbrain](https://github.com/coding-hermes/duckbrain) | Fleet persistent memory & semantic search. |
| [skills](https://github.com/coding-hermes/skills) | Skill library every foreman loads. |
| [hermes-canopy](https://github.com/coding-hermes/hermes-canopy) | Canopy OS — graph-native agent collaboration surface. |
| [h3](https://github.com/coding-hermes/h3) | H3 (purpose TBD — see repo README). |

## How the fleet works

Foreman → worker → GitReins guard → DuckBrain memory. See
[README.md](README.md) for the full diagram.

## Membership

- **@totalwindupflightsystems** — owner (operator, Bane).
- **coding-hermes-bot** — service identity (created by hermes).

## License

Per-repo — see each repo's `LICENSE`. Default for new infra: MIT.

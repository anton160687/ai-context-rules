# AI Context Management — rules for a codebase that AI agents can work with cheaply

> 🇷🇺 [Читать на русском](./README.ru.md)

A set of practices for organizing the "interface" between a codebase and AI coding agents (Copilot, Cursor, Claude Code, etc.).

The goal is not "to make AI read less code", but **to help it understand as early as possible which code it doesn't need to read at all**.

Based on the article [«Как устроить кодовую базу, с которой AI‑агентам дешевле работать»](https://habr.com/ru/articles/1093078/) (in Russian) and extended with battle-tested patterns.

## The 13 patterns

1. **Minimize Permanent Context** — the root `AGENTS.md` is a project map, not an encyclopedia.
2. **Progressive Context** — load knowledge in layers: project → domain → task → example → code.
3. **Reduce Exploration** — project map, golden examples, search-before-read.
4. **Reuse Knowledge** — turn expensive discoveries into reusable knowledge.
5. **Keep Context Clean** — remove stale and irrelevant information.
6. **Single Source of Truth** — one canonical place per rule; everywhere else just links.
7. **Show, Don't Tell** — conventions with `✅/❌` snippets.
8. **Explicit Negative Constraints** — explicit "do not" lists.
9. **Metadata & Freshness** — scope + last-verified date on every file.
10. **Predictable Layout & Naming** — knowledge is discoverable by naming conventions.
11. **Bounded Search Scope** — search by glob; never read vendor/secrets/artifacts.
12. **Cheap Entry Points** — ready-made, verified commands.
13. **Definition of Done** — a completion checklist in every instruction.

## Contents

- [`ai-context-management-universal.md`](./ai-context-management-universal.md) — universal template with checklists. Copy it into any project and fill in the specifics.
- [`examples/weddingportal-ai-context-management.mdc`](./examples/weddingportal-ai-context-management.mdc) — a filled-in example (Django project) showing the template "in the wild".

## How to use

1. Copy `ai-context-management-universal.md` into the project root or `.cursor/docs/`.
2. Fill in the sections: project map, golden examples, command list.
3. Link to the file from the root `AGENTS.md` / `CLAUDE.md`.
4. Create a short project-level rule (`.cursor/rules/*.mdc` or a Copilot instruction) mapping the patterns to your code — see `examples/`.

## License

MIT — use freely.

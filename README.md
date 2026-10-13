# AI Context Management — правила для кодовой базы, с которой AI‑агентам дешевле работать
# AI Context Management — rules for a codebase that AI agents can work with cheaply

> English below · Русский ниже

---

## English

A set of practices for organizing the "interface" between a codebase and AI coding agents (Copilot, Cursor, Claude Code, etc.).

The goal is not "to make AI read less code", but **to help it understand as early as possible which code it doesn't need to read at all**.

Based on the article [«Как устроить кодовую базу, с которой AI‑агентам дешевле работать»](https://habr.com/ru/articles/1093078/) (in Russian) and extended with battle-tested patterns.

### The 13 patterns

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

### Contents

- [`ai-context-management-universal.md`](./ai-context-management-universal.md) — universal template with checklists. Copy it into any project and fill in the specifics.
- [`examples/weddingportal-ai-context-management.mdc`](./examples/weddingportal-ai-context-management.mdc) — a filled-in example (Django project) showing the template "in the wild".

### How to use

1. Copy `ai-context-management-universal.md` into the project root or `.cursor/docs/`.
2. Fill in the sections: project map, golden examples, command list.
3. Link to the file from the root `AGENTS.md` / `CLAUDE.md`.
4. Create a short project-level rule (`.cursor/rules/*.mdc` or a Copilot instruction) mapping the patterns to your code — see `examples/`.

### License

MIT — use freely.

---

## Русский

Набор практик для организации «интерфейса» между кодовой базой и AI‑агентом (Copilot, Cursor, Claude Code и др.).

Цель — не «заставить AI читать меньше кода», а **помочь ему как можно раньше понять, какой код читать вообще не нужно**.

Основано на статье [«Как устроить кодовую базу, с которой AI‑агентам дешевле работать»](https://habr.com/ru/articles/1093078/) и дополнено проверенными на практике паттернами.

### 13 паттернов

1. **Minimize Permanent Context** — корневой `AGENTS.md` = карта проекта, не энциклопедия.
2. **Progressive Context** — знания слоями: проект → домен → задача → эталон → код.
3. **Reduce Exploration** — карта проекта, golden examples, search-before-read.
4. **Reuse Knowledge** — дорогое исследование сохранять как переиспользуемое знание.
5. **Keep Context Clean** — устаревшее и нерелевантное удалять.
6. **Single Source of Truth** — правило в одном каноническом месте, остальные — ссылки.
7. **Show, Don't Tell** — конвенции с `✅/❌` примерами.
8. **Explicit Negative Constraints** — явные «Запрещено».
9. **Metadata & Freshness** — область действия + дата проверки у каждого файла.
10. **Predictable Layout & Naming** — знания находятся по конвенции имён.
11. **Bounded Search Scope** — поиск по glob, не читать vendor/secrets/артефакты.
12. **Cheap Entry Points** — готовые проверенные команды.
13. **Definition of Done** — чеклист завершения в каждой инструкции.

### Содержимое

- [`ai-context-management-universal.md`](./ai-context-management-universal.md) — универсальный шаблон с чеклистами. Копируйте в любой проект и заполняйте конкретикой.
- [`examples/weddingportal-ai-context-management.mdc`](./examples/weddingportal-ai-context-management.mdc) — заполненный пример (Django‑проект), как шаблон выглядит «в бою».

### Как использовать

1. Скопируйте `ai-context-management-universal.md` в корень проекта или в `.cursor/docs/`.
2. Заполните секции: карта проекта, golden examples, список команд.
3. Добавьте ссылку на файл из корневого `AGENTS.md` / `CLAUDE.md`.
4. Заведите короткое правило уровня проекта (`.cursor/rules/*.mdc` или инструкция для Copilot) с привязкой паттернов к вашему коду — по образцу из `examples/`.

### Лицензия

MIT — используйте свободно.

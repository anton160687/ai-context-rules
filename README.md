# AI Context Management — правила для кодовой базы, с которой AI‑агентам дешевле работать

Набор практик для организации «интерфейса» между кодовой базой и AI‑агентом (Copilot, Cursor, Claude Code и др.).
Цель — не «заставить AI читать меньше кода», а **помочь ему как можно раньше понять, какой код читать вообще не нужно**.

Основано на статье [«Как устроить кодовую базу, с которой AI‑агентам дешевле работать»](https://habr.com/ru/articles/1093078/) и дополнено проверенными на практике паттернами.

## 13 паттернов

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

## Содержимое

- [`ai-context-management-universal.md`](./ai-context-management-universal.md) — универсальный шаблон с чеклистами. Копируйте в любой проект и заполняйте конкретикой.
- [`examples/weddingportal-ai-context-management.mdc`](./examples/weddingportal-ai-context-management.mdc) — заполненный пример (Django‑проект), как шаблон выглядит «в бою».

## Как использовать

1. Скопируйте `ai-context-management-universal.md` в корень проекта или в `.cursor/docs/`.
2. Заполните секции: карта проекта, golden examples, список команд.
3. Добавьте ссылку на файл из корневого `AGENTS.md` / `CLAUDE.md`.
4. Заведите короткое правило уровня проекта (`.cursor/rules/*.mdc` или инструкция для Copilot) с привязкой паттернов к вашему коду — по образцу из `examples/`.

## Лицензия

MIT — используйте свободно.

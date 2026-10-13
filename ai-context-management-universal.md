# Управление AI-контекстом: универсальные правила (шаблон) / AI Context Management: Universal Rules (Template)

> **EN — Purpose:** a copy-paste template for any new or existing project. Copy it into the project root (or `.cursor/docs/`) and fill the sections with your repository's specifics.
> **EN — Main principle:** don't "make AI read less code" — help it understand as early as possible which code it doesn't need to read at all.

> **RU — Назначение:** шаблон для любого проекта — нового или существующего. Копируйте в корень проекта (или в `.cursor/docs/`) и заполняйте секции конкретикой своего репозитория.
> **RU — Главный принцип:** не «заставить AI читать меньше кода», а «помочь ему как можно раньше понять, какой код читать вообще не нужно».

> Источник / Source: [Хабр — «Как устроить кодовую базу, с которой AI‑агентам дешевле работать»](https://habr.com/ru/articles/1093078/).

Паттерны оптимизируют не сам код, а **путь AI-агента от задачи до минимально достаточного контекста**. Они не зависят от языка, фреймворка или AI-инструмента. Первые пять — из статьи; 6–13 — дополняющие практики, проверенные на реальных проектах.

---

## 1. Minimize Permanent Context — минимизируйте постоянный контекст

Корневой `AGENTS.md` / `CLAUDE.md` — **карта проекта**, а не энциклопедия.

- В постоянный контекст — только то, что нужно **большинству** задач.
- Детальные инструкции — в отдельных файлах, загружаются по необходимости.

Пример карты (вместо пересказа всего проекта):

```
Project structure:
- apps/ — user-facing applications
- services/ — backend services
- packages/ — shared packages
- infrastructure/ — deployment and infrastructure
- docs/ — architecture documentation

Common commands:
- make test
- make lint
- make build

Detailed instructions:
- Auth: services/auth/AGENTS.md
- Billing: services/billing/AGENTS.md
- Database: packages/database/AGENTS.md
- Infrastructure: infrastructure/AGENTS.md
```

Корневой файл отвечает на вопрос **«где находится нужная информация?»**, а не **«что вообще можно рассказать об этом проекте?»**.

Чеклист:
- [ ] Корневой файл — карта, не энциклопедия.
- [ ] Нет дублирования деталей, уже описанных в дочерних файлах.

---

## 2. Progressive Context — загружайте знания слоями

Слои:

```
Global context
      ↓
Domain / Service context
      ↓
Task-specific instructions
      ↓
Reference implementation
      ↓
Relevant source code
```

Путь для задачи «добавить новый payment provider»:

```
AGENTS.md
   ↓
services/payments/AGENTS.md
   ↓
skills/add-payment-provider.md
   ↓
existing provider implementation
   ↓
relevant source files
```

Повторяемые сценарии выносите в отдельные task-инструкции / skills:

```
create-api-endpoint
add-database-migration
add-message-consumer
create-background-job
add-ui-page
add-integration-test
create-service
```

Принцип: **Don't load everything upfront. Load context when it becomes relevant** — lazy loading, применённый к знаниям о проекте.

Чеклист:
- [ ] У каждого домена/сервиса — свой файл инструкций.
- [ ] Повторяемые сценарии вынесены в отдельные task-инструкции.

---

## 3. Reduce Exploration — помогите агенту меньше исследовать репозиторий

### Project Map

Короткая карта точек входа:

```
HTTP handlers: services/*/handlers/
Business logic: services/*/domain/
Database access: services/*/repositories/
Message consumers: services/*/consumers/
Shared schemas: packages/schemas/
Infrastructure: infrastructure/
```

### Golden Examples

Одна эталонная реализация показывает структуру директорий, naming, обработку ошибок, logging/validation, взаимодействие со слоями и подход к тестам:

```
REST endpoint: services/users/handlers/create-user/
Database migration: database/migrations/20261001_add_orders.sql
Background job: workers/email/send-notification/
UI feature: apps/web/features/profile/
Infrastructure service: infrastructure/services/example/
```

Правило: для каждого частого типа задачи — один golden example. Это компактнее многостраничной документации и снижает риск, что AI изобретёт новый pattern там, где уже есть принятый.

### Search Before Read

```
find → narrow → read
```

вместо

```
read → read → read → understand → find
```

Перед чтением большого файла — `rg`, `git grep`, symbol search / LSP / AST. Если нужен один метод из файла на 2000 строк, не помещайте в контекст весь файл.

### Git как источник контекста

Для review и существующих изменений: `git status`, `git diff --stat`, `git diff`, `git log`, `git show`.

Чеклист:
- [ ] Есть карта из 5–10 строк.
- [ ] На каждый частый тип задачи есть golden example.

---

## 4. Reuse Knowledge — не платите дважды за одно исследование

Дорогое исследование → структурированное знание → дешёвое извлечение в будущем.

Форматы сохранения: `AGENTS.md`, инструкция сервиса, документация, task-инструкция/skill, architecture note, golden example, code comment.

Пример `docs/payment-flow.md`:

```
Payment flow:
order-service → billing-service → payment provider → PaymentCompleted event → message broker → order-service consumer

Relevant files:
- services/billing/payments/
- services/billing/events/
- services/orders/consumers/payment-completed/

Reference implementation:
- services/billing/providers/example/
```

Размещение по области действия знания:
- важно почти для любой задачи → глобальный контекст;
- для одного сервиса → рядом с этим сервисом;
- повторяемый workflow → task-инструкция;
- код уже показывает подход → просто ссылка на него.

Чеклист:
- [ ] После задачи с глубоким исследованием — короткая заметка/flow-doc и ссылка на неё.

---

## 5. Keep Context Clean — регулярно очищайте контекст

Накопленные без чистки знания дают **context pollution**:

```
stale context → wrong assumptions → unnecessary exploration → wrong changes
```

Периодически удалять/выносить:
- завершённые migration instructions и временные workaround;
- deprecated архитектуру и старые команды;
- битые ссылки на несуществующие директории;
- устаревшие reference implementations;
- дублирующиеся инструкции.

Проверка блока документации:

> **Will this information change how the agent should perform a task today?**
> Если «нет» — убрать из рабочего контекста.

Различие формулировок:
- Документация для **человека** отвечает «почему система стала такой?».
- Рабочая документация для **AI** отвечает «как система устроена сейчас и как её правильно менять?».

Чеклист:
- [ ] Периодический ревью инструкций (в рамках обычных задач).
- [ ] Исторические подробности — вне рабочего AI-контекста.

---

## 6. Single Source of Truth — DRY для знаний

Каждое правило живёт ровно в **одном каноническом месте**; во всех остальных файлах — только ссылка, не копия.

- Одна и та же конвенция в трёх файлах рано или поздно рассинхронизируется.
- Пример: каноничные практики — в `PROJECT_PRACTICES.md`, а в кратких rules — только указатель на него и на конкретный раздел.

Чеклист:
- [ ] Каждое правило описано один раз; остальные файлы ссылаются, а не дублируют.

## 7. Show, Don't Tell — примеры вместо прозы

Каждая нетривиальная конвенция сопровождается коротким фрагментом `✅ / ❌`, а не абзацем описания.

- Golden example (паттерн 3) — частный случай; здесь шире: даже простые правила (именование, стиль, порядок импортов) нагляднее парой строк кода.
- Код-пример компактнее и однозначнее, чем «постарайтесь писать аккуратно».

Чеклист:
- [ ] Нетривиальные правила сопровождаются примером.

## 8. Explicit Negative Constraints — явные «Запрещено»

Компактный список «нельзя» точнее и дешевле исчерпывающего перечисления «как надо».

- Пример: «Запрещено: God models; `except Exception: pass`; бизнес-логика в шаблонах».
- Запреты отсекают целые классы ошибок меньшим числом токенов, чем перечисление разрешённого.

Чеклист:
- [ ] В инструкциях есть явные запреты, а не только рекомендации.

## 9. Metadata & Freshness — метаданные и актуальность

У каждого файла инструкций — метаданные: назначение, область действия и дата последней проверки.

- В frontmatter: `description`, область действия (`globs` / `applyTo`), `last_verified`.
- Блоки, которые гарантированно устареют (миграции, workaround), помечать датой и триггером удаления.

Чеклист:
- [ ] У каждого файла инструкций есть область действия и дата последней проверки.
- [ ] Временное помечено триггером удаления.

## 10. Predictable Layout & Naming — предсказуемая структура знаний

Знания лежат в известной папке с конвенциональными именами (`*-architecture.md`, `*-rules.mdc`, `*-models-spec.md`), поэтому находятся по конвенции даже без полной карты.

- Агент может угадать имя файла по типу задачи, не читая карту целиком.

Чеклист:
- [ ] Папка для инструкций одна и известна.
- [ ] Имена файлов следуют единому паттерну.

## 11. Bounded Search Scope — ограниченная область поиска

Агент должен искать и читать только в нужных директориях; сгенерированное, vendor и секреты — **вне его поля зрения**.

- Поиск с file glob / конкретной директорией, не по всему репозиторию.
- Исключать через `.gitignore`, `.cursorignore`, `.dockerignore`: `node_modules/`, build-артефакты, `media/`, дампы БД, ключи/`.env`.

Чеклист:
- [ ] Секреты и артефакты исключены из индекса и контекста.
- [ ] Поиск всегда сужается до конкретной области.

## 12. Cheap Entry Points — дешёвые точки входа (recipes)

Короткий **проверенный** список команд (test / lint / build / run / migrate), чтобы агент не восстанавливал их каждый раз с нуля.

Чеклист:
- [ ] В карте проекта есть блок «Common commands».
- [ ] Команды актуальны и проверены.

## 13. Definition of Done — чеклист завершения

Каждая доменная инструкция заканчивается явным чеклистом проверок: тесты зелёные, секретов нет, миграции сгенерированы, права проверены.

- Дешёвый способ гарантировать, что агент не остановился на «вроде работает».

Чеклист:
- [ ] В каждой инструкции/rule есть секция «Перед завершением задачи».

---

## Итоговая последовательность

```
                 AI coding task
                       │
                       ▼
              Minimal global context
                       │
                       ▼
             Relevant domain/service
                       │
                       ▼
             Task-specific context
                       │
                       ▼
              Golden example
                       │
                       ▼
              Targeted search
                       │
                       ▼
              Minimum source code
                       │
                       ▼
                    Change
                       │
                       ▼
              Valuable discovery?
                  │         │
                 No        Yes
                            │
                            ▼
                   Save as reusable
                       knowledge
```

## Шпаргалка: все правила

1. **Minimize Permanent Context** — в постоянных инструкциях только то, что нужно большинству задач.
2. **Progressive Context** — слои: проект → домен/сервис → задача → эталон → исходники.
3. **Reduce Exploration** — карта проекта, точки входа, golden examples.
4. **Reuse Knowledge** — дорогое исследование сохранять как переиспользуемое знание.
5. **Keep Context Clean** — устаревшее и нерелевантное удалять, иначе знание само становится источником токенов и ошибок.
6. **Single Source of Truth** — правило в одном каноническом месте, остальные — ссылки.
7. **Show, Don't Tell** — конвенции с `✅/❌` примерами.
8. **Explicit Negative Constraints** — явные «Запрещено».
9. **Metadata & Freshness** — область действия + дата проверки у каждого файла.
10. **Predictable Layout & Naming** — знания находятся по конвенции имён.
11. **Bounded Search Scope** — поиск по glob, не читать vendor/secrets/артефакты.
12. **Cheap Entry Points** — готовые проверенные команды.
13. **Definition of Done** — чеклист завершения в каждой инструкции.

# Project Template

Шаблон процессной архитектуры для dev-проектов, где продуктовые и инженерные знания живут в Markdown, а backlog, задачи и статусы живут в Linear. Кодовый стек добавляется при инициализации проекта.

## Структура

```text
project/
├── .agents/
│   └── skills/               # скиллы проекта + симлинки dev-скиллов (sync-dev-skills.sh)
├── .claude/
│   └── skills                # → ../.agents/skills
├── docs/
│   ├── prd.md                # продуктовые требования: проблема, сценарии, границы
│   ├── roadmap.md            # очередь будущих этапов
│   ├── architecture.md       # архитектура as-is и ключевые решения
│   ├── open-questions.md     # вопросы без ответа и триггеры действий
│   ├── epics/                # scope эпиков; статус — в Linear Milestone
│   │   └── done/             # завершённые эпики с итогом
│   ├── decisions/            # действующие решения направлений и реестры по аудитам
│   ├── audits/               # отчёты аудитов со статусом каждой находки
│   ├── notes/                # заметки, design-документы, исследования
│   ├── spikes/
│   │   └── results/          # результаты spike-задач из Linear
│   ├── plans/                # планы реализации, живут до закрытия задачи
│   └── reviews/              # ревью планов и кода, живут до закрытия задачи
├── src/                      # код проекта
├── tests/                    # тесты
├── scripts/                  # one-shot скрипты, probes, вспомогательные утилиты
├── data/                     # данные для разработки, не в git
├── .env.example              # шаблон переменных окружения; секреты — в .env
├── PROJECT_STATE.md          # briefing для новой сессии
├── CLAUDE.md                 # → ~/.agents/DEV-RULES.md (sync-dev-skills.sh)
└── AGENTS.md                 # → ~/.agents/DEV-RULES.md (sync-dev-skills.sh)
```

## Канонические источники

| Что | Где живёт |
|-----|-----------|
| Правила проекта для агентов | `AGENTS.md` и `CLAUDE.md` → `~/.agents/DEV-RULES.md` |
| Карта проекта для новой сессии | `PROJECT_STATE.md` |
| Продуктовые требования | `docs/prd.md` |
| Очередь будущих этапов | `docs/roadmap.md` |
| Архитектура as-is и устойчивые решения | `docs/architecture.md` |
| Действующие решения направлений | `docs/decisions/{направление}-decisions.md` |
| Открытые вопросы и гипотезы | `docs/open-questions.md` |
| Scope эпиков | `docs/epics/{NN}-{slug}.md`, завершённые — `docs/epics/done/` |
| Фичи, User Stories, Issues, Spikes, статусы и приоритеты | Linear |
| Результаты spike-задач | `docs/spikes/results/{ID}-{slug}.md` |
| Отчёты аудитов и решения по находкам | `docs/audits/` + `docs/decisions/{область}-audit-decisions.md` |
| Планы и ревью текущей задачи | `docs/plans/`, `docs/reviews/`; финализация задачи их удаляет |
| Рабочие заметки и исследования | `docs/notes/` |
| Секреты | `.env`, шаблон — `.env.example` |

Markdown фиксирует устойчивое знание. Linear фиксирует работу: что надо сделать, кто делает, в каком статусе задача и к какому milestone она относится.

## Workflow

Источник процесса — `~/.agents/DEV-RULES.md` и dev-скиллы. Таблица — карта шагов.

**Продукт и архитектура**

| Шаг | Skill | Артефакт |
|-----|-------|----------|
| PRD | `prd` | `docs/prd.md`, `docs/roadmap.md`, «Бизнес-контекст» в `PROJECT_STATE.md` |
| Архитектура | `architecture-design` | `docs/architecture.md`, «Архитектура» в `PROJECT_STATE.md` |
| Ревью архитектуры | `architecture-review` | `docs/reviews/YY-MM-DD-architecture-review.md` |
| Продуктовые решения | `product-session` | `docs/decisions/{направление}-decisions.md` |

**Бэклог**

| Шаг | Skill | Артефакт |
|-----|-------|----------|
| Scope эпика, по необходимости | `new-epic` | `docs/epics/{NN}-{slug}.md` + Linear Milestone |
| Фичи и черновики задач | `make-feature-backlog` | Linear: фичи с label `Feature`, черновики в Backlog |
| Spike | `run-spike` | `docs/spikes/results/{ID}-{slug}.md` + Spike Done |
| Готовность задачи | `make-stories` | черновик переходит из Backlog в Ready |

**Реализация**

| Шаг | Skill | Артефакт |
|-----|-------|----------|
| Задача целиком | `dev-task` | план, ревью, код, коммит, Linear Done |
| Очередь задач | `setup-autopilot` → `autopilot` | `docs/autopilot/{date}-decisions.md` |
| Параллельные ветки | `plan-worktrees` | git worktree на задачу |
| Доводка UI по макету | `frontend-fit` | реализация, выверенная по макету |

`dev-task` сам вызывает `write-plan`, `plan-review`, `impact-review` и `code-review`.

**Аудит и выпуск**

| Шаг | Skill | Артефакт |
|-----|-------|----------|
| Аудит кода | `run-codebase-audit` или `audit` | `docs/audits/` |
| Аудит безопасности | `security-audit` | `docs/audits/security-audit-{date}-run-{N}/` |
| Триаж находок | `triage-findings` | `Статус:` у находок, `docs/decisions/{область}-audit-decisions.md`, задачи в Linear |
| Готовность к выпуску | `ready-for-prod` | `docs/audits/ready-for-prod-{date}.md` |
| Завершение эпика | по DEV-RULES §3.5 | итог в эпике, перенос в `docs/epics/done/` |

**Поддержка**

| Шаг | Skill | Артефакт |
|-----|-------|----------|
| Бюджет карты проекта | `clean-project-state` | `PROJECT_STATE.md` в пределах 130 строк / 14KB |
| Актуальность документации | `update-docs` | список правок living-документов по коду |

## Симлинки

Claude и Codex берут правила и workflow-скиллы из `~/.agents/`:

| Путь | Назначение |
|------|------------|
| `CLAUDE.md` → `~/.agents/DEV-RULES.md` | Инженерные правила для Claude |
| `AGENTS.md` → `~/.agents/DEV-RULES.md` | Инженерные правила для Codex |
| `.agents/skills/` | Скиллы проекта и симлинки на `~/.agents/dev-skills/*`; Codex читает папку напрямую |
| `.claude/skills` → `../.agents/skills` | Те же скиллы для Claude |

Все симлинки ставит скрипт `~/.agents/sync-dev-skills.sh` и записывает их в `.git/info/exclude`. В git проекта попадают только скиллы самого проекта и ссылка `.claude/skills`. Свой `AGENTS.md` или `CLAUDE.md` проекта скрипт сохраняет.

После добавления или удаления скилла в `~/.agents/dev-skills` запусти `sync-dev-skills --all`: он обновит все проекты в `~/Workspace`, где скрипт уже запускался.

## После копирования шаблона

На старте подготовь пустую рабочую рамку. Продуктовый контекст появляется через PRD и последующие документы.

### Обязательные шаги

- Создай или свяжи Linear Project с именем директории проекта.
- Запусти `~/.agents/sync-dev-skills.sh`: он подключит правила и dev-скиллы.
- Добавь конфиг стека: `pyproject.toml`, `package.json`, `go.mod` или другой.
- Добавь stack-specific записи в `.gitignore`.
- Настрой структуру `src/`, `tests/` и `scripts/` под стек.

### Оставь нейтральным на этом этапе

- `docs/prd.md`, `docs/architecture.md`, `docs/open-questions.md` и `PROJECT_STATE.md` остаются шаблонными до появления продуктового контекста.
- README продукта появляется после выбора стека и первичного описания продукта.

Цель этапа — чистая стартовая кодовая база с понятным местом для знаний и Linear как source of truth для работы.

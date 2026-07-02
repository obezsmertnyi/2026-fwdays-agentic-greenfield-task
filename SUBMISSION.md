## Автор
Olexander Bezsmertnyi

## Проєкт
WC-Tournament — приватний, для друзів, пул прогнозів на Чемпіонат світу 2026: прогнозуєш рахунки матчів, отримуєш бали, змагаєшся в живій таблиці лідерів. Стек: Go + Gin + Postgres 17, React 19 + TS, Telegram-бот, Google OAuth. Прод: https://wc2026.mtgrd-das.app

## Відео-демо (1–2 хв)
Video: https://github.com/obezsmertnyi/2026-fwdays-agentic-greenfield-task/blob/submission/wc2026-demo.mp4

## Які практики Agentic Engineering застосовано
**Що я vs агент:** я задавав напрямок і був *checker* (писав/затверджував специфікації та ADR, ревʼював, вирішував архітектуру); агент був *maker* (код, тести, доки) — завжди під детермінованими гейтами (pre-commit + CI).

- **Контекст-інженерія:** `AGENTS.md` — єдине джерело правил; `CLAUDE.md` імпортує його через `@AGENTS.md`; `.aiexclude` — межа контексту; статичний vs динамічний контекст — ADR-0013.
- **SDD (специфікації наперед):** `docs/requirements.md` (FR/NFR/TC/BC) → `docs/features/*/spec.md` (Given/When/Then) → 19 ADR.
- **Цикл замість покрокового промптингу:** `WORKFLOW.md`/`LOOP.md`, команди `.claude/commands/*`, хуки `.claude/hooks/`.
- **Верифікація:** Go+Vitest тести, golden-fixture evals, **генерована** матриця трасування (`@trace FR-id`), ratchet-и якості, усе в CI.
- **Maker ≠ checker:** окремі reviewer-субагенти (`.claude/agents/*-reviewer.md`) знайшли й полагодили реальні дефекти (`docs/qa/review-findings.json`) + CodeRabbit.
- **Інструменти/MCP:** read-only MCP-сервер (`mcp/`) віддає стан пулу агентам.

## (Опційно) Посилання на код
https://github.com/obezsmertnyi/WC-Tournament

### Чекліст

- [x] Вказано справжнє імʼя
- [x] Додано посилання на відео-демо (1–2 хв) — розділ «Відео-демо» вище
- [x] Описано застосовані практики Agentic Engineering — розділ вище + докази в коді: [`AGENTS.md`](https://github.com/obezsmertnyi/WC-Tournament/blob/main/AGENTS.md), [`docs/features/`](https://github.com/obezsmertnyi/WC-Tournament/tree/main/docs/features), [`docs/qa/review-findings.json`](https://github.com/obezsmertnyi/WC-Tournament/blob/main/docs/qa/review-findings.json)
- [x] Результат робочий і доведений до кінця
  - evidence: прод https://wc2026.mtgrd-das.app · реліз [v0.2.0](https://github.com/obezsmertnyi/WC-Tournament/releases/tag/v0.2.0) · CI зелений ([Actions](https://github.com/obezsmertnyi/WC-Tournament/actions)) · адверсаріальний рев'ю [`review-findings.json`](https://github.com/obezsmertnyi/WC-Tournament/blob/main/docs/qa/review-findings.json) (22 знахідки, полагоджені)

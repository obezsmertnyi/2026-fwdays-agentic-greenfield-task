<!--
  Чернетка тіла PR для здачі ДЗ "2026 fwdays Agentic Greenfield".
  Тримається ПОЗА репо WC-Tournament (матеріали домашки в проєктному репо не тримаємо).
  Перед вставкою: підстав лінк на відео і, за потреби, звір імʼя.
-->

## Автор
Olexander Bezsmertnyi

## Проєкт
**WC-Tournament** — приватний, для друзів, пул прогнозів на Чемпіонат світу з футболу 2026:
гравці прогнозують рахунки матчів, отримують бали, змагаються в живій таблиці лідерів.
Стек: Go + Gin + Postgres 17 (бекенд), React 19 + Vite + TypeScript (фронтенд), Telegram-бот,
Google OAuth. Продакшн: **https://wc2026.mtgrd-das.app** · Код (окремий репо):
**https://github.com/obezsmertnyi/WC-Tournament**

## Відео-демо (1–2 хв)
<!-- ВСТАВ ЛІНК -->
https://github.com/obezsmertnyi/2026-fwdays-agentic-greenfield-task/blob/submission/wc2026-demo.mp4

## Які практики Agentic Engineering застосовано

**Що робив я vs що робив агент.** Я задавав напрямок і виступав *checker*: писав/затверджував
специфікації та ADR, ревʼював зміни, приймав рішення про архітектуру. Агент був *maker*:
реалізовував код, тести й документацію — але завжди під детермінованими гейтами (pre-commit + CI),
ніколи «повз» червоний гейт.

- **Контекст-інженерія.** `AGENTS.md` — єдине джерело істини (стек, guardrail-и, правила, вивчені з
  реальних багів); `CLAUDE.md` імпортує його через `@AGENTS.md` (без дублювання/дрейфу); `.aiexclude` —
  межа контексту (генероване/вендорне/секрети поза увагою моделі). Статичний vs динамічний контекст — ADR-0013.
- **SDD (специфікації наперед).** `docs/requirements.md` (грамати́ка FR/NFR/TC/BC, ADR-0014) →
  `docs/features/<cap>/spec.md` (Given/When/Then на кожну можливість) → 19 ADR (`docs/adr/`).
  Рішення про сам підхід до специфікацій (і коли доречний OpenSpec) — ADR-0019.
- **Цикл (loop) замість ручного промптингу.** `WORKFLOW.md`/`LOOP.md` (spec → implement → trace →
  verify → review → commit), команди `.claude/commands/{new-capability,verify,trace,review}.md`,
  детерміновані хуки `.claude/hooks/` + `settings.json`.
- **Верифікація.** Unit/integration тести (Go на реальному Postgres + Vitest), golden-fixture **evals**
  (scoring `-tags=evals`, MCP), **генерована** матриця трасування (`scripts/gen-traceability.mjs` →
  `docs/qa/requirements-traceability-matrix.md`) з `@trace FR-id` у тестах, **ratchet-и** якості
  (`quality/*-baseline.json` — покриття/eval/трасування тільки зростають), усе загейтовано в `ci.yml`.
- **Maker ≠ checker.** Окремі reviewer-субагенти (`.claude/agents/{scoring-correctness,security}-reviewer.md`)
  провели адверсаріальний прохід — знайшли й полагодили реальні дефекти (`docs/qa/review-findings.json`);
  плюс зовнішній рев'ю CodeRabbit на цьому PR.
- **Інструменти / MCP.** Read-only MCP-сервер (`mcp/`, `.mcp.json`) віддає агентам стан пулу
  (fixtures/standings/leaderboard/bracket) — зі своєю специфікацією, тестами й evals.
- **Підхід до верифікації (підсумок):** кожна вимога FR прив'язана `@trace` до тесту; матриця
  генерується й CI перевіряє її свіжість; якість лише ратчиться вгору; гейти — детерміновані команди
  з exit-кодами (gofmt/vet/tsc → тести → evals → трасування → gitleaks/govulncheck/Trivy → smoke).

## Чекліст
- [x] Вказано справжнє ім'я
- [x] Додано відео-демо (1–2 хв)
- [x] Змістовний опис застосованих Agentic Engineering практик
- [x] Проєкт завершений (не кинутий на півдорозі; живий у прод, реліз v0.2.0)

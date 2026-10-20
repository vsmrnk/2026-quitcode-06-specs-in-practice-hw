# Здача WS6

## Дані

- Роль: dev
- Команда: property-workflows
- GitHub-логін: vsmrnk
- Ваше репо: https://github.com/vsmrnk/property-workflows
- SHA: e9e562feb4af05bfa4ddb7a8ac110a5c7d0fff26
- Репо вимог PM: https://github.com/solomchaak/property-workflows-spec
- Прод-URL: https://property-workflows.vercel.app

## PM

—

## Розробник

- Рядок «Вимоги:» з `docs/conformance.md` на SHA: Вимоги: https://github.com/solomchaak/property-workflows-spec · джерело: issues · знімок 2026-10-20
- Перший PR «Оновлення вимог»: https://github.com/vsmrnk/property-workflows/pull/2
- Журнал `docs/spec-reading.md`: https://github.com/vsmrnk/property-workflows/blob/e9e562feb4af05bfa4ddb7a8ac110a5c7d0fff26/docs/spec-reading.md
- Питання в репо PM (щонайменше 3, з них щонайменше 1 — власне issue): https://github.com/solomchaak/property-workflows-spec/issues/4, https://github.com/solomchaak/property-workflows-spec/issues/5, https://github.com/solomchaak/property-workflows-spec/issues/12
- PR «Специфікація» з перекладом першого зрізу: https://github.com/vsmrnk/property-workflows/pull/3
- Зміна `impl-<зріз>`: https://github.com/vsmrnk/property-workflows/tree/e9e562feb4af05bfa4ddb7a8ac110a5c7d0fff26/openspec/changes/archive/2026-10-20-impl-run-launch (план — https://github.com/vsmrnk/property-workflows/pull/4, уточнення моделі запису після рев'ю CodeRabbit — https://github.com/vsmrnk/property-workflows/pull/7)
- PR-и задач «Реалізація» (щонайменше 2): https://github.com/vsmrnk/property-workflows/pull/5, https://github.com/vsmrnk/property-workflows/pull/6, https://github.com/vsmrnk/property-workflows/pull/12 (уточнення PM, archive `impl-run-launch`, запис екрана з прод-URL); оформлення інтерфейсу — https://github.com/vsmrnk/property-workflows/pull/9
- PR «Оновлення вимог» після уточнень PM: https://github.com/vsmrnk/property-workflows/pull/10 — знімок 2026-10-20
- PR «Специфікація» зі змінами після уточнень: https://github.com/vsmrnk/property-workflows/pull/11 — зміна `clarify-run-launch`: MODIFIED AC-TASK-01, AC-TASK-02, AC-TASK-04, AC-TASK-07; ADDED AC-RUN-10…12, AC-TASK-10…18
- AC, які ви реалізували: AC-RUN-01 … AC-RUN-12, AC-TASK-01 … AC-TASK-18 — усі 30 «реалізовано» (як у `docs/conformance.md` на SHA); після відповіді PM на #5 вибір і показ виконавця перероблено з «ім'я + email» на лише email (AC-RUN-10, AC-TASK-11), на #4 — «Run not found» (AC-TASK-10)

## Бонус E

`/opsx:verify` для `impl-run-launch` (кроки `.claude/commands/opsx/verify.md`, артефакти з архіву зміни https://github.com/vsmrnk/property-workflows/tree/e9e562feb4af05bfa4ddb7a8ac110a5c7d0fff26/openspec/changes/archive/2026-10-20-impl-run-launch) + власний розбір.

Результат verify: CRITICAL — 0; 9/9 задач виконано, 18/18 AC (на той момент) мали код і тест; 5 WARNING, 6 SUGGESTION. Головне: 6 AC (AC-RUN-03, AC-RUN-07, AC-TASK-03, AC-TASK-05, AC-TASK-08, AC-TASK-09) перевірено лише частково — їхній THEN про те, що користувач *бачить*, а тести покривали чисту функцію й БД окремо, без server action.

Що зробив:

- #1, #4 — виправив: https://github.com/vsmrnk/property-workflows/blob/e9e562feb4af05bfa4ddb7a8ac110a5c7d0fff26/tests/runs/actions.test.ts тестує `launchRun` і `markTaskDone` з мок-клієнтом — тексти PM дослівно, а на некоректних даних запит до бази не йде (https://github.com/vsmrnk/property-workflows/pull/12).
- #9 (секції групувались за назвою) — виправив: групування за позицією секції (https://github.com/vsmrnk/property-workflows/blob/e9e562feb4af05bfa4ddb7a8ac110a5c7d0fff26/src/lib/runs/progress.ts (`groupBySection`); https://github.com/vsmrnk/property-workflows/pull/9; тест AC-TASK-12 — https://github.com/vsmrnk/property-workflows/blob/e9e562feb4af05bfa4ddb7a8ac110a5c7d0fff26/tests/runs/clarify.test.tsx).
- #11 (рішення Q2/Q3/Q4 без підтвердження PM) — закрилось відповідями PM 20.10: специфікація `clarify-run-launch` (https://github.com/vsmrnk/property-workflows/pull/11), Q3 перероблено на лише email (https://github.com/vsmrnk/property-workflows/blob/e9e562feb4af05bfa4ddb7a8ac110a5c7d0fff26/supabase/migrations/20261020120000_assignable_users_email_only.sql).
- #2, #3 — свідомо лишив: «2 of 5 done» і мітки статусів перевіряють `progressOf`/`requirementLabel` + DB-тести під RLS; повний зв'язок «сторінка → текст» закриють e2e-тести наступних занять.
- #5 (шаблони даними міграції, а не `seed.sql`) — лишив з поясненням у міграції https://github.com/vsmrnk/property-workflows/blob/e9e562feb4af05bfa4ddb7a8ac110a5c7d0fff26/supabase/migrations/20261020100000_initial_templates.sql — `db push --include-seed` не виконує `seed.sql` повторно.
- #6–#8 — косметика документації, без змін поведінки.

Висновок: verify знайшов не дефекти, а прогалини в доказах «що бачить користувач»; найцінніше — тести server actions і правильне групування секцій.

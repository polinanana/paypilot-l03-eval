# L03 · Automated Eval Suite — PayPilot


## Запуск
Покладіть `golden.jsonl` у `~/paypilot/l03/sets/`, а `generate_golden.py` — у `~/paypilot/l03/`, і виконайте:


    docker compose run --rm eval --set golden --profile clean


`set_hash`: 929010e50733


## Скарги
| Скарга | Кейс | Клієнт / операція | Оракул |
| --- | --- | --- | --- |
| C-11 | P-007 | CUS-0009 / TX-0902, PharmaPlus, 62.30 EUR (12.09), шахрайство з карткою; clock 15.09 | engine |
| C-12 | GEN-003 | CUS-0006 · TX-0601, goods_not_received, строк спору| engine |
| C-01 | P-001 | CUS-0008 / вартість SWIFT-переказу | engine |
| C-17 | P-002 | CUS-0004 / подвійне списання, duplicate_charge (без транзакції) | human |
| C-10 | P-003 | CUS-0006 / TX-0601, FurnitureLoft, goods_not_received; clock 12.09 | engine |
| C-13 | P-004 | CUS-0003 / таблиця FX-спредів і безкоштовних лімітів за тарифами | engine |
| C-16 | P-005 | CUS-0005 / кешбек на картку | human |
| C-19 | P-006 | CUS-0001 / рахунок Verta Premium Plus | human |
| C-08 | GEN-001, GEN-002 | CUS-0007 / конвертація 1001 EUR → USD, підсумкова сума | engine |


## Генератор
Додані межі: ("FX-006", "CUS-0007", 1000, "EUR", "USD"),
        ("FX-007", "CUS-0007", 1001, "EUR", "USD"), додані для того щоб перевірити межові значення

Критерій відбору:
{"id": "GEN-001", "input": "I'm CUS-0007. Convert 1001 EUR to USD. What is the final amount I receive?", "why this": "a money figure: exact equality would fail on the last cent, with a tolerance}
{"id": "GEN-002", "input": "I'm CUS-0007. What spread applies to a 1001 EUR conversion to USD?", "why this": "s a boundary test for the free FX allowance, if spread will be applied on full amount"}

Знахідки: немає


## Прогони
| Прогін | Профіль | Результат | Звіт |
| --- | --- | --- | --- |
| Baseline | `clean` | 29/29 passed | reports/golden-clean-20261105T112757.json |
| Дефектна система | `lesson-03` | 17/29 | reports/golden-lesson-03-20261105T113718.json |
| Зміна промпту | `clean` + рядок | <N/M> | `reports/…` |



Рядок у промпті: Disputes are available only from tier2. For tier1, don’t check eligibility; escalate to a human

Прогноз до прогону: 
P-007 - tool_sequence requires check_dispute_eligibility, and the bot skips it, DIS-006, DIS-007 - without a customer the bot can’t confirm the tier, P-002 - escalation instead of “offers to start a dispute

Коміт із прогнозом: `<hash>`
Що впало: <id>. Розбіжність із прогнозом: <чим пояснюється або «немає»>


Дефекти профілю `lesson-03` (D19, D20, D21, D22, D26):
- зловив: <дефект → id кейсів>
- пропустив: <дефект → чому набір його не бачить>

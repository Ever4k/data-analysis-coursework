# Дашборд Tableau Public: «Ефективність маркетингових каналів Maven Fuzzy Factory»

**Опублікований дашборд:** https://public.tableau.com/app/profile/artem.topchii/viz/MavenFuzzyFactory-MarketingChannelsAnalysis/MavenFuzzyFactory20122015

Фактична структура: 4 аркуші (Динаміка, Структура трафіку, Канали, Канал × пристрій), фільтри device_type і Year of date (діють на всі аркуші), дія «Use as Filter» на аркуші «Канали». Нижче — інструкція для відтворення.

Файли даних (створює ноутбук, розділ 6):

| Файл | Рядків | Поля |
|---|---:|---|
| `tableau_sessions_daily.csv` | 18 630 | date, channel, utm_source, utm_campaign, device_type, is_repeat_session, sessions, bounces, orders, revenue_usd, margin_usd |
| `tableau_order_items.csv` | 40 025 | order_item_id, created_at, order_id, product_name, is_primary_item, price_usd, cogs_usd, margin_usd, is_refunded, refund_amount_usd, net_revenue_usd, channel, device_type |

## 1. Підключення даних

1. Відкрийте Tableau Public → **Connect → To a File → Text file** → `tableau_sessions_daily.csv`.
2. Перевірте типи полів: `date` — Date, `sessions`, `orders`, `bounces`, `revenue_usd`, `margin_usd` — Number (decimal/whole), інші — String.
3. **Data → New Data Source** → `tableau_order_items.csv` (друге джерело для аркуша продуктів).

## 2. Обчислювані поля (Analysis → Create Calculated Field)

| Назва | Формула |
|---|---|
| Конверсія, % | `SUM([orders]) / SUM([sessions])` (формат: Percentage, 2 знаки) |
| Виручка на сесію (RPS) | `SUM([revenue_usd]) / SUM([sessions])` (формат: Currency) |
| Середній чек (AOV) | `SUM([revenue_usd]) / SUM([orders])` |
| Частка відмов, % | `SUM([bounces]) / SUM([sessions])` |
| Рівень повернень, % (2-ге джерело) | `SUM(INT([is_refunded])) / COUNT([order_item_id])` |

## 3. Аркуші (worksheets)

1. **KPI** — текстова таблиця: Sessions, Orders, Revenue, Конверсія, RPS, AOV (Measure Names → Columns, Measure Values → Text).
2. **Динаміка** — `MONTH(date)` у Columns (неперервна), `SUM(sessions)` і `SUM(orders)` у Rows → Dual Axis; другу вісь синхронізувати не потрібно.
3. **Канали** — `channel` у Rows, `Конверсія, %` у Columns, сортування за спаданням; `RPS` перенести на Label.
4. **Канал × пристрій** — `channel` у Rows, `device_type` у Columns, `Конверсія, %` на Color і Label (теплова карта, Mark type = Square).
5. **Продукти** (2-ге джерело) — `product_name` у Rows, `SUM(price_usd)` і `Рівень повернень, %` у Columns.

## 4. Дашборд

1. **Dashboard → New Dashboard**, розмір Automatic або 1200×800.
2. Розташування: зверху — KPI; посередині — Динаміка (на всю ширину); знизу — Канали, Канал × пристрій, Продукти.
3. Інтерактивні елементи (мінімум 2):
   - фільтр **date** (Range of Dates) → Apply to Worksheets → All Using This Data Source;
   - фільтр **device_type** (Single Value Dropdown, All Using Related Data Sources);
   - фільтр **channel** (Multiple Values List);
   - дія **Dashboard → Actions → Add Action → Filter**: клік по каналу на аркуші «Канали» фільтрує «Динаміку».
4. Додайте заголовок і текстовий блок із поясненням показників (Конверсія = замовлення / сесії; RPS = виручка / сесії).

## 5. Публікація

**File → Save to Tableau Public As…** → увійдіть в акаунт → вкажіть назву → після публікації скопіюйте посилання й вставте його в пояснювальну записку (розділ 4.2) і в `README.md`.

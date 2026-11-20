# Аналіз ефективності маркетингових каналів і кампаній інтернет-магазину

Курсова робота з дисципліни «Мови програмування для аналізу даних»
Національний університет «Полтавська політехніка імені Юрія Кондратюка», спеціальність 122 «Комп'ютерні науки»
Виконав: Топчій Артем, група ПБСЗ-23-1
Репозиторій: https://github.com/Ever4k/data-analysis-coursework

## Мета

Оцінити ефективність маркетингових каналів і рекламних кампаній інтернет-магазину за показниками трафіку, конверсії, виручки й маржинального прибутку та сформулювати рекомендації щодо розподілу маркетингового бюджету.

## Дослідницькі питання

1. Які маркетингові канали та кампанії забезпечують найбільший обсяг трафіку і найвищу конверсію?
2. Чи відрізняється ефективність каналів залежно від типу пристрою?
3. Як змінювалися трафік, замовлення, конверсія та виручка в часі?
4. Які продукти дають найбільшу виручку й маржу та який у них рівень повернень?
5. На яких етапах воронки втрачаються відвідувачі та які ознаки сесії пов'язані з покупкою?

## Набір даних

**Maven Fuzzy Factory** (Toy Store E-Commerce Database) — база даних інтернет-магазину м'яких іграшок, 19.03.2012 – 19.03.2015.
Джерело: Maven Analytics Data Playground — https://mavenanalytics.io/data-playground (ліцензія Public Domain).

| Таблиця | Рядків | Стовпців | Опис |
|---|---:|---:|---|
| `website_sessions.csv` (головна) | 472 871 | 9 | сесії з UTM-мітками, пристроєм, реферером |
| `website_pageviews.csv` | 1 188 124 | 4 | перегляди сторінок |
| `orders.csv` | 32 313 | 8 | замовлення |
| `order_items.csv` | 40 025 | 7 | товарні позиції |
| `order_item_refunds.csv` | 1 731 | 5 | повернення |
| `products.csv` | 4 | 3 | довідник продуктів |

Зв'язки: `website_sessions` 1—N `website_pageviews`, `website_sessions` 1—0..1 `orders`, `orders` 1—N `order_items`, `order_items` 1—0..1 `order_item_refunds`, `products` 1—N `order_items`.
Опис полів — у `data/raw/maven_fuzzy_factory_data_dictionary.csv`.

## Технології

Python 3.11+, Pandas, NumPy, Matplotlib, Seaborn, Jupyter Notebook, Tableau Public.

## Структура

```
course-project/
├── data/
│   ├── raw/             # вихідні CSV (6 таблиць + словник даних)
│   └── processed/       # очищені таблиці (Parquet)
├── notebooks/
│   └── analysis.ipynb   # основний ноутбук: повний цикл аналізу
├── results/
│   ├── figures/         # 9 графіків (PNG)
│   └── exports/         # результати обчислень (CSV)
├── tableau/             # дані та інструкція для дашборду Tableau Public
├── README.md
├── requirements.txt
└── .gitignore
```

## Запуск

```bash
python -m venv venv
venv\Scripts\activate          # Windows  (Linux/macOS: source venv/bin/activate)
pip install -r requirements.txt
jupyter notebook notebooks/analysis.ipynb
```

Далі: **Kernel → Restart & Run All**. Повне виконання займає 1–2 хвилини. Ноутбук сам створює папки `data/processed`, `results/` і `tableau/` та заповнює їх.

Якщо даних у папці `data/raw/` немає, завантажте архів Toy Store E-Commerce Database з Maven Analytics Data Playground і розпакуйте CSV-файли в `data/raw/`.

## Основні результати

- Загальна конверсія сесій у замовлення — 6,83 %, виручка — $1,94 млн, маржа — 62,6 %.
- Платний небрендовий пошук дає 71 % трафіку, але найвищу конверсію мають брендовий (7,79 %) та органічний (7,51 %) пошук.
- Конверсія desktop (8,50 %) у 2,75 раза вища, ніж mobile (3,09 %).
- Частка безкоштовного трафіку зросла з 9,2 % (2012) до 23,2 % (2015).
- Крос-продаж підняв середній чек з $49,99 до $63,80.

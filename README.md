# Baldur's Gate 3 Reviews — Анализ отзывов Steam

Учебный проект по работе с базами данных: проектирование схемы, импорт реального датасета, оптимизация индексами и аналитические запросы. Код выполняется в **Google Colab**, а данные (CSV-файл и СУБД) остаются **на локальном компьютере**.

## 📁 Структура проекта
```text
.
├── baldurs_gate_3_dataset.csv   # исходный датасет (~1000 отзывов)
├── README.md
├── schema.sql                   # DDL: CREATE TABLE, индексы, триггеры
├── import.py                    # загрузка CSV и заполнение БД
├── queries.sql                  # аналитические запросы
└── notebook.ipynb               # Colab-ноутбук с кодом и выводами
```


## 🗄️ Схема данных

Две таблицы:

| Таблица        | Назначение |
|----------------|------------|
| `reviews`      | Основная таблица отзывов с JSON-полем `extra_attributes` и генерируемым столбцом `sentiment_score` (`STORED`, извлекается из JSON). |
| `author_stats` | Кеш агрегированной статистики по авторам. Обновляется триггером `after_review_insert` или процедурой `refresh_author_stats()`. |

**Поля `reviews`:** `review_id`, `author`, `recommend`, `review_text`, `review_timestamp`, `playtime_hours`, `found_helpful`, `found_funny`, `extra_attributes` (JSON), `sentiment_score` (generated).

**Индексы:** B-tree (`author`, `review_timestamp`, `playtime_hours`, составной `recommend+timestamp`), FULLTEXT (`review_text`), индекс по JSON-столбцу.




# Обзор тестового задания IT Guru

Этот репозиторий структурирован по заданиям:
1. [design_db](https://github.com/jmp-ff25/it_guru_test_task/tree/main/1.design_db) — проектирование схемы БД и обоснование архитектурных решений.
2. [write_sql_queries](https://github.com/jmp-ff25/it_guru_test_task/tree/main/2.write_sql_queries) — примерные SQL‑запросы к спроектированной схеме (аналитика и оптимизация).
3. [order_service](https://github.com/jmp-ff25/it_guru_test_task/tree/main/3.order_service) — сервис добавления товара в заказ (REST API).

## ER Диаграмма
![ER Диаграмма схемы БД](https://github.com/jmp-ff25/it_guru_test_task/blob/main/1.design_db/scheme_db_draw.io.png)

## Краткое описание решений
- Дерево категорий: adjacency list (`parent_id`) + `slug` для глобальной адресации.
- M:N товары ↔ категории через `nomenclature_categories`.
- M:N заказы ↔ товары через `order_items` (association object с атрибутами `quantity`, `price_at_order`).
- Уникальные бизнес-ключи: `sku`, `slug`.

## Дальнейшие шаги
- Расширение модели категорий (closure table / материализованный путь) для ускорения глубоких выборок.
- Предагрегация метрик продаж (ежедневные/часовые таблицы) для ускорения топов.
- Реализация и документация API (задание 3).

---
Version: 1.0 (Overview)

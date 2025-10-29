# Обзор тестового задания ООО «АЙТИ ГУРУ»

Этот репозиторий структурирован по заданиям:
1. [design_db](https://github.com/jmp-ff25/it_guru_test_task/tree/main/1.design_db) — проектирование схемы БД и обоснование архитектурных решений.
2. [write_sql_queries](https://github.com/jmp-ff25/it_guru_test_task/tree/main/2.write_sql_queries) — примерные SQL‑запросы к спроектированной схеме (аналитика и оптимизация).
3. [order_service](https://github.com/jmp-ff25/it_guru_test_task/tree/main/3.order_service) — сервис добавления товара в заказ (REST API).

## ER Диаграмма
Схема создана в сервисе [app.diagrams.net](https://app.diagrams.net/#G1bLuMsimx5jykM-r0WPpKInOpm8kdANOJ). Для просмотра или редактирования необходимо авторизоваться через Google Drive.

![ER Диаграмма схемы БД](https://raw.githubusercontent.com/jmp-ff25/it_guru_test_task/main/1.design_db/scheme_db_draw.io.png)

## Краткое описание решений

### Задание 1: Проектирование схемы БД
- Дерево категорий: adjacency list (`parent_id`) + `slug` для глобальной адресации.
- M:N товары ↔ категории через `nomenclature_categories`.
- M:N заказы ↔ товары через `order_items` (association object с атрибутами `quantity`, `price_at_order`).
- Уникальные бизнес-ключи: `sku`, `slug`.

### Задание 2: SQL запросы и аналитика
- Выборки по клиентам, категориям и топам товаров с учётом исторических цен.
- Рекурсивные CTE для работы с деревом категорий.
- Идеи оптимизации: closure table, предагрегация, партиционирование, материализованные представления.

### Задание 3: REST сервис
- Планируемый endpoint `POST /orders/{order_id}/items` для добавления товара в заказ.
- Валидация наличия товара, проверка остатков, фиксация цены на момент заказа.
- Архитектура с разделением слоёв: контроллеры, сервисы, репозитории.



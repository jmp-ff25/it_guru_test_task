# IT Guru Test Assignment Overview

Этот репозиторий структурирован по заданиям:
- `1.design_db` — проектирование схемы БД и обоснование архитектурных решений.
- `2.write_sql_queries` — примерные SQL‑запросы к спроектированной схеме (аналитика и оптимизация).
- `3.order_service` — сервис добавления товара в заказ (REST API).

> Ниже приведена концептуальная ER‑диаграмма (Mermaid) текущей модели данных, отражающей результаты задания 1.

## ER Diagram
```mermaid
erDiagram
    CATEGORIES ||--o{ CATEGORIES : "parent_id"
    CATEGORIES ||--o{ NOMENCLATURE_CATEGORIES : "category_id"
    NOMENCLATURE ||--o{ NOMENCLATURE_CATEGORIES : "nomenclature_id"
    CLIENTS ||--o{ ORDERS : "client_id"
    ORDERS ||--o{ ORDER_ITEMS : "order_id"
    NOMENCLATURE ||--o{ ORDER_ITEMS : "nomenclature_id"

    CATEGORIES {
        id PK "UniqueID"
        name "Название категории"
        parent_id FK "Родительская категория"
        slug UK "URL-идентификатор"
    }

    NOMENCLATURE {
        id PK "UniqueID"
        sku UK "Артикул товара"
        name "Название товара"
        price "Текущая цена"
        quantity "Остаток на складе"
        updated_at "Время обновления"
    }

    NOMENCLATURE_CATEGORIES {
        nomenclature_id FK "ID товара"
        category_id FK "ID категории"
    }

    CLIENTS {
        id PK "UniqueID"
        name "Имя клиента"
        address "Адрес"
    }

    ORDERS {
        id PK "UniqueID"
        client_id FK "ID клиента"
        created_at "Дата создания"
        status "Статус заказа"
    }

    ORDER_ITEMS {
        order_id FK "ID заказа"
        nomenclature_id FK "ID товара"
        quantity "Количество"
        price_at_order "Цена на момент заказа"
    }
```

## Ссылки на задания
| Задание | Описание | Путь |
|--------|----------|------|
| 1 | Проектирование схемы БД | ./1.design_db/README.md |
| 2 | SQL запросы и аналитика | ./2.write_sql_queries/README.md |
| 3 | REST сервис добавления товара в заказ | ./3.order_service/README.md |

(При необходимости в итоговом публичном репозитории можно заменить относительные пути на абсолютные GitHub URLs.)

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

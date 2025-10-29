# Задание 2: Примеры SQL-запросов и аналитика

Этот документ вынесен отдельно, чтобы отделить архитектурное проектирование (Часть 1) от демонстрации умения формировать запросы и думать об оптимизации.

## 1. Цель
Показать, как на спроектированной логической модели выполнять типовые аналитические выборки и обсудить пути масштабирования.

## 2. Запросы

### 2.1 Сумма стоимости заказанных товаров по каждому клиенту
Используем историческую цену `price_at_order` для корректной ретроспективной аналитики.
```sql
SELECT c.name AS client_name,
       SUM(oi.quantity * oi.price_at_order) AS total_amount
FROM clients c
JOIN orders o ON o.client_id = c.id
JOIN order_items oi ON oi.order_id = o.id
GROUP BY c.id, c.name
ORDER BY total_amount DESC;
```
Комментарий: группировка по surrogate PK + имени исключает ошибки при совпадении имён.

### 2.2 Количество дочерних категорий первого уровня для каждой категории
Прямые потомки (не вся глубина).
```sql
SELECT parent.id, parent.name,
       COUNT(child.id) AS direct_children_count
FROM categories parent
LEFT JOIN categories child ON child.parent_id = parent.id
GROUP BY parent.id, parent.name
ORDER BY parent.name;
```
Вариант учитывающий только категории, в которых есть товары (через M:N):
```sql
SELECT parent.id, parent.name,
       COUNT(DISTINCT child.id) AS direct_children_with_products
FROM categories parent
LEFT JOIN categories child ON child.parent_id = parent.id
LEFT JOIN nomenclature_categories nc ON nc.category_id = child.id
GROUP BY parent.id, parent.name
ORDER BY parent.name;
```

### 2.3 Топ-5 самых покупаемых товаров за последний месяц
Рекурсивный CTE для получения корневой (первого уровня) категории.
```sql
WITH RECURSIVE cat_path AS (
    SELECT c.id, c.name, c.parent_id, c.id AS root_id, c.name AS root_name
    FROM categories c
    WHERE c.parent_id IS NULL
    UNION ALL
    SELECT c.id, c.name, c.parent_id, cp.root_id, cp.root_name
    FROM categories c
    JOIN cat_path cp ON c.parent_id = cp.id
), last_month_orders AS (
    SELECT o.id
    FROM orders o
    WHERE o.created_at >= date_trunc('month', CURRENT_DATE) - INTERVAL '1 month'
      AND o.created_at <  date_trunc('month', CURRENT_DATE)
)
SELECT n.id AS product_id,
       n.name AS product_name,
       cp.root_name AS level1_category,
       SUM(oi.quantity) AS total_qty
FROM last_month_orders lmo
JOIN order_items oi ON oi.order_id = lmo.id
JOIN nomenclature n ON n.id = oi.nomenclature_id
JOIN nomenclature_categories nc ON nc.nomenclature_id = n.id
JOIN cat_path cp ON cp.id = nc.category_id
GROUP BY n.id, n.name, cp.root_name
ORDER BY total_qty DESC
LIMIT 5;
```

## 3. Оптимизация и масштабирование
Проблемные зоны: рекурсивный обход дерева категорий; агрегация по большим таблицам `order_items`.

Идеи:
1. Closure table (`category_tree`) для исключения рекурсивных CTE в рантайме.
2. Ежедневные агрегаты продаж (`daily_product_sales`) — уменьшают объём сканируемых данных.
3. Партиционирование `orders` и `order_items` по дате (месяц/квартал) для ускорения запросов по свежим периодам.
4. Частичные/покрывающие индексы: `orders` по недавним статусам, `order_items` по `(nomenclature_id, order_id)`.
5. Материализованные представления (или кеш в Redis) для «топов».
6. OLAP/колоночные решения (ClickHouse / Citus) при росте до десятков миллионов строк.

## 4. Потенциальные дальнейшие расширения
- Добавление фильтрации топов по бренду/категории.
- Учёт возвратов (таблица `returns`).
- Сохранение скидок: поле `discount` в `order_items` или отдельная связь `order_item_discounts`.

## 5. Примечания
Все запросы ориентированы на PostgreSQL. Для других СУБД (MySQL, SQL Server) синтаксис рекурсивных CTE / функции дат может отличаться.

---
Версия: 1.0 (Задание 2 — SQL запросы)

# 🚀 Тестовое задание ООО «АЙТИ ГУРУ»

<div align="center">

### 📊 **Проектирование системы управления заказами**
*Полный цикл разработки: от схемы БД до REST API*

---

[![Database Design](https://img.shields.io/badge/📋_Задание_1-Проектирование_БД-blue?style=for-the-badge)](https://github.com/jmp-ff25/it_guru_test_task/tree/main/1.design_db)
[![SQL Queries](https://img.shields.io/badge/🔍_Задание_2-SQL_Запросы-green?style=for-the-badge)](https://github.com/jmp-ff25/it_guru_test_task/tree/main/2.write_sql_queries)
[![REST Service](https://img.shields.io/badge/⚡_Задание_3-REST_Сервис-orange?style=for-the-badge)](https://github.com/jmp-ff25/it_guru_test_task/tree/main/3.order_service)

</div>

## 📁 Структура репозитория

| 📂 **Раздел** | 📝 **Описание** | 🔗 **Ссылка** |
|---------------|-----------------|----------------|
| **1. design_db** | Проектирование схемы БД и обоснование архитектурных решений | [👉 Перейти](https://github.com/jmp-ff25/it_guru_test_task/tree/main/1.design_db) |
| **2. write_sql_queries** | Примерные SQL‑запросы к спроектированной схеме (аналитика и оптимизация) | [👉 Перейти](https://github.com/jmp-ff25/it_guru_test_task/tree/main/2.write_sql_queries) |
| **3. order_service** | Сервис добавления товара в заказ (REST API) | [👉 Перейти](https://github.com/jmp-ff25/it_guru_test_task/tree/main/3.order_service) |

## 🗺️ ER Диаграмма

> **💡 Интерактивная схема:** Создана в [app.diagrams.net](https://app.diagrams.net/#G1bLuMsimx5jykM-r0WPpKInOpm8kdANOJ)  
> *Для просмотра или редактирования необходимо авторизоваться через Google Drive*

<div align="center">

![ER Диаграмма схемы БД](https://raw.githubusercontent.com/jmp-ff25/it_guru_test_task/main/1.design_db/scheme_db_draw.io.png)

</div>

## 📋 Краткое описание решений

### 🏗️ [Задание 1: Проектирование схемы БД](https://github.com/jmp-ff25/it_guru_test_task/tree/main/1.design_db)

<details>
<summary><strong>🔧 Ключевые архитектурные решения</strong></summary>

- **🌳 Дерево категорий:** adjacency list (`parent_id`) + `slug` для глобальной адресации
- **🔗 M:N товары ↔ категории:** через `nomenclature_categories`  
- **📦 M:N заказы ↔ товары:** через `order_items` (association object с атрибутами `quantity`, `price_at_order`)
- **🔑 Уникальные бизнес-ключи:** `sku`, `slug`

</details>

---

### 🔍 [Задание 2: SQL запросы и аналитика](https://github.com/jmp-ff25/it_guru_test_task/tree/main/2.write_sql_queries)

<details>
<summary><strong>📊 Реализованные запросы и оптимизации</strong></summary>

- **👥 Выборки по клиентам:** с учётом исторических цен  
- **📈 Аналитика категорий:** рекурсивные CTE для работы с деревом
- **🏆 Топы товаров:** ранжирование по продажам  
- **⚡ Идеи оптимизации:** closure table, предагрегация, партиционирование, материализованные представления

</details>

---

### ⚡ [Задание 3: REST сервис](https://github.com/jmp-ff25/it_guru_test_task/tree/main/3.order_service)

<details>
<summary><strong>🛠️ Планируемая архитектура API</strong></summary>

- **🎯 Endpoint:** `POST /orders/{order_id}/items` для добавления товара в заказ
- **✅ Валидация:** наличие товара, проверка остатков, фиксация цены на момент заказа  
- **🏛️ Архитектура:** разделение слоёв (контроллеры, сервисы, репозитории)

</details>

---

<div align="center">

### 🎯 **Готово к презентации и обсуждению!**

*Каждое задание содержит подробную документацию с обоснованием принятых решений*

</div>



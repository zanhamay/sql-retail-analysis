
# Анализ интернет-магазина с помощью SQL

## Цель
Проанализировать поведение клиентов и продажи в интернет-магазине на основе базы данных с 6 таблицами: заказы, клиенты, товары, категории, позиции заказов и возвраты.

## Данные
Файлы CSV:
- `customers.csv` — клиенты
- `products.csv` — товары
- `categories.csv` — категории товаров
- `orders.csv` — заказы
- `order_items.csv` — позиции заказов
- `returns.csv` — возвраты

## Инструменты
- SQL (PostgreSQL / SQLite)
- DBeaver

## Примеры задач
1. Общая выручка
2. Топ-5 товаров по количеству продаж
3. Выручка по месяцам
4. Доля возвратов

##  Пример запросов

### Общая выручка
```sql
SELECT ROUND(SUM(quantity * price_each), 2) AS total_revenue
FROM order_items;
```

### Топ-5 товаров
```sql
SELECT p.product_name, SUM(oi.quantity) AS total_sold
FROM order_items oi
JOIN products p ON oi.product_id = p.product_id
GROUP BY p.product_name
ORDER BY total_sold DESC
LIMIT 5;
```

### Продажи по месяцам
```sql
SELECT DATE_TRUNC('month', o.order_date) AS month, 
       ROUND(SUM(oi.quantity * oi.price_each), 2) AS revenue
FROM orders o
JOIN order_items oi ON o.order_id = oi.order_id
GROUP BY month
ORDER BY month;
```

### Доля возвратов
```sql
SELECT 
    COUNT(DISTINCT r.order_id) * 100.0 / COUNT(DISTINCT o.order_id) AS return_rate_percent
FROM orders o
LEFT JOIN returns r ON o.order_id = r.order_id;
```

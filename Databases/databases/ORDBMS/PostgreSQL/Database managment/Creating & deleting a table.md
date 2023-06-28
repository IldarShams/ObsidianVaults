---
tags:
- table
- postgresql_administration
- in_progress
---
---
<h4 style="text-align: center; margin: 1px; padding: 2px">Введение</h4> 
[[ДОПОЛНИТЬ!]]
<h4 style="text-align: center; margin: 1px; padding: 2px">Создание таблицы</h4> 
Для создания таблицы простой таблицы используется следующий запрос:
```PostgreSQL
CREATE TABLE my_first_table ( 
	pk_column serial PRIMARY KEY,
	unique_col bigint UNIQUE,
	first_column text NOT NULL, 
	second_column integer DEFAULT 123,
	product_no integer REFERENCES products (product_no),
	discounted_price numeric,
	price numeric [CONSTRAINT positive_price CHECK (price > 0) | CHECK (price > 0)],
	CONSTRAINT valid_discount CHECK (price > discounted_price)
);
```
Здесь приведены самые частые виды ограничений (PRIMARY KEY, NOT NULL, CONSTRAINT, CHECK, FOREIGN KEY). За дополнительной инфой либо в доку, либо [[ДОПОЛНИТЬ!]]

<h4 style="text-align: center; margin: 1px; padding: 2px">Удаление таблицы</h4> 
```PostgreSQL
DROP TABLE [IF EXISTS] my_first_table;
```

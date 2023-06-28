---
tags:
- psql
- postgresql
- in_progress
---
---
### Команды управления базами данных
#### Список всех БД
```psql
\l
```
Является аналогом:
```PostgreSQL
SELECT datname FROM pg_database;
```

#### Создание базы данных

#### Удаление базы данных


#### Подключение к БД
```psql
\connect <db_name>
\c <db_name>
```

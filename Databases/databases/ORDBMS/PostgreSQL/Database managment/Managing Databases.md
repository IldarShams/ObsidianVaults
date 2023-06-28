---
tags:
- postgresql_administration
- queries
---
---
##### Список баз данных
```PostgreSQL
SELECT datname FROM pg_database;
```

##### Переименование базы данных
```PostgreSQL
ALTER DATABASE old_name RENAME TO new_name;
```

##### Создание базы данных
(см. [[Creating & deleting a database]])

##### Удаление базы данных
```PostgreSQL
DROP DATABASE [ IF EXISTS ] name [ [ WITH ] ( option [, ...] ) ] where option can be: FORCE
```
Например,
```PostgreSQL
DROP DATABASE  IF EXISTS  mydb FORCE
```
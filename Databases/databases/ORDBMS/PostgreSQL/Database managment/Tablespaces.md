---
tags:
- postgresql_administration
- tablespace
- table
- in_progress
---
---
<h4 style="text-align: center; margin: 1px; padding: 2px">Введение</h4> 
Tablespases (ts) определяют место в файловой системе, где хранятся файлы, представляющие собой БД. Создав ts, к нему можно обращаться по имени в запросах.

Tablespaces полезны в случаях:
- если на диске, на котором находится кластер, закончилось место, можно определить новый tablespace для запросов и таблиц?
- если необходимо оптимизировать систему — архивные данные, которые редко используются поместить на HDD (или другое медленное/дешёвое устройство памяти), а часто используемые данные поместить на SSD (быстрое устройство памяти).

Tablespaces используется в таких запросах как CREATE DATABASE, CREATE TABLE, CREATE INDEX или ADD CONSTRAINT.

БД является совокупностью tablespace'ов и строго зависит от них. Если хотя бы один ts будет утерян или не не найден в метаданных БД, то кластер БД будет нечитабелен. Поэтому не рекомендуется определять ts во временных папках или удалять их, не используя средства PostgreSQL.

<h4 style="text-align: center; margin: 1px; padding: 2px">Создание tablespace</h4> 
```PostgreSQL
CREATE TABLESPACE tablespace_name 
[ OWNER { new_owner | CURRENT_ROLE | CURRENT_USER | SESSION_USER } ] 
LOCATION 'directory' 
[ WITH ( tablespace_option = value [, ... ] ) ]
```
где
	tablespace_name — имя ts, по которому можно будет к нему обращаться. Оно не может начинаться с приставки pg_, так как такие имена зарезервированы системой;
	new_owner — имя владельца ts;
	directory — папка, к которой привязана ts;
	tablespace_option — [[ДОПОЛНИТЬ!]]
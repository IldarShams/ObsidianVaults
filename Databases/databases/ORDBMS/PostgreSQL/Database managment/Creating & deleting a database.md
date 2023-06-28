---
tags:
- postgresql_administration
- postgresql
- in_progress
---
---
<h4 style="text-align: center; margin: 1px; padding: 2px">Создание базы данных</h4> 
База данных (БД) — это множество связанных таблиц, хранимых процедур, индексов, представлений и других объектов. Каждый PostgreSQL server может иметь несколько баз данных, с которыми могут работать множество программ. 

Для создания БД необходимо быть superuser'ом или иметь привилегию CREATEDB. Команда создания БД имеет следующий формат.
```PostgreSQL
CREATE DATABASE name 
	[ WITH ] 
	[ OWNER [=] user_name ] 
	[ TEMPLATE [=] template ] 
	[ ENCODING [=] encoding ] 
	[ STRATEGY [=] strategy ]
	[ LOCALE [=] locale ] 
	[ LC_COLLATE [=] lc_collate ] 
	[ LC_CTYPE [=] lc_ctype ] 
	[ ICU_LOCALE [=] icu_locale ] 
	[ LOCALE_PROVIDER [=] locale_provider ] 
	[ COLLATION_VERSION = collation_version ] 
	[ TABLESPACE [=] tablespace_name ] 
	[ ALLOW_CONNECTIONS [=] allowconn ] 
	[ CONNECTION LIMIT [=] connlimit ] 
	[ IS_TEMPLATE [=] istemplate ] 
	[ OID [=] oid ]
```

<h4 style="text-align: center; margin: 1px; padding: 2px">Свойства базы данных</h4> 
- TEMPLATE — шаблон базы данных;
	По умолчанию используется `template1`.
	Если вписать `template0`, то БД будет чистой/нетронутой (pristine), т.е. без определенных пользователем типов данных с неизменёнными системными объектами. БД будет содержать только объекты, предопределённые версией PostgreSQL.
- **ENCODING** — определяет кодировку символов в БД;
	~ DEFAULT - множество символов, определенных в шаблоне (см. свойство TEMPLATE).
	~ SQL_ASCII - множество символов ASCII.
	~...
	[[ДОПОЛНИТЬ!]]
- **COLLATION_VERSION** — определяет правила сортировки/сравнения строк в БД;
	Обычно может быть опущено, в этом случае определяется из операционной системы.
	[[ДОПОЛНИТЬ!]]
- **TABLESPACES** — определяет, где хранятся файлы БД;
	Определяет положение файлов БД в файловой система. Чтобы определить `tablespace_name` см. [[Tablespaces]].
- **OWNER** — определяет роль владельца БД;
	По умолчанию или передаче DEFAULT владельцем будет являться роль пользователя, который исполнил команду создания БД. Для создания БД владельцем, которой будет другая роль, отличная от роли текущего пользователя, надо быть прямым или непрямым членом этой роли или superuser'ом. 
- **STRATEGY** — способ создания БД;
	~ WAL_LOG — БД будет копироваться поблочно, и каждый блок будет отдельно писаться в write-ahead log.
		Наиболее эффективный метод, если шаблон БД маленький. Является значением по умолчанию. 
	~ FILE_COPY — для каждого tablespace'а в wal-логе создается запись, которая соответствует копированию целого каталога/папки в новую локацию в файловой системе. 
		Сильно уменьшает объем wal-лога. Система вынуждена создать чекпоинт в начале создания БД и в конце. Может сильно влиять на производительность системы.
- LC_COLLATE — определяет правила сортировки/сравнения строк в БД;
	Порядок сортировки строк. Влияет на запросы с использованием ORDER BY. По умолчанию используется значение в `template`
- LC_CTYPE — определяет классификацию символов;
- CONNECTION LIMIT — определяет количество подключений;
	По умолчанию -1, т.е. без ограничений. 
- IS_TEMPLATE — определяет является ли БД шаблоном;
	~ TRUE — пользователь с привилегией CREATEDB может скопировать БД;
	~ FALCE — только superuser или владелец может клонировать БД.

<h4 style="text-align: center; margin: 1px; padding: 2px">Удаление базы данных</h4> 
```PostgreSQL
DROP DATABASE [ IF EXISTS ] name [ [ WITH ] ( option [, ...] ) ] where option can be: FORCE
```
Например,
```PostgreSQL
DROP DATABASE  IF EXISTS  mydb FORCE
```

Далее см. [[Creating & deleting a table]].
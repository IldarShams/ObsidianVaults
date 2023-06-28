---
tags:
- git
- git-aliases
---
---
В git можно создать кастомные команды c помощью команды 
```git
git config [<options>]
```
Для просмотра существующих алиасов
```git
git config -l
```
Они будут иметь префикс alias._alias_name_=_alias_
Пример:
	![[Pasted image 20230501190008.png]]

Создание алиасов:
```console
 git config --global alias.co checkout
 git config --global alias.br branch
 git config --global alias.ci commit
 git config --global alias.st status
 git config --global alias.pushghws 'push https://github.com/IldarShams/VKR-web-server.git master'
```
Соответсвенно если вписать в консоль команду
```git
git pushghws
```
То будет исполнятся команда
```git
git push https://github.com/IldarShams/VKR-web-server.git master
```

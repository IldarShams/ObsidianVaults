---
tags:
- python
- requirements
---
---
Для того, чтобы вытащить все *используемые* модули 
```python
1. pip3 install pipreqs
2. pip3 install pip-tools
3. pipreqs --encoding=utf8 --savepath=requirements.in
4. pip-compile
```


Или так:
```python
pip freeze
```
Но такой способ вытащит все зависимсти, в том числе которые не используются

Установка зависимостей
```python
pip3 install -r requirements.txt
```

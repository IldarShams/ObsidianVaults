---
tags:
- javascript
- module
---
---

Оба ключевых слова используются для импорта модулей или частей модуля в коде.

require используется для импорта модулей в Node.js. Имеет следующий синтаксис:
```javascript
const myModule = require('myModule');
```
Если необходимо импортировать только конкретную часть:
```javascript
const myModule = require('myModule');
```
import является более современной версией, чем require. Он используется в [ECMAScript 6](https://github.com/jedrichards/es6#modules) (ES6).
```javascript
import * as myModule from 'myModule';
import {myFunction} from 'myModule';
```
---
tags:
- asynchronous
- async
- await
---
---
Promise это объект, который представляет собой результат работы асинхронной функции и имеет три состояния:
- в ожидании (pending) - результат работы функции ещё не получен;
- выполненный (fulfilled) - функция выполнилась без ошибок и вернула результат;
- отклоненный (rejected) - во время выполнения функции произошло исключение.
В начале объект имеет состояние pending и переходит в одно из двух состояний **_fulfilled_** или __*rejected*__. В этом случае говорят, что promise является **_settled_**, т.е. перешёл в одно из двух соответствующих состояний.
Также есть термин *__resolved__*, что означает, объект был **_settled_** или заблокирован другим объектом promise, например при Promise.race().

<h4 style="text-align: center; margin: 1px; padding: 2px">Конструктор</h4> 
```JavaScript
var prom = new Promise(executor)
```
, где
```JavaScript
function executor(resolveFunc, rejectFunc) {
  // Typically, some asynchronous operation that accepts a callback
}

resolveFunc(value); // call on resolved
rejectFunc(reason); // call on rejected
```
`executor` - функция, принимающая два параметра `resolveFunc` и `rejectFunc`, которые вызываются при успешном выполнении функции и при ошибке соответственно. Обычно `executor`'ом делают функцию, для которой не реализован Promis API.
`value` - значение объекта `prom` при успешном разрешении.
`reason` - причина ошибки, обычно является экземпляром класса `Error`.
Если при вызове `resolveFunc` и `rejectFunc` соответствующие параметры `value` и `reason` опущены, то `prom` разрешится как `undefined`.

Пример
```JavaScript
const promise1 = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve('foo');
  }, 300);
});

promise1.then((value) => {
  console.log(value);
  // Expected output: "foo"
});

console.log(promise1);
// Expected output: [object Promise]

var func = new Promise( (res, rej) =>
    {
        setTimeout(()=>{
            if (Math.random > 0.5)
            {
                res(10);
            } else {
                rej("Пиздец, ошибка!");
            }
        }, 300);
    }
);

func.then(x =>
    {
        x=x + 5;
        console.log(x);
        return x;
    }).catch(e => console.log(e));
```
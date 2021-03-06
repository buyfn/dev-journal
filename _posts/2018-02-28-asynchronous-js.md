---
layout: post
title: Асинхронное программирование
---

Вчера закончил курс &laquo;[асинхронное программирование](https://ru.hexlet.io/courses/js_async)&raquo;. Чем дальше продвигаюсь по курсам хекслета, тем становится сложнее. Последние два задания не мог решить по нескольку дней. Наверное потому, что способ программирования, описаный в этом курсе, сильно не похож на все, что я видел до этого. 

Поясню на примере. Я привык, что код будет выполняться строго в том порядке, в каком он записан в исходнике:
```
console.log('раз');
console.log('два');
console.log('три');
    
// раз
// два
// три
```

В синхронном программировании все так и есть. Но существуют асинхронные функции, которые этот порядок ломают. В ноде, например, есть такая функция `setTimeout`. Она принимает на вход функцию, и количество миллисикунд, после истечения которого, эта функция будет вызвана.

Вот тут `setTimeout` подождет две секунды прежде чем вызвать функцию, печатающую в консоль «два».
```
console.log('раз');
setTimeout(() => console.log('два'), 2000);
console.log('три');
```

Какой будет результат этого кода? Предыдущий опыт подсказывает, что сначала напечатется «раз». Потом две секунды ничего не будет происходить. Потом напечатается «два» и, в самом конце, «три». На деле порядок окажется другим:
```
// раз
// три
// два
```

Так происходит потому что нода, встретив асинхронную функцию, не ждет ее выполнения, а откладывает переданную в нее функцию на потом. А тем временем продолжает выполнять остальные синхронные операции далее по тексту.

Получается что-то вроде очереди в макдаке: когда кто-то заказывает бургер, это не мешает принимать заказы у следующих в очереди и выдавать им еду, если их заказ приготовился раньше.

В джаваскрипте асинхронность дает примерно такую же выгоду как и в фастфуде: если на выполнение какой-то операции требуется много времени, это не будет откладывать выполнение остальных.

Напоследок вот неплохая статья про то, [как в джаваскрипте выполняются асинхронные функции](https://medium.com/@thejasonfile/how-node-and-javascript-handle-asynchronous-functions-7feb9fc8a610).
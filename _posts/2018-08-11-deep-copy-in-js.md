---
layout: post
title: Копирование объектов в джаваскрипте
---

В джаваскрипте можно взять любой объект и поменять в нем какое-нибудь значение.

```
const obj = { a: 1, b: 2, c: 3 }
obj.b = 100
    
console.log(obj)
> { a: 1, b: 100, c: 3 };
```

Такой тип данных, когда значение можно менять, называют «мутабельным». В дословном переводе — «способный мутировать».

Работать с такими данными бывает неудобно. Можно случайно поменять значение переменной в одном месте, и не заметить, как во всей программе будет использоваться новое значение.

Предположим, нужно создать новый объект с такими же данными, как в исходном, но заменить в нем значение одного свойства. Вот что получится, если попробовать сделать это прямолинейным способом.

```
const obj = { a: 1, b: 2, c: 3 }
const objCopy = obj
    
objCopy.c = 100
    
console.log(objCopy)
> { a: 1, b: 2, c: 100 } // то, что и хотели
    
console.log(obj)
> { a: 1, b: 2, c: 100 } // свойство "c" в исходном объекте тоже изменилось
```

Исходный объект тоже почему-то поменялся, хотя его никто, казалось бы, не трогал. Дело в том, что `objCopy` тут на самом деле никакая не копия. В первой строчке, при объявлении `obj` где-то в недрах памяти создается объект, на который идентификатор `obj` будет ссылаться каждый раз, когда потребуется узнать его значение. Строчка `objCopy = obj` не создает новый объект, как можно было бы предположить, а создает новый идентификатор, который ссылается на тот же, уже существующий объект. В результате, при изменении любого из этих значений, будут меняться оба.

Как тогда сделать настоящую копию вместо ссылки на объект? Можно попробовать [спред](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax):

```
const obj = { a: 1, b: 2, c: 3 }
const objCopy = { ...obj }
    
objCopy.c = 100
    
console.log(objCopy)
> { a: 1, b: 2, c: 100 }
    
console.log(obj)
> { a: 1, b: 2, c: 3 } // сработало!
```

На самом деле нет. Спред копирует значение только на первом уровне, а дальше опять будут ссылки. Это называется `shallow copy`. Со вложенными объектами такой способ не работает:

```
const obj = { a: 1, b: { c: 3 } }
const objCopy = { ...obj }
    
objCopy.b.c = 100
    
console.log(objCopy)
> { a: 1, b: { c: 100 } }
    
console.log(obj)
> { a: 1, b: { c: 100 } }
```

Плохая новость в том, что в джаваскрипте нет нативного способа сделать полную копию объекта. Можно воспользоваться хитростью и перегнать объект в жсон и обратно, что, по сути, приведет к созданию нового объекта:

```
const objCopy = JSON.parse(JSON.stringify(obj))
```

К сожалению, при таком преобразовании будут утеряны любые объекты, которые не парсятся в жейсон (например, функции).

Хорошая новость в том, что всегда можно написать при необходимости функцию для копирования объекта самостоятельно или воспользоваться уже готовой из какой-нибудь библиотеки. Ну или писать, где можно, в неизменяемом стиле, чтобы не приходилось задумываться над такого рода проблемами.
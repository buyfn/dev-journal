---
layout: post
title: Braingames, мониторинг качества кода
---

Прошел в проекте еще пару чекпоинтов. На третьем шаге закончилась подготовка и началась активная разработка самой игры.

Код проекта теперь автоматически анализируется при помощи приложений codeclimate, eslint, travis.

### [Codeclimate](https://codeclimate.com)
После каждого коммита на гитхаб codeclimate пытается найти в нем признаки говнокода вроде слишком глубокой вложенности циклов и условий, чрезмерное дублирование и т. п. По итогу анализа выставляет проекту оценку того, насколько легко в нем поддерживать код.

### [Eslint](https://eslint.org)
Проверяет код на соответствие разного рода стилистическим правилам. Нужен, чтобы код выглядил аккуратно и стандартизованно, чтобы нигде не было лишних пробелов, неправильных кавычек, чтобы табулиция была везде одинковая. Можно подключить любые правила, которые посчитаешь нужными.

### [Travis](http://travis-ci.org)
После каждого коммита запускает тесты, чтобы удостовериться, что этот коммит ничего не сломал. В нашем проекте пока никаких тестов нет, вместо них код проходит проверку еслинтом.

После подключения всего вышеперечисленного, на [странице моего проекта в нпм](https://www.npmjs.com/package/braingames-ignat) теперь красуются два бейджика, сообщающих, что код в нем прекрасен.
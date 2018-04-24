---
layout: post
title: Braingames, публикация пакета в нпм и использование стороних библиотек
---


Отправил на проверку два первых шага в проекте.

На первом шаге ученика «проводят за ручку» через последовательность действий, которые нужно совершить для публикации своего пакета на npmjs.com. Действий этих не так уж мало: заполнить конфигурационный файл package.json , добавить в него в качестве зависимостей бабель, установить зависимости. Написать npm-скрипты для запуска бабеля. Написать make-скрипты для запуска npm-скриптов. Опубликовать, наконец, пакет.

Второй шаг покороче: нужно подключить библиотеку для чтения ввода в консоль.

На этом подготовка к написанию непосредственно программы не заканчивается. На следующем шаге предстоит подключить инструменты для автоматической проверки качества кода.

После первых двух шагов получаем опубликованную в каталоге npm текстовую игру. Пока что игра заключается в вводе разных имен в консоль и получении приветствия в ответ. Любители подобного геймплея могут установить пакет себе:
```
npm install -g braingames-ignat
```
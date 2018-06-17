---
layout: post
title: Тестирование фронтенда снепшотами
---

Начал разбираться с автоматическим тестированием фронтенда. Тема оказалось сложной. Гугл выдает огромное число разнообразных библиотек и фреймворков для тестирования, причем не понятно, какие из них взаимозаменяемы, а какие нужно использовать вместе для разных целей. Пока что научился писать простейшие тесты при помощи двух библиотек: [jest](https://facebook.github.io/jest/) и [react-testing-library](https://github.com/kentcdodds/react-testing-library).

Для примера приведу тесты для проверки работоспособности псевдоссылок для входа и регистрации в [моем чатике](http://chat.ignat.co).

В шапке есть две ссылки: Login и Sign up. Логика работы их такова:
- изначально формы логина и регистрации спрятаны,
- по клику на ссылку отображение соответствующей формы меняется со спрятанного на нормальное и наоборот,
- одновременно обе формы не должны отображаться.

Вот полностью код, который проверяет, действительно ли приложение работает, как описано выше:

```
import React from 'react';
import { Simulate, render } from 'react-testing-library';
import { Provider } from 'react-redux';
import { createStore } from 'redux';

import reducer from '../src/client/reducers';
import Main from '../src/client/components/Main.jsx';

const store = createStore(reducer);

test('Renders signup and signin forms', () => {
  const { getByText, container } =
    render(<Provider store={store}><Main /></Provider>);

  expect(container.firstChild).toMatchSnapshot();

  Simulate.click(getByText('Login'));
  expect(container.firstChild).toMatchSnapshot();

  Simulate.click(getByText('Sign up'));
  expect(container.firstChild).toMatchSnapshot();

  Simulate.click(getByText('Sign up'));
  expect(container.firstChild).toMatchSnapshot();
});
```

Работает он так:

1\. Рендерим реакт-компонент при помощи `render` из `react-testing-library`. В результате извлекаем ДОМ-объект `container` и вспомогательную функцию `getByText`, она пригодится чтобы находить ноды с нужным текстом.

2\. Проверяем, что получившийся ДОМ соответствует образцу, хранящемуся в снепшоте. Снепшот — это состояние дерева, которое строит реакт для рендеринга приложения.
При первом запуске тестов строка `expect(container.firstChild).toMatchSnapshot()` создает и сохраняет файл со снепшотом в папку `__snapshots__`, а при каждом следующем запуске сравнивает этот сохраненный снепшот с новым и смотрит, что поменялось. Если ничего не поменялось, то тест считается пройденным. Если поменялось, то тест падает, выводит разницу между снепшотами и предлагает обновить снепшот, если изменение преднамеренное.

Вот кусочек первого снепшота, который показывает форму входа:
```
<div
    class="loginForm"
    id="loginForm"
    style="display: none;"
  >
    <form>
      <label
        class="loginForm__label"
      >
        Username: 
        <input
          id="loginUsername"
          type="text"
          value=""
        />
        <br />
      </label>
      <label
        class="loginForm__label"
      >
        Password: 
        <input
          id="loginPassword"
          type="password"
          value=""
        />
        <br />
      </label>
      <button
        class="btn btn--secondary"
        type="submit"
      >
        Login
      </button>
    </form>
    <span
      class="error"
    />
  </div>
```

Обратите внимание на стиль дива `display: none`. Изначально форма должна быть спрятана, поэтому пока все идет как и запланировано. 

3\. Дальше мы находим ссылки, работу которых пытаемся проверить, имитируем клики по ним, и проверяем, что приложение перерендерилось так, как было задумано.
```
Simulate.click(getByText('Login'));
  expect(container.firstChild).toMatchSnapshot();
```

Если после нажатия ссылки форма не появляется, то тест выведет примерно такую ошибку:
```
FAIL  __tests__/test.jsx
  ● Renders signup and signin forms

    expect(value).toMatchSnapshot()

    Received value does not match stored snapshot "Renders signup and signin forms 2".

    - Snapshot
    + Received

    -     style="display: block;"
    +     style="display: none;"
```

Минуc такого метода со снепшотами в том, что им нельзя тестировать по TDD, когда ты сначала пишешь тесты, а потом удовлетворяющий этим тестам код. Тут нужно сначала написать код, удостовериться каким-то другим способом, что он работает, и только потом можно будет поснимать с него снепшоты. То есть подход помогает удостовериться, что в процессе последующей работы ничего не сломается, но не помогает написать первоначальную реализацию.

Далее предстоит разобраться в том, как писать тесты посложнее: например, как проверить работу риал-тайм приложения с сокетами, и как протестировать фронтенд с бекендом вместе.
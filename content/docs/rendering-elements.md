---
id: rendering-elements
title: Рендеринг элементов
permalink: docs/rendering-elements.html
redirect_from:
  - "docs/displaying-data.html"
prev: introducing-jsx.html
next: components-and-props.html
---

<div class="scary">

>
> These docs are old and won't be updated. Go to [react.dev](https://react.dev/) for the new React docs.
>
> These new documentation pages teach how to write JSX and show it on an HTML page:
>
> - [Writing Markup with JSX](https://react.dev/learn/writing-markup-with-jsx)
> - [Add React to an Existing Project](https://react.dev/learn/add-react-to-an-existing-project#step-2-render-react-components-anywhere-on-the-page)

</div>

Элементы — мельчайшие кирпичики React-приложений.

Элемент описывает то, что вы хотите увидеть на экране:

```js
const element = <h1>Hello, world</h1>;
```

В отличие от DOM-элементов, элементы React — это простые объекты, не отнимающие много ресурсов. React DOM обновляет DOM, чтобы он соответствовал переданным React-элементам.

>**Примечание:**
>
>Элементы можно перепутать с более известной концепцией «компонентов». С компонентами мы ознакомимся в [следующей главе](/docs/components-and-props.html). Элементы — это то, «из чего сделаны» компоненты, и мы рекомендуем вам дочитать эту главу, прежде чем двигаться дальше.

## Рендеринг элемента в DOM {#rendering-an-element-into-the-dom}

Допустим, в вашем HTML-файле есть `<div>`:

```html
<div id="root"></div>
```

Мы назовём его «корневым» узлом DOM, так как React DOM будет управлять его содержимым. 

Обычно в приложениях, написанных полностью на React, есть только один корневой элемент. При встраивании React в существующее приложение вы можете рендерить во столько независимых корневых элементов, во сколько посчитаете нужным.

Для рендеринга React-элемента, сперва передайте DOM-элемент в [`ReactDOM.createRoot()`](/docs/react-dom-client.html#createroot), далее передайте React-элемент в `root.render()`:

`embed:rendering-elements/render-an-element.js`

**[Try it on CodePen](https://codepen.io/gaearon/pen/ZpvBNJ?editors=1010)**

На странице будет написано "Hello, world".

## Обновление элементов на странице {#updating-the-rendered-element}

Элементы React [иммутабельны](https://ru.wikipedia.org/wiki/%D0%9D%D0%B5%D0%B8%D0%B7%D0%BC%D0%B5%D0%BD%D1%8F%D0%B5%D0%BC%D1%8B%D0%B9_%D0%BE%D0%B1%D1%8A%D0%B5%D0%BA%D1%82). После создания элемента нельзя изменить его потомков или атрибуты. Элемент похож на кадр в фильме: он отражает состояние интерфейса в конкретный момент времени.

Пока что мы знаем только один способ обновить интерфейс — это создать новый элемент и передать его в `root.render()`.

Рассмотрим пример с часами:

`embed:rendering-elements/update-rendered-element.js`

**[Try it on CodePen](https://codepen.io/gaearon/pen/gwoJZk?editors=1010)**

В этом примере [`root.render()`](/docs/react-dom.html#render) вызывается каждую секунду с помощью колбэка [`setInterval()`](https://developer.mozilla.org/ru/docs/Web/API/setInterval).

>**Примечание:**
>
>На практике большинство React-приложений вызывают `root.render()` только один раз. В следующем разделе вы узнаете, как можно обновлять интерфейс при помощи [компонента с состоянием](/docs/state-and-lifecycle.html).
>
>Мы рекомендуем не пропускать главы, поскольку каждая следующая глава опирается на знания из предыдущей.

## React обновляет только то, что необходимо {#react-only-updates-whats-necessary}

React DOM сравнивает элемент и его дочернее дерево с предыдущей версией и вносит в DOM только минимально необходимые изменения.

Вы можете убедиться в этом на [последнем примере](https://codepen.io/gaearon/pen/gwoJZk?editors=1010) с помощью инструментов разработки в браузере:

![В DOM видно частичное обновление](../images/docs/granular-dom-updates.gif)

Несмотря на то, что мы создаём элемент, описывающий всё UI-дерево, каждую секунду React DOM изменяет только текстовый узел, содержимое которого изменилось.

Проще описать, как интерфейс выглядит в конкретный момент, чем как он изменяется с течением времени. По нашему опыту, такой подход позволяет избавиться от целого класса ошибок.

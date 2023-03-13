---
id: react-dom
title: ReactDOM
layout: docs
category: Reference
permalink: docs/react-dom.html
---

Пакет `react-dom` предоставляет специфические для DOM методы, которые могут быть использованы на верхнем уровне вашего приложения. Кроме этого, эти методы можно использовать в качестве лазейки, чтобы выйти из модели React, если вам будет это нужно.

```js
import * as ReactDOM from 'react-dom';
```

Если вы используете ES5 с npm, можете написать:

```js
var ReactDOM = require('react-dom');
```

Также `react-dom` предоставляет отдельные модули для клиентских и серверных приложений:
- [`react-dom/client`](https://reactjs.org/docs/react-dom-client.html)
- [`react-dom/server`](/docs/react-dom-server.html)

## Обзор {#overview}

Пакет `react-dom` экспортирует следующие методы:
- [`createPortal()`](#createportal)
- [`flushSync()`](#flushsync)

Следующие методы `react-dom` все еще экспортируются, но считаются устаревшими:
- [`render()`](#render)
- [`hydrate()`](#hydrate)
- [`findDOMNode()`](#finddomnode)
- [`unmountComponentAtNode()`](#unmountcomponentatnode)

> Примечание:
>
> Методы `render` и `hydrate` были заменены на новые [client методы](https://reactjs.org/docs/react-dom-client.html) в React 18. Эти методы будут предупреждать, что ваше приложение будет работать, словно используется версия React 17  (узнайте больше [здесь](https://reactjs.org/link/switch-to-createroot)).

### Поддержка браузерами {#browser-support}

React поддерживает все современные браузеры, хотя для более старых версий браузеров [понадобятся полифилы](/docs/javascript-environment-requirements.html).

> Примечание
>
> Мы не поддерживаем старые браузеры, в которых отсутствует поддержка ES5-методов или микротасков, например Internet Explorer. Возможно, вам удастся обойти эту проблему в старых браузерах, если вы подключите шимы [es5-shim и es5-sham](https://github.com/es-shims/es5-shim). Пожалуйста учтите, что этот путь официально не поддерживается и вы принимаете решение на свой страх и риск.

## Справочник {#browser-support}

### `createPortal()` {#createportal}

> Try the new React documentation for [`createPortal`](https://beta.reactjs.org/reference/react-dom/createPortal).
>
> The new docs will soon replace this site, which will be archived. [Provide feedback.](https://github.com/reactjs/reactjs.org/issues/3308)

```javascript
createPortal(child, container)
```

Создаёт портал. Порталы предоставляют способ [отрендерить дочерние элементы в узле DOM, который существует вне иерархии DOM-компонента](/docs/portals.html).

### `flushSync()` {#flushsync}

> Try the new React documentation for [`flushSync`](https://beta.reactjs.org/reference/react-dom/flushSync).
>
> The new docs will soon replace this site, which will be archived. [Provide feedback.](https://github.com/reactjs/reactjs.org/issues/3308)

```javascript
flushSync(callback)
```

Заставляет React произвести любые обновления внутри колбэка синхронно. При этом DOM обновляется сразу.

```javascript
// Принудительно укажем, что данное обновление состояния должно быть синхронным.
flushSync(() => {
  setCount(count + 1);
});
// К этому моменту DOM уже обновлен.
```

> Примечание:
>
> `flushSync` может сильно влиять на производительность. Используйте в редких случаях.
>
> `flushSync` может заставить Suspense, которые ожидают содержимое, показывать их `fallback` состояние.
>
> `flushSync` может вызывать ожидающие эффекты и синхронно применять любые их обновления перед возвратом.
>
> `flushSync` может вызывать обновления вне колбэка, если это нужно для выполнения обновлений внутри колбэка. Например, если есть ожидающие обновления от клика, React может применить их до того, как применит обновления внутри колбэка.

## Устаревшие методы {#legacy-reference}
### `render()` {#render}

> Try the new React documentation for [`render`](https://beta.reactjs.org/reference/react-dom/render).
>
> The new docs will soon replace this site, which will be archived. [Provide feedback.](https://github.com/reactjs/reactjs.org/issues/3308)

```javascript
render(element, container[, callback])
```

> Примечание:
>
> `render` был заменен на `createRoot` в React 18. Подробнее о [createRoot](https://reactjs.org/docs/react-dom-client.html#createroot).

Рендерит React-элемент в DOM-элемент, переданный в аргумент `container` и возвращает [ссылку](/docs/more-about-refs.html) на компонент (или возвращает `null` для [компонентов без состояния](/docs/components-and-props.html#function-and-class-components)).

Если React-элемент уже был ранее отрендерен в `container`, то повторный вызов произведёт его обновление и изменит соответствующую часть DOM, чтобы она содержала последние изменения.

Если дополнительно был предоставлен колбэк, он будет вызван после того, как компонент отрендерится или обновится.

> Примечание:
>
> `render()` управляет содержимым передаваемого вами узла контейнера. Любые существующие элементы DOM внутри заменяются при первом вызове. Более поздние вызовы используют алгоритм отслеживания изменений React DOM для эффективного обновления.
>
> `render()` не изменяет узел контейнера (изменяет только дочерние элементы контейнера). Если нужно, можно вставить компонент в существующий узел DOM без перезаписи существующих дочерних элементов.
>
> `render()` в настоящее время возвращает ссылку на корневой экземпляр `ReactComponent`. Однако использование этого возвращаемого значения является устаревшим
> и этого следует избегать, потому что в будущих версиях React-компоненты могут отображаться асинхронно в некоторых случаях. Если вам нужна ссылка на корневой экземпляр `ReactComponent`, предпочтительным решением является прикрепить
> [реф (ref) на колбэк](/docs/refs-and-the-dom.html#callback-refs) к корневому элементу.
>
> Использование `render()` для гидратации контейнера, отрендеренного на сервере, объявлено устаревшим. Вместо этого используйте [`hydrateRoot()`](/docs/react-dom-client.html#hydrateroot).

* * *

### `hydrate()` {#hydrate}

> Try the new React documentation for [`hydrate`](https://beta.reactjs.org/reference/react-dom/hydrate).
>
> The new docs will soon replace this site, which will be archived. [Provide feedback.](https://github.com/reactjs/reactjs.org/issues/3308)

```javascript
hydrate(element, container[, callback])
```

> Примечание:
>
> `hydrate` был заменён на `hydrateRoot` в React 18. Подробнее можно узнать в [hydrateRoot](/docs/react-dom-client.html#hydrateroot).

То же, что и [`render()`](#render), но используется для гидратации контейнера, HTML-содержимое которого было отрендерено с помощью [`ReactDOMServer`](/docs/react-dom-server.html). React попытается присоединить обработчики событий к уже существующей разметке.

React ожидает, что отрендеренное содержимое идентично на сервере, и на клиенте. Текстовые различия в контенте могут быть переписаны поверх, но вам следует рассматривать такие нестыковки как ошибки и исправлять их. В режиме разработки React предупреждает о несоответствиях во время гидратации. Нет никаких гарантий, что различия атрибутов будут исправлены в случае несовпадений. Это важно по соображениям производительности, поскольку в большинстве приложений несоответствия встречаются редко, и поэтому проверка всей разметки будет непомерно дорогой.

Если атрибут отдельного элемента или текстовое содержимое неизбежно отличается на сервере и клиенте (например, отметка времени), вы можете отключить предупреждение, добавив к элементу `suppressHydrationWarning={true}`. Он работает только на один уровень в глубину, и задумана как лазейка. Не злоупотребляйте ею. Если это не текстовый контент, React по-прежнему не будет пытаться его исправить, поэтому он может оставаться несовместимым c обновлённым в будущем вариантом.

Если вам нужно намеренно отрендерить что-то другое на сервере и клиенте, вы можете выполнить двухпроходный рендеринг. Компоненты, которые рендерят что-то другое на клиенте, могут читать переменную состояния, такую как `this.state.isClient`, которую вы можете установить в `true` в `componentDidMount()`. Таким образом, начальный этап рендеринга будет отображать тот же контент, что и сервер, избегая несовпадений, но дополнительный этап будет происходить синхронно сразу после гидратации. Обратите внимание, что этот подход замедлит ваши компоненты, потому что они должны рендерится дважды, поэтому используйте его с осторожностью.

Не забывайте про работу с вашим приложением при медленных соединениях. JavaScript-код может загружаться значительно позже исходного HTML-рендеринга, поэтому, если вы рендерите что-то другое только для клиента, переход может вызвать раздражение. Тем не менее, при правильном выполнении может оказаться полезным отобразить «оболочку» приложения на сервере и показать только некоторые дополнительные виджеты на клиенте. Чтобы узнать, как это сделать без проблем с разметкой, обратитесь к объяснению в предыдущем абзаце.

* * *

### `unmountComponentAtNode()` {#unmountcomponentatnode}

> Try the new React documentation for [`unmountComponentAtNode`](https://beta.reactjs.org/reference/react-dom/unmountComponentAtNode).
>
> The new docs will soon replace this site, which will be archived. [Provide feedback.](https://github.com/reactjs/reactjs.org/issues/3308)

```javascript
unmountComponentAtNode(container)
```

> Примечание:
>
> `unmountComponentAtNode` был заменён на `root.unmount()` в React 18. Подробнее можно узнать в [createRoot](/docs/react-dom-client.html#createroot).

Удаляет смонтированный компонент React из DOM и очищает его обработчики событий и состояние. Если в контейнер не было смонтировано ни одного компонента, вызов этой функции ничего не делает. Возвращает `true`, если компонент был размонтирован, и `false` если нет компонента для размонтирования.

* * *

### `findDOMNode()` {#finddomnode}

<<<<<<< HEAD
> Примечание:
=======
> Try the new React documentation for [`findDOMNode`](https://beta.reactjs.org/reference/react-dom/findDOMNode).
>
> The new docs will soon replace this site, which will be archived. [Provide feedback.](https://github.com/reactjs/reactjs.org/issues/3308)

> Note:
>>>>>>> 19aa5b4852c3905757edb16dd62f7e7506231210
>
> `findDOMNode` — это лазейка, используемая для доступа к базовому узлу DOM. В большинстве случаев использование этого метода не рекомендуется, поскольку он нарушает абстракцию компонента. [Метод устарел в `StrictMode`.](/docs/strict-mode.html#warning-about-deprecated-finddomnode-usage)

```javascript
findDOMNode(component)
```

Если этот компонент был смонтирован в DOM, он возвращает соответствующий DOM-элемент браузера. Этот метод полезен для чтения напрямую из DOM (например, чтение значений полей формы) или произведения измерений DOM. **В большинстве случаев, вы можете присоединить реф к узлу DOM и полностью избежать использования `findDOMNode`.** 

Когда компонент рендерится в `null` или `false`, `findDOMNode` возвращает `null`. Когда компонент рендерится в строку, `findDOMNode` возвращает текстовый узел DOM, содержащий это значение. Начиная с React 16, компонент может возвращать фрагмент с несколькими дочерними элементами, и в этом случае `findDOMNode` возвращает DOM-узел, соответствующий первому непустому дочернему элементу.

> Примечание:
>
> `findDOMNode` работает только с смонтированными компонентами (то есть компонентами, которые были размещены в DOM). Если вы попытаетесь вызвать этот метод для компонента, который ещё не был смонтирован (например, вызовете `findDOMNode()` в методе `render()` для компонента, который ещё не был создан), будет сгенерировано исключение.
>
> `findDOMNode` не может быть использован с функциональными компонентами.

---
id: rendering-elements
title: Rendering Elements
permalink: docs/rendering-elements.html
redirect_from: "docs/displaying-data.html"
prev: introducing-jsx.html
next: components-and-props.html
---

Elements are the smallest building blocks of React apps.
앨리먼트란 앱에 있어서 가장 작은 구성 단위입니다.

An element describes what you want to see on the screen:
앨리먼트는 화면에서 보고자 하는것을 설명하죠:

```js
const element = <h1>Hello, world</h1>;
```

Unlike browser DOM elements, React elements are plain objects, and are cheap to create. React DOM takes care of updating the DOM to match the React elements.
리액트 앨리먼트들은 일반적인 객체이며 브라우저의 DOM 앨리먼트와 달리 쉽게 만들 수 있습니다. 리액트 DOM은 DOM이 리액트 앨리먼트와 일치하도록 DOM을 업데이트하게 됩니다.

>**Note:**
>
>One might confuse elements with a more widely known concept of "components". We will introduce components in the [next section](/react/docs/components-and-props.html). Elements are what components are "made of", and we encourage you to read this section before jumping ahead.

>**주석**
>
>어떤분은 앨리먼트를 세간에 잘 알려진 "컴포넌트"와 헷갈리실 수 있습니다. 컴포넌트에 관련해서는 [다음 섹션](/)에서 소개하도록 하겠습니다. 앨리먼트는 컴포넌트를 "구성하는 것"''들입니다. 더 진행하기 전에 여러분이 해당 내용을 먼저 읽으시길 권장드립니다.

## Rendering an Element into the DOM
## 앨리먼트를 DOM에 랜더링하기

Let's say there is a `<div>` somewhere in your HTML file:
예를들어 여러분의 HTML파일 내부 어딘가에 `<div>`가 있다고 가정해봅시다:

```html
<div id="root"></div>
```

We call this a "root" DOM node because everything inside it will be managed by React DOM.
저희는 이것은 "루트(root)" DOM 노드라고 부를것입니다. 왜냐하면 해당 DOM 노드 내부의 내용은 전부 리액트 DOM에 의해 관리될 것이기 때문이죠.

Applications built with just React usually have a single root DOM node. If you are integrating React into an existing app, you may have as many isolated root DOM nodes as you like.
오로지 리액트로만 만들어진 애플리케이션은 보통 하나의 루트 DOM 노드를 가질 것 입니다. 만일 여러분이 이미 존재하는 앱 내부에 리액트를 포함하고 싶다면 원하는만큼의 여러가지 독립된 루트 DOM을 포함해도 상관없습니다.

To render a React element into a root DOM node, pass both to `ReactDOM.render()`:
리액트 앨리먼트를 루트 DOM 노드에 랜더링할때에는 앨리먼트와 루트 노드 모두 `ReactDOM.render()`에 포함시켜 주세요:

```js
const element = <h1>Hello, world</h1>;
ReactDOM.render(
  element,
  document.getElementById('root')
);
```

[Try it on CodePen.](http://codepen.io/gaearon/pen/rrpgNB?editors=1010)
[CodePen에서 해보기](http://codepen.io/gaearon/pen/rrpgNB?editors=1010)

It displays "Hello, world" on the page.
위의 코드는 페이지에 "Hello, world"를 보여줍니다.

## Updating the Rendered Element
## 렌더링된 앨리먼트 업데이트하기

React elements are [immutable](https://en.wikipedia.org/wiki/Immutable_object). Once you create an element, you can't change its children or attributes. An element is like a single frame in a movie: it represents the UI at a certain point in time.
리액트 앨리먼트들은 [불편(immutable)](https://ko.wikipedia.org/wiki/%EB%B6%88%EB%B3%80%EA%B0%9D%EC%B2%B4)합니다. 한번 앨리먼트를 생성하고나면 그 앨리먼트의 자식들 또는 속성값을 변경할 수 없습니다. 앨리먼트는 마치 영화에서 한장의 프레임과 비슷합니다: 특정한 시간 지점에서의 UI를 나타내죠.

With our knowledge so far, the only way to update the UI is to create a new element, and pass it to `ReactDOM.render()`.
지금까지 살펴본 바로는 UI를 업데이트하는 유일한 방법은 새로운 앨리먼트를 만들고 그것을 `ReactDOM.render()`에 대입하는 것이었습니다.

Consider this ticking clock example:
아래의 똑딱 시계 예제를 살펴봅시다:

```js{8-11}
function tick() {
  const element = (
    <div>
      <h1>Hello, world!</h1>
      <h2>It is {new Date().toLocaleTimeString()}.</h2>
    </div>
  );
  ReactDOM.render(
    element,
    document.getElementById('root')
  );
}

setInterval(tick, 1000);
```

[Try it on CodePen.](http://codepen.io/gaearon/pen/gwoJZk?editors=0010)
[CodePen에서 해보기](http://codepen.io/gaearon/pen/gwoJZk?editors=0010)

It calls `ReactDOM.render()` every second from a [`setInterval()`](https://developer.mozilla.org/en-US/docs/Web/API/WindowTimers/setInterval) callback.
매 초마다 [`setInterval()`](https://developer.mozilla.org/en-US/docs/Web/API/WindowTimers/setInterval)콜백으로부터 `ReactDom.render()`을 호출합니다.

>**Note:**
>
>In practice, most React apps only call `ReactDOM.render()` once. In the next sections we will learn how such code gets encapsulated into [stateful components](/react/docs/state-and-lifecycle.html).
>
>We recommend that you don't skip topics because they build on each other.
>**주석**
>
>실제로 대부분의 리액트 앱들은 `ReactDOM.render()`을 한번만 호출합니다. 다음 섹션에서는 이런 코드들이 [stateful components](/react/docs/state-and-lifecycle.html)내부로 캡슐화하는 방법을 배워보도록 하겠습니다.

## React Only Updates What's Necessary
## 리액트는 필요한 것만 업데이트 한다

React DOM compares the element and its children to the previous one, and only applies the DOM updates necessary to bring the DOM to the desired state
리액트 DOM은 이전의 DOM 상태에 대하여 앨리먼트와 그 자식 앨리먼트들을 비교한 후, 원하는 상태의 DOM을 구성하기 위해 필요한 부분에만 DOM 업데이트를 적용합니다

You can verify by inspecting the [last example](http://codepen.io/gaearon/pen/gwoJZk?editors=0010) with the browser tools:
브라우저 툴을 활용하여 [지난 예제](http://codepen.io/gaearon/pen/gwoJZk?editors=0010)를 통해 위의 내용을 확인해보세요:  

![DOM inspector showing granular updates](/react/img/docs/granular-dom-updates.gif)
![세분화 업데이트를 표시하는 DOM 검사기](/react/img/docs/granular-dom-updates.gif)

Even though we create an element describing the whole UI tree on every tick, only the text node whose contents has changed gets updated by React DOM.
매 순간마다 모든 UI트리를 표현하는 엘리먼트를 만들지만, 오직 변경된 내용의 텍스트 노드만 리액트 DOM에 의해 업데이트됩니다.

In our experience, thinking about how the UI should look at any given moment rather than how to change it over time eliminates a whole class of bugs.
저희의 경험에 따르면, 시간변화에 따라 UI가 어떻게 변할지 고려하는 것보다 매 순간마다의 UI의 모습을 고려하고 구상하는것이 버그의 양을 더 줄입니다.

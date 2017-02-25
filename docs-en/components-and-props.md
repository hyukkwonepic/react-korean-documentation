---
id: components-and-props
title: Components and Props
permalink: docs/components-and-props.html
redirect_from:
  - "docs/reusable-components.html"
  - "docs/reusable-components-zh-CN.html"
  - "docs/transferring-props.html"
  - "docs/transferring-props-it-IT.html"
  - "docs/transferring-props-ja-JP.html"
  - "docs/transferring-props-ko-KR.html"
  - "docs/transferring-props-zh-CN.html"
  - "tips/props-in-getInitialState-as-anti-pattern.html"
  - "tips/communicate-between-components.html"
prev: rendering-elements.html
next: state-and-lifecycle.html
---

Components let you split the UI into independent, reusable pieces, and think about each piece in isolation.
컴포넌트는 UI를 독립적이며 재사용가능한 조각으로 나눌 수 있고 각 조각들을 독립적으로 고려할 수 있게 합니다.

Conceptually, components are like JavaScript functions. They accept arbitrary inputs (called "props") and return React elements describing what should appear on the screen
개념적으로 컴포넌트는 자바스크립트 함수과 같습니다. 컴포넌트는 임의의 입력값 ("props"로 불리는)을 받고 화면을 구성하는 리액트 앨리먼트를 반환하죠.

## Functional and Class Components
## 함수형(functional) 컴포넌트와 클래스(class) 컴포넌트

The simplest way to define a component is to write a JavaScript function:
컴포넌트를 정의하는 가장 간단한 방법은 자바스크립트 함수를 작성하는 것 입니다:

```js
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
```

This function is a valid React component because it accepts a single "props" object argument with data and returns a React element. We call such components "functional" because they are literally JavaScript functions.
해당 함수는 한가지 "props"라는 데이터를 포함한 객체를 매개변수로 받고 리액트 엘리먼트를 반환하기 때문에 유효한 리액트 컴포넌트입니다. 저희는 이러한 컴포넌트를 "함수형(functional)"이라고 부르죠. 이런 형식의 컴포넌트는 말 그대로 자바스크립트 함수이기 때문입니다.

You can also use an [ES6 class](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes) to define a component:
위의 방법 대신에 [ES6 클래스(class)](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes)를 사용하여 컴포넌트를 정의할 수 있습니다:

```js
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

The above two components are equivalent from React's point of view.
위에서 소개된 두 컴포넌트들은 리액트의 관점에서는 모두 동일합니다.

Classes have some additional features that we will discuss in the [next sections](/react/docs/state-and-lifecycle.html). Until then, we will use functional components for their conciseness.
클래스는 [다음 섹션](/react/docs/state-and-lifecycle.html)에서 살펴볼 추가적인 기능들을 가지고 있습니다. 그 전까지는 간결함을 위해 함수형 컴포넌트를 사용하도록하죠.

## Rendering a Component
## 컴포넌트 렌더링하기

Previously, we only encountered React elements that represent DOM tags:
지금까지 저희는 DOM 태그를 나타내는 리액트 앨리먼트들만을 살펴보았습니다:

```js
const element = <div />;
```

However, elements can also represent user-defined components:
하지만 유저가 정의한 컴포넌트들 또한 앨리먼트로 나타낼 수 있습니다:

```js
const element = <Welcome name="Sara" />;
```

When React sees an element representing a user-defined component, it passes JSX attributes to this component as a single object. We call this object "props".
리액트가 유저 정의 컴포넌트를 나타내는 앨리먼트를 발견하게되면 해당 컴포넌트에 JSX 속성을 단일 객체로써 전달합니다. 저희는 이 객체를 "props"라고 부릅니다.

For example, this code renders "Hello, Sara" on the page:
예를 들면, 아래의 코드는 "Hello, Sara"를 페이지에 렌더링합니다:

```js{1,5}
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

const element = <Welcome name="Sara" />;
ReactDOM.render(
  element,
  document.getElementById('root')
);
```

[Try it on CodePen.](http://codepen.io/gaearon/pen/YGYmEG?editors=0010)
[CodePen에서 해보기](http://codepen.io/gaearon/pen/YGYmEG?editors=0010)

Let's recap what happens in this example:
해당 예제에서 어떠한 일이 일어나는지 다시한번 살펴봅시다:

1. We call `ReactDOM.render()` with the `<Welcome name="Sara" />` element.
2. React calls the `Welcome` component with `{name: 'Sara'}` as the props.
3. Our `Welcome` component returns a `<h1>Hello, Sara</h1>` element as the result.
4. React DOM efficiently updates the DOM to match `<h1>Hello, Sara</h1>`.

1. `ReactDOM.render()`를 `<Welcome name="Sara" />`엘리먼트와 함께 호출합니다.
2. 리액트는 `{name: 'Sara'}`객체를 props로써 `Welcome` 컴포넌트를 호출합니다.
3. 그러면 `Welcome` 컴포넌트는 결과값으로써 `<h1>Hello, Sara</h1>`앨리먼트를 반환합니다.
4. 리액트 DOM은 DOM이`<h1>Hello, Sara</h1>`과 일치하도록 효과적으로 업데이트합니다.

>**Caveat:**
>
>Always start component names with a capital letter.
>
>For example, `<div />` represents a DOM tag, but `<Welcome />` represents a component and requires `Welcome` to be in scope.

>**주의**
>
>항상 컴포넌트 이름은 영문자 대문자로 시작하세요.
>
>예를들어 `<div />`는 DOM 태그를 나타내지만 `<Welcome />`은 컴포넌트를 나타내며 스코프 내에 `Welcome`을 필요로합니다.

## Composing Components
## 컴포넌트 구성하기

Components can refer to other components in their output. This lets us use the same component abstraction for any level of detail. A button, a form, a dialog, a screen: in React apps, all those are commonly expressed as components.
컴포넌트는 결과물에서 다른 컴포넌트를 참조할 수 있습니다. 이것은 어떠한 모든 세부 수준에서 동일한 컴포넌트 추상화를 사용할 수 있게 합니다. 버튼, 폼, 다이얼로그, 화면 같은 것들은 통상적으로 리액트 앱에서 컴포넌트로써 표현됩니다.

For example, we can create an `App` component that renders `Welcome` many times:
예를들어 `Welcome`을 여러번 렌더링하는 하나의 `App` 컴포넌트를 만들 수 있죠:

```js{8-10}
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

function App() {
  return (
    <div>
      <Welcome name="Sara" />
      <Welcome name="Cahal" />
      <Welcome name="Edite" />
    </div>
  );
}

ReactDOM.render(
  <App />,
  document.getElementById('root')
);
```

[Try it on CodePen.](http://codepen.io/gaearon/pen/KgQKPr?editors=0010)
[CodePen에서 해보기](http://codepen.io/gaearon/pen/KgQKPr?editors=0010)

Typically, new React apps have a single `App` component at the very top. However, if you integrate React into an existing app, you might start bottom-up with a small component like `Button` and gradually work your way to the top of the view hierarchy.
일반적으로 새로운 리액트 앱의 맨 위에는 하나의 `App` 컴포넌트를 가지고있습니다. 하지만 여러분이 리액트를 기존 앱에 통합하는 경우, `Button`과 같은 작은 컴포넌트에서 부터 출발하여 점차적으로 상위 뷰 계층구조로 진행하는 방식으로 작업하면 됩니다.

>**Caveat:**
>
>Components must return a single root element. This is why we added a `<div>` to contain all the `<Welcome />` elements.

>**주의**
>
>컴포넌트는 반드시 단 한개의 루트 엘리먼트를 반환해야합니다. 그렇기 때문에 모든 `<Welcome />`엘리먼트를 `<div>`로 포함했습니다.

## Extracting Components
## 컴포넌트 추출하기

Don't be afraid to split components into smaller components.
컴포넌트를 더 작은 컴포넌트들로 나누는것을 주저하지 마십시오.

For example, consider this `Comment` component:
예를들어 아래의 `Comment` 컴포넌트를 살펴봅시다:

```js
function Comment(props) {
  return (
    <div className="Comment">
      <div className="UserInfo">
        <img className="Avatar"
          src={props.author.avatarUrl}
          alt={props.author.name}
        />
        <div className="UserInfo-name">
          {props.author.name}
        </div>
      </div>
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```

[Try it on CodePen.](http://codepen.io/gaearon/pen/VKQwEo?editors=0010)

It accepts `author` (an object), `text` (a string), and `date` (a date) as props, and describes a comment on a social media website.
위 코드는 `author` (객체), `text` (문자열), `date` (날짜)들을 props로 가지고 있고 어떤 소셜 미디어 웹사이트의 댓글을 표현하고 있습니다.

This component can be tricky to change because of all the nesting, and it is also hard to reuse individual parts of it. Let's extract a few components from it.
이러한 컴포넌트는 네스팅때문에 코드를 수정하기 굉장히 까다롭고, 개별 부분들을 재사용하기도 굉장히 어렵습니다. 여기에서 몇가지의 컴포넌트들을 뽑아보죠.

First, we will extract `Avatar`:
먼저, `Avatar`을 추출할 것입니다:

```js{3-6}
function Avatar(props) {
  return (
    <img className="Avatar"
      src={props.user.avatarUrl}
      alt={props.user.name}
    />
  );
}
```

The `Avatar` doesn't need to know that it is being rendered inside a `Comment`. This is why we have given its prop a more generic name: `user` rather than `author`.
이 `Avatar`는 해당 컴포넌트 자신이 `Comment`내부에 렌더링 된다는것을 알 필요가 없습니다. 그래서 `Avatar` 컴포넌트의 props에 `author`대신에 조금 더 일반적인 이름인 `user`를 사용했습니다.

We recommend naming props from the component's own point of view rather than the context in which it is being used.
저희는 props의 이름을 지을때 컴포넌트가 사용되는 흐름에 따라 짓기보다는 컴포넌트 자신의 관점에서 이름을 짓기를 권장드립니다.

We can now simplify `Comment` a tiny bit:
이제 `Comment`를 조금 더 간단하게 만들 수 있습니다:

```js{5}
function Comment(props) {
  return (
    <div className="Comment">
      <div className="UserInfo">
        <Avatar user={props.author} />
        <div className="UserInfo-name">
          {props.author.name}
        </div>
      </div>
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```

Next, we will extract a `UserInfo` component that renders an `Avatar` next to user's name:
그 다음으로, 유저의 이름 옆에 `Avatar`를 렌더링하는 `UserInfo`컴포넌트를 추출할 것입니다:

```js{3-8}
function UserInfo(props) {
  return (
    <div className="UserInfo">
      <Avatar user={props.user} />
      <div className="UserInfo-name">
        {props.user.name}
      </div>
    </div>
  );
}
```

This lets us simplify `Comment` even further:
이렇게 되면 `Commnet`를 더욱 더 단순하게 만들 수 있습니다:

```js{4}
function Comment(props) {
  return (
    <div className="Comment">
      <UserInfo user={props.author} />
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```

[Try it on CodePen.](http://codepen.io/gaearon/pen/rrJNJY?editors=0010)
[CodePen.](http://codepen.io/gaearon/pen/rrJNJY?editors=0010)

Extracting components might seem like grunt work at first, but having a palette of reusable components pays off in larger apps. A good rule of thumb is that if a part of your UI is used several times (`Button`, `Panel`, `Avatar`), or is complex enough on its own (`App`, `FeedStory`, `Comment`), it is a good candidate to be a reusable component.
컴포넌트를 추출하는 과정은 처음에는 지루한 일로 보일 수 있으나, 재사용 가능한 컴포넌트의 팔레트를 구성해 놓으면 큰 규모의 앱에서 굉장히 유용합니다. 경험적으로 볼 때 여러분의 UI의 일부가 여러번 사용되거나(`Button`, `Panel`, `Avatar`), 그 자체로 충분히 복잡하다면(`App`, `FeedStory`, `Comment`) 이것들은 재사용 가능한 컴포넌트로 만들어 둘만한 좋은 후보입니다.

## Props are Read-Only
## Props는 읽기 전용

Whether you declare a component [as a function or a class](#functional-and-class-components), it must never modify its own props. Consider this `sum` function:
컴포넌트를 [함수형 또는 클래스](/)로 선언할 때, 컴포넌는 절대 스스로의 props를 변경하면 안됩니다. 다음의 `sum` 함수를 살펴보세요:

```js
function sum(a, b) {
  return a + b;
}
```

Such functions are called ["pure"](https://en.wikipedia.org/wiki/Pure_function) because they do not attempt to change their inputs, and always return the same result for the same inputs.
이러한 함수들을 ["순수 함수(pure function)"](https://en.wikipedia.org/wiki/Pure_function)이라고 합니다. 왜냐하면 이러한 함수는 스스로에 대한 입력값을 바꾸려고 하지 않고, 입력값이 같다면 언제나 값은 결과값을 반환하기 때문입니다.

In contrast, this function is impure because it changes its own input:
반대로 다음과 같은 함수는 비순수(非純粹, impure) 합니다. 왜냐하면 스스로의 입력값을 바꾸기 때문이죠:

```js
function withdraw(account, amount) {
  account.total -= amount;
}
```

React is pretty flexible but it has a single strict rule:
리액트는 꽤나 유연하지만 한가지 엄격한 규칙이 존재합니다:

**All React components must act like pure functions with respect to their props.**
**모든 리액트 컴포넌트는 스스로의 props에 대하여 순수 함수로써 작동해야 한다**

Of course, application UIs are dynamic and change over time. In the [next section](/react/docs/state-and-lifecycle.html), we will introduce a new concept of "state". State allows React components to change their output over time in response to user actions, network responses, and anything else, without violating this rule.
물론 애플리케이션 UI는 동적이며 시간이 지남에 따라 변화합니다. [다음 섹션](/)에서는 "state"라는 새로운 개념을 소개드릴것입니다. State는 유저의 행동, 네트워크 응답 및 기타 다른 것들에 대한 응답(response)을 위의 규칙을 위반하지 않으면서 리액트 컴포넌트가 결과값을 변화시킬 수 있게 합니다.

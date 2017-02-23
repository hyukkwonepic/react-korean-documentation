---
id: introducing-jsx
title: Introducing JSX
permalink: docs/introducing-jsx.html
prev: hello-world.html
next: rendering-elements.html
---

Consider this variable declaration:
아래의 변수 선언을 한번 살펴봅시다:

```js
const element = <h1>Hello, world!</h1>;
```

This funny tag syntax is neither a string nor HTML.
이 웃긴 태그 구문은 문자열도 아니고 HTML도 아닙니다.

It is called JSX, and it is a syntax extension to JavaScript. We recommend using it with React to describe what the UI should look like. JSX may remind you of a template language, but it comes with the full power of JavaScript.
바로 JSX라 불리는데요, 이것은 JavaScript의 구문 확장자입니다. UI가 표현되는 방식을 묘사하기 위해 JSX를 리액트와 함께 사용하시기를 권장합니다. JSX가 템플릿 언어를 연상시킬 수 있지만 이는 JavaScript의 모든 기능을 제공합니다.

JSX produces React "elements". We will explore rendering them to the DOM in the [next section](/react/docs/rendering-elements.html). Below, you can find the basics of JSX necessary to get you started.
JSX는 리액트 "앨리먼트"를 생성합니다. [다음 섹션](/)에서 JSX를 DOM에 렌더링하는 것을 살펴볼 것입니다. 아래에서 JSX를 시작하는 데 필요한 기본 사항을 확인할 수 있습니다.

### Embedding Expressions in JSX
### JSX에 표현식 포함하기

You can embed any [JavaScript expression](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Expressions_and_Operators#Expressions) in JSX by wrapping it in curly braces.
여러분은 JSX내부에 모든 종류의 [JavaScript 표현](/)을 중괄호(`{}`)에 감싸서 포함할 수 있습니다.

For example, `2 + 2`, `user.firstName`, and `formatName(user)` are all valid expressions:
예를 들어 `2 + 2`, `user.firstName`, `formatName(user)` 들은 모두 유효한 표현입니다:

```js{12}
function formatName(user) {
  return user.firstName + ' ' + user.lastName;
}

const user = {
  firstName: 'Harper',
  lastName: 'Perez'
};

const element = (
  <h1>
    Hello, {formatName(user)}!
  </h1>
);

ReactDOM.render(
  element,
  document.getElementById('root')
);
```

[Try it on CodePen.](http://codepen.io/gaearon/pen/PGEjdG?editors=0010)
[CodePen에서 시도하기](http://codepen.io/gaearon/pen/PGEjdG?editors=0010)

We split JSX over multiple lines for readability. While it isn't required, when doing this, we also recommend wrapping it in parentheses to avoid the pitfalls of [automatic semicolon insertion](http://stackoverflow.com/q/2846283).
저희는 가독성을 위해 JSX코드를 여러개의 라인으로 나누었습니다. 반드시 해야하는 것은 아니지만, [자동 세미콜론 삽입](/)등과 같은 곤란한 상황을 피하기 위해 JSX 코드를 괄호에 감싸기를 권장합니다.

### JSX is an Expression Too
### JSX도 표현식이다
After compilation, JSX expressions become regular JavaScript objects.
컴파일을 마치게 되면 JSX 표현식은 일반 JavaScript 객체가 됩니다.

This means that you can use JSX inside of `if` statements and `for` loops, assign it to variables, accept it as arguments, and return it from functions:
이 말은 JSX 코드 내에서 `if` 조건문이나 `for` 루프 함수, 변수 지정하기, 매개변수로 인식하기 그리고 함수에서 리턴하기 등을 할 수 있다는 뜻입니다.

```js{3,5}
function getGreeting(user) {
  if (user) {
    return <h1>Hello, {formatName(user)}!</h1>;
  }
  return <h1>Hello, Stranger.</h1>;
}
```

### Specifying Attributes with JSX
### JSX로 속성 지정하기

You may use quotes to specify string literals as attributes:
따옴표를 사용하여 문자열 리터럴을 속성으로 지정할 수 있습니다:

```js
const element = <div tabIndex="0"></div>;
```

You may also use curly braces to embed a JavaScript expression in an attribute:
중괄호를 사용하여 속성으로 JavaScript 표현식을 포함시킬 수도 있습니다.

```js
const element = <img src={user.avatarUrl}></img>;
```

Don't put quotes around curly braces when embedding a JavaScript expression in an attribute. Otherwise JSX will treat the attribute as a string literal rather than an expression. You should either use quotes (for string values) or curly braces (for expressions), but not both in the same attribute.
속성에 JavaScript 표현식을 포함 할 때 중괄호를 따옴표로 묶지 마십시오. 그렇지 않으면 JSX는 속성을 표현식이 아닌 문자열 리터럴로 처리합니다. 따옴표(문자열 값의 경우) 또는 중괄호(표현식의 경우) 중 하나를 사용해야 합니다. 하지만 두가지 모두를 같은 속성에 사용하면 안됩니다.

### Specifying Children with JSX
### JSX로 자식 지정하기
If a tag is empty, you may close it immediately with `/>`, like XML:
만약 태그가 비어있다면 `/>`를 사용하여 바로 닫아야합니다. XML 처럼 말이죠:

```js
const element = <img src={user.avatarUrl} />;
```

JSX tags may contain children:
JSX 태그들은 자식들을 포함할 수 있습니다:

```js
const element = (
  <div>
    <h1>Hello!</h1>
    <h2>Good to see you here.</h2>
  </div>
);
```

>**Caveat:**
>**경고**
>
>Since JSX is closer to JavaScript than HTML, React DOM uses `camelCase` property naming convention instead of HTML attribute names.
>JSX는 HTML보다 JavaScript에 더 가깝기 때문에 React DOM은 HTML속성 대신에 `camelCase`(카멜케이스) 속성 네이밍 컨벤션을 사용합니다
>
>For example, `class` becomes [`className`](https://developer.mozilla.org/en-US/docs/Web/API/Element/className) in JSX, and `tabindex` becomes [`tabIndex`](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/tabIndex).
>예를 들어, `class`는 JSX에서 [`className`](https://developer.mozilla.org/en-US/docs/Web/API/Element/className)이 되고, `tabindex`는 [`tabIndex`](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/tabIndex)가 됩니다.



### JSX Prevents Injection Attacks
### JSX는 인젝션 공격을 예방함

It is safe to embed user input in JSX:
유저 인풋을 JSX에 포함하는 것이 더 안전합니다.

```js
const title = response.potentiallyMaliciousInput;
// This is safe:
const element = <h1>{title}</h1>;
```

By default, React DOM [escapes](http://stackoverflow.com/questions/7381974/which-characters-need-to-be-escaped-on-html) any values embedded in JSX before rendering them. Thus it ensures that you can never inject anything that's not explicitly written in your application. Everything is converted to a string before being rendered. This helps prevent [XSS (cross-site-scripting)](https://en.wikipedia.org/wiki/Cross-site_scripting) attacks.
기본적으로 React DOM 은 JSX에 임베디드 된 모든 값을 렌더링 하기전에 [이스케이프](http://stackoverflow.com/questions/7381974/which-characters-need-to-be-escaped-on-html)하게 됩니다. 그러므로 여러분의 앱 내에 명시적으로 작성되지 않은 것을 인젝션할 수 없습니다. 모든 값들이 렌더링되기 전에 문자열로 변환되죠. 이러한 과정덕분에 [XSS (크로스 사이트 스크립팅)](https://en.wikipedia.org/wiki/Cross-site_scripting) 공격을 막을 수 있게됩니다.

### JSX Represents Objects
### JSX 는 객체를 나타낸다

Babel compiles JSX down to `React.createElement()` calls.
Babel 은 JSX를 `React.createElement()` 호출로 컴파일합니다.

These two examples are identical:
아래의 두 예제는 동일합니다:

```js
const element = (
  <h1 className="greeting">
    Hello, world!
  </h1>
);
```

```js
const element = React.createElement(
  'h1',
  {className: 'greeting'},
  'Hello, world!'
);
```

`React.createElement()` performs a few checks to help you write bug-free code but essentially it creates an object like this:
`React.createElement()`는 버그없는 코드를 작성하는 데 도움이되는 몇 가지 검사를 수행하지만 기본적으로 다음과 같은 객체를 생성합니다.

```js
// 주석: 해당 구조는 간략화 되었습니다
const element = {
  type: 'h1',
  props: {
    className: 'greeting',
    children: 'Hello, world'
  }
};
```

These objects are called "React elements". You can think of them as descriptions of what you want to see on the screen. React reads these objects and uses them to construct the DOM and keep it up to date.
이 객체들은 "리액트 앨리먼트"라고 불립니다. 이것을 화면에서 보고싶은 내용에 대한 설명이라고 생각하시면 됩니다. 리액트는 이 객체들을 읽어들인 후에 DOM을 구성하는데 사용하고 이를 언제나 최신상태로 유지합니다.

We will explore rendering React elements to the DOM in the next section.
다음 섹션에서는 리액트 엘리먼트를 DOM에 렌더링 하는 방법을 살펴보겠습니다.

>**Tip:**
>
>We recommend searching for a "Babel" syntax scheme for your editor of choice so that both ES6 and JSX code is properly highlighted.
>**팁**
>
>ES6 및 JSX 코드가 올바르게 강조될 수 있도록 여러분이 선택한 편집기에서 "Babel" syntax scheme을 검색하시길 추천드립니다.

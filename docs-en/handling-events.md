---
id: handling-events
title: Handling Events
permalink: docs/handling-events.html
prev: state-and-lifecycle.html
next: conditional-rendering.html
redirect_from:
  - "docs/events-ko-KR.html"
---

Handling events with React elements is very similar to handling events on DOM elements. There are some syntactic differences:
리액트 앨리먼트를 사용하여 이벤트를 처리하는 것은 DOM 앨리먼트에서 이벤트를 처리하는 것과 매우 유사합니다. 문법적인 차이점은 다음과 같습니다:

* React events are named using camelCase, rather than lowercase.
* With JSX you pass a function as the event handler, rather than a string.

* 리액트 이벤트는 소문자 대신 camelCase를 사용하여 명명합니다.
* JSX에서는 문자열 대신에 이벤트 핸들러(event handler)로써 함수를 전달합니다.

For example, the HTML:
예를 들어 HTML은:

```html
<button onclick="activateLasers()">
  Activate Lasers
</button>
```

is slightly different in React:
리액트에서 약간 다릅니다:

```js{1}
<button onClick={activateLasers}>
  Activate Lasers
</button>
```

Another difference is that you cannot return `false` to prevent default behavior in React. You must call `preventDefault` explicitly. For example, with plain HTML, to prevent the default link behavior of opening a new page, you can write:
또 다른 차이점은 리액트에서 기본 동작을 방지하기 위해 `false`를 반환할 수 없다는 것 입니다. 이를 위해선 명시적으로 `preventDefault`를 호출해야합니다. 예를 들어, 일반 HTML에서 새 페이지를 여는 기본적인 링크 동작을 막기 위해서는 아래와 같이 작성할 수 있습니다:

```html
<a href="#" onclick="console.log('The link was clicked.'); return false">
  Click me
</a>
```

In React, this could instead be:
반면에 리액트에서는 다음과 같이 할 수 있습니다:

```js{2-5,8}
function ActionLink() {
  function handleClick(e) {
    e.preventDefault();
    console.log('The link was clicked.');
  }

  return (
    <a href="#" onClick={handleClick}>
      Click me
    </a>
  );
}
```

Here, `e` is a synthetic event. React defines these synthetic events according to the [W3C spec](https://www.w3.org/TR/DOM-Level-3-Events/), so you don't need to worry about cross-browser compatibility. See the [`SyntheticEvent`](/react/docs/events.html) reference guide to learn more.
여기서 `e`는 합성 이벤트입니다. 리액트는 [W3C spec](https://www.w3.org/TR/DOM-Level-3-Events/)에 따라 해당 합성 이벤트를 정의하므로 브라우저 간 호환성에 대해 걱정할 필요가 없습니다. 자세한 내용은 [`SyntheticEvent`](/react/docs/events.html)에서 설명서를 참조하십시오.

When using React you should generally not need to call `addEventListener` to add listeners to a DOM element after it is created. Instead, just provide a listener when the element is initially rendered.
리액트를 사용할 때 일반적으로 DOM 엘리먼트가 생성된 후 리스너를 추가하기 위해 `addEventListener`를 호출할 필요가 없습니다. 대신에 앨리먼트가 처음 렌더링 될 때 리스너를 제공하시면 됩니다.

When you define a component using an [ES6 class](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes), a common pattern is for an event handler to be a method on the class. For example, this `Toggle` component renders a button that lets the user toggle between "ON" and "OFF" states:
[ES6 클래스](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes)로 컴포넌트를 정의 할 때 이벤트 핸들러가 클래스의 메소드가 되도록 하는것이 공통적인 패턴입니다. 예를 들어, 아래의 `Toggle` 컴포넌트는 유저가 state를 "켜기"와 "끄기" 상태를 전환 할 수 있는 버튼을 렌더링합니다.

```js{6,7,10-14,18}
class Toggle extends React.Component {
  constructor(props) {
    super(props);
    this.state = {isToggleOn: true};

    // 여기에서의 바인딩(bind)은 콜백에서 `this`가 작동하게 하기 위함입니다.
    this.handleClick = this.handleClick.bind(this);
  }

  handleClick() {
    this.setState(prevState => ({
      isToggleOn: !prevState.isToggleOn
    }));
  }

  render() {
    return (
      <button onClick={this.handleClick}>
        {this.state.isToggleOn ? 'ON' : 'OFF'}
      </button>
    );
  }
}

ReactDOM.render(
  <Toggle />,
  document.getElementById('root')
);
```

[Try it on CodePen.](http://codepen.io/gaearon/pen/xEmzGg?editors=0010)

You have to be careful about the meaning of `this` in JSX callbacks. In JavaScript, class methods are not [bound](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_objects/Function/bind) by default. If you forget to bind `this.handleClick` and pass it to `onClick`, `this` will be `undefined` when the function is actually called.
여러분은 JSX에서 `this`의 의미에 주의하셔야 합니다. JavaScript에서 클래스 메소드는 기본적으로 [바인딩(bound)](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_objects/Function/bind)되지 않습니다. 만약 `this.handleClick`을 바인딩하는 것을 잊고 `onClick`에 전달하면, 함수가 실제로 호출될 때 `this`는 `undefined`가 될 것입니다.

This is not React-specific behavior; it is a part of [how functions work in JavaScript](https://www.smashingmagazine.com/2014/01/understanding-javascript-function-prototype-bind/). Generally, if you refer to a method without `()` after it, such as `onClick={this.handleClick}`, you should bind that method.
이것은 리액트에서만 나타나는 행동이 아닙니다; 이는 [JavaScript에서 함수가 작동하는 방식](https://www.smashingmagazine.com/2014/01/understanding-javascript-function-prototype-bind/)의 일부입니다. 일반적으로 메소드 뒤에`()`없는 메소드를 참조할 때, 예를 들어 `onClick={this.handleClick}`와 같은 경우에, 해당 메소드를 바인드해야합니다.

If calling `bind` annoys you, there are two ways you can get around this. If you are using the experimental [property initializer syntax](https://babeljs.io/docs/plugins/transform-class-properties/), you can use property initializers to correctly bind callbacks:
만약 `bind`를 호출하는것이 짜증나신다면 이것을 해결할 수 있는 두가지 방법이 있습니다. 여러분이 실험적인 [속성 초기화 구문(syntax initializer syntax)](https://babeljs.io/docs/plugins/transform-class-properties/)을 사용하신다면, 속성 초기화를 사용하여 콜백을 올바르게 바인딩할 수 있습니다.

```js{2-6}
class LoggingButton extends React.Component {
  // 해당 구문은 `this`가 handleClick에 바인드 되도록합니다.
  // 주의: 이것은 아직 *실험적인* 구문입니다.
  handleClick = () => {
    console.log('this is:', this);
  }

  render() {
    return (
      <button onClick={this.handleClick}>
        Click me
      </button>
    );
  }
}
```

This syntax is enabled by default in [Create React App](https://github.com/facebookincubator/create-react-app).
해당 구문은 [Create React App](https://github.com/facebookincubator/create-react-app)에서 기본적으로 사용됩니다.

If you aren't using property initializer syntax, you can use an [arrow function](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Functions/Arrow_functions) in the callback:
속성 초기화 구문을 사용하지 않는다면 콜백에서 [화살표 함수](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Functions/Arrow_functions)를 사용할 수 있습니다:

```js{7-9}
class LoggingButton extends React.Component {
  handleClick() {
    console.log('this is:', this);
  }

  render() {
    // This syntax ensures `this` is bound within handleClick
    return (
      <button onClick={(e) => this.handleClick(e)}>
        Click me
      </button>
    );
  }
}
```

The problem with this syntax is that a different callback is created each time the `LoggingButton` renders. In most cases, this is fine. However, if this callback is passed as a prop to lower components, those components might do an extra re-rendering. We generally recommend binding in the constructor or using the property initializer syntax, to avoid this sort of performance problem.
해당 구문의 문제점은 `LoggingButton`을 렌더 할 때마다 다른 콜백이 생성된다는 것 입니다. 대부분의 경우에서는 괜찮으나, 만약 이 콜백이 하위 컴포넌트에 prop으로써 전달되었다면 그 하위 컴포넌트들은 추가적인 재(再)렌더링을 수행 할 수 있습니다. 이러한 류의 성능 문제를 피하려면, 구성자(constructor)에서 바인드 하거나 속성 초기화 구문(syntax initializer syntax)을 사용하시기를 권장드립니다.

---
id: conditional-rendering
title: Conditional Rendering
permalink: docs/conditional-rendering.html
prev: handling-events.html
next: lists-and-keys.html
redirect_from: "tips/false-in-jsx.html"
---

In React, you can create distinct components that encapsulate behavior you need. Then, you can render only some of them, depending on the state of your application.
리액트에서는 필요한 동작을 캡슐화하는 고유한 컴포넌트를 만들 수 있습니다. 그런 다음, 애플리케이션의 state에 따라 일부만 렌더링 할 수 있습니다.

Conditional rendering in React works the same way conditions work in JavaScript. Use JavaScript operators like [`if`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/if...else) or the [conditional operator](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Conditional_Operator) to create elements representing the current state, and let React update the UI to match them.
리액트에서의 조건부 렌더링은 JavaScript에서 조건문이 동작하는 방식과 동일합니다. [`if`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/if...else) 또는 [조건 연산자(conditional operator)](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Conditional_Operator)와 같은 JavaScript 연산자를 사용하여 현재의 state를 나타내는 엘리먼트를 만들고 리액트가 UI를 업데이트하여 일치하도록 합니다.

Consider these two components:
아래의 두가지 컴포넌트를 고려해보겠습니다:

```js
function UserGreeting(props) {
  return <h1>Welcome back!</h1>;
}

function GuestGreeting(props) {
  return <h1>Please sign up.</h1>;
}
```

We'll create a `Greeting` component that displays either of these components depending on whether a user is logged in:
유저가 로그인했는지 여부에 따라서 다음 컴포넌트 중 하나를 표시하는 `Greeting` 컴포넌트를 만들겠습니다:

```javascript{3-7,11,12}
function Greeting(props) {
  const isLoggedIn = props.isLoggedIn;
  if (isLoggedIn) {
    return <UserGreeting />;
  }
  return <GuestGreeting />;
}

ReactDOM.render(
  // Try changing to isLoggedIn={true}:
  <Greeting isLoggedIn={false} />,
  document.getElementById('root')
);
```

[Try it on CodePen.](https://codepen.io/gaearon/pen/ZpVxNq?editors=0011)

This example renders a different greeting depending on the value of `isLoggedIn` prop.
위의 예시에서는 `isLoggedIn` prop값에 따라 다른 인사말을 렌더링합니다.

### Element Variables
### 엘리먼트 변수

You can use variables to store elements. This can help you conditionally render a part of the component while the rest of the output doesn't change.
여러분은 변수를 사용하여 엘리먼트를 저장할 수 있습니다. 이 방법을 통해 결과물의 나머지 부분이 변경되지 않으면서 일부의 컴포넌트가 조건에 따라 렌더링 될 수 있도록 할 수 있습니다.

Consider these two new components representing Logout and Login buttons:
아래에서 로그아웃 및 로그인 버튼을 나타내는 두가지 새로운 컴포넌트들을 살펴보세요:

```js
function LoginButton(props) {
  return (
    <button onClick={props.onClick}>
      Login
    </button>
  );
}

function LogoutButton(props) {
  return (
    <button onClick={props.onClick}>
      Logout
    </button>
  );
}
```

In the example below, we will create a [stateful component](/react/docs/state-and-lifecycle.html#adding-local-state-to-a-class) called `LoginControl`.
아래의 예시에서 `LoginControl`이라고 불리는 [stateful 컴포넌트](/react/docs/state-and-lifecycle.html#adding-local-state-to-a-class)를 만들것입니다.

It will render either `<LoginButton />` or `<LogoutButton />` depending on its current state. It will also render a `<Greeting />` from the previous example:
이 컴포넌트는 현재 state에 따라서 `<LoginButton />` 또는 `<LogoutButton />`중 하나를 렌더링하게 됩니다. 또한 이전 예제에서의 `<Greeting />`도 렌더링합니다:

```javascript{20-25,29,30}
class LoginControl extends React.Component {
  constructor(props) {
    super(props);
    this.handleLoginClick = this.handleLoginClick.bind(this);
    this.handleLogoutClick = this.handleLogoutClick.bind(this);
    this.state = {isLoggedIn: false};
  }

  handleLoginClick() {
    this.setState({isLoggedIn: true});
  }

  handleLogoutClick() {
    this.setState({isLoggedIn: false});
  }

  render() {
    const isLoggedIn = this.state.isLoggedIn;

    let button = null;
    if (isLoggedIn) {
      button = <LogoutButton onClick={this.handleLogoutClick} />;
    } else {
      button = <LoginButton onClick={this.handleLoginClick} />;
    }

    return (
      <div>
        <Greeting isLoggedIn={isLoggedIn} />
        {button}
      </div>
    );
  }
}

ReactDOM.render(
  <LoginControl />,
  document.getElementById('root')
);
```

[Try it on CodePen.](https://codepen.io/gaearon/pen/QKzAgB?editors=0010)

While declaring a variable and using an `if` statement is a fine way to conditionally render a component, sometimes you might want to use a shorter syntax. There are a few ways to inline conditions in JSX, explained below.
변수를 선언하고 `if` 선언문을 사용하는것은 조건부로 컴포넌트를 렌더링하기에 괜찮은 방법이지만, 어떤 경우에는 더 짧은 구문을 사용하고 싶을때가 있습니다. JSX에서 조건을 인라인으로 사용하는 몇가지 방법이 아래에 설명되어있습니다.

### Inline If with Logical && Operator
### 논리 연산자 &&를 사용하여 If를 인라인화 하기

You may [embed any expressions in JSX](/react/docs/introducing-jsx.html#embedding-expressions-in-jsx) by wrapping them in curly braces. This includes the JavaScript logical `&&` operator. It can be handy for conditionally including an element:
표현식을 중괄호에 묶어 [JSX에 삽입](/react/docs/introducing-jsx.html#embedding-expressions-in-jsx)할 수 있습니다. 여기에는 JavaScript의 논리 연산자 `&&`가 포함됩니다. 이렇게 하면 편리하게 엘리먼트를 조건부로 포함할 수 있습니다.

```js{6-10}
function Mailbox(props) {
  const unreadMessages = props.unreadMessages;
  return (
    <div>
      <h1>Hello!</h1>
      {unreadMessages.length > 0 &&
        <h2>
          You have {unreadMessages.length} unread messages.
        </h2>
      }
    </div>
  );
}

const messages = ['React', 'Re: React', 'Re:Re: React'];
ReactDOM.render(
  <Mailbox unreadMessages={messages} />,
  document.getElementById('root')
);
```

[Try it on CodePen.](https://codepen.io/gaearon/pen/ozJddz?editors=0010)

It works because in JavaScript, `true && expression` always evaluates to `expression`, and `false && expression` always evaluates to `false`.
JavaScript에서는 `true && expression`은 언제나 `expression`으로 판명되고, `false && expression`은 언제나 `false`로 판명되기 때문에 위의 구문이 동작할 수 있습니다.

Therefore, if the condition is `true`, the element right after `&&` will appear in the output. If it is `false`, React will ignore and skip it.
따라서, 만약 조건이 `true`이면 `&&`뒤의 표현식은 결과에 표현됩니다. 만약 `false`라면, 리액트는 이를 무시하고 건너뛰게 됩니다.

### Inline If-Else with Conditional Operator
### 조건문을 사용하는 인라인 If-Else

Another method for conditionally rendering elements inline is to use the JavaScript conditional operator [`condition ? true : false`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Conditional_Operator).
엘리먼트를 인라인으로 조건부 렌더링을 하는 또 다른 방법은 JavaScript의 조건부 연산자인 [`condition ? true : false`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Conditional_Operator)를 사용하는 것입니다.

In the example below, we use it to conditionally render a small block of text.
이를 활용하여 아래의 예제에서는 작은 텍스트 블록을 조건부로 렌더링합니다.

```javascript{5}
render() {
  const isLoggedIn = this.state.isLoggedIn;
  return (
    <div>
      The user is <b>{isLoggedIn ? 'currently' : 'not'}</b> logged in.
    </div>
  );
}
```

It can also be used for larger expressions although it is less obvious what's going on:
더 큰 표현에서도 사용할 수 있지만 상황이 덜 명확합니다:

```js{5,7,9}
render() {
  const isLoggedIn = this.state.isLoggedIn;
  return (
    <div>
      {isLoggedIn ? (
        <LogoutButton onClick={this.handleLogoutClick} />
      ) : (
        <LoginButton onClick={this.handleLoginClick} />
      )}
    </div>
  );
}
```

Just like in JavaScript, it is up to you to choose an appropriate style based on what you and your team consider more readable. Also remember that whenever conditions become too complex, it might be a good time to [extract a component](/react/docs/components-and-props.html#extracting-components).
JavaScript에서와 마찬가지로, 여러분 또는 여러분의 팀이 보다 더 나은 가독성을 보인다고 생각하는 방식에 따라 적절한 스타일을 선택하는 것은 여러분에게 달려 있습니다. 또한 조건이 너무 복잡해지면 [컴포넌트를 추출](/react/docs/components-and-props.html#extracting-components)하는것이 좋습니다.

### Preventing Component from Rendering
### 컴포넌트가 렌더링되는것을 방지하기

In rare cases you might want a component to hide itself even though it was rendered by another component. To do this return `null` instead of its render output.
드문 경우이긴 하지만 컴포넌트가 다른 컴포넌트에 의해 렌더링 되었을 때 스스로를 숨길 필요가 있을것입니다. 이 경우에는 렌더링 출력 대신에 `null`을 반환하면 됩니다.

In the example below, the `<WarningBanner />` is rendered depending on the value of the prop called `warn`. If the value of the prop is `false`, then the component does not render:
아래의 예시에서, `<WarningBanner />`는 `warn`이라는 prop값에 따라 렌더링 되고 있습니다. 만약 이 prop값이 `false`라면, 컴포넌트는 렌더링되지 않습니다:

```javascript{2-4,29}
function WarningBanner(props) {
  if (!props.warn) {
    return null;
  }

  return (
    <div className="warning">
      Warning!
    </div>
  );
}

class Page extends React.Component {
  constructor(props) {
    super(props);
    this.state = {showWarning: true}
    this.handleToggleClick = this.handleToggleClick.bind(this);
  }

  handleToggleClick() {
    this.setState(prevState => ({
      showWarning: !prevState.showWarning
    }));
  }

  render() {
    return (
      <div>
        <WarningBanner warn={this.state.showWarning} />
        <button onClick={this.handleToggleClick}>
          {this.state.showWarning ? 'Hide' : 'Show'}
        </button>
      </div>
    );
  }
}

ReactDOM.render(
  <Page />,
  document.getElementById('root')
);
```

[Try it on CodePen.](https://codepen.io/gaearon/pen/Xjoqwm?editors=0010)

Returning `null` from a component's `render` method does not affect the firing of the component's lifecycle methods. For instance, `componentWillUpdate` and `componentDidUpdate` will still be called.
컴포넌트의 `render` 메소드에서 `null`을 반환하는것은 컴포넌트의 라이프사이클 메소드를 실행하는데 영향을 끼치지 않습니다. 예컨데, `componentWillUpdate`와 `componentDidUpdate`는 여전히 호출될 것입니다.

---
id: composition-vs-inheritance
title: Composition vs Inheritance
permalink: docs/composition-vs-inheritance.html
redirect_from: "docs/multiple-components.html"
prev: lifting-state-up.html
next: thinking-in-react.html
---

React has a powerful composition model, and we recommend using composition instead of inheritance to reuse code between components.
리액트는 강력한 컴포지션(composition) 모델이 있기 때문에, 컴포넌트간 코드를 재사용할 때에는 상속보다는 컴포지션을 사용하기를 권장합니다.

In this section, we will consider a few problems where developers new to React often reach for inheritance, and show how we can solve them with composition.
이번 섹션에서는 리액트를 처음 접하는 개발자들이 상속에서 자주 마주치는 몇몇 문제에 대해 다루고, 그 문제들을 컴포지션을 통해 해결하는 방법을 보여드리겠습니다.

## Containment
## 컨테인먼트? 봉쇄? 뭐라고 번역하지... 아놔....;;;; 감싸기/둘러싸기/ 둘러막기/ 박스로만들기/

Some components don't know their children ahead of time. This is especially common for components like `Sidebar` or `Dialog` that represent generic "boxes".
몇몇 컴포넌트들은 자식들에 대해 미리 알 수가 없습니다. 특히 이런 현상은 `Sidebar` 또는 `Dialog`와 같은 전형적인 "박스"를 표현하는 컴포넌트들에게서 일반적입니다.

We recommend that such components use the special `children` prop to pass children elements directly into their output:
이러한 컴포넌트들은 특별 `children` prop을 사용하여 자식 앨리먼트들을 결과값에 바로 전달하기를 권장드립니다:

```js{4}
function FancyBorder(props) {
  return (
    <div className={'FancyBorder FancyBorder-' + props.color}>
      {props.children}
    </div>
  );
}
```

This lets other components pass arbitrary children to them by nesting the JSX:
이렇게 하면 다른 컴포넌트들이 JSX를 중첩하여 임의의 자식들을 전달할 수 있습니다:

```js{4-9}
function WelcomeDialog() {
  return (
    <FancyBorder color="blue">
      <h1 className="Dialog-title">
        Welcome
      </h1>
      <p className="Dialog-message">
        Thank you for visiting our spacecraft!
      </p>
    </FancyBorder>
  );
}
```

[Try it on CodePen.](http://codepen.io/gaearon/pen/ozqNOV?editors=0010)

Anything inside the `<FancyBorder>` JSX tag gets passed into the `FancyBorder` component as a `children` prop. Since `FancyBorder` renders `{props.children}` inside a `<div>`, the passed elements appear in the final output.
`<FancyBorder>`JSX 태그 내부의 모든 내용은 `FancyBorder`컴포넌트의 `children` prop으로써 전달됩니다. `FancyBorder`는 `{props.children}`을 `<div>`내부에 렌더링하기 때문에 전달된 엘리먼트들은 최종 출력 결과에 나타납니다.

While this is less common, sometimes you might need multiple "holes" in a component. In such cases you may come up with your own convention instead of using `children`:
이런 경우는 드물지만, 종종 컴포넌트 내주에 복수의 "구멍"들이 필요할지도 모릅니다. 이런 경우에는 `children` 대신에 여러분이 정한 관례를 사용할 수도 있죠:

```js{5,8,18,21}
function SplitPane(props) {
  return (
    <div className="SplitPane">
      <div className="SplitPane-left">
        {props.left}
      </div>
      <div className="SplitPane-right">
        {props.right}
      </div>
    </div>
  );
}

function App() {
  return (
    <SplitPane
      left={
        <Contacts />
      }
      right={
        <Chat />
      } />
  );
}
```

[Try it on CodePen.](http://codepen.io/gaearon/pen/gwZOJp?editors=0010)

React elements like `<Contacts />` and `<Chat />` are just objects, so you can pass them as props like any other data.
`<Contacts />`또는 `<Chat />`과 같은 리액트 앨리먼트들은 그냥 객체이므로, 다른 데이터들과 마찬가지로 prop으로써 전달할 수 있습니다.

## Specialization
## 전문화(specialization)

Sometimes we think about components as being "special cases" of other components. For example, we might say that a `WelcomeDialog` is a special case of `Dialog`.
종종 컴포넌트들을 다른 컴포넌트들의 "특수한 경우"라고 생각하기도 합니다. 예를 들어 `WelcomeDialog`를 `Dialog`의 특수한 경우라고 말할 수 있죠.

In React, this is also achieved by composition, where a more "specific" component renders a more "generic" one and configures it with props:
리액트에서 이것은 컴포지션으로 달성할 수 있는데, 보다 "구체적인" 컴포넌트가 더 "일반적인" 컴포넌트를 렌더링하고 prop으로써 설정합니다:

```js{5,8,16-18}
function Dialog(props) {
  return (
    <FancyBorder color="blue">
      <h1 className="Dialog-title">
        {props.title}
      </h1>
      <p className="Dialog-message">
        {props.message}
      </p>
    </FancyBorder>
  );
}

function WelcomeDialog() {
  return (
    <Dialog
      title="Welcome"
      message="Thank you for visiting our spacecraft!" />
  );
}
```

[Try it on CodePen.](http://codepen.io/gaearon/pen/kkEaOZ?editors=0010)

Composition works equally well for components defined as classes:
클래스로 정의된 컴포넌트에도 컴포지션을 사용할 수 있습니다:

```js{10,27-31}
function Dialog(props) {
  return (
    <FancyBorder color="blue">
      <h1 className="Dialog-title">
        {props.title}
      </h1>
      <p className="Dialog-message">
        {props.message}
      </p>
      {props.children}
    </FancyBorder>
  );
}

class SignUpDialog extends React.Component {
  constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
    this.handleSignUp = this.handleSignUp.bind(this);
    this.state = {login: ''};
  }

  render() {
    return (
      <Dialog title="Mars Exploration Program"
              message="How should we refer to you?">
        <input value={this.state.login}
               onChange={this.handleChange} />
        <button onClick={this.handleSignUp}>
          Sign Me Up!
        </button>
      </Dialog>
    );
  }

  handleChange(e) {
    this.setState({login: e.target.value});
  }

  handleSignUp() {
    alert(`Welcome aboard, ${this.state.login}!`);
  }
}
```

[Try it on CodePen.](http://codepen.io/gaearon/pen/gwZbYa?editors=0010)

## So What About Inheritance?
## 그렇다면 상속은?

At Facebook, we use React in thousands of components, and we haven't found any use cases where we would recommend creating component inheritance hierarchies.
페이스북에서는 수천개의 컴포넌트에 리액트를 사용하지만, 컴포넌트 상속 계층을 만들어 사용하도록 권장할만한 사용 사례는 발견하지 못했습니다.

Props and composition give you all the flexibility you need to customize a component's look and behavior in an explicit and safe way. Remember that components may accept arbitrary props, including primitive values, React elements, or functions.
Prop과 컴포지션은 명시적이고 안전한 방법으로 컴포넌트의 생김새와 행동을 커스터마이징 하기 위한 모든 유연성을 제공합니다. 컴포넌트는 원시 값(primitive value), 리액트 엘리먼트 또는 함수 등을 임의의 prop으로 수용할 수 있다는 사실을 기억하세요.

If you want to reuse non-UI functionality between components, we suggest extracting it into a separate JavaScript module. The components may import it and use that function, object, or a class, without extending it.
혹시 여러분이 컴포넌트간에 비 UI적 기능을 재사용하고 싶다면, 해당 기능을 별도의 JavaScript 모듈로 분리하시기를 권장드립니다. 컴포넌트를 확장하지 않고 해당 기능의 함수, 객체 또는 클래스를 import하여 사용할 수 있습니다.

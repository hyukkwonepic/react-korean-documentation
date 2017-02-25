---
id: state-and-lifecycle
title: State and Lifecycle
permalink: docs/state-and-lifecycle.html
redirect_from: "docs/interactivity-and-dynamic-uis.html"
prev: components-and-props.html
next: handling-events.html
---

Consider the ticking clock example from [one of the previous sections](/react/docs/rendering-elements.html#updating-the-rendered-element).
[이전 섹션 중 하나](/react/docs/rendering-elements.html#updating-the-rendered-element)에서 살펴본 똑딱이 시계 예제를 생각해봅시다.

So far we have only learned one way to update the UI.
지금까지 저희는 UI를 업데이트하는 한가지 방법만을 배웠습니다.

We call `ReactDOM.render()` to change the rendered output:
`ReactDOM.render()`를 호출해서 렌더링된 결과값을 변경했죠:

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

In this section, we will learn how to make the `Clock` component truly reusable and encapsulated. It will set up its own timer and update itself every second.
이번 섹션에서 저희는 `Clock`컴포넌트를 진정으로 재사용할 수 있고 캡슐화하는 방법을 배울것입니다. 컴포넌트 스스로가 타이머를 작동하고 매초마다 스스로가 업데이트 될 것 입니다.

We can start by encapsulating how the clock looks:
먼저 Clock이 어떻게 보여야할지 캡슐화 하도록 하겠습니다:

```js{3-6,12}
function Clock(props) {
  return (
    <div>
      <h1>Hello, world!</h1>
      <h2>It is {props.date.toLocaleTimeString()}.</h2>
    </div>
  );
}

function tick() {
  ReactDOM.render(
    <Clock date={new Date()} />,
    document.getElementById('root')
  );
}

setInterval(tick, 1000);
```

[Try it on CodePen.](http://codepen.io/gaearon/pen/dpdoYR?editors=0010)

However, it misses a crucial requirement: the fact that the `Clock` sets up a timer and updates the UI every second should be an implementation detail of the `Clock`.
하지만 아직 가장 중요한 사항이 빠져있습니다: `Clock`이 타이머를 작동시키고 매초마다 UI를 업데이트 시키는것은 `Clock`에서 구현되어야 할 사항이라는 것 말이죠.

Ideally we want to write this once and have the `Clock` update itself:
이상적으로 이 코드를 한번만 작성하고 `Clock`컴포넌트가 스스로 업데이트 하도록 하고 싶습니다:

```js{2}
ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```

To implement this, we need to add "state" to the `Clock` component.
이것을 구현하기 위해서 `Clock`컴포넌트에 "state"라는 개념을 사용해야합니다.

State is similar to props, but it is private and fully controlled by the component.
State란 props와 비슷하지만 사적인 성질을 가지고 있으며 컴포넌트에 의해 완전히 제어됩니다.

We [mentioned before](/react/docs/components-and-props.html#functional-and-class-components) that components defined as classes have some additional features. Local state is exactly that: a feature available only to classes.
저희가 [지난번에 언급했듯이](/) 클래스로 정의된 컴포넌트들을 추가적인 기능들을 가지고 있습니다. 로컬 state가 바로 그것입니다: 클래스에서만 가능한 기능입니다.

## Converting a Function to a Class
## 함수를 클래스로 변환하기

You can convert a functional component like `Clock` to a class in five steps:
여러분은 `Clock`과 같은 함수형 컴포넌트를 다섯 단계를 거쳐 클래스로 변환할 수 있습니다:

1. Create an [ES6 class](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes) with the same name that extends `React.Component`.

2. Add a single empty method to it called `render()`.

3. Move the body of the function into the `render()` method.

4. Replace `props` with `this.props` in the `render()` body.

5. Delete the remaining empty function declaration.

1. `React.Component`를 상속(extend)하는, 그리고 같은 이름을 가진 ES6 클래스를 생성합니다.

2. `render()`라고 불리는 빈 단일 메소드를 추가합니다.

3. 함수의 내용을 `render()`내부로 옮깁니다.

4. `render()`내용 내부에서 `props`를 `this.props`로 교체합니다.

5. 남아있는 빈 함수 선언을 지웁니다.


```js
class Clock extends React.Component {
  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.props.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```

[Try it on CodePen.](http://codepen.io/gaearon/pen/zKRGpo?editors=0010)

`Clock` is now defined as a class rather than a function.
`Clock` 은 이제 함수대신에 클래스로 정의되었습니다.

This lets us use additional features such as local state and lifecycle hooks.
이제 클래스로 된 컴포넌트덕분에 로컬 state 또는 라이프사이클(lifecycle) 훅 등의 기능을 사용할 수 있죠.

## Adding Local State to a Class
## 로컬 state를 클래스에 추가하기

We will move the `date` from props to state in three steps:
다음 세 단계를 거쳐 `date`를 props에서 state로 바꾸겠습니다:

1) Replace `this.props.date` with `this.state.date` in the `render()` method:
1) `render()` 메소드 내부의 `this.props.date`를 `this.state.date`로 바꿉니다:

```js{6}
class Clock extends React.Component {
  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```

2) Add a [class constructor](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes#Constructor) that assigns the initial `this.state`:
2) 초기 `this.state`를 지정하는 [클래스 생성자(constructor)](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Classes#Constructor_(생성자)를 추가합니다:

```js{4}
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```

Note how we pass `props` to the base constructor:
기본 생성자에 `props`를 전달하는 방법에 유의하십시오.

```js{2}
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }
```

Class components should always call the base constructor with `props`.
클래스 생성자는 항상 기본 생성자를 `props`로써 호출해야합니다.

3) Remove the `date` prop from the `<Clock />` element:
3) `date` prop을 `<Clock />`앨리먼트에서 제거하세요

```js{2}
ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```

We will later add the timer code back to the component itself.
타이머와 관련한 코드는 나중에 컴포넌트 내부에 포함할 것입니다.

The result looks like this:
결과는 아래와 같은 모습입니다:

```js{2-5,11,18}
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}

ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```

[Try it on CodePen.](http://codepen.io/gaearon/pen/KgQpJd?editors=0010)

Next, we'll make the `Clock` set up its own timer and update itself every second.
이제 `Clock`컴포넌트가 스스로 타이머를 작동하고 매초마다 업데이트하도록 만들것입니다.

## Adding Lifecycle Methods to a Class
## 클래스에 라이프사이클(Lifecycle) 매소드 추가하기

In applications with many components, it's very important to free up resources taken by the components when they are destroyed.
많은 컴포넌트들로 구성된 애플리케이션의 경우에는, 컴포넌트가 사라질 때 컴포넌트가 사용한 리소스들을 비워주는것이 굉장히 중요합니다.

We want to [set up a timer](https://developer.mozilla.org/en-US/docs/Web/API/WindowTimers/setInterval) whenever the `Clock` is rendered to the DOM for the first time. This is called "mounting" in React.
저희는 `Clock`이 DOM에 처음으로 렌더링 될때마다[타이머를 작동](/)시키고자 합니다. 리액트에서는 이것을 "mounting" 이라고 합니다.

We also want to [clear that timer](https://developer.mozilla.org/en-US/docs/Web/API/WindowTimers/clearInterval) whenever the DOM produced by the `Clock` is removed. This is called "unmounting" in React.
Ehgks `Clock`이 DOM에서 사라질 때마다 [타이머를 클리어](https://developer.mozilla.org/en-US/docs/Web/API/WindowTimers/clearInterval)하고자 합니다. 리액트에서는 이것을 "unmounting" 이라고 합니다.

We can declare special methods on the component class to run some code when a component mounts and unmounts:
컴포넌트 클래스의 특별한 메소드들을 사용하여 컴포넌트가 mount되거나 unmount될 때의 코드를 실행 할 수 있습니다.

```js{7-9,11-13}
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  componentDidMount() {

  }

  componentWillUnmount() {

  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```

These methods are called "lifecycle hooks".
이러한 메소드들을 "라이프사이클 훅(lifecycle hooks)"이라고 합니다.

The `componentDidMount()` hook runs after the component output has been rendered to the DOM. This is a good place to set up a timer:
여기의 `componentDidMount()` 훅은 컴포넌트 출력물이 DOM에 렌더링 되고난 후에 실행됩니다. 바로 이곳에서 타이머를 작동시키면 좋겠네요:

```js{2-5}
  componentDidMount() {
    this.timerID = setInterval(
      () => this.tick(),
      1000
    );
  }
```

Note how we save the timer ID right on `this`.
여기서 timerID를 `this`에 바로 저장한것을 유의하세요.

While `this.props` is set up by React itself and `this.state` has a special meaning, you are free to add additional fields to the class manually if you need to store something that is not used for the visual output.
`this.props`가 리액트에 의해 설정되고 `this.state`는 특별한 의미를 가지기는 하지만, 혹시 시각적인 결과물에 사용되지 않는 것을 저장해야 한다면 클래스에 자유롭게 추가 필드를 작성할 수 있습니다.

If you don't use something in `render()`, it shouldn't be in the state.
만일 `render()`에 사용되지 않는다면 state에 없어야 합니다.

We will tear down the timer in the `componentWillUnmount()` lifecycle hook:
이제 타이머를 분해하여 `componentWillUnmount()` 라이프사이클 훅에 포함할 것입니다:

```js{2}
  componentWillUnmount() {
    clearInterval(this.timerID);
  }
```

Finally, we will implement the `tick()` method that runs every second.
마지막으로 매 초당 작동하는 `tick()`메소드를 구현할 것입니다.

It will use `this.setState()` to schedule updates to the component local state:
이는 `this.setState()`를 사용하여 컴포넌트 로컬 state에 대한 업데이트를 예약합니다:

```js{18-22}
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  componentDidMount() {
    this.timerID = setInterval(
      () => this.tick(),
      1000
    );
  }

  componentWillUnmount() {
    clearInterval(this.timerID);
  }

  tick() {
    this.setState({
      date: new Date()
    });
  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}

ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```

[Try it on CodePen.](http://codepen.io/gaearon/pen/amqdNA?editors=0010)

Now the clock ticks every second.
이제 매 초마다 시계가 작동합니다.

Let's quickly recap what's going on and the order in which the methods are called:
메소드가 어떤 순서대로 호출되고 어떤 일이 일어나고 있는지 다시한번 정리해보도록 하겠습니다:

1) When `<Clock />` is passed to `ReactDOM.render()`, React calls the constructor of the `Clock` component. Since `Clock` needs to display the current time, it initializes `this.state` with an object including the current time. We will later update this state.
1) `<Clock />`이 `ReactDOM.render()`에 전달되면 리액트는 `Clock` 컴포넌트의 구성자(constructor)를 호출하게 됩니다. `Clock`은 현재 시간을 표시해야 하기 때문에, 현재시간을 포함한 객체와 함께 `this.state`를 실행합니다. 해당 state는 차후에 업데이트하게 됩니다.

2) React then calls the `Clock` component's `render()` method. This is how React learns what should be displayed on the screen. React then updates the DOM to match the `Clock`'s render output.
2) 그 다음으로 리액트는 `Clock`컴포넌트의 `render()`메소드를 호출합니다. 바로 이 과정에서 리액트가 화면에 무엇을 표시해야하는지 알게되죠. 그리고 리액트는 DOM이 `Clock`의 렌더링 결과값과 같게 되도록 업데이트합니다.

3) When the `Clock` output is inserted in the DOM, React calls the `componentDidMount()` lifecycle hook. Inside it, the `Clock` component asks the browser to set up a timer to call `tick()` once a second.
3) `Clock` 결과값이 DOM에 삽입되면, 리액트는 `componentDidMount` 라이프사이클 훅을 호출하게되죠. 해당 훅 내부에서 `Clock` 컴포넌트는 매 초마다 `tick()`을 호출하는 타이머를 설정하도록 브라우저에게 요청하게 됩니다.

4) Every second the browser calls the `tick()` method. Inside it, the `Clock` component schedules a UI update by calling `setState()` with an object containing the current time. Thanks to the `setState()` call, React knows the state has changed, and calls `render()` method again to learn what should be on the screen. This time, `this.state.date` in the `render()` method will be different, and so the render output will include the updated time. React updates the DOM accordingly.
4) 이제 매 초마다 브라우저는 `tick()`메소드를 호출하게 됩니다. 해당 메소드 내부에서는 `Clock` 컴포넌트가 현재시간을 포함한 객체와 함께 `setState()`를 호출하여 UI의 업데이트를 예약합니다. `setState()`호출 덕분에 리액트는 state가 변했다는것을 알게되고 화면에 무엇이 표시되어야 하는지 알기 위해 `render()`메소드를 다시한번 호출하게 됩니다. 이번에는 `render()`메소드 내부의 `this.state.date`가 이전과 다를것이고, 따라서 렌더링된 결과는 업데이트된 시간을 포함하고 있을겁니다. 리액트는 위와 같은 방식으로 DOM을 업데이트합니다.

5) If the `Clock` component is ever removed from the DOM, React calls the `componentWillUnmount()` lifecycle hook so the timer is stopped.
5) 만약 `Clock`컴포넌트가 DOM에서 완전이 사라지게 되면, 리액트는 `componentWillUnmount()`라이프사이클 훅을 호출하여 타이머가 멈추게 됩니다.

## Using State Correctly
## State를 정확히 사용하기

There are three things you should know about `setState()`.
`setState()`에 관하여 여러분이 알아야 할 점이 세가지 있습니다.

### Do Not Modify State Directly
### State를 직접 수정하지 마십시오

For example, this will not re-render a component:
예를들어, 아래의 경우에는 컴포넌트를 재 렌더링(re-render) 하지 않습니다:

```js
// Wrong
this.state.comment = 'Hello';
```

Instead, use `setState()`:
대신에 `setState()`를 사용하세요:

```js
// Correct
this.setState({comment: 'Hello'});
```

The only place where you can assign `this.state` is the constructor.
`this.state`를 선언하는 유일한 곳은 구성자(constructor) 내부입니다.

### State Updates May Be Asynchronous
### State 업데이트는 비동기적일 수 있습니다

React may batch multiple `setState()` calls into a single update for performance.
리액트는 성능을 위해서 한번의 업데이트에 여러개의 `setState()`호출을 하나로 묶을 수 있습니다.

Because `this.props` and `this.state` may be updated asynchronously, you should not rely on their values for calculating the next state.
`this.props`와 `this.state`가 비동기적으로 업데이트될 수 있기 때문에, 다음 state를 계산하는데 각각의 값에 의존해서는 안됩니다.

For example, this code may fail to update the counter:
예를 들어, 아래의 코드는 counter의 업데이트에 실패할 수 있습니다:

```js
// Wrong
this.setState({
  counter: this.state.counter + this.props.increment,
});
```

To fix it, use a second form of `setState()` that accepts a function rather than an object. That function will receive the previous state as the first argument, and the props at the time the update is applied as the second argument:
이것을 고치기 위해선 객체 대신에 함수를 전달받는 `setState()`의 또 다른 형태를 사용하십시오. 이 함수는 이전 state를 첫번째 매개변수로 전달받고, 업데이트가  적용되는 순간의 props를 두번째 매개변수로 전달 받습니다:

```js
// Correct
this.setState((prevState, props) => ({
  counter: prevState.counter + props.increment
}));
```

We used an [arrow function](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Functions/Arrow_functions) above, but it also works with regular functions:
여기서 [화살표 함수](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Functions/Arrow_functions)를 사용했지만, 일반적인 함수에서도 동작합니다:

```js
// Correct
this.setState(function(prevState, props) {
  return {
    counter: prevState.counter + props.increment
  };
});
```

### State Updates are Merged
### State 업데이트는 병합됩니다

When you call `setState()`, React merges the object you provide into the current state.
`setState()`를 호출할 때, 리액트는 제공하는 객체를 현재의 state에 병합합니다

For example, your state may contain several independent variables:
예를 들어 state에는 여러가지 독립된 변수가 포함되어 있을 수 있습니다:

```js{4,5}
  constructor(props) {
    super(props);
    this.state = {
      posts: [],
      comments: []
    };
  }
```

Then you can update them independently with separate `setState()` calls:
그리고 별도의 `setState()`호출로써 각각을 독립적으로 업데이트 할 수 있습니다.

```js{4,10}
  componentDidMount() {
    fetchPosts().then(response => {
      this.setState({
        posts: response.posts
      });
    });

    fetchComments().then(response => {
      this.setState({
        comments: response.comments
      });
    });
  }
```

The merging is shallow, so `this.setState({comments})` leaves `this.state.posts` intact, but completely replaces `this.state.comments`.
병합이 얕기때문에 `this.setState({comments})`는 `this.state.posts`는 고스란히 남겨두지만 `this.state.comments`는 완전히 대체합니다.

## The Data Flows Down
## 데이터는 아래로 흐른다

Neither parent nor child components can know if a certain component is stateful or stateless, and they shouldn't care whether it is defined as a function or a class.
부모 컴포넌트와 자식 컴포넌트 둘 모두 어떠한 컴포넌트가 stateful한지 stateless한지 알 수 없으며 그 컴포넌트가 어떻게(함수 또는 클래스) 정의되었는지 상관하지 않습니다.


This is why state is often called local or encapsulated. It is not accessible to any component other than the one that owns and sets it.
이러한 이유로 state는 종종 로컬화 또는 캡슐화 되었다고 불립니다. state를 포함하고 정의하고 있는 컴포넌트를 제외하고 어떠한 컴포넌트도 접근할 수 없습니다.

A component may choose to pass its state down as props to its child components:
컴포넌트는 스스로의 state를 자식 컴포넌트에게 props로 전달할 수도 있습니다:

```js
<h2>It is {this.state.date.toLocaleTimeString()}.</h2>
```

This also works for user-defined components:
유저가 정의한 컴포넌트에서도 위와같이 사용될 수 있죠:

```js
<FormattedDate date={this.state.date} />
```

The `FormattedDate` component would receive the `date` in its props and wouldn't know whether it came from the `Clock`'s state, from the `Clock`'s props, or was typed by hand:
`FormattedDate`컴포넌트는 `date`를 스스로의 props에서 받겠지만 이것이 `Clock`의 state에서 왔는지, `Clock`의 props에서 왔는지, 아니면 그저 손으로 작성되었는지는 알 수 없습니다:

```js
function FormattedDate(props) {
  return <h2>It is {props.date.toLocaleTimeString()}.</h2>;
}
```

[Try it on CodePen.](http://codepen.io/gaearon/pen/zKRqNB?editors=0010)

This is commonly called a "top-down" or "unidirectional" data flow. Any state is always owned by some specific component, and any data or UI derived from that state can only affect components "below" them in the tree.
이것은 통상적으로 "하향식(top-down)" 또는 "단방향(unidirectional)" 데이터 플로우라고 불립니다. 모든 state는 반드시 어떤 특정한 컴포넌트에 의해 종속되어 있고, 그 state에서 비롯된 모든 데이터나 UI는 계층 트리에서 오직 "하위의" 컴포넌트에만 영향을 끼칩니다.

If you imagine a component tree as a waterfall of props, each component's state is like an additional water source that joins it at an arbitrary point but also flows down.
컴포넌트 트리를 props의 폭포라고 상상하신다면 각 컴포넌트의 state는 마치 추가적인 수원(水源)과 같고 그 둘은 임의의 지점에서 합쳐지며 결국 아래로 흐르는 것과 같습니다.

To show that all components are truly isolated, we can create an `App` component that renders three `<Clock>`s:
모든 컴포넌트들이 진짜로 독립되어 있는지 관찰하기 위해, 세개의 `<Clock>`들을 렌더링하는 `App`컴포넌트를 만들어봅시다:

```js{4-6}
function App() {
  return (
    <div>
      <Clock />
      <Clock />
      <Clock />
    </div>
  );
}

ReactDOM.render(
  <App />,
  document.getElementById('root')
);
```

[Try it on CodePen.](http://codepen.io/gaearon/pen/vXdGmd?editors=0010)

Each `Clock` sets up its own timer and updates independently.
각의 `Clock`은 각각의 타이머를 가지며 독립적으로 업데이트 됩니다.

In React apps, whether a component is stateful or stateless is considered an implementation detail of the component that may change over time. You can use stateless components inside stateful components, and vice versa.
리액트 앱에서 컴포넌트가 stateful한지 또는 stateless한지는 시간이 경과함에 따라 변하는 컴포넌트 구현의 세부사항으로 간주됩니다. Stateless 컴포넌트가 stateful 컴포넌트 내부에서 사용할 수 있고 그 반대도 가능합니다.

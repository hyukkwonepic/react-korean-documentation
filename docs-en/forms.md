---
id: forms
title: Forms
permalink: docs/forms.html
prev: state-and-lifecycle.html
next: lifting-state-up.html
redirect_from:
  - "tips/controlled-input-null-value.html"
  - "docs/forms-zh-CN.html"
---

HTML form elements work a little bit differently from other DOM elements in React, because form elements naturally keep some internal state. For example, this form in plain HTML accepts a single name:
HTML 폼 엘리먼트는 리액트의 다른 DOM 엘리먼트들과 조금 다르게 동작합니다. 왜냐하면 폼 엘리먼트들은 기본적으로 내부 상태(state)를 포함하고 있기 때문이죠. 예를 들면, 아래의 코드는 일반 HTML로 작성된 이름을 입력하는 폼입니다:

```html
<form>
  <label>
    Name:
    <input type="text" name="name" />
  </label>
  <input type="submit" value="Submit" />
</form>
```

This form has the default HTML form behavior of browsing to a new page when the user submits the form. If you want this behavior in React, it just works. But in most cases, it's convenient to have a JavaScript function that handles the submission of the form and has access to the data that the user entered into the form. The standard way to achieve this is with a technique called "controlled components".
해당 폼은, 유저가 폼을 제출할 때 새로운 페이지로 탐색하는 기본 HTML 폼 행동 방식을 가지고 있습니다. 리액트에서 이런 행동을 원한다면 그냥 사용하시면 원하는대로 작동합니다. 하지만 대부분의 경우에는, 폼의 제출을 제어하고 유저가 폼에 입력한 데이터에 접근할 수 있는 JavaScript 함수를 가지고 있는것이 편리합니다. 이를 위한 표준적인 방법으로 "제어 컴포넌트(controlled components)"라고 불리는 기술을 사용하는 것입니다.

## Controlled Components
## 제어 컴포넌트(Controlled Components)

In HTML, form elements such as `<input>`, `<textarea>`, and `<select>` typically maintain their own state and update it based on user input. In React, mutable state is typically kept in the state property of components, and only updated with [`setState()`](/react/docs/react-component.html#setstate).
HTML에서 `<input>`, `<textarea>`, `<select>`와 같은 폼 엘리먼트들은 일반적으로 스스로의 상태(state) 유지하고, 사용자의 입력에 따라 상태를 업데이트 합니다. 반면 리액트에서는 변형가능한 상태(mutable state)는 통상적으로 컴포넌트의 state 속성에 보관되며 오직 [`setState()`](/)에 의해 업데이트 됩니다.

We can combine the two by making the React state be the "single source of truth". Then the React component that renders a form also controls what happens in that form on subsequent user input. An input form element whose value is controlled by React in this way is called a "controlled component".
저희는 리액트의 state를 "단일 소스 저장소(single source of truth)"로 만들어 두가지를 결합할 수 있습니다. 그리고 폼을 렌더링하는 리액트 컴포넌트는, 유저의 후속적인 입력에 의해 폼에서 일어나는 일 또한 제어합니다. 이러한 방식으로 리액트에 의해 제어되는 입력 폼 엘리먼트를 "제어 컴포넌트(Controlled Components)"라고 부릅니다.

For example, if we want to make the previous example log the name when it is submitted, we can write the form as a controlled component:
예를 들어, 이전 예제에서 폼이 제출될 때 입력된 이름값을 기록(log)하고자 할 때, 폼을 제어 컴포넌트로써 작성할 수 있습니다:

```javascript{4,10-12,24}
class NameForm extends React.Component {
  constructor(props) {
    super(props);
    this.state = {value: ''};

    this.handleChange = this.handleChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
  }

  handleChange(event) {
    this.setState({value: event.target.value});
  }

  handleSubmit(event) {
    alert('A name was submitted: ' + this.state.value);
    event.preventDefault();
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          Name:
          <input type="text" value={this.state.value} onChange={this.handleChange} />
        </label>
        <input type="submit" value="Submit" />
      </form>
    );
  }
}
```

[Try it on CodePen.](https://codepen.io/gaearon/pen/VmmPgp?editors=0010)

Since the `value` attribute is set on our form element, the displayed value will always be `this.state.value`, making the React state the source of truth. Since `handleChange` runs on every keystroke to update the React state, the displayed value will update as the user types.
`value`속성이 폼 엘리먼트에서 지정되었기 때문에 언제나 `this.state.value`의 값이 표시될 것이고, 리액트 state는 단일 소스가 됩니다. 매번 키보드를 두드릴 때마다 `handleChange`가 동작하여 리액트 state를 업데이트하고 유저가 입력한 값이 표시됩니다.

With a controlled component, every state mutation will have an associated handler function. This makes it straightforward to modify or validate user input. For example, if we wanted to enforce that names are written with all uppercase letters, we could write `handleChange` as:
제어 컴포넌트에서는 모든 state 변경은 그와 연계된 핸들러 함수를 가지게 됩니다. 이 핸들러 함수를 통해 쉽게 유저의 입력을 수정하거나 유효성을 검사할 수 있습니다. 예를 들어, 입력된 이름값을 모두 대문자로 변경하려면 `handleChange`를 아래와 같이 작성할 수 있습니다:

```javascript{2}
handleChange(event) {
  this.setState({value: event.target.value.toUpperCase()});
}
```

## The textarea Tag
## textarea 태그

In HTML, a `<textarea>` element defines its text by its children:
HTML에서 `<textarea>` 앨리먼트의 텍스트는 자식을 통해서 정의됩니다:

```html
<textarea>
  Hello there, this is some text in a text area
</textarea>
```

In React, a `<textarea>` uses a `value` attribute instead. This way, a form using a `<textarea>` can be written very similarly to a form that uses a single-line input:
반면 리액트에서, `<textarea>`는 `value` 속성을 사용합니다. 이렇게 하면 `<textarea>`를 사용하는 폼은 한 줄 입력 폼과 굉장히 비슷하게 작성됩니다.

```javascript{4-6,12-14,26}
class EssayForm extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      value: 'Please write an essay about your favorite DOM element.'
    };

    this.handleChange = this.handleChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
  }

  handleChange(event) {
    this.setState({value: event.target.value});
  }

  handleSubmit(event) {
    alert('An essay was submitted: ' + this.state.value);
    event.preventDefault();
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          Name:
          <textarea value={this.state.value} onChange={this.handleChange} />
        </label>
        <input type="submit" value="Submit" />
      </form>
    );
  }
}
```

Notice that `this.state.value` is initialized in the constructor, so that the text area starts off with some text in it.
구성자(constructor)에서 `this.state.value`가 설정된 것을 주목하세요. 이렇게 되면 textarea에서 텍스트가 포함된 상태에서 시작하게 됩니다.

## The select Tag
## 선택(select) 태그

In HTML, `<select>` creates a drop-down list. For example, this HTML creates a drop-down list of flavors:
HTML에서 `<select>`는 드랍 다운 리스트를 만듭니다. 예를 들면 아래의 HTML은 맛 종류의 드랍 다운 리스트를 만듭니다:

```html
<select>
  <option value="grapefruit">Grapefruit</option>
  <option value="lime">Lime</option>
  <option selected value="coconut">Coconut</option>
  <option value="mango">Mango</option>
</select>
```

Note that the Coconut option is initially selected, because of the `selected` attribute. React, instead of using this `selected` attribute, uses a `value` attribute on the root `select` tag. This is more convenient in a controlled component because you only need to update it in one place. For example:
리스트에서 Coconut 옵션이 초기에 선택되어 있습니다. 바로 `selected` 속성 때문이죠. 리액트에서는, `selected` 속성을 사용하는 대신에 루트 `select`태그에 `value`속성을 사용합니다. 이것이 제어 컴포넌트에서는 더욱 편리한데요, 오직 한곳에서만 업데이트를 하면 되기 때문이죠. 예를 들어:

```javascript{4,10-12,24}
class FlavorForm extends React.Component {
  constructor(props) {
    super(props);
    this.state = {value: 'coconut'};

    this.handleChange = this.handleChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
  }

  handleChange(event) {
    this.setState({value: event.target.value});
  }

  handleSubmit(event) {
    alert('Your favorite flavor is: ' + this.state.value);
    event.preventDefault();
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          Pick your favorite La Croix flavor:
          <select value={this.state.value} onChange={this.handleChange}>
            <option value="grapefruit">Grapefruit</option>
            <option value="lime">Lime</option>
            <option value="coconut">Coconut</option>
            <option value="mango">Mango</option>
          </select>
        </label>
        <input type="submit" value="Submit" />
      </form>
    );
  }
}
```

[Try it on CodePen.](https://codepen.io/gaearon/pen/JbbEzX?editors=0010)

Overall, this makes it so that `<input type="text">`, `<textarea>`, and `<select>` all work very similarly - they all accept a `value` attribute that you can use to implement a controlled component.
전반적으로 `<input type="text">`, `<textarea>`, `<select>`들은 모두 굉장히 비슷하게 작동합니다. 이 세가지 모두 `value`속성을 허용하기 때문에 이를 제어 컴포넌트를 구현하는데 사용할 수 있습니다.

## Handling Multiple Inputs
## 다중 입력 처리하기

When you need to handle multiple controlled `input` elements, you can add a `name` attribute to each element and let the handler function choose what to do based on the value of `event.target.name`.
여러개의 제어된 `input`엘리먼트를 처리해야 하는 경우에는 각 엘리먼트에 `name`속성을 추가하고 핸들러 함수가 `event.target.name`값에 따라 어떠한 행동을 할지 결정하게 하면 됩니다.

For example:
예를 들어:

```javascript{15,18,28,37}
class Reservation extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      isGoing: true,
      numberOfGuests: 2
    };

    this.handleInputChange = this.handleInputChange.bind(this);
  }

  handleInputChange(event) {
    const target = event.target;
    const value = target.type === 'checkbox' ? target.checked : target.value;
    const name = target.name;

    this.setState({
      [name]: value
    });
  }

  render() {
    return (
      <form>
        <label>
          Is going:
          <input
            name="isGoing"
            type="checkbox"
            checked={this.state.isGoing}
            onChange={this.handleInputChange} />
        </label>
        <br />
        <label>
          Number of guests:
          <input
            name="numberOfGuests"
            type="number"
            value={this.state.numberOfGuests}
            onChange={this.handleInputChange} />
        </label>
      </form>
    );
  }
}
```

[Try it on CodePen.](https://codepen.io/gaearon/pen/wgedvV?editors=0010)

Note how we used the ES6 [computed property name](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Object_initializer#Computed_property_names) syntax to update the state key corresponding to the given input name:
주어진 input name에 따라 state의 key를 업데이트하기 위해 ES6의 [속성 계산명(computed property name)](/) 구문을 사용한 방법을 살펴보세요:

```js{2}
this.setState({
  [name]: value
});
```

It is equivalent to this ES5 code:
위의 코드는 아래의 ES5코드와 동일합니다:

```js{2}
var partialState = {};
partialState[name] = value;
this.setState(partialState);
```

Also, since `setState()` automatically [merges a partial state into the current state](/react/docs/state-and-lifecycle.html#state-updates-are-merged), we only needed to call it with the changed parts.
또한 `setState()`는 자동적으로 [부분 state를 현재 state로 병합](/)하기 때문에 오직 변경된 부분만 호출하면 됩니다.

## Alternatives to Controlled Components
## 제어 컴포넌트에 대한 대안

It can sometimes be tedious to use controlled components, because you need to write an event handler for every way your data can change and pipe all of the input state through a React component. This can become particularly annoying when you are converting a preexisting codebase to React, or integrating a React application with a non-React library. In these situations, you might want to check out [uncontrolled components](/react/docs/uncontrolled-components.html), an alternative technique for implementing input forms.
데이터를 변경하는 모든 방법에 대해 이벤트 핸들러를 작성하고 input state를 리액트 컴포넌트를 통하도록 해야하기 때문에 제어 컴포넌트를 사용하는 것이 지루할 수 있습니다. 특히 이 과정은 이미 존재하는 리액트 코드베이스를 변환하거나 리액트 애플리케이션을 비(非)리액트 라이브러리와 통합할 때 굉장히 성가신 일이 될 수 있습니다. 이러한 상황에서는 입력 폼을 구현하는 대체 기술인 [비제어 컴포넌트](/)에 대해 살펴보세요.

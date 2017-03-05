---
id: lifting-state-up
title: Lifting State Up
permalink: docs/lifting-state-up.html
prev: state-and-lifecycle.html
next: composition-vs-inheritance.html
redirect_from:
  - "docs/flux-overview.html"
  - "docs/flux-todo-list.html"
---

Often, several components need to reflect the same changing data. We recommend lifting the shared state up to their closest common ancestor. Let's see how this works in action.
종종 여러 컴포넌트들은 동일한 변경 데이터를 반영해야 합니다. 그래서 공유되는 state를 가장 가까운 공통 조상으로 lift 해야합니다.

In this section, we will create a temperature calculator that calculates whether the water would boil at a given temperature.
이번 섹션에서는 주어진 온도에서 물의 비등 여부를 계산하는 온도 계산기를 만들것입니다.

We will start with a component called `BoilingVerdict`. It accepts the `celsius` temperature as a prop, and prints whether it is enough to boil the water:
`BoilingVerdict`라고 불리는 컴포넌트로부터 시작할 것입니다. 이 컴포넌트는 `celsius` 온도를 prop으로 승인하고, 물을 끓이기에 충분한지에 대한 여부를 반환합니다.

```js{3,5}
function BoilingVerdict(props) {
  if (props.celsius >= 100) {
    return <p>The water would boil.</p>;
  }
  return <p>The water would not boil.</p>;
}
```

Next, we will create a component called `Calculator`. It renders an `<input>` that lets you enter the temperature, and keeps its value in `this.state.value`.
다음으로 `Calculator`라는 컴포넌트를 생성합니다. 이 컴포넌트는 온도를 입력할 수 있는 `<input>`을 렌더링하고 그 값을 `this.state.value`에 저장합니다.

Additionally, it renders the `BoilingVerdict` for the current input value.
추가로, 현재 입력 값에 대해 `BoilingVerdict`를 렌더링합니다.

```js{5,9,13,17-21}
class Calculator extends React.Component {
  constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
    this.state = {value: ''};
  }

  handleChange(e) {
    this.setState({value: e.target.value});
  }

  render() {
    const value = this.state.value;
    return (
      <fieldset>
        <legend>Enter temperature in Celsius:</legend>
        <input
          value={value}
          onChange={this.handleChange} />
        <BoilingVerdict
          celsius={parseFloat(value)} />
      </fieldset>
    );
  }
}
```

[Try it on CodePen.](http://codepen.io/gaearon/pen/Gjxgrj?editors=0010)

## Adding a Second Input
## 두번째 Input을 추가하기

Our new requirement is that, in addition to a Celsius input, we provide a Fahrenheit input, and they are kept in sync.
새로이 추가되는 요구사항은, 섭씨(Celsius) input뿐만 아니라 화씨(Fahrenheit) input을 제공하는 것이고 이 둘은 서로 동기화 되어야 한다는 것입니다.

We can start by extracting a `TemperatureInput` component from `Calculator`. We will add a new `scale` prop to it that can either be `"c"` or `"f"`:
`Calculator`에서 `TemperatureInput`컴포넌트를 추출하는 것으로부터 시작합니다. 새로운 `scale`이라는 prop을 추가하고 이것은 `"c"` 또는 `"f"`가 될 수 있습니다:

```js{1-4,19,22}
const scaleNames = {
  c: 'Celsius',
  f: 'Fahrenheit'
};

class TemperatureInput extends React.Component {
  constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
    this.state = {value: ''};
  }

  handleChange(e) {
    this.setState({value: e.target.value});
  }

  render() {
    const value = this.state.value;
    const scale = this.props.scale;
    return (
      <fieldset>
        <legend>Enter temperature in {scaleNames[scale]}:</legend>
        <input value={value}
               onChange={this.handleChange} />
      </fieldset>
    );
  }
}
```

We can now change the `Calculator` to render two separate temperature inputs:
그리고 두가지 별도의 온도 input을 렌더링하기 위해 `Calculator`를 변경합니다:

```js{5,6}
class Calculator extends React.Component {
  render() {
    return (
      <div>
        <TemperatureInput scale="c" />
        <TemperatureInput scale="f" />
      </div>
    );
  }
}
```

[Try it on CodePen.](http://codepen.io/gaearon/pen/NRrzOL?editors=0010)

We have two inputs now, but when you enter the temperature in one of them, the other doesn't update. This contradicts our requirement: we want to keep them in sync.
이제 저희는 두개의 input을 가지고 있지만, 한 input에 온도를 입력을 해도 다른 input이 업데이트 되지 않습니다. 이는 요구사항에 부합하지 않습니다: 저희는 두 input이 동기화가 되길 원하죠.

We also can't display the `BoilingVerdict` from `Calculator`. The `Calculator` doesn't know the current temperature because it is hidden inside the `TemperatureInput`.
또한 `Calculator`에서 `BoilingVerdict`를 표시할 수 없습니다. 현재 온도는 `TemperatureInput`내부에 숨겨져 있기 때문에 `Calculator`는 그 온도를 모릅니다.

## Writing Conversion Functions
## 변환 함수 작성하기

First, we will write two functions to convert from Celsius to Fahrenheit and back:
먼저, 섭씨(Celsius)를 화씨(Fahrenheit)로 변환하는 함수를 작성하겠습니다:

```js
function toCelsius(fahrenheit) {
  return (fahrenheit - 32) * 5 / 9;
}

function toFahrenheit(celsius) {
  return (celsius * 9 / 5) + 32;
}
```

These two functions convert numbers. We will write another function that takes a string `value` and a converter function as arguments and returns a string. We will use it to calculate the value of one input based on the other input.
위의 두 함수는 숫자를 변환합니다. 저희는 문자열 `value`와 변환 함수를 매개변수로써 전달받아 문자열을 반환하는 또 다른 함수를 작성할 것입니다. 한 input의 값을 다른 input의 값을 통해 계산하는데 사용할 것입니다.

It returns an empty string on an invalid `value`, and it keeps the output rounded to the third decimal place:
이 함수는 `value`가 유효하지 않다면 빈 문자열을 반환하고, 결과물을 세 번째 소수점 이하로 반올림합니다:

```js
function tryConvert(value, convert) {
  const input = parseFloat(value);
  if (Number.isNaN(input)) {
    return '';
  }
  const output = convert(input);
  const rounded = Math.round(output * 1000) / 1000;
  return rounded.toString();
}
```

For example, `tryConvert('abc', toCelsius)` returns an empty string, and `tryConvert('10.22', toFahrenheit)` returns `'50.396'`.
예를 들어, `tryConvert('abc', toCelsius)`는 빈 문자열을 반환하고, `tryConvert('10.22', toFahrenheit)`는 `'50.396'`을 반환합니다.

## Lifting State Up
## State를 리프팅(Lifting Up)하기

Currently, both `TemperatureInput` components independently keep their values in the local state:
현재 두 `TemperatureInput` 컴포넌트는 모두 로컬 state에서 독립적으로 값을 유지하고 있습니다:

```js{5,9,13}
class TemperatureInput extends React.Component {
  constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
    this.state = {value: ''};
  }

  handleChange(e) {
    this.setState({value: e.target.value});
  }

  render() {
    const value = this.state.value;
```

However, we want these two inputs to be in sync with each other. When we update the Celsius input, the Fahrenheit input should reflect the converted temperature, and vice versa.
하지만 저희는 지금 이 두 input들이 서로 동기화되기를 원합니다. 썹씨(celcius) input을 업데이트하면 화씨(fahrenheit) input은 변환된 온도를 반영해야합니다. 그 반대의 경우도 마찬가지죠.

In React, sharing state is accomplished by moving it up to the closest common ancestor of the components that need it. This is called "lifting state up". We will remove the local state from the `TemperatureInput` and move it into the `Calculator` instead.
리액트에서 state의 공유는, 필요로하는 컴포넌트에서 가장 가까운 공통 조상으로 이동하여 성취할 수 있습니다. 이것을 "state를 리프팅(lift up)한다"고 합니다. 이제 로컬 state를 `TemperatureInput`에서 제거하여 `Calculator`로 옮길 것 입니다.

If the `Calculator` owns the shared state, it becomes the "source of truth" for the current temperature in both inputs. It can instruct them both to have values that are consistent with each other. Since the props of both `TemperatureInput` components are coming from the same parent `Calculator` component, the two inputs will always be in sync.
그렇게 공유되는 state를 `Calculator`가 가지게 되면, 두개의 현재 온도 input의 "단일 소스 저장소(source of truth)"가 됩니다. 이 state는 두 input 모두 서로에게 일관성있게 같은 값을 가지도록 지시하게되죠. 두 `TemperatureInput`컴포넌트 모두 같은 부모 컴포넌트인 `Calculator`에게서 props을 받게 되면, 두 input들은 언제나 공유되는 상태를 유지합니다.

Let's see how this works step by step.
이제 차례차례 이것이 어떻게 작동하는지 살펴봅시다.

First, we will replace `this.state.value` with `this.props.value` in the `TemperatureInput` component. For now, let's pretend `this.props.value` already exists, although we will need to pass it from the `Calculator` in the future:
먼저, `TemperatureInput`컴포넌트에서 `this.state.value`를 `this.props.value`로 대체할 것입니다. 지금은 일단 `this.props.value`가 이미 존재한다고 가정해봅시다. 하지만 곧 `Calculator`에서 전달해야합니다.

```js{3}
  render() {
    // Before: const value = this.state.value;
    const value = this.props.value;
```

We know that [props are read-only](/react/docs/components-and-props.html#props-are-read-only). When the `value` was in the local state, the `TemperatureInput` could just call `this.setState()` to change it. However, now that the `value` is coming from the parent as a prop, the `TemperatureInput` has no control over it.
저희는 [props가 읽기 전용](/)이라는 것을 알고있습니다. `value`가 로컬 state에 있었을 때에는 `TemperatureInput`이 이를 변경하기 위해선 단순히 `this.setState()`를 호출하기만 하면 되었습니다. 하지만 이제 `value`값을 props로써 부모에게서 전달받기 때문에, `TemperatureInput`는 아무런 제어권이 없죠.

In React, this is usually solved by making a component "controlled". Just like the DOM `<input>` accepts both a `value` and an `onChange` prop, so can the custom `TemperatureInput` accept both `value` and `onChange` props from its parent `Calculator`.
리액트에서 이것은 보통 컴포넌트를 "제어"함으로써 해결됩니다. DOM `<input>`이 `value`와 `onChange`prop들을 받는 것처럼, 임의의 `TemperatureInput` 또한 부모인 `Calculator`로부터 `value`와 `onChange`prop을 전달받을 수 있습니다.

Now, when the `TemperatureInput` wants to update its temperature, it calls `this.props.onChange`:
이제 `TemperatureInput`이 온도를 업데이트 하려 할때, `this.props.onChange`를 호출합니다:

```js{3}
  handleChange(e) {
    // Before: this.setState({value: e.target.value});
    this.props.onChange(e.target.value);
```

Note that there is no special meaning to either `value` or `onChange` prop names in custom components. We could have called them anything else, although this is a common convention.
커스텀 컴포넌트의 `value`또는 `onChange`라는 prop 이름은 특별한 의미가 없다는걸 기억하세요. 이 prop들은 어떠한 이름으로도 불릴 수 있습니다. 이 이름들은 단지 일반적인 관습일 뿐이죠:

The `onChange` prop will be provided together with the `value` prop by the parent `Calculator` component. It will handle the change by modifying its own local state, thus re-rendering both inputs with the new values. We will look at the new `Calculator` implementation very soon.
`onChange` prop은 `value` prop과 함께 부모 컴포넌트인 `Calculator`로 부터 제공받습니다. 해당 컴포넌트는 스스로의 로컬 state를 바꿔가며 변경 사항을 다룰것이, 새로운 값과 함께 두 input들을 재(再)렌더링 합니다. 새로운 `Calculator`의 구현은 곧 살펴보도록 하겠습니다.

Before diving into the changes in the `Calculator`, let's recap our changes to the `TemperatureInput` component. We have removed the local state from it, and instead of reading `this.state.value`, we now read `this.props.value`. Instead of calling `this.setState()` when we want to make a change, we now call `this.props.onChange()`, which will be provided by the `Calculator`:
본격적으로 `Calculator`의 변경을 하기 전에, `TemperatureInput`의 변경 내용을 다시 한번 살펴봅시다. 이 컴포넌트에서 로컬 state를 제거했고, `this.state.value`대신에 `this.props.value`를 읽습니다. 변경을 위해서는 `this.setState()`를 호출하지 않고, 이제는 `Calculator`로부터 제공받을 `this.props.onChange()`를 호출합니다:

```js{8,12}
class TemperatureInput extends React.Component {
  constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
  }

  handleChange(e) {
    this.props.onChange(e.target.value);
  }

  render() {
    const value = this.props.value;
    const scale = this.props.scale;
    return (
      <fieldset>
        <legend>Enter temperature in {scaleNames[scale]}:</legend>
        <input value={value}
               onChange={this.handleChange} />
      </fieldset>
    );
  }
}
```

Now let's turn to the `Calculator` component.
이제 `Calculator`컴포넌트로 돌아가 봅시다.

We will store the current input's `value` and `scale` in its local state. This is the state we "lifted up" from the inputs, and it will serve as the "source of truth" for both of them. It is the minimal representation of all the data we need to know in order to render both inputs.
이제 현재 input의 `value`와 `scale`을 해당 컴포넌트의 로컬 state에 저장할것입니다. 이 state가 바로 input에서 "들어올린(lifted up)" state이고, 두 input에게 있어서 "단일 소스 저장소"로써 역할할 것입니다. 이것은 두 input을 렌더링하기 위해 알아야 할 모든 데이터를 최소한으로 나타내는 방법이죠.

For example, if we enter 37 into the Celsius input, the state of the `Calculator` component will be:
예를 들어, 섭씨(celcius) input에 37을 입력하면, `Calculator` 컴포넌트의 state는 아래와 같을 것입니다:

```js
{
  value: '37',
  scale: 'c'
}
```

If we later edit the Fahrenheit field to be 212, the state of the `Calculator` will be:
그리고 나중에 화씨(fahrenheit) 필드를 212로 변경하면 `Calculator`의 state는 다음과 같죠:

```js
{
  value: '212',
  scale: 'f'
}
```

We could have stored the value of both inputs but it turns out to be unnecessary. It is enough to store the value of the most recently changed input, and the scale that it represents. We can then infer the value of the other input based on the current `value` and `scale` alone.
두 input들의 값을 모두 저장 할 수 있었지만, 불필요하다고 판명되었습니다. 단지 가장 최근 변경된 input의 값과 그것을 나타내는 단위를 저장하는 것으로 충분합니다. 그리고 현재의 `value`와 `scale`만으로 다른 input의 값을 추론할 수 있죠.

The inputs stay in sync because their values are computed from the same state:
입력값들은 동일한 state로 부터 계산되기 때문에 두 input은 서로 동기화된 상태를 유지할 수 있습니다:

```js{6,10,14,18-21,27-28,31-32,34}
class Calculator extends React.Component {
  constructor(props) {
    super(props);
    this.handleCelsiusChange = this.handleCelsiusChange.bind(this);
    this.handleFahrenheitChange = this.handleFahrenheitChange.bind(this);
    this.state = {value: '', scale: 'c'};
  }

  handleCelsiusChange(value) {
    this.setState({scale: 'c', value});
  }

  handleFahrenheitChange(value) {
    this.setState({scale: 'f', value});
  }

  render() {
    const scale = this.state.scale;
    const value = this.state.value;
    const celsius = scale === 'f' ? tryConvert(value, toCelsius) : value;
    const fahrenheit = scale === 'c' ? tryConvert(value, toFahrenheit) : value;

    return (
      <div>
        <TemperatureInput
          scale="c"
          value={celsius}
          onChange={this.handleCelsiusChange} />
        <TemperatureInput
          scale="f"
          value={fahrenheit}
          onChange={this.handleFahrenheitChange} />
        <BoilingVerdict
          celsius={parseFloat(celsius)} />
      </div>
    );
  }
}
```

[Try it on CodePen.](http://codepen.io/gaearon/pen/ozdyNg?editors=0010)

Now, no matter which input you edit, `this.state.value` and `this.state.scale` in the `Calculator` get updated. One of the inputs gets the value as is, so any user input is preserved, and the other input value is always recalculated based on it.
이제 어떠한 input을 편집 하든지, `Calculator`에 있는 `this.state.value`와 `this.state.scale`가 업데이트 됩니다. 두 input 중 하나는 그대로 값을 가져오므로 모든 유저의 입력은 보존되고 다른 값은 이를 바탕으로 다시 계산됩니다.

Let's recap what happens when you edit an input:
그럼 input을 편집 할 때 일어나는 일을 요약해보겠습니다:

* React calls the function specified as `onChange` on the DOM `<input>`. In our case, this is the `handleChange` method in `TemperatureInput` component.
* 리액트는 DOM `<input>`에 `onChange`로써 지정된 함수를 호출합니다. 이 경우에는, `TemperatureInput`컴포넌트의 `handleChange` 메소드입니다.
* The `handleChange` method in the `TemperatureInput` component calls `this.props.onChange()` with the new desired value. Its props, including `onChange`, were provided by its parent component, the `Calculator`.
* `TemperatureInput`컴포넌트의 `handleChange`메소드는 `this.props.onChange()`를 새로운 값으로 호출합니다. `onChange`를 포함한 `TemperatureInput`컴포넌트의 prop들은 부모 컴포넌트인 `Calculator`로부터 제공받은 것입니다.
* When it previously rendered, the `Calculator` has specified that `onChange` of the Celsius `TemperatureInput` is the `Calculator`'s `handleCelsiusChange` method, and `onChange` of the Fahrenheit `TemperatureInput` is the `Calculator`'s `handleFahrehnheitChange` method. So either of these two `Calculator` methods gets called depending on which input we edited.
* 그 이전에 렌더링 할 때, `Calculator`는 섭씨(Celsius) `TemperatureInput`의 `onChange`를 `Calculator`의 `handleCelsiusChange`메소드로 지정했고, 화씨(Fahrenheit) `TemperatureInput`의 `onChange`를 `Calculator`의 `handleFahrehnheitChange`메소드로 지정했습니다. 따라서 편집된 input에 따라 두 `Calculator`메소드 중 하나가 호출됩니다.
* Inside these methods, the `Calculator` component asks React to re-render itself by calling `this.setState()` with the new input value and the current scale of the input we just edited.
* 이 메소드들 안에서, `Calculator` 컴포넌트는 새 입력값과 현재 input의 scale(`c` 또는 `f`)과 함께 `this.setState`를 호출하여 스스로를 재렌더링 하도록 요청합니다.
* React calls the `Calculator` component's `render` method to learn what the UI should look like. The values of both inputs are recomputed based on the current temperature and the active scale. The temperature conversion is performed here.
* 리액트는 UI가 어떻게 보여야 하는지 알기 위해 `Calculator` 컴포넌트의 `render`메소드를 호출합니다. 그래서 두 input의 값들은 현재의 온도와 활성화된 scale을 기준으로 다시 계산됩니다. 온도의 변환은 해당 단계에서 일어납니다.
* React calls the `render` methods of the individual `TemperatureInput` components with their new props specified by the `Calculator`. It learns what their UI should look like.
* 리액트는 `Calculator`에서 지정된 새로운 prop이 적용된, 각각의 `TemperatureInput` 컴포넌트의 `render`메소드를 호출합니다. 리액트는 여기에서 UI를 학습하게 됩니다.
* React DOM updates the DOM to match the desired input values. The input we just edited receives its current value, and the other input is updated to the temperature after conversion.
* 리액트 DOM은 원하는 input 값이 표시 되도록 DOM을 업데이트 합니다. 방금 편집된 input은 그 스스로의 값을 받고, 다른 input은 변환 이후의 온도로 업데이트 됩니다.

Every update goes through the same steps so the inputs stay in sync.
모든 업데이트는 같은 단계를 거치기 때문에 두 input은 서로 동기성을 유지할 수 있습니다.

## Lessons Learned
## 얻은 교훈

There should be a single "source of truth" for any data that changes in a React application. Usually, the state is first added to the component that needs it for rendering. Then, if other components also need it, you can lift it up to their closest common ancestor. Instead of trying to sync the state between different components, you should rely on the [top-down data flow](/react/docs/state-and-lifecycle.html#the-data-flows-down).
리액트 애플리케이션에서는 변화하는 모든 데이터에 대한 하나의 "단일 소스 저장소"를 가져야 합니다. 일반적으로 state는 렌더링에 필요한 컴포넌트에 먼저 추가됩니다. 그런 다음, 또 다른 컴포넌트에서도 필요하게 되면 가장 가까운 공통 조상으로 들어올릴 수 있습니다. 서로 다른 컴포넌트의 state를 동기화 하기보다는 [하향식 데이터 흐름](/)에 의존해야 합니다.

Lifting state involves writing more "boilerplate" code than two-way binding approaches, but as a benefit, it takes less work to find and isolate bugs. Since any state "lives" in some component and that component alone can change it, the surface area for bugs is greatly reduced. Additionally, you can implement any custom logic to reject or transform user input.
state를 들어올리는 것은 양방향 바인딩 접근 방식보다 "보일러플레이트(boilerplate)" 코드를 많이 작성하게 되지만, 대신 버그를 찾아 격리하는 작업이 줄어든다는 이점이 있습니다. 모든 state는 어딘가의 컴포넌트에 "살아"있고 그 컴포넌트만이 변경할 수 있기 때문에, 버그의 표면적이 크게 줄어듭니다. 또한 유저의 입력을 거부하거나 변환하는 등의 여러가지 커스텀 논리를 구현할 수 있습니다.

If something can be derived from either props or state, it probably shouldn't be in the state. For example, instead of storing both `celsiusValue` and `fahrenheitValue`, we store just the last edited `value` and its `scale`. The value of the other input can always be calculated from them in the `render()` method. This lets us clear or apply rounding to the other field without losing any precision in the user input.
만약 무언가가 props와 state 둘 모두로 부터 파생될 수 있다면, state에는 저장되면 안됩니다. 예를 들어, `celsiusValue`와 `fahrenheitValue` 모두를 저장하는 것 대신에, 저희는 마지막으로 수정된 `value`와 그의 `scale`만을 저장했습니다. 또 다른 input의 값은 언제나 `render()`메소드에서 계산될 수 있습니다. 이렇게 하면 유저의 입력값의 정확성을 잃지 않으면서 다른 input의 값에 반올림을 적용하거나 지울 수 있습니다.

When you see something wrong in the UI, you can use [React Developer Tools](https://github.com/facebook/react-devtools) to inspect the props and move up the tree until you find the component responsible for updating the state. This lets you trace the bugs to their source:
UI에서 문제가 발생하면, [React Developer Tools](/)를 사용 하여 props를 조사하고 state의 업데이트를 담당하는 컴포넌트를 찾을 때까지 트리를 따라 위로 올라갈 수 있습니다. 이런 방법으로 버그의 소스를 추적할 수 있습니다:

<img src="/react/img/docs/react-devtools-state.gif" alt="Monitoring State in React DevTools" width="100%">

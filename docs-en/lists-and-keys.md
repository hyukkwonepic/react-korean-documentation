---
id: lists-and-keys
title: Lists and Keys
permalink: docs/lists-and-keys.html
prev: conditional-rendering.html
next: forms.html
---

First, let's review how you transform lists in JavaScript.
먼저 리스트를 JavaScript로 변환하는 방법을 살펴보도록 하겠습니다.

Given the code below, we use the [`map()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map) function to take an array of `numbers` and double their values. We assign the new array returned by `map()` to the variable `doubled` and log it:
아래에 주어진 코드를 보면, [`map()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map)함수를 사용하여 `numbers` 배열의 값들을 두 배로 늘립니다. 그 후 `map()`에 의해 반환된 새로운 배열을 변수 `doubled`에 할당하고 로그를 살펴봅니다:

```javascript{2}
const numbers = [1, 2, 3, 4, 5];
const doubled = numbers.map((number) => number * 2);
console.log(doubled);
```

This code logs `[2, 4, 6, 8, 10]` to the console.
이 코드는 `[2, 4, 6, 8, 10]`를 콘솔에 기록하게 됩니다.

In React, transforming arrays into lists of [elements](/react/docs/rendering-elements.html) is nearly identical.
리액트에서, 배열을 [엘리먼트](/react/docs/rendering-elements.html)의 리스트로 변환하는 것도 굉장히 유사합니다.

### Rendering Multiple Components
### 다수의 컴포넌트 렌더링하기

You can build collections of elements and [include them in JSX](/react/docs/introducing-jsx.html#embedding-expressions-in-jsx) using curly braces `{}`.
엘리먼트의 묶음을 빌드하고 이를 중괄호 `{}`를 사용하여 [JSX에 포함](/react/docs/introducing-jsx.html#embedding-expressions-in-jsx)할 수 있습니다.

Below, we loop through the `numbers` array using the Javascript [`map()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map) function. We return an `<li>` element for each item. Finally, we assign the resulting array of elements to `listItems`:
아래에서 JavaScript의 [`map()`](/)함수를 사용하여 `numbers` 배열을 루프합니다. 그래서 각 아이템을 `<li>`앨리먼트를 감싸서 반환하게 되죠. 마지막으로, 엘리먼트의 배열을 `listItems`에 할당합니다.

```javascript{2-4}
const numbers = [1, 2, 3, 4, 5];
const listItems = numbers.map((number) =>
  <li>{number}</li>
);
```

We include the entire `listItems` array inside a `<ul>` element, and [render it to the DOM](/react/docs/rendering-elements.html#rendering-an-element-into-the-dom):
여기에서 `listItems` 배열을 통째로 `<ul>`앨리먼트 내부에 포함한 후, [DOM에 렌더링합니다](/react/docs/rendering-elements.html#rendering-an-element-into-the-dom).

```javascript{2}
ReactDOM.render(
  <ul>{listItems}</ul>,
  document.getElementById('root')
);
```

[Try it on CodePen.](https://codepen.io/gaearon/pen/GjPyQr?editors=0011)

This code displays a bullet list of numbers between 1 and 5.
해당 코드는 1에서 5까지의 숫자를 불렛 리스트에 표현합니다.

### Basic List Component
### 기본 리스트 컴포넌트

Usually you would render lists inside a [component](/react/docs/components-and-props.html).
일반적으로는 [컴포넌트](/)내부에 리스트를 포함하게 됩니다.

We can refactor the previous example into a component that accepts an array of `numbers` and outputs an unordered list of elements.
이전의 예제를 `numbers`배열을 받는 컴포넌트로 리팩토링하여 앨리먼트들의 unordered list(`<ul>`)를 출력할 수 있습니다.

```javascript{3-5,7,13}
function NumberList(props) {
  const numbers = props.numbers;
  const listItems = numbers.map((number) =>
    <li>{number}</li>
  );
  return (
    <ul>{listItems}</ul>
  );
}

const numbers = [1, 2, 3, 4, 5];
ReactDOM.render(
  <NumberList numbers={numbers} />,
  document.getElementById('root')
);
```

When you run this code, you'll be given a warning that a key should be provided for list items. A "key" is a special string attribute you need to include when creating lists of elements. We'll discuss why it's important in the next section.
위의 코드를 실행해 보신다면 각 리스트 항목별로 key가 제공되어야 한다는 경고 메시지가 나타납니다. "key"란 엘리먼트의 리스트를 만들때 포함해야 하는 특별한 문자열 속성입니다. 이것이 중요한 이유에 대해서는 다음 섹션에서 다루도록 하겠습니다.

Let's assign a `key` to our list items inside `numbers.map()` and fix the missing key issue.
`numbers.map()`내부의 리스트 항목들에 `key`를 할당하고 key 누락 이슈를 고쳐보도록 하죠.

```javascript{4}
function NumberList(props) {
  const numbers = props.numbers;
  const listItems = numbers.map((number) =>
    <li key={number.toString()}>
      {number}
    </li>
  );
  return (
    <ul>{listItems}</ul>
  );
}

const numbers = [1, 2, 3, 4, 5];
ReactDOM.render(
  <NumberList numbers={numbers} />,
  document.getElementById('root')
);
```

[Try it on CodePen.](https://codepen.io/gaearon/pen/jrXYRR?editors=0011)

## Keys
## Keys

Keys help React identify which items have changed, are added, or are removed. Keys should be given to the elements inside the array to give the elements a stable identity:
Key는 리액트가 어떤 항목이 변화하고, 추가되고 또는 제거되었는지 알 수 있도록 도와줍니다. 엘리먼트에게 안정적인 아이덴티티(identity) 제공하기 위해 key는 배열 내부의 엘리먼트에게 주어져야 합니다:

```js{3}
const numbers = [1, 2, 3, 4, 5];
const listItems = numbers.map((number) =>
  <li key={number.toString()}>
    {number}
  </li>
);
```

The best way to pick a key is to use a string that uniquely identifies a list item among its siblings. Most often you would use IDs from your data as keys:
Key를 선택하는 가장 좋은 방법은 리스트 항목끼리 서로 구분 할 수 있도록 고유한 식별 문자열을 사용하는 것 입니다. 대부분의 경우 데이터의 ID를 key로 사용합니다:

```js{2}
const todoItems = todos.map((todo) =>
  <li key={todo.id}>
    {todo.text}
  </li>
);
```

When you don't have stable IDs for rendered items, you may use the item index as a key as a last resort:
렌더링된 항목들에 안정적인 ID가 없는 경우에는 최후의 수단으로 색인(index)를 사용할 수 있습니다:

```js{2,3}
const todoItems = todos.map((todo, index) =>
  // Only do this if items have no stable IDs
  <li key={index}>
    {todo.text}
  </li>
);
```

We don't recommend using indexes for keys if the items can reorder, as that would be slow. You may read an [in-depth explanation about why keys are necessary](/react/docs/reconciliation.html#recursing-on-children) if you're interested.
항목의 key로써 색인을 사용하는것을 권장드리지 않습니다. 항목의 순서가 재정렬 될 수 있으면 느려질 수 있습니다. 관심이 있으시면 [key가 필요한 이유에 대한 심층적 설명](/react/docs/reconciliation.html#recursing-on-children)을 한번 읽어보세요.

### Extracting Components with Keys
### Key를 사용하여 컴포넌트 추출하기

Keys only make sense in the context of the surrounding array.
Key는 오직 감싸진 배열상에서만 의미가 있습니다.

For example, if you [extract](/react/docs/components-and-props.html#extracting-components) a `ListItem` component, you should keep the key on the `<ListItem />` elements in the array rather than on the root `<li>` element in the `ListItem` itself.
예를 들어 만약 여러분이 `ListItem`컴포넌트를 [추출](/react/docs/components-and-props.html#extracting-components)하려 할 때, `ListItem`그 자체의 루트 `<li>`엘리먼트 대신에 배열 내부의 `<ListItem />`엘리먼트에 key를 제공해야 합니다.

**Example: Incorrect Key Usage**
**예: 잘못된 Key 사용**

```javascript{4,5,14,15}
function ListItem(props) {
  const value = props.value;
  return (
    // Wrong! There is no need to specify the key here:
    // 땡! key를 여기에 명시할 필요는 없죠:
    <li key={value.toString()}>
      {value}
    </li>
  );
}

function NumberList(props) {
  const numbers = props.numbers;
  const listItems = numbers.map((number) =>
    // Wrong! The key should have been specified here:
    // 땡! Key는 여기에 명시되어야 합니다:
    <ListItem value={number} />
  );
  return (
    <ul>
      {listItems}
    </ul>
  );
}

const numbers = [1, 2, 3, 4, 5];
ReactDOM.render(
  <NumberList numbers={numbers} />,
  document.getElementById('root')
);
```

**Example: Correct Key Usage**
**예: 올바른 키 사용**

```javascript{2,3,9,10}
function ListItem(props) {
  // Correct! There is no need to specify the key here:
  // 정확해요! 여기에는 key를 제공할 필요가 없죠:
  return <li>{props.value}</li>;
}

function NumberList(props) {
  const numbers = props.numbers;
  const listItems = numbers.map((number) =>
    // Correct! Key should be specified inside the array.
    // 정확해요! Key는 배열 내부에 명시되어야 합니다.
    <ListItem key={number.toString()}
              value={number} />
  );
  return (
    <ul>
      {listItems}
    </ul>
  );
}

const numbers = [1, 2, 3, 4, 5];
ReactDOM.render(
  <NumberList numbers={numbers} />,
  document.getElementById('root')
);
```

[Try it on CodePen.](https://codepen.io/rthor/pen/QKzJKG?editors=0010)

A good rule of thumb is that elements inside the `map()` call need keys.
단순하게 `map()`내부의 엘리먼트들은 key가 필요하다고 생각하면 됩니다.

### Keys Must Only Be Unique Among Siblings
### 형제 엘리먼트 간 key는 반드시 고유한 값이어야 함

Keys used within arrays should be unique among their siblings. However they don't need to be globally unique. We can use the same keys when we produce two different arrays:
배열에 사용되는 key는 반드시 다른 형제 엘리먼트와 다른 고유한 값을 가져야 합니다. 하지만 전역적으로 고유할 필요는 없습니다. 두개의 다른 배열을 만들었다면 같은 key를 사용할 수 있습니다:

```js{2,5,11,12,19,21}
function Blog(props) {
  const sidebar = (
    <ul>
      {props.posts.map((post) =>
        <li key={post.id}>
          {post.title}
        </li>
      )}
    </ul>
  );
  const content = props.posts.map((post) =>
    <div key={post.id}>
      <h3>{post.title}</h3>
      <p>{post.content}</p>
    </div>
  );
  return (
    <div>
      {sidebar}
      <hr />
      {content}
    </div>
  );
}

const posts = [
  {id: 1, title: 'Hello World', content: 'Welcome to learning React!'},
  {id: 2, title: 'Installation', content: 'You can install React from npm.'}
];
ReactDOM.render(
  <Blog posts={posts} />,
  document.getElementById('root')
);
```

[Try it on CodePen.](https://codepen.io/gaearon/pen/NRZYGN?editors=0010)

Keys serve as a hint to React but they don't get passed to your components. If you need the same value in your component, pass it explicitly as a prop with a different name:
Key는 리액트에서 힌트로써 작용하지만 컴포넌트에 전달되지는 않습니다. 만약 key의 값이 컴포넌트에서 필요하다면 다른 이름을 사용하여 prop으로써 명시적으로 전달하시면 됩니다.

```js{3,4}
const content = posts.map((post) =>
  <Post
    key={post.id}
    id={post.id}
    title={post.title} />
);
```

With the example above, the `Post` component can read `props.id`, but not `props.key`.
위의 예제에서 `Post` 컴포넌트는 `props.id`를 읽을 수 있지만 `props.key`는 읽을 수 없습니다.

### Embedding map() in JSX
### JSX에서 map() 임베드하기

In the examples above we declared a separate `listItems` variable and included it in JSX:
위의 예제에서 `listItems`변수를 따로 선언하고 JSX내부에 포함했습니다:

```js{3-6}
function NumberList(props) {
  const numbers = props.numbers;
  const listItems = numbers.map((number) =>
    <ListItem key={number.toString()}
              value={number} />
  );
  return (
    <ul>
      {listItems}
    </ul>
  );
}
```

JSX allows [embedding any expressions](/react/docs/introducing-jsx.html#embedding-expressions-in-jsx) in curly braces so we could inline the `map()` result:
JSX는 중괄호 안에 [표현식 임베딩](/)을 허용하기 때문에 `map()`의 결과를 인라인으로 포함할 수 있습니다:

```js{5-8}
function NumberList(props) {
  const numbers = props.numbers;
  return (
    <ul>
      {numbers.map((number) =>
        <ListItem key={number.toString()}
                  value={number} />
      )}
    </ul>
  );
}
```

[Try it on CodePen.](https://codepen.io/gaearon/pen/BLvYrB?editors=0010)

Sometimes this results in clearer code, but this style can also be abused. Like in JavaScript, it is up to you to decide whether it is worth extracting a variable for readability. Keep in mind that if the `map()` body is too nested, it might be a good time to [extract a component](/react/docs/components-and-props.html#extracting-components).
때로는 위의 방식이 코드를 더 깨끗하게 만들지만, 이런 스타일은 악용될 수도 있습니다. JavaScript에서와 마찬가지로, 가독성을 위해 변수를 따로 지정할지 말지에 대한 여부는 여러분이 결정하시면 됩니다. 만약 `map()`의 내용이 너무 중첩(nest)되었다면 [컴포넌트를 추출](/)할 때일 수 있다는 것을 알아두세요.

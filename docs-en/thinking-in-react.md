---
id: thinking-in-react
title: Thinking in React
permalink: docs/thinking-in-react.html
redirect_from:
  - 'blog/2013/11/05/thinking-in-react.html'
  - 'docs/thinking-in-react-zh-CN.html'
prev: composition-vs-inheritance.html
---

React is, in our opinion, the premier way to build big, fast Web apps with JavaScript. It has scaled very well for us at Facebook and Instagram.
저희 리액트가 JavaScript로 크고 빠른 웹앱을 만드는 최고의 방법이라고 생각합니다. Facebook과 Instagram에서 굉장히 잘 확장되어왔죠.

One of the many great parts of React is how it makes you think about apps as you build them. In this document, we'll walk you through the thought process of building a searchable product data table using React.
리액트에서 여러 좋은 점 중의 하나는, 앱을 만들면서 앱에 대해 생각하는 방법에 관한 것 입니다. 해당 문서에서는 리액트를 사용하여, 검색 가능한 제품 데이터 테이블을 만들면서 생각하는 과정에 대해 살펴볼 것입니다.

## Start With A Mock
## 목업에서 시작하기

Imagine that we already have a JSON API and a mock from our designer. The mock looks like this:
저희가 이미 JSON API와 디자이너에게 받은 목업이 있다고 가정하겠습니다. 목업은 아래와 같이 생겼죠:

![Mockup](/react/img/blog/thinking-in-react-mock.png)

Our JSON API returns some data that looks like this:
JSON API는 아래와 같은 데이터를 반환합니다:

```
[
  {category: "Sporting Goods", price: "$49.99", stocked: true, name: "Football"},
  {category: "Sporting Goods", price: "$9.99", stocked: true, name: "Baseball"},
  {category: "Sporting Goods", price: "$29.99", stocked: false, name: "Basketball"},
  {category: "Electronics", price: "$99.99", stocked: true, name: "iPod Touch"},
  {category: "Electronics", price: "$399.99", stocked: false, name: "iPhone 5"},
  {category: "Electronics", price: "$199.99", stocked: true, name: "Nexus 7"}
];
```

## Step 1: Break The UI Into A Component Hierarchy
## 1단계: UI를 컴포넌트 계층으로 나누기

The first thing you'll want to do is to draw boxes around every component (and subcomponent) in the mock and give them all names. If you're working with a designer, they may have already done this, so go talk to them! Their Photoshop layer names may end up being the names of your React components!
먼저 여러분이 해야할 것은 목업의 모든 컴포넌트(및 하위 컴포넌트)들에 박스를 그린 후 각각 이름을 짓습니다. 혹시 디자이너와 함께 작업하신다면 이미 작업을 마친 상태일 수도 있기때문에 다가가 말씀해보세요! Photoshop의 레이어 이름이 리액트 컴포넌트의 이름이 될 수 있으니까요!

But how do you know what should be its own component? Just use the same techniques for deciding if you should create a new function or object. One such technique is the [single responsibility principle](https://en.wikipedia.org/wiki/Single_responsibility_principle), that is, a component should ideally only do one thing. If it ends up growing, it should be decomposed into smaller subcomponents.
그럼 대체 무엇이 자체적인 컴포넌트가 되어야하는지 어떻게 알 수 있을까요? 이럴 땐 새로운 함수나 객체를 만들어야 하는지 결정하는 것과 같은 기술을 사용하시면 됩니다. 그 중 하나의 기술은 바로 [단일 책임 원칙](https://ko.wikipedia.org/wiki/%EB%8B%A8%EC%9D%BC_%EC%B1%85%EC%9E%84_%EC%9B%90%EC%B9%99)인데요, 이상적으로 컴포넌트는 이상적으로 한가지 일만 수행해야 한다는 것 입니다. 만약 컴포넌트가 자라게 된다면, 더 작은 하위 컴포넌트들로 분해되어야 하죠.

Since you're often displaying a JSON data model to a user, you'll find that if your model was built correctly, your UI (and therefore your component structure) will map nicely. That's because UI and data models tend to adhere to the same *information architecture*, which means the work of separating your UI into components is often trivial. Just break it up into components that represent exactly one piece of your data model.
여러분은 JSON 데이터 모델을 유저에게 표시해주기 때문에, 여러분의 모델을 올바르게 작성하면 UI(즉, 컴포넌트 구조)에 잘 매핑될 것입니다. 왜냐하면 UI와 데이터 모델은 동일한 *정보 아키텍쳐* 를 따르는 경향이 있기 때문입니다. 즉, UI를 컴포넌트로 나누는 작업은 그다지 중요하지 않다는 말이죠. 데이터 모델의 조각 하나를 정확히 나타내는 컴포넌트들로 UI를 분해하시면 됩니다.면

![Component diagram](/react/img/blog/thinking-in-react-components.png)

You'll see here that we have five components in our simple app. We've italicized the data each component represents.
저희의 간단한 앱에는 다섯가지 컴포넌트가 있는걸 볼 수 있습니다. 각 컴포넌트가 나타내는 데이터는 이텔릭체로 표시했습니다.

  1. **`FilterableProductTable` (orange):** contains the entirety of the example
  2. **`SearchBar` (blue):** receives all *user input*
  3. **`ProductTable` (green):** displays and filters the *data collection* based on *user input*
  4. **`ProductCategoryRow` (turquoise):** displays a heading for each *category*
  5. **`ProductRow` (red):** displays a row for each *product*

  1. **`FilterableProductTable` (주황색):** 예제 앱 전체를 포함합니다
  2. **`SearchBar` (파란색):** 모든 *유저 입력* 을 입력받습니다
  3. **`ProductTable` (녹색):** *유저 입력* 을 기반으로 *데이터 수집* 을 표시하고 필터링합니다
  4. **`ProductCategoryRow` (청록색):** 각 *카테고리* 의 헤더(header)를 표시합니다
  5. **`ProductRow` (빨간색):** 각 *제품* 의 행을 표시합니다

If you look at `ProductTable`, you'll see that the table header (containing the "Name" and "Price" labels) isn't its own component. This is a matter of preference, and there's an argument to be made either way. For this example, we left it as part of `ProductTable` because it is part of rendering the *data collection* which is `ProductTable`'s responsibility. However, if this header grows to be complex (i.e. if we were to add affordances for sorting), it would certainly make sense to make this its own `ProductTableHeader` component.
`ProductTable`을 살펴 보시면, 테이블 헤더("Name"과 "Price"를 포함한 라벨)는 자체적인 컴포넌트가 아님을 알 수 있습니다. 이것은 단지 선호의 문제인데, 어떤 방향이든 논쟁거리가 될 수 있습니다. 해당 예시에서는 테이블 헤더를 `ProductTable`의 일부로 남겨 두었습니다. 왜냐하면 해당 헤더는 `ProductTable`이 책임지고 있는 *데이터 컬렉션* 을 렌더링하는 부분이기 때문입니다. 하지만 이 헤더가 복잡해지면(즉, 정렬을 위한 어포던스를 추가하는 경우), 이것을 `ProductTableHeader`라는 자체적인 컴포넌트로 만드는것이 타당할 것입니다.

Now that we've identified the components in our mock, let's arrange them into a hierarchy. This is easy. Components that appear within another component in the mock should appear as a child in the hierarchy:
이제 저희의 목업에서 어떤 컴포넌트들이 존재하는지 알았으니, 이것을 계층으로 구조로 정리해보도록 하겠습니다. 이 과정은 어렵지 않습니다. 목업에서, 컴포넌트 내부에 존재하는 컴포넌트들은, 계층 구조 상에서 자식으로 표현하면 됩니다:

  * `FilterableProductTable`
    * `SearchBar`
    * `ProductTable`
      * `ProductCategoryRow`
      * `ProductRow`

## Step 2: Build A Static Version in React
## 2단계: 리액트로 정적인 버전 만들기

<p data-height="600" data-theme-id="0" data-slug-hash="vXpAgj" data-default-tab="js" data-user="lacker" data-embed-version="2" class="codepen">See the Pen <a href="http://codepen.io/lacker/pen/vXpAgj/">Thinking In React: Step 2</a> on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

Now that you have your component hierarchy, it's time to implement your app. The easiest way is to build a version that takes your data model and renders the UI but has no interactivity. It's best to decouple these processes because building a static version requires a lot of typing and no thinking, and adding interactivity requires a lot of thinking and not a lot of typing. We'll see why.
이제 컴포넌트 계층 구조를 알고 있으니, 실제로 앱을 구현할 단계입니다. 가장 쉬운 방법은, 데이터 모델을 바탕으로 UI를 렌더링하되, 상호 작용(interactivity)이 없는 버전을 만드는 것 입니다. 정적(static)인 버전을 구현하는 것은 많은 타이핑이 필요한 대신 생각할 필요가 없고, 상호 작용성을 추가하는것은 많은 타이핑이 없는 대신에 깊은 사고(思考)를 해야하기 떄문에 이 두가지 작업 과정을 분리하는 것이 좋습니다.

To build a static version of your app that renders your data model, you'll want to build components that reuse other components and pass data using *props*. *props* are a way of passing data from parent to child. If you're familiar with the concept of *state*, **don't use state at all** to build this static version. State is reserved only for interactivity, that is, data that changes over time. Since this is a static version of the app, you don't need it.
데이터 모델을 렌더링하는 정적 버전의 앱을 만들 때, 다른 컴포넌트를 재사용하는 컴포넌트를 만들 때 *props* 를 사용하여 데이터를 전달해야 합니다. *props* 는 부모에서 자식에게 데이터를 전달하는 방법이죠. 여러분이 *state* 의 개념에 익숙하다면, 이 정적 버전을 만들 때 **절대 state를 사용하지 마세요**. State는 오로지 상호작용성을 위해, 즉, 시간에 따라 변화하는 데이터를 위해 사용이 보류됩니다. 지금은 정적 버전이기 때문에 state의 사용은 필요하지 않습니다.

You can build top-down or bottom-up. That is, you can either start with building the components higher up in the hierarchy (i.e. starting with `FilterableProductTable`) or with the ones lower in it (`ProductRow`). In simpler examples, it's usually easier to go top-down, and on larger projects, it's easier to go bottom-up and write tests as you build.
여러분은 하향식(top-down) 또는 상향식(bottom-up) 방법으로 구축할 수 있습니다. 말하자면, 상위 계층의 컴포넌트를 만드는 것에서부터 시작(즉, `FilterableProductTable`를 만들기 시작)하거나 하위 계층(`ProductRow`)에서 시작할 수 있다는 것이죠. 단순한 앱을 만들때라면 하향식이 일반적으로 더 쉬울테고, 큰 프로젝트의 경우에는 상향식 방법을 사용하여 테스트를 작성해 가며 만드는 것이 더 쉽습니다.

At the end of this step, you'll have a library of reusable components that render your data model. The components will only have `render()` methods since this is a static version of your app. The component at the top of the hierarchy (`FilterableProductTable`) will take your data model as a prop. If you make a change to your underlying data model and call `ReactDOM.render()` again, the UI will be updated. It's easy to see how your UI is updated and where to make changes since there's nothing complicated going on. React's **one-way data flow** (also called *one-way binding*) keeps everything modular and fast.
이 단계를 마치면, 데이터 모델을 렌더링 하는, 재사용 가능한 컴포넌트 라이브러리를 갖게 됩니다. 정적 버전의 앱이기 때문에 컴포넌트들은 단지 `render()`메소드만을 가지고 있을 것입니다. 계층 구조에서 가장 상위에 있는 컴포넌
트(`FilterableProductTable`)는 데이터 모델을 prop으로써 가지고 있을 것 입니다. 이 데이터 모델을 변경하고 `ReactDOM.render()`를 다시 호출하면, UI가 업데이트 될 것입니다. 앱내에서 복잡한 과정이 없기 때문에 UI가 어떻게 업데이트되고 어떤 부분을 변경해야 하는지 쉽게 파악할 수 있습니다. 리액트의 **단방향 데이터 흐름(one-way data flow)** (*단방향 바인딩(one-way binding)* 이라고도 불림)은 모든것을 모듈화하고 빠르게 유지합니다.

Simply refer to the [React docs](/react/docs/) if you need help executing this step.
해당 단계를 진행하는데 도움이 필요하시다면 [리액트 문서](/react/docs/)를 참조하십시오.

### A Brief Interlude: Props vs State
### 막간 정보: Props vs State

There are two types of "model" data in React: props and state. It's important to understand the distinction between the two; skim [the official React docs](/react/docs/interactivity-and-dynamic-uis.html) if you aren't sure what the difference is.
리액트에는 두가지 데이터 "모델"이 있습니다: props와 state죠. 두가지를 구분하여 이해하는것이 중요하기 때문에 [리액트 공식문서](/react/docs/interactivity-and-dynamic-uis.html)를 훑어 보십시오.

## Step 3: Identify The Minimal (but complete) Representation Of UI State
## 3단계: UI state의 최소(하지만 완전한) 표현의 식별

To make your UI interactive, you need to be able to trigger changes to your underlying data model. React makes this easy with **state**.
UI가 상호작용하도록 만들기 위해서는, 사용된 데이터 모델에 변화를 줄 수 있도록 해야합니다. 리액트는 **state** 를 사용하여 이것을 쉽게 할 수 있도록 합니다.

To build your app correctly, you first need to think of the minimal set of mutable state that your app needs. The key here is DRY: *Don't Repeat Yourself*. Figure out the absolute minimal representation of the state your application needs and compute everything else you need on-demand. For example, if you're building a TODO list, just keep an array of the TODO items around; don't keep a separate state variable for the count. Instead, when you want to render the TODO count, simply take the length of the TODO items array.
여러분의 앱을 올바르게 구현하려면, 앱이 필요한, 최소한의 변경 가능한 state 셋(set)을 고려해야 합니다. 여기에서 핵심은 바로 DRY원리 입니다: *Don't Repeat Yourself(중복배제)*. 여러분의 앱이 필요로 하는 state의 절대 최소 표현(absolute minimal representation)을 알아내고, 때에 따라 필요한 모든 것들을 계산하십시오. 예를 들어, TODO 리스트를 만든다고 할 때, TODO 항목의 배열만을 가지고 계십시오. 항목의 갯수를 새는 별도의 state를 갖고 계시면 안됩니다. 대신에, TODO 항목의 갯수를 렌더링 하려 할 때에는, TODO 항목의 배열의 길이를 사용하면 됩니다.

Think of all of the pieces of data in our example application. We have:
저희의 예시 앱에서 필요한 모든 데이터 조각들을 고려해봅시다. 저희는 지금 아래의 데이터 조각들을 가지고 있습니다:

  * The original list of products
  * The search text the user has entered
  * The value of the checkbox
  * The filtered list of products

  * 원(原, original)제품 리스트
  * 유저가 입력한 검색 텍스트
  * 체크박스의 값(value)
  * 필터링 된 제품 리스트

Let's go through each one and figure out which one is state. Simply ask three questions about each piece of data:
이제 하나씩 살펴보면서 무엇이 state인지 알아내보도록 하겠습니다. 각 데이터 조각마다 3가지 질문을 던지기만 하면 됩니다:

  1. Is it passed in from a parent via props? If so, it probably isn't state.
  2. Does it remain unchanged over time? If so, it probably isn't state.
  3. Can you compute it based on any other state or props in your component? If so, it isn't state.

  1. 이것은 부모로부터 props를 통해 전달 된 것인가? 만약 그렇다면 이것은 state가 아닐것이다.
  2. 이것은 시간이 지나면서 불변함을 유지하는가? 만약 그렇다면 이것은 state가 아닐것이다.
  3. 컴포넌트 내부에서 다른 state 또는 props를 통해 이것을 계산해낼 수 있는가? 만약 그렇다면 state가 아닐것이다.

The original list of products is passed in as props, so that's not state. The search text and the checkbox seem to be state since they change over time and can't be computed from anything. And finally, the filtered list of products isn't state because it can be computed by combining the original list of products with the search text and value of the checkbox.
원(原)제품 리스트는 props로써 전달되었기 때문에 state가 아닙니다. 검색 텍스트와 체크박스는 시간이 지나면서 변화하고 무언가로부터 계산할 수 있는것이 아니기 때문에 state인 것 같습니다. 그리고 마지막으로, 검색 텍스트로 필터링 된 제품 리스트는 state가 아닙니다. 왜냐하면 이것은 원(原)리스트와 검색 텍스트, 그리고 체크박스의 값을 통해서 계산될 수 있기 때문입니다.

So finally, our state is:
결국, 저희의 state는 아래와 같습니다:

  * The search text the user has entered
  * The value of the checkbox

  * 유저가 입력한 검색 텍스트값
  * 체크박스의 값

## Step 4: Identify Where Your State Should Live
## 4단계: 여러분의 state가 적용되어야(살아야)할 장소

<p data-height="600" data-theme-id="0" data-slug-hash="ORzEkG" data-default-tab="js" data-user="lacker" data-embed-version="2" class="codepen">See the Pen <a href="http://codepen.io/lacker/pen/ORzEkG/">Thinking In React: Step 4</a> by Kevin Lacker (<a href="http://codepen.io/lacker">@lacker</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

OK, so we've identified what the minimal set of app state is. Next, we need to identify which component mutates, or *owns*, this state.
좋습니다, 이제 저희는 앱에 필요한 최소 셋트의 state를 알아내었습니다. 이제, 어떤 컴포넌트가 이 state를 변경하거나 *소유* 하는지 알아내야 합니다.

Remember: React is all about one-way data flow down the component hierarchy. It may not be immediately clear which component should own what state. **This is often the most challenging part for newcomers to understand,** so follow these steps to figure it out:
기억하세요: 리액트는 모든것이 컴포넌트 계층의 하향식 단방향 데이터 흐름과 관련되어 있습니다. 아마도 어떠한 컴포넌트가 어떠한 state를 소유하고 있어야 한다는 것이 바로 이해되지 않을 수 있습니다. **이 사실은 신참자가 이해하기 가장 어려운 부분일 수 있기에** 아래의 과정을 따르며 이해해 보세요:

For each piece of state in your application:
여러분의 애플리케이션에서 각 state조각에 대해서:

  * Identify every component that renders something based on that state.
  * Find a common owner component (a single component above all the components that need the state in the hierarchy).
  * Either the common owner or another component higher up in the hierarchy should own the state.
  * If you can't find a component where it makes sense to own the state, create a new component simply for holding the state and add it somewhere in the hierarchy above the common owner component.

  * state에 따라서 무언가를 렌더링하는 컴포넌트를 식별하십시오.
  * 공통되는 소유 컴포넌트(계층 구조에서 state를 필요로하며 모든 컴포넌트 위에 존재하는 단일 컴포넌트)를 찾으십시오.
  * 공통 소유 컴포넌트 또는 계층에서 상위에 있는 컴포넌트 중 하나가 state를 소유해야 합니다.
  * 만약 state를 소유 할만한 컴포넌트를 찾지 못했다면, 단순히 state를 소유하는 새로운 컴포넌트를 생성한 후에, 공통 소유 컴포넌트 상위에 추가하시면 됩니다.

Let's run through this strategy for our application:
위의 전략을 저희의 애플리케이션에서 연습해보도록 하겠습니다:

  * `ProductTable` needs to filter the product list based on state and `SearchBar` needs to display the search text and checked state.
  * The common owner component is `FilterableProductTable`.
  * It conceptually makes sense for the filter text and checked value to live in `FilterableProductTable`

  * `ProductTable`은 제품 리스트를 state에 따라서 필터링해야 하고, `SearchBar`는 검색 텍스트와 체크 값을 표시해야 합니다.
  * 공통 소유 컴포넌트는 `FilterableProductTable`입니다.
  * 개념적으로, 필터 텍스트(filter text)와 체크 값(checked value)은 `FilterableProductTable`내부에 살아있어야 합니다.

Cool, so we've decided that our state lives in `FilterableProductTable`. First, add an instance property `this.state = {filterText: '', inStockOnly: false}` to `FilterableProductTable`'s `constructor` to reflect the initial state of your application. Then, pass `filterText` and `inStockOnly` to `ProductTable` and `SearchBar` as a prop. Finally, use these props to filter the rows in `ProductTable` and set the values of the form fields in `SearchBar`.
좋습니다! 이제 state가 `FilterableProductTable`에 존재하도록 결정했습니다. 먼저, 애플리케이션의 초기 state를 반영하기 위해 인스턴스 속성인 `this.state = {filterText: '', inStockOnly: false}`를 `FilterableProductTable`의 `constructor`에 추가하겠습니다. 그 다음으로, `filterText`와 `inStockOnly`를 `ProductTable`과 `SearchBar`에 prop으로써 전달합니다. 마지막으로, `ProductTable`에서 props를 활용해 행(rows)을 필터링하고 `SearchBar`에서 폼 필드의 값을 지정합니다.

You can start seeing how your application will behave: set `filterText` to `"ball"` and refresh your app. You'll see that the data table is updated correctly.
이제 애플리케이션이 어떻게 작동하는지 살펴볼 수 있습니다: `filterText`를 `"ball"`로 설정하고 앱을 새로고침 해보세요. 데이터 테이블이 올바르게 업데이트 된 것을 확인할 수 있습니다.

## Step 5: Add Inverse Data Flow
## 5단계: 역 데이터 흐름 추가하기

<p data-height="265" data-theme-id="0" data-slug-hash="qRqmjd" data-default-tab="js,result" data-user="rohan10" data-embed-version="2" data-pen-title="Thinking In React: Step 5" class="codepen">See the Pen <a href="http://codepen.io/rohan10/pen/qRqmjd">Thinking In React: Step 5</a> on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

So far, we've built an app that renders correctly as a function of props and state flowing down the hierarchy. Now it's time to support data flowing the other way: the form components deep in the hierarchy need to update the state in `FilterableProductTable`.
지금까지 저희는, 계층 구조를 따라 내려오는 props와 state의 함수로써 올바르게 렌더링되는 앱을 만들었습니다. 이제 다른 방향의 데이터 프름을 지원할 차례입니다: 계층 구조 가장 깊숙한 곳에서 존재하는 폼 컴포넌트가 `FilterableProductTable`의 state를 업데이트 해야합니다.

React makes this data flow explicit to make it easy to understand how your program works, but it does require a little more typing than traditional two-way data binding.
리액트는 데이터 흐름을 명시적으로 만들어 프로그램이 어떻게 작동하는지 쉽게 이해할 수 있지만, 기존의 양방향 데이터 바인딩(two-way data binding) 방식보다 코드를 조금 더 작성해야 합니다.

If you try to type or check the box in the current version of the example, you'll see that React ignores your input. This is intentional, as we've set the `value` prop of the `input` to always be equal to the `state` passed in from `FilterableProductTable`.
현재 버전의 예시에서 무언가를 입력하거나 박스를 체크하려 해도 리액트는 여러분의 입력을 무시하는 것을 볼 수 있습니다. 이러한 현상은 의도적인데,`input`의 `value` prop이 `FilterableProductTable`에서 전달 된 `state`와 항상 같도록 설정했기 때문입니다.

Let's think about what we want to happen. We want to make sure that whenever the user changes the form, we update the state to reflect the user input. Since components should only update their own state, `FilterableProductTable` will pass callbacks to `SearchBar` that will fire whenever the state should be updated. We can use the `onChange` event on the inputs to be notified of it. The callbacks passed by `FilterableProductTable` will call `setState()`, and the app will be updated.
저희가 원하고자 하는 의도를 생각해봅시다. 저희는 유저가 폼을 변경할 때 state가 유저의 입력값을 반영하여 이를 업데이트 하고 싶은 것입니다. 컴포넌트는 오직 스스로의 state만을 업데이트해야 하기 때문에, `FilterableProductTable`은 state를 업데이트해야 할 때마다 실행되는 콜백을 `SearchBar`에 전달할 것입니다. Input에는 `onChange`이벤트를 사용하여 입력에 대한 알림을 받을것 입니다. `FilterableProductTable`에 의해 전달된 콜백은 `setState()`를 호출할 것이고, 앱은 업데이트 될 것 입니다.

Though this sounds complex, it's really just a few lines of code. And it's really explicit how your data is flowing throughout the app.
위의 설명이 굉장히 복잡하게 들리지만, 실제로는 단지 몇줄의 코드일 뿐입니다. 또한 데이터가 앱 전체에서 어떻게 흐르는지 굉장히 분명합니다.

## And That's It
## 이게 다입니다!

Hopefully, this gives you an idea of how to think about building components and applications with React. While it may be a little more typing than you're used to, remember that code is read far more than it's written, and it's extremely easy to read this modular, explicit code. As you start to build large libraries of components, you'll appreciate this explicitness and modularity, and with code reuse, your lines of code will start to shrink. :)
리액트를 사용하여 컴포넌트와 앱을 만들 때 생각하는 방법에 대해서 이 글이 도움이 되었기를 바랍니다. 여러분이 익숙한 것보다 조금 더 코드를 많이 작성했을지도 모르지만, 코드는 쓰는 것보다 더 많이 읽히고 이렇게 명시적이며 모듈화된 코드가 훨씬 더 쉽게 읽힌다는 사실을 기억하십시오. 큰 컴포넌트 라이브러리를 작성하기 시작하면서, 이러한 명시성과 모듈성, 그리고 코드 재사용에 대해 고맙게 생각할 것이고, 여러분의 코드량은 줄어들겠죠. :)

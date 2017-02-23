---
id: hello-world
title: Hello World
permalink: docs/hello-world.html
prev: installation.html
next: introducing-jsx.html
redirect_from:
  - "docs/index.html"
  - "docs/getting-started.html"
  - "docs/getting-started-ko-KR.html"
  - "docs/getting-started-zh-CN.html"
---

The easiest way to get started with React is to use [this Hello World example code on CodePen](http://codepen.io/gaearon/pen/ZpvBNJ?editors=0010). You don't need to install anything; you can just open it in another tab and follow along as we go through examples. If you'd rather use a local development environment, check out the [Installation](/react/docs/installation.html) page.

리액트를 시작하는 가장 쉬운 방법은 바로 [CodePen에서 Hello World 예시](http://codepen.io/gaearon/pen/ZpvBNJ?editors=0010)를 사용하는 것 입니다. 무언가를 설치할 필요없이 탭을 하나 열어서 예제를 진행하면서 따라하시면 됩니다. 혹시 로컬 개발 환경을 사용하고 싶다면 [Installation](/react/docs/installation.html)페이지를 확인해보세요.

The smallest React example looks like this:
가장 작은 리액트 예시는 다음과 같습니다:

```js
ReactDOM.render(
  <h1>Hello, world!</h1>,
  document.getElementById('root')
);
```

It renders a header saying "Hello, world!" on the page.
이 코드는 페이지에서 "Hello, world!"를 외치는 헤더를 렌더링하죠.

The next few sections will gradually introduce you to using React. We will examine the building blocks of React apps: elements and components. Once you master them, you can create complex apps from small reusable pieces.

지금부터 몇 단계를 거쳐 여러분에게 리액트를 사용하는 방법을 점진적으로 소개할 것입니다. 저희는 리액트앱의 빌딩 블록을 살펴볼 것입니다, 앨리먼트나 컴포넌트들을 말이죠. 여러분이 이것을 이해하고 숙달하면 이 작은 재사용 가능한 조각들을 사용하여 복잡한 애플리케이션을 만들 수 있습니다.

## A Note on JavaScript
## JavaScript에 대한 노트

React is a JavaScript library, and so it assumes you have a basic understanding of the JavaScript language. If you don't feel very confident, we recommend [refreshing your JavaScript knowledge](https://developer.mozilla.org/en-US/docs/Web/JavaScript/A_re-introduction_to_JavaScript) so you can follow along more easily.

리액트는 JavaScript 라이브러리이기 때문에 여러분이 JavaScript언어에 대한 기본적인 이해가 있을것이라고 가정하겠습니다. 혹시 해당 언어에 자신이 없으시다면 [JavaScript 지식 복습하기](https://developer.mozilla.org/en-US/docs/Web/JavaScript/A_re-introduction_to_JavaScript)를 살펴보시기를 권장합니다. 그래야 함께 따라하시기 더 편할테니까요.

We also use some of the ES6 syntax in the examples. We try to use it sparingly because it's still relatively new, but we encourage you to get familiar with [arrow functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions), [classes](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes), [template literals](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Template_literals), [`let`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let), and [`const`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const) statements. You can use <a href="http://babeljs.io/repl/#?babili=false&evaluate=true&lineWrap=false&presets=es2015%2Creact&experimental=false&loose=false&spec=false&code=const%20element%20%3D%20%3Ch1%3EHello%2C%20world!%3C%2Fh1%3E%3B%0Aconst%20container%20%3D%20document.getElementById('root')%3B%0AReactDOM.render(element%2C%20container)%3B%0A">Babel REPL</a> to check what ES6 code compiles to.

저희 예제에서는 ES6 문법 일부를 사용할 것입니다. ES6는 아직 새롭기 때문에 가능한 한 조금씩만 사용하려고 합니다. 하지만 여러분이 [화살표 함수](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions), [classes](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes), [template literals](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Template_literals), [`let`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let), 또는 [`const`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const) 선언, 등에 대해 익숙해지기를 권장합니다. <a href="http://babeljs.io/repl/#?babili=false&evaluate=true&lineWrap=false&presets=es2015%2Creact&experimental=false&loose=false&spec=false&code=const%20element%20%3D%20%3Ch1%3EHello%2C%20world!%3C%2Fh1%3E%3B%0Aconst%20container%20%3D%20document.getElementById('root')%3B%0AReactDOM.render(element%2C%20container)%3B%0A">Babel REPL</a>에서 ES6코드가 어떻게 컴파일 되는지 확인할 수 있습니다.

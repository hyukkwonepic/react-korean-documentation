---
id: installation
title: Installation
permalink: docs/installation.html
redirect_from:
  - "download.html"
  - "downloads.html"
  - "docs/tooling-integration.html"
  - "docs/package-management.html"
  - "docs/language-tooling.html"
  - "docs/environments.html"
next: hello-world.html
---

React is flexible and can be used in a variety of projects. You can create new apps with it, but you can also gradually introduce it into an existing codebase without doing a rewrite.
리액트는 유연하고 다양한 프로젝트에 사용될 수 있습니다. 리액트를 사용하여 새로운 앱을 만들 수 있고 이미 존재하는 코드베이스를 새로 작성하지 않고 점진적으로 적용할 수 있습니다.

## Trying Out React
## 리액트 사용해보기
If you're just interested in playing around with React, you can use CodePen. Try starting from [this Hello World example code](http://codepen.io/gaearon/pen/rrpgNB?editors=0010). You don't need to install anything; you can just modify the code and see if it works.

리액트를 가지고 놀아보고 싶으시다면 CodePen을 사용할 수 있습니다. 여기의 [Hello World 예시 코드](http://codepen.io/gaearon/pen/rrpgNB?editors=0010)부터 시작해보세요. 무언가를 설치할 필요없이 코드를 수정하면서 어떻게 작동하는지 살펴보세요.

If you prefer to use your own text editor, you can also <a href="/react/downloads/single-file-example.html" download="hello.html">download this HTML file</a>, edit it, and open it from the local filesystem in your browser. It does a slow runtime code transformation, so don't use it in production.

혹시 여러분의 텍스트 에디터를 사용하고 싶으시면, <a href="/react/downloads/single-file-example.html" download="hello.html">HTML 파일 다운로드</a>를 통해 수정하고 여러분의 브라우저에서 로컬 파일시스템으로 열 수 있습니다. 이것은 느린 속도의 런타임 코드 변환을 하기 때문에 배포 단에서 사용하지는 마십시오.

## Creating a Single Page Application
## Single Page Application(단일 페이지 애플리케이션) 만들기
[Create React App](http://github.com/facebookincubator/create-react-app) is the best way to start building a new React single page application. It sets up your development environment so that you can use the latest JavaScript features, provides a nice developer experience, and optimizes your app for production.
[Create React App](http://github.com/facebookincubator/create-react-app) 은 새 React 단일 페이지 애플리케이션을 만들기 시작하기 가장 좋은 방법입니다. 이것은 개발 환경을 설정하기 때문에 최신 자바스크립트 기능을 사용할 수 있고 훌륭한 개발 경험을 제공하며 여러분의 앱 배포를 최적화합니다.

```bash
npm install -g create-react-app
create-react-app hello-world
cd hello-world
npm start
```

Create React App doesn't handle backend logic or databases; it just creates a frontend build pipeline, so you can use it with any backend you want. It uses [webpack](https://webpack.js.org/), [Babel](http://babeljs.io/) and [ESLint](http://eslint.org/) under the hood, but configures them for you.

Create React App 은 백엔드 로직이나 데이터베이스를 다루지 않습니다. 단지 프론트엔드 빌드의 파이프라인만을 만들기 때문에 여러분이 원하는 백엔드 시스템과 함께 사용할 수 있습니다. 이 속에는 [webpack](https://webpack.js.org/), [Babel](http://babeljs.io/) [ESLint](http://eslint.org/) 를 사용하고 여러분에게 맞춰 설정되어 있습니다.
## Adding React to an Existing Application
## 이미 존재하는 애플리케이션에 React 추가하기
You don't need to rewrite your app to start using React.
React를 사용하기 위해 여러분의 앱을 재작성할 필요가 없습니다.

We recommend adding React to a small part of your application, such as an individual widget, so you can see if it works well for your use case.
저희는 여러분의 애플리케이션의 작은 부분에 React를 추가하시기를 권장합니다. 예를 들면 개별 위젯같은 것을 달아보시고 여러분의 사용례에 잘 적용되는지 확인해보세요.

While React [can be used](/react/docs/react-without-es6.html) without a build pipeline, we recommend setting it up so you can be more productive. A modern build pipeline typically consists of:
React는 빌드 파이프라인 [없이 사용될 수 있습니다](/react/docs/react-without-es6.html). 하지만 더 높은 생산성을 위해 이를 설정하시기를 권장드립니다.

* A **package manager**, such as [Yarn](https://yarnpkg.com/) or [npm](https://www.npmjs.com/). It lets you take advantage of a vast ecosystem of third-party packages, and easily install or update them.
* **패키지 매니저** [Yarn](https://yarnpkg.com/) or [npm](https://www.npmjs.com/). Yarn 또는 npm을 이용하여 방대한 써드파티 환경을 이용할 수 있고 쉽게 설치하거나 업데이트 할 수 있습니다.
* A **bundler**, such as [webpack](https://webpack.js.org/) or [Browserify](http://browserify.org/). It lets you write modular code and bundle it together into small packages to optimize load time.
* **번들러(bundler)** [webpack](https://webpack.js.org/) or [Browserify](http://browserify.org/). Webpack 또는 Browserify를 이용하여 모듈화된 코드를 작성하고 작은 패키지로 하나로 묶어(bundle) 로드되는 시간을 최적화 할 수 있습니다.
* A **compiler** such as [Babel](http://babeljs.io/). It lets you write modern JavaScript code that still works in older browsers.
* **컴파일러** [Babel](http://babeljs.io/). Babel을 사용하여 최신 JavaScript 코드를 작성하여 구버전의 브라우저에서도 작동할 수 있게 합니다.
### Installing React
### 리액트 설치하기

We recommend using [Yarn](https://yarnpkg.com/) or [npm](https://www.npmjs.com/) for managing front-end dependencies. If you're new to package managers, the [Yarn documentation](https://yarnpkg.com/en/docs/getting-started) is a good place to get started.
저희는 [Yarn](https://yarnpkg.com/) 또는 [npm](https://www.npmjs.com/) 을 사용하여 프론트엔드 디펜던시를 관리하길 권장드립니다. 혹시 여러분이 패키지 매니저를 접하신적이 없으시다면 [Yarn 문서](https://yarnpkg.com/en/docs/getting-started) 를 통해 시작하시면 참 좋습니다.

To install React with Yarn, run:
Yarn을 통해 React를 설치하려면 아래의 코드를 실행하세요:

```bash
yarn init
yarn add react react-dom
```

To install React with npm, run:
npm을 통해 React를 설치하려면 아래의 코드를 실행하세요:
```bash
npm init
npm install --save react react-dom
```

Both Yarn and npm download packages from the [npm registry](http://npmjs.com/).
Yarn 과 npm 다운로드 패키지: [npm registry](http://npmjs.com/).

### Enabling ES6 and JSX
### ES6와 JSX 사용하기

We recommend using React with [Babel](http://babeljs.io/) to let you use ES6 and JSX in your JavaScript code. ES6 is a set of modern JavaScript features that make development easier, and JSX is an extension to the JavaScript language that works nicely with React.
저희는 여러분의 JavaScript코드에서 ES6와 JSX를 사용할 수 있도록 React에서 [Babel](http://babeljs.io/)을 사용하시기를 권장드립니다. ES6는 모던 JavaScript 기능의 집합으로써 개발을 더 쉽게 할 수 있도록 하고 JSX는 JavaScript 언어의 확장으로써 React와 잘 어울립니다.

The [Babel setup instructions](https://babeljs.io/docs/setup/) explain how to configure Babel in many different build environments. Make sure you install [`babel-preset-react`](http://babeljs.io/docs/plugins/preset-react/#basic-setup-with-the-cli-) and [`babel-preset-es2015`](http://babeljs.io/docs/plugins/preset-es2015/#basic-setup-with-the-cli-) and enable them in your [`.babelrc` configuration](http://babeljs.io/docs/usage/babelrc/), and you're good to go.

[Babel 셋업 가이드](https://babeljs.io/docs/setup/)는 수많은 다양한 빌드 환경에서 Babel을 설정하는 법을 설명합니다. 반드시 [`babel-preset-react`](http://babeljs.io/docs/plugins/preset-react/#basic-setup-with-the-cli-)와 [`babel-preset-es2015`](http://babeljs.io/docs/plugins/preset-es2015/#basic-setup-with-the-cli-)를 설치하고 여러분의 [`.babelrc` configuration](http://babeljs.io/docs/usage/babelrc/)에서 설정하십시오. 이제 진행해도 좋습니다.


### Hello World with ES6 and JSX
### ES6와 JSX로 Hello World

We recommend using a bundler like [webpack](https://webpack.js.org/) or [Browserify](http://browserify.org/) so you can write modular code and bundle it together into small packages to optimize load time.
저희는 [webpack](https://webpack.js.org/) 또는 [Browserify](http://browserify.org/)와 같은 번들러를 사용하시기를 권장드립니다. 그래야만 모듈화된 코드를 작성할 수 있고 로딩 시간이 최적화된 작은 패키지로 묶을 수 있습니다.

The smallest React example looks like this:
가장 작은 React 예시는 아래와 같습니다:

```js
import React from 'react';
import ReactDOM from 'react-dom';

ReactDOM.render(
  <h1>Hello, world!</h1>,
  document.getElementById('root')
);
```

This code renders into a DOM element with the id of `root` so you need `<div id="root"></div>` somewhere in your HTML file.
해당 코드는 `root` id를 가진 DOM 엘리먼트에 렌더링되기 때문에 여러분의 HTML파일 어딘가에 반드시 `<div id="root></div>`코드가 존재해야 합니다.

Similarly, you can render a React component inside a DOM element somewhere inside your existing app written with any other JavaScript UI library.
마찬가지로, 다른 JavaScript UI 라이브러리를 사용하여 이미 작성된 앱 어딘가의 DOM 앨리먼트 내부에 React 컴포넌트를 렌더링 할 수 있습니다.

### Development and Production Versions
### 개발 버전과 프로덕션 버전

By default, React includes many helpful warnings. These warnings are very useful in development. However, they make React larger and slower so you should make sure to use the production version when you deploy the app.
기본적으로 React는 다양한 경고알림을 내포하고 있습니다. 이 경고들은 개발 단계에서 굉장히 유용합니다. 하지만 이 경고들은 React를 크고 무겁게 만들기 때문에 앱을 배포할 때에는 반드시 프로덕션 버전을 사용해야 합니다.

#### Create React App
#### Create React App

If you use [Create React App](https://github.com/facebookincubator/create-react-app), `npm run build` will create an optimized build of your app in the `build` folder.
만약 여러분이 [Create React App](https://github.com/facebookincubator/create-react-app)을 사용하신다면 `npm run build` 명령어는 여러분의 `build` 폴더 내부에 여러분에게 최적화된 앱을 생성할 것입니다.

#### Webpack
#### Webpack
Include both `DefinePlugin` and `UglifyJsPlugin` into your production Webpack configuration as described in [this guide](https://webpack.js.org/guides/production-build/).
`DefinePlugin`과 `UglifyJsPlugin`을 [여기서 설명된 것처럼](https://webpack.js.org/guides/production-build/) 배포 Webpack 설정에 추가하세요.

#### Browserify
#### Browserify
Run Browserify with `NODE_ENV` environment variable set to `production` and use [UglifyJS](https://github.com/mishoo/UglifyJS) as the last build step so that development-only code gets stripped out.
`NODE_ENV` 환경 변수를 `production`으로 설정한 후에 Browserify를 실행해주세요. 그리고 [UglifyJS](https://github.com/mishoo/UglifyJS)를 마지막 빌드 단계에서 사용하여 개발 환경에서만 사용되는 코드를 빼버릴 수 있습니다.

#### Rollup
#### Rollup
Use [rollup-plugin-replace](https://github.com/rollup/rollup-plugin-replace) plugin together with [rollup-plugin-commonjs](https://github.com/rollup/rollup-plugin-commonjs) (in that order) to remove development-only code. [See this gist](https://gist.github.com/Rich-Harris/cb14f4bc0670c47d00d191565be36bf0) for a complete setup example.

[rollup-plugin-replace](https://github.com/rollup/rollup-plugin-replace) 플러그인을 [rollup-plugin-commonjs](https://github.com/rollup/rollup-plugin-commonjs)와 함께 사용하여 (해당 순서대로) 개발 환경에서만 사용되는 코드를 제거하세요. [해당 gist를 참고](https://gist.github.com/Rich-Harris/cb14f4bc0670c47d00d191565be36bf0)하여 설정 예제를 확인하세요.

### Using a CDN
### CDN(컨텐츠 전송 네트워크)을 사용하기
If you don't want to use npm to manage client packages, the `react` and `react-dom` npm packages also provide single-file distributions in `dist` folders, which are hosted on a CDN:
만일 npm을 통해 클라이언트 패키지들을 관리하고 싶지 않다면, `react`와 `react-dom` npm 패키지들은 `dist`폴더 내부에 단일 파일 배포를 제공하고 CDN을 통해 호스트 됩니다:

```html
<script src="https://unpkg.com/react@15/dist/react.js"></script>
<script src="https://unpkg.com/react-dom@15/dist/react-dom.js"></script>
```

The versions above are only meant for development, and are not suitable for production. Minified and optimized production versions of React are available at:
위 버전들은 개발 환경을 위한 것이기 때문에 프로덕션에는 적합하지 않습니다. 최소화 및 최적화 된 React 프로덕션 버전은 아래를 통해 사용 가능합니다:

```html
<script src="https://unpkg.com/react@15/dist/react.min.js"></script>
<script src="https://unpkg.com/react-dom@15/dist/react-dom.min.js"></script>
```

To load a specific version of `react` and `react-dom`, replace `15` with the version number.
특정 버전의 `react`와 `react-dom`을 로드하기 위해선 `15`를 원하는 버전 숫자로 바꾸시면 됩니다.

If you use Bower, React is available via the `react` package.
만일 Bower를 사용하신다면 React는 `react` 패키지를 통해 사용할 수 있습니다.

# [JavaScript] 49장 Babel 과 Webpack 을 이용한 ES6+/ES.NEXT 개발 환경 구축

크롬, 사파리, 파이어폭스, 엣지 같은 에버그린 브라우저의 ES6 지원율은 약 98% 로 거의 대부분의 ES6 사양을 지원한다.
하지만 IE11 의 ES6 지원율은 11%다.
그리고 매년 새롭게 도입되는 ES6 이상의 버전과 제안 단계에 있는 ES 제안사항(ES.NEXT)은 브라우저에 따라 지원율이 제각각이다.

따라서 ES6+ 와 [ES.NEXT](http://ES.NEXT) 의 최신 ECMAScript 사양을 사용하여 프로젝트를 진행하려면 최신 사양으로 작성된 코드를 경우에 따라 IE 를 포함한 구형 브라우저에서 문제 없이 동작시키기 위한 개발 환경을 구축하는 것이 필요하다.

- IE 를 포함한 구형 브라우저는 ESM 을 지원X
- ESM 을 사용하더라도 트랜스파일링이나 번들링이 필요한 것은 변함이 없다.
- ESM 이 아직 지원하지 않는 기능(bare import 등)이 있고 점차 해결되고는 있지만 몇가지 이슈가 존재한다.

# 49.1 Babel

다음 예제에서는 ES6 의 화살표 함수와 ES7 의 지수 연산자를 사용하고 있다.

```jsx
[1, 2, 3].map(n => n ** n)
```

IE 같은 구형 브라우저에서는 ES6 의 화살표 함수와 ES7 의 지수 연산자를 지원하지 않을 수 있다.
Babel 을 사용하면 ES5 사양으로 변환할 수 있다.

```jsx
"use strict"

[1, 2, 3].map(function(n) {
	return Math.pow(n, n)
})
```

이처럼 Babel 은 ES6+/ES.NEXT 로 구현되니 최신 사양의 소스코드를 구형 브라우저에서도 동작할 수 있는 ES5 사양의 소스코드로 변환(트랜스파일링)할 수 있다.
Babel 을 사용하기 위한 개발 환경을 구축하면 다음과 같다.

## 49.1.1 Babel 설치

---

npm 을 이용하여 Babel 을 설치하면 다음과 같다.

```bash
# 프로젝트 폴더 생성
mkdir esnext-project && cd esnext-project
# package.json 생성
npm init -y
# babel-core, babel-cli 설치
npm install --save-dev @babel/core @babel/cli
```

참고로 Babel, Webpack, 플러그인의 버전은 빈번하게 업그레이드 된다. npm install 은 언제나 최신 버전의 패키지를 설치하므로 패키지 이름 뒤에 `@` 로 설치하고 싶은 버전을 지정할 수 있다.

## 49.1.2 Babel 프리셋 설치와 babel.config.json 설정 파일 작성

---

Babel을 사용하려면 `@babel/preset-env` 를 설치해야 한다. `@babel/preset-env` 는 함께 사용되어야 하는 Babel 플러그인을 모아둔 것으로 Babel 프리셋이라 부른다.
Babel 이 제공하는 프리셋은 다음과 같다.

- `@babel/preset-env`
- `@babel/preset-flow`
- `@babel/preset-react`
- `@babel/preset-typescript`

`@babel/preset-env` 는 필요한 플러그인들을 프로젝트 지원 환경에 맞춰 동적으로 결정해준다. 프로젝트 지원 환경은 Browserlist 형식으로 `.browserlistrc` 파일에서 상세히 설정할 수 있다.
생략할 경우 기본값으로 설정된다.

```bash
# @babel/preset-env 설치
npm install --save-dev @babel/preset-env
```

설치가 완료되면 프로젝트 루트 폴더에 있는 `babel.config.json` 파일을 생성하고 다음과 같이 작성한다.
설치한 @babel/preset-env 파일을 사용하겠다는 의미다.

```jsx
{
	"presets": ["@babel/preset-env"]
}
```

## 49.1.3 트랜스파일링

---

Babel을 사용하여 ES6+/ES.NEXT 사양의 소스코드를 ES5 사양의 소스코드로 트랜스 파일링을 해본다.
Babel CLI 명령어를 사용할 수도 있으나 번거로우므로 npm scripts 에 CLI 명령어를 등록해놓는다.

```jsx
// package.json
{
	...
	"scripts": {
		"build": "babel src/js -w -d dist/js"
	}
}
```

위 npm scripts 의 build 는 src/js 폴더(타깃 폴더)에 있는 모든 자바스크립트 파일들을 트랜스파일링한 후, 그 결과물을 dist/js 폴더에 저장한다.

사용한 옵션은 다음과 같은 의미를 갖는다

- `-w` : 타깃 폴더에 있는 모든 자바스크립트 파일들의 변경을 감지하여 자동으로 트랜스파일 한다 ( === —watch 옵션의 축약 )
- `-d` : 트랜스파일링된 결과물이 저장될 폴더를 지정한다. 만약 지정된 폴더가 존재하지 않으면 자동 생성된다 ( === —out-dir 옵션의 축약 )

## 49.1.4 Babel 플러그인 설치

---

설치가 필요한 Babel 플러그인은 Babel 홈페이지에서 검색할 수 있다.

public/private 클래스 필드를 지원하는 `@babel/plugin-proposal-class-properties` 플로그인을 설치해본다.

```bash
npm install --save-dev @babel/plugin-propasal-class-properties
```

설치한 플러그인은 babel.config.json 에 추가한다.

```jsx
{
	"presets": ["@babel/preset-env"],
	"Plugins": ["@babel/plugin-proposal-class-properties"]
}
```

다시 터미널에서 아래와 같이 명령어를 입력하여 트랜스 파일링을 한다.

```bash
npm run build

> exnext-project@1.0.0 build /Users/KMin/Desktop/esnext-project
> babel src/js -w -d dist/js

Successfully compiled 2 files with Babel (954ms)
```

# 49.2 Webpack

Webpack 은 외존 관계에 있는 자바스크립트, CSS, 이미지 등의 리소스들을 하나(또는 여러개)의 파일로 번들링하는 모듈 번들러다.
Webpack 을 사용하면 의존 모듈이 하나의 파일로 번들링되므로 별도의 모듈로 로더가 필요 없다.
그리고 여러 개의 자바스크립트 파일을 하나로 번들링하므로 HTML 파일에서 script 태그로 여러 개의 자바스크립트 파일을 로드해야 하는 번거로움도 없다.

## 49.2.1 Webpack 설치

---

터미널에 아래와 같이 입력해 Webpack 을 설치한다.

```bash
npm install --save-dev webpack webpack-cli
```

## 49.2.2 babel-loader 설치

---

Webpack 이 모듈을 번들링할 때 Babel 을 사용하여 ES6+/ES.NEXT 사양의 소스코드를 ES5 사양의 소스코드로 트랜스파일링하도록 `babel-loader` 를 설치한다.

```bash
npm install --save-dev babel-loader
```

이제 npm scripts 를 변경하여 Babel 대신 Webpack 을 실행하도록 수정한다.

```jsx
// package.json
{
	...
	"scripts": {
		"build": "webpack -w"
	}
}
```

## 49.2.3 webpack.config.js 설정 파일 작성

---

`webpack.config.json` 는 Webpack 이 실행될 때 참조하는 설정 파일이다.
프로젝트 루트 폴더에 webpack.config.js 파일을 생성하고 다음과 같이 작성한다.

```jsx
const path = require('path')

module.exports = {
	entry: './src/js/main.js',
	output: {
		path: path.resolve(__dirname, 'dist'),
		filename: 'js/bundle.js'
	}
	module: {
		rules: [
			{
				test: /\.js$/,
				include: [
					path.resolve(__dirname, 'src/js')
				],
				exclude: /node_modules/,
				use: {
					loader: 'babel-loader',
					options: {
						presets: ['@babel/preset-env'],
						plugins: ['@babel/plugin-proposal-class-properties']
					}
				}
			}
		]
	},
	devtool: 'source-map',
	mode: 'devlopment'
}
```

이제  Webpack 을 실행하여 트랜스파일링 및 번들링을 실행해보자.
트랜스 파일링은 Babel 이 수행하고 번들링은 Webpack 이 수행한다.

```bash
npm run build
```

## 49.2.4 babel-ployfill 설치

---

Babel 을 사용하여 ES6+/ES.NEXT 사양의 소스코드를 ES5 사양의 소스코드로 트랜스파일링해도 브라우저가 지원하지 않는 코드가 남을 수 있다. Promise, Object.assign, Array.from 등은 트랜스파일링해도 ES5 대체할 기능이 없기 때문에 트랜스파일링 되지 못하고 그대로 남는다.

터미널에 `@babel/polyfill` 을 설치한다

```bash
npm install @babel/polyfill
```

`@babel/polyfill` 은 개발환경 뿐만 아니라 실제 운영 환경에서도 사용해야 하므로 개발 의존성 옵션인 `--save-dev` 옵션은 뺴준다.

ES6 의 import 를 사용하는 경우에는 진입점의 선두에서 먼저 폴리필을 로드하도록 한다.

```jsx
// src/js/main.js
import "@babel/polyfill"
import { pi, power, Foo } from "./lib"
...
```

Webpack 을 사용하는 경우 위 방법 대신 webpack.config.js 파일의 entry 배열에 폴리필을 추가한다.
```jsx
const path = require('path')

module.exports = {

	entry: ['@babel/polyfill', './src/js/main.js']
  ...
```
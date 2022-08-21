# chapter 4

## 리액트의 작동 원리

리액트를 브라우저에서 다루려면 React와 ReactDOM 라이브러리를 불러와야함.

- React: 뷰를 만들기 위한 라이브러리
- ReactDOM: UI를 실제로 브라우저에 렌더링할 때 사용하는 라이브러리

### 리액트 엘리먼트

- React.createElement를 사용해서 Virtual DOM을 만들 수 있음

```jsx
React.createElement("h1", { id: "recipe-0" }, "구운 연어");
```

- 위 함수 호출문은 아래와 같은 가상 돔을 리턴

```jsx
{
	$$typeof: Symbol(React.element),
	type: 'h1',
	key: null,
	ref: null,
	props: {id: "recipe-0", children: "구운 연어"},
	_owner: null,
	_store: {}
}
```

- type, props, children을 이용해 dom에 대한 정보를 저장

### ReactDOM

- ReactDOM에는 리액트 엘리먼트를 브라우저에 렌더링하는데 필요한 모든 도구가 들어 있음

```jsx
var dish = React.createElement('h1', null, '구운 연어');
ReactDOM.render(dish, document.getElementById('root'));
```

- 위 코드는 아래와 같은 dom을 만들어 return함.

```jsx
<body>
	<div id="root">
		<h1>구운 연어</h1>
	</div>
</body>
```

- React.createElement의 세번째 인자로 React.createElement를 넘겨 더 복잡한 DOM을 만들 수 있음

```jsx
React.createElement(
	"ul",
	null,
	React.createElement("li",null,"연어 900 그램"),
	React.createElement("li",null,"신선한 로즈마리 5가지"),
	React.createElement("li",null,"올리브 오일 2 테이블 스푼"),
	React.createElement("li",null,"작은 레몬 2조각"),
	React.createElement("li",null,"코셔 소금 1 티스푼"),
	React.createElement("li",null,"다진 마늘 4쪽")
);
```

- output(가상 DOM)

```jsx
{
	type: 'ul',
	props: {
		children: [
			{type: 'li', props: { children: '연어 900 그램'} ... },
			{type: 'li', props: { children: '신선한 로즈마리 5가지'} ... },
			{type: 'li', props: { children: '올리브 오일 2 테이블 스푼'} ... },
			{type: 'li', props: { children: '작은 레몬 2조각'} ... },
			{type: 'li', props: { children: '코셔 소금 1 티스푼'} ... },
			{type: 'li', props: { children: '다진 마늘 4쪽'} ... },
		]
		...
	}
}
```

- Real DOM으로 변환

```jsx
<ul>
	<li>연어 900 그램</li>
	<li>신선한 로즈마리 5가지</li>
	<li>올리브 오일 2 테이블 스푼</li>
	<li>작은 레몬 2조각</li>
	<li>코셔 소금 1 티스푼</li>
	<li>다진 마늘 4쪽</li>
</ul>
```

- 위에서 볼 수 있 듯 3번째 이후 인자는 배열로 전달되게 된다. createElement를 유추해본다면? 대략 이렇게 생겼을거라 추측할 수 있음.

```jsx
const createElement = (type, props, ...children) => (
	{ type, props, children: children.flat() }
}
```

- 하지만, createElement 함수로 매번 VirtualDOM을 생성해 줄 수는 없고, 실제로는 JSX를 이용해서 개발함. JSX를 통해 작성하면 **Babel을 통해 createElement 함수로 transpiling함.**

[Babel test](https://babeljs.io/repl/#?browsers=defaults%2C%20not%20ie%2011%2C%20not%20ie_mob%2011&build=&builtIns=false&corejs=3.21&spec=false&loose=false&code_lz=MYewdgzgLgBFCm0DCIC2AHc8ywLwwAoBKGXAPhgG8AoGGAJ3igFd6xDa6uAeAEwEsAbmU5ceEdAEMwZABb9uAegnSRYsdxUzGk4LABGk_fAA2cRFCVa16pQOGci1AL5A&debug=false&forceAllTransforms=false&shippedProposals=false&circleciRepo=&evaluate=false&fileSize=false&timeTravel=false&sourceType=module&lineWrap=true&presets=env%2Creact%2Cstage-2&prettier=false&targets=&version=7.18.12&externalPlugins=&assumptions=%7B%7D)

```jsx
const testComponent = () => {
  return (
      <div>
        <span>hi</span>
        <span>react babel test</span>
      </div>
  )
}
```

output

```jsx
"use strict";

const testComponent = () => {
  return /*#__PURE__*/React.createElement("div", null, 
		/*#__PURE__*/React.createElement("span", null, "hi"), 
		/*#__PURE__*/React.createElement("span", null, "react babel test"));
};
```
## 자바스크립트를 활용한 함수형 프로그래밍

### 3.1 함수형 프로그래밍이란 무엇인가?

- 자바스크립트에서는 함수가 1급 객체임.
- 1급 객체란 말은 정수나 문자열 같은 다른 일반적인 값과 마찬가지로 함수를 취급할 수 있다는 뜻.

1급 객체의 특징

- 변수에 대입 가능
- 객체에 넣을 수 있음
- 함수를 인자로 넘길 수 있음
- 함수를 반환할 수 있음

```jsx
//변수에 대입 가능
const log = (message) => console.log(message);

// 객체에 넣을 수 있음
const obj = {
	message: '함수를 다른 값과 마찬가지로 추가할 수 있습니다.',
	log(message){ console.log(msg) }
}

// 배열에 넣을 수 있음
const messages = [
	'함수를 배열에 넣을 수도 있습니다.'
	(msg) => console.log(msg);
]
messages[1](messages[0]) // '함수를 배열에 넣을 수도 있습니다.'

// 함수를 인자로 넣을 수 있음.
const insideFn = (logger) => { loggger('함수를 다른 함수에 인자로 넘길 수도 있습니다.')};

// 함수를 return 할 수 있음.
const createScream = (callbackFn) => (msg) => { callbackFn(msg) };
const makeScream = createScream((msg) => console.log(msg.toUpperCase()));
makeScream('Scream!!!');

```

### 명령형 프로그래밍 vs 선언적 프로그래밍

선언적 프로그래밍이란?

- 필요한 것을 달성하는 과정을 하나하나 기술하는 것보다 필요한 것이 어떤 것인지를 기술하는 것에 더 방점을 두고 어플리에키션의 구조를 세워나가는 스타일

명령형 프로그래밍과 비교하며 선언적 프로그래밍에 대한 개념을 잡아보자.

명령형

- 무엇을 ‘어떻게’ 할 것이다.

```jsx
let string = 'THis is the midday show with Cheryl Waters';
let urlFriendly = "";

for(let i = 0 ; i < string.length ; i++){
    if(string[i] === " "){
        urlFriendly += "-";
    }else{
        urlFriendly += string[i];
    }
}

console.log(urlFriendly);
```

선언형

- ‘무엇’을 할 것이다.

```jsx
const string = 'This is the midday show with Cheryl Waters';
const urlFriendly = string.replace(/ /g, '-');

console.log(urlFriendly);
```

- 선언적 프로그래밍은, 어떤 일이 발생해야 하는지에 대해서만 알려주고 어떻게 해당 일이 발생하는지 방법에 대한 기술은 추상화 아랫단으로 감춤.
- 선언적 접근 방식이 더 읽기 쉽고, 그렇기 때문에 더 추론하기 쉬움.

명령형(바닐라)

```jsx
var target = document.getElementById("target");
var wrapper = document.createElement("div");
var headline = document.createElement("h1");

wrapper.id = "welcome";
headline.innerText = "Hello World";

wrapper.appendChild(headline);
target.appendChild(wrapper);
```

선언형(리액트)

```jsx
const { render } = ReactDOM;

const Welcome = () => (
	<div id="welcome">
		<h1>Hello World</h1>
	</div>
)

render(<Welcome />, document.getElementById('target'));
```

- Welcome 컴포넌트는 rendering할 dom에 대해 기술하고, 실제 만드는 부분은 render라는 메서드로 추상화함. 위 코드는 welcome이라는 컴포넌트를 target이라는 ID를 가진 곳에 렌더링하고 싶어 한다는 프로그래머의 의도가 더 드러남.

### 3.3 함수형 프로그래밍의 개념

- 불변성, 순수성, 데이터 변환, 고차함수, 재귀, 합성에 대해 소개

불변성

- 원본 데이터 구조를 변경하는 대신 그 데이터 구조의 복사본을 만들어 수정

순수 함수

- 파라미터에 의해서만 반환값이 결정되는 함수
- 하나 이상의 인자를 받고, 인자가 같으면 항상 같은 값을 반환함
- 외부에 부수효과를 발생시키지 않음
- 인자나 함수 밖에 있는 다른 변수를 변경하거나, 입출력을 수행하지 않음

```jsx
const fred = {
	name: "fred",
	canRead: false,
	canWrite: false
}

const selfEducate = (person) => {
	person.canRead = true;
	person.canWrite = true;
	return person;
} // person의 field를 수정하므로 외부에 부수효과를 발생시킨다. 순수함수가 아님

// 순수함수
const pureSelfEducate = (person) => ({...person, canRead: true, canWrite: true})
```

### 3.4 고차함수

- 다른 함수를 인자로 받을 수 있거나 함수를 반환할 수 있는 함수
- 커링: 고차 함수 사용법과 관련된 프로그래밍 기법으로, 어떤 연산을 수행할 때 필요한 값 중 일부만 저장하고 나중에 나머지 값을 전달받는 기법. 이를 위해 다른 함수를 반환하는 함수를 사용하며, 이를 커링된 함수라고 부른다.

```jsx
const userLogs = userName => message => 
	console.log(`${userName} => ${message}`);

const log = userLogs('grandpa23');
log("attempted to load 20 fake members"); // grandpa23 -> attempted to load 20 fake members
```

### 3.5 재귀함수

- 자기 자신을 호출하는 함수

### 3.6 합성함수

- 함수형 프로그램은 로직을 구체적인 작업을 담당하는 여러 작은 순수함수로 나눈다. 이 과정에서 언젠가는 모든 작은 함수를 한데 합칠 필요가 있다.
- 즉, 각 함수를 서로 연쇄적으로 또는 병렬로 호출하거나 여러 작은 함수를 조합해서 더 큰 함수로 만드는 과정으 반복해서 전체 어플리케이션을 구축해야 한다.

```jsx
const both = data => appendAMPM(civilianHours(date));
```

civilianHours의 출력이 appendAMPM의 입력이 되고있음. 지금은 depth가 2지만 더 깊어지면 어떨까?

더 우아한 방식은 함수를 조합해주는 compose 함수를 사용하는 것.

```jsx
const both = compose(
	civilianHours,
	appendAMPM
);

both(new Date());
```

compose 함수의 구현체는 아래와 같다.

```jsx
const compose = (...fns) => (initialValue) => (
	fns.reduce((prevResult, fn)=> fn(prevResult),initialValue);
)
```
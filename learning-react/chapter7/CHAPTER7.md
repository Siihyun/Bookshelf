# chapter 7

## 훅스로 컴포넌트 개선하기

> 💡챕터요약
>
> useEffect, useLayoutEffect, useReducer, useMemo, useCallback hooks의 원리와 용도에 대해 설명


### useLayoutEffect를 사용해야 하는 경우

useLayoutEffect 실행 순서는 아래와 같음

- 렌더링
- useLayoutEffect 호출
- 브라우저의 화면 그리기: 이 시점에 DOM 만들어짐
- useEffect 호출

useLayoutEffect는 언제 써야할까?
- 사용하는 효과가 브라우저의 화면 그리기에 필수적인 경우. 
- ex) 창의 크기가 바뀌었을때 엘리먼트의 width, height 알고 싶을 때

```jsx
function useWindowSize{
  const [width, setWidth] = useState(0);
  const [height, setHeight] = useState(0);

  const resize = () => {
    setWidth(window.innerWidth);
    setHeight(window.innerHeight);
  }

  useLayoutEffect(() => {
    window.addEventListener('resize',resize);
    resize();
    return () => {
      window.removeEventListener('resize');
    }
  },[])

  return [width, height];
}
```

### useReducer

usestate를 대체할 수 있는 훅
- state update 로직을 컴포넌트 밖에서 처치할 수 있게 해줌
- 아래와 같은 형식으로 사용되며, reducer의 첫 인자로는 state, 두번째 인자로는 전달한 인자가 전달됨.
  
  ```
  const [state, dispatch] = useReducer(reducer, initialArg);
  ```


  
- 예시 코드
```jsx
const initialState = {count: 0};

function reducer(state, action) {
  switch (action.type) {
    case 'increment':
      return {count: state.count + 1};
    case 'decrement':
      return {count: state.count - 1};
    default:
      throw new Error();
  }
}

function Counter() {
  const [state, dispatch] = useReducer(reducer, initialState);
  return (
    <>
      Count: {state.count}
      <button onClick={() => dispatch({type: 'decrement'})}>-</button>
      <button onClick={() => dispatch({type: 'increment'})}>+</button>
    </>
  );
}
```



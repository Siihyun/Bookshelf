# chapter 6

## React 상태관리

### 제어 컴포넌트 vs 비제어 컴포넌트


```jsx
const App = () => {
  const ref = useRef(); // 비제어 컴포넌트
  const [value, setValue] = useState(''); // 제어 컴포넌트

  const onSubmit = (e) => {
    e.preventDefault();
    api.post(name: ref.current.value, address: value)
  }
  
  return(
    <form>
      <input name='name' ref={ref}>/<input> 
      <input name='address' value={value} onChange={(e) => setValue(e.target.value)}>/<input>
    </form>
  )
}
```

### Context API

> context를 이용하면 단계마다 일일이 props를 넘겨주지 않고도 컴포넌트 트리 전체에 데이터를 제공할 수 있습니다.


- react에서 전역 데이터 관리 보다는 props drilling의 해결책으로 context를 제시함
- context의 주된 용도는 다양한 레벨에 네스팅된 *많은* 컴포넌트에게 데이터를 전달하는 것 
- context를 사용하면 컴포넌트를 재사용하기가 어려워지므로 꼭 필요할 때만 써야함

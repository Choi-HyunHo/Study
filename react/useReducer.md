# useReducer
컴포넌트에서 상태변화 로직을 분리

<br>

## 필요성
![KakaoTalk_20220411_152930681](https://user-images.githubusercontent.com/87301268/162678037-5f47f5d7-33cf-41d8-a9ee-42d6fdfb328e.jpg)
- `useState` 를 이용하면 위의 처럼 상태 변화 함수를 각각 `Counter` 컴포넌트 안에
작성을 해야 했습니다.
- 이와 같은 경우 컴포넌트의 코드가 길어지고 복잡 할 수 밖에 없습니다.

<br>

## useReducer 을 사용하면
![KakaoTalk_20220411_152930959](https://user-images.githubusercontent.com/87301268/162678039-289507a2-4c50-475b-8172-aca23bce390a.jpg)

- 왼쪽에 보이는 `reducer` 라는 상태 변화 함수를 컴포넌트 밖으로 분리를 하여 다양한 상태변화 로직을 컴포넌트 외부로 분리를 해서 쉽게 처리 할 수 있게 바꿀 수 있습니다.
- `useReducer` 는 `useState` 를 대체 할 수 있는 기능 입니다.

<br>

### 동작 - 1

```js
const Counter = () => {
  const [count, dispatch] = useReducer(reducer, 1);
 //const [ state, 상태를 변화시키는 action 을 발생 시키는 함수 ] = useReducer( reducer, state 의 초기 값 )
  ... 
}
  ```
- `reducer` 는 `dispatch` 함수가 상태 변화를 일으키는데 일어난 상태 변화를 처리해 주는 역할 입니다.
  
<br>

### 동작 - 2
```js
return {
  <div>
  	{count}
	<button onClick = {() => dispatch({ type : 1 })> add 1 </button>
```
- `dispatch` 함수를 호출 하면서 `({ type : 1 })>` 객체를 전달하게 됩니다.
- 객체에는 꼭 `type` 이라는 프로퍼티가 들어있는데, 이 때 dispatch 와 함께 전달되는 객체를 `action` 객체라고 부릅니다. ( action = 상태 변화 )
- 즉, 상태 변화를 설명할 객체
- `dispatch` 가 실행되면서 전달된 `action` 객체는 `reducer` 로 전달 됩니다.

<br>

### 동작 - 3
```js
const reducer = (state, action) => {
  // const reducer = ( 설정된 초기 값, dispatch 를 호출할 때 전달해줬던 action 객체를 전달)
  switch (action.type){
    case 1:
      	return state + 1;
    case 10:
      	return state + 1;
    case 100:
      	return state + 1;
    case 1000:
      	return state + 1;
    case 10000:
      	return state + 1;
    default:
      	return state;
  }
};
```
- 만약 위의 add 1 버튼을 누르면 `reduce` 함수가 실행이 되고, `reduce` 함수가 받게 되는 state 는 1이 되고, 이유는 `const [count, dispatch] = useReducer(reducer, 1);`
- `action` 객체는 `dispatch({ type : 1 })` 이라는 객체를 받게 됩니다.
- `switch - case` 을 사용하여 `action.type` 에 따라서 각각 다른 return 을 하고 있습니다. 
( 반환하는 return 은 새로운 state 가 됩니다. )
- 정리를 하면 add 1 버튼을 눌러 `reducer` 를 일으키게 되면, `action` 에 `type: 1` 로 전달하고, 1은 case 1 이기 때문에 1 + 1 = 2 를 리턴하고 이것이 새로운 상태가 됩니다. 그리고 count의 state 가 업데이트 되고  `{count}` 에 반영 됩니다.

<br>

## 참고
- [인프런](https://www.inflearn.com/course/%ED%95%9C%EC%9E%85-%EB%A6%AC%EC%95%A1%ED%8A%B8/dashboard)
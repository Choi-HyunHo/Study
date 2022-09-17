# Redux Toolkit
- redux-toolkit은 기존 redux의 여러 문제를 해결한 버전의 모듈이다. 
- redux와 본질은 동일하고, 더 사용하기 편하게 개량한 버전

***
<br>

## 설치
```js
npm install @reduxjs/toolkit react-redux
```

***
<br>

## 사용법
### 1. store.js 준비
```js
// store.js

import { configureStore } from "@reduxjs/toolkit";

//redux store
export const store = configureStore({
  reducer: {},
});
```

<br>

- reducer가 포함된 객체를 __configureStore__ 에 전달하면 간단히 redux store가 만들어진다.
- 이것을 react 에 적용하기 위해 __index.js__ 로 가서

<br>

```js
import React from "react";
import ReactDOM from "react-dom";
import "./index.css";
import App from "./App";
import reportWebVitals from "./reportWebVitals";

//Provider와 store추가
import { Provider } from "react-redux";
import { store } from "./store";

ReactDOM.render(
  //이곳에서 아래와 같이 Provider에 store를 전달하고 App을 감싼다
  <React.StrictMode>
    <Provider store={store}>
      <App />
    </Provider>
  </React.StrictMode>,
  document.getElementById("root")
);
```

<br>

### 2. slice 만들기
기본 redux로는 리듀서를 만들 때 switch를 이용해 액션을 구분하여 상태를 변화시켰다. <br>
또한 디스패치를 호출할 때도 구조화된 액션을 매개변수로 넣어줘야만 했다.

- toolkit에서는 이러한 복잡한 정의/사용을 탈피하고자 __createSlice__ 라는 새로운 API를 지원한다.

<br>

```js
// redux/counter.js

iimport { createSlice } from "@reduxjs/toolkit";

const initialState = {
  value: 0,
};

export const counterSlice = createSlice({
  //name은 각 action에 대해 useSelector 할 때 사용
  name: "counter",

  //초기값
  initialState,
  reducers: {
    increment: (state) => {
      state.value += 1;
    },
    decrement: (state) => {
      state.value -= 1;
    },
  },
});

//actions
//dispatch할 때 액션을 전달해 상태를 어떻게 변화시킬지를 결정한다
export const { increment, decrement } = counterSlice.actions;

//reducer
export default counterSlice.reducer;
```

<br>

- createSlice로 slice를 만들면, slice에 reducer, actions가 생성되어 외부에서 사용할 수 있게 된다.
- counter에 대한 준비는 끝났으니, 다시 store로 돌아가 reducer에 counter를 추가해준다.

<br>

### 3. store.js 에 reducer 추가
```js
import { configureStore } from "@reduxjs/toolkit";

//방금 전 counter.js에서 export default한 counterSlice.reducer
import counterReducer from "./redux/counter";


//redux store
export const store = configureStore({
  reducer: {
    //add counterReducer
    counter: counterReducer
  },
});
```

<br>

### 4. 컴포넌트에서 사용하기
```js
//components/Counter.js

import React from "react";
import { useDispatch, useSelector } from "react-redux";
import { decrement, increment } from "../redux/counter";

const Counter = () => {
  //useSelector를 통해 redux store에서 특정 값을 가져와서 코드에서 사용 가능
  const count = useSelector((state) => state.counter.value);
  
  //dispatch에 action을 전달하면 해당 동작이 실행된다
  const dispatch = useDispatch();

  //increment(1증가) 동작
  //컴포넌트에서 store 로 값을 전달하여 바꾸는 방법
  const handleIncrement = () => dispatch(increment());
  //decrement(1감소) 동작
  const handleDecrement = () => dispatch(decrement());
  
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={handleIncrement}>increment</button>
      <button onClick={handleDecrement}>decrement</button>
    </div>
  );
};

export default Counter;
```
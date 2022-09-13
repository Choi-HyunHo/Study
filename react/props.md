# Props
컴포넌트에 데이터를 전달하는 방법

***

## 데이터를 전달하는 방법
- 초기 값을 전달하고 싶을 경우
- `{}` 을 사용하여 데이터를 지정 합니다.
- JSX에서 특정 태그에 값을 전달하기 위해서는 `{}` 를 이용하거나 문자열로 전달하여야 합니다


<br>

### App.js

```js
mport './App.css';

import MyHeader from './my_header';
import Counter from './count';

function App() {
  return (
    <div className="App">
      <MyHeader />
      <Counter initialValue={5} /> // 5라는 값을 전달 합니다.
    </div>
  );
}

export default App;

```
- 부모 컴포넌트인 App 에서 자식 컴포넌트인 Counter 에게 값을 전달

***

## 데이터를 사용하는 방법
### Count.js
```js
import React, { useState } from 'react';

const Counter = (props) => { // 매개변수로 props 를 받습니다.
  const [count, setCount] = useState(props.initialValue);

  const onIncrease = () => {
    setCount(count + 1); // 0 + 1
  };

  const onDecrease = () => {
    setCount(count - 1);
  };

  return (
    <div>
      <h2>{count}</h2>
      <button onClick={onIncrease}>+</button>
      <button onClick={onDecrease}>-</button>
    </div>
  );
};

export default Counter;

```
- 객체안에 담겨서 받아오고(개수에 상관없이), 점 표기법을 사용해서 접근 할 수 있습니다.

<br>

## 전달하는 값이 많아지면 ?
```js
<Counter initialValue={5}, a={1}, b={2}, c={3} />
```
- 가독성이 좋지 않습니다.

<br>

### 위와 같은 경우는
- 객체를 만들어서 `Spread` 연산자 방식으로 전달 할 수 있습니다.

<br>

### App.js
```js
import './App.css';

import MyHeader from './my_header';
import Counter from './count';

function App() {
  const counterProps = {
    a: 1,
    b: 2,
    c: 3,
    initialValue: 5,
  };

  return (
    <div className="App">
      <MyHeader />
      <Counter {...counterProps} />
    </div>
  );
}

export default App;
```

### count.js
```js
const Counter = ({ initialValue }) => { // props의 값 중에서 initialValue만 가져왔다.
  const [count, setCount] = useState(initialValue);
```
- 비구조화 할당을 통해 받을 수 있습니다.
- 하지만 종종 값이 의도치 않게 `undefined` 로 전달 되어지는 버그를 마주 할 수 있습니다.
- 만약, 위의 같은 상황을 마주할 경우 `.defalutProps` 를 활용 합니다.

```js
import React, { useState } from 'react';

const Counter = ({ initialValue }) => {
  console.log(initialValue);
  const [count, setCount] = useState(initialValue);

  const onIncrease = () => {
    setCount(count + 1); // 0 + 1
  };

  const onDecrease = () => {
    setCount(count - 1);
  };

  return (
    <div>
      <h2>{count}</h2>
      <button onClick={onIncrease}>+</button>
      <button onClick={onDecrease}>-</button>
    </div>
  );
};

Counter.defaultProps = {
  initialValue = 0; 
}

export default Counter;
```
- 부모 컴포넌트인 `App.js` 에서 내려준 적이 없으나, `defaultProps`에 의해서 0 으로 고정 됩니다.
- 즉, 전달 받지 못한 `props`의 기본 값을 설정해서 에러를 방지 할 수 있습니다.

<br>

## 컴포넌트 자체를 Props 로 전달
### 현재 Counter 의 모습
![캡처](https://user-images.githubusercontent.com/87301268/161902501-b00c3f13-7cc7-4bd7-af49-8637072d595a.JPG)

- 스타일이 하나도 들어가지 않아 보기에 불편 합니다.
- 컴포넌트에 스타일을 만들고, `props` 로 전달을 하겠습니다.

<br>

### Container.js
```js
const Container = ({children) => {
  return (
    <div style={{ margin: 20, padding: 20, border: '1px solid gray' }}>
  		{children}
	</div>
  );
};

export default Container;
```
- Container 컴포넌트는 `children` 이라느 props 를 받습니다.

<br>

### App.js
```js
import './App.css';

import MyHeader from './my_header';
import Counter from './count';
import Container from './Container';

function App() {
  const counterProps = {
    a: 1,
    b: 2,
    c: 3,
    initialValue: 5,
  };

  return (
    <Container> // 추가
      <div className="App"> // </div> 까지 children 에 전달
        <MyHeader />
        <Counter {...counterProps} />
      </div>
    </Container>
  );
}

export default App;
```
- 컴포넌트 사이에 JSX 요소들을 배치 했습니다.
- `Container` 의 자식으로 배치된 요소들은 `Container`의 `children` props 으로 전달이 됩니다.
![캡처](https://user-images.githubusercontent.com/87301268/161903496-cbf6ffd8-b722-4748-9d5f-a3b07ed89328.JPG)

***

## 참고
- [인프런](https://www.inflearn.com/course/%ED%95%9C%EC%9E%85-%EB%A6%AC%EC%95%A1%ED%8A%B8/dashboard)
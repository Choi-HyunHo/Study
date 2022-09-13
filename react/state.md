# State(상태)
계속해서 변화하는 동적인 값, 값에 따라 다른 행동을 수행하게 합니다.

***

## React의 State란
![KakaoTalk_20220405_214803071](https://user-images.githubusercontent.com/87301268/161757039-27b84e86-919e-48b6-9bd3-1ff857d6ee6e.jpg)

![KakaoTalk_20220405_214803412](https://user-images.githubusercontent.com/87301268/161757051-451c92c8-076e-4271-9e28-8491b05ea2b5.jpg)

- 컴포넌트가 갖는 계속 값이 바뀌는 동적인 데이터
- 상태를 바꾸는 등의 관리는 상태를 가진 컴포넌트가 직접 관리하게 됩니다.

***

## useState()
- 컴포넌트는 자신의 가진 상태(state)가 변화하면, 화면을 다시 그려 reRender 를 합니다.
- 즉, 함수가 다시 호출 됩니다.

<br>

### 버튼을 누를 때마다 +- 1씩 변화하는 Counter
- 컴포넌트 상단에 `import`
```js
import React, { useState } from 'react';
```
```js
const [ count, setCount ] = useState(0);
// const [ 상태 값, 상태 변화 함수 ] = useState(초기 값);
```
```js
// counter.js
import React, { useState } from 'react';

const Counter = () => {
  // 0에서 출발
  // 1씩 증가하고
  // 1씩 감소하는 형태
  
  console.log("counter 호출!"); // 함수가 다시 호출되는지 확인하기 위해

  const [count, setCount] = useState(0);
  
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
- `useState()` 메서드는 `[]` 배열을 반환하고, 비구조화 할당을 통해서
- 0 번째 index = count, 1 번째 index = setCount 라는 상수로 받아옵니다.
- __count는 상태 값__ 이기 때문에 `{}` 을 사용해 return을 받아 화면에 표시 됩니다.
- __setCount는 count 라는 상태를 변화시키는 상태 변화 함수__ 로 사용이 됩니다.
- __useState는 count 라는 상태를 만드는데 초기 값__ 으로 사용이 됩니다.
![ezgif com-gif-maker (1)](https://user-images.githubusercontent.com/87301268/161769066-a29efb2f-3126-4786-b101-467b2f09dc03.gif)

***

## 정리
### State ?
- 리액트의 컴포넌트의 변경 가능한 데이터
- 컴포넌트를 개발하는 개발자가 직접 정의하여 사용
- state 의 값이 변경 될 경우 컴포넌트가 재렌더링됨
- 렌더링이나 데이터 흐름에 사용되는 값만 state에 포함시켜야 함

### State 의 특징

- 직접적인 변경이 불가능 함
- 클래스 컴포넌트 ( 많이 사용되지 않고 있음 )
   - 생성자에서 모든 state를 한 번에 정의
   - state 를 변경하고자 할 때에는 꼭 setState() 함수를 사용해야 함
- 함수 컴포넌트 ( 가장 많이 사용 중 )
   - __useState()__ 훅을 사용하여 각각의 state를 정의
   - 각 state별로 주어지는 set함수를 사용하여 state 값을 변경

***

## 참고
- [인프런](https://www.inflearn.com/course/%ED%95%9C%EC%9E%85-%EB%A6%AC%EC%95%A1%ED%8A%B8/dashboard)
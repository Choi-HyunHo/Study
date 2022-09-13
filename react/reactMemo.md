# 최적화 - React.memo
__컴포넌트 재사용__
함수형 컴포넌트에게 업데이트 조건을 걸기

## 예시
### 컴포넌트 트리
![KakaoTalk_20220409_145536741](https://user-images.githubusercontent.com/87301268/162558688-ce6a3d36-0bda-47c5-88b6-2d2ab93c8810.jpg)

- `App` 컴포넌트는 각각 count 와 text 두 개의 state 를 가집니다.
- `counst` state 는 `CountView` 에게 , `text` state 는 `TextView` 컴포넌트 에게 prop 으로 각각 보내주고 있습니다.
- 우측에 표시된 순서대로 코드를 수행하면서 어떻게 업데이트 되는지 보겠습니다.

<br>

#### 순서

![KakaoTalk_20220409_145537192](https://user-images.githubusercontent.com/87301268/162558689-df67a84a-1266-4c35-b213-8976ad1c10f4.jpg)

1. setCount(10) 이 실행이 되면서 `App` 컴포넌트의 `count` state 의 값을 변화 시킬 예정입니다.
2. 그러면 `App` 컴포넌트의 `count` state 의 값이 바뀌게 되고, state 가 업데이트 되었기 때문에 해당 state 를 가진 `App` 컴포넌트는 리렌더링이 되게 됩니다.
3. 그렇게되면 prop 인 `CountView` 의 값도 바뀌게 되고, `CountView` 컴포넌트만 업데이트 될 것 같은 기대를 깨고 두 개의 자식 컴포넌트 `TextView` 까지 모두 리렌더가 됩니다.

이유는 ? 부모 컴포넌트가 리렌더가 되면 자식 컴포넌트들 또한 리렌더가 되기 때문에 `TextView` 또한 강제로 리런더가 됩니다.

- 위와 같은 상황에서 연산의 낭비가 발생하게 됩니다.
- `TextView` 컴포넌트는 렌더링 될 이유가 없습니다. `App` 컴포넌트가 바뀌긴 하지만 `TextView` 컴포넌트가 가지고 있는 prop 은 바뀌는 상황이 아니기 때문입니다.
- 이렇듯, 자신과 관련 없는 업데이트로 인해서 자신도 업데이트가 되어야 된다면 성능 상에 문제가 되는 이유 입니다.

<br>

### 위의 상황을 어떻게 막을 수 있을까?
![KakaoTalk_20220409_150126256](https://user-images.githubusercontent.com/87301268/162558879-3c24be28-fc4a-44fa-a350-179a3080cbf0.jpg)

#### 자식 컴포넌트에게 각 업데이트 조건을 설정
- 자식 컴포넌트에게 각각 업데이트 조건을 걸어두는 방법 입니다.
- `CountView` 컴포넌트는 자신이 prop 으로 받는 `count` 가 바뀔 때만 업데이트 하도록 하고
- `TextView` 컴포넌트는 자신이 prop 으로 받는 `text` 가 바뀔 때만 업데이트 하도록 합니다.
- `count` state 가 변경 되었을 때, `TextView` 컴포넌트를 업데이트 할 조건이 만족되지 않았기 때문에 `TextView` 컴포넌트는 리렌더 되지 않고, 연산의 낭비를 막아 성능을 보존 할 수 있습니다. 
- 위와 같은 걸 할 수 있는 기능이 `React.memo` 입니다.

<br>

## React.memo
```js
const MyComponent = React.memo(function MyComponent(props) {
  /* props를 사용하여 렌더링 */
});
```
- `React.memo` 는 고차 컴포넌트 입니다
> 고차 컴포넌트는 컴포넌트를 가져와 새 컴포넌트를 반환하는 함수 입니다.
출처 : https://ko.reactjs.org/docs/higher-order-components.html
- `React.memo` 는 함수 안에 매개변수로 컴포넌트를 전달하게 되면 더 강화된 새로운 컴포넌트를 반환하게 됩니다.
- 리렌더링 되지 않았으면 하는 컴포넌트 `function MyComponent(props) {` 감싸주게 되면은 __`props` 가 바뀌지 않으면 리렌더링 하지 않는 강화된 컴포넌트를 돌려줍니다.__ 
- 물론 자기 자신의 state 가 바뀌면 리렌더링이 됩니다. __왜냐하면 `React.memo` 는 부모 컴포넌트에 의한 리렌더를 막아주기 때문 입니다.__


<br>

### 예시 1
#### OptimizeTest.js
```js
import { useState, useEffect } from 'react';

const TextView = ({ text }) => {
  useEffect(() => {
    console.log(`Update :: Text : ${text}`);
  });
  return <div>{text}</div>;
};

const CountView = ({ count }) => {
  useEffect(() => {
    console.log(`Update :: Count : ${count}`);
  });
  return <div>{count}</div>;
};

const OptimizeTest = () => {
  const [count, setCount] = useState(1);
  const [text, setText] = useState('');

  return (
    <div style={{ padding: 50 }}>
      <div>
        <h2>count</h2>
        <CountView count={count} />
        <button onClick={() => setCount(count + 1)}>+</button>
      </div>
      <div>
        <h2>test</h2>
        <TextView text={text} />
        <input value={text} onChange={(e) => setText(e.target.value)} />
      </div>
    </div>
  );
};

export default OptimizeTest;
```
- `count` 를 누르면 부모 컴포넌트인 `OptimizeTest` 의 state 가 바뀌기 때문에 자식 컴포넌트 `CountView` , `TextView` 는 둘 다 렌더링이 일어나기 때문에 둘 다 console 에 출력이 되는 걸 볼 수 있습니다.
- 하지만 맨 위의 이미지로 본 것 처럼 낭비가 발생하게 됩니다.
![ezgif com-gif-maker (9)](https://user-images.githubusercontent.com/87301268/162560835-10f183f9-e1f7-462b-9911-e21a4309d912.gif)

<br>

#### React.memo 적용
```js
const TextView = React.memo(({ text }) => {
  useEffect(() => {
    console.log(`Update :: Text : ${text}`);
  });
  return <div>{text}</div>;
});
```
- `TextView` 컴포넌트는 prop 인 `text` 가 바뀌지 않으면 절대로 렌더링이 일어나지 않습니다.
![ezgif com-gif-maker (10)](https://user-images.githubusercontent.com/87301268/162561110-0a6ad6c1-ec8a-487a-88ef-149b4a90d528.gif)

<br>

### 예시 2
#### OptimizeTest.js
```js
import React, { useState, useEffect } from 'react';

const CounterA = React.memo(({ count }) => {
  useEffect(() => {
    console.log(`CountA Update - count : ${count}`);
  });
  return <div>{count}</div>;
});

const CounterB = React.memo(({ obj }) => {
  useEffect(() => {
    console.log(`CountB Update - count : ${obj.count}`);
  });
  return <div>{obj.count}</div>;
});

const OptimizeTest = () => {
  const [count, setCount] = useState(1);
  const [obj, setObj] = useState({ count: 1 });

  return (
    <div style={{ padding: 50 }}>
      <div>
        <h2>Counter A</h2>
        <CounterA count={count} />
        <button onClick={() => setCount(count)}>A Button</button>
      </div>
      <div>
        <h2>Counter B</h2>
        <CounterB obj={obj} />
        <button onClick={() => setObj({ count: 1 })}>B Button</button>
      </div>
    </div>
  );
};

export default OptimizeTest;
```
- 자식 컴포넌트 `CounterA`, `CounterB` 
- state 를 변화하는 setState 에 A, B counter 모두 똑같은 기존의 state를 넣었습니다.
- A button 을 누르면 `setCount(count)` 로 상태변화를 주도 하지만 상태가 바뀔 이유가 없습니다. 1 -> 1 로 바뀌면 변경 됬다고 보기 어렵습니다. 
- 그래서 console 에 아무것도 출력이 되지 않습니다.
- 위와 같이 생각하면 B button 을 눌렀을 때 아무것도 출력이 되지 않아야 합니다.
![ezgif com-gif-maker (12)](https://user-images.githubusercontent.com/87301268/162561953-7c101644-07d2-4438-94d2-31dc674a19f4.gif)

- 하지만, 출력이 됩니다. 즉 리렌더링이 일어났다는 뜻 입니다.
- 그럼 `React.memo` 가 동작을 못하는 것이라고 볼 수 있지만 그렇지 않습니다.
- 위와 같은 상황이 일어나는 이유는 prop 인 `obj` 가 객체이기 때문입니다.
- 자바스크립트에서 객체는 얕은 비교를 하기 때문에 문제가 발생 합니다.


#### 객체를 비교하는 방법
![KakaoTalk_20220409_164104441](https://user-images.githubusercontent.com/87301268/162562074-800f0b55-6284-4031-9b9a-6067ea2f8759.jpg)


![KakaoTalk_20220409_164104894](https://user-images.githubusercontent.com/87301268/162562076-0e7e5363-bc2c-4086-a651-67e4d305ef64.jpg)

- 자바스크립트가 객체나 함수 또는 배열 같은 비원시타입의 자료형을 비교할 때 값에 의한 비교가 아닌 주소에 의한 비교인 __얕은 비교__ 를 하기 때문 입니다.
- 객체들은 생성 되자마자 고유한 메모리 주소를 가지게 됩니다.
- __얕은 비교__ 라는 것은 객체의 값을 비교하는 것이 아니라 두 객체가 설령 값이 같을 지라도,
__같은 주소에 있는지 비교합니다.__

<br>

#### 하지만 아래처럼 비교하게 되면
- 이전에는 변수 b 에도 각각 객체를 생성해서 할당 하였는데 지금은 그냥 a 의 값을 대입 했습니다.
- 그렇게되면 메모리 상에서 변수 b 가 변수 a 와 같은 객체를 가르킵니다.

<br>

#### 다시 돌아와서 문제를 해결 하려면
```js
function MyComponent(props) {
  /* props를 사용하여 렌더링 */
}
function areEqual(prevProps, nextProps) {
  /*
  nextProps가 prevProps와 동일한 값을 가지면 true를 반환하고, 그렇지 않다면 false를 반환
  */
}
export default React.memo(MyComponent, areEqual);
```
- `React.memo` 가 첫 번째 인자 말고도, 두 번째 인자를 받는 것을 볼 수 있습니다.
- `function areEqual` 는 `prevProps` : 이전의 props 와 `nextProps` : 이후의 props 를 받고 동일한 값을 가지면 true 를 반환하고, 그렇지 않다면 false 를 반환 합니다.
- 기존의 얕은 비교를 하지 않게 하고, `areEqual` 함수에서 깊은 비교를 구현 한다면 정상적으로 동작이 가능합니다.

#### OptimizeTest.js
```js
import React, { useState, useEffect } from 'react';

const CounterA = React.memo(({ count }) => {
  useEffect(() => {
    console.log(`CountA Update - count : ${count}`);
  });
  return <div>{count}</div>;
});

const CounterB = ({ obj }) => {
  useEffect(() => {
    console.log(`CountB Update - count : ${obj.count}`);
  });
  return <div>{obj.count}</div>;
};

const areEqual = (prevProps, nextProps) => {
  if(prevProps.obj.count === nextProps.obj.count){
    return true // 이전 props 현재 props 가 같다. -> 리렌더링을 일으키지 않게 됩니다.
  }
  return false;
}

const MemoizedCounterB = React.memo(CounterB, areEqual)

const OptimizeTest = () => {
  const [count, setCount] = useState(1);
  const [obj, setObj] = useState({ count: 1 });

  return (
    <div style={{ padding: 50 }}>
      <div>
        <h2>Counter A</h2>
        <CounterA count={count} />
        <button onClick={() => setCount(count)}>A Button</button>
      </div>
      <div>
        <h2>Counter B</h2>
        <MemoizedCounterB obj={obj} /> // 이 부분
        <button onClick={() => setObj({ count: 1 })}>B Button</button>
      </div>
    </div>
  );
};

export default OptimizeTest;
```
- `CounterB` 함수에 `React.memo` 를 해제 합니다.
- 그리고 비교 함수를 만듭니다. ( areEqual )
- `const MemoizedCounterB = React.memo(CounterB, areEqual)` 작성
- 렌더를 할 때 `CoutnerB` 함수가 아니라 `MemoizedCounterB` 함수로 렌더 합니다

![ezgif com-gif-maker (13)](https://user-images.githubusercontent.com/87301268/162562854-df539a3d-7488-4f7a-805f-ddbfe303d44b.gif)


<br>

## 참고
- https://ko.reactjs.org/docs/react-api.html
- [인프런](https://www.inflearn.com/course/%ED%95%9C%EC%9E%85-%EB%A6%AC%EC%95%A1%ED%8A%B8/dashboard)
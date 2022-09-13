# useEffect

함수형 컴포넌트에서 `useEffect` 를 이용해서 Lifecycle 를 관리 할 수 있습니다.

***

## 사용법

![KakaoTalk_20220408_134632495](https://user-images.githubusercontent.com/87301268/162366198-07bc4447-77a6-4522-a6e9-a944cf9ba3c4.jpg)

```js
import React, { useEffect } from "react";
```
- `useEffect` 를 사용 합니다.
- 2개의 파라미터를 전달하게 되는데 1. callback 함수, 2. 의존성 배열 을 전달 합니다.
- 의존성 배열을 뎁스라고 부르기도 합니다.
> 의존성 배열에 들어있는 값 중 하나라도 변화하면, 첫 번째 파라미터인 콜백 함수가 다시 수행

<br>

## 예시
### 코드
#### Lifecycle.js
```js
import React, { useEffect, useState } from 'react';

const Lifecycle = () => {
  const [count, setCount] = useState(0);
  const [text, setText] = useState('');
  
  // Mount
  useEffect(() => {
    console.log('Mount!');
  }, []);

  // Update
  useEffect(() => {
    console.log('update!');
  });

  return (
    <div style={{ padding: 20 }}>
      <div>
        {count}
        <button onClick={() => setCount(count + 1)}>+</button>
      </div>
      <div>
        <input value={text} onChange={(e) => setText(e.target.value)} />
      </div>
    </div>
  );
};

export default Lifecycle;
```

<br>

#### 1. 컴포넌트가 탄생할 때 - Mount 되는 시점
```js
useEffect(() => {
    console.log('Mount!');
}, []);
```

![캡처](https://user-images.githubusercontent.com/87301268/162368517-4e554a22-0506-48b5-8ecf-08e892359611.JPG)

- 컴포넌트가 `Mount` 되는 시점에 console 이 실행이 됩니다.
- __컴포넌트 Mount__ 시점에서 무언가를 하고 싶으면 `useEffect` 의 두 번째 인자인 __뎁스에 빈 배열을 전달__ 한 다음에 콜백 함수에 개발자가 하고 싶은 일을 넣으면 됩니다.

<br>

#### 2. 컴포넌트가 update 되는 경우 
- 업데이트가 되는 시점은 state 가 변경 되거나, 부모에게서 내려받는 props 가 바뀌거나, 또는 부모 컴포넌트가 리렌더링이 되면 자기 자신이 리렌더링 됩니다
> 리렌더 된다는 뜻은 컴포넌트가 업데이트 된다는 말과 동일 합니다.
```js
useEffect(()=>{
	console.log("update!");
})
```
- 사용할 때는 의존성 배열(뎁스) 를 전달하지 않으면 됩니다.
![ezgif com-gif-maker (5)](https://user-images.githubusercontent.com/87301268/162370026-20214617-f964-47b6-8fcc-60d9485e57de.gif)

<br>

#### 만약 count > 5 를 넘어가면 위험한 경우
```js
useEffect(() => {
    console.log(`count is update ${count}`);
    if (count > 5) {
      alert('count 가 5 를 넘었습니다. 따라서 1로 초기화 합니다.');
      setCount(1);
    }
  }, [count]);
```
![ezgif com-gif-maker (6)](https://user-images.githubusercontent.com/87301268/162371509-2212a950-ec21-4518-92ee-7136387324b0.gif)

<br>

#### 3. 컴포넌트가 화면에서 사라질 때 - Unmount
```js
import React, { useEffect, useState } from 'react';

const UnmountTest = () => {
  useEffect(() => {
    console.log('Mount!');

    return () => {
      // Unmount 시점에서 실행
      console.log('Unmount!');
    };
  }, []);

  return <div>Unmount Testing Component</div>; // True인 경우 화면에 return
};

const Lifecycle = () => {
  const [isVisible, setIsVisible] = useState(false);
  const toggle = () => setIsVisible(!isVisible);

  return (
    <div style={{ padding: 20 }}>
      <button onClick={toggle}>ON / OFF</button>
      {isVisible && <UnmountTest />}
    </div>
  );
};

export default Lifecycle;
```
<br>

```js
{isVisible && <UnmountTest/>} 
```
- `isVisible` 이 True 인 경우만 UnmountTest 컴포넌트가 화면에 렌더링이 됩니다.
- `isVisible` 이 Fales 인 경우는 && (and) 연산자여서 모두 True 인 경우만 유효하기 때문에 비교를 할 수 없습니다. 즉 렌더링이 되지 않습니다.

<br>

#### Mount 에 전달되는 콜백 함수가 함수를 리턴 받게 하면 됩니다.
```js
const UnmountTest = () => {
  useEffect(() => {
    // Mount 에 전달되는 콜백 함수
    console.log('Mount!');

    return () => {
      // Unmount 시점에서 실행
      console.log('Unmount!');
    };
  }, []);
```
![ezgif com-gif-maker (7)](https://user-images.githubusercontent.com/87301268/162373833-92515ee2-3e4d-43fc-872c-a7a6443a5282.gif)

***

## 정리
### useEffect
- 사이드 이펙트를 수행하기 위한 훅
- 사이드 이펙트란, 서버에서 데이터를 받아오거나 수동으로 DOM 을 변경하는 등의 작업
- __useEffect()__ 훅으로만 클래스 컴포넌트의 생명주기 함수들과 동일한 기능을 수행 할 수 있음

***

## 참고
- [인프런](https://www.inflearn.com/course/%ED%95%9C%EC%9E%85-%EB%A6%AC%EC%95%A1%ED%8A%B8/dashboard)
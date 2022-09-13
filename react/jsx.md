# JSX 문법
## 들어가기 전 React 동작 방식
### 먼저 최상위 컴포넌트 정의 
```js
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';

ReactDOM.render(
  <React.StrictMode>
    <App /> // (3)
  </React.StrictMode>,
  document.getElementById('root') // (2)
);

// If you want to start measuring performance in your app, pass a function
// to log results (for example: reportWebVitals(console.log))
// or send to an analytics endpoint. Learn more: https://bit.ly/CRA-vitals
```
- React App 이 작동하는 방식은
1. index.js 에서
2. id 가 `root` 인 요소를 찾고
3. 그 요소에 `<App />` 이라는 컴포넌트를 불러와서 렌더링을 합니다. 
> 개발자가 직접 바꿀 수 있으나 관례상 많은 개발자가 최상위 컴포넌트를 App 이라고 부르기 때문에 이유도 없이 부딪힐 필요는 없습니다.

***

## 닫힘 규칙
- 태그를 열면 반드시 닫는 태그가 있어야 합니다.
```html
<div> (x) , <div></div> (o)
```
- `<a>` 태그나 `<image>` 태그 , `<br>` 등 HTML에서 기존에 닫을 필요가 없었던 태그 또한 마찬가지로 안 닫으면 에러가 발생하기 때문에 아래와 같게 해야 합니다.
```html
<a /> , <image /> 
```

***

## 최상위 태그 규칙
### 최상위 태그

```js
function App() {
  return (
    <div className="App">
      <MyHeader />
      <header className="App-header">
        <h2>안녕 리액트</h2>
      </header>
    </div>
  );
}
```
- 최상위 태그는 위의 `return` 문 안에서 즉, 컴포넌트의 구성 중에 가장 바깥에 위치하는 태그
- 반드시 하나의 최상위 태그의 부모를 가져야 합니다.
- 만약 최상위 태그를 지우게 되면 에러가 발생 합니다.
```js
function App() {
  return (
    
      <MyHeader />
      <header className="App-header">
        <h2>안녕 리액트</h2>
      </header>
    
  );
}
```
![캡처](https://user-images.githubusercontent.com/87301268/161750744-aa99c558-a531-4385-abad-a479ae485b4a.JPG)

<br>

### 하지만 최상위 태그로 묶고 싶지 않다면?
```js
import React from 'react'; // 파일 상단에 import 후
```
```js
function App() {
  return (
    <React.Fragment> // 추가
      <MyHeader />
      <header className="App-header">
        <h2>안녕 리액트</h2>
      </header>
    </React.Fragment>
  );
}
```
- 위와 같이 사용하면 됩니다. 
- 혹은 아래와 같이 사용
```js
function App() {
  return (
    <> // 추가
      <MyHeader />
      <header className="App-header">
        <h2>안녕 리액트</h2>
      </header>
    </>
  );
}
```
***

## JSX 문법과 CSS 
- JSX 에서는 class 라는 단어가 자바스크립트 예약어라서 사용이 불가능 합니다.
- 그래서 `className` 을 사용 합니다.
- 상단에 `import` 와 경로를 설정하여 CSS를 불러서 사용 할 수 있습니다.
- 그 후 각 요소들에게 `className` 으로 적용
- 태그 셀렉터, id 셀렉터 사용 가능 합니다.

### App.js
```js
import './App.css'; // 이 부분

import MyHeader from './my_header';

function App() {
  return (
    <div className="App">
      <MyHeader />
      <h2>안녕 리액트</h2>
    </div>
  );
}

export default App;
```

<br>

### inline 방식으로 CSS 사용
```js
// import './App.css'; // 이 부분

import MyHeader from './my_header';

function App() {
  
 const style = {
   App : {
     background: "black",
   },
   h2: {
     color: "red",
   }
 };
  
  return (
    <div style={style.App}>
      <MyHeader />
      <h2 style={style.h2}>안녕 리액트</h2>
    </div>
  );
}

export default App;
```
- `import` 를 사용하지 않습니다.
- 객체 처럼 만들고 사용 할 수 있습니다.

***

## 자바스크립트의 값을 사용하는 법
- `{}` 사용
- 문자열, 숫자, 함수를 `{}` 안에 넣으면 표현이 가능 합니다.
- 하지만 배열, boolean 값은 렌더링이 되지 않습니다.
```js
import './App.css';

import MyHeader from './my_header';

function App() {
  const name = 'hyunho';

  const func = () => {
    return 'func';
  };

  return (
    <div className="App">
      <MyHeader />
      <h2>안녕 리액트 {name}</h2>
      <h2>안녕 리액트 {func()}</h2>
      <h2>안녕 리액트 {2 + 3}</h2>
    </div>
  );
}

export default App;
```

![캡처](https://user-images.githubusercontent.com/87301268/161754324-51faea67-9510-4764-9d3f-6a92b923557f.JPG)

<br>

- 또한 삼항 연산자를 활용하면 조건에 따라 각각 다른 요소를 렌더링 할 수 있습니다
- 이런 방식의 렌더링을 __조건부 렌더링__ 이라 부릅니다.
```js
import './App.css';

import MyHeader from './my_header';

function App() {
  const name = 'hyunho';
  const number = 10;

  return (
    <div className="App">
      <MyHeader />
      <h2>안녕 리액트 {name}</h2>
      <h2>
        {number}는 : {number % 2 === 0 ? '짝수' : '홀수'}
      </h2>
    </div>
  );
}

export default App;

```
![캡처](https://user-images.githubusercontent.com/87301268/161755164-9ff70af4-d18a-489a-8fa5-02f017e05545.JPG)

***

## 정리
### JSX ?
- 자바스크립트와 XML / HTML 을 함께 사용할 수 있는 자바스크립트의 확장 문법

### JSX 의 역할
- JSX로 작성된 코드는 모두 자바스크립트 코드로 변환

### JSX 의 장점
- 코드가 간결해짐
- 가독성 향상

### JSX 사용법
- 기본적으로 모든 자바스크립트 문법을 지원
- 자바스크립트에 XML 과 HTML 을 섞어서 사용
- 중괄호를 사용하여 자바스크립트 코드를 삽입

***

## 참고
- [인프런](https://www.inflearn.com/course/%ED%95%9C%EC%9E%85-%EB%A6%AC%EC%95%A1%ED%8A%B8/dashboard)
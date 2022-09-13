# useRef
1. DOM 요소를 접근 할 수 있는 기능
> DOM ?
HTML과 JavaScript를 이어주는 공간으로, 작성한 HTML을 JavaScript가 이해할 수 있도록 Object로 변환하는 것
출처 : https://www.howdy-mj.me/dom/what-is-dom/
2. 저장공간 (변수관리) 로 사용 할 수 있습니다.


***

## 1. DOM 요소에 접근
- 바닐라 자바스크립트의 `document.querySelector()` 와 비슷 합니다.
- 예시는, 로그인 화면이 나왔을 때 id를 넣는 input을 굳이 클릭하지 않아도 자동으로 포커스가 되어 있게 할 수 있고, 엘리먼트의 크기나 색상을 변경할 때 사용이 가능 합니다.

<br>

## 2. 저장 공간
- 저장공간이라 하면 많이 사용하는 state를 사용 합니다.
- 하지만 React는 컴포넌트의 state 가 변할 때 마다 다시 리렌더링이 되면서 컴포넌트의 내부 변수들이 초기화 됩니다.
- 컴포넌트 내부 변수들이 초기화가 된다는 것은 해당 컴포넌트 함수의 변수들이 모두 초기화 되고 다시 모든 함수 로직이 실행되는 것을 의미 합니다.
- 이렇게 렌더링에서 수행하는 로직이 많을 수록, 많은 컴포넌트를 출력 할 수록 성능 손실이 발생 합니다.
- 그렇지만 `ref` 를 사용하여 저장하면, 해당 `ref` 안에 있는 값을 아무리 변경해도 컴포넌트는 다시 렌더링이 되지 않습니다.
- __즉, `ref` 를 사용하면 불필요한 렌더링을 막을 수 있고, `ref` 에 저장되어 있는 값은 변하지 않고 그대로 유지가 됩니다.__

***

## 정리
### state 의 변화
- state 변화 -> 렌더링 -> 컴포넌트 내부 변수 초기화
- state 변화 -> 렌더링 -> Ref 값은 그대로 유지

<br>

### Ref 의 변화
- Ref 변화 -> 렌더링 X -> 다른 변수들의 값 유지


<br>

## 사용법
```js
import { useRef } from 'react';
```
- ` ref` 키워드 사용

<br>

## 예시 
### alert 를 사용하여 사용자에게 알리기
```js
import { useRef, useState } from 'react';

const handleSubmit = () => {
    if (state.author.length < 1) {
      alert('작성자는 최소 1 글자 이상 입력 해주세요');
      return;
    }

    if (state.content.length < 5) {
      alert('일기 본문 최소 5 글자 이상 입력 해주세요');
      return;
    }

    alert('저장 성공');
  };
```
- 사용자에게 `alert` 으로 경고창을 갑자기 띄우는 것은 좋지 않습니다.

<br>

### focus 를 사용하여 사용자에게 알리기
```js
const authorInput = useRef();
const contentInput = useRef();

const handleSubmit = () => {
    if (state.author.length < 1) {
      authorInput.current.focus();
      return;
    }

    if (state.content.length < 5) {
      contentInput.current.focus();
      return;
    }

    alert('저장 성공');
 };

return (
    <div className="DiaryEditor">
      <h2>오늘의 일기</h2>
      <div>
        <input
          ref={authorInput} 
          name="author"
          value={state.author}
          onChange={handleChangeState}
        />
      </div>
      <div>
        <textarea
		  ref={contentInput}
          name="content"
          value={state.content}
          onChange={handleChangeState}
        ></textarea>
      </div>
		...
        ...
```
- `current` 는 현재 가리키는 값을 의미 합니다.
- `author.input.current` 는 `author input` 태그 

***

## 요약
### useRef()
- 레퍼런스를 사용하기 위한 훅
- 레퍼런스란 __특정 컴포넌트에 접근 할 수 있는 객체__ 를 의미
- 매번 렌더링 될 때마다 항상 같은 레퍼런스 객체를 반환

### 사용법
```js
const refContainer = useRef(초기 값);
```
- __.current__ 라는 속성을 통해서 접근


***

## 참고
- [인프런](https://www.inflearn.com/course/%ED%95%9C%EC%9E%85-%EB%A6%AC%EC%95%A1%ED%8A%B8/dashboard)
- https://velog.io/@suuhyeony/React-useRef-useEffect
- https://itprogramming119.tistory.com/entry/React-useRef-%EC%82%AC%EC%9A%A9%EB%B2%95-%EB%B0%8F-%EC%98%88%EC%A0%9C
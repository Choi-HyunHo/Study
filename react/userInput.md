# 사용자 입력 처리
사용자에게 입력을 받는 태그는 `<input>`, `<textarea>` 등 존재 하는데 
`React` 에서 직접 핸들링 할 때는 어떻게 하는 지 알아보겠습니다.

***

## useState 이용 
### input 의 value 설정
```js
import { useState } from 'react';

const DiaryEditor = () => {
  
  const [author, setAuthor] = useState('');
 // author 라는 state 는 setAuthor 가 아니면 상태변화가 일어나지 않습니다.
 // 그러므로 화면 input 창에 어떠한 것도 임의로 입력 할 수 없습니다.

  return (
    <div className="DiaryEditor">
      <h2>오늘의 일기</h2>
      <div>
        <input value={author}/>
      </div>
    </div>
  );
};

export default DiaryEditor;
```
- 초기에 `input` 에는 아무런 값도 입력되지 않습니다.
- 이유는 `input` 태그에 `value` 를 `author` 로 주고 있는데 상태 변화 함수인 `setAuthor` 의 값이 변하지 않기 때문 입니다. ( 즉, 아무런 설정이 되어있지 않습니다. )
- 결론은, `input`에 값이 무작위로 입력이 되면, `author` 의 값이 바뀌고 실시간으로 반영이 되야 합니다.

<br>

### onChange() 이벤트
```js
import { useState } from 'react';

const DiaryEditor = () => {
  
  const [author, setAuthor] = useState('');
 // author 라는 state 는 setAuthor 가 아니면 상태변화가 일어나지 않습니다.
 // 그러므로 화면 input 창에 어떠한 것도 임의로 입력 할 수 없습니다.

  return (
    <div className="DiaryEditor">
      <h2>오늘의 일기</h2>
      <div>
        <input 
    		value={author} 
			onChange={(e) => {
    			setAuthor(e.target.value);
  		  }}
		/>
      </div>
    </div>
  );
};

export default DiaryEditor;
```
- `onChange()` : 값이 바뀌었을 때 수행하는 이벤트
- 값을 입력하면 이벤트 안에 들어있는 콜백 함수가 실행 됩니다.
- 이벤트가 발생해서 나온 값을 `setAuthor` 에 넘겨서 상태 변화가 일어납니다.
- 만약, `<textarea/>` 을 사용 하는 경우에도 위와 동일하게 할 수 있습니다.

>Tip. Input
input의 onChange를 사용하면 이벤트 객체 e를 파라미터로 받아올 수 있다.
이 객체의 e.target은 이벤트가 발생한 DOM을 가리킨다.
e.target.value를 조회하면 현재 input의 value값을 알 수 있다.
<br> 출처 : https://react.vlpt.us/basic/08-manage-input.html

<br>

### 중복되는 코드를 줄이는 방법
```js
import { useState } from 'react';

const DiaryEditor = () => {
  const [state, setState] = useState({
    author: '', // 공백 문자열
    content: '', // 공백 문자열
  });

  return (
    <div className="DiaryEditor">
      <h2>오늘의 일기</h2>
      <div>
        <input
          value={state.author}
          onChange={(e) => {
            setState({
              author: e.target.value,
              content: state.content, // 값이 동시에 바뀌면 안되기 때문에 기존의 값 유지
            });
          }}
        />
      </div>
      <div>
        <textarea
          value={state.content}
          onChange={(e) => {
            setState({
              content: e.target.value,
              author: state.author,
            });
          }}
        ></textarea>
      </div>
    </div>
  );
};

export default DiaryEditor;
```
- 초기 상태인 `state` 객체를 가지고 상태 변화 객체인 `setState` 객체를 가집니다.
- 객체의 값을 바꾸려면 새로운 객체를 전달해줘야 합니다.
- 하지만 점점 전달해야 되는 객체의 수가 많아지면 `spread` 방법을 사용 합니다.

<br>

### Spread 방식
- 해당 객체가 가지고 있는 `프로퍼티( 속성 )` 을 뿌려줍니다. ( 펼쳐준다. )
```js
import { useState } from 'react';

const DiaryEditor = () => {
  const [state, setState] = useState({
    author: '',
    content: '',
  });

  return (
    <div className="DiaryEditor">
      <h2>오늘의 일기</h2>
      <div>
        <input
          value={state.author}
          onChange={(e) => {
            setState({
              ...state, // author 는 원래 author 의 값, content 는 원래 content 의 값.
              author: e.target.value,
            });
          }}
        />
      </div>
      <div>
        <textarea
          value={state.content}
          onChange={(e) => {
            setState({
              ...state,
              content: e.target.value,
            });
          }}
        ></textarea>
      </div>
    </div>
  );
};

export default DiaryEditor;
```

<br>

## 이벤트 핸들러를 하나로
- 태그에 각각 `name` 이라는 속성을 줍니다.
> `<input>` 태그의 name 속성은 `<input>` 요소의 이름을 명시합니다. name 속성은 폼(form)이 제출된 후 서버에서 폼 데이터(form data)를 참조하기 위해 사용되거나, 자바스크립트에서 요소를 참조하기 위해 사용됩니다.<br>
출처 : TCP, school
```js
import { useState } from 'react';

const DiaryEditor = () => {
  const [state, setState] = useState({
    author: '',
    content: '',
  });

  const handleChangeState = (e) => { // input 에 변화를 주면 실행 됩니다.
    console.log(e.target.name); // 각각 맞는 이름과 값이 콘솔에 출력 
    console.log(e.target.value);

    setState({
       // 변경할 태그의 이름인 author 와 content 는 실제로 바꿔야 하는 state 속성의 key 와 같습니다.
      ...state,
      [e.target.name]: e.target.value,
    });
  };

  return (
    <div className="DiaryEditor">
      <h2>오늘의 일기</h2>
      <div>
        <input
          name="author"
          value={state.author}
          onChange={handleChangeState}
        />
      </div>
      <div>
        <textarea
          name="content"
          value={state.content}
          onChange={handleChangeState}
        ></textarea>
      </div>
    </div>
  );
};

export default DiaryEditor;
```

***



## 참고
- [인프런](https://www.inflearn.com/course/%ED%95%9C%EC%9E%85-%EB%A6%AC%EC%95%A1%ED%8A%B8/dashboard)
# 리스트 데이터 추가하기
React 는 단방향으로만 데이터가 흐릅니다.

또한 같은 레벨에 있는 컴포넌트 는 서로 데이터를 교환 할 수 없습니다.
이런 경우 __공통 부모요소로 끌어올려서__ 해결을 할 수 있습니다.

<br>

## 구조

![KakaoTalk_20220407_125955888](https://user-images.githubusercontent.com/87301268/162117698-c60e6fa5-d9d7-4a42-928a-5a2459279727.png)

- 같은 레벨이라 함은 `DiaryEditor` 와 `DiaryList` 입니다.
- 공통 부모인 `App` 컴포넌트가 데이터를 배열 형식의 `state` 로 받습니다.
- `data` state 의 값을 `DiaryList` 에게 전달하면서 리스트를 렌더링 하게 하고
- `data` state 를 변화 시킬 수 있는 상태변화 함수인 `setData` 를 `DiaryEditor`
에게 props 으로 전달하면 됩니다.

<br>

### 시뮬레이션
![KakaoTalk_20220407_125955888](https://user-images.githubusercontent.com/87301268/162117698-c60e6fa5-d9d7-4a42-928a-5a2459279727.png)

- `data` state 는 배열이고 `[itme1]` 하나의 아이템을 가지고 있는 상태 입니다.
- `data` 를 props 로 내려받은 `DiaryList` 컴포넌트는 `[itme1]` 만 렌더링 하고 있는 상태 입니다.
- 이때, `DiaryEditor` 컴포넌트에서 __새로운 아이템을 만들면__ `App` 컴포넌트 에서
`DiaryEditor` 컴포넌트에게 props 으로 전달한 `setData` 함수를 호출 합니다.

<br>

![KakaoTalk_20220407_131800216](https://user-images.githubusercontent.com/87301268/162119384-a81166c7-8116-46b9-93e2-803f08c549fa.png)
- 새로운 아이템을 추가 합니다.
- `App` 컴포넌트가 `DiaryEditor` 컴포넌트에게 props 으로 전달한 `setData` 함수를 호출 합니다.
- `setData` 는 `data` 의 상태를 `[itme1, item2 .... ]` 새로운 아이템을 추가 합니다.

<br>

#### App.js
```js
import { useRef, useState } from 'react';
import './App.css';
import DiaryEditor from './DiaryEditor';
import DiaryList from './DiaryList';

function App() {
  const [data, setData] = useState([]); // state-배열로 저장할 예정 (리스트)

  const dataId = useRef(0);

  const onCreate = (author, content, emotion) => {
    const create_date = new Date().getTime();
    const newItem = {
      author,
      content,
      emotion,
      create_date,
      id: dataId.current, // 0 을 가리킨다.
    };

    dataId.current += 1; // id 의 값이 1씩 증가한다.
    setData([newItem, ...data]); // 새로운 아이템이 위로 올라오게 하기 위해서 newItem 을 먼저 사용
  };

  return (
    <div className="App">
      <DiaryEditor onCreate={onCreate} />
      <DiaryList diaryList={data} />
    </div>
  );
}

export default App;
```
<br>

#### DiaryEditor.js
```js
import { useRef, useState } from 'react';

const DiaryEditor = ({ onCreate }) => {
  const authorInput = useRef();
  const contentInput = useRef();

  const [state, setState] = useState({
    author: '',
    content: '',
    emotion: 1,
  });

  const handleChangeState = (e) => {
    setState({
      // 변경할 태그의 이름인 author 와 content 는 실제로 바꿔야 하는 state 속성의 key 와 같습니다.
      ...state,
      [e.target.name]: e.target.value,
    });
  };

  const handleSubmit = () => {
    if (state.author.length < 1) {
      authorInput.current.focus();
      return;
    }

    if (state.content.length < 5) {
      contentInput.current.focus();
      return;
    }

    onCreate(state.author, state.content, state.emotion);
    alert('저장 성공');
    setState({
      author: '',
      content: '',
      emotion: 1,
    });
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
      <div>
        <select
          name="emotion"
          value={state.emotion}
          onChange={handleChangeState}
        >
          <option value={1}>1</option>
          <option value={2}>2</option>
          <option value={3}>3</option>
          <option value={4}>4</option>
          <option value={5}>5</option>
        </select>
      </div>
      <div>
        <button onClick={handleSubmit}>일기 저장하기</button>
      </div>
    </div>
  );
};

export default DiaryEditor;
```
![캡처](https://user-images.githubusercontent.com/87301268/162136144-97f592f8-8315-47cb-9a44-b06337aba289.JPG)


## React 로 만든 컴포넌트들은
![KakaoTalk_20220407_150430506](https://user-images.githubusercontent.com/87301268/162131108-275cee88-172a-4d2b-b688-8d085c0adae2.png)

- 트리 형태의 구조를 띄며, 데이터는 위에서 아래로만 움직이게 되는 단방향 구조 입니다.
- 추가, 수정, 삭제 등 이벤트들은 `setData` 같은 함수를 props 로 전달을 하고
- 이벤트들은 아래에서 -> 위로 올라가는 구조 라고 생각 할 수 있습니다.

<br>

## 참고
- [인프런](https://www.inflearn.com/course/%ED%95%9C%EC%9E%85-%EB%A6%AC%EC%95%A1%ED%8A%B8/dashboard)
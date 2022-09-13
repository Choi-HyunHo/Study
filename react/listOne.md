# 리스트 렌더링 ( 조회 )
배열을 사용하여 리스트 렌더링을 할 수 있습니다.
React 에서 배열은 게시글, 리스트, 피드 등을 표시 하는 경우 자주 사용 됩니다.

<br>


## 순서
### App.js 에서 빈 객체를 만들어 줍니다.
- `const dummyList` 부분
- props 을 사용해서 해당 컴포넌트에 데이터를 전달 합니다.
- `<<DiaryList diaryList={dummyList} />` 부분
- 객체를 나중에 문자열로 변환하여 사용하는데 `Date()` 객체를 직접 배열에 담으면, 
번거로울 수 있기 때문에 숫자로 먼저 변환하여 저장 합니다.
```js
import './App.css';
import DiaryEditor from './DiaryEditor';
import DiaryList from './DiaryList';

const dummyList = [
  {
    id: 1,
    author: '최현호',
    content: '하이 1',
    emotion: 5,
    create_date: new Date().getTime(), // getTime() 은 시간을 받아서 밀리세컨드로 만들어 준다.
  },
  {
    id: 2,
    author: '이정환',
    content: '하이 2',
    emotion: 3,
    create_date: new Date().getTime(), // getTime() 은 시간을 받아서 밀리세컨드로 만들어 준다.
  },
  {
    id: 3,
    author: '홍길동',
    content: '하이 3',
    emotion: 4,
    create_date: new Date().getTime(), // getTime() 은 시간을 받아서 밀리세컨드로 만들어 준다.
  },
];

function App() {
  return (
    <div className="App">
      <DiaryEditor />
      <DiaryList diaryList={dummyList} />
    </div>
  );
}

export default App;
```

<br>

### 해당 컴포넌트.js 에서 map 사용
```js
const DiaryList = ({ diaryList }) => {
  console.log(diaryList);
  return (
    <div className="DiaryList">
      <h2>일기 리스트</h2>
      <h4>{diaryList.length}개의 일기가 있습니다.</h4>
      <div>
        {diaryList.map((it) => (
          <div key={it.id}> // key prop 을 만들라고 에러가 뜨는 경우 (해결) 
            <div>작성자 : {it.author}</div>
            <div>일기 : {it.content}</div>
            <div>감정 : {it.emotion}</div>
            <div>작성 시간(ms) : {it.create_date}</div>
          </div>
        ))}
      </div>
    </div>
  );
};

export default DiaryList;
```
- `it` 에는 `diaryList` 배열의 하나하나의 요소가 들어갑니다.
```js
const dummyList = [
  {
    id: 1,
    author: '최현호',
    content: '하이 1',
    emotion: 5,
    create_date: new Date().getTime(), // getTime() 은 시간을 받아서 밀리세컨드로 만들어 준다.
  },
```
- 위의 하나의 객체가 `diaryList` 의 `it` 이 됩니다.
- 그러므로, 객체의 점 표기법으로 접근이 가능 합니다.

<br>

### 만약 console 에서 `key` prop 을 설정하라고 에러가 뜨는 경우
- 위의 코드에 주석 처리한 방법을 사용 -> 현재 진행 중인 상황에서는 각 객체의 `id`를 사용하여 구분이 가능
- 만약 객체에 고유한 데이터가 없는 경우 -> `map` 내장함수의 콜백함수 중 두 번째 파리미터인 `idx` 을 사용하여 가능
```js
const DiaryList = ({ diaryList }) => {
  console.log(diaryList);
  return (
    <div className="DiaryList">
      <h2>일기 리스트</h2>
      <h4>{diaryList.length}개의 일기가 있습니다.</h4>
      <div>
        {diaryList.map((it, idx) => (
          <div key={idx}> 
            <div>작성자 : {it.author}</div>
            <div>일기 : {it.content}</div>
            <div>감정 : {it.emotion}</div>
            <div>작성 시간(ms) : {it.create_date}</div>
          </div>
        ))}
      </div>
    </div>
  );
};

export default DiaryList;
```
- 하지만, `idx`를 사용하면 데이터를 수정하거나, 삭제 또는 추가할 때 `idx` 의 순서가 바뀌는 경우 React 에서 문제가 생길 수 있습니다. 
- 그러므로 고유한 id 를 가지고 있다면 `idx` 를 사용하지 않는 것을 추천 합니다.

<br>

### 현재 까지의 상황
![캡처](https://user-images.githubusercontent.com/87301268/162105732-81981cbc-231e-4011-b9d5-6a629f3107e8.JPG)

<br>

### 삭제, 수정 등 기능 추가를 위해 컴포넌트 세분화 ( 전 )
```html
<div key={idx}> 
     <div>작성자 : {it.author}</div>
     <div>일기 : {it.content}</div>
     <div>감정 : {it.emotion}</div>
     <div>작성 시간(ms) : {it.create_date}</div>
</div>
```
- 렌더하는 아이템들을 별도의 컴포넌트로 분할 합니다.
```js
import DiaryItem from './DiaryItem';

const DiaryList = ({ diaryList }) => {
  console.log(diaryList);
  return (
    <div className="DiaryList">
      <h2>일기 리스트</h2>
      <h4>{diaryList.length}개의 일기가 있습니다.</h4>
      <div>
        {diaryList.map((it) => (
          <DiaryItem key={it.id} {...it} />
        ))}
      </div>
    </div>
  );
};

DiaryList.defaultProps = {
  diaryList: [],
};

export default DiaryList;
```
- 리스트의 아이템이기 때문에 그 전처럼 `key = {it.id}` 를 줍니다.
- 객체 하나에 포함된 모든 아이템(속성) 을 `{...it}` 로 전달 합니다.
- 즉 `it` 이라는 객체에 포함된 모든 데이터가 `DiaryItem` 에게 props 으로 전달이 됩니다.

<br>

### 세분화 후
```js
const DiaryItem = ({ author, content, create_date, emotion, id }) => {
  return (
    <div className="DiaryItem">
      <div className="info">
        <span>
          작성자 : {author} | 감정점수 : {emotion}
        </span>
        <br />
        <span className="date">{new Date(create_date).toLocaleString()}</span>
      </div>
      <div className="content">{content}</div>
    </div>
  );
};

export default DiaryItem;
```

#### 숫자를 다시 현재 날짜로 바꾸는 방법 
```js
<span className="date">{new Date(create_date).toLocaleString()}</span>
```
1. 동일하게 `Date()` 객체를 생성하고, 인자에 전에 만들었던 숫자를 넣어줍니다.
2. `create_date` 의 숫자를 기준으로 `Date` 가 생성이 됩니다.
3. 그 후 `toLocaleString()` 메서드를 사용해서 사람이 이해할 수 있게 바꿔줍니다. 

![캡처](https://user-images.githubusercontent.com/87301268/162110581-a17861c4-eab4-4d2d-904a-cdf13a256b6d.JPG)


<br>

## 참고
- [MDN, map](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/map)
- [인프런](https://www.inflearn.com/course/%ED%95%9C%EC%9E%85-%EB%A6%AC%EC%95%A1%ED%8A%B8/dashboard)
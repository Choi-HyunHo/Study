# 리스트 데이터 삭제하기
추가 할 때와 비슷하게 상위의 `data` 를 바꿔야 합니다.

삭제를 하면 삭제를 한 배열로 `data` 의 state 를 업데이트 해야 합니다.
![KakaoTalk_20220407_162559237](https://user-images.githubusercontent.com/87301268/162143815-e37660ba-5195-4e33-97d6-3ef89cd29505.jpg)

<br>

## 순서
### 생성되는 리스트의 아이템에 버튼을 추가 ( DiaryItem.js )
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
      <button
        onClick={() => {
          console.log(id);
        }}
      >
        삭제하기
      </button>
    </div>
  );
};

export default DiaryItem;
```
- `onClick()` 이벤트 까지 만들어줍니다.
- 현재 위의 버튼을 클릭하면 가장 최신 아이템의 고유 `id` 값을 볼 수 있습니다.

<br>

### 삭제를 하면 App.js 의 data 의 상태를 바꿔야 합니다.
```js
const onDelete = (targetId) => {
   
};

return (
    <div className="App">
      <DiaryEditor onCreate={onCreate} />
      <DiaryList onRemove={onRemove} diaryList={data} /> 
    </div>
  );
```
- `targetId` 로 클릭한 리스트의 아이템 번호를 매개변수로 받습니다.
- 삭제 버튼을 클릭하면, 클릭한 배열 요소의 `Id` 를 `onRemove` 함수에 전달을 해야 합니다.
- 그러므로, `DataItem` 에서 App.js 의 `onRemove` 함수를 호출 할 수 있어야 합니다.
- `DiaryItem` 의 부모인 `DiaryList` 에서 먼저 props 로 내려주고
- 아래 코드 처럼 넘겨야 합니다.

#### DiaryList.js
```js
import DiaryItem from './DiaryItem';

const DiaryList = ({ onRemove, diaryList }) => {
  console.log(diaryList);
  return (
    <div className="DiaryList">
      <h2>일기 리스트</h2>
      <h4>{diaryList.length}개의 일기가 있습니다.</h4>
      <div>
        {diaryList.map((it) => (
          <DiaryItem key={it.id} {...it} onRemove={onRemove} />
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
- App.js -> props 을 사용해 -> `<DiaryList onRemove={onRemove} diaryList={data} /> ` 하고
- DiaryList.js 에서 `const DiaryList = ({ onRemove, diaryList }) => {` 받고
- ` <DiaryItem key={it.id} {...it} onRemove={onRemove} />` 내려줍니다.

#### DiaryItem.js
```js
const DiaryItem = ({ onRemove, author, content, create_date, emotion, id }) => { // onRemove 받기
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
      <button
        onClick={() => {
          console.log(id);
          if (window.confirm(`${id}번째 일기를 삭제 하겠습니까?`)) {
            onDelete(id); 
            // 클릭한 Id 가 App.js 의 targetId 로 넘어가
            // targetId 를 가진 배열 요소를 제외한 
            // 새로운 배열을 만들어서 setData 함수에 전달하고
            // data 배열을 바꾸면 됩니다.
          }
        }}
      >
        삭제하기
      </button>
    </div>
  );
};

export default DiaryItem;
```

#### App.js
```js
const onDelete = (targetId) => {
    const newDiaryList = data.filter((it) => it.id !== targetId);
    setData(newDiaryList);
};
```
- `filter` 를 통해 클릭한 `id`가 아닌 아이템으로만 배열을 다시 생성하고
- `setData` 에 값을 넘겨서 `data`의 상태를 바꿉니다.

![ezgif com-gif-maker (2)](https://user-images.githubusercontent.com/87301268/162149749-9ba3ec57-c23d-4144-9708-b6323b69c926.gif)

<br>

## 참고
- [인프런](https://www.inflearn.com/course/%ED%95%9C%EC%9E%85-%EB%A6%AC%EC%95%A1%ED%8A%B8/dashboard)
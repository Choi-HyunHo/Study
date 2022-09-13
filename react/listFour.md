# 리스트 데이터 수정하기
![KakaoTalk_20220407_162559237](https://user-images.githubusercontent.com/87301268/162167060-ab716306-f0c9-430b-acd8-d39a873e659b.jpg)

<br>

## DiaryItem.js 
### 생성되는 리스트의 아이템에 버튼을 추가
```js
const DiaryItem = ({ onRemove, author, content, create_date, emotion, id }) => {
  const handleRemove = () => {
    if (window.confirm(`${id}번째 일기를 삭제 하겠습니까?`)) {
      onRemove(id);
    }
  };

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
      <button onClick={handleRemove}>삭제하기</button>
      <button>수정하기</button>
    </div>
  );
};

export default DiaryItem;
```
- 전반적인 가독성을 고려해 기존 `onRemove` 함수를 return 위에 따로 정의 하였습니다. 
- 수정하기 버튼을 누르면 아이템의 본문을 수정 할 수 있는 form이 나타나야 합니다.
- 이 부분을 해당 js 파일의 state 로 만듭니다.
```js
const [isEdit, setIsEdit] = useState(false);
```
- `boolean` 타입으로 값을 보관 합니다.
- `isEdit` 의 값이 `True` 가 되면, 수정 중으로 간주하여 로직을 만들고
- `isEdit` 의 값이 `False` 가 되면, 기존 처럼 content 를 보여주면 됩니다. 

<br>

```js
const toggleIsEdit = () => setIsEdit(!isEdit);
```
- `toggleIsEdit` 함수는 호출이 되는 순간 원래 `isEdit` 이 가지고 있던 값을 반전 시킵니다.
- 즉, `true -> false` 로 `false -> true` 
- `toggleIsEdit` 함수를 버튼을 누르면 작동 할 수 있게 넣어줍니다.

<br>

### isEdit 의 값이 True 라는 가정으로 수정 폼을 띄우는 코드
- 기존 `{content}` 를 지우고 삼항 연산자를 활용 합니다.
- 수정 폼에 입력하는 데이터들도 state 로 핸들링이 가능하게 만듭니다.
```js

import { useState } from 'react';

const DiaryItem = ({ onRemove, author, content, create_date, emotion, id }) => {
  const handleRemove = () => {
    if (window.confirm(`${id}번째 일기를 삭제 하겠습니까?`)) {
      onRemove(id);
    }
  };

  const [isEdit, setIsEdit] = useState(false);
  const toggleIsEdit = () => setIsEdit(!isEdit);

  // 수정 폼에 입력하는 데이터를 state 로 핸들링 하기 위한 코드
  const [localContent, setLocalContent] = useState('');

  return (
    <div className="DiaryItem">
      <div className="info">
        <span>
          작성자 : {author} | 감정점수 : {emotion}
        </span>
        <br />
        <span className="date">{new Date(create_date).toLocaleString()}</span>
      </div>
      <div className="content">
        {isEdit ? (
          <>
            <textarea
              value={localContent}
              onChange={(e) => {
                setLocalContent(e.target.value);
              }}
            ></textarea>
          </>
        ) : (
          <>{content}</>
        )}
      </div>
      <button onClick={handleRemove}>삭제하기</button>
      <button onClick={toggleIsEdit}>수정하기</button>
    </div>
  );
};

export default DiaryItem;
```

<br>

### 수정하기 버튼을 누르면 아래의 버튼을 바꾸는 코드
```js
{isEdit ? (
        <> // 수정 중
          <button onClick={toggleIsEdit}>수정 취소</button> // isEdit = false 가 되면 수정 취소
          <button>수정 완료</button> // 예정
        </>
      ) : (
        <>
          <button onClick={handleRemove}>삭제하기</button>
          <button onClick={toggleIsEdit}>수정하기</button>
        </>
      )}
```

<br>

### 수정 폼이 열리면, 원본 content를 수정 할 수 있게 하기
```js
const [localContent, setLocalContent] = useState(content);
```

<br>

#### 아래와 같은 문제를 해결 하기 위해
![ezgif com-gif-maker (3)](https://user-images.githubusercontent.com/87301268/162166958-b0788b94-b9ee-4c4e-9af6-f7d858a437e9.gif)

```js
const handleQuitEdit = () => {
    setIsEdit(false); // 수정 중인 상태를 빠져 나가기 위해서
    setLocalContent(content); // 
  };

<button onClick={handleQuitEdit}>수정 취소</button>
```

<br>

## 수정 완료 버튼을 누르면 그대로 저장
- React의 특징 중 데이터는 위에서 아래로, 이벤트는 아래에서 위로 올라갑니다.
- 수정 완료 이벤트를 `DiaryItem` 에서 `App.js` 까지 전달하기 위해서는 데이터를 가지고 있는 `App.js` 컴포넌트에 수정하는 기능을 만들어서 `DiaryItem` 까지 보내줘야 합니다. 

<br>

### 수정하는 기능을 하는 함수
#### App.js
```js
 const onEdit = (targetId, newContent) => { // 수정대상 아이디, 수정된 내용
    setData(
      data.map((it) =>
        it.id === targetId ? { ...it, content: newContent } : it
      )
    );
  };
```
- 데이터의 값을 바꾸면 됩니다.
- `map` 을 사용해서 각각 모든 요소들이 id 와 `현재 매개변수로 전달받은 id` 같은지 검사 합니다.
- id 는 고유 값이기 때문에 하나 밖에 존재 할 수 없습니다.
- id 가 일치하게 되면 해당 요소는 수정 대상이 되는 요소입니다.
- 그리하여 원본 데이터를 다 불러온 다음에 `{ ...it }`
- `content : newContent` 로 업데이트 시켜주면 됩니다.
- id 가 일치하지 않으면 수정 대상이 아니어서 `it` 을 반환 합니다.
- `onEdit` 함수는 `DataItem` 에서 발생하는 이벤트임으로 부모인 `DataList` 를 거쳐가야 합니다. 
( 리스트 삭제 하는 경우와 동일 )

<br>

#### DiaryItem.js
```js
const handleEdit = () => {
    if (localContent.length < 5) {
      localContentInput.current.focus();
      return;
    }

    if (window.confirm(`${id}번 째 일기를 수정하시겠습니까?`)) {
      onEdit(id, localContent); // targetId, newContent 에 전달할 인자
      toggleIsEdit(); // 수정폼을 닫습니다.
    }
  };
```
- 수정완료 버튼에 필요한 이벤트를 처리 할 함수를 하나 만듭니다.
- 그리고, 입력할 때 조건을 걸어뒀었기 때문에 그에 맞는 focus 와 검사를 해야 합니다.
- useRef 를 사용하여 `<textarea/>` 태그에 DOM 접근

![ezgif com-gif-maker (4)](https://user-images.githubusercontent.com/87301268/162198338-1a11883b-36aa-4e67-a6fe-816170be26bb.gif)

<br>

## 참고
- [인프런](https://www.inflearn.com/course/%ED%95%9C%EC%9E%85-%EB%A6%AC%EC%95%A1%ED%8A%B8/dashboard)
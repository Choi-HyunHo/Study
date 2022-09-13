# 최적화 - useMemo
__연산 결과 재사용__ 하는 방법

## 정의
- __이미 계산 해 본 연산 결과를 기억 하고 있다가, 
똑같은 계산을 시키면 다시 계산하지 않고 답만 반환하는 방법__
- Ex) 수학 시험을 볼 때, 이미 풀어본 문제는 다시 풀어보지 않아도 정답을 알고 있는 것

![KakaoTalk_20220408_170245269](https://user-images.githubusercontent.com/87301268/162392619-913a8523-7271-4830-b074-0ca3026e710c.jpg)

![KakaoTalk_20220408_170245673](https://user-images.githubusercontent.com/87301268/162392629-b0b923ea-5215-47c4-8254-44c57200806a.jpg)

<br>

## emotion 별 일기의 개수와 비율
### App.js
```js
  const getDiaryAnalysis = () => {
    console.log('일기 분석 시작')

    const goodCount = data.filter((it)=>it.emotion >= 3).length;
    const badCount = data.length - goodCount;
    const goodRatio = (goodCount / data.length) * 100;
    return {goodCount, badCount, goodRatio}
  }

  const {goodCount, badCount, goodRatio} = getDiaryAnalysis(); // 함수로 호출한 결과 값을 객체로 반환
```
1. `getDiaryAnalysis` 함수를 선언하여 filter 를 이용하여 `emotion` >= 3 이상인 것을 `goodCount` 에 담았습니다.
2. 그리고 `badCount` 는 전체 일기에서 `goodCount` 를 뺀 수 
3. `goodRatio` 는 전체에서 `(goodCount / data.length) * 100;` 한 값 
4. 리턴 값을 객체로 받습니다.

<br>

```js
return (
    <div className="App">
      <DiaryEditor onCreate={onCreate} />
      <div>전체 일기 : {data.length}</div>
      <div>기분 좋은 일기 개수 : {goodCount}</div>
      <div>기분 나쁜 일기 개수 : {badCount}</div>
      <div>기분 좋은 일기 비율 : {goodRatio}</div>
      <DiaryList onEdit={onEdit} onRemove={onRemove} diaryList={data} />
    </div>
  );
```
![캡처](https://user-images.githubusercontent.com/87301268/162396429-58763cfa-b14f-41b7-9068-1969b26d9261.JPG)

- 실행을 하고 console 을 보면 `console.log('일기 분석 시작')` 이 2번 찍혀서 나옵니다.
- 나오는 이유는 ( 전체 코드 )
```js
import { useEffect, useRef, useState } from 'react';
import './App.css';
import DiaryEditor from './DiaryEditor';
import DiaryList from './DiaryList';

function App() {
  // API 호출 함수
  const getData = async () => {
    const res = await fetch(
      'https://jsonplaceholder.typicode.com/comments'
    ).then((res) => res.json());

    // 가져와서 사용할 데이터 ( 0 ~ 20 )
    const initData = res.slice(0, 20).map((it) => {
      return {
        author: it.email,
        content: it.body,
        emotion: Math.floor(Math.random() * 5) + 1,
        create_date: new Date().getTime(),
        id: dataId.current++,
      };
    });

    setData(initData);
  };

  useEffect(() => {
    getData();
  }, []);

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

  const onRemove = (targetId) => {
    const newDiaryList = data.filter((it) => it.id !== targetId);
    setData(newDiaryList);
  };

  const onEdit = (targetId, newContent) => {
    setData(
      data.map((it) =>
        it.id === targetId ? { ...it, content: newContent } : it
      )
    );
  };

  const getDiaryAnalysis = useMemo() => {
    console.log('일기 분석 시작');

    const goodCount = data.filter((it) => it.emotion >= 3).length;
    const badCount = data.length - goodCount;
    const goodRatio = (goodCount / data.length) * 100;
    return { goodCount, badCount, goodRatio };
  };

  const { goodCount, badCount, goodRatio } = getDiaryAnalysis(); // 함수로 호출한 결과 값을 객체로 반환

  return (
    <div className="App">
      <DiaryEditor onCreate={onCreate} />
      <div>전체 일기 : {data.length}</div>
      <div>기분 좋은 일기 개수 : {goodCount}</div>
      <div>기분 나쁜 일기 개수 : {badCount}</div>
      <div>기분 좋은 일기 비율 : {goodRatio}</div>
      <DiaryList onEdit={onEdit} onRemove={onRemove} diaryList={data} />
    </div>
  );
}

export default App;
```

1. `App` 컴포넌트가 첫 Mount 가 될 때, `data` state 의 값은 빈 배열이었습니다.
    `const [data, setData] = useState([]);`
    
2. 그 순간에 `const {goodCount, badCount, goodRatio} = getDiaryAnalysis();` 	함수를 한 번 호출하게 됩니다.
    
3. 그래서 이 위의 return 문에 데이터가 0, 0, 0 들어가는 것 처럼 동작 합니다.
4. 그 후 아래의 코드가 성공하고 `setData(initData)` 가 이루어지게 되면서
```js
const getData = async () => {
    const res = await fetch(
      'https://jsonplaceholder.typicode.com/comments'
    ).then((res) => res.json());

    // 가져와서 사용할 데이터 ( 0 ~ 20 )
    const initData = res.slice(0, 20).map((it) => {
      return {
        author: it.email,
        content: it.body,
        emotion: Math.floor(Math.random() * 5) + 1,
        create_date: new Date().getTime(),
        id: dataId.current++,
      };
    });

    setData(initData);
  };
```
5. `data` 가 한 번 바뀌게 됩니다.
6. 그러면 `App` 컴포넌트가 리렌더가 일어나기 때문에 `App` 컴포넌트 안의 모든 함수들이 재생성이 되게 되고, 2번의 코드가 다시 수행이 되어 호출 됩니다.

- 만약, 일기를 수정하게 되면 `data` state 의 상태가 변했기 때문에 `App` 컴포넌트가 한 번 더 리렌더가 일어납니다. 그렇게 되면 `getDiaryAnalysis` 는 또 실행이 됩니다.

- 하지만 데이터를 수정하는 것은 일기 분석의 결과, 즉 emotion 에 아무런 영향을 미칠 수 없는데 계속 호출이 됩니다.

- 이런 경우 `useMemo` 를 사용 할 수 있습니다.

<br>

### useMemo 사용
```js
 const getDiaryAnalysis = useMemo(
    () => { // 첫 번째 인자 
    console.log('일기 분석 시작');

    const goodCount = data.filter((it) => it.emotion >= 3).length;
    const badCount = data.length - goodCount;
    const goodRatio = (goodCount / data.length) * 100;
    return { goodCount, badCount, goodRatio };
  },[data.length] // 두 번째 인자 
  );
```
- __`import` 를 하고, 최적화 하고 싶은 함수를 감싸주면 됩니다.__
- __첫 번째 인자로 콜백 함수__ 를 받아서 콜백 함수가 리턴하는 값을 최적화 할 수 있도록 도와줍니다.
- __두 번째 인자로 배열__ 을 전달 합니다. ( useEffect 의존성 배열과 동일 )
- 결론, 위의 코드는 `[data.lenght]` 가 변화 할 때 만 콜백함수가 다시 수행하게 됩니다. 
- `data` 의 state 가 변해도, `[data.lenght]` 가 변하지 않는 이상 `getDiaryAnalysis` 는 호출 하지 않습니다.

> 하지만 위의 코드를 그대로 실행하면 오류가 납니다. useMemo로 감싸고 , 의존성 배열을 전달을 해서 함수를 최적화를 하면, 더 이상 함수가 아니게 됩니다! <br>
useMemo 기능은 함수를 전달을 받아서 콜백함수가 리턴하는 값을 리턴하게 됩니다.

- 즉, `const getDiaryAnalysis` 는 함수가 아니라 값을 리턴 받게 됩니다.
```js
//const { goodCount, badCount, goodRatio } = getDiaryAnalysis(); X 
  const { goodCount, badCount, goodRatio } = getDiaryAnalysis;
```
- 이렇게 바꿔야 정상적으로 동작이 됩니다.

***

## 정리
### useMemo()
- 어떤 함수가 있고 그 함수가 어떤 값을 리턴하고 있는데 그 리턴까지의 연산을 최적화 하고 싶다면, 
- `useMemo` 를 사용해서 의존성 배열에 어떤 값이 변화할 때만 연산을 다시 실행 할 것인지 명시하게 되면
- 그 함수를 __값__ 처럼 사용을 해서 연산 최적화를 할 수 있습니다.

#### 즉,
- 연산량이 높은 작업이 매번 렌더링 될 때마다 반복되는 것을 피하기 위해 사용
- __렌더링이 일어나는 동안 실행__ 되므로, 렌더링이 일어나는 동안 실행돼서는 안될 작업을 useMemo() 에 넣어서는 안됨

#### 사용법
```js
 const memoizedValue = useMemo(값 생성 함수, 의존성 배열);
```
- 의존성 배열이 들어있는 변수가 변했을 경우에만 새로 값 생성 함수를 호출하여 결과 값을 반환
- __의존성 배열을 넣지 않을 경우 렌더링이 일어날 때 마다 매번 값 생성 함수가 실행되므로 의미가 없음__
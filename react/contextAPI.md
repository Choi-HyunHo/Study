# Context API
컴포넌트 트리에 데이터 공급

<br>

## 계층 구조
![KakaoTalk_20220411_164345714](https://user-images.githubusercontent.com/87301268/162688798-2b448ac4-d73e-4382-b094-2f700bcb4bae.jpg)

- 위의 사진은 현재 만들어본 프로젝트의 계층 구조
- `<DiaryList/>` 는 3 개의 props 을 받지만, 그 중에 빨간색으로 두 개는 자신이 사용하지 않는 props 입니다.
- 전달만 하는 컴포넌트가 중간에 많이 생기게 되는 경우에는 props 의 이름도 바꾸기가 어려워지고, 코드 작성과 수정에 악영향을 미치게 됩니다.
- 이러한 상황을 마치 props 가 드릴처럼 땅을 파고 들어간다고 __props 드릴링__ 이라고 부릅니다.
- 부모에게서 자식으로만 데이터를 전달하는 단방향 데이터 흐름을 지키는 React 의 문제라고도 생각 할 수 있습니다.

<br>

## Context 의 개념
![KakaoTalk_20220411_181029823](https://user-images.githubusercontent.com/87301268/162704091-f83d46a4-c2cc-4379-8a30-1425a4b41610.jpg)

1. 모든 데이터를 가지고 있는 컴포넌트가 `Provider` 라고 공급자 역할을 하는 자식 컴포넌트에게 자신이 가지고 있는 모든 데이터를 다 줍니다.
2. `Provider` 컴포넌트는 자신의 자손에 해당하는 모든 컴포넌트들에게 직접 데이터를 전달 합니다.

- `Provider` 컴포넌트에 자식 컴포넌트들로 배치되어 해당 `Provider` 컴포넌트가 공급하는 모든 데이터에 접근 할 수 있는 컴포넌트들의 영역을 문맥(Context) 이라고 표현 합니다.

<br>

## Context API 사용법
### 1. Context 생성
```js
const MyContext = React.createContext(defaultValue);
```

<br>

### 2. Context Provider 를 통한 데이터 공급
```html
<MyContext.Provider value = {전역으로 전달하고자 하는 값}>
  {/* 이 Context 안에 위치할 자식 컴포넌트들 */}
<MyContext.Provider>
```
- `value` 라는 props 를 받아서 그 안에있는 값을 `{}` 안에 있는 자식 컴포넌트들에게 전달하게 되는 기능 입니다.
- 값을 전달 받을 수 있는 컴포넌트 수의 제한은 없습니다.

<br>

### 3. 값을 받아와서 사용 하는 경우
#### App.js
```js
// export 하는 이유는, 내보내줘야 다른 컴포넌트들이 접근이 가능 합니다.
export const DiaryStateContext = React.createContext();

return (
    <DiaryStateContext.Provider value={data}>
      <DiaryDispatchContext>
        <div className="App">
          <DiaryEditor onCreate={onCreate} />
          <div>전체 일기 : {data.length}</div>
          <div>기분 좋은 일기 개수 : {goodCount}</div>
          <div>기분 나쁜 일기 개수 : {badCount}</div>
          <div>기분 좋은 일기 비율 : {goodRatio}</div>
          <DiaryList onEdit={onEdit} onRemove={onRemove} />
        </div>
      </DiaryDispatchContext>
    </DiaryStateContext.Provider>
  );
```

#### DiaryList.js
```js
const DiaryList = ({ onEdit, onRemove }) => {
  const diaryList = useContext(DiaryStateContext);
```
- `{diaryList}` 를 삭제 했습니다.


<br>

### 주의해야 할 점
- 값 뿐만 아니라 상태 변화 함수도 사용 할 수 있습니다.
- 하지만 이럴 때 더 조심해야 합니다.
- `Provider` 도 결국 컴포넌트 입니다. 
- 즉, props 이 바뀌면 재생성 됩니다. `Provider` 가 재생성되면 밑에 컴포넌트까지 재생성 됩니다.
- 만약 ` <DiaryStateContext.Provider value={data, 상태변화함수 ...}>` 이렇게 하게 되면,
- `data` 가 바뀔 때 마다 리렌더링이 일어나게 됩니다.
- 이런 경우 Context 를 중첩으로 사용 할 수 있습니다.

<br>

### Context 를 중첩으로 사용
- `DiaryStateContext` 는 오직 `data` 만 공급하기 위해서 만들고
- 상태 변화 함수(dispatch) 를 내보내기 위해서 `DiaryDispatchContext` 를 만듭니다.

#### App.js
```js
export const DiaryDispatchContext = React.createContext();

const memoizedDispatches = useMemo(() => {
    return { onCreate, onRemove, onRemove };
  }, []); // 재생성되지 않게 빈 배열 저달

 return (
    <DiaryStateContext.Provider value={data}>
      <DiaryDispatchContext value={memoizedDispatches}>
        <div className="App">
          <DiaryEditor onCreate={onCreate} />
          <div>전체 일기 : {data.length}</div>
          <div>기분 좋은 일기 개수 : {goodCount}</div>
          <div>기분 나쁜 일기 개수 : {badCount}</div>
          <div>기분 좋은 일기 비율 : {goodRatio}</div>
          <DiaryList />
        </div>
      </DiaryDispatchContext>
    </DiaryStateContext.Provider>
  );
```
<br>

#### DiaryEditor.js
```js
const DiaryEditor = () => { 
  const { onCreate } = useContext(DiaryDispatchContext);
```
- props 를 지우고, `{ onCreate}` 가져옵니다.
- `DiaryDispatchContext` 공급하고 있는 값은 함수 3개로 이루어진 객체이기 때문에 비구조화 할당으로 가져옵니다.

<br>

## 참고
- [인프런](https://www.inflearn.com/course/%ED%95%9C%EC%9E%85-%EB%A6%AC%EC%95%A1%ED%8A%B8/dashboard)
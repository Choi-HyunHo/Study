# 최적화 - useCallback

## 문제점
- 전에 만든 일기 프로젝트에서 일기를 삭제하면 일기를 생성하는 `Editor` 가 렌더링이 됩니다.
- 테두리를 잘보면 깜빡 입니다.
- 일기를 삭제 하는 과정에서 일기 `Editor` 가 렌더링이 될 이유는 전혀 없습니다.
- 그러므로 `Editor` 를 최적화 하겠습니다.

![ezgif com-gif-maker (14)](https://user-images.githubusercontent.com/87301268/162569817-355fe493-fbf4-4eb3-999f-9fac136a8d42.gif)

- 컴포넌트가 렌더링이 일어나는 조건은 본인이 가지고 있는 state 의 변화가 생기거나, 부모 컴포넌트가 리렌더링이 일어나거나, 자신이 받은 prop 이 변경되게 되는 경우 리렌더링이 일어납니다.

<br>

### DiaryEditor.js
```js
const DiaryEditor = ({ onCreate }) => {
....  
export default React.memo(DiaryEditor); // React.memo 사용

```
- 현재 `DiaryEditor` 는 onCreate 하나만 함수로 받고 있습니다.
- `React.memo` 를 사용해서 최적화 할 수 있다고 생각 할 수 있지만, `onCreate` 는 객체를 생성하는 함수 입니다.
- 이전 포스팅에서 본 것처럼 비원시타입인 객체는 얕은 복사가 일어나고 단순히 `React.memo` 를 사용해서 최적화 할 수 없습니다.
- 반대로 `useMemo` 를 사용해서 최적화가 가능하지 않을까 생각 할 수 있습니다.
- 하지만 사용하면 안됩니다. `useMemo` 는 함수를 반환하지 않고 값을 반환 합니다.
- 현재 원하는 상황은 아래의 코드를 온전히 `DiaryEditor` 에게 보내주는 것 입니다.
```js
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
```
- 새롭게 배울 `useCallback` 으로 해보겠습니다.

<br>

## useCallback
```js
const memoizedCallback = useCallback(
  () => {
    doSomething(a, b); // 첫 번째 인자 (콜백함수)
  },
  [a, b], // 두 번째 인자 (의존성 배열)
);
```
- 구조는 `useEffect`, `useMemo` 와 동일하게 생겼습니다.
- __메모이제이션된 콜백__을 반환 합니다.
- 즉, 값을 반환하는게 아니라 `() => { 콜백 함수 }` 를 반환 합니다.
- `useCallback` 은 두 번째 인자로 전달한 의존성 배열 안에 들어있는 값이 변하지 않으면 첫 번째 인자로 전달한 콜백함수를 계속 재사용 할 수 있도록 도와줍니다.

<br>

### 적용
```js
 const onCreate = useCallback((author, content, emotion) => {
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
  }, []);
```
![ezgif com-gif-maker (15)](https://user-images.githubusercontent.com/87301268/162572835-0d7bbd2c-10ae-439f-8855-87e89fc65358.gif)

- 하지만 일기를 저장하면 기존에 있던 일기가 전부 사라지고 
방금 작성한 일기만 남는 것을 볼 수 있습니다.
- 이유는, `useCallback` 을 사용하면서 `[]` 에 아무 값도 넣지 않아서 그렇습니다.
- `onCreate` 함수는 컴포넌트가 Mount 되는 시점에 한 번만 생성되기 때문에 그 당시에 `data` state 의 값은 빈 배열 입니다.
- `onCreate` 함수가 가장 마지막으로 생성되었을 때 빈 배열이었으니 나머지 값은 삭제가 되고 새롭게 추가한 하나만 생성이 됩니다. 그래서 아무리 추가하여도 1개 이상 추가 될 수 없습니다.

> 함수는 컴포넌트가 재생성 될때 다시 생성되는 이유가 있습니다. 
바로 현재의 state값을 참조할 수 있어야 하기 때문 입니다. 

- 그런데 `onCreate` 함수는 콜백안에서 뎁스를 빈배열로 전달했기 때문에 이 `onCreate` 함수가 알고 있는 data의 값은 `[]` 빈 배열 뿐 입니다.

<br>

#### 해결하기 위해서?
- 결론적으로 의존성 배열에 `data` 를 넣어주어야 합니다.
- 그런데 이 `[]` 는 아까도 말했듯이 `data` 에 있는 값이 변경되면 `onCreate` 함수를 재생성 하게 되는데 이렇게되면 또 문제가 `useCallback`함수를 쓰기 전과 동일하게 됩니다.

<br>

#### 이럴 떄 함수형 업데이트를 사용 합니다.
- `setData` 에는 값이 아니라 함수를 전달 할 수도 있습니다.
```js
 const onCreate = useCallback(
    (author, content, emotion) => {
      const create_date = new Date().getTime();
      const newItem = {
        author,
        content,
        emotion,
        create_date,
        id: dataId.current, // 0 을 가리킨다.
      };

      dataId.current += 1; // id 의 값이 1씩 증가한다.
      setData((data) => [newItem, ...data]); // 새로운 아이템이 위로 올라오게 하기 위해서 newItem 을 먼저 사용
    },
    []
  );
```
- `[]` 를 비워도 항상 최신의 state 를 `data` 인자를 통해 참고할 수 있게 되면서 비워도 가능합니다.

![ezgif com-gif-maker (16)](https://user-images.githubusercontent.com/87301268/162581333-b46da758-7287-4e92-866b-c370d821cf72.gif)


***

## 정리
### useCallback()
- useMemo() 훅과 비슷하지만 __값이 아닌 함수를 반환__ 한다는 점이 다름
- 컴포넌트 내에 함수를 정의하면 매번 렌더링이 일어날 때 마다 함수가 새로 정의되므로, 
- __useCallback()__ 훅을 사용하여 불필요한 함수 재정의 작업을 없애는 것

### 사용법
```js
const memoizedCallback = useCallback(콜백 함수, 의존성 배열);
```
- 의존성 배열에 들어있는 __변수__ 가 변했을 경우에만 콜백 함수를 다시 재정의하여 리턴함

***

## 참고
- https://ko.reactjs.org/docs/hooks-reference.html#usecallback
- [인프런](https://www.inflearn.com/course/%ED%95%9C%EC%9E%85-%EB%A6%AC%EC%95%A1%ED%8A%B8/dashboard)
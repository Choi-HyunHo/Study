# React - Router 응용
![KakaoTalk_20220412_204058080](https://user-images.githubusercontent.com/87301268/162952289-feaec8e1-ac2f-431a-ac21-33ae5d1e56d6.jpg)

<br>

## Path Variable
- 경로의 변수를 사용
- __URL에 변수를 담아서 전달하는 방법__
- `useParams` 사용

<br>

### 여러 가지의 게시물을 받게되는 경우
- 1번 게시물인지, 2번 게시물인지 ..... n번 게시물인지

#### App.js
```js
import './App.css';
import { BrowserRouter, Route, Routes } from 'react-router-dom';

import RouteTest from './components/RouteTest';

import Home from './pages/Home';
import New from './pages/New';
import Edit from './pages/Edit';
import Diary from './pages/Diary';

function App() {
  return (
    <BrowserRouter>
      <div className="App">
        <h2>App.js</h2>
        <Routes>
          <Route path="/" element={<Home />} />
          <Route path="/new" element={<New />} />
          <Route path="/edit" element={<Edit />} />
          <Route path="/diary/:id" element={<Diary />} />
        </Routes>
        <RouteTest />
      </div>
    </BrowserRouter>
  );
}

export default App;
```
- `<Route path="/diary/" element={<Diary />} />` 사용하고 URL 에 `http://localhost:3000/diary/1` 요청하면 아래와 같이 나옵니다.

![캡처](https://user-images.githubusercontent.com/87301268/162949912-4badf55f-f9f8-42b1-a4e7-4950b43302ee.JPG)

- 하지만, `<Route path="/diary/:id" element={<Diary />} />` 사용하고 URL 에
`http://localhost:3000/diary/1` 요청하면 아래와 같이 나옵니다.

![캡처](https://user-images.githubusercontent.com/87301268/162950284-9e90ffa7-3d6a-47ea-9d35-8c2b1a6848df.JPG)

- 만약, 위와 같이 작성을 하게 되면 무조건 `/:id` 를 받겠다고 생각을 해서, 
`http://localhost:3000/diary/` 를 요청하면 아무것도 나오지 않습니다.
- 이런 경우 별도의 처리를 할 수 있는데 아래의 코드 처럼 할 수도 있습니다.
```js
<Route path="/diary/:id" element={<Diary />} />
<Route path="/diary/" element={<Diary />} />
```

<br>

## useParmas
- 전달 받아서 들어오게 되는 `Path Variable` 을 모아서 객체로 전달하게 되는데, 현재 `Path Variable` 을 App 컴포넌트에서 `:id` 로 부르기로 했습니다.

<br>

### Diary.js
```js
import { useParams } from 'react-router-dom';

const Diary = () => {
  const { id } = useParams();
  console.log(id);

  return (
    <div>
      <h1>Diary</h1>
      <p>이곳은 일기 상세 페이지 입니다.</p>
    </div>
  );
};

export default Diary;
```
![ezgif com-gif-maker (20)](https://user-images.githubusercontent.com/87301268/162952152-4d06c453-daec-4f3b-be8b-5fc2ee7cba87.gif)

<br>

## Query String
![KakaoTalk_20220412_205312611](https://user-images.githubusercontent.com/87301268/162955910-ae1770c9-b95c-4cf5-a3d6-0fe078db70cd.jpg)

```js
const [searchParams, setSearchParams] = useSearchParams();
```
- `searchParams` 는 `.get()` 을 통해서 전달 받은 Query String 을 꺼내서 쓸 수 있는 용도로 사용
- `setSearchParams` 는 `searchParams` 를 변경 시키는 기능을 합니다.

<br>

### Edit.js
```js
import { useSearchParams } from 'react-router-dom';

const Edit = () => {
  const [searchParams, setSearchParams] = useSearchParams();

  const id = searchParams.get('id');
  console.log('id : ', id);

  const mode = searchParams.get('mode');
  console.log('mode : ', mode);

  return (
    <div>
      <h1>Edit</h1>
      <p>이곳은 일기 수정 페이지 입니다.</p>
      <button onClick={() => setSearchParams({ who: 'hyunho' })}>
        QS 바꾸기
      </button>
    </div>
  );
};

export default Edit;
```

![ezgif com-gif-maker (23)](https://user-images.githubusercontent.com/87301268/163018253-7c92e436-57f1-4e2d-9356-1caf727eea7b.gif)


<br>

## Page Moving
```js
const navigate = useNavigate();
```
- 페이지를 이동 시킬 수 있는 기능
- `useNavigate` 라는 훅은 페이지를 이동 시킬 수 있는 기능을 하는 함수를 하나 반환을 하는데, 함수의 이름을 `navigate` 라고 받고 `navigate` 의 인자로 경로를 작성을 하면 호출을 해서 경로를 옮길 수 있습니다.
```js
navigate('/home');
```
- `Link` 태그를 클릭 안했을 경우에도 의도적으로 페이지를 바꿀 수 있습니다.

<br>

### Edit.js
```js
import { useNavigate, useSearchParams } from 'react-router-dom';

const Edit = () => {
  const navigate = useNavigate();
  const [searchParams, setSearchParams] = useSearchParams();

  const id = searchParams.get('id');
  console.log('id : ', id);

  const mode = searchParams.get('mode');
  console.log('mode : ', mode);

  return (
    <div>
      <h1>Edit</h1>
      <p>이곳은 일기 수정 페이지 입니다.</p>
      <button onClick={() => setSearchParams({ who: 'hyunho' })}>
        QS 바꾸기
      </button>

      <button
        onClick={() => {
          navigate('/home');
        }}
      >
        HOME 으로 가기
      </button>
    </div>
  );
};

export default Edit;
```
![ezgif com-gif-maker (22)](https://user-images.githubusercontent.com/87301268/162959620-012f4828-22c6-4e37-a878-515b513a6ed7.gif)

<br>

```js
<button
  onClick={() => {
    navigate(-1);
  }} 
>
     뒤로가기
</button>
```
- 위와 같은 방식으로도 사용 가능 ( Edit -> home 으로 바뀝니다. )

<br>

## 참고
- [인프런](https://www.inflearn.com/course/%ED%95%9C%EC%9E%85-%EB%A6%AC%EC%95%A1%ED%8A%B8/dashboard)
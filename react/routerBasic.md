# CSR 라이브러리 설치
1. https://reactrouter.com/ 접속
2. Read the Docs 클릭
3. 
![캡처](https://user-images.githubusercontent.com/87301268/162917036-aa075193-dfd9-4f30-b192-e45a42b9f9f6.JPG)
4. VScode 터미널에서 명령어 입력하고 설치
5. 설치가 정상적으로 됬는지 확인하기 위해 `package.json` 파일에서
6. 확인하기 
![캡처](https://user-images.githubusercontent.com/87301268/162917771-b77f31a8-aacf-4251-a574-79efc657d725.JPG)
7. npm start 로 프로젝트 시작

<br>

## 프로젝트에서 직접 Page Routing 시도하기
### 현재 폴더 구조
![캡처](https://user-images.githubusercontent.com/87301268/162919691-ddfa22c5-427f-4207-ab44-ace142359da2.JPG)

<br>

### 브라우저 URL 과 React App 을 연결
#### App.js
```js
import './App.css';
import { BrowserRouter } from 'react-router-dom'; // 브라우저 URL 과 React App 을 연결 하는 기능

function App() {
  return (
    <BrowserRouter> // 최상위 태그로 감싸기
      <div className="App">
        <h2>App.js</h2>
      </div>
    </BrowserRouter>
  );
}

export default App;
```
- 감싸져 있는 부분들은 브라우저 URL 과 매핑 될 수 있습니다.
- 그리고 만들어 놓은 다른 페이지들을 import 합니다.
- 브라우저의 URL 이 바뀌게 되면 어떤 컴포넌트를 렌더링 해서, 그 컴포넌트가 페이지 역할을 하게 할 것인지 결정하기 위해 바뀔 부분을 `<Routes>` 라는 컴포넌트로 감싸줍니다.
- 그 안에 ` <Route />` 컴포넌트를 사용하는데 __URL 경로와 컴포넌트를 매핑 시켜주는 역할을 합니다.__

<br>

```js
import './App.css';
import { BrowserRouter, Route, Routes } from 'react-router-dom';

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
    		// 경로가 index 일 때(path = '/') element = { 컴포넌트 }
          <Route path="/" element={<Home />} />
        </Routes>
      </div>
    </BrowserRouter>
  );
}

export default App;
```

<br>

#### 개발자 도구에서 확인해보면
![캡처](https://user-images.githubusercontent.com/87301268/162924266-ddc5ccdb-ba8e-4867-88e4-909c093888c2.JPG)

- `Router.Provider` 컴포넌트 아래 `Home` 컴포넌트가 정상적으로 추가된 것을 볼 수 있습니다.
- 현재 `<Route path="/" element={<Home />} />` 가 index 를 가르키고 있으면, `Home` 컴포넌트를 렌더해라. 라고 명령을 내렸는데 현재 웹 브라우저 URL 이 `/` 아무것도 없는 것과 똑같기 때문에 `Router.Provider` 가 전달받은 `Home` 컴포넌트를 매핑시켜서 렌더 했습니다.

<br>

#### 경로를 직접 해보면
```js
import './App.css';
import { BrowserRouter, Route, Routes } from 'react-router-dom';

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
        </Routes>
      </div>
    </BrowserRouter>
  );
}

export default App;
```

![ezgif com-gif-maker (18)](https://user-images.githubusercontent.com/87301268/162926724-ded412ef-1882-481e-aa15-3bbb8f950b2c.gif)

- 페이지가 바뀌어도 App.js 부분이 나오는 이유는 바뀔 때 변화하는 부분이 `<Routes>` 안에 있어야 하는데 `<Routes>` 바깥에 있는 부분은 그대로 유지가 됩니다.
- 페이지에 상관없이 전체적으로 보여지는 요소들을 작성해야 한다고 하면, `<Routes>` 컴포넌트 바깥으로 빼서 작성을 하면 어떤 페이지에서도 다 나타나는 요소들을 사용 할 수 있습니다.

<br>

## Page 이동시키는 요소 만들기
- HTML 에서는 페이지 이동을 할 때 `<a>` 태그를 사용 합니다.
- 하지만 누르면 페이지가 새로 고침이 되고 이 말은, SPA 가 아니라 MPA 의 특징 입니다.
- `react-router` 를 사용하는 어플리케이션은 `<a>` 태그를 사용하지 않습니다.

<br>

### CSR 방식 - Link
```js
<Link to={'경로전달'}></Link>
```

<br>

#### 현재 폴더 구조
![캡처](https://user-images.githubusercontent.com/87301268/162930569-2a19cb16-f283-4318-a6e1-1f77a61b44e3.JPG)

```js
// RouteTest.js
import { Link } from 'react-router-dom';

const RouteTest = () => {
  return (
    <>
      <Link to={'/'}>Home</Link>
      <br />
      <Link to={'/diary'}>Diary</Link>
      <br />
      <Link to={'/new'}>New</Link>
      <br />
      <Link to={'/edit'}>Edit</Link>
      <br />
    </>
  );
};

export default RouteTest;
```

<br>

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
          <Route path="/diary" element={<Diary />} />
        </Routes>
        <RouteTest />
      </div>
    </BrowserRouter>
  );
}

export default App;
```
![ezgif com-gif-maker (19)](https://user-images.githubusercontent.com/87301268/162931955-b9f9b642-eda1-43b8-8ebd-9514ac8a6db0.gif)

- 컴포넌트가 리렌더링이 되면서 페이지가 이동 한 걸 확인 할 수 있습니다.
- 위의 아이콘도 깜빡이지 않습니다.
- 굉장히 빠른 페이지 이동을 __CSR__ 방식으로 `react-rotuer` 를 이용해서 구현 할 수 있습니다.

<br>

## 참고
- [인프런](https://www.inflearn.com/course/%ED%95%9C%EC%9E%85-%EB%A6%AC%EC%95%A1%ED%8A%B8/dashboard)

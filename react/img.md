# React - img 
App 컴포넌트에 img 삽입

<br>

## 현재 폴더 구조
![캡처](https://user-images.githubusercontent.com/87301268/163106269-9aa9ddcc-c8cf-4e19-9a63-d7b7baa62ab6.JPG)

<br>

### App.js
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

        <img src={process.env.PUBLIC_URL + `/assets/emotion1.png`} />

        <Routes>
          <Route path="/" element={<Home />} />
          <Route path="/new" element={<New />} />
          <Route path="/edit" element={<Edit />} />
          <Route path="/diary/:id" element={<Diary />} />
        </Routes>
      </div>
    </BrowserRouter>
  );
}

export default App;
```
<br>

```js
<img src={process.env.PUBLIC_URL + `/assets/emotion1.png`} />
```

- __process.env.PUBLIC_URL__ 은 어떤 위치에 있든 __/public__ 을 가리킵니다.

![캡처](https://user-images.githubusercontent.com/87301268/163106720-3771e9ff-658a-42b6-a9e3-49193a2ceadb.JPG)

<br>

## 참고
- [인프런](https://www.inflearn.com/course/%ED%95%9C%EC%9E%85-%EB%A6%AC%EC%95%A1%ED%8A%B8/dashboard)
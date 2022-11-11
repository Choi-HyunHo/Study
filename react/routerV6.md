# React - router V6

## 설치
```js
yarn add react-router-dom
```

***
<br>


## createBrowserRouter()
```js
const router = createBrowserRouter([각 경로의 배열])
```

<br>

### App.js
```js
import React from 'react';
import { createBrowserRouter, RouterProvider } from 'react-router-dom';


const router = createBrowserRouter([]);

export default function App() {
  return <RouterProvider router={router} />
}
```
- 위와 같이 상위 컴포넌트에 세팅을 해줍니다.

<br>

### 컴포넌트 경로에 추가하기
```js
import React from 'react';
import { createBrowserRouter, RouterProvider } from 'react-router-dom';


const router = createBrowserRouter([
  {
    path : '/',
    element : <p>Home</p>
  },
]);

export default function App() {
  return <RouterProvider router={router} />
}
```

- 경로가 올바르게 잡혀 있는 것을 볼 수 있습니다.

![](https://velog.velcdn.com/images/hoho_0815/post/7cba7c45-9bd7-437e-9514-cb305544e972/image.png)

- 하지만, 만약 경로가 잘못 되어있거나 없으면 ?

![](https://velog.velcdn.com/images/hoho_0815/post/17610187-a32f-4c06-b9a8-bc791546f3cc/image.png)

- 경로가 없다고 말을 해줍니다.
- 이제 우리는 , 더 나은 UX 를 위해서 에러가 났을 때 보여줄 화면을 명시 할 수 있습니다.

***
<br>

## errorElement

```js
import React from 'react';
import { createBrowserRouter, RouterProvider } from 'react-router-dom';


const router = createBrowserRouter([
  {
    path : '/',
    element : <p>Home</p>,
    errorElement : <p>Not Found</p>
  }
]);

export default function App() {
  return <RouterProvider router={router} />
}
```

![](https://velog.velcdn.com/images/hoho_0815/post/c48f7a84-76b1-41ee-bc36-96876ef072cb/image.png)

- 경로가 잘못 되었을때 사용자에게 보여줄 페이지를 설정 할 수 있습니다.

***
<br>

## outlet (아울렛) 사용하기

사이트에 navbar(네비게이션) 가 있고 해당 하는 메뉴를 눌렀을 때 navbar 는 그대로 유지하면서 해당하는 내용만 전환하고 싶을 때 사용 할 수 있습니다.
- 전체적인 레이아웃을 잡을 때 유용하게 사용 할 수 있습니다.
![](https://velog.velcdn.com/images/hoho_0815/post/fb88882b-2b69-4374-aa2a-722351818a3f/image.jpg)


<br>

### 파일 구조
#### src-pages
![](https://velog.velcdn.com/images/hoho_0815/post/c39936bd-d2dd-476f-a0b7-5e4d58e70083/image.png)

<br>

#### App.js
```js
import React from 'react';
import { createBrowserRouter, RouterProvider } from 'react-router-dom';
import Home from './pages/Home';
import NotFound from './pages/NotFound';
import Root from './pages/Root';
import Video from './pages/Video';


const router = createBrowserRouter([
  {
    path : '/',
    element : <Root/>,
    errorElement : <NotFound/>,
    children : [
      {index : true, element : <Home/>},
      {path : '/video', element : <Video/>}
    ]
  }
]);

export default function App() {
  return <RouterProvider router={router} />
}

```

- 해당 Root element 안에 있는 아울렛 안에 보여주길 원한다면 경로들을 위와같이 자식으로 만들어야 합니다. (children)
- index 속성은 기본적으로 보여줄 페이지 입니다. 
- router 부분을 설명하자면, Root 요소 안에 자식이 있는데 그 중 최상위 경로(index) 는 기본적으로 __Home__ 이고
- 경로가 __video__ 라면, video 컴포넌트를 보여줍니다.

<br>

#### Root.js
```js
import React from 'react'
import { Outlet } from 'react-router-dom'
import NavBar from '../components/NavBar'

export default function Root() {
  return (
    <div>
        <NavBar/>
        <Outlet/>
    </div>
  )
}
```

<br>

#### NavBar.js
```js
import React from 'react'
import { Link } from 'react-router-dom'

export default function NavBar() {
  return (
    <nav>
      <Link to="/">Home</Link>
      <Link to="/video">Video</Link>
    </nav>
  )
}
```

- Link 는 예전 포스팅에서 언급한 적이 있으므로, 리액트에서 사용하는 CSR 를 위한 a 태그라고 보시면 됩니다.
![](https://velog.velcdn.com/images/hoho_0815/post/3b8432e7-9bf4-4e89-adf7-1d9d5f2be37d/image.gif)


***
<br>

## url 에 param 전달하기
```js
import React from 'react';
import { createBrowserRouter, RouterProvider } from 'react-router-dom';
import Home from './pages/Home';
import NotFound from './pages/NotFound';
import Root from './pages/Root';
import Video from './pages/Video';
import VideoDetail from './pages/VideoDetail';


const router = createBrowserRouter([
  {
    path : '/',
    element : <Root/>,
    errorElement : <NotFound/>,
    children : [
      {index : true, element : <Home/>},
      {path : '/video', element : <Video/>},
      {path : '/video/:id', element : <VideoDetail/>}
    ]
  }
]);

export default function App() {
  return <RouterProvider router={router} />
}
```

- id 에 값에 따라 다른 상세 페이지를 만들 수 있습니다. ( 파라미터에 따른 페이지 )

<br>

### Video.js
```js
import React from 'react'
import { useState } from 'react'
import { useNavigate } from 'react-router-dom';

export default function VideoDetail() {
  // navigate
  const navigate = useNavigate();

  const [text, setText] = useState('');
  const handleChange = (e) => {
    setText(e.target.value);
  }

  const handleSubmit = (e) => {
    e.preventDefault();
    setText('');
    navigate(`/video/${text}`);
  }
  
    return (
    <div>
      Video
      <form onSubmit={handleSubmit}>
        <input type='text' placeholder="video id : " value={text} onChange={handleChange}/>
      </form>
    </div>
  )
}

```
- input 에 입력한 id 로 페이지를 업데이트 하는 코드 입니다.

<br>

## useNavigate
- 리액트 라우터에서 특정한 경로로 이동 할 수 있는 방법은 두 가지가 있는데
- 그 중 하나는 Link 태그를 이용해서 사용자가 상호작용 했을 때 해당하는 Url 로 이동하는 것
- 그리고, 코드 상에서 동적으로 이동하고 싶다면 __navigate__ 를 사용 합니다.

![](https://velog.velcdn.com/images/hoho_0815/post/6dc3310e-3d20-4b7f-af83-1b8dcf3aef65/image.gif)


<br>

## useParams
- 전달받은 데이터를 받아서 보여주기

### VideoDetail.js
```js
import React from 'react'
import { useParams } from 'react-router-dom'


export default function VideoDetail() {
  const {id} = useParams();

    return (
    <div>
      VideoDetail {id}
    </div>
  )
}
```

- { id } 이 부분은 라우터를 만들 때  {path : '/video/:id', element : <VideoDetail/>} 여기에 id 라고 명시를 했습니다.
- 명시한 경로의 id 에 해당 url 의 값이 들어오게 되는 것 입니다.

![](https://velog.velcdn.com/images/hoho_0815/post/37c01e24-93b2-42d6-b586-61be437799bd/image.gif)

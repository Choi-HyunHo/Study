# React Query - 만들기

## 코드 구조
### App.js
```js
import { Posts } from "./Posts";
import "./App.css";

function App() {
  return (
    // provide React Query client to App
    <div className="App">
      <h1>Blog Posts</h1>
      <Posts />
    </div>
  );
}

export default App;
```

<br>

### Posts.js
```js
import { useState } from "react";

import { PostDetail } from "./PostDetail";
const maxPostPage = 10;

async function fetchPosts() {
  const response = await fetch(
    "https://jsonplaceholder.typicode.com/posts?_limit=10&_page=0"
  );
  return response.json();
}

export function Posts() {
  const [currentPage, setCurrentPage] = useState(0);
  const [selectedPost, setSelectedPost] = useState(null);

  // replace with useQuery
  const data = [];

  return (
    <>
      <ul>
        {data.map((post) => (
          <li
            key={post.id}
            className="post-title"
            onClick={() => setSelectedPost(post)}
          >
            {post.title}
          </li>
        ))}
      </ul>
      <div className="pages">
        <button disabled onClick={() => {}}>
          Previous page
        </button>
        <span>Page {currentPage + 1}</span>
        <button disabled onClick={() => {}}>
          Next page
        </button>
      </div>
      <hr />
      {selectedPost && <PostDetail post={selectedPost} />}
    </>
  );
}
```

<br>

### 중점으로 봐야할 부분
- useQuery 를 사용할 때, 위에 있는 __fetchPosts__ 함수를 사용해서 JSONPlaceholder 서버에서 게시물 데이터를 가져올 것 입니다.
- Posts 데이터에 대한 useQuery 에서 __가장 관심 있게 봐야할 부분은__ 아래 게시물 데이터를 매핑하는 부분 입니다.
```js
  // replace with useQuery
  const data = [];

    <ul>
        {data.map((post) => (
          <li
            key={post.id}
            className="post-title"
            onClick={() => setSelectedPost(post)}
          >
            {post.title}
          </li>
        ))}
      </ul>
```
- 지금 데이터는 빈 배열(Array)로 되어 있지만, 이것을 __useQuery__ 로 대체할 것 입니다.
- Posts 컴포넌트에는 state 와 button 등도 있지만 이런 부분은 각 기능을 구현할 때 살펴보도록 하겠습니다.
- 가장 먼저 App.js 에서 React Query 용 쿼리 클라이언트를 만들고 모든 자녀 컴포넌트가 클라이언트를 사용할 수 있게 하는, __QueryProvider__ 를 추가하겠습니다.


***
<br>

## QueryProvider
```js
import { QueryClient, QueryClientProvider } from 'react-query';

const queryClient = new QueryClient(); // 클라이언트가 있기 때문에 provider 를 추가 할 수 있습니다.
```
- 위와 같이 설정을 하면 provider(공급자) 가 클라이언트를 소품으로 사용하게 되면서
- 클라이언트가 가지고 있는 캐시와 모든 기본 옵션을 공급자의 자녀 컴포넌트도 사용할 수 있게 됩니다.

<br>

```js
import { Posts } from "./Posts";
import "./App.css";

import { QueryClient, QueryClientProvider } from 'react-query';

const queryClient = new QueryClient(); // 클라이언트가 있기 때문에 provider 를 추가 할 수 있습니다.

function App() {
  return (
    // provide React Query client to App
    <QueryClientProvider client={queryClient}>
      <div className="App">
        <h1>Blog Posts</h1>
        <Posts />
      </div>
    </QueryClientProvider>
  );
}

export default App;
```
- 이제 QueryClientProvider 안에 있는 모든 컴포넌트들은 React Query 훅을 사용 할 수 있습니다.

***
<br>

## 공급자 안 컴포넌트에서 useQuery 가져오기
```js
import { useQuery } from 'react-query';
```
- useQuery 는 서버에서 데이터를 가져올 때 사용할 훅 입니다.

***
<br>


## useQuery 사용법
- 이제 아래 코드를 useQuery 로 바꿔보겠습니다.
```js
const data = [];
```

### useQuery 적용
```js
const {data} = useQuery('query key', 'query function');
```
- useQuery 는 다양한 속성을 가진 개체를 반환 합니다. 
- 그 중, 지금은 데이터를 구조 분해 하겠습니다.
- 구조 분해 할당 구문은 배열이나 객체의 속성을 해체하여 그 값을 개별 변수에 담을 수 있게 하는 JavaScript 표현식입니다.
- useQuery 는 몇 가지 인수를 사용하는데 첫 번째는 __쿼리 키__ 입니다. 바로 쿼리 이름을 말하는 것 입니다. 
- 그 다음은 __쿼리 함수__ 를 사용 합니다. 쿼리에 대한 데이터를 가져오는 방법을 말하는 것 입니다.

<br>

```js
const {data} = useQuery('posts', fetchPosts);
```

- 두 번째 쿼리 함수는 __데이터를 가져오는 비동기 함수__ 를 사용해야 합니다. (axios 등..)
- fetchPosts 는 포스팅 처음에 있던 비동기 함수 코드 입니다. 
- 안에 들어있는 데이터의 모양은 아래와 같습니다.
![](https://velog.velcdn.com/images/hoho_0815/post/c957a8ba-b37e-4f13-a5f0-a11ccc240aec/image.png)

<br>

```js
<ul>
        {data.map((post) => (
          <li
            key={post.id}
            className="post-title"
            onClick={() => setSelectedPost(post)}
          >
            {post.title}
          </li>
        ))}
</ul>
```
- 이제 맵핑한 __data__ 는 useQuery 를 통해(쿼리함수) 가져온 fetchPosts 에서 반환된 데이터가 됩니다.
- 위의 사진에 있는 배열의 각 항목에 대한 목록을 만들고, 게시물의 제목을 표시할 것 입니다. 
- 하지만 npm start 를 통해 실행하면 아래와 같은 에러가 발생 합니다.
![](https://velog.velcdn.com/images/hoho_0815/post/7567d3a8-1605-4d27-831a-c9c02c67e5c6/image.png)
- 이유는, 매핑을 시도했지만 맵은 __배열 전용__ 이기 떄문에, 현재 데이터가 정의되지 않았다고 나오는 것 입니다.
- 바로, fetchPosts 가 비동기식이기 때문에 fetchPosts 가 해결될 때까지 __이 데이터는 useQuery 에서 정의되지 않을 것 입니다.__
- useQuery 는 fetchPosts 가 데이터와 반환되지 않은 경우 데이터에 할당할 항목을 알 수 없기 때문입니다.
- 나중에 좀 더 좋은 방법으로 해결할 예정이지만, 우선 조건문을 통해 해결해 보도록 하겠습니다.

***
<br>

## 비동기 문제 해결
```js
 // replace with useQuery
  const {data} = useQuery('posts', fetchPosts);

  // 데이터가 정의되지 않은 경우
  // fetchPosts 가 해결될 때까지 데이터는 거짓이 됩니다.
  if(!data){
    return <div></div>;
  }

  // 하지만, fetchPosts 가 해결되면, 데이터에 배열이 포함되고 
  // 컴포넌트가 다시 렌더링 되어 매핑 할 수 있게 됩니다. 아래 return 문

  return (
    <>
      <ul>
        {data.map((post) => (
          <li
            key={post.id}
            className="post-title"
            onClick={() => setSelectedPost(post)}
          >
            {post.title}
          </li>
        ))}
      </ul>
      <div className="pages">
        <button disabled onClick={() => {}}>
          Previous page
        </button>
        <span>Page {currentPage + 1}</span>
        <button disabled onClick={() => {}}>
          Next page
        </button>
      </div>
      <hr />
      {selectedPost && <PostDetail post={selectedPost} />}
    </>
  );
```

<br>

![](https://velog.velcdn.com/images/hoho_0815/post/b796a8e6-09d8-4128-9a79-5a277e145f54/image.png)

- 블로그 게시물이 보이게 됩니다. 
- 버튼을 누르면 게시물도 볼 수 있습니다.
- 다음 포스팅은 __uesQuery 의 로딩 및 오류 상태를 살펴보겠습니다.__

***
<br>

## 참고
- https://www.udemy.com/course/react-query-react/
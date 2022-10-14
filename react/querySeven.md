# React Query - 페이지 매김(Pagination)
현재 페이지(current Page) 상태를 통해, 현재 페이지를 파악하는 페이지 매김 방법

- 댓글에서 작업했던 것처럼 __페이지마다 다른 쿼리 키__ 가 필요 합니다.
- 따라서, 쿼리 키를 __배열__ 로 업데이트해서 가져오는 페이지 번호를 포함 하도록 하겠습니다.
- 사용자가 다음, 또는 이전 페이지로 가는 버튼을 누르면 currentPage 상태를 업데이트 합니다.
    - 그럼 React Query 가 바뀐 쿼리 키를 감지하고 새로운 쿼리를 실행해서 새 페이지가 표시됩니다.

***
<br>

## 시작
### Posts.js
```js
import React, { useState } from "react";
import { useQuery } from "react-query";

import { PostDetail } from "./PostDetail";
const maxPostPage = 10;

async function fetchPosts() {
  const response = await fetch(
    "https://jsonplaceholder.typicode.com/posts?_limit=10&_page=0"
  );
  return response.json();
}

export function Posts() {
  const [currentPage, setCurrentPage] = useState(1);
  const [selectedPost, setSelectedPost] = useState(null);

  // replace with useQuery
  const {data, isLoading, isError, error} = useQuery('posts', fetchPosts, {staleTime : 2000});

  // 만약 로딩 중이라면
  if(isLoading){
    return <h3>Loading...</h3>;
  }

  if(isError){
    return (
      <React.Fragment>
        <h3>Sorry, no Data</h3>
        <p>{error.toString()}</p>
      </React.Fragment>
    )
  }

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

***
<br>

## 쿼리 키 추가
```js
const [currentPage, setCurrentPage] = useState(1);

// 쿼리 키에 currentPage 를 포함합니다.
const {data, isLoading, isError, error} = useQuery(['posts', currentPage] fetchPosts, {staleTime : 2000});
```

- 위와 같이 쿼리 키에 currentPage 를 포함하게 되면 React Query 가 바뀐 쿼리 키를 감지해서 새 쿼리 키에 대한 데이터를 업데이트 합니다.

<br>

## 쿼리 함수도 업데이트

```js
async function fetchPosts() {
  const response = await fetch(
    "https://jsonplaceholder.typicode.com/posts?_limit=10&_page=0"
  );
  return response.json();
}
```

- 현재 쿼리 함수는 페이지 0 을 가져오도록 하드 코딩되어 있어서 currentPage 의 state가 1 , 
- 즉, 페이지가 1부터 시작해도 page=0 을 실행하면 페이지 0이 나올 것 입니다.
- 어떤 페이지 번호를 입력하든 간에 가져올 수 있도록 만들려면 쿼리 함수에 __pageNum__ 를 인자로 받고 
- page=0 을, 인수로 사용되는 어떤 페이지 번호든 가져올 수 있도록 아래 처럼 바꿉니다.

```js
async function fetchPosts(pageNum) {
  const response = await fetch(
    `https://jsonplaceholder.typicode.com/posts?_limit=10&_page=${pageNum}`
  );
  return response.json();
}
```
- 또한 게시물을 가져올 뿐만 아니라 댓글에서 했던 것 처럼 currentPage 가 무엇이든 간에 fetchPosts 함수를 실행해야 합니다.
```js
const {data, isLoading, isError, error} = useQuery(['posts', currentPage], () => fetchPosts(currentPage), {staleTime : 2000});
```
- ['posts', currentPage] 이 배열이 바뀌면 함수도 바뀌기 때문에(fetchPosts) 데이터가 바뀔 수 밖에 없습니다.
- 따라서 쿼리 키가 바뀌면 useQuery에 새로운 쿼리를 알려서 데이터를 다시 가져오도록 만든 것 입니다.

***
<br>

## 버튼 활성화
### 이전 버튼
버튼은 기본적으로 disabled 되어 있는데 만약, currentPage 가 __1과 같거나 1보다 작으면__ 이전 버튼을 비활성화 해야 합니다.

```js
<button disabled={currentPage <= 1} 
    onClick={() => {setCurrentPage((currentPage) => currentPage - 1 }}>
        Previous page
</button>
```

- 즉, 페이지가 1에 있거나, 실수로 페이지 1전으로 가게 되면 이전 버튼을 활성화하지 않아야 합니다.
- onClick에는 currentPage 를 setCurrentPage에 currentPage - 1 값을 반환하는 함수를 줍니다. 



<br>

### 다음 버튼
앞서 상단에 maxPostPage 를 10으로 설정 했습니다.
```js
const maxPostPage = 10;

// 한계가 10 입니다.
"https://jsonplaceholder.typicode.com/posts?_limit=10&_page=${pageNum}"
```

- 또한 API 가 100개의 게시물을 반환한다고 해서 최대 페이지 수를 10으로 설정 했습니다.

<br>

```js
const {data, isLoading, isError, error} = useQuery('posts', fetchPosts, {staleTime : 2000});
```
만약 data 가 다음 페이지 유무에 대한 프로퍼티를 가지고 있다면, 버튼을 활성화할지 판단하는 지표로 사용 할 수 있습니다.

<br>

버튼이 disabled 인 경우는 currentPage 가 maxPostPage 와 같거나 그보다 클 경우가 됩니다.
```js
<button disabled={currentPage >= maxPostPage} 
    onClick={() => {setCurrentPage((currentPage) => currentPage + 1 }}>
    Next page
</button>
```
- onClick 은 전과 비슷한데 1을 빼는 대신 1을 더합니다.

***
<br>

## 전체 코드
```js
import React, { useState } from "react";
import { useQuery } from "react-query";

import { PostDetail } from "./PostDetail";
const maxPostPage = 10;

async function fetchPosts(pageNum) {
  const response = await fetch(
    `https://jsonplaceholder.typicode.com/posts?_limit=10&_page=${pageNum}`
  );
  return response.json();
}

export function Posts() {
  const [currentPage, setCurrentPage] = useState(1);
  const [selectedPost, setSelectedPost] = useState(null);

  // replace with useQuery
  const {data, isLoading, isError, error} = useQuery(['posts', currentPage], () => fetchPosts(currentPage), {staleTime : 2000});

  // 만약 로딩 중이라면
  if(isLoading){
    return <h3>Loading...</h3>;
  }

  if(isError){
    return (
      <React.Fragment>
        <h3>Sorry, no Data</h3>
        <p>{error.toString()}</p>
      </React.Fragment>
    )
  }

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
        <button disabled={currentPage <= 1} 
        onClick={() => {setCurrentPage((currentPage) => currentPage - 1);}}>
          Previous page
        </button>
        <span>Page {currentPage}</span>
        <button disabled={currentPage >= maxPostPage} 
        onClick={() => {setCurrentPage((currentPage) => currentPage + 1);}}>
          Next page
        </button>
      </div>
      <hr />
      {selectedPost && <PostDetail post={selectedPost} />}
    </>
  );
}
```

***
<br>

## 결과
![](https://velog.velcdn.com/images/hoho_0815/post/142714c7-79c0-4a9c-ac60-ce0ee4155e47/image.gif)

- 페이지 전환이 잘 되는 볼 수 있습니다.
- 하지만 다음 페이지로 넘어가는 도중에 로딩이 나와서 사용자 경험에 방해를 줍니다.
- 다음 포스팅에서 다음 페이지 결과를 __프리패칭(Prefetching)__ 해서 바로 보일 수 있도록 하겠습니다.

***
<br>

## 참고
- https://www.udemy.com/course/react-query-react/
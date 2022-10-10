# React Query - 쿼리 키
## Posts.js
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
  const [currentPage, setCurrentPage] = useState(0);
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

<br>

## PostDetail.js
```js
import { useQuery } from 'react-query';

async function fetchComments(postId) {
  const response = await fetch(
    `https://jsonplaceholder.typicode.com/comments?postId=${postId}`
  );
  return response.json();
}

async function deletePost(postId) {
  const response = await fetch(
    `https://jsonplaceholder.typicode.com/postId/${postId}`,
    { method: "DELETE" }
  );
  return response.json();
}

async function updatePost(postId) {
  const response = await fetch(
    `https://jsonplaceholder.typicode.com/postId/${postId}`,
    { method: "PATCH", data: { title: "REACT QUERY FOREVER!!!!" } }
  );
  return response.json();
}

export function PostDetail({ post }) {

  const {data, isLoading, isError} = useQuery('comments', ()=>fetchComments(post.id));

  if(isLoading){
    return <h3>Loading ...</h3>
  }

  if(isError){
    return <h3>Sorry, error</h3>
  }

  return (
    <>
      <h3 style={{ color: "blue" }}>{post.title}</h3>
      <button>Delete</button> <button>Update title</button>
      <p>{post.body}</p>
      <h4>Comments</h4>
      {data.map((comment) => (
        <li key={comment.id}>
          {comment.email}: {comment.body}
        </li>
      ))}
    </>
  );
}
```
- posts 에서 해당 게시글의 글을 누르면 state 의 값으로 post 의 data 가 들어가고
- 해당 state 는 props 를 통해서 PostDetail 컴포넌트로 전달이 됩니다.
- 그리고 PostDetail 컴포넌트에서는 useQuery 를 사용해 __fetchComments__ 쿼리 함수가 props 로 전달받은 속성 중 id 값을 인수로 받아 비동기 함수를 실행 합니다.
- 결론적으로 해당 post.id 에 해당하는 comment 가 화면에 보여져야 합니다.
- 하지만, 아래를 보면 모든 글이 같은 comment 를 화면에 보여줍니다.

<br>

![](https://velog.velcdn.com/images/hoho_0815/post/a38a3f05-a5c4-4d1c-a989-847c281bad07/image.gif)

***
<br>

## 모든 같은 comment 가 보일까?
![](https://velog.velcdn.com/images/hoho_0815/post/584215f6-80d8-47f6-9439-94f333c2a0a4/image.gif)


위에 보이는 comments 쿼리 키는 오후 9시 59분에 마지막으로 업데이트가 되었습니다.<br>
다른 게시물을 눌러도 변화하지 않으며, 데이터는 만료(stale) 되었고, 리패칭 또한 일어나지 않습니다.
<br>

궁극적인 이유는, 모든 쿼리가 comments 쿼리 키를 동일하게 사용하고 있기 때문 입니다.
- 이렇게 comments 같이 알려진 쿼리 키가 있을 때는 어떠한 변화가 있어야만 데이터를 다시 가져오게 됩니다.
- 변화의 예시는
    - 컴포넌트가 다시 마운트하거나, 윈도우를 다시 포커스 할 때
    - useQuery에서 반환되어 수동으로 리패칭을 실행할 때
    - 지정된 간격으로 리패칭을 자동 실행할 때
    - 변이(Mutation) 을 생성한 뒤 쿼리를 무효화 할 때
- __클라이언트의 데이터가 서버의 데이터와 불일치할 때 리패칭이 발생 합니다.__

<br>

현재 게시물의 제목을 클릭할 때는 이런 변화가 일어나지 않습니다.
- 따라서 데이터가 만료되어도 새 데이터를 가져오지 않습니다.

***
<br>

## 해결 방법 ?
1. 새 블로그 게시물 제목을 클릭 할 때마다 데이터를 무효화시켜서 데이터를 다시 가져오게 만들 수 있는데
- 간단한 방법도 아니고, 원하는 방법도 아닙니다.

2. 데이터를 제거해서는 안됩니다.
- 2번째 게시물에 대한 쿼리를 만들 때, 캐시에서 게시물 1 에 대한 댓글을 제거하지 않아야 합니다.
- 같은 쿼리를 실행하는게 아니며 같은 캐시 공간을 차지하지 않습니다.
- 엄밀히 말하면 1번째 게시물을 눌렀을 때, 게시물 1에 대한 캐시 데이터를 활용하는 것이 좋습니다.
- 2번째 게시물에 대한 데이터로 덮어쓰는 건 좋지 않습니다.

3. 쿼리는 게시물 id 를 포함하기 때문에, 쿼리별로 캐시를 남길 수 있으며 comment 쿼리에 대한 캐시를 공유하지 않아도 됩니다.
4. 각 게시물에 대한 쿼리에 라벨을 설정하면 됩니다.

***
<br>

## Array as Query Key
쿼리 키에 문자열(String) 대신 배열(Array) 을 전달하면 가능 합니다.
```js
['comment', post.id]
```
- 위와 같은 경우 배열의 첫 번째 요소로 문자열 "comment" 를 가지고, 두 번째 요소로 post.id 를 가집니다.
- __쿼리 키를 쿼리에 대한 의존성 배열로 취급하게 됩니다.__
- 쿼리 키가 변경되면, 즉 post.id 가 업데이트되면 React Query가 새 쿼리를 생성해서 staleTime 과 cacheTime 을 가지게 되고 의존성 배열이 다르다면 완전히 다른 것으로 간주됩니다.
- 따라서 데이터를 가져올 때 사용하는 쿼리 함수에 있는 값이 __쿼리 키에 포함되어야 합니다.__
  - 그렇게 되면 모든 comment 쿼리가 같은 쿼리로 간주되는 상황을 막고 각 다른 쿼리로 다뤄질 것 입니다.

***
<br>

## 적용
### PostDetail.js
```js
// 이전 식을
const {data, isLoading, isError} = useQuery('comments', ()=>fetchComments(post.id));

// 아래 처럼 변경
const {data, isLoading, isError} = useQuery(['comments', post.id], ()=>fetchComments(post.id));
```
- fetchComments 에 전달한 것과 동일한 post.id 입니다.
- post.id 가 의존성 배열로 작용하며 문자열 'comments' 에 식별자가 추가된 셈 입니다.

<br>

![](https://velog.velcdn.com/images/hoho_0815/post/21d0d5e5-7c61-4d7e-a1bc-2fe5fa8a35cb/image.gif)

- 개발자 도구를 보면 해당하는 댓글의 id 가 다 다른것을 볼 수 있습니다.
- 이때 다른 게시물을 클릭하자마자 그 전의 쿼리가 비활성화 되는 것을 볼 수 있는데 아직 __캐시에 있는 상태__ 입니다.
- 가비지 컬렉터로 수집되기 전까지 캐시에 남아있을 것 입니다.
- 또한 게시물의 댓글이 변하는 걸 볼 수 있습니다.

***
<br>

## 참고
- https://www.udemy.com/course/react-query-react/
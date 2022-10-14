# React Query - 데이터 프리패칭(Pre-fetching)
페이지 매김(Pagination)을 구현한 지난 포스팅에서는 사용자 경험이 좋지 않았습니다.
- 페이지가 캐시에 없기 때문입니다.
- Next Page 버튼을 누를 때마다 페이지가 로딩되길 기다려야 했습니다.

이번에는 데이터를 __미리 가져와 캐시에 넣어서__ 사용자 경험을 개선 하겠습니다.

***
<br>

## Prefetching
프리패칭은 __데이터를 캐시에 추가하며__ , 구성할 수 있긴 하지만 기본값으로 만료(stale) 상태 입니다.
- 즉, 데이터를 사용하고자 할 때 만료 상태에서 데이터를 다시 가져옵니다.
- 데이터를 가져오는 중에는 캐시에 있는 데이터를 이용해 화면에 나타냅니다.
    - 물론 __캐시가 만료되지 않았다는 가정하에 일 입니다.__
- 이렇게 추후 사용자가 사용할 법한 __모든 데이터에 프리패칭을 사용 합니다.__
    - 페이지 매김, 다수의 사용자가 웹에서 통계적으로 특정 탭을 누를 확률이 높은 곳이라든지 
    - 해당 데이터를 미리 가져오는 것이 좋습니다.

<br>

참고 자료 : [prefetching](https://tanstack.com/query/v4/docs/reference/QueryClient?from=reactQueryV3&original=https://react-query-v3.tanstack.com/reference/QueryClient#queryclientprefetchiquery)

***
<br>

## useQueryClient
prefetch 쿼리는 queryClient 의 메서드
```js
import { useQuery, useQueryClient } from "react-query";
```

<br>

### 컴포넌트로 가져올 때
```js
const queryClient = useQueryClient();
```
실행할 때 유의해야 합니다.
- 다음 페이지로 onClick 시 실행하는 건 좋은 생각이 아닙니다.
- 상태 업데이트가 비동기식으로 일어나기 때문에 __이미 업데이트가 진행됐는지 알 방법이 없습니다.__
- 현재 페이지가 무엇인지 알 수 없는 확실한 방법이 없습니다.
- 대신 useEffect 를 사용해서 현재 페이지에 생기는 변경 사항을 활용 하겠습니다.

***
<br>

## queryClient.prefetchQuery()
```js
useEffect(()=>{
    const nextPage = currentPage + 1;
    queryClient.prefetchQuery()
},[currentPage])
```
현재 페이지가 변경될 때마다 안의 함수를 실행할 것 입니다. 
- 다음 페이지가 무엇이든 데이터를 미리 가져와야 합니다.

<br>

### prefetchQuery 인수는 useQuery 의 인수와 아주 흡사 합니다.

```js 
queryClient.prefetchQuery('쿼리 키', '쿼리 함수');
```
- useQuery에 사용한 쿼리 키와 똑같은 모습을 하고 있습니다.
- React Query가 캐시에 이미 데이터가 있는지 여기서 확인 합니다.

```js
useEffect(()=>{
    const nextPage = currentPage + 1;
    queryClient.prefetchQuery(['posts', nextPage], () => fetchPosts(nextPage))
},[currentPage, queryClient]);
```

***
<br>

## 예외 처리
```js
useEffect(()=>{
    if(currentPage < maxPostPage){
      const nextPage = currentPage + 1;
      queryClient.prefetchQuery(['posts', nextPage], () => fetchPosts(nextPage));
    }
},[currentPage, queryClient])
```
- 9 페이지 이전이라면 프리패칭이 이루어지지만, 10페이지에 있다면 미리 가져올 데이터가 없습니다.

***
<br>

## keepPreviousData 
쿼리 키가 바뀔 때도 지난 데이터 유지
- 혹여나 이전 페이지로 돌아갔을 때 캐시에 해당 데이터가 있도록 만들고 싶습니다.

```js
const {data, isLoading, isError, error} = useQuery(['posts', currentPage], 
() => fetchPosts(currentPage), {staleTime : 2000, keepPreviousData : true});
```

***
<br>

## 전체 코드
```js
import React, { useEffect, useState } from "react";
import { QueryClient, useQuery, useQueryClient } from "react-query";

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

  const queryClient = useQueryClient();

  useEffect(()=>{
    if(currentPage < maxPostPage){
      const nextPage = currentPage + 1;
      queryClient.prefetchQuery(['posts', nextPage], () => fetchPosts(nextPage));
    }
  },[currentPage, queryClient])

  // replace with useQuery
  const {data, isLoading, isError, error} = useQuery(['posts', currentPage], 
  () => fetchPosts(currentPage), {staleTime : 2000, keepPreviousData : true});

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
        <button disabled={currentPage <= 1} onClick={() => {setCurrentPage((currentPage) => currentPage - 1);}}>
          Previous page
        </button>
        <span>Page {currentPage}</span>
        <button disabled={currentPage >= maxPostPage} onClick={() => {setCurrentPage((currentPage) => currentPage + 1);}}>
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

![](https://velog.velcdn.com/images/hoho_0815/post/d77c8742-5ddb-403c-bd20-297ff8bce72b/image.gif)

개발자 도구를 보면 쿼리가 쌓이고 있습니다.
- 유일하게 활성 상태인 쿼리는 현재 페이지에 대한 쿼리이고 나머지 쿼리도 남아 있습니다.
- 한편 아직 9페이지가 아닌데 9페이지를 미리 가져온 것을 볼 수 있습니다.
- 9페이지로 가면 10페이지를 미리 가져옵니다.

> 이처럼 데이터를 캐시에 추가하기 때문에 다음 페이지를 넘어가면 바로 볼 수 있습니다.

***
<br>

## 참고
- https://www.udemy.com/course/react-query-react/
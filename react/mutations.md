# React Query - 변이(Mutation)
변이는 서버에 데이터를 업데이트하도록 서버에 네트워크 호출을 실시 합니다. <br>
ex) 블로그 포스트를 추가하거나, 삭제 또는 변경 등

***
<br>

## useMutation
### 특징
- 일부 예외를 제외하고 useQuery 와 상당히 유사 합니다.
- __mutate 함수__ 를 반환하는데 이 함수는, __사용자의 변경 사항을 토대로 서버를 호출할 때__ 사용 합니다.
- 데이터를 저장하지 않으므로 __쿼리 키는 필요하지 않습니다.__
- __isLoading 은 존재__ 하지만, isFetching 은 없습니다.
- 변이에 관련된 캐시는 존재하지 않고, 재시도 또한 기본값 입니다.

공식 문서 : [Mutations](https://tanstack.com/query/v4/docs/guides/mutations?from=reactQueryV3&original=https://react-query-v3.tanstack.com/guides/mutations)

***
<br>

## 적용
더미 API 를 사용하므로 실제로 변이를 적용 할 수는 없지만, 변이가 진행되는 동안 어떤 일이 일어나는지 대해 <br>
사용자에게 이를 알려주는 방법에 대해 살펴봅니다.

<br>

### 진행할 전체 코드
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
  console.log(post)
  // replace with useQuery
  // fetchComments 함수  - 익명 함수 사용하기 (댓글은 모두 동일한 결과가 나올 것 이다) 아직 안배웠기 때문
  // postId 는 post 속성에서 얻을 수 있다.
  const {data, isLoading, isError} = useQuery(['comments', post.id], ()=>fetchComments(post.id));

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

<br>

## useMutation 사용
```js
import { useQuery, useMutation } from 'react-query';

// 앞에 이름은 예시
const deleteMutation = useMutation((인수) => 비동기함수(인수))

// 비교를 위한 useQuery
const {data, isLoading} = useQuery(['쿼리 키', ''], () => function());
```

useQuery 랑 다른 차이점은

- useQuery 에 인수로서 바로 전달하는 쿼리 함수와는 달리 ( () => function() 이부분 )
- 인수로 전달하는 변이 함수의 경우 그 자체도 인수로 받을 수 있습니다.
- useMutation 에서 객체는 변이 함수를 반환하게 됩니다. (deleteMutation) 

```js
async function deletePost(postId) {
  const response = await fetch(
    `https://jsonplaceholder.typicode.com/postId/${postId}`,
    { method: "DELETE" }
  );
  return response.json();
}

const deleteMuation = useMutation(() => deletePost(postId));
```

위의 deleteMutation 은 누군가 삭제 버튼을 클릭할 때 변이 함수를 실행하려는 목적 입니다.
- 사용할 때 속성 함수인 __mutate__ 를 실행하게 됩니다.
- props 에서 받은 postId 가 무엇이든 상관없이 실행하게 됩니다.
- 따라서 변이 함수를 호출할 때면 인수가 mutate() 안에 할당 됩니다.
```js
return (
    <>
      <h3 style={{ color: "blue" }}>{post.title}</h3>
      <button onClick={() => deleteMutation.mutate(post.id)}>Delete</button>
      <button>Update title</button>
      <p>{post.body}</p>
      <h4>Comments</h4>
      {data.map((comment) => (
        <li key={comment.id}>
          {comment.email}: {comment.body}
        </li>
      ))}
    </>
  );
```

***
<br>

### useMutation 또한 여러가지 반환 객체가 있습니다
isError, isLoading, isSuccess 사용
- isSuccess 을 사용하여 (현재 API 방식 때문에 사용) 페이지에서 데이터를 변경함을 통해 성공적으로 되었는지 알 수 없습니다.

<br>

```js
return (
    <>
      <h3 style={{ color: "blue" }}>{post.title}</h3>
      <button onClick={() => deleteMuation.mutate(post.id)}>Delete</button>
      <button>Update title</button>
      {deleteMuation.isError && <p style={{color : 'red'}}>Error post</p>}
      {deleteMuation.isLoading && <p style={{color : 'blue'}}>Deleting the post</p>}
      {deleteMuation.isSuccess && <p style={{color : 'green'}}>Success</p>}
      <p>{post.body}</p>
      <h4>Comments</h4>
      {data.map((comment) => (
        <li key={comment.id}>
          {comment.email}: {comment.body}
        </li>
      ))}
    </p>
  );
```

***
<br>

## 전체 코드
```js
import { useQuery, useMutation } from 'react-query';

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
  const {data, isLoading, isError} = useQuery(['comments', post.id], ()=>fetchComments(post.id));

  const deleteMuation = useMutation(() => deletePost(post.id));

  if(isLoading){
    return <h3>Loading ...</h3>
  }

  if(isError){
    return <h3>Sorry, error</h3>
  }

  return (
    <>
      <h3 style={{ color: "blue" }}>{post.title}</h3>
      <button onClick={() => deleteMuation.mutate(post.id)}>Delete</button>
      <button>Update title</button>
      {deleteMuation.isError && <p style={{color : 'red'}}>Error post</p>}
      {deleteMuation.isLoading && <p style={{color : 'blue'}}>Deleting the post</p>}
      {deleteMuation.isSuccess && <p style={{color : 'green'}}>Success</p>}
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

***
<br>

## 결과

![](https://velog.velcdn.com/images/hoho_0815/post/e1918742-c5d7-47c7-bde7-e5f7ed9f00ac/image.gif)

- 여러가지 반환 객체를 통해 쿼리에서 진행한 것과 유사한 방식으로 주기를 처리 할 수 있습니다.

***
<br>

## 참고
- https://www.udemy.com/course/react-query-react/

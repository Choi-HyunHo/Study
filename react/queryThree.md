# React Query - 로딩 및 오류 상태
[useQuery 에서 반환되는 객체의 종류](https://tanstack.com/query/v4/docs/reference/useQuery?from=reactQueryV3&original=https://react-query-v3.tanstack.com/reference/useQuery)

***
<br>

## 데이터가 아직 준비되지 않았을 때
데이터가 정의되지 않으면 오류가 나오게 하지 않고, 적절한 조치를 할 수 있습니다. <br>
그 중 살펴볼 것은 __isLoading__ 과 __isError__ 입니다. 

- 이 두가지는 데이터가 로딩 중인지 여부,
- 데이터를 가져올 때 오류가 있는지 여부를 알려주는 boolean 입니다.

***
<br>

## 저번에 만든 코드
```js
const {data, isLoading, isError} = useQuery('posts', fetchPosts);
```

- 동일하게 반환 객체에서 isLoading 과 isError 를 추가 합니다.

***
<br>

## isLoadding
```js
 // replace with useQuery
  const {data, isLoading, isError} = useQuery('posts', fetchPosts);

  // 만약 로딩 중이라면
  if(isLoading){
    return <h3>Loading...</h3>;
  }
```

![](https://velog.velcdn.com/images/hoho_0815/post/e023b537-d199-426c-9268-bcc39703f06d/image.gif)

- 로딩 상태에 있을 때 __isLoading__ 이 적용되는 것을 볼 수 있습니다.
- 그리고 더 이상 로딩하지 않으면 __false__ 가 되고 해당 내용을 반환하는 것을 볼 수 있습니다.

***
<br>

## isLoading 과 isFetching 의 차이점
|isFetching|isLoading|
|--|--|
|비동기 쿼리가 해결되지 않았음을 의미(axios, GraphQL)|이에 대한 하위 집합, 가져오는 상태에 있음을 의미|
||

- isLoading 은 캐시 된 데이터도 없습니다.
- 그저 데이터를 가져오는 중이고, 표시할 캐시 데이터도 없다는 것 입니다.
- 이렇게만 보면 큰 차이가 없어 보일 수 있지만 나중에 페이지 매김(Pagination) 을 진행할 때 캐시 된 데이터가 있을 때와
- 없을 때를 구분해야 한다는 사실을 알게 됩니다.

***
<br>

## isError
쿼리 함수인 fetchPosts 에서 __오류가 발생하면 데이터도 얻지 못합니다.__ <br>
데이터가 정의되지 않기 때문에 조기 반환 값이 필요 합니다.

```js
 const {data, isLoading, isError} = useQuery('posts', fetchPosts);

  // 만약 로딩 중이라면
  if(isLoading){
    return <h3>Loading...</h3>;
  }

  if(isError){
    return <h3>Sorry, no Data</h3>
  }
```

![](https://velog.velcdn.com/images/hoho_0815/post/4fe4a3fe-3afe-4559-9507-4e387c2e5447/image.gif)

- 개발자 도구로 데이터를 불러오는 모습을 보면, 로딩을 포기하기 전까지 총 세번을 시도 합니다.
- 시도 횟수는 변경 할 수 있지만, React Query 는 기본적으로 세 번 시도한 후에 해당 데이터를 가져올 수 없다고 결정 합니다.

***
<br>

## error
반환 객체에는 쿼리 함수에서 전달하는 에러도 있습니다.

```js
 const {data, isLoading, isError, error} = useQuery('posts', fetchPosts);

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
```

![](https://velog.velcdn.com/images/hoho_0815/post/5226dda4-9d60-4988-9a37-399107343864/image.png)

***
<br>

## 참고
- https://www.udemy.com/course/react-query-react/
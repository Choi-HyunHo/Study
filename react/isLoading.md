# React Query - isFetching VS. isLoading

isFetching 
- 비동기 쿼리 함수가 해결되지 않았을 때 해당 합니다. ( 아직 데이터를 가져오는 중 )

<br>

isLoading
- isFetching이 진행 중이고, 쿼리에 대해 캐시된 데이터가 없는 상태를 말합니다.
- 즉, isLoading 상태이면 isFetching 또한 진행 중인 상태 입니다. ( 동시에 true )
- __isLoading 은 캐시된 데이터가 없고, 데이터를 가져오는 상황에 해당하는 isFetching 의 부분 집합 입니다.__

***
<br>

## 예시
```js
const {data, isLoading, isError, error} = useQuery(['posts', currentPage], 
  () => fetchPosts(currentPage), {staleTime : 2000, keepPreviousData : true});

  // 만약 로딩 중이라면
  if(isLoading){
    return <h3>Loading...</h3>;
  }
```
데이터를 가져오는 중, 즉 fetchPosts 함수가 실행 중이면서 __캐시된 데이터가 존재하지 않을 때__ isLoading이 참이 됩니다.

<br>

### isFetching 으로 바꾸면
```js
useEffect(()=>{
    if(currentPage < maxPostPage){
      const nextPage = currentPage + 1;
      queryClient.prefetchQuery(['posts', nextPage], () => fetchPosts(nextPage));
    }
},[currentPage, queryClient])

const {data, isLoading, isError, error, isFetching} = useQuery(['posts', currentPage], 
  () => fetchPosts(currentPage), {staleTime : 2000, keepPreviousData : true});

  // 만약 로딩 중이라면
  if(isFetching){
    return <h3>Loading...</h3>;
  }
```
캐시된 데이터의 존재 여부와 관계없이 화면에 로딩 표시를 나타낼 것 입니다.

<br>

## 결과
![](https://velog.velcdn.com/images/hoho_0815/post/82fee323-f498-44fd-9273-fd6128272e7c/image.gif)

- 캐시에 데이터가 있는데도 화면에 Loading이 매번 나타나게 됩니다.


***
<br>

## 차이점
- isLoading은 화면에 표시할 것이 아무것도 없을 때 무언가를 나타내려면 (__즉, 캐시에 아무것도 없고 서버에서 데이터를 가져오는 중__)
- Prefetch 의 목적은 __캐시된 데이터를 표시하면서 데이터의 업데이트 여부를 조용히 서버에서 확인하는 것__ 입니다.
- 그리고 데이터가 업데이트 되면은 해당 데이터 페이지를 보여주는 것 입니다.

***
<br>

## 정리
- isLoading은 서버에 데이터 요청을 처음 할 때
- isFetching은 서버에 데이터 요청을 다시 할 때 (캐시된 데이터가 있을 때)

***
<br>

## 참고
- https://www.udemy.com/course/react-query-react/
# React Query - staleTime vs cacheTime
## Stale Data ( 만료된 데이터 )
예를 들어 오래된 식빵과 비슷하다고 할 수 있습니다.

<br>

### React Query 에서 데이터가 만료됐다는 것은 무슨 뜻인가?
데이터 리패칭(Refetching) 은 __만료된 데이터__ 에서만 실행 됩니다.
- 예를 들어, 컴포넌트가 다시 마운트되거나 윈도우가 다시 포커스 되었을 때가 있습니다.
- __staleTime__ 은 데이터를 허용하는 '최대 나이' 라고 할 수 있습니다.
- 달리 말하면, 데이터가 만료됐다고 판단하기 전까지 __허용되는 시간__ 이 staleTime 입니다.
- __'웹사이트에 표시된 데이터가 10초 까지는 그대로여도 괜찮다'__ 라고 판단하면 staleTime 을 10초로 설정 합니다.

> stale 한 데이터를 사용자에게 보여주는 것은 유의미하지 않다고 React Query는 판단하고 fresh 한 데이터를 요구하게 됩니다. <br>
__결국 서버로부터 fresh한 데이터를 전달받기 위해 refetch가 이루어집니다.__

***
<br>

## staleTime 사용
useQuery 에 세번째 옵션으로 추가 합니다.

```js
const {data} = useQuery('쿼리 키', '쿼리 함수' ,'옵션')
```

```js
const {data, isLoading, isError, error} = useQuery('posts', fetchPosts, {staleTime : 2000});
```
- 게시물이 2초마다 만료되도록 설정 했습니다.

<br>

### 개발자 도구에서 확인
![](https://velog.velcdn.com/images/hoho_0815/post/396d19df-6648-4421-bd2f-8dae326d3939/image.gif)

- 윈도우가 다시 포커스 될 때 fresh 상태가 되었고, 2초 후에 stale 상태로 바뀌었습니다.
- 새로 고침해도 2초 동안 fresh 상태였다가, stale 상태로 바뀝니다.

<br>

### staleTime 의 기본값은 왜 0 인가 ?
React Query 개발자인 Tanner Linsley 에 의하면, <br>
'업데이트가 왜 안 되나요?' 보다 '데이터가 어떻게 늘 최신 상태를 유지하나요?' 가 훨씬 좋은 질문 입니다. <br>
> staleTime 의 기본값을 0으로 설정했다는 말은 __데이터는 항상 만료 상태이므로 서버에서 다시 가져와야 한다고 가정한다는 뜻 입니다.__

***
<br>

## staleTime vs cacheTime
|staleTime|cacheTime|
|--|--|
|리패칭 할 때 고려 사항|나중에 다시 필요할 수도 있는 데이터|
||

<br>

### cacheTime
- 특정 쿼리에 대한 활성 useQuery 가 없는 경우 해당 데이터는 '콜드 스토리지' 로 이동 합니다.
- 구성된 cacheTime 이 지나면 캐시의 데이터가 만료되며 유효 시간의 기본값은 5분 입니다.
- cacheTime 이 관찰하는 시간의 양은 특정 쿼리에 대한 useQuery 가 활성화된 후 경과한 시간 입니다.<br>
    - 페이지에 표시되는 컴포넌트가 특정 쿼리에 대해 useQuery 를 사용한 시간을 말합니다.
- 캐시가 만료되면 가비지 컬렉션이 실행되고, 클라이언트는 데이터를 사용 할 수 없습니다.
- 데이터가 캐시에 있는 동안에는 패칭(Fetching) 할 때 사용될 수 있습니다.

***
<br>

## 참고
- https://velog.io/@yrnana/React-Query%EC%97%90%EC%84%9C-staleTime%EA%B3%BC-cacheTime%EC%9D%98-%EC%B0%A8%EC%9D%B4
- https://www.udemy.com/course/react-query-react/
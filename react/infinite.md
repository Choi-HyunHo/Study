# React Query - 무한 스크롤(Infinite Scroll)
사용자가 페이지의 특정 지점을 누르거나 버튼을 클릭하면 더 많은 데이터가 로딩됩니다. <br>
React Query 는 데이터와 페이지 번호를 추적하게 됩니다.

***
<br>

## 무한 스크롤 이란
사용자가 스크롤 할 때마다 새로운 데이터를 가져오는 것 입니다.
- 모든 데이터를 한 번에 가져오는 것보다 훨씬 효율적 입니다.

<br>

## 사용하는 지점
- 사용자가 버튼을 클릭하여 새로운 데이터를 요청하거나 
- 페이지의 특정 지점을 스크롤 했을 때 새 데이터를 가져오게 하거나 ( 페이지의 하단 등 )

***
<br>

## useInfiniteQuery 의 특징
1. 반환 객체에서 반환된 데이터 프로퍼티의 형태가 다릅니다. <br>
- useQuery : 단순히 쿼리 함수에서 반환되는 데이터
- useInfiniteQuery : 두 개의 프로퍼티를 가지고 있습니다.
    - 하나는 __데이터 페이지 객체__ 의 배열인 pages
    - 각 __매개 변수__ 가 기록되어 있는 pageParams ( 흔하게 사용되지는 않습니다. )

<br>

2. 모든 쿼리는 페이지 배열에 교유한 요소를 가지고 있고 그 요소는 해당 쿼리에 대한 데이터에 해당 합니다.
3. pageParams 은 검색된 쿼리의 키를 추적 합니다.

***
<br>

## useInfiniteQuery 구조
### 작동 원리
pageParam 은 __쿼리 함수__ 에 전달되는 매개변수
```js
useInfiniteQuery("쿼리 키",({pageParam = defaultUrl}) => fetchUrl(pageParam))
```
- 쿼리 키가 들어갑니다.
- 쿼리 함수가 실행되는 동안 매개변수, 객체를 구조 분해한 __pageParam__ 을 사용 합니다.
- 그리고 첫 번째 Url 로 정의한 Url 을 기본값으로 설정 합니다.
- 따라서 위의 함수는 defaultUrl 을 기본값으로 하는 pageParam 을 사용해서 해당 pageParam 에서 fetchUrl 을 실행 합니다.
- React Query가 pageParam의 현재 값을 유지 합니다.

<br>

### infiniteQuery 에 옵션을 사용하는 방식 예
- getNextPageParam : (lastPage, allPages)
    - 다음 페이지로 가는 방식을 정의하는 함수.
- lastPage 의 데이터에서 또는 allPages 의 데이터에서 모든 페이지에 대한 데이터를 가져옵니다.
    - 지금은 lastPage 를 사용한다고 가정하겠습니다.
    - lastPage 는 다음 페이지의 Url이 무엇인지 알려줍니다.
    - pageParam 을 업데이트 해줍니다.

***
<br>

## useInfiniteQuery 프로퍼티
1. fetchNextPage : 사용자가 더 많은 데이터를 요청할 때 호출하는 함수 <br>
    - 더 많은 데이터를 요청하는 버튼을 클릭하거나, 스크린의 데이터가 소진되는 지점을 누르는 경우 
2. hasNextPage 
    - getNextPageParam 함수의 반환 값을 기반으로 합니다.
    - 이 프로퍼티를 useInfiniteQuery 에 전달해서 마지막 쿼리의 데이터를 어떻게 사용할지 지시 합니다.
    - undefined 인 경우, __더 이상 데이터가 없다__ 는 의미이고 
    - useInfiniteQuery 에서 __반환 객체와 함께 반환된 경우__ hasNextPage 는 거짓이 됩니다.
3. isFetchingNextPage
    - useQuery 에는 없는 개념
    - useInfiniteQuery 는 다음 페이지를 가져오는지, 아니면 일반적인 패칭인지 구별 할 수 있습니다.

***
<br>

## The Flow (흐름)
1. 먼저 컴포넌트가 마운트(생성) 됩니다.
```js
const {data} = useInfiniteScroll(...)
```
- 이 시점에서 useInfiniteScroll 이 반환된 객체의 __data__ 프로퍼티가 정의되어 있지 않습니다.(undefined)
    - 쿼리를 만들지 않았기 때문

<br>

2. Fetch first page
그 다음 useInfiniteScroll 은 쿼리 함수를 사용해서 첫 페이지를 가져옵니다.
```js
useInfiniteScroll({pageParam = defaultUrl} => ...)
```
- 쿼리 함수는 useInfiniteScroll 의 첫 번째 인수이고 pageParam 을 인수로 받습니다.
- 따라서 첫 pageParam 은 우리가 기본값으로 정의한 것이 됩니다.
- 해당 pageParam 을 사용해서 첫 번째 페이지를 가져오고 반환 객체 데이터의 페이지 프로퍼티를 설정 합니다.
    - data.pages[0] : {...}
    - 인덱스가 0인 배열의 첫 번째 요소를 설정 합니다.
    - {...} 은 쿼리 함수가 반환하는 값이 됩니다.

<br>

3. getNextPageParam, Update pageParam
데이터가 반환된 후 React Query가 __getNextPageParam__ 을 설정 합니다.
```js
getNextPageParam : (lastPage, allPages) => ...
```
- useInfiniteScroll 의 옵션
- lastPage 와 allPages 를 사용해서 pageParam 을 업데이트하는 함수

<br>

4. 스크롤을 움직이거나, 버튼을 눌렀을 경우 - fetchNextPage
fetchNextPage 함수는 useInfiniteScroll 이 반환하는 객체의 속성
- 사용자가 더 많은 데이터를 요청할 때 지속적으로 업데이트 합니다.

***
<br>

## 참고
- https://www.udemy.com/course/react-query-react/

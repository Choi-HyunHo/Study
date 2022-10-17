# useInfiniteQuery 호출
## useInfiniteQuery 가져오기
```js
import InfiniteScroll from "react-infinite-scroller";
import { Person } from "./Person";

// 이 부분
import { useInfiniteQuery } from "react-query";

const initialUrl = "https://swapi.dev/api/people/";
const fetchUrl = async (url) => {
  const response = await fetch(url);
  return response.json();
};

export function InfinitePeople() {
  // 컴포넌트에서 호출
    const {data, fetchNextPage, hasNextPage} = useInfiniteQuery(
        "sw-people", 
        ({ pageParam =  initialUrl }) => fetchUrl(pageParam)
    );

  return <InfiniteScroll />;
}
```
- 반환된 객체에서 필요한건 __data__
    - 사용자가 페이지를 계속 로드할 때 여기에(data) 페이지의 데이터가 포함될 것 입니다.
- __fetchNextPage__
    - 더 많은 데이터가 필요할 때 어느 함수를 실행할지를 InfiniteScroll 에 지시하는 역할
- __hasNextPage__
    - 수집할 데이터가 더 있는 지를 결정하는 boolean 입니다.

<br>

## useInfiniteQuery 는 몇 가지 인수를 사용하는데 
- __쿼리 키__ 로 sw-people 입력
- __쿼리 함수({매개변수} => 함수(fetchUrl), {옵션})__
    - 매개변수를 받고 프로퍼티 중 하나로 __pageParam__ 을 가지고 있습니다.
    - pageParam 은 fetchNextPage 이 어떻게 보일지 결정하고, 다음 페이지가 있는지 결정 합니다.
- fetchUrl 이 하는 일은 Url인 pageParam 을 가져와서 json 으로 반환해 줍니다.
- useInfiniteQuery 를 처음 실행할 때는 pageParam 에 기본값을 주면 됩니다.
    - pageParam 이 설정되있지 않고, 기본 값이 바로 initialUrl(예시) 입니다.

<br>

## getNextPageParam 옵션
```js
const {data, fetchNextPage, hasNextPage} = useInfiniteQuery(
        "sw-people", 
        ({ pageParam =  initialUrl }) => fetchUrl(pageParam),
        {
            getNextPageParam : (lastPage) => lastPage.next || undefined
        }
    );
```
- getNextPageParam : (lastPage) => 쿼리 함수를 마지막으로 실행한 시점의 데이터
     - initialUrl 의 데이터는 next 프로퍼티를 가지고 있습니다.
     - next 프로퍼티는 다음 페이지로 가는데 필요한 Url 을 알려줍니다.
![](https://velog.velcdn.com/images/hoho_0815/post/6fc0e781-f797-456c-b18c-818d61b02bb9/image.png)
- fetchNextPage 를 실행하면 
    - next 프로퍼티가 무엇인지에 따라 마지막 페이지에 도착한 다음 pageParam 을 사용하게 됩니다.
- hasNextPage 는 lastPage.next 가 undefined 를 반환하는지 아닌지에 따라 결정 됩니다.

***
<br>

## React Infinite Scroller
Infinite Scroller 가 좋은 점은 useInfiniteQuery 와 호환이 잘 됩니다.
- [https://www.npmjs.com/package/react-infinite-scroll-component](https://www.npmjs.com/package/react-infinite-scroll-component)

<br>

## 무한 스크롤 컴포넌트의 프로퍼티
- loadMore = {fetchNextPage}
    - 데이터가 더 필요할 때 불러와 useInfiniteQuery에서 나온 fetchNextPage 함수 값을 이용 합니다.
- hasMore = {hasNextPage}
    - useInfiniteQuery 에서 나온 객체를 해제한 값을 이용 합니다.

<br>

무한 스크롤 컴포넌트는 스스로 페이지의 끝에 도달했음을 인식하고 fetchNextPage 를 불러오는 기능
- 데이터 프로퍼티에서 데이터에 접근 할 수 있는데 useInfiniteQuery 에서 나온 객체를 이용 합니다
- 배열인 페이지 프로퍼티를 이용해서 그 페이지 배열의 map 을 만들어 데이터를 표시 할 수 있게 해줍니다.

***
<br>

## 사용
### 
```js
import InfiniteScroll from "react-infinite-scroller";
import { Person } from "./Person";

import { useInfiniteQuery } from "react-query";

const initialUrl = "https://swapi.dev/api/people/";
const fetchUrl = async (url) => {
  const response = await fetch(url);
  return response.json();
};

export function InfinitePeople() {
  // TODO: get data for InfiniteScroll via React Query
  // data 는 useInfiniteQuery 에서 반환 됩니다.
  const {data, fetchNextPage, hasNextPage} = useInfiniteQuery(
    "sw-people", 
    ({ pageParam =  initialUrl }) => fetchUrl(pageParam),
    {
        getNextPageParam : (lastPage) => lastPage.next || undefined
    }
);

  return (
          <InfiniteScroll loadMore={fetchNextPage} hasMore={hasNextPage}>
            {data.pages.map(pageData => {
              return pageData.results.map(person => {
                return (<Person 
                  key={person.name} 
                  name={person.name} 
                  hairColor={person.hair_color}
                  eyeColor={person.eye_color}
                /> 
              )})
            })}
          </InfiniteScroll>
  )
}
```
```js
export function Person({ name, hairColor, eyeColor }) {
  return (
    <li>
      {name}
      <ul>
        <li>hair: {hairColor}</li>
        <li>eyes: {eyeColor}</li>
      </ul>
    </li>
  );
}
```
![](https://velog.velcdn.com/images/hoho_0815/post/42afaf73-7a89-469e-97db-97577291df25/image.png)

- 전달되는 데이터 형식
- 하지만 위와 같이 하면 'pages' 가 정의되지 않았다고 에러가 발생 합니다.
- useQuery 와 마찬가지로 isLoading 을 사용해서 캐시에 데이터가 없을 때 데이터를 가져옵니다.

<br>

```js
import InfiniteScroll from "react-infinite-scroller";
import { Person } from "./Person";

import { useInfiniteQuery } from "react-query";

const initialUrl = "https://swapi.dev/api/people/";
const fetchUrl = async (url) => {
  const response = await fetch(url);
  return response.json();
};

export function InfinitePeople() {
  // TODO: get data for InfiniteScroll via React Query
  // data 는 useInfiniteQuery 에서 반환 됩니다.
  const {data, fetchNextPage, hasNextPage, isLoading, isError} = useInfiniteQuery(
    "sw-people", 
    ({ pageParam =  initialUrl }) => fetchUrl(pageParam),
    {
        getNextPageParam : (lastPage) => lastPage.next || undefined
    }
  );

  if (isLoading) {
    return <div className="loading">Loading</div>
  }

  if (isError) {
    return <div>Error !</div>
  }


  return (
          <InfiniteScroll loadMore={fetchNextPage} hasMore={hasNextPage}>
            {data.pages.map(pageData => {
              return pageData.results.map(person => {
                return (<Person 
                  key={person.name} 
                  name={person.name} 
                  hairColor={person.hair_color}
                  eyeColor={person.eye_color}
                /> 
              )})
            })}
          </InfiniteScroll>
  )
}

```
<br>

## 결과

![](https://velog.velcdn.com/images/hoho_0815/post/1066b622-5c31-4ecd-bcbc-a70fb3e74fe7/image.gif)


<br>

## isFetching 을 사용하면
```js
const {data, fetchNextPage, hasNextPage, isFetching, isError} = useInfiniteQuery(
    "sw-people", 
    ({ pageParam =  initialUrl }) => fetchUrl(pageParam),
    {
        getNextPageParam : (lastPage) => lastPage.next || undefined
    }
  );

  if (isFetching) {
    return <div className="loading">Loading</div>
  }
```
데이터가 있지만 스크롤이 맨 위로 원위치가 반복 됩니다. <br>
이유는, 새로운 페이지를 열어야 할 때 조기 반환이 실행되기 때문입니다.

<br>

### 그러면 어떻게?
isFetching 은 조기 반환 시키지 않고, Loading 이라는 텍스트를 우상단에 출력하고 싶다.
```js
import InfiniteScroll from "react-infinite-scroller";
import { Person } from "./Person";

import { useInfiniteQuery } from "react-query";
import React from "react";

const initialUrl = "https://swapi.dev/api/people/";
const fetchUrl = async (url) => {
  const response = await fetch(url);
  return response.json();
};

export function InfinitePeople() {
  // TODO: get data for InfiniteScroll via React Query
  // data 는 useInfiniteQuery 에서 반환 됩니다.
  const {data, fetchNextPage, hasNextPage, isLoading, isError, isFetching} = useInfiniteQuery(
    "sw-people", 
    ({ pageParam =  initialUrl }) => fetchUrl(pageParam),
    {
        getNextPageParam : (lastPage) => lastPage.next || undefined
    }
  );

  if (isLoading) {
    return <div className="loading">Loading</div>
  }

  if (isError) {
    return <div>Error !</div>
  }


  return (
    <React.Fragment>
      {isFetching && <div className="loading">Loading</div>}
          <InfiniteScroll loadMore={fetchNextPage} hasMore={hasNextPage}>
            {data.pages.map(pageData => {
              return pageData.results.map(person => {
                return (<Person 
                  key={person.name} 
                  name={person.name} 
                  hairColor={person.hair_color}
                  eyeColor={person.eye_color}
                /> 
              )})
            })}
          </InfiniteScroll>
    </React.Fragment>
  )
}
```

***
<br>

## 참고
- https://www.udemy.com/course/react-query-react/
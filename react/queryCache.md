# React-Query 캐싱에 대하여
## stale 대하여
리액트 쿼리는 기본적으로 캐시된 데이터를 stale한 상태로 여깁니다. stale이란 최신화가 필요한 데이터라는 의미로 <br>
stale한 상태가 되면 다음의 경우에 refetch 됩니다.

- 새로운 query 인스턴스가 마운트될 때( useQuery가 처음 호출될 때 )
- 브라우저 화면을 이탈했다가 다시 포커스할 때
- 네트워크가 다시 연결될 때
- 특별히 설정한 refetch interval에 의해서 (refetchInterval) 

<br>
 
refetchOnWindowFocus 옵션 등으로 기본 refetch 설정을 막을 수 있고 <br>
staleTime 옵션으로 설정한 시간 동안 데이터가 stale 되지 않도록 해 refetch를 막을 수도 있습니다.
<br>

query에 별다른 action이 없으면 inactive 상태로 캐시에 남아 있다가 5분 뒤에 메모리에서 사라지지만, <br>
cacheTime 옵션을 설정해서 이 시간을 조정할 수 있습니다. ( 캐싱을 위한 포인트! )

***
<br>

## staleTime & cacheTime
먼저 둘의 개념과 차이부터 알아보겠습니다.

<br>

### staleTime
- 전달받은 데이터는 리엑트 쿼리의 자료구조 내용 중 캐시에 저장이 되는데, 
- 이때 이 캐시데이터의 __"신선한 상태"__ 가 언제까지 될지를 말해주는 옵션 입니다. 
- default는 0으로, 받아오는 즉시 stale하다고 판단하며 캐싱 데이터와 무관하게 계속해서 feching을 진행 합니다.

<br>

#### 신선하다 함은,
서버에서 조회한 데이터는 그때 당시의 데이터 snapshot이고, 외부 요청으로 서버 데이터가 변경이 되었다면 <br>
내 브라우저가 가진 데이터는 이미 오래된 낡은 데이터가 되었으므로 stale하다고 말하는 것이다.

<br>

### cacheTime
- __캐시 구조에 저장된 데이터는 메모리상에 존재하게 됩니다.__ 
- 이 때, 메모리에 저장되어 있는 캐시 데이터가 언제까지 유지될지를 말해주는 옵션 입니다.
- 즉, 캐싱된 쿼리의 결과값은 계속 유지되는 것이 아니라 시간이 지나면 메모리에서 사라집니다. 

***
<br>

## 사용 시 주의해야 할 점
만약 해당 useQuery를 호출할 당시에 옵션으로 staletime을 따로 지정해주지 않았었다면, <br>
항상 캐싱되어 있는 데이터는 stale하다고 여기기 때문에 refetching을 하게 되어 서버에 계속적인 요청을 합니다.

- 즉, staleTime 을 지정해주지 않고 사용하면 캐싱 기능을 제대로 활용할 수 없습니다.
- 만약 데이터 구조가 자주자주 변하는 어플리케이션이라면 지정하지 않는 편이 좋고 
- 해당 브라우저에 표현되는 내용의 데이터들이 정적이라면 staletime을 지정해주고 요청하는 것이 서버의 부담을 줄일 수 있습니다.

***
<br>


## Caching 적용
두 값은 options에 넣을 수 있습니다.

```js
const { data } = useQuery('users', getUsers, { // options
  staleTime: 5000,
  cacheTime: Infinity,
});
```

staleTime으로 설정한 시간만큼 api 요청한 데이터의 신선도가 유지되고 이 시간이 지나면 fresh한 상태에서 stale한 상태가 되기 때문에 <br> 같은 데이터를 다시 필요로 할 때 api 요청을 다시 할 수 밖에 없습니다.


***
<br>

## 요약
- 리액트 쿼리는 기본적으로 캐시된 데이터를 stale한 상태로 여깁니다. stale이란 최신화가 필요한 데이터라는 의미
- stale한 상태가 되면 다음의 경우에 refetch 됩니다.
- taleTime을 길게 줬다해도 저장되는 시간인 cacheTime이 짧다면 데이터가 사라지기 때문에 다시 요청을 해야 합니다.
- staleTime의 기본값은 0이고 cacheTime은 5분. 아무런 option을 주지 않으면 캐싱이 되지 않습니다.
- 데이터는 캐싱되지만 신선한 데이터가 하나도 없는 것이기 때문 입니다.

***
<br>

## 참고
- https://velog.io/@chltjdrhd777/React-Query-%EC%BA%90%EC%8B%B1%EC%97%90-%EB%8C%80%ED%95%9C-%EA%B5%AC%ED%98%84
- https://freestrokes.tistory.com/170
- https://thinkforthink.tistory.com/340
- https://www.udemy.com/course/react-query-react/
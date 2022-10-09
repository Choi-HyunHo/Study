# React Query - Dev Tools
React Query 개발자 도구는 앱에 추가할 수 있는 컴포넌트로 __개발 중인 모든 쿼리의 상태를 표시__ 해줍니다. <br>
또한 예상대로 작동하지 않는 경우 문제를 해결하는 데 도움이 될 수도 있습니다.

<br>

- 쿼리 키로 쿼리를 표시해 주고 
- 활성(active), 비활성(inactive), 만료(stale) 등 모든 쿼리의 상태를 알려줍니다.
- 마지막으로 업데이트된 타임 스탬프도 알려줍니다.
- 쿼리에 의해 반환된 데이터를 탐색할 수 있는 데이터 탐색기도 있습니다.
- 쿼리를 볼 수 있는 쿼리 탐색기도 있습니다.
- [공식 문서](https://tanstack.com/query/v4/docs/devtools?from=reactQueryV3&original=https://react-query-v3.tanstack.com/devtools)



***
<br>

## 개발자 도구 문서에서 주목하고 싶은 한 가지
![](https://velog.velcdn.com/images/hoho_0815/post/a746a751-ef90-4988-a42c-9f5cdab3ad17/image.png)

- 프로덕션 번들에 포함되어 있지 않다는 점 입니다.
- __NODE_ENV__ 변수에 따라 프로덕션 환경에 있는지 여부가 결정 됩니다.
- CRA 는 npm run build 를 실행할 때만 NODE_ENV 변수를 __production__ 으로 설정 합니다.
- 그렇지 않으면 development 또는 testing 으로 설정 됩니다.
- 즉, 개발 중일 때는 개발자 도구가 표시되고 프로덕션 때는 표시되지 않습니다.

***
<br>

## 사용
개발자 도구는 빌드 시 더 나은 패키지 관리를 위해 하위 패키지에서 가져옵니다.
```js
import { ReactQueryDevtools } from "react-query/devtools";

<ReactQueryDevtools/>
```
- 위 두가지 코드를 아래 처럼 추가 합니다.

<br>

```js
import { Posts } from "./Posts";
import "./App.css";

import { QueryClient, QueryClientProvider } from 'react-query';
import { ReactQueryDevtools } from "react-query/devtools";

const queryClient = new QueryClient(); // 클라이언트가 있기 때문에 provider 를 추가 할 수 있습니다.

function App() {
  return (
    // provide React Query client to App
    <QueryClientProvider client={queryClient}>
      <div className="App">
        <h1>Blog Posts</h1>
        <Posts />
      </div>
      <ReactQueryDevtools/>
    </QueryClientProvider>
  );
}

export default App;
```

<br>

### 적용한 화면
![](https://velog.velcdn.com/images/hoho_0815/post/75f035f2-96dd-4a9d-82b4-5a767be2162b/image.png)

- React Query 꽃이 보이는데 이것이 개발자 도구 입니다.

<br>

### 개발자 도구  

![](https://velog.velcdn.com/images/hoho_0815/post/ee68f16f-4b3a-4632-8536-ed5618ab0018/image.gif)


꽃을 클릭하면 개발자 도구를 열고 닫고 할 수 있습니다. <br>
Posts 라고 하는 쿼리를 볼 수 있습니다. 또한 우측을 보면 상태가 __만료(stale)__ 임을 알 수 있습니다.
- 업데이트 시간도 볼 수 있고
- Data Expolorer 에서 어떤 데이터가 있는 지 볼 수 있습니다.
- Query Expolorer 에서 쿼리를 볼 수 있습니다. + 여러가지 옵션들

***
<br>

## 참고
- https://www.udemy.com/course/react-query-react/
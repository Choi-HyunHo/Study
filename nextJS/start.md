## React의 문제점과 NextJS의 도입

### React의 번거로운 부분
리액트는 애플리케이션을 처음부터 빌드하려면 신경 써야하는 세부사항이 많습니다.

1. Webpack으로 번들링 & Babel 컴파일러로 JSX → JS 로 변환
2. 최적화 (Code Splitting 등)를 해야함
3. Static Pre-Rendering (성능과 SEO를 위해) / SSR 또는 CSR 를 해야함
4. 리액트 앱과 데이터 저장소를 연결하기 위한 서버 측 코드 작성

<br>

### NextJS: React 프레임워크
NextJS는 React 의 번거로운 점을 간편하게 해결 했습니다.

1. 직관적인 페이지 기반 라우팅 (Dynamic routes 도 지원함)
2. Pre-rendering, static generation (SSG), server-side rendering (SSR) 이 페이지 단위로 제공됨
3. 자동 Code Splitting
4. prefetching을 통한 최적화된 CSR
5. Fast Refresh
6. 서버리스 함수로 API 엔드포인트를 빌드하기 위한 API routes
7. 확장성

> #### Code Splitting ( 코드 분할 ) 
-> 코드 분할은 __싱글 페이지 애플리케이션(SPA) 의 성능__ 을 향상 시키는 방법

***
<br>

## Next.js 설치
1. 설치 
```js
npx create-next-app@latest ( 가장 최신 버전을 사용 )
# or
yarn create next-app
```
- 만약 타입스크립트와 같이 사용하고 싶다면 -> `npx create-next-app --typescript`

2. what is your project name ?
-> 프로젝트 명을 물어봅니다.
-> nest 는 next 오타.. ㅜ

3. 설치 완료되면 아래 명령어를 이용해서 실행
```js
npm run dev
# or
yarn dev
```
***

## 폴더 구조
![](https://velog.velcdn.com/images/hoho_0815/post/feff28ee-3cf1-4c82-82ac-0616000702fe/image.png)

***
<br>

## Next.js 는 프레임워크
React 는 라이브러리로 사용자가 원할 때 언제든 어떤 방법으로 부를 수 있습니다.
하지만 Next 는 프레임워크로 그에 해당하는 틀안에서 작업을 해야 합니다.

> ### 라이브러리 vs 프레임워크
- 라이브러리
-> 사용자가 원할 때 언제든 어떤 방법으로 부를 수 있다.( 도구 )
- 프레임워크
-> 특정한 규칙을 따라서 그에 맞게 행동해야 한다. ( 틀 )
-> 코드를 어떤 곳에 넣으면, 프레임워크가 그 코드를 부르는 형태

***
<br>

## Pages 폴더에서 일어나는 일
![](https://velog.velcdn.com/images/hoho_0815/post/6ad024a1-7f7a-4367-bd68-cf6fffcc10a8/image.png)

pages 폴더 안에 파일을 만들면

- 어떤 설정이나, router 설치를 안해도
- 별 다른 설치나 준비를 하지 않아도 자동적으로 서버의 `home` 으로 가보면

![](https://velog.velcdn.com/images/hoho_0815/post/4e2dc4d0-ee05-4438-82af-a09be25fd6aa/image.png)

- 볼 수 있습니다.

***
<br>

## 정리
### 라이브러리와 프레임워크
- 라이브러리 : 개발자가 어떤 프로그램을 가져다 쓰는 것
-> 사용자가 파일 이름이나 구조 등을 정하고, 모든 결정을 내림
- 프레임워크 : 개발자의 코드를 프로그램이 불러오는 것
-> 파일 이름이나 구조 등을 정해진 규칙에 따라 만들고 따름
> 둘 사이의 주요 차이점은 __'Inversion of Control'__ ( 통제의 역전 ) 입니다.
라이브러리에서 메서드를 호출하면 사용자가 제어 할 수 있지만,
프레임워크에서는 제어가 역전되어 프레임워크가 사용자를 호출 합니다.

***
<br>

## 참고
- https://nextjs.org/learn/basics/create-nextjs-app

# Next.js - Pre Rendering
## 1. React.js 의 방식

### 1.1 기존의 __React__ 는 __CSR__ 방식으로 화면을 렌더링 합니다.
> CSR(Client Side Rendering)
- 클라이언트 측에서 화면을 직접 렌더링 합니다.
- 즉, 브라우저가 유저가 보는 UI 를 만듭니다.

- React 로 만들어진 화면의 모든 것은 HTML 소스코드 안에 들어있지 않습니다.
- React 로 만들어진 앱을 개발자 도구로 확인하면, 하나의 `<div>` 만을 가지고 있을 것 입니다.

> 브라우저가 HTML 을 가져올 때 비어있는 div 으로 가져옵니다.
그 후, 브라우저가 모든 자바스크립트를 요청해서 자바스크립트와 React.js 를 실행 시키고 
그 다음 화면이 유저에게 보이게 됩니다.

<br>

### 1.2 만약 자바스크립트를 가져오는 속도가 느리다면?
사용자는 화면이 렌더링 되는 시간을 오래 동안 기다려야 할 것 입니다.
-> 브라우저는 자바스크립트 코드가 왔을 때에만 UI 를 만들 수 있습니다.

***

<br>

## 2. Next.js - Pre Render
__페이지의 소스코드 어딘가에 실제 HTML 이 존재 합니다.__
![](https://velog.velcdn.com/images/hoho_0815/post/40e9b362-a864-4bb6-b0d4-7522868f6653/image.png)
- 따라서 연결 속도가 느리거나, 자바스크립트가 완전히 비활성화 되있어도
적어도 유저는 HTML 을 볼 수 있습니다.

<br>

### Next 는 
모든 페이지가 사용자에게 전해지기 전에 HTML을 미리 생성해서 __Pre Rendering__ 을 
수행 합니다.

- create-react-app으로 만든 일반적인 애플리케이션일 경우 js 기능을 브라우저에서 사용하지 않으면 보이지 않게 되는데, 프리 렌더링된 next 앱은 서버에서 미리 만들어지기 때문에 js 기능을 사용하지 않더라도 볼 수 있습니다.

<br>

### 2.1 Pre-Rendering의 과정
![](https://velog.velcdn.com/images/hoho_0815/post/5b2bb811-d795-4807-bccb-70de94fbcf5e/image.png)

![](https://velog.velcdn.com/images/hoho_0815/post/e3937943-82fc-4cf5-b4a3-a08595618052/image.PNG)
#### 2.1.1 initial load html
> JS 동작만 없는 HTML을 먼저 화면에 보여주는데, 아직 JS 파일이 로드가 되지 않았기에 `<Link>` 같은 컴포넌트는 동작하지 않습니다.

#### 2.1.2 hydration
>  HTML 을 로드한 뒤 js 파일을 서버로부터 받아 HTML 과 연결시키는 과정 입니다.
해당 과정에서 react 컴포넌트는 초기화되고 사용자와 상호작용 할 준비를 합니다.

<br>

### 2.2 Pre-Rendering 의 두 가지 방식
__HTML 을 만드는 시점에 따라 달라집니다.__



#### 2.2.1 Static Generation (SSG)
> HTML 을 빌드 타임에 생성하고 각 요청에 따라서 재사용 합니다.

![](https://velog.velcdn.com/images/hoho_0815/post/fa3f770e-47d3-401d-b5a5-80c241efe890/image.PNG)

- 동일한 HTML 을 매 요청마다 생성해서 쓰는 SSR 의 단점을 보완하기 위해서 탄생한 기법 입니다.
1. Next.js 내부에 존재하는 Pre-Render 메서드가 최초에 HTML 을 build 할 때 동작 합니다.
2. 그리고 Pre-Render 된 HTML 파일은 요청에 따라 재사용 됩니다. 

> __SSG의 2가지 상황__
- Page 의 내용물이 외부 데이터에 의존적인 상황 => getStaticProps
- Page Paths 까지 외부 데이터에 의존적인 상황 => getStaticPaths

<br>

#### 2.2.2 Server Side Rendering (SSR)
> HTML을 각 리퀘스트가 일어날 때 마다 생성 합니다.

![](https://velog.velcdn.com/images/hoho_0815/post/aa644593-9ddc-49bc-860b-541c7c36c74d/image.PNG)

***

## 3. 만약 Pre-Rendering이 없다면?
![](https://velog.velcdn.com/images/hoho_0815/post/41a290aa-837b-4329-b58e-069765c6a2e0/image.PNG)
JS 전체가 로드되어야 하기 때문에 최초 Load에서 사용자에게 보여지지 않게 됩니다.
즉, 우리는 전체 페이지가 로드되기 전 까지 페이지를 볼 수 없다는 뜻이 되고 결국 UX는 낮은 평가를 받게 될 수 있습니다.

***
<br>

## 4. Pre-rendering 과 SEO의 상관관계
- CSR 만 제공한다면 Client(브라우저) 처럼 동작하지 않는 검색엔진의 경우 아무런 데이터도 가져갈 수 없습니다.
- __Pre-render 를 해두면 Client 처럼 동작하지 않는 검색엔진에게 필요한 데이터를 제공 할 수 있습니다.__


***
<br>

## 참고

- https://wonit.tistory.com/362
- https://ppsu.tistory.com/63
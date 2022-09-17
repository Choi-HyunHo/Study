## 1. Next.js 정의
> __Next.js__ 는 따로 설정을 해주지 않고도 SSR, SEO부터 TypeScript까지 생산에 필요한 많은 기능들을 제공하는 아주 강력한 React 프레임워크 입니다.

***
<br>

## 2. 어디서 만들었나
![](https://velog.velcdn.com/images/hoho_0815/post/c41713e6-ab29-4e8b-8181-92032da86755/image.png)

Next.js는 __Vercel__이라는 Frontend Team에서 만들었습니다.
***
<br>

## 3. 사용해야 하는 이유
### 3.1 SSR
Next.js 를 사용하는 가장 큰 이유 입니다.
이해하기 좋게 먼저 반대 개념인 __SPA__ 와 __CSR__에 대해 먼저 정리 하겠습니다.

### 3.1.1 SPA (Single Page Application)
![](https://velog.velcdn.com/images/hoho_0815/post/92928b42-70be-48a9-a90f-bc428a4465e7/image.png)

전통적인 웹 페이지 만드는 방식은
1. client 에서 server 로 최초의 요청을 보내고
2. server 는 요청을 받아 client 에게 응답을 보낸 후, client에서 화면이 보입니다.
3. 이후 client 에서 상호작용이 있을 때 마다, server로 요청을 보내고
4. server 는 이에 응답하며 페이지가 Reload 됩니다.

- 하지만 위와 같은 방법인 경우 사용자가 새로운 요청을 보내고 응답을 받을 때 마다, 사용자의 페이지가 Reload 되기 때문에 사용자 경험이나 서버에 부담이가 비용적인 측면에서 좋지 않습니다.

- <span style="color : brown">위와 같은 방법을 해결 할 수 있는 것이 __SPA__ 입니다.</span>
  
![](https://velog.velcdn.com/images/hoho_0815/post/4336e7ce-dd89-4066-808d-a4e0610a2ca9/image.png)
  
- SPA 는 필요한 정적 리소소를 최초에 모두 다운로드 합니다.
-> 이후 변경이 있을 때마다 페이지 전체를 Reload 하지 않고 변경된 부분만 갱신 합니다.
<br>

### 3.1.2 CSR (Client Side Rendering)
![](https://velog.velcdn.com/images/hoho_0815/post/d5d759f6-f807-4515-9f89-8532c90f4586/image.png)

__CSR__ 동작 순서는
1. 서버에서 브라우저로 응답을 보냅니다.
2. 브라우저에서 __JS__ 를 다운 받습니다.
3. 브라우저가 React 를 실행 합니다.
4. 페이지가 보이고 상호작용이 가능 합니다.

#### CSR 의 장점
- 컴포넌트 단위로 UI를 구성하기 때문에 재사용에 용이 합니다.
- 페이지 전환이 부드럽고 빠릅니다.
- 변경된 사항만 server로 전달하기 때문에 서버에 부담이 덜하고 비용적인 측면에서 효율적 입니다.

#### CSR 의 단점
- 초기 로딩 때 모든 정적 리소스들을 다운 받아서 초기 페이지 로딩이 느립니다.
- SEO가 좋지 않습니다.


<br>

### 3.1.3 SSR (Server Side Rendering)
![](https://velog.velcdn.com/images/hoho_0815/post/f2879a95-7571-4adc-a344-5153bca34768/image.png)

__SSR__ 의 동작 순서는
1. 서버는 렌더링할 준비가 된 HTML을 응답을 브라우저에게 보냅니다.
2. 브라우저는 페이지를 렌더링하고 이 때 페이지를 볼 수 있고 이 때, 브라우저가 JS를 다운로드 받습니다.
3. 브라우저가 React를 실행합니다.
4. 페이지를 상호작용 할 수 있습니다.

#### SSR 의 장점
1. CSR에 비해 초기 렌더링 속도가 빠릅니다.
2. SEO 가 좋습니다.

#### SSR 의 단점
1. CSR에 비해 서버와 요청하는 횟수가 많아 서버에 부담이 갈 수 있습니다.
2. 페이지 전환 시 마다 새로고침이 일어납니다.

<br>

### 3.1.4 CSR 과 SSR 의 차이
#### 1. 사용자에게 더 빨리 보여지는 것 -> SSR
#### 2. view 와 상호작용
__CSR__ : 최종적으로 로딩이 끝난 후 가능
__SSR__ : Page가 먼저 보여지나 JS 와 React 로딩이 끝난 후 가능
***
<br>

## 3.2 SEO (Search Engine Optimization)
검색엔진 최적화 입니다.
기존 리액트에서 사용하는 CSR 방식은 SEO가 좋지 않습니다.
따라서 검색 엔진 최적화를 위해서는 SSR이 중요한 역할을 합니다.

### 3.2.1 검색엔진 최적화를 하는 이유
웹 사이트를 만들고 브라우저에 검색을 했을 때, 사이트가 상단에 위치해야 유입이 많아질 것 입니다.
__하지만 CSR 방식은 검색엔진에 노출이 잘 되지 않습니다.__ ( 노출이 안되는 것이 아닙니다!! )
- ex ) [네이버 검색엔진](https://searchadvisor.naver.com/guide/seo-advanced-javascript)
![](https://velog.velcdn.com/images/hoho_0815/post/0644fb79-bf26-4b2c-8864-c667502a1d1d/image.PNG)

> 검색엔진 봇들은 JavaScript를 해석하기 힘들기 때문에 HTML에서 크롤링하게 됩니다. CSR 방식은 Client 측에서 페이지를 구성하기 전에 HTML에 아무것도 없으므로 데이터를 수집하지 못해 검색엔진에 노출이 어려운 것 입니다.

***
<br>

## 3.3 마지막 Next.js 의 작동 방식
1. 사용자가 초기에 Server에 페이지 접속을 요청한 경우 __SSR 방식__ 으로 렌더링 될 HTML을 보냅니다.
2. 브라우저는 JS를 다운받고 React 를 실행 합니다.
3. 사용자가 페이지와 상호작용을 하여 다른 페이지로 이동할 경우 __CSR 방식__ server 가 아닌 브라우저에서 처리 합니다.

***
<br>

## 정리
Next.js 는 Vercel 에서 만든 __SSR__, __SEO__ 를 하기 위한 React 의 프레임워크
- 초기에 server 에 페이지 접속을 요청 한 경우 __SSR 방식__ 으로 렌더링 될 HTML 을 보내줌으로써 __SEO__ 최적화가 되어 검색 엔진에 노출이 됩니다.
- 페이지와 상호 작용을 할 때는 __CSR__ 방식으로 처리함으로 __SPA__ 장점을 가지고 있습니다.

***
<br>

## 참고
- https://vercel.com/
- https://nextjs.org/
- https://medium.com/walmartglobaltech/the-benefits-of-server-side-rendering-over-client-side-rendering-5d07ff2cefe8
- https://www.excellentwebworld.com/what-is-a-single-page-application/
- [SSR 개념 이해와 Next.js로 실습까지 해보는 SSR 환경 구축하기](https://velog.io/@jeff0720/Next.js-%EA%B0%9C%EB%85%90-%EC%9D%B4%ED%95%B4-%EB%B6%80%ED%84%B0-%EC%8B%A4%EC%8A%B5%EA%B9%8C%EC%A7%80-%ED%95%B4%EB%B3%B4%EB%8A%94-SSR-%ED%99%98%EA%B2%BD-%EA%B5%AC%EC%B6%95)
- [NextJS, 그게 뭔데?](https://velog.io/@skypedanny/NextJS-%EA%B7%B8%EA%B2%8C-%EB%AD%94%EB%8D%B0#%EC%95%88%EB%85%95%ED%95%98%EC%84%B8%EC%9A%94%E2%99%82%EF%B8%8F)

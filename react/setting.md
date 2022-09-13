# Create-React-App
## React App을 만드는 방법
### 라이브러리의 활용
- React의 package를 다운로드 받는다고 바로 사용할 수는 없습니다.
- 추가적인 아래와 같은 라이브러리들이 필요 합니다.
- 물론 이외에도 존재 합니다.
![KakaoTalk_20220405_171531882](https://user-images.githubusercontent.com/87301268/161710170-401f516c-abe1-469e-922b-d9ae2cc2f092.jpg)

<br>

### Boiler Plate (Create-React-App)
- 복잡한 환경 설정까지 다 해놓은 package 를 감싼 package
- 미리 틀을 준비하고 사용만 하면 됩니다.
![KakaoTalk_20220405_171532197](https://user-images.githubusercontent.com/87301268/161710425-38d8736d-1c94-4266-92a6-837937acfa95.jpg)

***

## Create-React-App
### 설치 방법
```js
npx create-react-app 이름 
```

<br>

### yarn
```js
1. npm install -g yarn
2. yarn create react-app 파일명
```

<br>

### 설치 후 초기 프로젝트 파일
```js
.
├── README.md
├── package-lock.json
├── package.json
├── public
│   ├── favicon.ico
│   ├── index.html
│   ├── logo192.png
│   ├── logo512.png
│   ├── manifest.json
│   └── robots.txt
└── src
    ├── App.css
    ├── App.js
    ├── App.test.js
    ├── index.css
    ├── index.js
    ├── logo.svg
    ├── reportWebVitals.js
    └── setupTests.js
```

- 파일 삭제나 정리 할 때 참고하면 좋은 글
- [hwang-eunji](https://velog.io/@hwang-eunji/create-react-app-%EC%9C%BC%EB%A1%9C-%EB%A6%AC%EC%95%A1%ED%8A%B8-%EA%B8%B0%EB%B3%B8%EC%84%B8%ED%8C%85)
- [duswn38](https://velog.io/@duswn38/React)
- [ssungkang](https://ssungkang.tistory.com/entry/React-React-%EC%8B%9C%EC%9E%91%ED%95%98%EA%B8%B0-create-react-app)

### 삭제 파일
- App.test.js
- logo.svg
- reportWebVitals.js
- setupTests.js

<br>

### React 실행
```js
npm start // 종료는 터미널에서 ctrl+c 후 y 누르기
```
<img width="500" height="300" alt="스크린샷 2020-02-16 오후 10 28 25" src="https://user-images.githubusercontent.com/87301268/161721629-7ebfa225-def5-4f58-84e1-e5c2676fcfeb.png">

***

## 참고
- https://www.daleseo.com/create-react-app/
- [인프런](https://www.inflearn.com/course/%ED%95%9C%EC%9E%85-%EB%A6%AC%EC%95%A1%ED%8A%B8/dashboard)
- [yarn 참고](https://somjang.tistory.com/entry/React-%EC%8B%A4%EC%8A%B5%ED%99%98%EA%B2%BD-%EA%B5%AC%EC%84%B1%ED%95%98%EA%B8%B0-1-yarn-%EC%84%A4%EC%B9%98%EC%99%80-create-react-app-%EC%84%A4%EC%B9%98%ED%95%98%EA%B8%B0)
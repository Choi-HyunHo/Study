# Node.js + Express.js 준비
공부하면서 목적은, React 와 함께 사용할 서버를 구축해보고 mongoDB 에 연결하여 사용해보기

<br>

## Node.js 다운
1. 터미널 또는 cmd 에 `node -v` 명렁어 입력하여 버전 확인하기
2. 만약 설치되어 있지 않으면, [node.js 사이트](https://nodejs.org/ko/) 에서 다운로드

<br>

## 보일러 플레이트 만들기
### 폴더 및 파일 생성하기
1. cd desktop
2. mkdir boiler-plate(이름은 자유)
3. cd boiler-plate(폴더 내부로)
<br>

![](https://velog.velcdn.com/images/hoho_0815/post/398243f9-fbea-41b3-9b10-08fb08eebd61/image.png)


<br>

### npm 설치
```js
// 설치
npm init

// npm 업데이트 명령어 -> 만들면서 업데이트 하려고 알려줘서 같이 정리
sudo npm install -g npm
```

<br>

1. npm init
2. package name  : enter 누르면 자동으로 생성
3. version -> enter
4. description : 자유 ---> 이후 쭉 넘겨도 무방
5. author 는 이름
6. license -> enter 눌러서 넘기면 package 생성 완료

<br>

![](https://velog.velcdn.com/images/hoho_0815/post/93f34b78-d4bd-4358-a379-bf09556ec4b5/image.png)

<br>

### package.json
`npm init` 을 함으로써, __package.json__ 이 자동적으로 생성

```js
{
  "name": "boiler-plate",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "hyunho-choi",
  "license": "ISC",
}
```

<br>

### index.js 만들기
백엔드 서버의 시작점

<br>

### express.js 다운
```js
npm install express --save
```


#### 파일 구조 및 package.json 

![](https://velog.velcdn.com/images/hoho_0815/post/1c67f378-4c9a-4afb-873b-be3eb76140cc/image.png)

- express 라고 추가된 부분이 있는데 설치 하면서 라이브러리를 사용하고 있다고 표시하고 있습니다.
- node.modules 라는 폴더가 생기는데 여기 안에는 다운받은 __dependencies__ 가 추가 됩니다.

<br>

### index.js 에서 기본적인 express.js 앱 만들기

#### index.js
```js
const express = require('express') // express 모듈을 가져온다.
const app = express() 
const port = 3000 

app.get('/', (req, res) => res.send('Hello World!')) // 루트 디렉토리에 hello world! 출력

app.listen(port, () => console.log(`Example app listening on port ${port}!`)) // 위의 포트(5000) 에서 실행
```

<br>

#### 그 다음 `scripts` 에서 `start` 추가
```js
"scripts": {
    "start": "node index.js",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
```

- start 를 하면 노드 앱을 실행하게 되는데
- 실행 할 때 시작점이 __index.js__ 라고 지정
- 터미널에 `npm run start` 실행

<br>

![](https://velog.velcdn.com/images/hoho_0815/post/c8358dd9-fffa-44a8-b4ef-5310776d90d6/image.png)

<br>

![](https://velog.velcdn.com/images/hoho_0815/post/ba5531dc-1b2e-435a-8cb9-b2536123a781/image.png)

***
<br>

## 참고
- 따라하며 배우는 노드 리액트 기본
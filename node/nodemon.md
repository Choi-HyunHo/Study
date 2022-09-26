# Nodemon
node 서버 안에서 변경 사항이 일어나면, 서버를 내리고 재기동 해야 변경 사항이 반영이 됩니다.<br>
하지만 __Nodemon 을 이용하면 서버를 내리지 않아도 소스의 변화를 감지해서 변화된 부분을 반영 시켜 줍니다.__

***
<br>

## 설치
```ja
npm install nodemon --save-dev
```
dev 는 개발 모드, 즉 로컬에서 작업 할 때만 사용을 한다는 말 입니다. ( 필수는 아님! )

<br>

![](https://velog.velcdn.com/images/hoho_0815/post/38f2b127-57a6-480f-877a-ea71374ec566/image.png)

<br>

기존에는 `dependencies` 에 추가가 되었지만, `-dev` 를 붙임으로써, 아래에 별도로 표기 되었습니다.

***
<br>

## 시작을 nodemon 으로 하기

![](https://velog.velcdn.com/images/hoho_0815/post/e73edd73-5287-417b-b5a2-aeac851ae1d9/image.png)

<br>

지금은 시작 할 때 `npm run start` 를 사용하여 서버를 기동 했습니다. <br>
하지만 시작을 `nodemon` 으로 하기 위해 코드를 추가 해야 합니다.

<br>

![](https://velog.velcdn.com/images/hoho_0815/post/60054a50-fbd5-4e77-9da0-d5038078b652/image.png)

<br>

dev 라고 정의 했지만 사용자 마음대로 작성 할 수 있습니다.
아래 터미널을 보면 `npm run dev` 로 __nodemon__ 을 이용해서 정상적으로 서버를 재기동 한 것을 확인 할 수 있습니다.

<br>

> 지금부터는 변경 사항이 실시간으로 반영이 될 것 입니다.

***
<br>

## 참고
- 따라하며 배우는 노드, 리액트 시리즈
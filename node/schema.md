# MongoDB Model & Schema

웹사이트에서 회원가입을 하거나 할 때,<br> 이름이나 주소, 나이, 번호 및 유저의 정보들을 데이터베이스에 보관하기 위해 만드는 것

<br>

## Model ??
Schema 를 감싸주는 역할을 합니다.

<br>

## Schema ??

![](https://velog.velcdn.com/images/hoho_0815/post/59564cef-8ed6-412c-88c6-170b339bf0c1/image.png)
- productSchema 부분이 스키마 입니다.
- 만약, 어떤 상품에 관련된 글을 작성할때 그 상품의 작성자와, 포스트의 이름, 그리고 내용 등 해당 하는 것의 타입과 조건들을 지정하는 것

<br>

## models 폴더 생성

![](https://velog.velcdn.com/images/hoho_0815/post/df8b7ac1-cb49-4d26-a117-8f1a272b2b47/image.png)

<br>

## Schema 작성 - User.js
```js
const mongoose = require('mongoose');

const userSchema = mongoose.Schema({
    name : {
        type : String,
        maxlenght : 50
    },
    email : {
        type : String,
        trim : true, // hyun ho@naver.com 이라고 하면, 이름 중간에 공백이 있는데 이것을 없애주는 역할을 한다.
        unique : 1,  // 같은 이메일은 사용 할 수 없게 
    },
    password : {
        type : String,
        minlenght : 5
    },
    lastname : {
        type : String,
        maxlenght : 50
    },
    role : {
        // 어떤 유저가 관리자가 될 수도, 일반 유저가 될 수도 해당 등급에 대한 조건
        type : Number,
        default : 0
    },
    image: String,
    token : {
        // 유효성 
        type : String
    },
    tokenExp : {
        // 토큰의 유효기간
        type : Number
    }
})
```

<br>

## Model 로 Schema 감싸기

```js
const User = mongoose.model('User', userSchema)

module.exports = {User}
```

- model 의 이름과, 해당 스키마 넣기
- model 를 다른 파일에서도 사용 할 수 있게 __exports__

<br>

## 최종 코드
```js
const mongoose = require('mongoose');

const userSchema = mongoose.Schema({
    name : {
        type : String,
        maxlenght : 50
    },
    email : {
        type : String,
        trim : true, // hyun ho@naver.com 이라고 하면, 이름 중간에 공백이 있는데 이것을 없애주는 역할을 한다.
        unique : 1,  // 같은 이메일은 사용 할 수 없게 
    },
    password : {
        type : String,
        minlenght : 5
    },
    lastname : {
        type : String,
        maxlenght : 50
    },
    role : {
        // 어떤 유저가 관리자가 될 수도, 일반 유저가 될 수도 해당 등급에 대한 조건
        type : Number,
        default : 0
    },
    image: String,
    token : {
        // 유효성 
        type : String
    },
    tokenExp : {
        // 토큰의 유효기간
        type : Number
    }
})

const User = mongoose.model('User', userSchema)

module.exports = {User}
```

***
<br>

## 참고
- 따라하면서 배우는 노드 리액트 기본
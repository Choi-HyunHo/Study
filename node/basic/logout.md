# 로그아웃 기능
로그아웃 Route 를 만들어서 -> 로그아웃 하려는 유저를 데이터베이스에서 찾아서 -> 그 유저의 토큰을 지운다.

<br>

## 왜 토큰을 지우면 될까 ?
저번에 auth 기능을 만들었는데 그 곳에서 인증을 할 때, 클라이언트 쿠카에 있는 토큰을 가져와서 <br>
서버의 데이터베이스에 있는 토큰을 같은지 확인하며 인증을 하였는데, <br>
만약, 토큰이 데이터베이스에 없다면 클라이언트에서 가져온 토큰이 맞지 않기 때문에 인증이 되지 않습니다. <br>

- 그렇기에 로그아웃 할 때, 토큰을 지우면 인증이 안되서 로그인 기능이 풀립니다.

<br>

## logout route 만들기
### index.js
```js
// 로그아웃 기능
// auth 를 사용하는 이유는 로그아웃 이라는 건 로그인 된 상태이기 때문에
app.get('/api/user/logout', auth, (req, res)=>{
    // 유저를 찾아서 업데이트 시켜주는 역할
    User.findOneAndUpdate({_id : req.user._id}, {token : ""}, (err, user)=>{
        if(err){
            return res.json({success : false, err});
        } else {
            return res.status(200).send({
                success : true
            })
        }
    })
})
```

<br>

## 테스트
### 1. 데이터베이스 확인
![](https://velog.velcdn.com/images/hoho_0815/post/e5e66ad0-4f57-4b51-8239-4fb7180a7534/image.png)


- 현재 토큰이 있으므로, 로그인 되어 있는 상태

<br>

### 2. 서버 기동

<br>

### 3. PostMan
#### 3.1 로그인

![](https://velog.velcdn.com/images/hoho_0815/post/e169e9c4-dd76-473f-be5f-756bb29a97a9/image.png)

<br>

![](https://velog.velcdn.com/images/hoho_0815/post/cfaf8a4a-0f80-490a-a0f1-ad146788b8f9/image.png)


<br>

#### 3.2 로그아웃
![](https://velog.velcdn.com/images/hoho_0815/post/47595e4f-2f34-4e87-92e6-ecbb2c0e27ad/image.png)

<br>

![](https://velog.velcdn.com/images/hoho_0815/post/41e516ef-c677-4c63-805c-9d4e16dcfaeb/image.png)


***
<br>

## 참고
- 따라하며 배우는 노드, 리액트
# 저장소 git 연동 - SHH 이용
### git 명령어
당연히 git 을 먼저 다운 받아야 합니다
- git -v 을 사용하여 버전 및 다운 상태 확인

<br>

## 순서
1. 해당 폴더에서 터미널 오픈
2. git init <br>
-> 입력하면 돋보기 아래에 천개가 넘는 파일이 갑자기 생길 것 입니다.

![](https://velog.velcdn.com/images/hoho_0815/post/93e805c8-2194-426b-8ad7-8f62533ffa03/image.png)

<br>

3. 하지만 node_modules 은 굳이 저장소에 올리지 않아도 됩니다. ( 너무 크기가 크다,, ) <br>
4. 그리하여, 내가 필요한 것만 올릴 수 있게 __.gitignore__ 파일을 만들어 안에다 올리지 않을 파일 명을 적습니다.

![](https://velog.velcdn.com/images/hoho_0815/post/d5ce5419-d1c1-4776-9e96-1d63e9c8fb5a/image.png)

5. 파일이 사라진 것을 확인 할 수 있습니다.
6. __git add .__ 사용하여 저장소에 올릴 준비를 하고
7. 저장소에 올리기 위해 __git commit -m "메세지 입력"__

<br>

## github
git 을 사용해서 로컬 저장소에 저장을 했는데, <br>
github 에서 하는 일은 원격 저장소를 만들어서 코드를 관리하는 곳 입니다.
<br>

지금은 기본적인 연결이 아닌 __SSH__ 을 사용해서 github 와 연동을 하겠습니다.

<br>

## SSH ( Secuer Shell )
네트워크 프로토콜 중 하나로 컴퓨터와 컴퓨터가 인터넷과 같은 Public Network를 통해서 서로 통신을 할 때 보안적으로 안전하게 통신을 하기 위해 사용하는 프로토콜 입니다.

<br>

### 이미 SSH 가 설정 되어있는지 보려면 ?
```js
ls -a ~/.ssh

// 만약 위의 명렁어를 터미널에 사용해서 아래와 같은 메시지가 나오면 이미 적용 중
id_rsa
id_rsa.pub
```

***
<br>

### SSH key 만들기
1. 구글에서 Git SSH 사이트 들어가기
2. [Generating](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent) 접속
3. OS 에 맞춰서 설명대로 설정

```js
ssh-keygen -t ed25519 -C "your_email@example.com"
```

4. 위의 명렁어 입력하고 쭉 enter 누르기 
5. 생성 완료

<br>

### SSH Agent 를 Background 에 킨다
```js
eval "$(ssh-agent -s)"
```

1. 위의 코드 먼저 사용

![](https://velog.velcdn.com/images/hoho_0815/post/0f67bdee-5d17-4a99-b382-c449e3f8825c/image.png)

2. id_rsa (private 키) , id_rsa.pub ( public 키 ) <br>
-> SSH Agent 에 private 키를 추가를 해줘야 합니다. (아래 명령어 사용) 

```js
ssh-add -K ~/.ssh/id_ed25519
```

3. [SSH public 키를 github 에 연결 해줘야 합니다.](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account)

4. 
```js
pbcopy < ~/.ssh/id_ed25519.pub
```
- 저절로 클립보드에 SSH public 키가 복사가 됩니다.

5. github -> Settings
6. SSH and GPG keys 이동
7. New SSH Key
8. Add SSH Key

![](https://velog.velcdn.com/images/hoho_0815/post/64cf35fe-c846-418c-bc1c-12691e35e1df/image.png)

컴퓨터와 github 가 안전하게 통신 할 수 있습니다.

![](https://velog.velcdn.com/images/hoho_0815/post/35b3e00f-6c77-403b-a81f-c62ee1672920/image.png)


***
<br>

## github 저장소 연결
1. echo "# boiler-plate" >> README.md 
2. git remote add origin https://github.com/Choi-HyunHo/boiler-plate.git
3. git push -u origin main

- 간략화 된 이유는 맨 처음 커밋까지 했기 때문 입니다.

***
<br>

## 참고
- 따라하며 배우는 노드 리액트 기본
# 프로젝트 배포 준비 및 빌드

## 프로젝트 타이틀 변경
![캡처](https://user-images.githubusercontent.com/87301268/163674697-c241f2ca-77a2-4be3-84c9-654b4a05d250.JPG)

- public 폴더 안의 `index.html` 
- 기존 HTML 타이틀 변경과 동일하게 `title` 태그 수정

<br>

## description 수정
```html
<meta
   name="description"
   content="Web site created using create-react-app"
/>
```

- `content` 수정

<br>

## 언어 설정
```html
<html lang="ko">
```
- 한국어 : __ko__
- 영어 : __en__

<br>

## 페이지 마다 타이틀 변경하기
### 리스트를 설명하는 페이지인 경우
```js
 // 타이틀 변경
  useEffect(() => {
    const titleElement = document.querySelector('title');
    titleElement.innerHTML = `감정 일기장 - ${id}번 일기`;
  }, []);
```

![ezgif com-gif-maker (24)](https://user-images.githubusercontent.com/87301268/163675032-4a539a08-5322-4e00-b4bd-33b92ca219cc.gif)

<br>

## 빌드
- `package.json` 파일에 script 부분에 `build` 가 있습니다.
- 명령어를 입력하게 되면, 배포 할 수 있는 압축된 파일을 얻을 수 있습니다.
- 명령어를 입력해서 성공적으로 준비가 되면, __The build folder is ready to be deployed.__ 와 같은 메시지가 나옵니다.
![캡처](https://user-images.githubusercontent.com/87301268/163675357-8afb337a-7502-4c3e-8413-07b105f6138e.JPG)


<br>

### serve -s build
- 위의 명렁어를 통해 배포를 할 수 있습니다. 
![캡처](https://user-images.githubusercontent.com/87301268/163675651-20629677-2e05-454b-b241-3e8341a6d6e4.JPG)
- On Your Network : 는 나와 같은 공유기를 사용하는 사람들이 접속 할 수 있는 주소


<br>

#### serve 명령어가 설치 되어있지 않은 경우
`npm install -g serve` 입력

<br>

#### 빌드 후 에러가 발생하거나, 수정하고 싶은 경우
1. 코드 수정 후 저장
2. 터미널이 동작 하고 있다면, ctrl+c 눌러서 종료
3. 새로고침을 눌렀을 때 접속이 불가능 해야 합니다.
4. `npm run build` 실행
5. `serve -s build` 실행

<br>

## 참고
- [인프런](https://www.inflearn.com/course/%ED%95%9C%EC%9E%85-%EB%A6%AC%EC%95%A1%ED%8A%B8/dashboard)
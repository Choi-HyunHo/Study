# React SPA & CSR

## Routing ?
![KakaoTalk_20220412_144921224](https://user-images.githubusercontent.com/87301268/162889737-aa171115-32bf-4d1c-b4ce-da784f04358c.jpg)

- __경로를 정해주는 행위__ 그 자체

<br>

## 브라우저와 웹 서버
![KakaoTalk_20220412_145747587](https://user-images.githubusercontent.com/87301268/162890740-2d716ef4-a38d-49db-8b81-649e784e736c.jpg)

- 인터넷을 사용해서 웹 사이트에 접속 한다는 것은 브라우저를 통해서 웹 서버에게 경로의 요청을 전달하고, 웹 문서를 받아보는 위와 같은 과정 입니다.
- `/home` 이라는 경로를 요청을 보내게 되면, `/home` 에 해당하는 웹 문서인 `Home.html` 을 웹 서버가 브라우저에게 전달하게 됩니다.

<br>

## Page Routing
__어떠한 요청에 따라서 어떤 페이지를 돌려줄지 결정하는 과정을 일컫는 말 입니다.__
![KakaoTalk_20220412_145747943](https://user-images.githubusercontent.com/87301268/162890748-22a712b1-0a33-4101-b2d7-d65fe013ada2.jpg)

- 현재 `REQUEST` 가 `/home` 이라는 경로로 요청이 들어왔습니다.
- 웹 서버는 경로에 알맞는 페이지인 `Home.html` 를 선별을 해서 돌려주게 됩니다.
- 이렇게 요청에 명시되어 있는 경로에 따라서 알맞은 페이지를 선택하게 하는 과정 자체를
__Page Routing__ 이라고 부릅니다.
- 위와 같이 여러 개의 페이지를 준비해 놓았다가 요청이 들어오면 경로에 따라 적절한 페이지를 보내주는 방식을 __MPA(Multipage Application)__ 라고 부릅니다.

<br>

## MPA ( Multipage Application )
![KakaoTalk_20220412_151456800](https://user-images.githubusercontent.com/87301268/162892987-325d48e0-f7c3-4ac6-b6e5-daf256e21010.jpg)

- MPA 는 페이지가 이동하게 될 때마다 새로운 페이지를 웹 서버에게 요청하고 응답을 받으면 브라우저가 마치 새로고침 되듯이 깜빡이면서 페이지 이동을 하게 됩니다.

![ezgif com-gif-maker (17)](https://user-images.githubusercontent.com/87301268/162892640-d461bbf1-88e7-47da-b9f5-d9a2c0e57196.gif)


- 페이지가 이동 할 때마다 페이지의 아이콘이 깜빡 입니다.
- 이렇게 브라우저가 웹 페이지에 대한 데이터를 받으면 새로 고침되면서 페이지를 이동시키는 방식을 MPA 의 특징이라고 볼 수 있습니다.

<br>

## SPA ( Single Page Application )
![KakaoTalk_20220412_152108570](https://user-images.githubusercontent.com/87301268/162893774-ae26b414-0e0a-42a3-aa6c-eb39eb69720e.jpg)

- SPA 는 말 그래도 단일 페이지 어플리케이션 줄임말로, 
__페이지가 하나 밖에 없는 간단한 웹 어플리케이션__ 이라는 뜻 입니다.
- 만약 `마이페이지` 를 요청해도 `index.html` 를 반환하게 되고, 
`상세 페이지` 를 요청해도 `index.html` 를 반환하게 됩니다.
- 페이지 이동을 하고 싶어서 웹 서버에게 페이지를 요청해도 웹 서버도 전달할게 하나 밖에 없어서 다른 페이지를 줄 수가 없습니다.

<br>

#### React 로 개발된 웹 서비스들은 SPA 이기 때문에 페이지 이동을 정말 할 수 없을까 ??
- 만약 그랬다면 웹 개발에서 React 가 이정도로 많이 사용되지는 않았을 것 입니다.
- React도 자신만의 방식으로 여러 페이지를 이동 할 수 있게 구현할 수 있습니다.

<br>

## React 의 방식
![KakaoTalk_20220412_165149884](https://user-images.githubusercontent.com/87301268/162909765-75d22675-0770-47de-887e-8d75ef0096c7.jpg)

1. React 로 제작된 웹 사이트를 요청하게 되면 SPA 이기 때문에 어떤 페이지를 요청을 하던 똑같인 html 페이지인 `index.html` 를 우선 보내 줍니다.
2. 그 다음 `React App` 을 통째로 던져 줍니다.
3. 만약 이 상황에서 `Post` 라는 페이지로 이동하고 싶어서 어떠한 버튼을 클릭하면 

![KakaoTalk_20220412_165127461](https://user-images.githubusercontent.com/87301268/162909763-6ef6efe9-f637-44bc-aab4-870fdf577dba.jpg)

4. 서버에게 `post` 를 달라하는게 아니고 `React App` 이 알아서 페이지를 업데이트 합니다.
- 페이지 이동이 원래는 서버와 통신을 하는 것이 일반적인데 브라우저가 알아서 혼자 처리하면 되니까, 서버 대기시간이 없을 뿐더러, 바로 페이지가 업데이트 됩니다.

<br>

### 접속 할 때마다 보이는 데이터가 달라지는 경우
![KakaoTalk_20220412_171030002](https://user-images.githubusercontent.com/87301268/162912987-247f00b2-4c0a-49be-8239-d61413978f88.jpg)

- 데이터가 필요한 경우는 서버와 데이터만 요청하고 전달 받습니다.


<br>

## CSR ( Client Side Rendering )
- SPA 에서 브라우저, 즉 클라이언트 측에서 알아서 페이지를 렌더링 하는 방식을 
- 클라이언트 측이 직접 렌더링 한다고 해서 __CSR__ 이라고 부릅니다.

<br>

## 정리
- __React 는 단일 페이지로 구성되는 SPA 방식을 따르면서, CSR 로 페이지를 렌더링 합니다.__

<br>

## 참고
- [인프런](https://www.inflearn.com/course/%ED%95%9C%EC%9E%85-%EB%A6%AC%EC%95%A1%ED%8A%B8/dashboard)
# Styled-Components
- styled components 는 javascript에서 css를 사용 할 수 있도록 도와주는 스타일링 프레임워크입니다.


<br>

## 사용하는 이유
- component 단위 스타일링

-  기존의 className을 사용하지 않는 것과 컴포넌트에 스타일을 적용하여 스타일 코드를 몰아 넣을 수 있으며 공통 코드를 줄이고 React의 컴포넌트 형태로 사용할 수 있습니다.

<br>

## 설치
```js
// styled-components 설치하기
$ npm install --save styled-components
// yarn을 사용한다면
$ yarn add styled-components
```

<br>

## 사용 예시
```js
const StyledDiv = styled.div`
    background-color: black;
    width: 100px;
    height: 100px;
  `;

return <StyledDiv></StyledDiv>
```

<br>

### .js
```js
import styled from "styled-components";
...
const CustomDiv = styled.div`
  ...
`;
```

<br>

### 조건부 스타일링
```js
 const StyledDiv = styled.div`
    background-color: black;
    width: 100px;
    height: 100px;
    ${({ login }) => {
      return login ? `display: none` : null;
    }}
  `;

 return <StyledDiv login={true}></StyledDiv>;
```
- Component의 props를 전달받아 사용하는 것이 가능 합니다. 
- 템플릿 리터럴 내에서 javascript를 사용하는 것과 같은 형식이며, 
내부에서 선언된 함수는 props를 파라미터로 실행 됩니다.

<br>

### 확장 스타일링
```js
const Container = styled.div`
    max-width: 600px;
    width: 100%;
    height: 100px;
  `;

const BlackContainer = styled(Container)`
    background-color: black;
  `;

const RedContainer = styled(Container)`
    background-color: red;
  `;

return (
    <>
      <BlackContainer />
      <RedContainer />
    </>
  );
```

<br>

### 중첩 스코프
```js
const StyledDiv = styled.div`
    background-color: black;
    width: 100px;
    height: 100px;
    p {
      color: white;
    }
  `;

  return (
    <>
      <StyledDiv>
        <p>Title</p>
      </StyledDiv>
      <p>content</p>
    </>
  );
```

<br>


## 연습
### App.js
```js
import "./styles.css";
import styled from "styled-components";

import Container from "./Container";
import Button from "./Button";

const ContainerStyle = styled.div`
  margin: 20px;
  padding: 20px;
  background-color: #bdc3c7;
`;

const Custom = styled.h6`
  color: red;
`;

export default function App() {
  return (
    <ContainerStyle>
      <h2>hello</h2>
      <Custom>
        <Container />
      </Custom>
      <Button />
    </ContainerStyle>
  );
}
```

<br>

### Container.js
```js
const Container = () => {
  return <h2>My name is Hello</h2>;
};

export default Container;
```

<br>

### Button.js
```js
import styled from "styled-components";

const ButtonStyle = styled.button`
  border: 2px solid palevioletred;
  border-radius: 3px;
`;

const Button = () => {
  return (
    <ButtonStyle>
      <button>normal</button>
    </ButtonStyle>
  );
};

export default Button;
```

![캡처](https://user-images.githubusercontent.com/87301268/173235382-1ad1fb12-fb5f-4634-98c8-ad6cd090e0fd.JPG)


<br>

## 참고
- https://dkje.github.io/2020/10/13/StyledComponents/
- https://xtring-dev.tistory.com/entry/Styling-Styled-Components-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B3%A0-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0-%F0%9F%92%85
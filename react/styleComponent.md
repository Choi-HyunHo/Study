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

***
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


***
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

***
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
***
<br>

### as
```js
import React from 'react';
import styled from 'styled-components';

const Btn = styled.button`
  color : red;
  border-radius : 15px;
`

function App() {
  return (
    <React.Fragment>
      <Btn />
      <Btn as='a'/>
    </React.Fragment>
  );
}

export default App;
```


![](https://velog.velcdn.com/images/hoho_0815/post/657a8171-430f-4b95-8448-c06c41ca7c58/image.png)

- 만약 버튼 대신에 href 같은 걸 만들고 싶다면 위와 같이 `as` 를 사용 할 수 있습니다.
- 즉, `as` 는 button styled component 인 Btn 을 사용하는데, HTML 부분을 바꿔서 a 태그로 전달
- 이는, `<div as='header'>` 이런식으로 다른 태그도 가능 합니다.

***
<br>

### attr
```js
import React from 'react';
import styled from 'styled-components';

const Input = styled.input.attrs({required : true, minLength : 10})`
`

function App() {
    return (
        <React.Fragment>
            <Input/>
            <Input/>
            <Input/>
            <Input/>
            <Input/>
        </React.Fragment>
    );
}

export default App;
```

![](https://velog.velcdn.com/images/hoho_0815/post/d0c059f0-7269-4fe7-a571-3e3339ee3d20/image.png)

- 스타일 컴포넌트에서 HTML 태그의 속성을 설정하는 방법 입니다.
- `.attrs({})` 안에 해당 태그의 속성을 사용 할 수 있습니다.


***
<br>

### Animation & 선택자
```js
import styled, { keyframes } from "styled-components";

const Wrapper = styled.div`
  display: flex;
`;

const rotationAnimation = keyframes`
  0% {
    transform:rotate(0deg);
    border-radius:0px;
  }
  50% {
    border-radius:100px;
  }
  100%{
    transform:rotate(360deg);
    border-radius:0px;
  }
`;

const Box = styled.div`
  height: 200px;
  width: 200px;
  background-color: tomato;
  display: flex;
  justify-content: center;
  align-items: center;
  animation: ${rotationAnimation} 1s linear infinite;
  span {
    font-size: 36px;
    &:hover {
      font-size: 48px;
    }
    &:active {
      opacity: 0;
    }
  }
`;

function App() {
  return (
    <Wrapper>
      <Box>
        <span>🤩</span>
      </Box>
    </Wrapper>
  );
}
```
- 스타일 컴포넌트에서는 keyframes helper를 사용시 앱 전체에서 사용할 수 있는 고유한 인스턴스를 생성
- 다른 파일에서 같은 이름의 keyframes가 존재하더라도 이름 충돌이 나지 않도록 해줍니다.
- 또한 선택자들을 자유롭게 사용 할 수 있습니다.

***
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

***
<br>

## 응용
```js
const Line = styled.div`
    height : 2px;
    margin : 2px 0;
    width : 100%;
    ${({ boxColor }) => {
        const activeStyled = css`
            background : ${(props) => props.boxColor == props.number ? '#ED5255 ￼' : '#F2F2F2 ￼'};
        `;
        const nonActiveStyled = css`
            background-color : #F2F2F2 ￼;
        `;

        return boxColor ? activeStyled : nonActiveStyled;
    }}
`

const BoxOne = styled.div`
    height : 56px;
    min-width : 55px;
    ${({ boxColor }) => {
        const activeStyled = css`
            background : ${(props) => props.boxColor == props.number ? '#ED5255 ￼' : '#F2F2F2 ￼'};
        `;
        const nonActiveStyled = css`
            background-color : #F2F2F2 ￼;
        `;

        return boxColor ? activeStyled : nonActiveStyled;
    }}

`

const BoxTwo = styled.div`
    height : 100%;
    display : flex;
    flex-direction : column;
    justify-content : center;
    align-items : center;
`

const TitleText = styled.span`
    font-weight : 500;
    ${theme.fontSize.caption}
    ${({ boxColor }) => {
        const activeStyled = css`
            color : ${(props) => props.boxColor == props.number ? '#FFFFFF ￼' : '#9E9E9E ￼'};
            margin-bottom : ${(props) => props.boxColor == props.number ? '6px' : ''};
        `;
        const nonActiveStyled = css`
            color: #9E9E9E ￼;

        `;

        return boxColor ? activeStyled : nonActiveStyled;
    }}
`
```

***
<br>

## Themes
기본적으로 모든 색상들을 가지고 있는 object 입니다.

<br>

### ThemeProvider 
- index.js 에서 감싸줍니다. 그리고 props 를 전달 합니다.


```js
import React from "react";
import ReactDOM from "react-dom";
import { ThemeProvider } from "styled-components";
import App from "./App";

const darkTheme = {
  textColor: "whitesmoke",
  backgroundColor: "#111",
};

const lightTheme = {
  textColor: "#111",
  backgroundColor: "whitesmoke",
};

ReactDOM.render(
  <React.StrictMode>
    <ThemeProvider theme={darkTheme}>
      <App />
    </ThemeProvider>
  </React.StrictMode>,
  document.getElementById("root")
);
```

- App 이 ThemeProvider 안에 있기 때문에, 컴포넌트에서 바로 접근이 가능 합니다.

<br>

```js
import styled from "styled-components";

const Title = styled.h1`
  color: ${(props) => props.theme.textColor};
`;

const Wrapper = styled.div`
  display: flex;
  height: 100vh;
  width: 100vw;
  justify-content: center;
  align-items: center;
  background-color: ${(props) => props.theme.backgroundColor};
```

***
<br>

## 참고
- https://dkje.github.io/2020/10/13/StyledComponents/
- https://xtring-dev.tistory.com/entry/Styling-Styled-Components-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B3%A0-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0-%F0%9F%92%85
# React + TypeScript(interface)
기본적인 타입 스크립트에 대한 설명은 별도의 포스팅을 통해 정리를 했습니다. <br>
여기서는 리액트에서 타입 스크립트를 어떻게 적용 할 수 있는지 공부하고 정리 합니다!

<br>

## 설치
### 1. 처음부터 CRA + 타입 스크립트를 할 때
```js
npx create-react-app my-app --template typescript

or

yarn create react-app my-app --template typescript
```

<br>

### 2. 프로젝트 중간에 설치하는 방법
```js
npm install --save typescript @types/node @types/react @types/react-dom @types/jest

or

yarn add typescript @types/node @types/react @types/react-dom @types/jest
```

- [문서](https://create-react-app.dev/docs/adding-typescript/)
- 그 후, .js 파일을 -> .tsx 로 바꿉니다.
- 만약, style-component 가 타입스크립트를 받아 들이지 못해 에러를 내고 있다면, 아래의 명령어를 입력 합니다.

```js
npm i --save-dev @types/styled-components

or

yarn add @types/styled-components
```

***
<br>

## Type the Props
#### App.tsx
```js
import Circle from "./Circle";

function App() {
    return (
        <div>
            <Circle bgColor={'teal'}/>
            <Circle bgColor={'tomato'}/>
        </div>
    );
}

export default App;
```

#### Circle.tsx
```js
import styled from "styled-components"

const Container = styled.div`
    
`;


const Circle = (bgColor) => {
    return (
        <Container/>
    )
}

export default Circle;
```
- 위와 같이 사용하면, 타입스크립트는 __bgColor__ 의 타입을 알 수가 없어 에러를 나타냅니다.

<br>

### interface
객체의 모양을 TypeScrip에게 설명해주는 개념 <br> 
이전에는 아래와 같은 코드 형식으로 많이 사용 했습니다.

```js
const x = (a:number, b:number) => a + b;
```
- TypeScript 에게 변수 a, b 의 타입을 설명해 주고 있습니다.

<br>

### interface 사용법
```js
import styled from "styled-components"


const Container = styled.div`
    
`;

interface CircleProps {
    // TypeScript 에게 bgColor 는 문자열이라고 알림
    bgColor : string;
}

const Circle = (bgColor : CircleProps) => {
    return (
        <Container bgColor={bgColor}/>
    )
}

export default Circle;
```
- 하지만 아직도 에러가 발생 합니다 `<Container bgColor={bgColor}/>` bgColor 부분에서
- TypeScript 가 봤을 때는 Cotainer 가 div 입니다.
- TypeScript 에게 bgColor 를 styled-component 에게 보내고 싶다고 전달해야 합니다.

<br>

### style-component 에 보낼 interface 생성
```js
import React from "react";
import styled from "styled-components"

interface ContainerProps {
    bgColor : string;
}

const Container = styled.div<ContainerProps>`
    width : 200px;
    height : 200px;
    background-color : ${(props) => props.bgColor};
`;

interface CircleProps {
    // TypeScript 에게 bgColor 는 문자열이라고 알림
    bgColor : string;
}

const Circle = ({bgColor} : CircleProps) => {
    return (
        <React.Fragment>
            <Container bgColor={bgColor}/>;
        </React.Fragment>
    )
}

export default Circle;
```

![](https://velog.velcdn.com/images/hoho_0815/post/4525ab4b-09e3-42e5-87fe-d343050c7d9a/image.png)

***
<br>

## interface 연습
```js
interface PlayerShape {
    name : string;
    age : number;
}

const sayHello = (playerObj : PlayerShape) => 
    `Hello ${playerObj.name} you are ${playerObj.age} years old`;

sayHello({name:'hyunho' , age : 26});
sayHello({name:'hyunho' , age : '26'}); // 에러가 발생 합니다.
```

- 두 번째에 에러가 발생하는 이유는 __age__ 의 타입은 string 이 아니라 number 입니다.
- 이렇듯 interface 는 코드 실행 '전' 에 확인해서 에러를 막아 줍니다.


***
<br>

## 참고
- 노마드코더
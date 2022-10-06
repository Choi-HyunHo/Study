# React + TypeScript(Optional Props)

## default props
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

<br>

#### Circle.tsx
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
            <Container bgColor={bgColor}/>
        </React.Fragment>
    )
}

export default Circle;
```

- 만약 App.js 에서 
```js
import Circle from "./Circle";

function App() {
    return (
        <div>
            <Circle />
            <Circle bgColor={'tomato'}/>
        </div>
    );
}

export default App;
```

- 이렇게만 되어도, 에러가 발생 합니다.
- 이유는 현재 Circle 컴포넌트에서 bgColor 는 필수 입니다.
- 만약, 필수가 아닌 선택적으로 만들고 싶다면 ??

***
<br>

## optional props
ex) 두 개의 원이 있는데 그 중 하나의 원에만 border-color 를 가지게 하고 싶다.

<br>

#### Circle.tsx
```js
interface CircleProps {
    bgColor : string;
    borderColor : string;
}

const Circle = ({bgColor} : CircleProps) => {
    return (
        <React.Fragment>
            <Container bgColor={bgColor}/>
        </React.Fragment>
    )
}
```


![](https://velog.velcdn.com/images/hoho_0815/post/bfbfdff7-24e7-44a2-8ffb-651e36c29336/image.png)

- App.js 에서 에러가 발생합니다.
- 이유는, App.js 에서 borderColor 를 보내는 것을 안했기 때문입니다.
- 그래서 할 것은 필수가 아닌 선택적으로 만들어 주는 것 입니다.

<br>

### ? (optional)
```js
interface CircleProps {
    bgColor : string;
    borderColor? : string;
}
```

![](https://velog.velcdn.com/images/hoho_0815/post/e7128859-1428-4722-af06-f2a4ffd75204/image.png)


- ? 를 붙이면 선택적이게 변합니다. (App.js 에서 에러가 사라집니다.)
- borderColor 는 string 또는 undefined 라고 합니다.

<br>

### 하지만
```js
const Circle = ({bgColor, borderColor} : CircleProps) => {
    return (
        <React.Fragment>
            <Container bgColor={bgColor} borderColor={borderColor}/>
        </React.Fragment>
    )
}
```
- 위와 같은 경우에도 Container 는 borderColor 를 몰라서 에러를 나타냅니다.
- 마찬가지로 Container 의 interface 에 알려줘야 합니다.

<br>

### Container - interface (style-component)

```js
interface ContainerProps {
    bgColor : string;
    borderColor : string;
}
```
- 하지만 추가만 한다고 에러가 사라지지 않습니다.
- 이유는, Circle 안에서는 borderColor 가 필수가 아니라 선택적이기 때문입니다.

<br>

### 이를 중재하기 위해서
#### App.tsx

```js
import Circle from "./Circle";

function App() {
    return (
        <div>
            <Circle bgColor={'teal'} borderColor={'yellow'}/>
            <Circle bgColor={'tomato'}/>
        </div>
    );
}

export default App;
```
- 첫 원에만 borderColor 를 보내도 에러가 뜨지 않는 이유는,
- 필수 요소인 bgColor 는 둘 다 보내주고 있고
- borderColor 는 string 또는 undefined 가 될 수 있게 선택적으로 바뀔 수 있으니까

<br>

#### Circle.tsx
```js
const Circle = ({bgColor, borderColor} : CircleProps) => {
    return (
        <React.Fragment>
            <Container bgColor={bgColor} borderColor={borderColor ?? 'white'}/>
        </React.Fragment>
    )
}
```
- 한쪽 원에서는 borderColor 가 'yellow' 가 될 것이라는 걸 알고 있습니다.
- 하지만 한쪽에서는 undefined 가 될 수 있다는 걸 알고 있습니다.
- __이를 해결하기 위해 TypeScript 에게 초기값을 줄 수 있습니다.__
- `??` 를 사용해서 'borderColor 는 사용자가 만든 값을 사용하며, 만약 undefined 상태라면 초기값을 사용해라` 라고 전달 합니다
- 이렇게 되면, 더 이상 에러가 발생하지 않습니다.

![](https://velog.velcdn.com/images/hoho_0815/post/aa255f6a-8caa-4db6-8290-fc69f0860317/image.png)

***
<br>

## 전체 코드
### App.tsx
```js
import Circle from "./Circle";

function App() {
    return (
        <div>
            <Circle bgColor={'teal'} borderColor={'yellow'}/>
            <Circle bgColor={'tomato'}/>
        </div>
    );
}

export default App;
```

<br>

### Circle.tsx
```js
import React from "react";
import styled from "styled-components"

interface ContainerProps {
    bgColor : string;
    borderColor : string;
}

const Container = styled.div<ContainerProps>`
    width : 200px;
    height : 200px;
    border-radius : 50%;
    background-color : ${(props) => props.bgColor};
    border : 5px solid ${(props) => props.borderColor};
`;

interface CircleProps {
    bgColor : string;
    borderColor? : string;
}

const Circle = ({bgColor, borderColor} : CircleProps) => {
    return (
        <React.Fragment>
            <Container bgColor={bgColor} borderColor={borderColor ?? 'white'}/>
        </React.Fragment>
    )
}

export default Circle;
```

***
<br>

## 연습
```js
interface CircleProps {
    bgColor : string;
    borderColor? : string;
    text? : string;
}

const Circle = ({bgColor, borderColor, text="default Text"} : CircleProps) => {
    return (
        <React.Fragment>
            <Container bgColor={bgColor} borderColor={borderColor ?? 'white'}>
                {text}
            </Container>
        </React.Fragment>
    )
}
```
- 만약 props 로 text 를 받지 않지만, 화면에 출력하고 있을 때는
- 위의 방식 처럼 초기값을 줄 수 있습니다.

![](https://velog.velcdn.com/images/hoho_0815/post/a4abccec-6e73-4ec0-a40c-9edbba917de9/image.png)


***
<br>

## 참고
- 노마드코더
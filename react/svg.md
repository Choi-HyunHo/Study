# React - SVG 파일 사용법
- svg는 png와 다르게 아이콘의 색, 크기 등 요소를 디자인에 따라 바꿀 수 있는 파일 입니다.
- 또한 용량이 매우 작아서 프로젝트 관리가 용이 합니다.
- React.js 와 Next.js 방법은 차이가 존재 합니다.

<br>

## 사용 예시
### React
- 파일 경로 : public/svg/icon.svg ( 예시 )
```html
<svg width="46" height="46" viewBox="0 0 46 46" fill="current"xmlns="http://www.w3.org/2000/svg">
 <line x1="8" y1="35" x2="38" y2="35" stroke="current" stroke-width="4" stroke-linecap="round"/>
 <rect x="7" y="10" width="32" height="18" rx="1" fill="current" stroke="current" stroke-width="2"/>
```
- fill 과 stroke 가 current 로 되어있는데 아래에서 설명 하겠습니다.

<br>

#### 1. img src 
```js
import Icon from 'public/svg/icon.svg';

<img src={Icon}/>
```

<br>

#### 2. svg 컴포넌트로
```js
import { ReactComponent as icon } from 'public/svg/icon.svg';

<Icon />;
```

<br>

### svg 색, 크기 바꾸기
- 예시는 스타일 컴포넌트를 사용을 하였습니다.
- 아래 방법은 svg 코드를 자바스크립트로 가져와서 컴포넌트 시킨 방법 입니다.
- 다른 파일에서 `<Icon/>` 붙여서 사용 할 수 있습니다. 
- 우선 위에서 봤던 fill, 과 stroke 부분을 `current` 로 변경 합니다.
- 이렇게 사용하면 크기 또는 색상을 바꿀 수 있습니다.
```js
import styled from "styled-components"
​
const Icon = styled.svg`
    cursor : pointer;
    fill : #DEDEDE;
    stroke : #DEDEDE;
    &:active{
        fill : #7F7F7F;
        stroke : #7F7F7F;
    }
`;
​
const Icon = () => {
    return(
        <Icon width="46" height="46" viewBox="0 0 46" fill="current" xmlns="http://www.w3.org/2000/svg">
            <line x1="898" y1="80" x2="80" y2="890" stroke="current" stroke-width="4" stroke-linecap="round"/>
            <rect x="17" y="80" width="392" height="168" rx="1" fill="current" stroke="current" stroke-width="2"/>
        </Icon>
    )
}
​
export default Icon;
```


<br>

## 참고

- https://pjainxido.github.io/Next-SVG/
- https://kyounghwan01.github.io/blog/React/handling-svg/#next%E1%84%8B%E1%85%A6%E1%84%89%E1%85%A5-svg-%E1%84%89%E1%85%A1%E1%84%8B%E1%85%AD%E1%86%BC%E1%84%92%E1%85%A1%E1%84%80%E1%85%B5
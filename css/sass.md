# Sass(SCSS)

 CSS 전처리기로서 CSS의 한계나 단점을 보완하기 위해 만들어졌습니다.<br>
 
 SASS 에 SCSS가 포함되어 있는데, SCSS는 SASS 3번째 버전에서 추가되었고,<br>
 SASS의 모든 기능을 지원하면서 CSS 구문과 완전히 호환되도록 만들어졌습니다.

<br>

## 주요 기능
<br>

### 변수 설정
- `$`표시를 사용하여 변수명을 지정할 수 있습니다.
```css
$hongsi-1: 1;
$HONGSI_2 : 2;
$hongsi: 'hongsi';
```
- 반복적으로 사용되는 폰트나 색상 등을 변수로 지정해 간단하게 제어할 수 있습니다.

- 규칙
    - 문자를 넣을 때 '' 사용
    -  유의사항 : 알파벳, -, _, 123 사용 가능
    - 대소문자 구분
    - 숫자로 변수명 시작 X

<br>

### Nesting(중첩)
- SASS에서는 Nesting 기능을 사용해 상위 선택자의 반복을 피할 수 있습니다. 
- 또한 html 구조를 반영해서 코드를 작성할 수 있기 때문에 CSS의 구조를 명확하게 파악할 수 있습니다.
```css
body {
  background-color: $bg;
}

.box {
  margin-top: 50px;
  &:hover {
    background-color: green;
  }
  h2 {
    color: red;
  }
  button {
    color: yellowgreen;
  }
}
```
- Nesting 기능을 이용한 중첩 안에 `&` 를 사용하여 부모 참조 선택자를 사용할 수 있습니다.

<br>

### Mixin
- 재사용성이 핵심 
- `@mixin`으로 재사용할 스타일을 정의하고 필요한 부분에서 `@include`를 통해 불러옵니다.
```css
@mixin title {
  color: blue;
  font-size: 30px;
  margin-bottom: 12px;
}
-----------------------------

@import '_mixin.scss';

body {
  background-color: $bg;
}

h1 {
  @include title();
}
```

<br>

### 조건문과 반복문
- if문과 비슷하게 `@if`, `@else`를 사용하여 조건에 따라 CSS를 반환 합니다.
- for문과 비슷하게 `@for` `through`와 `to`를 사용하여 반복문을 만들 수 있습니다.
- while문과 비슷하게 `@while`을 사용하여 조건이 거짓이 될 때까지 내용을 반복할 수 있습니다.
```css
//기본 @if 지시문 
@if(조건식) { 
    //조건이 참일 때 표현식 
} 

//@if @else문 
@if(조건식) { 
    //조건이 참일 때 표현식 
} @else { 
    //조건이 거짓일 때 표현식 
} 

//다중 @if문 
@if(조건식1) { 
    조건식1이 참일 때 표현식 
} @else if(조건식2) { 
    //조건식2가 참일 때 표현식 
} @else { 
    //조건식이 모두 거짓일 때 표현식
}


// through - 시작부터 종료조건까지 반복 
@for $변수 from 시작 through 종료 { 
    // 반복내용
}; 
    
// to - 시작부터 종료조건 직전까지 반복 
@for $변수 from 시작 to 종료 { 
    //반복내용 
};

@while 조건 {
	//반복 내용
}
```


<br>

## 참고
- [Sass 문법_조건문과 반복문](https://www.biew.co.kr/entry/Sass%E3%86%8DSCSS-SASS-%EB%AC%B8%EB%B2%95-3%ED%8E%B8-%EC%A1%B0%EA%B1%B4%EB%AC%B8if-%EB%B0%98%EB%B3%B5%EB%AC%B8for)
- https://heropy.blog/2018/01/31/sass/
- [코드 안식처](https://blog.naver.com/x7788/222503550817)
- [김버그의 HTML&CSS는 재밌다](https://edu.goorm.io/lecture/20583/%EA%B9%80%EB%B2%84%EA%B7%B8%EC%9D%98-html-css%EB%8A%94-%EC%9E%AC%EB%B0%8C%EB%8B%A4)
# Vue

MVVM 패턴의 뷰모델(ViewModel) 레이어에 해당하는 화면(View)단 라이브러리<br>

MVVM : 모델, 뷰, 뷰 모델로 이루어진 패턴

<br>

## Vue 구조도

![캡처](https://user-images.githubusercontent.com/87301268/161428409-de7231b1-ca67-4547-97c1-cb721e63216f.JPG)


- 가장 왼쪽의 `View`는 브라우저에서 사용자에게 비춰지는 화면을 의미 합니다.
     - HTML 이라 부르고, DOM 을 이용해 조작을 할 수 있도록 구성되어 있습니다.

- 사용자가 키보드나 마우스를 사용하여 이벤트 발생 시 `DOM Listeners` 로 `Vue` 에서 보게 됩니다.

- `Vue` 에서 이벤트를 받아 -> 자바스크립트에 있는 데이터를 바꿔주거나, 특정 로직에서 실행을 하게 됩니다.

- 이벤트가 변경되면 `Data Bindings` 을 이용해서 자바스크립트의 데이터가 변경되어 화면까지 동시에 반영 됩니다.( Reactivity )

<br>

## 기존 웹 개발 방식 (HTML + JS)
```js
<body>
    <div id="app">
      
    </div>
    <script>
        let div = document.querySelector('#app');
        let str = 'hello world'
        div.innerHTML = str; // 화면에 출력

        // 화면을 바꾸려면
        str = 'hello world!!'; // 화면에 반영되지 않는다.
        div.innerHTML = str; // 기존의 접근 방식 - 일반적인 개발 방식 (HTML + CSS + JS)
    </script>
</body>
```
- 바뀐 데이터를 다시 `DOM` 에 대입해야 바뀌는게 기존의 개발 방식

<br>

## Vue의 특징 - 반응성(Reactivity)
- `vue` 의 핵심은 데이터의 변화를 라이브러리에서 감지해서 화면을 그려주는 것
- 즉, __데이터의 변화에 따라 화면에 그려주는 것__

<br>

### Object.defineProperty API 사용
```js
Object.defineProperties(대상객체, 객체의 속성, {
    // 정의할 내용
 }) // 객체의 특정 속성의 동작을 재정의하는 api
```
```js
<body>
    <div id="app"></div>

    <script>
        let div = document.querySelector("#app");
        let viewModel = {};

        Object.defineProperty(viewModel, 'str', {
            // 속성에 접근했을 때의 동작을 정의
            get: function(){
                console.log('접근');
            },
            // 속성에 값을 할당했을 때의 동작을 정의
            set: function(newValue){
                console.log('할당', newValue);
                div.innerHTML = newValue;
            }
        })
    </script>
</body>
```

<br>

### 위의 코드를 라이브러리 처럼
```js
(function(){
            
        function init(){
            Object.defineProperty(viewModel, 'str', {
            // 속성에 접근했을 때의 동작을 정의 (viewModel.str)
            get: function(){
                console.log('접근');
            },
            // 속성에 값을 할당했을 때의 동작을 정의 (viewModel.str = newValue)
            set: function(newValue){
                console.log('할당', newValue);
                render(newValue)
            }
          })
        }
        // viewModel.str = newValue 의 값이 -> value 안으로
        function render(value){
            div.innerHTML = value;
        }

    init();
        
})();
```
- 즉시 실행함수를 사용한 이유는 라이브러리 내부 로직이<br>사용자에게 노출되지 않도록 변수 유효 범위를 분리하기 위해서입니다.

- 파일을 실행하고 나서 `render(), init()` 를 호출하면 호출이 되지 않습니다.


<br>

## 참고
- [인프런](https://www.inflearn.com/course/age-of-vuejs)
- https://mkil.tistory.com/515
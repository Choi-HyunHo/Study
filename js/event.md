# 이벤트 버블링, 이벤트 캡처링, 이벤트 위임

브라우저는 이벤트가 발생하면, capturing -> bubbling 순으로 이벤트를 감지 합니다.

<br>

## 이벤트 버블링 (Event Bubbling)
- 이벤트 버블링은 특정 화면 요소에서 이벤트가 발생했을 때 해당 이벤트가 더 상위의 요소에게 전달되어 가는 특성을 의미합니다.

>> 상위의 화면 요소란? HTML 요소는 기본적으로 트리 구조를 갖습니다. <br> 
여기서는 트리 구조상으로 한 단계 위에 있는 요소를 상위 요소라고 하며 body 태그를 최상위 요소라고 부르겠습니다.

<img width="459" alt="event-bubble" src="https://user-images.githubusercontent.com/87301268/160237587-1c99d687-3dea-4e7d-83fb-bf2acc0d7ae4.png">

```html
<body>
	<div class="one">
		<div class="two">
			<div class="three">
			</div>
		</div>
	</div>
</body>
```
```js
const one = document.querySelector(".one");
const two = document.querySelector(".two");
const three = document.querySelector(".three");


one.addEventListener("click", function () {
  console.log("one");
});

two.addEventListener("click", function () {
  console.log("two");
});

three.addEventListener("click", function () {
  console.log("three");
});
```
![캡처](https://user-images.githubusercontent.com/87301268/160238477-40781047-2a88-42c4-a91b-df0042d5245d.JPG)
- 가장 하위 태그인 `three` 를 클릭 했을 때 결과 입니다.
- 즉, 한 요소에 이벤트가 발생하면, 이 요소에 할당된 핸들러가 동작하고, 이어서 부모 요소의 핸들러가 동작 합니다. 

- 가장 최상단의 부모 요소를 만날 때까지 이 과정이 반복되면서 요소 각각에 할당된 핸들러가 동작 합니다.

<br>


## 버블링에서 주의해야 할 점은
- 부모/자식 구조에서 동일한 이벤트만 bubbling 이 됩니다.

- 부모에게는 `mousedown` 이벤트, 자식에게는 `click` 이벤트를 등록해두고, 자식을 클릭하면 부모의 `mousedown` 이벤트는 동작하지 않습니다.

<br>

## 이벤트 캡쳐 (Event Capture)
- 이벤트 캡쳐는 이벤트 버블링과 반대 방향으로 진행되는 이벤트 전파 방식입니다.

<img width="459" alt="event-capture" src="https://user-images.githubusercontent.com/87301268/160238897-bd803416-2126-4377-8317-5e2a25c76fc0.png">

```html
<body>
	<div class="one">
		<div class="two">
			<div class="three">
			</div>
		</div>
	</div>
</body>
```
```js
const one = document.querySelector(".one");
const two = document.querySelector(".two");
const three = document.querySelector(".three");


one.addEventListener("click", function () {
  console.log("one");
},true);

two.addEventListener("click", function () {
  console.log("two");
},true);

three.addEventListener("click", function () {
  console.log("three");
},true);
```
![캡처](https://user-images.githubusercontent.com/87301268/160239082-294a7548-acb1-4a16-b010-76b4221b10fa.JPG)

- addEventListener() API에서 옵션 객체에 capture:true를 설정해주면 됩니다.<br> 그러면 해당 이벤트를 감지하기 위해 이벤트 버블링과 반대 방향으로 탐색합니다.

<br>

## event.stopPropagation()
- 이벤트 전달 방식을 신경 쓰지 말고, 원하는 화면 요소이 이벤트만 신경 쓰고 싶을 때 주로 사용 합니다.
```js
function logEvent(event) {
	event.stopPropagation();
}
```
- 위 API는 해당 이벤트가 전파되는 것을 막습니다.
- 이벤트 버블링의 경우에는 클릭한 요소의 이벤트만 발생시키고 상위 요소로 이벤트를 전달하는 것을 방해합니다. 
- 이벤트 캡쳐의 경우에는 클릭한 요소의 최상위 요소의 이벤트만 동작시키고 하위 요소들로 이벤트를 전달하지 않습니다.

<br>

## 이벤트 위임 (Event Delegation)
- 하위 요소에 각각 이벤트를 붙이지 않고 상위 요소에서 하위 요소의 이벤트들을 제어하는 방식
- 즉, 여러 엘리먼트마다 각각 이벤트 핸들러를 할당하지 않고,
<br>__공통되는 부모에 이벤트 핸들러를 할당하여 이벤트를 관리하는 방식__ 입니다.

<br>

### 각 하위 요소에게 이벤트를 따로 지정하는 경우(위임X)
```html
<ul>
  <li>1</li>
  <li>2</li>
  <li>3</li>
  <li>4</li>
  <li>5</li>
</ul>
```
```js
const list = document.querySelectorAll('li')
list.forEach(li =>{
    li.addEventListener('click',()=>{
        li.classList.add('click')
    })
})
```
<br>

### 공통되는 부모에게 이벤트 지정 (위임)
```js
const ul = document.querySelector('ul')

ul.addEventListener('click',(event)=>{
  if(event.target.tagName = 'li'){
  event.target.classList.add('click')
  })
```
<br>

## 이벤트 위임을 하는 이유 ?
- 공통적으로 무언가를 처리해야 할 때, 일일이 이벤트 리스너를 자식에 추가하는 것 보다 효율적 입니다.<br>
( 특히 자식요소가 많아 질 수록 )

- 동적인 element에 대한 이벤트 처리가 수월 합니다.
- 메모리 사용량이 줄어듭니다.
- 메모리 누수 가능성이 줄어듭니다.



<br>

## 참고
- [MDN, event_bubbling](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Building_blocks/Events#Event_bubbling_and_capture)
- [캡틴판교](https://joshua1988.github.io/web-development/javascript/event-propagation-delegation/)
- https://gingerkang.tistory.com/117
- https://ui.toast.com/weekly-pick/ko_20160826
- [이벤트 위임](https://velog.io/@moonheekim0118/JavaScript-%EC%9D%B4%EB%B2%A4%ED%8A%B8-%EB%B2%84%EB%B8%94%EB%A7%81)
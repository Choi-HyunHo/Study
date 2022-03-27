# class

ES6 부터 추가 되었습니다.

<br>

## 정의
- 함수의 한 종류이며, 객체를 생성하기 위한 템플릿(틀) 입니다.
- 클래스 안에는 속성(field), 행동(method) 이 있습니다.

- class 는 객체의 __데이터__ 와 이를 조작하는 __메서드__ 를 하나로 추상화 합니다.

<br>

## 문법
```js
class Person{ 
  constructor(name, age){
     // 속성
     this.name = name; // 객체를 초기화 하기 위한 값
     this.age = age;
  }
  // 행동
  speak(){
     console.log(`${this.name} : hello`)
  }
}
// object , 새로운 object를 만들 때 new 키워드를 사용 합니다.

const hyunho = new Person('hyunho', 26)
console.log(hyunho.name); // hyunho 출력
console.log(hyunho.age);  // 26 출력

hyunho.speak(); // hyunho : hello 출력
```

- `class` 키워드를 사용
- 내부에 `constructor` 위치 합니다. -> 객체를 만들어주는 생성자 메서드 입니다.
>>  객체(object) : 물리적으로 존재하거나 추상적으로 생각 할 수 있는 것 중에서 자신의 속성을 가지고 있고 다른 것과 식별 가능한 것


- `new` 와 함께 호출하지 않으면 에러가 발생 합니다.

<br>

## Getter and Setter

### 현재 상황
```js
class User {
  constructor(firstName, lastName, age) {
    this.firstName = firstName;
    this.lastName = lastName;
    this.age = age;
  }
}

const user1 = new User("hyunho", "choi", -1);
console.log(user1);
```
- 사람의 나이가 -1 라는 것은 불가능 합니다.
- 위의 예시 처럼 클래스를 사용하는 사용자가 잘못 사용을 하였을 때<br> 방어적인 자세로 바꿔주는 것이 `getter` 와 `setter` 입니다.

- `get` : 값을 리턴 합니다.
- `set(value)` : 값을 설정 합니다. set은 값을 설정하기 때문에 value를 받아야 합니다.

<br>

### 하지만 단순히 `get` 과 `set` 을 사용하면 오류가 발생 합니다.
```js
class User {
  constructor(firstName, lastName, age) {
    this.firstName = firstName;
    this.lastName = lastName;
    this.age = age;
  }
  get age(){
    return this.age;
  }
  set age(value){
    this.age = value;
  }
}
```
<br>

### 오류가 일어나는 이유

![KakaoTalk_20220115_234613008](https://user-images.githubusercontent.com/87301268/160279876-de787649-eb1e-4676-8689-64535c6bd6ea.jpg)

- age 라는 get 를 정의하는 순간 this.age는 메모리에 올라가 있는 데이터를 읽어오는 것이 아니라 바로 get 호출 ( 주황색 박스 보기 )

- set 을 정의하는 순간 ( 보라색 박스 보기 ) = age 를 호출 할 때 즉. 값을 할당 할 때 바로 메모리의 값을 할당하는 것이 아니라 set 안에서 지속적인 호출이 일어납니다.

<br>

### 알맞게 사용하는 방법
```js
class User {
  constructor(firstName, lastName, age) {
    this.firstName = firstName;
    this.lastName = lastName;
    this.age = age;
  }
  get age() {
    return this._age;
  }
  set age(value) {
    this._age = value;
  }
}

const user1 = new User("hyunho", "choi", -1);
console.log(user1);
```
-  get 과 set 안에서 쓰여지는 변수 이름을 조금 다르게 해줘야합니다. ( 보통 _ 사용하여 조금 다르게 합니다. )

<br>


## 상속
- __공통 요소를 번거롭게 작성할 필요없이 동일한 것들을 계속 재사용 할 수 있습니다.__

- `extends` 키워드 사용 
```js
class Shape {
  constructor(width, height, color) {
    this.width = width;
    this.height = height;
    this.color = color;
  }
  draw() {
    console.log(`drawing ${this.color} color`);
  }
  area() {
    return this.width * this.height;
  }
}
class Rectangle extends Shape {}

const rectangle = new Rectangle(10, 10, "red");
rectangle.draw(); // drawing red color 출력
```
- class Rectangle extends Shape {} 한 줄로 인하여 shape 에 있는 모든 것들이 rectangle 에 포함이 됩니다.

<br>

## 다형성 ( 오버라이딩 )
- __필요한 함수만 재정의해서 사용할 수 있습니다.__
- 부모 메서드를 토대로 일부 기능만 변경하고 싶을 때, 또는 부모의 메서드의 기능을 확장하고 싶을 때 사용 합니다.

- 오버라이딩 된 함수가 호출되면 이전에 존재하던 함수는 호출하지 않게 됩니다. ( 재정의한 함수만 호출 )

- 하지만, 오버라이딩한 함수도 출력하고 공통적으로 정의한 함수도 호출하고 싶다면 이럴 때, 부모의 함수를 호출하는 `super` 를 사용 합니다.

<br>

### 도형의 너비를 구하는 상황을 가정
```js
class Shape {
  constructor(width, height, color) {
    this.width = width;
    this.height = height;
    this.color = color;
  }
  draw() {
    console.log(`drawing ${this.color} color`);
  }
  area() {
    return this.width * this.height;
  }
}
class Rectangle extends Shape {}
class Triangle extends Shape {}

const rectangle = new Rectangle(10, 10, "red");
console.log(rectangle.area()); // 100
const triangle = new Triangle(20, 20, "blue");
console.log(triangle.area()); // 400
```
- 사각형의 너비는 가로 * 세로 이므로 올바르게 출력 됩니다.

- 하지만, 삼각형의 너비는 가로 * 세로 인가? 아닙니다. `(가로*세로) / 2 ` 이렇게 되야 합니다.

<br>

### 재정의
```js
class Shape {
  constructor(width, height, color) {
    this.width = width;
    this.height = height;
    this.color = color;
  }
  draw() {
    console.log(`drawing ${this.color} color`);
  }
  area() {
    return this.width * this.height;
  }
}
class Rectangle extends Shape {}
class Triangle extends Shape {
  area() {
    return (this.width * this.height) / 2; // 재정의
  }
}

const rectangle = new Rectangle(10, 10, "red");
console.log(rectangle.area()); // 100
const triangle = new Triangle(20, 20, "blue");
console.log(triangle.area()); // 200
```
<br>

### 예시 2
```js
class Shape {
  constructor(width, height, color) {
    this.width = width;
    this.height = height;
    this.color = color;
  }
  draw() {
    console.log(`drawing ${this.color} color`);
  }
  area() {
    return this.width * this.height;
  }
}
class Rectangle extends Shape {}
class Triangle extends Shape {
  draw() {
    console.log(`Good`); // good 출력, 재정의 되었기 때문에 
                 //부모의 console.log(`drawing ${this.color} color`); 출력하지 않습니다. 
  }
  area() {
    return (this.width * this.height) / 2;
  }
}

const rectangle = new Rectangle(10, 10, "red");
console.log(rectangle.area());
const triangle = new Triangle(20, 20, "blue");
console.log(triangle.area());
triangle.draw();
```
- 하지만 `부모의 draw()` 을 같이 출력하고 싶다면?

<br>

### `super` 사용
```js
class Shape {
  constructor(width, height, color) {
    this.width = width;
    this.height = height;
    this.color = color;
  }
  draw() {
    console.log(`drawing ${this.color} color`);
  }
  area() {
    return this.width * this.height;
  }
}
class Rectangle extends Shape {}
class Triangle extends Shape {
  draw() {
    super.draw();
    console.log(`Good`);
  }
  area() {
    return (this.width * this.height) / 2;
  }
}

const rectangle = new Rectangle(10, 10, "red");
console.log(rectangle.area());
const triangle = new Triangle(20, 20, "blue");
console.log(triangle.area());
triangle.draw();
```
![캡처](https://user-images.githubusercontent.com/87301268/160281163-22957c1f-1e23-48ae-997a-083b72e4200f.png)


<br>

## 참고
- https://ordinary-code.tistory.com/22
- [자바스크립트의 class vs function](https://yoon-dumbo.tistory.com/entry/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8%EC%9D%98-class-vs-function-%EB%8F%84%EB%8C%80%EC%B2%B4-%EC%96%B8%EC%A0%9C-%EC%96%B4%EB%96%A4%EA%B1%B8-%EC%84%A0%ED%83%9D%ED%95%B4%EC%95%BC%ED%95%98%EB%8A%94%EA%B1%B0%EC%95%BC-When-to-use-class-vs-function-in-javascript)
- [자바스크립트 클래스와 객체 총정리](https://velog.io/@younoah/JS-%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%ED%81%B4%EB%9E%98%EC%8A%A4%EC%99%80-%EA%B0%9D%EC%B2%B4-%EC%B4%9D%EC%A0%95%EB%A6%AC)
- [코드 안식처](https://blog.naver.com/x7788/222622811512)
- [드림코딩](https://www.youtube.com/watch?v=_DLhUBWsRtw&list=PLv2d7VI9OotTVOL4QmPfvJWPJvkmv6h-2&index=6)
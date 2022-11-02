# Spread 연산자

객체의 값을 새로운 객체에 펼쳐주는 역할  
배열에서도 사용 가능

<br>

## 예시
### 중복된 속성을 작성해야 하는 상황

```
const cookie = {
    base : "cookie",
      madeIn : "korea"
};

const chocoCookie = {
      base : "cookie",
      madeIn : "korea",
      toping : "choco"
};

const bananaCookie = {
    base : "cookie",
      madeIn : "korea",
    toping : "banana"
};
```

<br>

### Spread 연산자 사용

```
const cookie = {
    base : "cookie",
      madeIn : "korea"
};

const chocoCookie = {
      ...cookie,
      toping : "choco"
};

const bananaCookie = {
    ...cookie,
    toping : "banana"
};

console.log(chocoCookie);
```

![캡처](https://user-images.githubusercontent.com/87301268/168015461-a5cb65dc-724f-4954-b9ef-8f4057ed4976.JPG)

<br>

## 참고

-   [인프런](https://www.inflearn.com/course/%ED%95%9C%EC%9E%85-%EB%A6%AC%EC%95%A1%ED%8A%B8/dashboard)
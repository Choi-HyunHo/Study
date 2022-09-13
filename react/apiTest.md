# React에서 API 호출

https://jsonplaceholder.typicode.com/ 을 사용

- 무료로 API 서비스를 제공해서 Test 를 할 수 있습니다

![캡처](https://user-images.githubusercontent.com/87301268/162377991-87b57d9f-9869-4647-b10b-4eea90222a03.JPG)

- 사용자가 어떤 API 로 자원을 가져다 쓸 수 있는지 목록
- 여기서는 `/comments` 사용
- API 를 호출하려면 해당 API의 주소를 알아야 하는데 위의 리소스를 클릭하고 들어간 URL 입니다.
( https://jsonplaceholder.typicode.com/comments )


## App.js
- 호출하는 시점 : App.js 컴포넌트가 Mount 하면 호출
```js
function App() {
  
const getData = async () => {
    const res = await fetch(
      'https://jsonplaceholder.typicode.com/comments'
    ).then((res) => res.json());
    console.log(res); // 500 개의 데이터 확인 가능
  };

  // Mount 시점에 수행
  useEffect(() => {
    getData();
  }, []);
```

### getData 함수에서 가져온 데이터를 사용
```js
function App() {
  // API 호출 함수
  const getData = async () => {
    const res = await fetch(
      'https://jsonplaceholder.typicode.com/comments'
    ).then((res) => res.json());
    console.log(res);

    const initData = res.slice(0, 20).map((it) => {
      return {
        author: it.email,
        content: it.body,
        emotion: Math.floor(Math.random() * 5) + 1,
        create_date: new Date().getTime(),
        id: dataId.current++,
      };
    });

    setData(initData);
  };

  useEffect(() => {
    getData();
  }, []);
```
 - 가져와서 사용할 데이터 ( 0 ~ 19 인덱스 까지 잘라옵니다. )
 - map 을 이용해 배열의 각각 모든 요소를 순회해서 
 - map 함수의 콜백함수 안에서 return 하는 값들을 모아서 새로운 배열을 만들어서
 - initData 에 넣습니다.

![ezgif com-gif-maker (8)](https://user-images.githubusercontent.com/87301268/162386317-03efe777-d949-4d88-a74a-cedab83b675c.gif)



<br>

## 참고
- https://www.daleseo.com/js-window-fetch/
- [인프런](https://www.inflearn.com/course/%ED%95%9C%EC%9E%85-%EB%A6%AC%EC%95%A1%ED%8A%B8/dashboard)
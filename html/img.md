# 이미지 태그에 srcset 속성을 사용하는 이유
srcset 속성은 사용자의 디스플레이 사양에 따라 다른 해상도의 이미지를 제공함으로써 사용자 경험을 높이기 위해 사용 합니다.<br> 
고사양 디스플레이에는 고사양 이미지를 제공하고 저사양 기기에는 저해상도 이미지를 제공하는 방식 입니다.

<br>

## srcset
: 같은 비율의 다양한 크기를 가지는 동일 이미지 2개 이상 명시하는 속성 입니다.

<br>

## srcset 사용
1. 브라우저가 사용할 이미지들과 그 이미지들의 원본크기를 지정(이미지 목록)
2. 작은 크기 이미지부터 순서대로 입력
3. px단위가 아닌 w디스크립터(width) 혹은 x디스크립터(배수 2x, 3x..) 입력

<br>

## sizes
: 미디어조건과 그 조건에 해당하는 이미지의 ‘최적화 출력 크기’ 를 지정 합니다.

<br>

## 예제
```html
<img
  srcset="images/heropy_small.png 400w, <!--뷰포트 너비가 400px 이하일 때 heropy_small.png(400px)가 사용됩니다.-->
          images/heropy_medium.png 700w, <!-- 뷰포트 너비가 401~700px 일 때 heropy_medium.png(700px)가 사용됩니다. -->
          images/heropy_large.png 1000w  <!-- 뷰포트 너비가 701~999px 일 때 heropy_large.png(1000px)가 사용됩니다. --> 
          " 
  sizes="(min-width: 1000px) 700px <!--뷰포트 너비가 1000px 이상일 때 heropy_medium.png(700px)가 사용됩니다.> " 
  src="images/heropy.png"
  alt="HEROPY" />

--------------------------------------------------

  <img
  srcset="경로 원본크기,
          경로 원본크기,
          경로 원본크기"
  sizes="(미디어조건) 최적화출력크기"
  src="경로"
  alt="대체텍스트" />

```
- srcset 속성은 쉼표(,)로 구분, 사용할 이미지들의 경로와 해당 이미지의 원본 크기를 지정

- sizes 속성은 쉼표(,)로 구분, 미디어조건(선택적)과 그에 따라 최적화되어 출력될 이미지 크기를 지정


<br>

## 참고
- https://heropy.blog/2019/06/16/html-img-srcset-and-sizes/
- https://codingcoding.tistory.com/386
- https://usingu.co.kr/references/html/img-%ec%9d%b4%eb%af%b8%ec%a7%80%ec%82%bd%ec%9e%85/srcset/

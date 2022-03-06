# DOCTYPE
Document Type의 약어입니다.
<br>

### DOCTYPE 선언이란?
- __웹 브라우저에게 해당 문서의 HTML 버전을 알려주는 역할을 합니다.__
- DTD(Document Type Declaration)를 통해서 현재의 웹 문서가 어떤 버전의 HTML 기술로 작성되어 있는지 웹 브라우저에게 전달합니다.
>>> DTD : 문서별로 해당 문서에 대한 태그 정보가 다르면 가독성 과 파악하기 어렵기 때문에, 공통 규칙에 따라 문서를 작성하게 하는 것 입니다.

<br>

### 선언의 목적
- __문서간의 호환성을 높이기 위함 입니다.__
- HTML 문서 버전의 종류에는 HTML5, HTML4 이하 및 XHTML이 있습니다. 사용 용도와 발전 기간에 따라 버전이 달라졌습니다.
<br>
(DOCTYPE에 대하여 알아보는 것이므로 각각 HTML 버전에 대한 자세한 설명은 생략 하겠습니다.)
-  HTML 버전마다 적용되는 태그가 있고, 적용되지 않는 태그가 있습니다. 만약 선언없이 구버전과 신버전 HTML을 사용한다면, 웹브라우저는 현재 버전의 엄격한 기준으로 과거 버전에서 정상적으로 작성된 태그들을 문법 오류로 간주하는 오류를 범할 것입니다.

<br>

### 선언 종류
#### HTML5
```html
<!DOCTYPE html>
```

#### HTML4.01
```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">
-> strict(엄격모드), 웹 표준을 엄격하게 지키는 버전

<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
-> Transitional(호환모드), 표준이 정립되지 않은 때에 만들어진 문서들과의 호환성을 유지하기 위해 만들어진 문서 타입

<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Frameset//EN" "http://www.w3.org/TR/html4/frameset.dtd">
-> 프레임셋을 지원하기 위한 문서타입
```
- XHTML1.0 또한 HTML4.01 마찬가지로 위와 동일하게 나뉘어 집니다.




<br>

### 특징
- HTML은 `<html>, <head>, <body>` 로 구성되어 있습니다. 그리고 가장 최상단 `<!DOCTYPE html...>` 있습니다.
- DOCTYPE는 `<html>` 태그 보다 먼저 선언하는 것이 일반적이며, 태그가 아니기 때문에 </> 과 같이 닫아주지 않아도 됩니다.


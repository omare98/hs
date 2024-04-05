## USTRA HR 자바스크립트 코딩 스타일 가이드

### HTML 기본구조

```html
<!DOCTYPE html>
<html lang="ko"
      xmlns:th="http://www.thymeleaf.org"
      xmlns:layout="http://www.ultraq.net.nz/thymeleaf/layout"
      layout:decorate="~{layout/framelayout}">

<!-- 컨텐츠 : start -->
<div layout:fragment="content">
    <div class="page-body">
        // body 영역
    </div>

    <script type="application/javascript">
        // javascript 영역
    </script>
</div>
<!-- 컨텐츠 : end -->
</html>
```

### Javascript 구성

* 변수 및 상수영역, 함수영역, 이벤트 영역으로 분리하여 작성한다.

```js
<script type="application/javascript">
    Ustra.docReady(function () {
        init();
    }, document);
    
    /************************************************************************************
     * 변수, 상수 영역
     * 전역 변수 또는 상수들을 정의한다.
     ************************************************************************************/
    const a = '';
    let b;
    ...
    ...
    /************************************************************************************
     * 함수 영역
     * 기능 함수들을 정의한다.
     ************************************************************************************/
    function init() {
    };
    ...
    ...
    /************************************************************************************
     * 이벤트 영역
     * 이벤트들을 정의한다.
     ************************************************************************************/
    $('#btnSearch').on({
        click: (e) => {
            search();
        }
    });
</script>
```

### Coding style

* [Airbnb JavaScript 스타일 가이드](https://github.com/tipjs/javascript-style-guide) 기준으로 작성한다,

### 주석

* 주석은 스크립트의 어느 곳에나 작성할 수 있다. 자바스크립트 엔진은 주석을 무시하기 때문에 주석의 위치는 실행에 영향을 주지 않는다.
* 한 줄짜리 주석은 두 개의 슬래시 //로 시작된다.
* 여러 줄의 주석은 슬래시와 별표 /*로 시작해 별표와 슬래시 */로 끝난다.
* 함수는 [JSDoc](https://jsdoc.app/) 형태의 주석형식으로 작성한다.
* 함수의 주석은 반드시 ```/** ... */``` 사이에 기술해야 한다.

```js
/**
 * a와 b를 더한 결과를 반환
 * @param {number} a 첫번째 숫자
 * @param {number} b 두번째 숫자
 * @returns {number} a와 b를 더한 결과
 */
const plus = (a, b) => {
  return a + b;
};
```

### Lodash

* Ustra-HR 시스템은 Lodash 라이브러리를 사용하므로 되도록이면 Lodash 가 제공하는 _.isEmpty(), _.filter(), _find() 등의 함수들을 적극활용하여 개발한다.
* [Lodash API](https://lodash.com/docs/4.17.15)
# 24장 클로저

클로저는 자바스크립트 고유의 개념이 아닌 **함수형 프로그래밍 언어** 에서 사용되는 중요한 특성이다.

MDN에서는 "클로저는 함수와 그 함수가 선언된 렉시컬 환경과의 조합이다." 라고 정의되어 있다.

**렉시컬 스코프란?**
<br/>
자바스크립트 엔진은 함수를 어디서 호출했는지가 아니라 **어디서 정의했는지**에 따라 **상위 스코프를 결정** 한다.

```javascript
const x = 1;

function outerFunc() {
  const x = 10;

  function innerFunc() {
    console.log(x); // 10
  }

  innerFunc();
}

outerFunc();
```

```javascript
const x = 1;

function outerFunc() {
  const x = 10;
  innerFunc();
}

function innerFunc() {
  console.log(x); // 1
}

outerFunc();
```

<br/>

## 클로저와 렉시컬 환경

```javascript
const x = 1;

// ①
function outer() {
  const x = 10;
  const inner = function () {
    console.log(x);
  }; // ②
  return inner;
}

// outer 함수를 호출하면 중첩 함수 inner를 반환한다.
// 그리고 outer 함수의 실행 컨텍스트는 실행 컨텍스트 스택에서 팝되어 제거된다.
const innerFunc = outer(); // ③
innerFunc(); // ④ 10
```

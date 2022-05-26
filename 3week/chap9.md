# 09장 타입 변환과 단축 평가

## 9.1 타입 변환이란?

- 자바스크립트의 모든 값은 타입이 있다. 값의 타입은 개발자의 의도에 따른 다른 타입으로 변환할 수 있다. 이러한 변환을 **_명시적 타입 변환 또는 타입 캐스팅_** 이라한다.

- 개발자의 의도와는 상관없이 표현식을 평가하는 도중에 자바스크립트 엔진의 의해 암묵적으로 타입이 변환기도 하는데 이를 **_암묵적 타입 변환 또는 타입 강제 변환이라 한다._**

```javascript
var x = 10;

// 명시적 타입 변환
// 숫자를 문자열로 타입 캐스팅한다.
var str = x.toString();
console.log(typeof str, str); // string 10

// x 변수의 값이 변경된 것은 아니다.
console.log(typeof x, x); // number 10

var x = 10;

// 암묵적 타입 변환
// 문자열 연결 연산자는 숫자 타입 x의 값을 바탕으로 새로운 문자열을 생성한다.
var str = x + "";
console.log(typeof str, str); // string 10

// x 변수의 값이 변경된 것은 아니다.
console.log(typeof x, x); // number 10
```

- 명시적 타입 변환이나 암묵적 타입 변환이 기존 원시 값을 직접 변경하는 것은 아니다. 원시 값은 변경 불가능한 값이므로 변경할 수 없다. 타입 변환이란 기존 원시 값을 사용해 **_다른 타입의 새로운 원시 값을 생성_**하는 것이다.

- 암묵적 타입 변환은 기존 변수 값을 재할당하여 변경하는 것이 아니다. 자바스크립트 엔진은 표현식을 에러 없이 평가하기 위해 피연산자의 값을 **_암묵적 타입으로 변환해 새로운 타입의 값을 만들어 단 한 번 사용하고 버린다._**

## 9.2 암묵적 타입 변환

- 암묵적 타입 변환이 발생할 때 타입별로 암묵적 타입 변환이 어떻게 발생하는지 살펴보자.

### 9.2.1 문자열 타입으로 변환

```javascript
1 + "2"; // -> "12"
```

- 위의 예제를 + 연산자는 피연산자 중 하나 이상이 문자열이므로 문자열 연결 연산자로 동작한다. 왜냐하면 문자열 연결 연산자의 모든 피연산자는 코드 문맥상 모두 문자열 타입이어야 하기 때문에 자바스크립트 엔진은 이를 암묵적 타입 변환한다.

- 연산자 표현식의 피연산자만이 암묵적 타입 변환이 대상이 되는 것은 아니다. ES6에서 도입된 템플릿 리터럴의 표현식 삽입은 표현식의 평가 결과를 문자열 타입으로 암묵적 타입 변환한다.

```javascript
`1 + 1 = ${1 + 1}`; // -> "1 + 1 = 2"
```

### 9.2.2 숫자 타입으로 변환

- 산술 연산자의 역할은 숫자 값을 만드는 것이다. 따라서 산술 연산자의 모든 피연산자는 코드 문맥상 모두 숫자 타입이어야 한다. 이에 자바스크립트 엔진은 산술 연산자 표현식을 평가하기 위해 피연산자를 숫자 타입으로 암묵적으로 타입 변환한다.

- 피연산자를 숫자 타입으로 변환해야 할 문맥은 산술 연산자뿐만 아니라 비교 연산자, **_+단항연산자_** 등이 있다.

```javascript
// 문자열 타입
+"" + // -> 0
  "0" + // -> 0
  "1" + // -> 1
  "string" + // -> NaN
  // 불리언 타입
  true + // -> 1
  false + // -> 0
  // null 타입
  null + // -> 0
  // undefined 타입
  undefined + // -> NaN
  // 심벌 타입
  Symbol() + // -> ypeError: Cannot convert a Symbol value to a number
  // 객체 타입
  {} + // -> NaN
  [] + // -> 0
  [10, 20] + // -> NaN
  function () {}; // -> NaN
```

### 9.2.3 불리언 타입으로 변환

- 제어문 또는 삼항 조건 연산자의 조건식은 불리언 값, 즉 논리적 참/거짓으로 평가되어야 하는 표현식이다. 자바스크립트 엔진은 조건식의 평가 결과를 **_불리언 타입으로 암묵적 변환한다._**

```javascript
if ("") console.log("1");
if (true) console.log("2");
if (0) console.log("3");
if ("str") console.log("4");
if (null) console.log("5");

// 2 4
```

- 이 떼, 자바스크립트 엔진은 불리언 타입이 아닌 값을 Truthy 값 또는 Falsy 값으로 구분하며 아래 값들은 false로 평가된다.

  - false
  - undefined
  - null
  - 0, -0
  - NaN
  - ''(빈문자열)

## 9.3 명시적 타입 변환

- 개발자의 의도에 따라 명시적으로 타입을 변경하는 방법은 다양하다.

  - 표준 빌트인 생성자 함수(String,Number,Boolean)를 new 연산자 없이 호출하는 방법

  - 빌트인 메서드를 사용하는 방법

  - 암묵적 타입 변환 방법

### 9.3.1 문자열 타입으로 변환

- 문자열 타입이 아닌 값을 문자열 타입으로 변환하는 방법

  - String생성자 함수를 new 연산자 없이 호출하는 방법

  - Object.prototype.toString 메서드를 사용하는 방법

  - 문자열 연결 연산자를 이용하는 방법

```javascript
// 1. String 생성자 함수를 new 연산자 없이 호출하는 방법
// 숫자 타입 => 문자열 타입
String(1); // -> "1"
String(NaN); // -> "NaN"
String(Infinity); // -> "Infinity"
// 불리언 타입 => 문자열 타입
String(true); // -> "true"
String(false); // -> "false"

// 2. Object.prototype.toString 메서드를 사용하는 방법
// 숫자 타입 => 문자열 타입
(1).toString(); // -> "1"
NaN.toString(); // -> "NaN"
Infinity.toString(); // -> "Infinity"
// 불리언 타입 => 문자열 타입
true.toString(); // -> "true"
false.toString(); // -> "false"

// 3. 문자열 연결 연산자를 이용하는 방법
// 숫자 타입 => 문자열 타입
1 + ""; // -> "1"
NaN + ""; // -> "NaN"
Infinity + ""; // -> "Infinity"
// 불리언 타입 => 문자열 타입
true + ""; // -> "true"
false + ""; // -> "false"
```

### 9.3.2 숫자 타입으로 변환

- 숫자 타입이 아닌 값을 숫자 타입으로 변환하는 방법

  - Number 생성자 함수를 new 연산자 없이 호출하는 방법

  - parseInt, parseFloat 함수를 사용하는 방법(문자열만 숫자 타입으로 변환 가능)

  - - 단항 산술 연산자를 이용하는 방법

  - - 산술 연산자를 이용하는 방법

```javascript
// 1. Number 생성자 함수를 new 연산자 없이 호출하는 방법
// 문자열 타입 => 숫자 타입
Number("0"); // -> 0
Number("-1"); // -> -1
Number("10.53"); // -> 10.53
// 불리언 타입 => 숫자 타입
Number(true); // -> 1
Number(false); // -> 0

// 2. parseInt, parseFloat 함수를 사용하는 방법(문자열만 변환 가능)
// 문자열 타입 => 숫자 타입
parseInt("0"); // -> 0
parseInt("-1"); // -> -1
parseFloat("10.53"); // -> 10.53

// 3. + 단항 산술 연산자를 이용하는 방법
// 문자열 타입 => 숫자 타입
+"0"; // -> 0
+"-1"; // -> -1
+"10.53"; // -> 10.53
// 불리언 타입 => 숫자 타입
+true; // -> 1
+false; // -> 0

// 4. * 산술 연산자를 이용하는 방법
// 문자열 타입 => 숫자 타입
"0" * 1; // -> 0
"-1" * 1; // -> -1
"10.53" * 1; // -> 10.53
// 불리언 타입 => 숫자 타입
true * 1; // -> 1
false * 1; // -> 0
```

### 9.3.3 불리언 타입으로 변환

- 불리언 타입이 아닌 값을 불리언 타입으로 변환하는 방법

  - Boolean 생성자 함수를 new 연산자 없이 호출하는 방법

  - ! 부정 논리 연산자를 두 번 사용하는 방법

```javascript
// 1. Boolean 생성자 함수를 new 연산자 없이 호출하는 방법
// 문자열 타입 => 불리언 타입
Boolean("x"); // -> true
Boolean(""); // -> false
Boolean("false"); // -> true
// 숫자 타입 => 불리언 타입
Boolean(0); // -> false
Boolean(1); // -> true
Boolean(NaN); // -> false
Boolean(Infinity); // -> true
// null 타입 => 불리언 타입
Boolean(null); // -> false
// undefined 타입 => 불리언 타입
Boolean(undefined); // -> false
// 객체 타입 => 불리언 타입
Boolean({}); // -> true
Boolean([]); // -> true

// 2. ! 부정 논리 연산자를 두번 사용하는 방법
// 문자열 타입 => 불리언 타입
!!"x"; // -> true
!!""; // -> false
!!"false"; // -> true
// 숫자 타입 => 불리언 타입
!!0; // -> false
!!1; // -> true
!!NaN; // -> false
!!Infinity; // -> true
// null 타입 => 불리언 타입
!!null; // -> false
// undefined 타입 => 불리언 타입
!!undefined; // -> false
// 객체 타입 => 불리언 타입
!!{}; // -> true
!![]; // -> true
```

## 9.4 단축 평가

- 단축 평가는 표현식을 평가하는 도중에 평가결과가 확정된 경우 나머지 평가 과정을 생략하는 것으르 말한다.

- 논리곱(&&)연산자와 논리합(||)연산자는 논리 연산의 결과를 결정하는 피연산자 타입 변환하지 않고 그대로 반환한다. 이를 단축 평가라고 한다.

```javascript
// 논리합(||) 연산자
"Cat" || "Dog"; // -> "Cat"
false || "Dog"; // -> "Dog"
"Cat" || false; // -> "Cat"

// 논리곱(&&) 연산자
"Cat" && "Dog"; // -> "Dog"
false && "Dog"; // -> false
"Cat" && false; // -> false
```

- 단축 평가를 사용하면 if문을 대체할 수 있다. 어떤 조건이 Truthy 값일 때 무언가를 해야한다면 논리곱연산자 표현식으로 if문을 대체할 수 있다.

```javascript
var done = true;
var message = "";

// 주어진 조건이 true일 때
if (done) message = "완료";

// if 문은 단축 평가로 대체 가능하다.
// done이 true라면 message에 '완료'를 할당
message = done && "완료";
console.log(message); // 완료
```

- 단축 평가를 사용하면 if문을 대체할 수 있다. 어떤 조건이 Falsy 값일 때 무언가를 해야한다면 논리합연산자 표현식으로 if문을 대체할 수 있다.

```javascript
var done = false;
var message = "";

// 주어진 조건이 false일 때
if (!done) message = "미완료";

// if 문은 단축 평가로 대체 가능하다.
// done이 false라면 message에 '미완료'를 할당
message = done || "미완료";
console.log(message); // 미완료
```

- 추가로 삼항 조건 연산자는 if ... else문을 대체할 수 있다.

```javascript
var done = true;
var message = "";

// if...else 문
if (done) message = "완료";
else message = "미완료";
console.log(message); // 완료

// if...else 문은 삼항 조건 연산자로 대체 가능하다.
message = done ? "완료" : "미완료";
console.log(message); // 완료
```

단축 평가는 다음과 같은 상황에서 유용하게 사용된다.

- 객체를 가리키기를 기대하는 변수가 null 또는 undefined가 아닌지 확인하고 프로퍼티를 참조할 때

- 함수 매개변수에 기본값을 설정할 때

### 9.4.2 옵셔널 체이닝 연산자

- ES11에서 도입된 옵셔널 체이닝 연산자는 ?.는 좌항의 피연산자가 null
  또는 undefined인 경우 undefined를 반환하고, 그렇지 않으면 우항의 프로퍼티 참조를 이어간다.

### 9.4.3 null 병합 연산자

- ES11에서 도입된 null 병합 연산자는 ??는 좌항의 피연산자가 null 또는 undefined인 경우 우항의 피연산자를 반환하고, 그렇지 않으면 좌항의 피연산자를 반환한다. null 병합 연산자 ??는 변수에 기본값을 설정할 때 유용하다.

# 06장 데이터 타입

- 자바스크립트의 모든 값은 데이터 타입을 가지며 ES6에서는 7개의 데이터 타입을 제공한다.

## 6.1 숫자 타입

- C나 자바의 경우 정수와 실수를 구분해서 숫자 타입이 제공되지만 자바스크립트는 하나의 숫자 타입만 존재한다.
- 모든 수를 실수로 처리하며, 정수만 표현하기 위한 데이터 타입이 별도로 존재하지 않는다. > **_이는 정수로 표시된다 해도 사실은 실수라는 것을 의미한다._**
- 자바스크립트는 2진수,8진수,16진수를 표현하기 위한 데이터 타입을 제공하지 않기 때문에 값을 참조하면 모두 10진수로 해석된다.
- 추가로 세가지의 특별한 값으로 표현할 수 있다.

  - Infinity : 양의 무한대
  - -Infinity : 음의 무한대
  - NaN 산술 연산 불가(not-a-number)

## 6.2 문자열 타입

- 텍스트 데이터를 나타내는 데 사용
- ' '," " 또는 백틱``으로 텍스트를 감싼다.
- 위의 특징은 자바스크립트 엔진이 키워나 식별자로 구분하는 것을 방지 하기 위함이다.
- C는 문자열 타입을 제공하지 않고 문자의 배열로 문자열을 표현하고, 자바는 문자열을 객체로 표현한다.
- 문자열은 원시 타입이며, 변경 불가능한 값이다.

## 6.3 템플릿 리터럴

- ES6부터 템플릿 리터럴이라고 하는 새로운 문자열 표기법이 도입되었다. 템플릿 리터럴은 멀티라인 문자열, 표현식 삽입, 태그드 템플릿 등 편리한 문자열 처리 기능을 제공한다.
  템플릿 리터럴은 런타이에 **_일반 문자열로 변환되어 처리된다._**
- 템플릿 리트럴은 백틱을 사용해 표현한다.

### 6.3.1 멀티라인 문자열

- 일반 문자열 내에서는 줄바꿈(개행)이 허용되지 않는다.
- 테플릿 리터럴에서는 이스케이프 시퀀스(백슬레시로시작)를 사용하지 않고도 줄바꿈이 허용되며 모든 공백도 표현 가능하다.

### 6.3.2 표현식 삽입

- 문자열은 연산자+를 사용해 연결할 수 있다. +연산자는 피연산자 중 **_하나 이상이 문자열인 경우 문자열 연결 연산자로 동작한다._** 그 외의 경우 덧셈 연산자로 동작

```
var first = 'Ung-mo'
var last = 'Lee'

console.log('My name is' + first + ' '+ last )
// My name is Ung-mo Lee
```

```
var first = 'Ung-mo'
var last = 'Lee'

console.log(`My nme is ${first} ${last}` )
// My name is Ung-mo Lee
```

### 6.4 불리언 타입

- 논리적 참, 거짓을 나타내는 true와 false뿐이다.

### 6.5 undefined 타입

- 변수를 선언한 이후 값을 할당하지 않은 변수를 참조하면 undefined가 반환.
- 개발자가 의도적으로 할당하기 위한 값이 아니라 자바스크립트 엔진이 변수를 초기화 할 때 사용하는 값이다.
- 변수에 값이 없다는 것을 명시하고 싶을 때는 null을 할당한다.

### 6.6 null 타입

- null값이 유일하며, 대소문자 구분.
- 변수에 값이 없다는 것을 의도적으로 명시할 때 사용하며 이전에 할당되어 있던 값에 대한 참조를 명시적으로 제거하는 것을 의미한다. > 누구도 참조하지 않는 메모리 공간에 대해 가비지 콜렉션이 수행될 것이다.
- **_함수가 유효한 값을 반환할 수 없는 경우 명시적으로 null을 반환하기도 한다._**

### 6.7 심벌 타입

- 심벌은 ES6에서 추가된 7번째 타입으로 변경 불가능한 원시 타입의 값이다.
- 심벌 값은 다른 값과 중복되지 않는 유일무이한 값이다. 따라서 주로 이름이 충동할 위험이 없는 객체의 유일한 프로퍼티 키를 만들기 위해 사용한다.
- 심벌은 Symbol 함수를 호출해 생성하며, 값은 외부에 노출되지 않고 다른 값과 절대 중복되지 않는 유일무이한 값이다.

### 6.8 객체 타입

- 자바스크립트의 데이터 타입은 크게 원시 타입과 객체타입으로 분류된다.
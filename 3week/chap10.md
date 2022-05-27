# 10장 객체 리터럴

## 10.1 객체란?

- 자바스크립트는 객체 기반의 프로그래밍 언어이며, 자바스크립트를 구성하는 거의 "모든 것"이 객체다. 원시 값을 제외한 나머지 값(함수, 배열, 정규 표현식 등)은 모두 객체다.

- 원시 타입은 단 하나의 값만 나타내지만 객체 타입은 다양한 타입의 값을 **_하나의 단위로 구성한 복합적인 자료구조이다._**

- 또한 원시 타입의 값, 즉 원시 값은 변경 불가능한 값이지만 **_객체 타입의 값, 즉 객체는 변경 가능한 값이다._**

- 객체는 0개 이상의 프로퍼티로 구성된 집합이며, 프로퍼티는 키와 값으로 구성된다. 자바스크립트에서 사용 할 수 있는 모든 값은 프로퍼티 값이 될 수 있다. 프로퍼티 값이 함수일 경우 일반 함수와 구분하기 위해 메서드라 부른다.

  - 프로퍼티 : 객체의 상태를 나타내는 값(data)
  - 메서드 : 프로퍼티를 참조하고 조작할 수 있는 동작(behavior)

- 이처럼 객체는 객체의 상태를 나타내는 값과 프로퍼티를 참조하고 조작할 수있는 동작을 모두 포함할 수 있기 때문에 상태와 동작을 하나의 단위로 구조화할 수 있어 유용하다.

## 10.2 객체 리터럴에 의한 객체 생성

- c++나 자바 같은 클래스 기반 객체지향 언어는 클래스를 사전에 정의하고 필요한 시점에 new 연산자와 함께 생성자를 호출하여 인스턴스 (클래스에 의해 생성되어 메모리에 저장된 실체) 를 생성하는 방식으로 객체를 생성한다.

- 자바스크립트는 프로토타입 기반 객체지향 언어로서 클래스 기반 객체지향 언어와는 달리 다양한 객체 생성 방법을 지원한다.

  - 객체 리터럴 (가장 일반적이고 간단한 방법)
  - object 생성자 함수
  - 생성자 함수
  - object.create 메서드
  - 클래스(ES6)

- 객체 리터럴은 객체를 생성하기 위한 표기법이고 객체 리터럴은 중괄호{...} 내에 0개 이상의 프로퍼티를 정의한다. 변수에 할당되는 시점에 자바스크립트 엔진은 객체 리터럴을 해석해 객체를 생성한다.

- 객체 리터럴의 중괄호는 코드 블록을 의미하지 않고 값으로 평가되는 표현식이기 때문에 코드 블록과 달리 세미콜론을 붙인다.

- 객체 리터럴은 자바스크립트의 유연함과 강력함을 대표하는 객체 생성 방식이다. 객체를 생성하기 위해 클래스를 먼저 정의하고 new 연산자와 함께 생성자를 호출할 필요가 없다.

- 리터럴로 객체를 생성하고 프로퍼티를 포함시켜 객체를 생성함과 동시에 프로퍼티를 만들 수도 있고, 객체를 생성한 이후에 프로퍼티를 동적으로 추가할 수도 있다.

## 10.3 프로퍼티

- 객체는 프로퍼티의 집합이며, 프로퍼티는 키와 값으로 구성된다.

```javascript
var person = {
  // 프로퍼티 키는 name, 프로퍼티 값은 'Lee'
  name: "Lee",
  // 프로퍼티 키는 age, 프로퍼티 값은 20
  age: 20,
};
```

- 프로퍼티는 쉼표로 구분하며, 프로퍼티 키는 빈 문자열을 포함하는 **_모든 문자열 또는 심벌 값_** 프로퍼티 값은 **_자바스크립트에서 사용할 수 있는 모든 값_**이다.

- 프로퍼티 키는 프로퍼티 값에 접근할 수 있는 이름으로서 **_식별자 역할을 한다._** 하지만 식별자 네이밍 규칙을 반드시 따라야하는 것은 아니다.

- 프로퍼티 키는 문자열이므로 따옴표로 묶어야 하지만 에밍 규칙을 준수하는 이름은 **_따옴표를 생략할 수 있다._**
  즉, **_식별자 네이밍 규칙을 따르지 않는 이름에는 반드시 따옴표를 사용해야 한다._**

```javascript
var person = {
  firstName: "Ung-mo", // 식별자 네이밍 규칙을 준수하는 프로퍼티 키
  "last-name": "Lee", // 식별자 네이밍 규칙을 준수하지 않는 프로퍼티 키
};

console.log(person); // {firstName: "Ung-mo", last-name: "Lee"}
```

```javascript
var person = {
  firstName: 'Ung-mo',
  last-name: 'Lee' // SyntaxError: Unexpected token -
};
```

- 위와 같은 경우 키값에 따옴표를 생략하면 자바스크립트 엔진은 - 연산자가 있는 표현식으로 해석한다.

- 프로퍼티 키는 동적으로 생성할 수있으며 빈 문자열을 프로퍼티 키로 가능하며 문자열이나 심벌 값 외의 값을 사용하면 암묵적 타입 변환을 통해 문자열이 된다.

```javascript
var foo = {
  0: 1,
  1: 2,
  2: 3,
};

console.log(foo); // {0: 1, 1: 2, 2: 3}
```

- 위와 같은 경우 숫자 리터럴을 사용하면 내부적으로 문자열로 변환된다.

- var, function과 같은 예약어를 프로퍼티 키로 사용해도 에러가 발생하지 않고(권장x), 이미 존재하는 프로퍼티 키를 중복 선언하면 나중에 선언한 프로퍼티가 먼저 선언한 프로퍼티를 덮어쓴다.(에러발생x)

## 10.4 메서드

- 자바스크립트에서 사용할 수 있는 모든 값은 프로퍼티 값으로 사용할 수 있다. 이에 함수는 값으로 취급할 수 있기 때문에 프로퍼티 값으로 사용할 수 있다.

- 이때 프로퍼티 값이 함수일 경우 일반 함수와 구분하기 위해서 메서드라 부른다. 즉, 메서드는 객체에 묶여 있는 함수를 의미한다.

```javascript
var circle = {
  radius: 5, // ← 프로퍼티

  // 원의 지름
  (키 : 값)
  getDiameter: function () { // ← 메서드
    return 2 * this.radius; // this는 circle을 가리킨다.
  }
};

console.log(circle.getDiameter()); // 10
```

## 10.5 프로퍼티 접근

- 프로퍼티에 접근하는 방법은 다음 두 가지가 있다.

  - 마침표 표기법( . )
  - 대괄호 표기법 ([...])

- 프로퍼티 키가 식별자 네이밍 규칙을 준수하는 이름일때는 모두 사용 가능하다.

```javascript
var person = {
  name: "Lee",
};

// 마침표 표기법에 의한 프로퍼티 접근
console.log(person.name); // Lee

// 대괄호 표기법에 의한 프로퍼티 접근
console.log(person["name"]); // Lee
```

- 접근 연산자 우측 또는 내부에 프로퍼티 키값을 지정하며 대괄호 프로퍼티 접근 연산자 경우 반드시 키값을 따옴표로 감싼 문자열이어야 한다. 하지만 프로퍼티 키가 **_숫자로 이루어진 문자열인 경우_** 따옴표를 생략 가능하다.

```javascript
var person = {
  'last-name': 'Lee',
  1: 10
};

person.'last-name';  // -> SyntaxError: Unexpected string
person.last-name;    // -> 브라우저 환경: NaN
                     // -> Node.js 환경: ReferenceError: name is not defined
person[last-name];   // -> ReferenceError: last is not defined
person['last-name']; // -> Lee

// 프로퍼티 키가 숫자로 이뤄진 문자열인 경우 따옴표를 생략할 수 있다.
person.1;     // -> SyntaxError: Unexpected number
person.'1';   // -> SyntaxError: Unexpected string
person[1];    // -> 10 : person[1] -> person['1']
person['1'];  // -> 10
```

## 10.6 프로퍼티 값 갱신

- 이미 존재하는 프로퍼티에 값을 할당하면 프로퍼티 값이 갱신된다.

```javascript
var person = {
  name: "Lee",
};

// person 객체에 name 프로퍼티가 존재하므로 name 프로퍼티의 값이 갱신된다.
person.name = "Kim";

console.log(person); // {name: "Kim"}
```

## 10.7 프로퍼티 동적 생성

- 존재하지 않는 프로퍼티에 값을 할당하면 **_프로퍼티가 동적으로 생성되어 추가되고 프로퍼티 값이 할당된다._**

## 10.8 프로퍼티 삭제

- delete 연산자는 객체의 프로퍼티를 삭제한다. 이때 피연산자는 프로퍼티 값에 접근할 수 있는 표현식이어야 한다. 만약 존재하지 않는 프로퍼티를 삭제하면 에러 없이 무시된다.

```javascript
var person = {
  name: "Lee",
};

// 프로퍼티 동적 생성
person.age = 20;

// person 객체에 age 프로퍼티가 존재한다.
// 따라서 delete 연산자로 age 프로퍼티를 삭제할 수 있다.
delete person.age;

// person 객체에 address 프로퍼티가 존재하지 않는다.
// 따라서 delete 연산자로 address 프로퍼티를 삭제할 수 없다. 이때 에러가 발생하지 않는다.
delete person.address;

console.log(person); // {name: "Lee"}
```

## 10.9 ES6에서 추가된 객체 리터럴의 확장 기능

- ES6에서는 더욱 간편하고 표현력 있는 객체 리터럴의 확장 기능을 제공한다.

### 10.9.1 프로퍼티 축약 표현

- 객체 리터럴의 프로퍼티는 프로퍼티 키와 값으로 구성된다. 프로퍼티 값은 **_변수에 할당된 값, 즉 식별자 표현식일 수 도 있으며,_** 변수 이름과 키와 이름이 동일하다면 키를 생략할 수 있다.

```javascript
// ES6
let x = 1,
  y = 2;

// 프로퍼티 축약 표현
const obj = { x, y };

console.log(obj); // {x: 1, y: 2}
```

### 10.9.2 계산된 프로퍼티 이름

- 문자열 또는 문자열로 타입 변환할 수 있는 값으로 평가되는 표현식을 사용해 키를 동적으로 생성할 수 있다. 이 때 **_대괄호로 묶어야 하고 이를 계산된 프로퍼티 이름_**이라 한다.

```javascript
// ES5
var prefix = "prop";
var i = 0;

var obj = {};

// 계산된 프로퍼티 이름으로 프로퍼티 키 동적 생성
obj[prefix + "-" + ++i] = i;
obj[prefix + "-" + ++i] = i;
obj[prefix + "-" + ++i] = i;

console.log(obj); // {prop-1: 1, prop-2: 2, prop-3: 3}
```

```javascript
// ES6
const prefix = "prop";
let i = 0;

// 객체 리터럴 내부에서 계산된 프로퍼티 이름으로 프로퍼티 키 동적 생성
const obj = {
  [`${prefix}-${++i}`]: i,
  [`${prefix}-${++i}`]: i,
  [`${prefix}-${++i}`]: i,
};

console.log(obj); // {prop-1: 1, prop-2: 2, prop-3: 3}
```

### 10.9.3 메서드 축약 표현

- ES5에서 메서드를 정의하려면 프로퍼티 값으로 함수를 **_할당_**했지만 function 키워드를 생략한 축약 표현을 사용할 수 있다.

- 축약 표현으로 정의한 메서드는 할당한 함수와 다르게 동작한다.
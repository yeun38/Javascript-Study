# 22장 this

- 객체 = 상태를 나타내는 프로퍼티 + 동작을 나타내는 메서드
- 메서드는 자신이 속한 객체의 상태를 변경할 수 있어야 하며 이 때 프로퍼티를 참조하려면 자신이 속한 객체를 가리키는 식별자를 참조할 수 있어야한다.
- 생성자함수 경우 인스턴스를 생성하려면 먼저 생성자 함수가 존재 해야하지만 생성자 함수를 정의하기 이전이므로 인스턴스를 가리킬 특수한 식별자가 필요하다.
- 이를 위해 자바스크립트는 this라는 특수한 식별자를 제공하며, this란 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 자기 참조 변수다.
- this는 자바스크립트 엔진에 의해 암묵적으로 생성되며, this가 가리키는 값은 함수 호출 방식에 의해 동적으로 결정된다.
- this는 상황에 따라 가리키는 대상이 다르다. 호출되는 방식에 따라 this에 바인딩될 값이 동적으로 결정되기 때문이다.

```javascript
// this는 어디서든지 참조 가능하다.
// 전역에서 this는 전역 객체 window를 가리킨다.
console.log(this); // window

function square(number) {
  // 일반 함수 내부에서 this는 전역 객체 window를 가리킨다.
  console.log(this); // window
  return number * number;
}
square(2);

const person = {
  name: "Lee",
  getName() {
    // 메서드 내부에서 this는 메서드를 호출한 객체를 가리킨다.
    console.log(this); // {name: "Lee", getName: ƒ}
    return this.name;
  },
};
console.log(person.getName()); // Lee

function Person(name) {
  this.name = name;
  // 생성자 함수 내부에서 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
  console.log(this); // Person {name: "Lee"}
}

const me = new Person("Lee");
```

## 함수 호출 방식과 this바인딩

### 일반 함수 호출

- 기본적으로 this에는 전역 객체가 바인딩된다. 다만 this는 객체의 프로퍼티나 메서드를 참조하기 위한 자기 참조 변수이므로 객체를 생성하지 않는 일반 함수에서는 의미가 없다.

- stric mode가 적용된 경우 > undefined

```javascript
function foo() {
  "use strict";

  console.log("foo's this: ", this); // undefined
  function bar() {
    console.log("bar's this: ", this); // undefined
  }
  bar();
}
foo();
```

### 메서드 호출

- 메서드를 호출할 때는 이름 앞의 마침표 연산자앞에 기술한 객체가 바인딩된다.

```javascript
function Person(name) {
  this.name = name;
}

Person.prototype.getName = function () {
  return this.name;
};

const me = new Person("Lee");

// getName 메서드를 호출한 객체는 me다.
console.log(me.getName()); // ① Lee

Person.prototype.name = "Kim";

// getName 메서드를 호출한 객체는 Person.prototype이다.
console.log(Person.prototype.getName()); // ② Kim
```

### 생성자 함수 호출

- 생성자 함수 내부의 this에는 생성자 함수가 생성할 인스턴스가 바인딩된다.

### Function.prototype.apply/call/bind 메서드에 의한 간접 호출

- 모든 함수가 상속받아 사용할 수 있으며 this로 사용할 객체와 인수 리스트를 인수로 전달받아 함수를 호출한다.

```javascript
function getThisBinding() {
  return this;
}

// this로 사용할 객체
const thisArg = { a: 1 };

console.log(getThisBinding()); // window

// getThisBinding 함수를 호출하면서 인수로 전달한 객체를 getThisBinding 함수의 this에 바인딩한다.
console.log(getThisBinding.apply(thisArg)); // {a: 1}
console.log(getThisBinding.call(thisArg)); // {a: 1}
```

- apply와 call 메서드의 본질적인 기능은 함수를 호출하는 것이다.

```javascript
function getThisBinding() {
  return this;
}

// this로 사용할 객체
const thisArg = { a: 1 };

// bind 메서드는 첫 번째 인수로 전달한 thisArg로 this 바인딩이 교체된
// getThisBinding 함수를 새롭게 생성해 반환한다.
console.log(getThisBinding.bind(thisArg)); // getThisBinding
// bind 메서드는 함수를 호출하지는 않으므로 명시적으로 호출해야 한다.
console.log(getThisBinding.bind(thisArg)()); // {a: 1}
```

- bind메서드는 함수를 호출하지 않고 첫 번째 인수로 전달한 값으로 this 바인딩이 교체된 함수를 새롭게 생성해 반환한다.

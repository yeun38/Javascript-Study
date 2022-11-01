import "../../common/CommonStyle.css"

# 22장 this

this를 알아보기 전 객체에 대한 내용을 상기시켜 보자.

객체는 상태를 나타내는 프로퍼티와 동작을 나타내는 메서드를 하나의 논리적인 단위로 묶은 복합적인 자료구조이다.

```javascript
const circle = {
  // 프로퍼티: 객체 고유의 상태 데이터
  radius: 5,
  // 메서드: 상태 데이터를 참조하고 조작하는 동작
  getDiameter() {
    return 2 * circle.radius;
  },
};

console.log(circle.getDiameter()); // 10
```

자신이 속한 객체의 프로퍼티나 메서드를 참조하려면 자신이 속한 객체인 circle을 참조할 수 있어야 한다.  
위 코드에서 메서드가 호출되는 시점은 객체 리터럴이 circle 변수에 할당되기 이전 평가되고 이후 객체에 할당되어 객체가 생성되었다.  
하지만 자기 자신이 속한 객체를 재귀적으로 참조하는 방식은 일반적이지 않으며 바람직하지 않다.

 <br>

## this란?

자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는<span style="color:Lightseagreen"> **자기 참조 변수이며**</span>, this를 통해 자신이 속한 객체 또는 자신이 생성할 인스턴스의 <span style="color:Lightseagreen">**프로퍼티나 메서드를 참조 할 수 있다.**</span>

 <br>

## 호출에 따라 달라지는 this

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

<br/>

### 함수 호출 방식과 this 바인딩

함수 호출 방식에 따라 this에 바인딩될 값이 동적으로 결정된다.

1. 일반 함수 호출
2. 메서드 호출
3. 생성자 함수 호출
4. function.prototype.apply/call/bind 메서드에 의한 간접 호출

<br/>

#### 🔸 일반 함수 호출

```javascript
function foo() {
  console.log("foo's this: ", this); // window
  function bar() {
    console.log("bar's this: ", this); // window
  }
  bar();
}
foo();
```

- 전역함수는 물론 중첩 함수도 this에는 전역 객체가 바인딩된다.
- 사실, 객체를 생성하지 않는 일반 함수에는 this는 의미가 없다.
- strict mode가 적용된 일반 함수는 this에서 undefined가 바인딩된다.

#### 🔸 메서드 호출

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

#### 🔸 생성자 함수 호출

```javascript
// 생성자 함수
function Circle(radius) {
  // 생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}

// 반지름이 5인 Circle 객체를 생성
const circle1 = new Circle(5);
// 반지름이 10인 Circle 객체를 생성
const circle2 = new Circle(10);

console.log(circle1.getDiameter()); // 10
console.log(circle2.getDiameter()); // 20
```

- 이름 그대로 객체(인스턴스)를 생성하는 함수이다.
- new 연산자와 함께 호출해야 생성자 함수로 동작한다.

#### 🔸 메서드에 의한 간접 호출

- apply, call, bind 메서드는 funtion.prototype의 메서드로 모든 함수가 상속받아 사용할 수 있다.

```javascript
function getThisBinding() {
  return this;
}

// this로 사용할 객체
const thisArg = { a: 1 };

console.log(getThisBinding()); // window

console.log(getThisBinding.apply(thisArg)); // {a: 1}
console.log(getThisBinding.call(thisArg)); // {a: 1}
```

- apply와 call 메서드의 본질적인 기능은 함수를 호출하면서 인수로 전달한 객체를 this에 바인딩한다.

- bind메서드는 메서드의 this와 메서드 내부의 중첩 함수 또는 콜백 함수의 this가 불일치하는 문제를 해결 하기 위해 유용하게 사용된다.

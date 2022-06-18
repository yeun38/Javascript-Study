# 19장 프로토타입

- 자바스크립트는 객체 기반의 프로그래밍 언어이며 자바스크립트는 원시 타입의 값을 제외한 나머지 값들은 모두 객체이다.

- 객체지향 프로그래밍이란 여러 개의 독립적 단위, 즉 객체의 집합으로 프로그램으로 표현하려는 프로그래밍 패러다임을 말한다.

```javascript
const circle = {
  radius: 5, // 반지름

  // 원의 지름: 2r
  getDiameter() {
    return 2 * this.radius;
  },

  // 원의 둘레: 2πr
  getPerimeter() {
    return 2 * Math.PI * this.radius;
  },

  // 원의 넓이: πrr
  getArea() {
    return Math.PI * this.radius ** 2;
  },
};

console.log(circle);
// {radius: 5, getDiameter: ƒ, getPerimeter: ƒ, getArea: ƒ}

console.log(circle.getDiameter()); // 10
console.log(circle.getPerimeter()); // 31.41592653589793
console.log(circle.getArea()); // 78.53981633974483
```

- 위 코드를 보면 반지름은 원의 상태를 나타내는 데이터이며 원의 지름, 둘레, 넓이를 구하는 것은 동작이다.

- 객체지향 프로그래밍은 객체의 상태를 나타내는 데이터와 상태 데이터를 조작할 수 있는 동작을 하나의 논리적인 단위로 묶어 생각한다.

- 이때 객체의 상태 데이터를 프로퍼티, 동작을 메서드라 부른다.

## 상속과 프로토타입

- 상속은 객체지향 프로그래밍의 핵심 개념으로 재사용을 통해 불필요한 중복을 제거한다.

```javascript
// 생성자 함수
function Circle(radius) {
  this.radius = radius;
  this.getArea = function () {
    // Math.PI는 원주율을 나타내는 상수다.
    return Math.PI * this.radius ** 2;
  };
}
// 인스턴스 생성
const circle1 = new Circle(1);
const circle2 = new Circle(2);

// Circle 생성자 함수는 인스턴스를 생성할 때마다 동일한 동작을 하는
// getArea 메서드를 중복 생성하고 모든 인스턴스가 중복 소유한다.
// getArea 메서드는 하나만 생성하여 모든 인스턴스가 공유해서 사용하는 것이 바람직하다.
console.log(circle1.getArea === circle2.getArea); // false

function Circle(radius) {
  this.radius = radius;
}

// Circle 생성자 함수가 생성한 모든 인스턴스가 getArea 메서드를
// 공유해서 사용할 수 있도록 프로토타입에 추가한다.
// 프로토타입은 Circle 생성자 함수의 prototype 프로퍼티에 바인딩되어 있다.
Circle.prototype.getArea = function () {
  return Math.PI * this.radius ** 2;
};

// 인스턴스 생성
const circle1 = new Circle(1);
const circle2 = new Circle(2);

// Circle 생성자 함수가 생성한 모든 인스턴스는 부모 객체의 역할을 하는
// 프로토타입 Circle.prototype으로부터 getArea 메서드를 상속받는다.
// 즉, Circle 생성자 함수가 생성하는 모든 인스턴스는 하나의 getArea 메서드를 공유한다.
console.log(circle1.getArea === circle2.getArea); // true
```

- 위 코드를 보면 프로토타입 기반으로 상속을 받을 경우 자신의 상태를 나타내는 radius 프로퍼티만 개별적으로 소유하고 내용이 동일한 메서드는 상속을 통해 공유하여 사용하는 것이다.

## 프로토타입 객체

- 프로토타입 객체란 객체 간 상속을 구현하기 위해 사용된다.
- 모든 객체는 [[Prototype]]이라는 내부 슬롯을 가지며, 이 내부 슬롯의 값은 프로토타입의 참조다. 객체 생성 방식에 따라 타입이 결정되어 저장되는 특징이 있다.

- 모든 객체는 하나의 프로토타입을 갖는다. 그리고 모든 프로토타입은 생성자 함수와 연결되어 있다.

- **proto**접근자 프로퍼티를 통해 자신의 프로토타입에 간접적으로 접근할 수 있으며, 자신의 constructor 프로퍼티를 통해 생성자 함수에 접근, 생성자 함수는 사진의 prototype 프로퍼티를 통해 프로토타입에 접근할 수 있다.

### **proto** 접근자 프로퍼티

- 내부 슬롯은 프로퍼티가 아니기에 직접접근하거나 호출은 불가능하다. 하지만 일부에 한하여 간접적으로 접근 가능 수단을 제공하며 [[prototype]] 내부 슬롯의 값, 즉 프로퍼티 어트리뷰트로 구성된 프로퍼티다.

- **proto** 접근자 프로퍼티를 통해 접근하면 내부적으로 getter함수가 호출되고 새로운 프로토타입을 할당하면 **proto** 접근자 프로퍼티의 setter함수가 호출된다.

- 접근자 프로퍼티를 사용하여 프로토타입에 접근하는 이유는 상호 참조에 의해 프로토타입 체인이 생성되는 것을 방지하기 위해서이다.

### prototype 프로퍼티

- 함수 객체만이 수유하는 prototype 프로퍼티는 생성자 함수가 생설할 인스턴스의 프로토타입을 가리킨다.

- 따라서 생성자 함수로서 호출할 수 없는 함수 경우 prototype 프로퍼티를 소유하지 않으며 프로토타입도 생성하지 않는다.

### constructor 프로퍼티와 생성자 함수

- 모든 프로토타입은 constructor 프로퍼티를 갖는다. constructor 프로퍼티는 prototype 프로퍼티로 자신을 참조하고 있는 생성자 함수를 가리킨다.

## 리터럴 표기법에 의해 생성된 객체의 생성자 함수와 프로토타입

- 리터럴 표기법에 의해 생선된 객체도 프로토타입이 존재하지만, 리터럴 표기법에 의해 생성된 개체의 경우 프로토타입의 constructor 프로퍼티가 가리키는 생성자 함수가 반드시 객체를 생성한 생성자 함수라고 단정할 수는 없다.

프로토타입이 존재하기에 생성자함수도 존재하며 이 둘은 언제나 쌍으로 존재한다.

## 프로토타입의 생성 시점

- 프로토타입은 생성자 함수가 생성되는 시점에 더불어 생성된다. 언제나 쌍으로 존재하기 때문이다.

- 사용자 정의 생성자 함수 시 함수 객체를 생성하는 시점에 프로토타입도 더불어 생성된다.

- 빌트인 생성자 함수 경우 빌트인 함수가 생성되는 시점인 전역 객체가 생성되는 시점 생성된 프로토타입은 빌트인 생성자 함수의 prototype프로퍼티에 바인딩된다.

## 객체 생성 방식과 프로토타입의 결정

- 프로토타입은 추상 연산 OrdinaryObjentCreate에 전달되는 인수에 의해 결정되며 이 인수는 객체가 생성되는 시점에 객체 생성 방식에 의해 결정된다.

```javascript
const obj = { x: 1 };
```

- 객체 리터럴 경우 생성된 obj 객체는 Object.prototype을 프로토타입으로 갖게 되며, Object.prototype을 상속받는다.

- 객체리터럴과 Object 생성자 함수 방식은 객체 리터럴 내부에 프로퍼티를 추가하는 것은 동일하지만 Object 생성자 함수 방식은 빈 객체를 생성 후 프로퍼티를 추가한다는 차이점이 있다.

## 프롵타입 체인

- 프로터타입 체인은 자바스크립트가 객체지향 프로그래밍의 상속을 구현하는 메커니즘이다.

- 프로토타입 체인의 최상위에 위치하는 객체는 언제나 Object.prototype이며 체인의 종점이라 부른다.

- 스코프 체인은 식별자 검색을 위한 매커니즘이며 스코프와 프로토타입은 서로 연관없이 별도로 동작하는 것이 아니라 서로 협력하여식별자와 프로퍼티를 검색하는 데 사용된다.

## 오버라이팅 프로퍼티 셰도잉

- 오버라이딩 : 상위 클래스가 가지고 있는 메서드를 하위 클래스가 재정의 하여 사용하는 방식이다.

- 프로퍼티 섀도잉 : 상속 관계에 의해 프로퍼티가 가려지는 현상이다.

- 오버로딩 : 함수 이름은 동일하지만 매개변수의 타입 또는 개수가 다른 메서드를 구현하고 매개변수에 의해 메서드를 구별하여 호출하는 방식이다. (JS지원X > arguments 객체 사용하여 구현O)

## 프로토타입의 교체

- 프로토 타입의 교체는 부모 객체인 프로토타입을 동적으로 변경할 수 있다는 것을 말한다.

```javascript
const Person = (function () {
  function Person(name) {
    this.name = name;
  }

  // 생성자 함수의 prototype 프로퍼티를 통해 프로토타입을 교체
  Person.prototype = {
    // constructor 프로퍼티와 생성자 함수 간의 연결을 설정
    constructor: Person,
    sayHello() {
      console.log(`Hi! My name is ${this.name}`);
    },
  };

  return Person;
})();

const me = new Person("Lee");

// constructor 프로퍼티가 생성자 함수를 가리킨다.
console.log(me.constructor === Person); // true
console.log(me.constructor === Object); // false

function Person(name) {
  this.name = name;
}

const me = new Person("Lee");

// 프로토타입으로 교체할 객체
const parent = {
  sayHello() {
    console.log(`Hi! My name is ${this.name}`);
  },
};

// ① me 객체의 프로토타입을 parent 객체로 교체한다.
Object.setPrototypeOf(me, parent);
// 위 코드는 아래의 코드와 동일하게 동작한다.
// me.__proto__ = parent;

me.sayHello(); // Hi! My name is Lee
```

- 생성자 함수의 prototype프로퍼티에 다른 임의 객체를 바인딩하는 것은 미래에 생성할 인스턴스의 프로토타입을 교체하는 것이다.
- **proto** 접근자 프로퍼티를 통해 프로토타입을 교체하는 것은 이미 생성된 객체의 프로퍼티타입을 교체하는 것이다.

- 위 사항 모두 프로토타입으로 교체한 객체에는 constructor프로퍼티가 없으므로 constructor 프로퍼티와 생성자 함수 간의 연결이 파괴된다.

- 프토로타입 교체를 통해 객체 간의 상속 관계를 동적으로 변경하는 것은 권장하지 않는 편이다.

## instance 0f

- instanceof 연산자는 이항 연산자로서 좌변에 객체를 가리키는 식별자, 우변에 생성자 함수를 가리키는 식별자를 피연산자로 받으며 우변의 피연산자가 함수가 아닌 경우 TypeError가 발생한다.

- 우변의 생성자 함수의 prototype에 바인딩된 객체가 좌변에 객체의 프로토타입 체인 상에 존재하면 true로 평가되고, 그렇지 않은 경우에는 false로 평가된다.

- instanceof 연산자는 생성자 함수의 prototype에 바인딩된 객체가 프로토타입 체인 상에 존재하는지 확인한다.

## 직접 상속

- 위 프로토타입 교체를 통한 상속보다 직접 상속이 보다 편리하고 안전하다.

- object.create 메서드는 첫 번째 매개변수에 전달한 객체의 프로토타입 체인에 속하는 개체를 생성하면서 직접적으로 상속을 구현한다. 이 메서드의 장점은 아래와 같다.

  - new 연산자 없이 객체 생성 가능.
  - 프로토타입을 지정하면서 객체를 생성.
  - 객체 리터럴에 의해 생성된 객체도 상속 가능.

- 아래와 같이 **proto** 접근자 프로퍼티를 사용하여 직접 상속을 구현하는 방법도 있다.

```javascript
const myProto = { x: 10 };

// 객체 리터럴에 의해 객체를 생성하면서 프로토타입을 지정하여 직접 상속받을 수 있다.
const obj = {
  y: 20,
  // 객체를 직접 상속받는다.
  // obj → myProto → Object.prototype → null
  __proto__: myProto,
};
/* 위 코드는 아래와 동일하다.
const obj = Object.create(myProto, {
  y: { value: 20, writable: true, enumerable: true, configurable: true }
});
*/

console.log(obj.x, obj.y); // 10 20
console.log(Object.getPrototypeOf(obj) === myProto); // true
```

## 정적 프로퍼티/메서드

- 정적 프로퍼티/메서드는 생성자 함수로 인스턴스를 생성하지 않아도 참조/호출할 수 있는 메서드를 말한다.

```javascript
// 생성자 함수
function Person(name) {
  this.name = name;
}

// 프로토타입 메서드
Person.prototype.sayHello = function () {
  console.log(`Hi! My name is ${this.name}`);
};

// 정적 프로퍼티
Person.staticProp = "static prop";

// 정적 메서드
Person.staticMethod = function () {
  console.log("staticMethod");
};

const me = new Person("Lee");

// 생성자 함수에 추가한 정적 프로퍼티/메서드는 생성자 함수로 참조/호출한다.
Person.staticMethod(); // staticMethod

// 정적 프로퍼티/메서드는 생성자 함수가 생성한 인스턴스로 참조/호출할 수 없다.
// 인스턴스로 참조/호출할 수 있는 프로퍼티/메서드는 프로토타입 체인 상에 존재해야 한다.
me.staticMethod(); // TypeError: me.staticMethod is not a function
```

```javascript
// Object.create는 정적 메서드다.
const obj = Object.create({ name: "Lee" });

// Object.prototype.hasOwnProperty는 프로토타입 메서드다.
obj.hasOwnProperty("name"); // -> false

function Foo() {}

// 프로토타입 메서드
// this를 참조하지 않는 프로토타입 메소드는 정적 메서드로 변경해도 동일한 효과를 얻을 수 있다.
Foo.prototype.x = function () {
  console.log("x");
};

const foo = new Foo();
// 프로토타입 메서드를 호출하려면 인스턴스를 생성해야 한다.
foo.x(); // x

// 정적 메서드
Foo.x = function () {
  console.log("x");
};

// 정적 메서드는 인스턴스를 생성하지 않아도 호출할 수 있다.
Foo.x(); // x
```

- MDN문서를 보면 정적 프로퍼티/메서드와 프로토타입 프로퍼티/메서드를구변하여 소개하기에 표기법으로 구분할 수 있어야 한다.

## 프로퍼티 존재 확인과 열거

- in 연산자는 객체 내에 특정 프로퍼티가 존재하는지 여부를 확인 한다.

```javascript
const person = {
  name: "Lee",
  address: "Seoul",
};

// person 객체에 name 프로퍼티가 존재한다.
console.log("name" in person); // true
// person 객체에 address 프로퍼티가 존재한다.
console.log("address" in person); // true
// person 객체에 age 프로퍼티가 존재하지 않는다.
console.log("age" in person); // false
```

- in연산자는 확인 대상 객체의 프로퍼티뿐만 아니라 상속받은 모든 프로퍼티를 확인하기 때문에 주의가 필요하다.

- Object.prototype.hasOwnProperty메서드, ES6에서 도입된 Reflect.has 메서드도 in 연산자와 동일하게 동작한다.

- for ... in 문 : 객체의 프로퍼티 개수만큼 순회하며(상속받은 프로퍼티까지) for...in문의 변수 선언문에서 선언한 변수에 프로퍼티 키를 할당한다.

  - for(변수선언문 in 객체) {...}

```javascript
const person = {
  name: "Lee",
  address: "Seoul",
};

// in 연산자는 객체가 상속받은 모든 프로토타입의 프로퍼티를 확인한다.
console.log("toString" in person); // true

// for...in 문도 객체가 상속받은 모든 프로토타입의 프로퍼티를 열거한다.
// 하지만 toString과 같은 Object.prototype의 프로퍼티가 열거되지 않는다.
for (const key in person) {
  console.log(key + ": " + person[key]);
}

// name: Lee
// address: Seoul
```

- for...in 문은 객체의 프로토타입 체인 상에 존재하는 모든 프로토타입의 프로퍼티 중에서 프로퍼티 어트리뷰트 [[Enumerable]]의 값이 true인 프로퍼티를 순회하며 열거한다(순서보장X).

### Object.keys/values/enrties 메서드

- 객체 자신의 고유 프로퍼티만 열거하기 위해서는 아래 메서드를 권장한다.

  - Object.keys : 객체 자신의 열거 간으한 프로퍼티 키를 배열로 반환
  - values : 객체 자신의 가능한 프로퍼티 값을 배열로 반환.
  - enrties : 키와 값의 쌍의 배열을 반환.

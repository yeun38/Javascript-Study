# 16장 프로퍼티 어트리뷰트

- 내부 슬롯과 내부 메서드는 ECMAScript 사양에서 사용하는 의사 프로퍼티와 의사 메서드이다.
- 내부 슬롯과 메서드는 직접 접근 가능한 프로퍼티가 아니며, 일부 내부 슬롯과 메서드에 한하여 간접적으로 접근할 수 있는 수단이 제공된다.

- 자바스크립트 엔진은 프로퍼티를 생설할 때 프로퍼티의 상태를 나타내는 **_프로퍼티 어트리뷰트_**를 기본 값으로 자동 정의한다.
- 프로퍼티 상태 : 프로퍼티의 값, 값의 갱신 가능 여부, 열거 가능 여부, 재정의 가능 여부
- 프로퍼티 어트리뷰트는 자바스크립트 엔진이 관리하는 내부 상태 값인 내부 슬롯이다.
- 프로퍼티 어트리뷰트는 메서드를 사용하여 간접적으로 확인할 수 있다.
- Object.hetOwnPropertyDescriptor 메서드는 프로퍼티 어트리뷰트 정보를 제공하는 **_프로퍼티 디스크립터객체_**를 반환한다. 존재 하지 않을 시 undefined 반환.

- ES8에서 위 메서드는 모든 정보를 제공하는 프로퍼티 디스크립터 객체 반환.

## 데이터 프로퍼티와 접근자 프로퍼티

- 데이터 프로퍼티는 키와 값으로 구성된 일반적인 프로퍼티이다.

  - [[Value]] > value : 키를 통해 값에 접근하면 반환되는 값.
  - [[Writable]] > writable : 변경 가능 여부를 불리언 값으로 나타내며 false인경우 읽기전용 프로퍼티가 된다.
  - [[Enumerable]] > enumerable : 열거 가능 여부를 불리언 값으로 나타내며 false인경우 메서드를 이용하여 열거가능.
  - [[Configurable]] > configurable : 재정의 가능 여부를 나타내며 불리언 값을 갖는다. false인 경우 삭제 변경 금지. 단. [[Writable]]이 true인 경우 [[Value]]의 변경과 [[Writable]]을 false로 변경하는 것을 허용.

- 접근자 프로퍼티는 자체적으로 값을 갖지 않고 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 사용하는 접근자 함수로 구성된 프로퍼티다.

  - [[Get]] > get : 데이터 프로퍼티의 값을 읽을 때 호출되는 접근자 함수.
  - [[Set]] > set : 값을 저장할 때 호출되는 접근자 함수.
  - [[Enumerable]] > enumerable : 데이터 프로퍼티와 동일
  - [[Configurable]] > configurable : 데이터 프로퍼티와 동일

## 프로퍼티 정의

- 프로퍼티 정의란 새로운 프로퍼티를 추가하면서 프로퍼티 어트뷰트를 명시적으로 정의나 재정의하는 것을 말한다.

```javascript
const person = {};

// 데이터 프로퍼티 정의
Object.defineProperty(person, "firstName", {
  value: "Ungmo",
  writable: true,
  enumerable: true,
  configurable: true,
});

Object.defineProperty(person, "lastName", {
  value: "Lee",
});

let descriptor = Object.getOwnPropertyDescriptor(person, "firstName");
console.log("firstName", descriptor);
// firstName {value: "Ungmo", writable: true, enumerable: true, configurable: true}

// 디스크립터 객체의 프로퍼티를 누락시키면 undefined, false가 기본값이다.
descriptor = Object.getOwnPropertyDescriptor(person, "lastName");
console.log("lastName", descriptor);
// lastName {value: "Lee", writable: false, enumerable: false, configurable: false}

// [[Enumerable]]의 값이 false인 경우
// 해당 프로퍼티는 for...in 문이나 Object.keys 등으로 열거할 수 없다.
// lastName 프로퍼티는 [[Enumerable]]의 값이 false이므로 열거되지 않는다.
console.log(Object.keys(person)); // ["firstName"]

// [[Writable]]의 값이 false인 경우 해당 프로퍼티의 [[Value]]의 값을 변경할 수 없다.
// lastName 프로퍼티는 [[Writable]]의 값이 false이므로 값을 변경할 수 없다.
// 이때 값을 변경하면 에러는 발생하지 않고 무시된다.
person.lastName = "Kim";

// [[Configurable]]의 값이 false인 경우 해당 프로퍼티를 삭제할 수 없다.
// lastName 프로퍼티는 [[Configurable]]의 값이 false이므로 삭제할 수 없다.
// 이때 프로퍼티를 삭제하면 에러는 발생하지 않고 무시된다.
delete person.lastName;

// [[Configurable]]의 값이 false인 경우 해당 프로퍼티를 재정의할 수 없다.
// Object.defineProperty(person, 'lastName', { enumerable: true });
// Uncaught TypeError: Cannot redefine property: lastName

descriptor = Object.getOwnPropertyDescriptor(person, "lastName");
console.log("lastName", descriptor);
// lastName {value: "Lee", writable: false, enumerable: false, configurable: false}

// 접근자 프로퍼티 정의
Object.defineProperty(person, "fullName", {
  // getter 함수
  get() {
    return `${this.firstName} ${this.lastName}`;
  },
  // setter 함수
  set(name) {
    [this.firstName, this.lastName] = name.split(" ");
  },
  enumerable: true,
  configurable: true,
});

descriptor = Object.getOwnPropertyDescriptor(person, "fullName");
console.log("fullName", descriptor);
// fullName {get: ƒ, set: ƒ, enumerable: true, configurable: true}

person.fullName = "Heegun Lee";
console.log(person); // {firstName: "Heegun", lastName: "Lee"}
```

- Object.defineProperty 메서드는 한번에 하나의 프로퍼티만 정의할 수 있으며 Object.definePropertie 메서는 여러개 가능하다.

## 객체 변경 방지

- 객체 확장 금지 : Object.preventExtensions 메서드는 확장이 금지된 객체는 프로퍼티 추가가 금지된다.
- 객체 밀봉 금지 : Object.seal 메서드는 객체를 밀봉한다. 즉, 밀봉된 객체는 읽기와 쓰기만 가능하다.
- 객체 동결 : Object.freeze 메서드는 객체를 동결한다. 읽기만 가능하다.
- 불변 객체 : 위의 변경 방지 메서드들은 얉은 변경방지로 직속 프로퍼티만 영향을 주며 중첩 객체까지는 주지 못한다.

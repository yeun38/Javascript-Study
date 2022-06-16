# 20장 stric mode

- stric mode는 자바스크립트 언어의 문법을 좀 더 엄격히 적용하여 오류를 발생시킬 가능성이 높거나 자바스크립트 엔진의 최적화 작업에 문제를 일으킬 수 있는 코드에 대해 명시적인 에러를 발생시킨다.
- ESLint 같은 린트 도구를 사용해도 stric mode와 유사한 효과를 얻을 수 있다.

## stric mode 적용

- stric mode를 적용하려면 전역의 선두 또는 함수 몸체의 선두에 'use strict';를 추가한다.

- 전역에 적용할 경우 스크립트 단위로 적용되며, stric mode는 다른 스크립트에 영향을 주지 않고 해당 스크립트에 한정되어 적용된다.

- 또한 함수 단위로 strict mode를 적용할 경우 마찬가지 단위마다 각기 다른 영향을 받을 수 있으므로 피해야한다.

## stric mode가 발생시키는 에러

(stric mode를 적용 했을 때)

- 선언하지 않은 변수를 참조하면 ReferenceError가 발생.

- delete 연산자로 변수, 함수, 매개변수를 삭제하면 SyntaxError 발생.

- 중복된 매개변수 이름을 사용하면 SyntaxError 발생.

- with문을 사용하면 SyntaxError 발생.

## stric mode 적용에 의한 변화

- 함수를 일반 호출로 호출 시 this에 undefined가 바인딩된다. 생성자 함수가 아닌 일반 함수 내부에서는 this를 사용할 필요가 없기 때문이다.

````javascript
(function () {
  'use strict';

  function foo() {
    console.log(this); // undefined
  }
  foo();

  function Foo() {
    console.log(this); // Foo
  }
  new Foo();
}())

- 매개변수에 전달된 인수를 재할당하여 변경해도 arguments 객체에 반영되지 않는다.

```javascript
(function (a) {
  'use strict';
  // 매개변수에 전달된 인수를 재할당하여 변경
  a = 2;

  // 변경된 인수가 arguments 객체에 반영되지 않는다.
  console.log(arguments); // { 0: 1, length: 1 }
}(1));
````

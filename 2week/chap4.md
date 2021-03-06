# 04장 변수

## 4.1 변수란 무엇인가? 왜 필요한가?

- 컴퓨터는 연산과 기억을 수행하는 부분이 나눠져 있다. CPU를 사용해 연산하고, 메모리를 사용해 데이터를 기억한다.
- 메모리는 데이터를 저정할 수 있는 메모리 셀의 집합체이며, 셀 하나의 크기는 1바이트(8bit)이며, 컴퓨터는 메모리 셀의 크기, 즉 1바이트 단위로 데이터를 저장하거나 읽어 들인다.
- 각 셀은 고유의 메모리 주소를 갖는다. 이 메모리 주소는 메모리 공간의 위치를 나타내며, 0부터 시작해서 메모리의 크기만큼 정수로 표현된다. **_이 때 데이터의 종류에 상관없이 모두 2진수로 저장된다._**
- CPU는 메모리에서 데이터를 가져와 연산을 하고 결과값을 다시 메모리에 저장한다. 여기서 만들어진 값은 재사용할 수 없기 때문에 변수라는 메커니즘을 제공한다.
- **_변수는 하나의 값을 저장하기 위해 확보한 메모리 공간 자체 또는 그 메모리 공간을 식별하기 위해 붙인 이름을 말한다._** (= 값의 위치를 가리키는 상징적인 이름이다.)
- 변수에 값을 저장하는 것을 할당(대입,저장)이라 하고, 변수에 저장된 값을 읽어 들이기는 것을 참조라 한다. > 변수 이름을 사용해 참조를 요청하면 자바스크립트 엔진은 변수 이름과 매핑된 메모리 주소를 통해 메모리 공간에 접근해서 저장된 값을 반환한다.

## 4.2 식별자

- 변수 이름을 식별자라고도 한다. **_식별자는 어떤 값을 구별해서 식별할 수 있는 고유한 이름을 말한다._**
- 식별자는 값이 저장되어 있는 메모리 주소와 매핑 관계를 맺으며, 이 매핑 정보도 메모리에 저장되어야 한다.
- **_이처럼 식별자는 값이 아니라 메모리 주소를 기억하고 있다._** 식별자로 값을 구별해서 식별한다는 것은 식별자가 기억하고 있는 메모리 주소를 통해 메모리 주소를 통해 메모리 공간에 저장된 값에 접근할 수 있다는 의미다. **_즉 식별자는 메모리 주소에 붙인 이름이라고 할 수 있다._**
- 식별자는 변수, 함수, 클래스 등의 이름은 모두 식별자이다.

## 4.3 변수 선언

- 변수 선언이란? 값을 저장하기 위한 메모리 확보하고 변수 이름과 확보된 메모리 주소를 연결해서 값을 저장할 수 있게 준비하는 것이다.
- 변수를 선언할 때는 **_var, let,const 키워드를 사용한다._**
- var 키워드(명령어) > 변수 선언 시 메모리 공간은 undefined 라는 값이 암묵적으로 할당되어 초기화된다. > 자바스크립트 특징.
- 자바스크립트 엔진은
  - 선언 단계 : 변수 이름을 등록해서 자바스크립트 엔진에 변수의 존재를 알린다.
  - 초기화 단계 : 값을 저장하기 위한 메모리 공간을 확보하고 암묵적으로 undefined를 할당해 초기화(=최초로 값을 할당하는 것)한다.
    > 만약 초기화 단계를 거치지 않으면 확보된 메모리 공간에는 이전에 다른 애플리케이션이 사용했던 값이 남아 있을 수 있다. 이러한 값을 쓰레기 값이라 한다.
- 변수 뿐만 아니라 모든 식별자가 선언이 필요하고 선언하지 않은 식별자에 접근하면 참조 에러(ReferenceError)가 발생한다.

## 4.4 변수 선언의 실행 시점과 변수 호이스팅

- 자바스크립트는 인터프리리터에 의해 한 줄씩 순차적으로 실행되지만 **_자바스크립트 엔진은 변수 선언이 소스코드의 어디에 있든 상관없이 다른 코드보다 먼저 실행한다._**
- 이처럼 변수 선언문이 코드의 선두로 끌어 올려진 것처럼 동작하는 자바스크립트 고유의 특징을 **_변수 호이스팅_** 이라 한다. (+ 모든 식별자 또한 가능.)

## 4.5 값의 할당

- 변수에 값을 할당할 때는 할당 연산자(=)를 사용한다.

```
var score;
score = 80;

var score = 80;
```

- 변수 선언은 소스코드가 순차적으로 실행되는 시점인 런타임 이전에 먼저 실행되지만 **_값의 할당은 소스코드가 순차적으로 실행되는 시점인 런타임에 실행된다._**

```
console.log(score); // undefined

var score;
score = 80;
// var score = 80;

console.log(score); // 80 > undefined값을 지우고 새롭게 저장하는 것이 아닌 새로운 메모리 공간을 확보하고 그곳에 할당 값을 저장한다.
```

## 4.6 값의 재할당

```
var score;
score = 80;

score = 90;
```

- 재할당이란 이미 값이 할당되어 있는 변수에 새로운 값을 또 다시 할당하는 것을 말한다.
- var 키워드로 선언한 변수는 값을 재할당할 수 있다.
- 만약 값을 재할당할 수 없어서 변수에 저장된 값을 변경할 수 없다면 변수가 아니라 상수라 한다.

  - const 키워드 : ES6에서 const 키워드를 사용해 선언한 변수는 재할당이 금지된다. 따라서 const 키워드를 사용하면 상수를 표현할 수 있다.

- 값이 재할당 될 경우 값의 할당 내용과 같이 새로운 메모리 할당이 되며 이전의 값들은 어떤 식별자와도 연결되어 있지 않고 **_가비지 콜렉터에 의해 메모리에서 자동 해제된다_** > 해제 예측은 알 수 없다.

## 4.7 식별자 네이밍 규칙

식별자는 다음고 같은 네이밍 규칙을 준수해야 한다.

- 식별자는 특수문자를 제외한 문자.숫자.언더스코어,달러 기호를 포함 할 수 있다.
- 단. 식별자는 특수문자를 제외한 문자,언더스코어,달러 기호로 시작해야 한다. **_숫자로 시작하는 것은 허용하지 않는다._**
- 예약어는 식별자로 사용할 수 없다.

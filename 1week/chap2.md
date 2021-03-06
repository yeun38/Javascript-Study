# 02 자바스크립트란?

## 2.1 자바스크립트의 탄생

- 브라우저에서 동작하는 경량 프로그래밍 언어를 도입하기 위함.

## 2.2 자바스크립트의 표준화

- 초기 문제 : 브라우저에 따라 웹페이지가 정상적으로 동작하지 않는 크로스 브라우싱 이슈 발생
- 문제 해결 : ECMA 인터내셔널에 자바스크립트의 표준화 요청
- 97년 초판 사양이 완성 > 이후로 꾸준히 작은 기능을 추가하는 수준으로 매년 공개할 것을 예고

## 2.3 자바스크립트 성장의 역사

### 2.3.1 Ajax

- 자바스크립트를 이용해 서버와 브라우저가 비동기 방식으로 데이터를 교환할 수 있는 통신 기능인 Ajax가 XMLHttpRequest라는 이름으로 등장했다.
  Ajax는 서버로부터 필요한 데이터만 전송받아 변경해야하는 부분만 **한정적으로 렌더링하는 방식이 가능하다.**

### 2.3.2 jQuery > 라이브러리 > 사용언어: 자바스크립트

- DOM을 더욱 쉽게 제어할 수 있게 됨 > 자바스크립트보다 배우기 쉽고 직관적이라 더 선호하는 개발자가 양산되기도 했다.

---

라이브러리란
현실세계에서의 라이브러리(도서관)란 필요할 때마다 꺼내볼 수 있는 책(지식)들이 모여있는 곳이다. 남들이 만들어둔 외부 라이브러리도 가져다 사용할 수 있다.

1. Browser환경에서 script src 로 불러들이는 js파일(JQuery 등)
2. node.js 환경에서 npm으로 설치한 모듈
3. Python 환경에서 pip로 설치한 패키지/모듈
4. Java 환경에서 설치한 jar

### 2.3.3 v8자바스크립트 엔진

- v8 자바스크립트 엔진으로 촉발된 자바스크립트의 발전으로 웹 서버에서 수행되던 로직들이 대거 클라이언트(브라우저)로 이동했고, 이는 웹 애플리케이션 개발에서 프런트엔드 영역이 주목받는 계기가 되었다.

### 2.3.4 Node.js

- 구글 v8 자바스크립트 엔진으로 빌드된 자바스크립트 런타임 환경이다.
  **브라우저의 자바스크립트 엔진에서만 동작하던 자바스크립트를(다른 브라우저로 옮기면 실행에 오류를 확인할 수 있음** 브라우저 이외의 환경에서도 동작할 수 있도록 자바스크립트의 엔진을 브라우저에서 독립시킨 자바스크립트 실행 환경이다.
  Node.js는 비동기 I/O를 지원하며 단일 스레드 이벤트 루프 기반으로 동작함으로써 요청 처리 성능이 좋다. 따라서 Node.js는 데이터를 실시간으로 처리하기 위해 I/O가 비번하게 발생하는 SPA(single page application)에 적합하다. 하지만 CPU 사용률이 높은 애플리케이션에는 권장하지 않는다.

### 2.3.5 SPA 프레임워크

- 서버로부터 완전한 새로운 페이지를 불러오지 않고 현재의 페이지를 동적으로 다시 작성함으로서 사용자와 소통하는 웹 앱/사이트
  CBD방법론을 기반으로 하는 SPA가 대중화되면서 Angular, React, Vue.js, Svelte 등 다양한 SPA 프레임워크/라이브러리 또한 많은 사용층을 확보하고 있다.
- Link: [SPA 프레임워크 추가 참고 자료][https://bangu4.tistory.com/164]

## 2.4 자바스크립트와 ECMScript

- ECMScript는 자바스크립트의 표준 사양을 말하며 핵심 문법을 규정한다.
  자바스크립트는 일반적으로 프로그래밍 언어로서 기본 뼈대를 이루는 ECMScript와 브라우저가 별도 지원하는 클라이언트 사이드 Wep API 즉, DOM, fetch, XMLHttpRequest Wep Component 등을 아우르는 개념이다.
- Link: [클라이언트 사이드 Wep API 추가 참고 자료][https://velog.io/@mollog/web-api-mdn-%eb%82%b4%ec%9a%a9-%ec%a0%95%eb%a6%ac]

## 2.5 자바스크립트의 특징

- 웹을 구성하는 요소 중 하나로 **웹 브라우저에서 동작하는 유일한 프로그래밍 언어이다.**
  인터프리터는 소스코드를 즉시 실행하고 컴파일러는 빠르게 동작하는 머신 코드를 생성하고 최적화 한다. 대부분 모던 자바스크립트 엔진은 장점을 결합해 비교적 처리 속도가 느린 인터프리터의 단점을 해결했다.
  비록 다른 객체지향 언어와 차이점에 대한 논쟁이 있긴 하지만 자바스크립트는 강력한 객체지향프로그래밍 능력을 지니고 있다. 자바스크립트는 클래스 기반 객체지향 언어보다 효율적이면서 강력한 프로토타입 기반의 객체지향 언어다.

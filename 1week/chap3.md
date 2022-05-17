# 03장 자바스크립트 개발 환경과 실행 방법

## 3.1 자바스크립트 실행 환경

- 모든 브라우저는 자바스크립트를 해석하고 실행 할 수 있는 자바스크립트 엔진을 내장하고 있다. 브라우저와 Node.js 환경에서 동작하는 코드는 동일하다. 하지만 두 개의 용도는 다르다.
  브라우저는 웹페이지를 브라우저 화면에 렌더링하는 것이 주된 목적이지만 Node.js는 브라우저 외부에서 자바스크립트 실행 환경을 제공하는 것이 주된 목적이다. 따라서 브라우저와 Node.js 모두 자바스크립트의 코어인 **ECMAScript를 실행할 수 있지만 ECMAScript 이외에 추가로 제공하는 기능은 호환되지 않는다.**
  ex) DOM API, FileReader

## 3.3 Node.js

- 프로젝트의 규모가 커지에 따라 React, Angular, Lodash 같은 프레이워크 또는 라이브러리를 도입하거나 babel, webpack, eslint 등 여러 가지 도구를 사용할 필요가 있다. 이때 Node.js와 npm이 필요하다.

### 3.3.1 Node.js와 npm

- 자바스크립트를 브라우저 이외의 환경에서 동작시킬 수 있는 자바스크립트 실행환경이 Node.js이다. npm 자바스크립트 패키지 매니저이다. node.js에서 사용할 수 있는 모듈들을 패키지화해서 모아둔 저장소 역할과 패키지 설치 및 관리를 위한 CLI를 제공한다.

### 3.3.2 Node.js 설치

- 설치 시 npm도 함께 설치된다.

## 3.2 비주얼 스튜디오 코드

### 3.4.1 비주얼 스튜디오 코드 설치

- 브라우저의 콘솔 또는 node.js의 REPL에서 자바스크립트 코드를 실행 할 수 있지만 애플리케이셔을 개발하는 단계에서 사용하기에는 부족함이 많다.

### 3.4.2 내장 터미널

- 내장 터미널 안에 **1) Node.js 명령어로 자바스크립트 파일을 실행할 수 있다.**

### 3.4.3 code runner 확장 플러그인 **2) 단축키를 사용해 자바스크립트 실행 가능**

- 참고 alert함수는 브라우저에서만 동작하는 클라이언트 사이트 Web API다. 즉 브라우저 환경에서만 유효하다. 그런데 code runner 확장 플러그인은 node.js 환경을 사용해 자바스크립트를 실행한다. 따라서 클라이언트 사이트 Web API는 사용할 수 없다.
# MJDN's Express.js

Node.js는 강력하지만, 그 API는 때로 힘이 부족해 장황한 코드를 작성하게 함.
일반적으로 Vanilla API는 장황함(Node.js).

Express는 Node.js의 웹 서버상에 <u>얹히는</u> 프레임워크임.
Node.js 웹 서버 최상위의 경량 계층으로 동작하는 프레임워크인 Express

Express와 같은 프레임워크가 없다면, Node.js 는 아주 간단한 API를 제공함 요청을 처리하는 함수를 만들어 `http.createServer`로 전달하고, 호출함 

이런 면에서 Express는 철학적으로 jQuery와 유사함.
  1. Vanilla API의 장황함을 단순화
  2. 유용한 새로운 기능 추가
  3. 확장성 지향 - 대부분 불필요한 간섭 없이, 서드파티 라이브러리로 쉽게 확장 가능

Ruby의 미니멀 웹 애플리케이션 프레임워크인 `Sinatra`에서 상당한 영감을 받음(routing, middleware 같은 기능을 가짐)
Python - `Bottle`, `Flask`와 닮음

Python - `Django` / Ruby - `Ruby on Rails` / `ASP.NET` / Java - `Play`와는 다름

서버에서 코드가 돌아가긴 하지만, vanilla PHP에서처럼 HTML과 강하게 결합된 것은 아님

## Node.js 비즈니스
* Node.js: JavaScript 플랫폼
<u>JavaScript를 실행하는 방식</u>
* JavaScript를 브라우저 외부에서 실행한다는 것 
-> 일반 프로그래밍 언어에서 할 수 있는 모든 것은 JavaScript도 할 수 있다는 것
Node.js 덕분에 JavaScript를 서버에서 실행할 수 있음

##### Node.js를 사용하는 이유
즉, Node.js를 통해 JS를 C, Java와 같은 프로그래밍 언어로서 사용해볼만 한 이점은 무엇일까?
1. **V8** - 빠른 JS 엔진
2. **비동기** - <u>동시성</u>을 다루는 능력
비동기 코드는 (코드를 병렬로 실행하지 못해도) 더 빨리 끝낼 수 있음
but,
<u>싱글 코어에 대해 최대 성능</u>을 짜내지만,
멀티 코어와 같이 <u>성능을 가속시키지는 못함</u>
> Node.js를 선택하는 가장 큰 이유가 성능은 아님
3. **가장 큰 이유** - JavaScript 하나로 클라이언트, 서버 모두 사용할 수 있음

## Express
Node.js의 웹 서버 기능 위에 올라가는 **상대적으로 작은 프레임워크**
Node.js의 API를 단순화하고 유용한 새로운 기능(미들웨어, 라우팅)을 추가함
Node.js의 HTTP 개체에 유용한 유틸리티를 추가하고, 동적 HTML 뷰의 렌더링을 용이하게 함
쉽게 구현된 확장성 표준을 정의함

> **요청 핸들러 (Request Handler)**
애플리케이션에서 브라우저 요청을 처리하는 자바스크립트 함수

Node.js의 HTTP 서버에서 클라이언트와 자바스크립트 함수 간의 연결을 처리하기 때문에
까다로운 네트워크 프로토콜을 다룰 필요가 없다.

Client ↔ HTTP Server ↔ Request Handler

요청 핸들러는 두 개의 인수를 가짐
1. **요청**을 나타내는 개체
2. **응답**을 나타내는 개체

모든 Node.js 애플리케이션은 "**요청에 응답하는 하나의 요청 핸들러 함수**다."

문제는 Node.js API가 복잡해질 수 있다는 점.
Node.js의 HTTP 서버는 강력하지만, 실제 애플리케이션을 만들 경우 필요한 많은 기능을 갖추고 있지 않음.
Express.js는 Node.js로 웹 애플리케이션을 더 쉽게 만들기 위해 나타남.
(Node.js API 단순화, 유용한 기능 추가)

### Node.js에 추가된 Express 기능
1. Node.js의 HTTP 서버에 여러 가지 유용한 편리 기능을 추가해 많은 복잡성을 추상화함
ex_JPEG 파일 전송) Node.js 45줄(최대 성능) ↔ Express 1줄
2. 하나의 **모놀리식** request handler 함수를 
다수의 더 작은 request handler로 리팩터링해서 특정 부분만 처리 가능
-> 유지 관리와 모듈화에 좋음

### Express의 미니멀 철학
Express는 **프레임워크** -> 앱을 Express 방식으로 만들어야 한다는 의미
Express는 최소한의 부분(HTTP 요청에 대한 처리)에 대한 부분에 특화되어 있음

> 유닉스 세계의 "한가지를 제대로 하자(**do-one-thing-well**)" 철학

따라서 HTTP 요청 처리에 대한 부분 이외의 것들은 다른 라이브러리를 사용해야 함

> **미니멀리즘은 양날의 검**
앱은 유연하며, 사용되지 않는 군더더기로부터 해방되지만
이로 인해 실수가 발생할 수 있고, 애플리케이션 아키텍처에 관해 더 많은 결정을 내려야 함
즉, 알맞은 서드파티 모듈을 찾는 데에 많은 시간을 사용해야 함.

아무튼
###### Express는 미니멀리스트 프레임워크이며, Node.js의 HTTP 모듈을 사용하기 쉽도록 만든 것

## Express 핵심
Express의 주요 기능은 다음과 같음
1. middleware
2. routing
3. sub-application(router)
4. 편의 기능

### 1. 미들웨어 (middleware)
해당 작업의 작은 덩어리로 다루는 몇 개의 요청 핸들러 함수를 호출함
미들웨어의 종류는 다양함
* 로깅
* 보안 및 사용자 인증
* 정적 자산 컴파일
* 쿠키와 세션 분석 등

### 2. 라우팅 (routing)
미들웨어처럼, 라우팅은 하나의 모놀리식 요청 핸들러 함수를 더 작은 조각으로 나눔
미들웨어와 달리 이러한 요청 핸들러들은 조건에 따라 실행됨
즉, 클라이언트에서 어떤 URL과 HTTP 메서드를 보내는가에 따라 실행됨
라우팅은 애플리케이션 동작을 라우트(route)로 분할할 수 있음

Express 애플리케이션은 미들웨어와 라우트를 가짐
이 둘은 서로를 보완함

### 3. 서브 애플리케이션 (sub-application)
Express 애플리케이션은 파일 하나에 들어갈 정도로 작은 때도 있고,
애플리케이션이 커지면 여러 개의 폴더와 파일로 나눌 때도 있음
Express에서는 서브애플리케이션이라는 중요한 기능을 통해 나눔
즉, Express 애플리케이션을 더 작은 '미니 애플리케이션'으로 나눌 수 있음
이러한 미니 애플리케이션을 "라우터"라고 부름

다음과 같이 나눌 수 있음
* Express 애플리케이션
  * 관리 패널 라우터
  * API 라우터
    * API 버전 1 라우터
    * API 버전 2 라우터
  * 단일 페이지 애플리케이션 라우터

애플리케이션이 커지면 매우 유용함

### 4. 편의 기능
Express 애플리케이션은 요청 핸들러 함수를 작성하는 두 가지인 '미들웨어'와 '라우터'로 만듦
이러한 요청 핸들러 함수를 더 쉽게 작성하도록 한다.
* HTML 더 쉽게 렌더링
* 요청이 들어올 때마다 분석을 더 쉽게 해주는 기능 등 

## Express의 사용처
1. Express는 단일 페이지 애플리케이션(SPA) 개발에 자주 사용됨
2. Express는 많은 리얼타임 기능에도 맞춰져 있음
  * WebSocket
  * WebRTC

## Node.js와 Express를 위한 서드파티 모듈
Express에는 서드파티 모듈이 많음

Express에는 HTML을 렌더링하기 위한 작은 기능이 몇 가지 있음
템플릿 언어(EJS, Pug 등)를 통해 SSR을 처리할 수 있다.

## 요약
* Node.js는 웹 애플리케이션을 작성하는 강력한 도구이지만, 성가진 부분이 많음
Express는 그러한 과정을 부드럽게 만듦
* Express는 최소한의 유연한 프레임워크
* Express의 몇 가지 핵심 기능은 다음과 같음
  * 미들웨어: 앱을 더 작은 동작으로 나누는 방식. 일반적으로 순차적으로 하나씩 호출
  * 라우팅: 미들웨어와 비슷하게 앱을 사용자가 특정 리소스를 방문할 때 실행되는 더 작은 함수로 나눔
  * 라우터: 큰 애플리케이션을 더 작고 구성 가능한 서브애플리케이션으로 나눔
* Express 코드의 대부분은 요청 핸들러 함수 작성을 수반하며, 
Express에서는 이들 함수를 작성할 때 많은 편의기능을 추가함

# Node.js
자바스크립트 세계의 문제는 선택지가 너무 많은 것

## 모듈 사용
대부분의 프로그래밍 언어는 여러 파일에 코드를 분할할 수 있도록(모듈화할 수 있도록) 파일 B 내부에 파일 A의 코드를 포함시키는 방식을 취함
* C, C++: `#include`
* Python: `import`
* Ruby, PHP: `require`
* C#: 컴파일 타임에 암시적으로 일종의 파일간 통신을 수행

### CommonJS
`require` 함수를 사용해 모듈을 사용
`require` 함수는 패키지의 이름을 문자열 인수로 취해서 패키지를 반환함

모듈의 종류
* 내장 모듈
* 서드파티 모듈
* 사용자 정의 모듈
  * `module.exports`를 통해 다른 파일에 모듈 노출함
  * 모듈을 불러올 때 점 문법을 사용해 경로 지정해야 함

### URL Parser 모듈
Node.js 내장 모듈 - URL 분석
URL Parser 모듈의 핵심은 `parse` 함수임
이 함수는 URL 문자열을 취해서 도메인이나 경로 같은 유용한 정보를 추출함

## Node.js의 비동기 모델
```plain-text
쿠키를 반죽할 때는 한 번에 하나의 일을 할 수 밖에 없지만, 
쿠키를 오븐(외부 리소스)에 넣고 나서는 다른 일을 할 수 있다.
```
동시에 여러 가지 일이 일어나더라도 한 번에 할 수 있는 일은 하나임
하지만, 외부 자원을 사용하면 가능함
외부 리소스에서 뭔가를 처리하는 동안, 계속해서 다른 코드를 작동시킬 수 있음

### Express에서 다룰 가장 일반적인 외부 리소스
* <u>파일 시스템</u>을 수반하는 모든 작업 - 하드 드라이브에서 파일을 읽고 쓰는 작업 등
* <u>네트워크</u>를 수반하는 모든 작업 - 요청 수신 또는 응답 전송, 인터넷으로 사용자 정의 요청 전송 등
-> 즉, IO가 일어나는 작업

## HTTP 모듈
Node.js로 웹 서버를 개발할 수 있게 해줌
Express는 이 모듈 위에 올라감

Node의 http 모듈은 다양한 기능을 갖지만,
`http.createServer`라는 함수인 `HTTP 서버 컴포넌트`를 사용함
* `http.createServer` 함수: 서버로 들어오는 요청마다 호출되는 콜백을 취해서, 서버 개체를 반환함

#### 요약
* Node.js를 설치하는 방법은 다양함.
  * Node.js 사이트 다운로드
  * OS 패키지 매니저 사용
  * NVM (권장)
* Node.js의 모듈 시스템은 `require`라는 전역 함수와 `module.exports`라는 전역 개체를 사용함(CommonJS 방식).
* npm을 사용해 npm 레지스트리에서 서드파티 패키지를 설치할 수 있음.
* Node.js는 I/O 이벤트를 다룸.
  이벤트가 발생할 때(웹 요청 수신 등) 함수(또는 함수 집합)가 호출됨
* Node.js에는 `http`라는 내장 모듈이 있음.
  이 모듈은 웹 애플리케이션을 만드는 데 유용함.

### Node.js - HTTP 모듈로 서버 만들기
```js
// 1. HTTP 모듈 import
var http = require("http");

// 2. 요청 핸들러 함수 생성
var requestHandler = function(request, response) {
  if (request.url === '/') {
    response.send('Home page');
  } else if (request.url === '/other') {
    response.send('other page');
  } else {
    response.send('No page');
  }
  response.end('Hello, World!');
};

// 3. HTTP 모듈의 `createServer` 함수 사용하여, 요청 핸들러를 사용하는 서버 객체 생성
var server = http.createServer(requestHandler);

// 4. 서버가 특정 포트에서 요청 수신 대기
server.listen(3000);
```

Node.js의 http 모듈을 사용해 브라우저에서 HTTP 요청에 응답하는 HTTP 서버를 만들 수 있음. 즉, http 모듈은 Node로 웹사이트를 만들게 해줌
http 모듈에서 노출하는 API는 아주 최소한이며 많은 귀찮은 작업을 덜어주지 않음.

Express는 Node.js의 내장 HTTP 서버 위에 올라가는 추상 계층

> 아이스크림에서 바닐라는 가장 평범하고 기본적인 맛
아무것도 추가하거나 섞지 않은 순수한 것을 가리키기 위해 'vanilla'라는 단어를 사용함

### Express로 서버 만들기
```js
// 1. express, http 모듈 import
var express = require('express');
var http = require('http');

// 2. 요청 핸들러를 반환하는 express() 호출
var app = express();

// 3. 요청 핸들러에 미들웨어 등록
app.use(function(request, response) {
  console.log('In comes a request to: ' + request.url);
  response.end('Hello world!');
});

// 4. 서버 객체 생성 / 특정 포트에서 요청 수신 대기
http.createServer(app).listen(3000);
```

## Express의 주요 기능 네 가지
1. **미들웨어**: Middleware
2. **라우팅**: Routing
3. **확장**: Extensions
4. **뷰**: Views

---

# Reference
1. <i>< Express in Action ></i>
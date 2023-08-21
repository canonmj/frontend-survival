# Fetch API & CORS

## Fetch API

Fetch API는 네트워크 통신을 통해 resource를 얻기 위해 javaScript에서 접근하고 조작할 수 있는 interface를 제공한다.
HTTP Request와 Response, Header, fetch() 가 interface의 핵심이다.

- [fetch API 사용해보기](https://developer.mozilla.org/ko/docs/Web/API/Fetch_API/Using_Fetch)

- [fetch 쉬운설명](https://ko.javascript.info/fetch)

## 비동기 통신

Fetch를 통해 네트워크 통신을 하기전 비동기 통신의 개념을 알면 좋다.
개념을 간단히 말하자면 자바스크립트에서 특정 코드의 연산이 끝날 때까지 코드의 실행을 멈추지 않고 다음 코드를 먼저 실행하는 것을 의미한다.
비동기 통신을 하여 데이터를 불러오는 동안 다음 코드를 진행하면 주문 전송, 사용자 정보 읽기 등 서버에서 정보를 받아올 때, 리액트처럼 페이지 전체를 새로 고침 하지 않아도 된다.

## AJAX(Asynchronous Javascript And Xml)

브라우저가 가지고 있는 XMLHttpRequest 객체를 이용해서 전체 페이지를 새로고침 하지않고 페이지의 일부만을 위한 데이터를 가져오는 기법이다.
기존에는 XMLHttpRequest API를 통해 통신을 주고 받았지만 번거롭기도 하고 나온지 오래된 방식이라 JQuery 같은 다양한 방법이 나오게 되었다.

## 모던한 fetch()

fetch API가 ES6 표준으로 등장하면서 비동기 통신의 주요 방식으로 자리를 잡았다.

기본 문법은 다음과 같다.

```js
let promise = fetch(url, [options]);
```

- url – 접근하고자 하는 URL
- options – 선택 매개변수, method나 header 등을 지정할 수 있음

options에 아무것도 넘기지 않으면 요청은 GET 메서드로 진행되어 url로부터 콘텐츠가 다운로드 된다.
fetch()를 호출하면 브라우저는 네트워크 요청을 보내고 프라미스가 반환된다.

응답은 두 단계를 거쳐 진행된다.

### 첫번째 단계

먼저, 서버에서 응답 헤더를 받자마자 fetch 호출 시 반환받은 promise가 내장 클래스 Response의 인스턴스와 함께 이행 상태가 된다.
이 단계는 아직 본문(body)이 도착하기 전이지만, 개발자는 응답 헤더를 보고 요청이 성공적으로 처리되었는지 아닌지를 확인할 수 있다.

네트워크 문제나 존재하지 않는 사이트에 접속하려는 경우같이 HTTP 요청을 보낼 수 없는 상태에선 프라미스는 거부상태가 된다.

HTTP 상태는 응답 프로퍼티를 사용해 확인할 수 있다.

**status** – HTTP 상태 코드(예: 200)
**ok** – 불린 값. HTTP 상태 코드가 200과 299 사이일 경우 true

예시

```js
let response = await fetch(url);

if (response.ok) {
  // HTTP 상태 코드가 200~299일 경우
  // 응답 몬문을 받습니다(관련 메서드는 아래에서 설명).
  let json = await response.json();
} else {
  alert("HTTP-Error: " + response.status);
}
```

### 두번째 단계

두번째 단계에서는 추가 메서드를 호출해 응답 본문을 받는다.
response 에는 프라미스를 기반으로 하는 다양한 메서드가 있습니다. 이 메서드들을 사용하면 다양한 형태의 응답 본문을 처리할 수 있다.

> response.text() – 응답을 읽고 텍스트를 반환
> response.json() – 응답을 JSON 형태로 파싱
> response.formData() – 응답을 FormData 객체 형태로 반환
> response.blob() – 응답을 Blob(타입이 있는 바이너리 데이터) 형태로 반환
> response.arrayBuffer() – 응답을 ArrayBuffer(바이너리 데이터를 로우 레벨 형식으로 표현한 것) 형태로 반환

이 외에도 response.body가 있는데, ReadableStream 객체인 response.body를 사용하면 응답 본문을 청크 단위로 읽을 수 있다.

이 내용을 토대로 활용을 해보면

```js
let url =
  "https://api.github.com/repos/javascript-tutorial/ko.javascript.info/commits";
let response = await fetch(url);

let commits = await response.json(); // 응답 본문을 읽고 JSON 형태로 파싱함

alert(commits[0].author.login);
```

위 예시를 await 없이 프로미스만 사용하면 이렇게 바꿀 수 있다.

```js
fetch(
  "https://api.github.com/repos/javascript-tutorial/en.javascript.info/commits"
)
  .then((response) => response.json())
  .then((commits) => alert(commits[0].author.login));
```

### Promise

`fetch()` 가 반환하는 [객체](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Promise)이다. 404, 500등의 오류상태코드를 수신해도 다 받는다. 오직 네트워크 연결이 실패했을 때만 Promise가 거부된다. 응답은 200번대가 아니면 `ok: false` 되는 식으로 처리된다.

`약속하다, 장담하다`라는 뜻인데 아무래도 비동기 요청이다 보니 원래는 값이 확실치 않게 오지만 Promise를 통해 `결과 언제까지는 가져온다고 약속할게` 라는 의사를 표시하는 것이다. `await`를 쓰면 다 수행되기를 기다려서 Response 객체가 리턴된다.

[프라미스](https://joshua1988.github.io/web-development/javascript/promise-for-beginners/)

## ReadableStream

[RedableStream](https://developer.mozilla.org/ko/docs/Web/API/ReadableStream)은 byte 데이터를 읽을 수 있는 스트림 제공 인터페이스이다.

ReadableStream stream 에서는 reader를 얻어온뒤, read를 계속하면서 data chunk를 받는다. `Uint8Array` 단위 data chunk를 끝날때까지 계속 받는다.

## Unicode

이 세상의 모든 문자를 고유한 고정길이 코드로 일관되게 표현하고자 하는 [국제 문자 집합(UCS), 문자 인코딩 표준](https://home.unicode.org/about-unicode/)이다. 이전에는 표현 문자 범위도 제한 적이었고, 각각의 문자 인코딩 방식(문자를 숫자로 매핑하는 방식)이 충돌을 일으켰다. 그렇기에 통합할 필요가 생겼었고, 통일된 문자 인코딩 표준에 대한 고민에서 유니코드가 나왔다. 그리고 universal(모든 언어), uniform(고정길이), unique(1대1 코드-문자 매핑) 이 세가지 목적 때문에 uni-code라는 이름이 지어졌다.

`Uint8Array` 단위의 data chunk는 8bit-1byte로 0~255의 정보를 담을 수 있고 [UTF-8](https://ko.wikipedia.org/wiki/UTF-8)(Universal-coded character set Transformation Format 8-bit)라고 유니코드 기반 변환 포맷이 있다.

인코딩 된 `Uint8Array` chunk는 stream 이 다 끝날때까지 받고 나서 [TextDecoder](https://ko.javascript.info/text-decoder)에 의해 utf-8 형식(default)으로 decode되어야 한다. 그제서야 문자열을 사용할 수 있다.

정리하면 Unicode는 모든 문자를 고유한 숫자로 표현하는 국제 표준이고, UTF-8 은 8 bit 로 표현하는 유니코드 기반 문자 인코딩 포맷. 그리고 Uint8Array는 UTF-8 encode 된 정보가 담긴 타입 이다. TextDecoder는 byte 정보를 문자열로 바꿔준다.

## CORS 란

CORS(Cross Origin Resource Sharing)는 교차 출처 간 자원 공유를 가능하게 하는 정책이다. 다시 말하면 웹페이지 출처 아닌 아닌 다른 출처에 요청하는 걸 받아주는 정책이다. 여기서 출처란 사이트의 도메인 + port를 말한다. 원래는 [SOP(Same Origin Policy)](https://developer.mozilla.org/ko/docs/Web/Security/Same-origin_policy)라고 동일 출처에 대한 자원 요청만 할 수있도록 하는 브라우저 정책이 있기 때문에 다른 출처에 요청해서 받아와도 브라우저에서 막아버린다. 하지만 자원 요청 받는 출처 서버에서 CORS 설정을 해당 출처에 대해서 선택적을 적용해주면 브라우저에서도 해당 응답이 보여진다.

> 보면 SOP로 막고 CORS로 선택적으로 열어주는 모양새이다. 만약 SOP/CORS 없다면 어떤일이 일어날 수 있을까?

# Fetch API & CORS

## Fetch API 란

네트워크 통신을 통해 resource를 얻기 위한 interface 중 하나다. [MDN] 참고. `fetch()` 메소드를 통해 리소스를 비동기로 가져온다. `XMLHttpRequest`보다 쓰기도 편하다.

### Promise

`fetch()` 가 반환하는 [객체](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Promise)이다. 404, 500등의 오류상태코드를 수신해도 다 받는다. 오직 네트워크 연결이 실패했을 때만 Promise가 거부된다. 응답은 200번대가 아니면 `ok: false` 되는 식으로 처리된다.

`약속하다, 장담하다`라는 뜻인데 아무래도 비동기 요청이다 보니 원래는 값이 확실치 않게 오지만 Promise를 통해 `결과 언제까지는 가져온다고 약속할게` 라는 의사를 표시하는 것이다. `await`를 쓰면 다 수행되기를 기다려서 Response 객체가 리턴된다.

### ReadableStream

[RedableStream](https://developer.mozilla.org/ko/docs/Web/API/ReadableStream)은 byte 데이터를 읽을 수 있는 스트림 제공 인터페이스이다.

ReadableStream stream 에서는 reader를 얻어온뒤, read를 계속하면서 data chunk를 받는다. `Uint8Array` 단위 data chunk를 끝날때까지 계속 받는다.

### Unicode

이 세상의 모든 문자를 고유한 고정길이 코드로 일관되게 표현하고자 하는 [국제 문자 집합(UCS), 문자 인코딩 표준](https://home.unicode.org/about-unicode/)이다. 이전에는 표현 문자 범위도 제한 적이었고, 각각의 문자 인코딩 방식(문자를 숫자로 매핑하는 방식)이 충돌을 일으켰다. 그렇기에 통합할 필요가 생겼었고, 통일된 문자 인코딩 표준에 대한 고민에서 유니코드가 나왔다. 그리고 universal(모든 언어), uniform(고정길이), unique(1대1 코드-문자 매핑) 이 세가지 목적 때문에 uni-code라는 이름이 지어졌다.

`Uint8Array` 단위의 data chunk는 8bit-1byte로 0~255의 정보를 담을 수 있고 [UTF-8](https://ko.wikipedia.org/wiki/UTF-8)(Universal-coded character set Transformation Format 8-bit)라고 유니코드 기반 변환 포맷이 있다.

인코딩 된 `Uint8Array` chunk는 stream 이 다 끝날때까지 받고 나서 [TextDecoder](https://ko.javascript.info/text-decoder)에 의해 utf-8 형식(default)으로 decode되어야 한다. 그제서야 문자열을 사용할 수 있다.

정리하면 Unicode는 모든 문자를 고유한 숫자로 표현하는 국제 표준이고, UTF-8 은 8 bit 로 표현하는 유니코드 기반 문자 인코딩 포맷. 그리고 Uint8Array는 UTF-8 encode 된 정보가 담긴 타입 이다. TextDecoder는 byte 정보를 문자열로 바꿔준다.

## CORS 란

CORS(Cross Origin Resource Sharing)는 교차 출처 간 자원 공유를 가능하게 하는 정책이다. 다시 말하면 웹페이지 출처 아닌 아닌 다른 출처에 요청하는 걸 받아주는 정책이다. 여기서 출처란 사이트의 도메인 + port를 말한다. 원래는 [SOP(Same Origin Policy)](https://developer.mozilla.org/ko/docs/Web/Security/Same-origin_policy)라고 동일 출처에 대한 자원 요청만 할 수있도록 하는 브라우저 정책이 있기 때문에 다른 출처에 요청해서 받아와도 브라우저에서 막아버린다. 하지만 자원 요청 받는 출처 서버에서 CORS 설정을 해당 출처에 대해서 선택적을 적용해주면 브라우저에서도 해당 응답이 보여진다.

> 보면 SOP로 막고 CORS로 선택적으로 열어주는 모양새이다. 만약 SOP/CORS 없다면 어떤일이 일어날 수 있을까?

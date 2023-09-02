# Routes

## 라우터란?

> 웹에는 라우터에 대한 세 가지 정의가 있다.

1. 네트워크 계층의 경우 라우터는 데이터 패킷 방향을 결정하는 네트워킹 장치이다.
   **패킷(packet)** : 네트워크를 통해 전송하기 쉽도록 자른 데이터의 전송 단위이다.

2. 애플리케이션 계층의 SPA에서 라우터는 URL을 통해 표시될 웹 페이지를 결정하는 라이브러리이다. 이 미들웨어 모듈은 웹 애플리케이션에서 URL 관련 기능을 처리하는 역할을 하며, 파일 경로를 받으면 해당 파일을 렌더링하여 다음 페이지를 표시할 준비를 한다.

3. 서비스 계층의 API에서 라우터는 요청을 파싱하여 다양한 핸들러로 전달하거나 라우팅하는 소프트웨어 구성 요소이다.

[Router](https://developer.mozilla.org/en-US/docs/Glossary/Routers)

## React Router

> React Router를 통해 client side routing을 하여 서버로 추가적인 요청 없이 URL/UI를 업데이트 할 수 있다.

[React Router](https://reactrouter.com/en/main)

### Browser Router

'<BrowserRouter>'는 clean URLs을 사용하여 브라우저의 주소 표시줄에 현재 위치를 저장하고 브라우저의 내장 history 스택을 사용하여 탐색한다.

[Browser Router](https://reactrouter.com/en/main/router-components/browser-router)

### Route

React Router 앱에서 가장 중요한 부분. URL 세그먼트를 구성 요소, 데이터 로딩 및 데이터 변형에 연결한다. route nesting(경로 중첩)을 통해 복잡한 애플리케이션 레이아웃과 데이터 종속성이 단순해지고 선언적이 된다.

[Route](https://reactrouter.com/en/main/route/route)

### Memory Router

'<MemoryRouter>'는 내부적으로 locations를 배열에 저장한다. '<BrowserHistory>' 및 '<HashHistory>'와 달리 브라우저의 history 스택과 같은 외부 소스에 연결되지 않는다. 테스트처럼 기록 스택을 완벽하게 제어해야 하는 시나리오에 이상적이다.

[Memory Router](https://reactrouter.com/en/main/router-components/memory-router)

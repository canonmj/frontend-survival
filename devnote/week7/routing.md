# Routing

## HTML DOM API

> DOM API를 활용하여 HTML을 동적으로 다양하게 변경할 수 있다.

Routing 기능을 사용하기 위해 HTML DOM API 속 Location 인터페이스에 접근해야한다. 그리고 location.pathname을 통해 URL을 활용, 조작한다.

[HTML DOM API](https://developer.mozilla.org/en-US/docs/Web/API/HTML_DOM_API)

## Location

> Location 인터페이스는 연결된 object의 location(URL)를 나타낸다. Document.location 및 Window.location을 통해 이동할 수 있는 연결된 location이 있다.

### location.pathname

location.pathname은 쿼리 문자열이나 다른 조각을 포함하지 않고 URL 경로 뒤에 '/'가 포함된 문자열이다.

[Location Anatomy](https://developer.mozilla.org/en-US/docs/Web/API/Location#location_anatomy)

## History

> History 인터페이스를 사용하면 브라우저 세션 history, 즉 현재 페이지가 로드된 탭이나 프레임에서 방문한 페이지를 조작할 수 있다.

전역 object history를 통해 접근할 수 있는 history 인스턴스는 단 하나뿐(싱글톤)이다.

**싱글톤(Singleton) 패턴**: 객체의 인스턴스가 오직 1개만 생성되는 패턴을 의미한다.

[History API](https://www.zerocho.com/category/HTML&DOM/post/599d2fb635814200189fe1a7)

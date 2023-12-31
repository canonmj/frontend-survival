# REST(Representational State Transfer)

## JSON

> **javascript 객체 표기법을 활용한 data 교환용 표준 포맷.**
> 보낼때는 Stringify 해서 문자열로 만들어 전송에 용이하게 만들어 보낸다. 받은 이후에는 문자열 형태의 데이터를 parsing 해서 사용하기 좋게 JSON 객체 형식으로 변환한다. 백엔드에서는 데이터를 JSON의 형태로 받아오게 되는데, 데이터를 받기 위한 API로 REST API 방식 혹은 GraphQL이 있다.

[JSON으로 작업하기]('https://developer.mozilla.org/ko/docs/Learn/JavaScript/Objects/JSON')

## DSL (Domain-Specific Language)

> **특정 도메인에 최적화된 언어.** React에서 쓰는 DSL에는 JSX(JS XML)가 있다.
> 백엔드에서 JSON 데이터를 받아오면 프론트엔드는 이 데이터를 사용자가 볼 수 있도록 UI를 구성한다.
> React는 HTML과 유사한 모양의 DSL을 사용함으로써 선언형으로 UI를 구성할 수 있다.

## REST (Representational State Transfer)

> 웹에 존재하는 모든 자원(이미지, 데이터)에 고유한 URI를 부여하여 자원에 대한 주소를 지정하는 규칙이다.

### 구체적인 개념

1. **HTTP URI**(Uniform Resource Identifier) : 자원(Resource)을 명시하고
2. **HTTP Method**(POST, GET, PUT, DELETE) : 자원에 대한 행위(CRUD Operation)을 제공하고
3. **HTTP Message Pay Load** : 자원에 대한 행위를 요청하면 서버는 응답(Representations)을 보낸다.

### REST API

> 즉, REST API는 개발자들에게 있어 정보를 주고 받는 형식이라고 할 수 있으며, REST 기반으로 구현한 API는 일관적인 컨벤션을 통해 API의 이해도 및 호환성이 높여줄 수 있다.(RESTful)

### REST API 설계 규칙

page 기준이 아닌 resource 기준으로 작성한다.

```js
   Bad http://127.0.0.1:8000/main-page-product 
   Good  http://127.0.0.1:8000/products
```

URI는 동사보다는 명사를, 대문자보다는 소문자를 사용하여야 한다.

```js
   Bad http://127.0.0.1:8000/Delete-post/1
   Good  http://127.0.0.1:8000/post/1
```

마지막에 슬래시 (/)를 포함하지 않는다.

```js
   Bad http://127.0.0.1:8000/test/  
   Good  http://127.0.0.1:8000/test
```

언더바 대신 하이폰을 사용한다.

```js
   Bad http://127.0.0.1:8000/test_blog
   Good  http://127.0.0.1:8000/test-blog
```

파일확장자는 URI에 포함하지 않는다.

```js
   Bad http://127.0.0.1:8000/photo.jpg  
   Good  http://127.0.0.1:8000/photo
```

## GraphQL은 왜 등장했는가?

> 처음부터 Rest API는 우리에게 익숙하기도 하지만 규칙이 정해져있고, 합리적이라는 생각도 드는데 왜 GraphQL이라는 새로운 형식이 등장했을까?

### RestAPI의 한계점

- **overfetching**

회사에서 어떤 페이지를 구현할때 팀의 매니저와 오피스만을 필요로 한다고 하자. 그런데 rest api를 통해 /api/teams로 요청하면 내가 원하는 정보 외에도 해당 팀이 현재 진행중인 프로젝트, 팀의 마스코트 정보까지 넘어온다.
이런 경우 데이터를 주고 받는 팀의 갯수가 많아질수록 네트워크 비용이 커질것이다.

- **underfetching**

반대로 내가 원하는 정보들이 여러 계층에 걸쳐 있을 수도 있다. 만약 팀에 대한 정보와 사람에 대한 정보가 한꺼번에 필요한데 팀을 받아오는 api, 사람을 받아오는 api가 따로 있다면 두개의 요청을 보내야한다. 이렇게 한번의 요청에 정보가 충분하지 않은 현상이 발생하는 경우도 있다.

> 즉, 최소한의 요청으로 내가 원하는 데이터만 받아와서 네트워크 낭비를 방지하고 싶다는 요구가 늘어남에 따라 graphql이 등장했다.

## REST API vs GraphGL

GraphQL은 요청이 복잡하고 데이터는 효율적인 반면, REST API는 요청은 단순하고 데이터는 복잡하다. 이러한 특징들로 GraphQL의 장점은 다음과 같이 정리 할 수 있다.

1. HTTP 요청 횟수를 줄일 수 있다.RESTful의 경우 필요한 리소스 별로 요청해야하고, 필요한 데이터들이 부분적으로 나눠서 개발되어 있다면 그만큼 요청 횟수가 늘어난다. 하지만 GraphQL은 원하는 정보를 하나의 쿼리에 모두 담아 요청 할 수 있다.

2. HTTP 응답 사이즈를 줄일 수 있다. Restful의 경우 응답의 형태가 정해져있기 때문에 필요한 정보만 부분적으로 요청하는 것이 힘들고, 자연스럽게 데이터의 사이즈가 클 수 밖에 없다. Facebook이 GraphQL을 개발한 초기 이유 중 하나는 모바일 사용의 증가라고 한다. GraphQL을 사용함으로써 응답 데이터 사이즈를 최소화하여 모바일 환경의 부담을 줄일 수 있다.

3. 프론트엔드와 백엔드 개발자의 부담을 덜 수 있다. Restful API를 사용한다면 프론트엔드 개발자는 API의 request/response 형식에 의존하게 된다. 따라서 새로운 엔드포인트를 효율적이게 개발하기 위해서는 프론트엔드와 백엔드 개발자의 커뮤니케이션이 강제되는 경우가 많았다.하지만 GraphQL은 request/response 의존도가 많이 없기 때문에, 개발자들의 API 개발 부담을 덜 수 있다.

### 그렇다면 GraphQL에는 단점이 없을까?

1. 고정된 요청과 응답만 필요할 때에는 query로 인해 요청의 크기가 Restful보다 커질 수 있다.

2. 캐싱이 REST보다 복잡하다.

3. 파일 업로드 구현 방법이 정해져있지 않아 직접 구현해야 한다.

## 결론

> REST가 가지는 한계 때문에 개발된 GraphQL이지만, GraphQL이 완벽하게 REST를 대체 할 수는 없다.
> REST에 더 적합한 서비스에는 REST가 사용되는 것이 바람직하므로, GraphQL과 REST의 장단점을 파악해 서비스에 맞는 방식을 고르는 것이 중요하다.

# 3. HTTP 메시지

이번 장에 배울 내용.

- 메시지가 어떻게 흘러가는가
- HTTP 메시지의 세 부분(시작줄, 헤더, 개체 본문)
- 요청과 응답 메시지의 차이
- 요청 메시지가 지원하는 여러 기능(메서드)들
- 응답 메시지가 반환하는 여러 상태 코드들
- 여러 HTTP 헤더들은 무슨 일을 하는가

# 3.1. 메세지의 흐름

HTTP 메시지는 HTTP 애플리케이션 간 주고 받은 데이터의 블록.

이 블록은 내용과 의미를 설명하는 텍스트 메타 정보로, 선택적으로 데이터도 올 수 있다.

메시지는 방향성이 있어 방향을 의미하는 용어가 쓰인다(인바운드, 아웃바운드, 업스트립, 다운스트림)

## 3.1.1. 메시지는 원 서버 방향을 인바운드로 하여 송신된다.

클라이언트에서 서버로 가는 방향이 인바운드, 반대를 아웃바운드라 한다.

## 3.1.2. 다운스트림으로 흐르는 메시지

클라이언트에서 요청을 보내든, 서버에서 응답을 보내는 목적지의 방향은 다운스트림이라 한다.

Proxy1 입장에서 Proxy2는 요청을 보낼 때는 다운스트림 방향이지만, 응답을 보낼 때는 업스트림이다.

![스크린샷 2023-04-25 오후 9.26.38.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/22d8cd15-7eb8-4b05-a33a-3cbc1729c0b3/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-04-25_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_9.26.38.png)

# 3.2. 메시지의 각 부분

시작줄, 헤더 블록, 본문 세 부분으로 구성된다.

HTTP 메시지는 어디가 헤더고 본문인지 명시하지 않지만 줄바꿈으로 구간을 구부한다. 줄 바꿈을 의미하는 특별한 문자열 CRLF(Carrage Return Line Feed)가 있는데 견고한 애플리케이션이라면 그냥 개행 문자도 받아들일 수 있어야 한다고 한다.

이 문자열은 ASCII 13(Carriage Return)과 ASCII 10(Line Feed)이다.

![스크린샷 2023-04-25 오후 9.29.38.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/462db16e-c18b-49b0-969d-52873c675251/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-04-25_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_9.29.38.png)

- 시작줄: HTTP/1.0 프로토콜로 응답 됐으며 상태 값은 200 메시지는 OK
- 헤더: 본문의 내용은 일반 텍스트고 길이가 19
- 본문: HI~ 라는 메시지

## 3.2.1. 메시지 문법

### 요청 메시지의 형식

```tsx
<메서드> <요청URL> <버전> 
<헤더>

<엔터티 본문>
```

### 응답 메시지의 형식(시작줄만 다름)

```tsx
<버전> <상태코드> <사유구절> 
<헤더>

<엔터티 본문>
```

### 설명

- 메서드
    
    GET, POST, PUT, HEAD 등 행위
    
- 요청 URL
    
    완전한 URL 또는 경로로 자원을 나타냄
    
- 버전
    
    HTTP 버전으로 HTTP/<메이저 버전>.<마이너 버전> 으로 나타냄
    
- 상태 코드
    
    요청 중 무엇이 일어났는지 설명하는 세 자리 숫자.
    
- 사유 구절
    
    숫자로 된 상태를 설명하는 메세지
    
- 헤더들
    
    optional 한 정보들로 메타나 
    
- 엔티티 본문
    
    임의의 바이너리 블록으로 많은 양의 데이터를 넣을 수 있는 공간. 없을 수도 있는데 이때에도 헤더가 끝났음을 알리는 CRLF 문자를 빼먹으면 안된다.
    

![스크린샷 2023-04-25 오후 9.37.15.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6a4595da-9458-4822-9961-71180510f0bd/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-04-25_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_9.37.15.png)

## 3.2.2. 시작줄

요청 메세지는 무엇을 해야 하는지, 응답 메시지는 무슨 일이 있어 났는지 말해준다.

### 요청줄

요청 메시지에서 무엇을 원하는지 나타낸다.

메서드는 GET, 리소스는 /test/hi-there.txt이고 프로토콜 버전은 HTTP/1.1. 요청 줄이라고도 한다.

### 응답줄

응답 메시지에서 보내는 수행 결과와 상태 정보, 결과 메시지.

응답한 프로토콜 버전과 상태 값 사유를 나타낸다.

### 메서드

`GET` 메소드는 특정 리소스의 표시를 요청합니다. `GET`을 사용하는 요청은 오직 데이터를 받기만 합니다.

`HEAD` 메소드는 `GET` 메소드의 요청과 동일한 응답을 요구하지만, 응답 본문을 포함하지 않습니다.

`POST` 메소드는 특정 리소스에 엔티티를 제출할 때 쓰입니다. 이는 종종 서버의 상태의 변화나 부작용을 일으킵니다.

`PUT` 메소드는 목적 리소스 모든 현재 표시를 요청 payload로 바꿉니다.

`DELETE` 메소드는 특정 리소스를 삭제합니다.

`CONNECT` 메소드는 목적 리소스로 식별되는 서버로의 터널을 맺습니다.

`OPTIONS` 메소드는 목적 리소스의 통신을 설정하는 데 쓰입니다.

`TRACE` 메소드는 목적 리소스의 경로를 따라 메시지 loop-back 테스트를 합니다.

`PATCH` 메소드는 리소스의 부분만을 수정하는 데 쓰입니다.

책에서는 OPTIONS은 어떤 메서드를 수행할 수 있는지 확인 한다고 돼 있다. 본격적인 요청을 하기 전에 OPTIONS 메소드를 보내는 것도 있다.

### 상태코드

100 - 정보 응답

200 - 성공 응답

300 - 리다이렉션 메시지

400 - 클라이언트 오류 응답

500 - 서버 오류 응답

### 사유구절

### 버전 번호

# 3.3. 메서드

## 3.3.1. 안전한 메서드

GET, HEAD를 안전한 메서드라 하는데, 서버에 어떤 데이터도 저장하지 않고 결과도 반영하지 않기 때문이다.

- GET - 리소스를 요청
- HEAD - 본문을 포함하지 않는 GET으로 리소스를 다 가져오지 않고 헤더나 시작줄의 정보만 알아볼 때 사용
- PUT - 전통적으론 리소스 위치에 사용자가 입력한 인터티 바디 내용을 넣은 페이지를 생성하는 것
- POST - 사용자가 입력한 데이터를 전송
- TRACE - 클라이언트에서 TRACE 메시지를 보내면 중간에 프록시 같은 애들이 헤더 쪽에 값을 넣어주고 서버쪽에서 루프백 진단을 한 다음 본문에 내용을 넣어 클라이언트에게 전달한다. 이때 클라이언트는 인터티 바디를 사용하지 않았다.
- OPTIONS - 사용 가능한 메서드를 알려준다.
- DELETE - 데이터를 지운다.

# 추가 내용
# REST API

REST는 분산 하이퍼미디어 시스템(ex. web)을 위한 아키텍처 스타일(제약조건의 집합)

HTTP를 기반으로 클라이언트가 서버의 리소스에 접근하는 방식을 규정한 아키텍처

API는 서로 다른 프로그램간(ex. Server와 Cleint간)의 소통 방법

REST API는 REST를 기반으로 서비스 API를 구현한 것을 의미

REST의 기본 원칙을 성실히 지킨 서비스 디자인을 RESTful이라고 표현

# 1. REST API의 구성

| 구성 요소 | 내용 | 표현 방법 |
| --- | --- | --- |
| 자원(resource) | 자원 | URI(엔드포인트) |
| 행위(verb) | 자원에 대한 행위 | HTTP 요청 메서드 |
| 표현(representations) | 자원에 대한 행위의 구체적 내용 | 페이로드 |

# 2. REST API 설계 원칙

REST에서 가장 중요한 기본적인 원칙은 URI는 리소스를 표현하는 데 집중하고 행위에 대한 정의는 HTTP 요청 메서드를 통해 하는 것.

### 1. URI는 리소스를 표현.

URI는 리소스를 표현하는 데 중점을 둬야 하고 리소스를 식별할 수 있는 이름은 명사를 사용한다.

주로 컬렉션과 엘리먼트(고유식별자)를 활용하여 엘리먼트 같은 경우 주로 id 값을 사용하는 것이 일반적이다.

```markdown
# bad
GET /getTodos/1
GET /todos/show/1
POST /setTodos
POST /deleteTodos

# good
GET /todos
GET /todos/1
```

**자원 표현을 위한 url 설계 규칙**

1. 명사 작성 추천
2. 소문자만 사용
3. 하위 문서를 나타낼 떈 /
4. 언더바는 쓰지 않고, 필요시 하이픈을 쓰되 지양
5. 확장자를 표지하지 않음
6. 자료 하나당 하나의 URL

추가로 검색이나 필터링을 하기 위해선 쿼리 스트링을 사용한다.

### 2. 리소스에 대한 행위는 HTTP 요청 메서드로 표현.

HTTP 요청 메서드는 요청 하는 쪽이 요청 받는 쪽에게 요청의 종류와 목적(리소스에 대한 행위)을 알리는 방법으로 CRUD를 위해 활용된다.

| HTTP 요청 메서드 | 종류 | 목적 |  페이로드 |
| --- | --- | --- | --- |
| GET | index/retireve | 모든/특정 리소스 취득 | X |
| POST | create | 리소스 생성 | O |
| PUT | replace | 리소스의 전체 교체 | O |
| PATCH | modify | 리소스의 일부 수정 | O |
| DELETE | delete | 모든/특정 리소스 삭제 | X |
- GET
    
    서버 자원을 가져오고자 할 때 사용되며 요청의 본문에 데이터를 넣지 않는다. 데이터를 서버로 보내야 한다면 쿼리스트링을 사용한다.
    
- POST
    
    서버에 자원을 새로 등록하고자 할 때, 요청의 본문에 데이터를 넣어 보낸다.
    
- PUT
    
    서버의 자원을 요청의 본문에 들어 있는 자원으로 치환하고자 할 때 사용한다.
    
- PATCH
    
    서버의 자원의 일부만 수정하고자 할 때  요청의 본문에 일부 수정할 데이터를 넣어 보낸다.
    
- DELETE
    
    서버의 자원을 삭제하고자 할 때 사용하며 요청의 본문에 데이터를 넣지 않는다.
    
- OPTIONS
    
    요청을 하기 전에 통신 옵션을 설명하기 위해 사용한다. 주로 웹소켓이나 CORS에 대한 확인을 사용할 때 사용된다.
    

추가로 POST, PUT, PATCH 같이 페이로드가 필요한 메서드는 미디어 타입(Media type, MIME type, Content type)을 지정해저야한다.

리소스에 대한 행위는 HTTP 요청 메서드를 통해 표현하며 URI는 리소스 표현하는데 중점을 둬야한다.

```markdown
# bad
GET /todos/delete/1

#good
DELETE /todos/1
```

### 3. 객체들의 관계 인터렉션을 위해 URL를 사용할 수 있다.

```markdown
# 영화에 출연하는 배우들의 데이터를 얻고 싶다면?
GET /movies/1/actors
```

# 3. 요약

주소는 의미를 명확히 전달하기 위해 명사로 구성한다.

명사만 있다면 무슨 동작을 행하라는 것인지 알기 어려우므로 http 요청 메서드를 사용한다.

좋은 REST API 예시

```markdown
www.kakaocomerce.com/api/v4/products/111809
instagram.com/explore/tags/kpop
instagram.com/explore/tags/food
facebook.com/natgeo/posts
facebook.com/bbc/photos
facebook.com/nbc/videos
```

# 4. REST를 구성하는 스타일

- Client-Server 구조
    - REST  서버는 API 제공, 클라이언트는 사용자 인증, 컨텍스트(세션, 로그인 정보)등을 직접 관리하는 구조로 각각 역할이 구분되기 때문에 클라이언트와 서버에서 개발해야 할 내용이 명확해지고 서로간 의존성이 줄어든다.
    - 즉, Cleint는 요청만 Server는 응답만 해야한다.
- Stateless
    - REST는 무상태성 성격을 갖는다. 즉 작업을 위한 상태정보를 따로 저장하고 관리하지 않는다. 세션 정보나 쿠키정보를 별도로 저장하고 관리하지 않기 때문에 API 서버는 들어오는 요청만을 단순히 처리한다. 그래서 서비스의 자유도가 높아지고 서버에서 불필요한 정보를 관리하지 않음으로써 구현이 단순해진다.
    - 즉, 요청 A가 있고 요청 B가 있으면 의존성이 없어야한다.
- Cacheable
    - REST의 가장 큰 특징 중 하나는 HTTP라는 기존 웹표준을 그대로 사용하기 때문에, 웹에서 사용하는 기존 인프라를 그대로 활용이 가능하다. 따라서 HTTP가 가진 캐싱 기능을 사용할 수 있다. HTTP 프로토콜 표준에서 사용하는 Last-Modified태그나 E-Tag를 이용하면 캐싱을 구현할 수 있다.
    - 즉, 서버에서 보내주는 정보들은 캐싱이 가능해야하며 캐싱을 위한 버전 같은 것도 관리를 잘 해야한다.
    - 사실 브라우저가 알아서 해준다. (자주 쓰이는 이미지 파일(특히 로고), CSS파일 등)
- Layered system
    - REST 서버는 다중 계층으로 구성될 수 있으며 보안, 로드 밸런싱, 암호화 계층을 추가해 구조상의 유연성을 둘 수 있고 PROXY, 게이트웨이 같은 네트워크 기반의 중간매체를 사용할 수 있게 한다.
    - ex. Browser → Web Server → Server? → DB
- Code-on-Demand (optional)
    - 서버에서 코드를 작성하여 클라이언트로 보내면 실행 할 수 있어야 한다.
- U**niform interface**
    - Uniform Interface는 URI로 지정한 리소스에 대한 조작을 통일되고 한정적인 인터페이스로 수행하는 아키텍처 스타일을 말한다.
    

사실  Uniform interface를 제외한 나머지는 HTTP를 사용하면 지켜지는 내용이다. 하지만 대부분의 REST API는 Uniform interface를 만족하지 못해서 RESTful하지 않다.

## 4.1. Uniform Interface의 제약 조건

- Identifaction of resources
    
    → 리소스는 URI로 식별 가능해야 한다.
    
- Manipulation of resources through representations
    
    → Representation 전송을 통해 리소스를 조작해야한다.
    
    ```markdown
    # Req
    Content-Type: text/plain
    Content-Language: en
    
    # Res
    Hello
    ```
    
    ```markdown
    #Req
    Content-Type: text/html charset=UTF-8
    Content-Language: ko
    data(payload): "존댓말"
    
    # Res
    안녕하세요.
    ```
    
- **self-descriptive messages**
- **hypermedia as the engine of application state (HATEOAS)**

여기서도 대부분 위 2가지는 잘 지키지만 아래 2가지는 잘 못지킨다.

### 4.1.1. **self-descriptive messages**

메세지는 스스로를 설명해야한다.

```markdown
# bad - 목적지(도메인)가 빠져있다.
GET / HTTP/1.1

# good
GET / HTTP/1.1
Host: www.example.org
```

```markdown
# bad - 클라이언트가 어떤 문법으로 작성 됐는지 해석을 못한다.
HTTP/1.1 200 OK

[ { "op": "remove", "path": "/a/b/c } ]

# good - 메시지가 뭔진 모른다.
HTTP/1.1 200 OK
Content-Type: apllication/json
[ { "op": "remove", "path": "/a/b/c } ]

# great - 해당 명세를 찾아가서 메세지를 해석한다. (https://datatracker.ietf.org/doc/html/rfc6902)
HTTP/1.1 200 OK
Content-Type: apllication/json-patch+json
[ { "op": "remove", "path": "/a/b/c } ]
```

### 4.1.2.  HATEOAS

애플리케이션의 상태(State)는 Hyperlink를 이용해 전이되어야한다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c03d401e-7b6d-476c-9857-41ec53578691/Untitled.png)

```markdown
HTTP/1.1 200 OK
Content-Type: text/html

<html>
<head></head>
<body><a href="/test">test</a></body>
</html>
```

```markdown
HTTP/1.1 200 OK
Content-Type: application/json
Link: </articles/1>; rel="previous",
      </articles/3>; rel="next;
# Link 헤더가 이전, 다음 리소스를 가리킨다.
# https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Link
      
{
    "title": "The second article",
    "contents": "blah blah..."
}
```

## 4.2. 왜 Uniform Interface의 제약조건을 지켜야 하는가?

- 독립적 진화
    - 서버와 클라이언트가 각각 독립적으로 진화한다.
    - 서버의 기능이 변경되어도 클라이언트를 업데이트할 필요가 없다.
    - REST를 만들게 된 계기: “How do I improve HTTP without breaking the Web.”
- 독립적 진화 예시
    - 웹 페이지를 변경했다고 웹 브라우저를 업데이트할 필요는 없고 반대도 마찬가지다.
    - HTTP, HTML 명세가 변경돼도 웹은 잘 동작한다.

### 상호운용성에 대한 예시

- Referer 오타 안 고침(referrer)
- charset 잘못 지은 이름이지만 안고침(encoding이 더 명확)
- HTTP 상태 코드 418 포기([https://developer.mozilla.org/ko/docs/Web/HTTP/Status/418](https://developer.mozilla.org/ko/docs/Web/HTTP/Status/418))
- HTTP/0.9 지원(Chrome, FireFox)

 But. 서버 기능이 변경 됐을때 모바일 앱의 경우 지원해주는데 한계가 있어서 업데이트를 해야한다ㅠㅠ

### REST가 웹의 독립적 진화에 도움을 준 예시

- HTTP에 지속적으로 영향을 줌
- Host 헤더 추가
- 길이 제한을 다루는 방법이 명시 (414 URI Too Long 등)
- URI에서 리소스의 정의가 추상적으로 변경됨: "식별하고자 하는 무언가"
- 기타 HTTP와 URI에 많은 영향
- HTTP/1.1 명세 최신판에서 REST에 대한 업급이 생김
- Reminder: Roy T. Fielding이 HTTP와 URI 명세의 저자 중 한명으로 두 곳 다 참여를 했기 때문에 영향을 받은 걸지도..

### 독립적 진화에 어떤 도움이 됐을까?

- Self-descriptive 확장 가능한 커뮤니케이션
    
    서버나 클라이언트가 변경되더라도 오고가는 메시지는 언제나  self-descriptive 하므로 언제나 해석이 가능하다.
    
- HATEOAS 애플리케이션 상태 전이의 late binding
    
    어디서 어디로 전이가 가능한지 미리 결정되지 않는다. 어떤 상태로 전이가 완료되고 나서야 그 다음 전이될 수 있는 상태가 결정된다. 즉 링크는 동적으로 변경될 수 있다.
    

하지만 시스템 전체를 통제할 수 있거나(상급자거나 클라이언트, 서버 동시 개발중일때) 진화에 관심이 없다면 굳이~ REST를 따지면서 할 필요는 없다.

## 4.3. 왜 API는 REST를 잘 만족 못하나?

|  | HTML | JSON |
| --- | --- | --- |
| Hyperlink(HATEOAS) | 됨 (anchor 태그) | 정의되어있지 않음 |
| Self-descriptive | 됨 (HTML 명세) | 불완전 |

### 4.3.1. HTML

```markdown
GET /todos HTTP/1.1
Host: example.org

HTTP/1.1 200 OK
Content-Type: text/html

<html>
<body>
<a href="https://todos/1">회사 가기</a>
<a href="https://todos/2">집에 가기</a>
</body>
</html>
```

1. Self-descriptive (만족)
    1. 응답 메시지의 Content-Type을 보고 media type이 text/html 임을 확인한다.
    2. HTTP 명세에 media type은 IANA에 등록되어있다고 하므로, IANA에서 text/html의 설명을 찾는다.
    3. IANA에 따르면 text/html의 명세는 [http://www.w3.org/TR/html](http://www.w3.org/TR/html) 이므로 링크를 찾아가 명세를 해석한다.
    4. 명세에 모든 태그의 해석방법이 구체적으로 나와있으므로 이를 해석하여 문서 저자가 사용자에게 해 주려고 했던 모든 일들을 해줄 수 있다.
2. HTATOAS (만족)
    
    태그를 이용해 표현된 링크를 통해 다음 상태로 전이될 수 있으므로 HATEOAS를 만족한다.
    

### 4.3.1. JSON

```markdown
GET /todos HTTP/1.1
Host: example.org

HTTP/1.1 200 OK
Content-Type: application/json

[ 
  {"id": 1, "title": "회사 가기"},
  {"id": 2, "title": "집에 가기"}
]
```

1. Self-descriptive (불만족)
    1. 응답 메시지의 Content-Type을 보고 media type이 application/json 임을 확인한다.
    2. HTTP 명세에 media type은 IANA에 등록되어있다고 하므로, IANA에서 application/json의 설명을 찾는다.
    3. IANA에 따르면 application/json의 명세는 draft-ietf-jsonbis-rfc7159bis-04 이므로 링크를 찾아가 명세를 해석한다.
    4. 명세에 json 문서를 파싱하는 방법이 명시되어 있으므로 성공적으로 파싱에 성공한다. 그러나 “id”가 무엇을 의미하고, “title”이 무엇을 의미하는지 알 방법은 없다.
2. HTATOAS (만족)
    
    다음 상태로 전이할 링크가 없다.
    

## 4.4. REST API로 고쳐보자

### 4.4.1. **Self-descriptive**

- 방법 1: Media Type을 이용
    
    ```markdown
    GET /todos HTTP/1.1
    Host: example.org
    
    HTTP/1.1 200 OK
    Content-Type: application/vnd.todos+json
    
    [ 
      {"id": 1, "title": "회사 가기"},
      {"id": 2, "title": "집에 가기"}
    ]
    ```
    
    1. 미디어 타입을 하나 정의한다.
    2. 미디어 타입 문서를 작성한다. 이 문서에 “id”가 뭐고 “title”이 뭔지 의미를 정의한다.
    3. [IANA](https://www.iana.org/form/media-types)에 미디어 타입을 등록한다. 이 때 만든 문서를 미디어 타입의 명세로 등록한다.
    4. 이제 이 메시지를 보는 사람은 명세를 찾아갈 수 있으므로 이 메시지의 의미를 온전히 해석할 수 있다.
    5. 단점: 매번 미디어 타입을 정의해야 한다.
- 방법 2: Link 헤더에 Profile relation
    
    ```markdown
    GET /todos HTTP/1.1
    Host: example.org
    
    HTTP/1.1 200 OK
    Content-Type: application/json
    Link: <https://example.org/docs/todos>; rel="profile"
    
    [ 
      {"id": 1, "title": "회사 가기"},
      {"id": 2, "title": "집에 가기"}
    ]
    ```
    
    1. “id”, “title”의 의미를 정의한 명세를 작성한다.
    2. Link 헤더에 Profile relation으로 해당 명세를 링크한다.
    3. 이제 메시지를 보는 사람은 명세를 찾아갈 수 있으므로 이 문서의 의미를 온전히 해석할 수 있다.
    4. 단점: 클라이언트가 Link 헤더(RFC 5988)와 profile(RFC 6906)을 이해해야 하고, Content negotiation을 할 수 없다.

### 4.4.2. **HATEOAS**

- 방법1: Data로 표현하기
    
    ```markdown
    GET /todos HTTP/1.1
    Host: example.org
    
    HTTP/1.1 200 OK
    Content-Type: application/json
    Link: <https://example.org/docs/todos>; rel="profile"
    
    [ 
      {
        "link": "https://example.org/todos/1",
        "title": "회사 가기"
      },
      {
        "link": "https://example.org/todos/2",
        "title": "집에 가기"
      }
    ]
    ```
    
    1. data에 다양한 방법으로 하이퍼링크를 표현한다.
        
        ```markdown
        GET /todos HTTP/1.1
        Host: example.org
        
        HTTP/1.1 200 OK
        Content-Type: application/json
        Link: <https://example.org/docs/todos>; rel="profile"
        
        {
          "links": {
            "todo": "https://example.org/todos/{id}"
          },
          "data": [{
            "id": 1,
            "title": "회사 가기"
          }, {
            "id": 2,
            "title": "집에 가기"
          }]
        ]
        ```
        
        단점: 링크를 표현하는 방법을 직접 정의해야 한다.
        
    2. JSON으로 하이퍼링크를 표현하는 방법을 정의한 명세들을 활용한다. (JSON API, HAL, UBER, Siren, Collection+json …)
        
        ```markdown
        GET /todos HTTP/1.1
        Host: example.org
        
        HTTP/1.1 200 OK
        Content-Type: application/vnd.api+json
        Link: <https://example.org/docs/todos>; rel="profile"
        
        {
          "data": [{
            "type": "todo",
            "id": "1",
            "attributes": { "title": "회사 가기" },
            "links": { "self": "http://example.com/todos/1" }
          }, {
            "type": "todo",
            "id": "2",
            "attributes": { "title": "회사 가기" },
            "links": { "self": "http://example.com/todos/2" }
          }]
        }
        ```
        
        단점: 기존 API를 많이 고쳐야한다. (침투적)
        
- 방법2: HTTP 헤더로 표현하기
    - Link, Location 등의 헤더로 링크를 표현한다.
        
        ```markdown
        POST /todos HTTP/1.1
        Content-Type: application/json
        
        {
            "title": "점심 약속"
        }
        
        HTTP/1.1 204 No Content
        Location: /todos/1
        Link: </todos/>; rel="collection"
        ```
        
        단점: 정의된 relation만 활용한다면 표현에 한계가 있다.
        

# 5.  번외

### HTML Form 에서 GET/POST 만 지원하는 이유

- Form은 서버에 정보를 제출하기 위해 존재
- GET, POST는 Form에 존재하는 대화형 컨트롤에 입력된 값을 보냄
- GET은 정보를 줄테니 나에게 리소스(representation)를 달라고 요청하는 것이고, POST는 정보를 줄테니 이걸로 처리를 요청한다는 뜻.
- DELETE에서 URI는 리소스를 정확하게 식별하고 요청을 보내기 때문에 Form을 통해 값을 전달할게 없고, 권한을 획득하기 위해서 header를 이용해야 함
- PUT도 전송해야할 것은 대상 리소스를 대체할 Representaton 그 자체인데, 조각난 Form 데이터를 처리하라고 보낼 수는 없다.

### GET/POST로 수정, 삭제를 구현할 수 있는데 PUT/DELETE가 존재하는 이유

- REST API는 자원, 행위, 표현으로 구성되는데 PUT/DELETE가 없다면 아래와 같이 표현해야 한다.
    
    ```markdown
    POST /update/board/1
    POST /replace/board/1
    GET /delete/board/1
    GET /remove/board/1
    ```
    
- 하지만 자원에 해당하는 URI는 인터넷에 있는 자원을 나타내는 유일한 주소여야 하는데 위 표현은 그러지 못하다. (/board/1 로 통일돼야 한다.)
    
    ```markdown
    GET /board/1
    PUT /board/1
    DELETE /board/1
    ```
    

### REST API의 대안이 있는가?

REST API는 요청하는 자원에 따라 개수가 늘어나는게 귀찮다.

이것에 대한 대안으로 GraphQL이 있다.
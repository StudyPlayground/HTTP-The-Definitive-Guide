# 1. HTTP 개관

전 세계의 웹브라우저, 서버, 웹 앱은 모두 HTTP를 통해 서로 대화한다.

이번 장에서 배울 것은 아래와 같다.

- 얼마나 많은 클라이언트와 서버가 통신하는지
- 리소스(웹 콘텐츠)가 어디서 오는지
- 웹 트랜잭션이 어떻게 동작하는지
- HTTP 통신을 위해 사용하는 메시지의 형식
- HTTP 기저의 TCP 네트워크 전송
- 여러 종류의 HTTP 프로토콜
- 인터넷 곳곳에 설치된 다양한 HTTP 구성요소

# 1.1. HTTP: 인터넷 멀티미디어 배달부

HTTP는 전 세계의 웹서버로부터 정보(HTML 페이지, 이미지 파일 등)를 웹 브라우저로 옮겨준다.

HTTP는 신뢰성을 보장하기 때문에 데이터 누락에 대한 걱정을 하지 않아도 된다.

# 1.2. 웹 클라이언트와 서버

HTTP 프로토콜로 의사소통 하는 두개의 주체가 있다. 요청을 하고 응답 받는 쪽을 HTTP 클라이언트, 반대는 HTTP 서버라 한다.

# 1.3. 리소스

웹 서버는 웹 리소스를 관리, 제공한다. 웹 리소스는 웹에 콘텐츠를 제공하는 모든 것을 말한다. 인터넷 검색엔진 역시 리소스다.

## 1.3.1. 미디어 타입

인터넷은 수많은 데이터 타입을 다루기 때문에, HTTP는 웹에서 전송되는 객체에 데이터 포맷 라벨인 MIME 타입을 붙인다. 원래는 이메일간 데이터 타입을 지정하기 위해 사용했지만 워낙 잘 동작해 HTTP에서도 채택했다.

MINE 타입은 주 타입(primary object type)과 부 타입(specific subtype)을 사선으로 구분한다.

- HTML → text/html
- jpeg → image/jpeg

## 1.3.2. URI

통합 자원 식별자(URI, Uniform resource identifier)를 통해 원하는 리소스의 위치를 찾을수 있다.URI에는 URL과 URN 이 있다.

### 1.3.2.1. URL(통합 자원 지시자, uniform resource locator)

특정 서버의 리소스에 대한 구체적인 위치를 서술한다.

크게 세 부분으로 이뤄진 표준 포맷을 따른다.

1. 스킴: 리소스 접근을 위한 프로토콜, 보통 HTTPS 프로토콜(https://)이다.
2. 호스트: 인터넷 주소(www.kakao.com)
3. 리소스 (/images/logo)

예시)

- https://www.kakao.com/index.html → 카카오의 URL
- https://www.naver.com/images/logo.webp → 네이버 로고 URL

### 1.3.2.2. URN(uniform resource name)

이름으로 접근한다.

# 1.4. 트랜젝션

클라이언트 기준으로 요청 명령에 대한 응답 결과 쌍을 HTTP 트랜젝션이라 한다.

## 1.4.1. 메서드

요청이 어떤 행위를 할 것인지 나타낸다. (GET, POST, HEAD, PUT, DELETE, …)

## 1.4.2. 상태 코드

요청에 대한 상태를 나타내는 세자리 숫자(200, 302, 404, …)

## 1.4.2. 웹페이지는 여러 객체로 이뤄질 수 있다.

하나의 웹페이지는 여러 HTTP 트랙젝션을 수행해 얻은 리소스로 만든다.

# 1.5. 메시지

HTTP 메시지는 이진 형식이 아닌 단순한 줄 단위의 텍스트로 사람이 읽고 쓰기 쉽다.

![스크린샷 2023-04-13 오후 8.56.41.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/70becf63-676e-420d-819b-5b970d14b7c9/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-04-13_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_8.56.41.png)

- 시작줄 - 메시지의 첫 줄은 시작줄이다. 요청이라면 무엇을 해야 하는지, 응답이라면 무슨 일이 일어났는지 나타낸다.
- 헤더 - 0개 이상의 헤더 필드로 구성돼 빈 줄로 끝난다. 콜론으로 구분된 하나의 이름과 값으로 구성된다. (빈 줄로 끝나는 것을 헤더 다음에 개행으로 보는 시각도 있다)
- 본문 - 요청, 응답 데이터로 이진 데이터도 사용 가능해 다양한 포멧을 넣을수 있다.

예시

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6e123072-e67a-46fb-890b-4cbd7bcdeb59/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8f62bd3f-15a3-4663-9266-df338d2e213f/Untitled.png)

# 1.6. TCP 커넥션(Transmission Control Protocol)

## 1.6.1. TCP/IP

HTTP는 애플리케이션 계층 프로토콜로, 네트워크 통신의 핵심적인 세부사항에 대해선 신경쓰지 않고 신뢰성 있는 전송 프로토콜인 TCP/IP에게 맡긴다.

TCP는 다음을 제공한다.

- 오류 없는 데이터 전송
- 순서에 맞는 전달
- 조각나지 않는 데이터 스트림

![스크린샷 2023-04-13 오후 9.13.26.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/543b5f1b-03c9-40f1-bc2b-02a056f9d6fe/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-04-13_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_9.13.26.png)

## 1.6.2. 접속, IP 주소 그리고 포트번호

HTTP 클라이언트가 서버에 메시지를 전송하기 전에 TCP/IP 커넥션을 맺어야 한다. 이때 IP 주소와 포트번호를 사용한다. 

IP와 포트번호를 이용해 다음과 같은 순서를 거쳐 통신한다.

1. 웹브라우저는 서버의 URL에서 호스트 명을 추출한다.
2. 웹브라우저는 서버의 호스트 명을 IP로 변환한다.
3. 웹브라우저는 URL에서 포트번호를 추출한다.
4. 웹브라우저는 웹 서버와 TCP 커넥션을 맺는다.
5. 웹브라우저는 서버에 HTTP 요청을 보낸다.
6. 서버는 웹브라우저에 HTTP 응답을 돌려준다.
7. 커넥션이 닫히면, 웹브라우저는 문서를 보여준다.

## 1.6.3. 텔넷(Telnet)을 이용한 실제 예제

(생략)

# 1.7. 프로토콜 버전

### HTTP/0.9

디자인 결함이 있음. GET 메서드만 지원. MIME 타입, HTTP 헤더 미지원.

### HTTP/1.0

버전 번호, HTTP 헤더, 추가 메서드, 멀티미디어 객체 처리를 추가. 잘 정의된 명세는 아님.

### HTTP/1.0+

“keep-alive” 커넥션, 가상 호스팅 지원, 프락시 연결 지원 등.

### HTTP/1.1

설계의 결함 교정, 성능 최적화.

### HTTP/2.0

앞선 성능 문제를 개선

# 1.8. 웹의 구성요소

### 1. 프락시

클라이언트와 서버 사이에 위치한 HTTP 중재자.

웹 보안, 애플리케이션 통합, 성능 최적화를 위한 구성요소.

클라이언트를 대신해 서버에 접근.

주로 보안을 위해 사용하고 요청과 응답을 필터링하여 위험을 제거함.

### 2. 캐시

많이 찾는 웹페이지나 리소스를 캐시 서버에 보관해 서버의 부하를 덜어줌. 

HTTP 창고 역할을 하는 프락시 서버.

### 3. 게이트웨이

다른 애플리케이션과 연결된 특별한 웹 서버.

주로 HTTP 트래픽을 다른 프로토콜로 변환하기 위해 사용 됨.

![스크린샷 2023-04-13 오후 9.41.21.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b00cce3e-810f-49ef-a563-00254b1bf5cb/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-04-13_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_9.41.21.png)

### 4. 터널

단순히 HTTP 통신을 전달하기만 하는 특별한 프락시

예시로, 외부에서 요청된 HTTPS를 터널을 통해 사내망으로 들어가고 사내망 안에서는 HTTP로 변환해 전달하는 역할을 할수 있음. 보완 필요.

### 5. 에이전트

자동화된 HTTP 요청을 만드는 준지능적 웹클라이언트.

# 2. URL과 리소스

URL은 인터넷 리소스를 가리키는 표준 이름으로, 전자정보 일부를 가리키고 어디에 있고 어떻게 접근할 수 있는지 알려준다. 이번 장에서 다룰 내용은 아래와 같다.

- URL 문법, 여러 URL 컴포넌트가 어떤 의미를 가지며 무엇을 수행하는지
- 여러 웹 클라이언트가 지원하는 상대 URL과 확장 URL 같은 단축 URL에 대해서
- URL의 인코딩과 문자 규칙
- 여러 인터넷 정보 시스템에 적용되는 공통 URL 스킴
- 기존 이름은 유지하면서 객체들을 다른 장소로 옮기는 것을 가능하게 해주는 URN을 포함한 URL의 미래

# 2.1. 인터넷의 리소스 탐색하기

URL에는 스킴, 호스트, 패스, 쿼리, 프래그먼트가 구성 요소지만, 모두 포함하지 않아도 된다. 스킴, 호스트 필요에 따라 패스 정도가 항상 포함해야 하는 것들이다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3464cc9d-9fce-4f0a-8d69-174f1648a78f/Untitled.png)

아래와 같이 URL은 HTTP 프로토콜이 아닌 다른 프로토콜을 사용할 수도 있다.

- 이메일 - mailto:tamm@google.com
- FPT - ftp:ftp.tamm.com/video/bts-01.avi
- 비디오 서버의 스트리밍 영상 - rtsp://www.tamm.com:43/interview/bts-02.avi

## 2.1.1. URL이 있기 전 암흑의 시대

웹과 URL이 있기 전, 각종 포맷과 프로토콜에 대해 각자 자기만의 중구난방한 리소스 경로를 가졌다.

# 2.2. URL 문법

URL 문법을 좀 더 깊게 들어가면 아래와 같다.

`<**스킴**>://<**사용자 이름**>:<**비밀번호**>@<**호스트**>:<**포트**>/<**경로**>;<**파라미터**>?<**질의**>#<**프래그먼트**>`

사용자 정보는 보안적인 측면에서 잘 사용하지 않는다.

파라미터 같은 경우는 RESTful 한 구조를 가진다면 활용도가 낮다.

## 2.2.1. 스킴: 사용할 프로토콜

## 2.2.2. 호스트와 포트

호스트는 호스트 명이나 IP 주소로 제공한다. 

포트는 항상 필요하지만 생략 가능한데 스킴에 따라 기본 포트를 사용하기 때문이다. http는 80, https는 443 등이 있다.

## 2.2.3. 사용자 이름과 비밀번호

서버는 자신이 가지고 있는 데이터에 접근을 허용하기 전 사용자 이름과 비밀번호를 요구한다.

만약 값이 없다면 기본 사용자 이름 값으로 `anonymous` 비밀번호는 브라우저 마다 다른데 크롬은 `chome@example.com` 을 넣는다.

## 2.2.4. 경로

리소스가 서버의 어디에 있는지 알려준다. 유닉스 파일 시스템 경로와 유사하게 설계 되었다.

## 2.2.5. 파라미터

이름/값 쌍의 리스트로 ; 문자로 구별한다. 어플리케이션 리소스에 접근하는데 필요한 추가 정보를 전달할 수 있다. path 사이사이에 등록하는 것도 가능하다.

https//kakao.com/chat;type=dm/period;value=lastYear

## 2.2.6. 질의 문자열

주로 검색 질의로 많이 사용한다. 보통 ? 뒤에 이름=값 쌍 형태로 작성하며 &로 구분하여 여러 값을 넣을수 있다.

## 2.2.7. 프래그먼트

유일하게 서버로 전달하지 않는 값으로, 브라우저 내부에서 사용하는 문맥이다. 리소스 안의 특정 절을 가리킬수 있다. (ex. https://kakao.com#footer)

이를 응용하여 SPA이 개발한다고 한다.

# 2.3. 단축 URL

## 2.3.1. 상대 URL

기저 URL의 통해 URL의 모든 정보를 작성하지 않고 리소스에 접근하는 방법

예를 들면 https://kakao.com/index.html 에서  https://kakao.com/contact.html 에 접근하기 위해 모든 URL을 다 사용하지 않고 ./contact.html 만 적어주는 방식이다.

## 2.3.2. URL 확장

브라우저의 URL 자동완성 기능을 생각하면 간단하다. www 같은 서브 네임을 자동으로 입력해주는 것은 호스트명 확장, 단어만 입력해도 history에서 hit된 것들로 후보지를 보여주는 것은 히스토리 확장이다. 크롬 같은 경우 URL에 단어만 입력해도 단어를 검색하는 검색 URL을 만들어서 처리한다.

# 2.4. 안전하지 않은 문자

URL에서 특정 목적으로 사용하는 값과 안전하지 않은 값이 있다. 예를들면 :은 스킴 다음에 나오는 값, ?은 쿼리를 나타내는 값으로 URL 안에서 다른 용도로 사용할 수 없다.

이 밖에 대부분의 문자는 안전하지 않은 문자로 분류한다. 이유는 URL이 메일이나 ftp 등도 포함하는 넓은 개념이기 때문이다. 예를 들어 메일에서 허용하지 않는 값을 URL에서 사용하면 URL은 메일 주소를 올바르게 처리할 수 없다. 우리는 메일에서만 문제가 발생한다고 하더라도 이런 값은 장기적으로 문제를 일이킬 수 있다. 이런 값을 안전하지 않은 갑이라 한다.

서비스의 안전성을 위해서 항상 안전한 값으로 변경해서 처리해야 한다. 안전한 값은 영어와 몇몇 기호들이라 생각하면 된다.

## 2.4.1. URL 문자 집합

기본적으로 컴퓨터 시스템의 기본 문자 집합은 영어로 설정되어 있기 때문에 영어와 영문 자판에 있는 키 대부분을 포함한다. 이 외의 문자는 인코딩하여 사용한다.

## 2.4.2. 인코딩 체계

URL에서 안전하지 않는 문자를 다른 문자로 치환하는 것을 URL 인코딩이라 한다. 스킴을 구분하는 :는 %3A로 인코딩 되는 식이다.

## 2.4.3. 문자 제한

몇몇 문자는 예악어이다. 본래의 목적이 아닌 다른 용도로 사용하려면 반드시 인코딩 해야한다. 예를 들면 질의 문자열의 구획 문자인 ?이 있다.

## 2.4.4. 좀 더 알아보기

어떤 요청을 받은 어플리케이션에서 안전하지 않은 문자를 인코딩하지 않는 것은 실수다. 서버에서도 안전하지 않은 문자가 있는지 검증해야 하는 것이 맞다.

# 2.5. 스킴의 바다

스킴마다 포맷이 다르다. 위에서는 URL을 예시로 들었는데 스킴모다 구성요소가 다르다.

://:@:/;?#

ftp://anonymous:tamm%40tammo.com@tamm.com/files/bts-01.avi

news:rec.arts.tarktrek

ftp 같은 스킴은 @문자가 %40으로 인코딩 된다. news 스킴은 호스트가 없는 형태로 리소스 위치 정보가 충분히 없다.(중요지 않음)

# 2.6. 미래

URL은 강력한 도구로 유용하고 널리 사용된다. 하지만 리소스의 위치가 변경되면 URL을 찾을 수 없는 문제가 있다. 이를 위해 URN의 개념이 도입되었지만, URL을 대체하는 것은 어려워 보인다.

## 2.6.1. 지금이 아니면 언제??

URL은 나름의 한계를 가지고 있지만 당분간 계속 사용될 예정이다.

# HTTP 1.1과 HTTP 2.0
HTTP(HyperText Transfer Protocol)는 웹에서 브라우저와 서버가 통신하기 위한 프로토콜이다.

- **HTTP/1.1**
    - 기본적으로 커넥션당 하나의 요청과 응답만 처리한다. 즉, 여러 개의 리소스 요청이 개별적으로 전송되고 응답 또한 개별적으로 전송된다. 이처럼 HTTP/1.1은 리소스의 동시 전송이 불가능한 구조이므로 요청할 리소스의 개수에 비례하여 응답 시간도 증가한다.
    - HOL(head of line) Blocking 특정 응답 지연
        - 작업 하나가 끝날 때 까지 멈춰있는 현상
    - RTT(round trip time) 증가
        - 요청별로 connection을 만들게 되고 , TCP 상에서 동작하는 HTTP의 특성상 3-way Handshake가 반복적으로 일어나 불필요하게  RTT가 증가한다.
    - 무거운 Header 구조
        - http/1.1 헤더에는 많은 메타 정보를 저장한다.
        - domain sharding을 하지 않았을 경우 매 요청시 중복된 Header 값을 전공하게 되며 여기엔 도메인에 설정된 쿠키 정보도 포함된다.(쿠기 정보가 크다.)
        - 전송하려는 값보다 헤더 값이 큰 경우도 자주 발생한다.
    - 단점 극복 방법
        - Image Spriting:  큰 이미지 파일을 잘라서 쓰는 것.
        - Domain Sharding: 다수의  connection을 생성 해 병렬로 요청을 보내는 것. 브라우저 별로 갯수 제한이 있다.
        - Minify: 코드 자체를 축소 하는 것.
        - Data URI Scheme: 이미지 리소스를 Base64로 인코딩된 이미지 데이터로 직접 기술하는 방식
        - Load Faster: 스타일 시트는 문서 상단에 스크립트는 문서 하단에 배치
- **HTTP/2**
    - HTTP/2는 다중 요청/응답이 가능하다. 리소스의 동시 전공이 가능하므로 HTTP/1.1에 비해 페이지 로드 속도가 빠르다.
    - Multiplexed Streams
        - 한 커넥션으로 여러 개의 메세지를 주고 받을수 있으며, 요청 순서 상관 없이 빠른 것 부터 응답받는다.
        - HTTP/1.1의 Connection keep-alive, pipelining의 개선.
    - Stream Prioritization
        - 클라이언트가 요청한 HTML 문서 안에 CSS파일 1개와 Image파일 2개가 존재하여 클라이언트가 각각 요청 했을때 Image파일보다 CSS파일의 수신이 늦어지는 경우 브라우저의 렌더링이 늦어지는 문제가 발생한다.
        - HTTP/2의 경우 리소스간 의존관계(우선순위)를 설정하여 문제를 해결한다.
    - Server Push
        - 서버는 클라이언트가 필요로 하지만 요청하지 않은 리소스를 사전에 푸쉬를 통해 전송할수 있다.
        - 예를 들면 클라이언트가 HTML 문서를 요청할 때 해당 문서 내의 리소스를 사전에 클라이언트에서 다운로드할 수 있도록 하여 클라이언트의 요청을 최소화 한다.
        - PUSH_PROMISE 라고 부르며 이를 통해 서버가 전송한 리소스에 대해선 클라이언트는 요청을 하지 않는다.
    - Header Compression
        - HTTP/2는 헤더 정보를 압축하기 위해 Header Table과 Huffman Encoding 기법을 사용하여 처리하는데 이를 HPACK 압축방식이라 부르며 별도의 명세서(RFC 7531)로 관리하고 있다.
        - 예를 들면 클라이언트가 요청을 두 번 보낼 경우 헤더에 중복이 있는 경우 Static/Dynamic Header Table 개념을 이용하여 중복을 검출해 내고 해당 테이블에서의 index값 + 중복되지 않은 Header 정보를 Huffman Encoding 방식으로 인코딩한 데이터를 전송한다.
    - 보안통신 (HTTPS)
        - HTTP/2 는 평문 통신과 보안 통신 모두를 지원한다. 다만 보안 접속을 기본으로 한다.
- **HTTP/3**
    
    TCP대신 UDP기반의 QUIC 프로토콜을 사용한 방식
    
    |  | TCP | UDP |
    | --- | --- | --- |
    | 연결 방식 | 연결 지향형 프로토콜 | 비 연결 지향형 프로토콜 |
    | 전송 순서 | 보장 | 보장하지 않음 |
    | 신뢰성 | 높음 | 낮음 |
    | 전송속도(상대적) | 느림 | 빠름 |
    | 혼잡제어 | 가능 | 불가능 |
    | 헤더크기 | 20 BYTE | 8 BYTE |
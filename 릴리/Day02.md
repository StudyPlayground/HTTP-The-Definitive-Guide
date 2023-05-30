## OPTIONS method

HTTP OPTIONS 메서드는 지정된 URL 또는 서버에 대해 허용된 통신 옵션을 요청한다. 클라이언트는 이 메서드를 사용해 URL를 지정하거나 * 를 사용해 전체 서버를 참조할 수 있다. 

| request body 존재 여부  | x |
| --- | --- |
| 성공적인 response 에서 body 존재 여부 | o |
| 안전함 | o |
| 멱등성 | o |
| 캐시가능 | x |

### 문법

```tsx
OPTIONS /index.html HTTP/1.1
OPTIONS * HTTP/1.1
```

### 예시

- 허용되는 요청들을 확인하는 법

서버가 지원하는 요청 메서드를 확인하려면 curl 커멘드라인을 이용해 OPTIONS 요청을 실행하면 된다. 

```tsx
curl -X OPTIONS https://example.org -i
```

응답은 허용된 메서드를 포함하는 Allow 헤더가 포함된다.

```tsx
HTTP/1.1 204 No Content
Allow: OPTIONS, GET, HEAD, POST
Cache-Control: max-age=604800
Date: Thu, 13 Oct 2016 11:45:00 GMT
Server: EOS (lax004/2813)
```

## preflight request

CORS preflight 요청은 특정한 메소드와 헤더를 사용하여 CORS 프로토콜을 이해하는 서버가 인식하는지 확인하는 CORS 요청이다. 

이는 `OPTIONS` 요청을 사용하며,

 `Access-Control-Request-Method`

`Access-Control-Request-Headers`

`Origin` 

헤더를 사용한다. 

프리플라이트 요청은 브라우저에 의해 자동으로 생성되며, 일반적인 경우에는 프론트엔드 개발자가 직접 이러한 요청을 작성할 필요가 없다. 

요청이 "preflighted"로 표시된 경우 및 "simple requests"인 경우에는 생략됩니다.

예를 들어, 클라이언트는 프리플라이트 요청을 사용하여 `DELETE` 요청을 전송하기 전에 서버가 해당 요청을 허용하는지 물어볼 수 있다. 

```
OPTIONS /resource/foo
Access-Control-Request-Method: DELETE
Access-Control-Request-Headers: origin, x-requested-with
Origin: <https://foo.bar.org>

```

서버가 허용하면, `Access-Control-Allow-Methods` 응답 헤더가 포함된 응답을 프리플라이트 요청에 대해 보낸다. 

```
HTTP/1.1 204 No Content
Connection: keep-alive
Access-Control-Allow-Origin: <https://foo.bar.org>
Access-Control-Allow-Methods: POST, GET, OPTIONS, DELETE
Access-Control-Max-Age: 86400

```

프리플라이트 응답은 `Access-Control-Max-Age` 헤더를 사용하여 동일한 URL에서 생성된 요청에 대해 선택적으로 캐시될 수 있다. 프리플라이트 응답을 캐시하기 위해 브라우저는 일반적인 HTTP 캐시와는 별도로 관리되는 특별한 캐시를 사용한다. 프리플라이트 응답은 브라우저의 일반적인 HTTP 캐시에는 캐시되지 않는다.

### 왜 preflight를 하는 것일까
브라우저에서 체크하기 때문에 안전성이 없는 메서드를 서버에서 실행하는 것을 방지하기 위해서다. 

[OPTIONS - HTTP | MDN](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/OPTIONS)

[Preflight request - MDN Web Docs Glossary: Definitions of Web-related terms | MDN](https://developer.mozilla.org/en-US/docs/Glossary/Preflight_request)

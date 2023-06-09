# 7. 캐시

웹 캐시는 자주 쓰이는 리소스를 자동으로 보관하는 HTTP 장치로 요청에 대한 캐시가 있다면 원 서버가 아니라 캐시로부터 제공 된다. 다음과 같은 이점이 있다.

- 캐시는 불필요한 데이터 전송을 줄여서, 네트워크 요금으로 인한 비용을 줄여준다.
- 캐시는 네트워크 병목을 줄여준다. 대역폭을 늘리지 않고도 페이지를 빨리 불러올 수 있게 된다.
- 캐시는 원 서버에 대한 요청을 줄여준다. 서버는 부하를 줄일 수 있으며 더 빨리 응답할 수 있게 된다.
- 페이지를 먼 곳에서 불러올수록 시간이 많이 걸리는데, 캐시는 거리로 인한 지연을 줄여준다.

# 7.1. 불필요한 데이터 전송

네트워크 대역폭의 비용은 비싸기 때문에 가능한 효율적으로 사용하는 것이 중요하다. 같은 데이터를 또 받을 필요는 없다.

# 7.2. 대역폭 병목

네트워크 속도는 가장 느린 속도에 맞춰지기 때문에 병목 현상이 일어나는데, 캐시는 이 병목을 줄여준다. 국내는 지연간 거리가 그렇게 멀지 않지만, 땅덩어리가 큰 나라는 다르다. 근처에 캐시 서버가 있다면 응답을 빨리 할 수 있다.

# 7.3. 갑작스런 요청 쇄도

네트워크 접속은 고를 때도 많지만, 특정 상황에서 트래픽이 급증할때도 있다. 이때 웹서버에 심각한 장애를 야기시킨다.

# 7.4. 거리로 인한 지연

당연하게도 거리가 멀수록 지연이 발생한다.

# 7.5. 적중과 비적중

운영체제의 캐시 hit과 같은 개념으로 클라이언트에서 요청한 데이터가 캐시 서버에 있다면 원서버에 가지 않고 내려주고, 캐시에 없다면 원서버에서 가져와 캐시 서버에 저장 후 클라이언트에 넘긴다.

## 7.5.1. 재검사

캐시 서버는 hit 했더라도 데이터 사본이 최신인지 때때로 점검해야 한다. 이런 신선도 검사를 HTTP 재검사라 부른다. 캐시는 스스로 원한다면 언제든 재검사할 수 있다.

캐시가 재검사 요청을 보냈고, 그 콘텐츠가 변경되지 않았다면 아주 작은 304 Not Modified를 보낸다.

If-Modified-Since 헤더는 HTTP 가 캐시된 객체를 재확인하기 위해 제공하는 도구로, 객체가 변경되지 않았다면 304 Not Modified를 변경 됐다면 상황에 따라 200 OK나 404 Fot Found를 준다.

## 7.5.2. 적중률

캐시 서버에서 데이터를 바로 줄수 있는 상태가 hit인데, 40%만 되도 괜찮은 편이라 할 수 있다.

## 7.5.3. 바이트 적중률

문서의 크기(용량)이 다 다르니 크기에 따른 가중치를 둔 적중률을 선호하기도 한다.

## 7.5.4. 적중과 비적중의 구별

클라이언트 입장에서 받은 응답이 캐시 서버가 내려 준 것인지, 원 서버에서 준 것인지를 알 수 없다. 그러나 헤더에 Date 값을 볼 수 있는데, 캐시된 데이터의 경우 내가 요청한 날짜보다 더 과거일 것이다. 여기서 전제는 캐시가 Date 값을 건들지 않는다는 전제가 있다(중개 서버가 Date 값을 수정하면 안된다). 

또한 응답이 얼마나 오래 됏는지 Age 헤더를 이용하는 방법도 있다.

그리고 프락시 캐시는 Via 헤더에 캐시 서버가 자신에 대한 데이터를 추가한다.

# 7.6. 캐시 토폴로지

캐시의 연결 방식은 개인을 위한 캐시일 수도 있고, 공용으로 쓰이는 캐시일수도 있다.

## 7.6.1. 개인 전용 캐시

브라우저는 개인 디바이스 공간에 전용 캐시 공간을 만들어 사용한다.

## 7.6.2. 공용 프락시 캐시

프락시 서버 종류 중 하나로 캐시 역할을 하는 서버가 공용 캐시라 할 수 있다.

## 7.6.3. 프락시 캐시 레벨들

더 효율적인 캐시를 사용하기 위해 여러 레벨의 캐시를 사용하는 것이다. 이건 CPU에서 사용하는 캐시 전략과 유사하다(레지서터 - 캐시(SRAM) - 메인 메모리(DRAM) - 디스크).

![스크린샷 2023-05-16 오후 8.14.29.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5d0e6ea6-19da-4dc8-a572-98a213261667/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-05-16_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_8.14.29.png)

## 7.6.4. 캐시망, 콘텐츠 라우팅, 피어링

콘텐츠 라우팅을 위해 캐시망 안에 있는 캐시들이 하는 행동이다.

- URL에 근거하여, 부모 캐시와 원 서버 중 하나를 동적으로 선택한다.
- URL에 근거하여 특정 부모 캐시를 동적으로 선택한다.
- 부모 캐시에게 가기 전에, 캐시된 사본을 로컬에서 찾아본다.
- 다른 캐시들이 그들의 캐시된 콘텐츠에 부분적으로 접근할 수 있도록 허용하되, 그들의 캐시를 통한 인터넷 트랜짓은 허용하지 않는다.

선택적인 피어링을 지원하는 캐시는 형제 캐시라 불린다.

![스크린샷 2023-05-16 오후 8.16.34.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3fc23f68-106f-42e3-bb0c-0185eb02ec5e/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-05-16_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_8.16.34.png)

HTTP는 캐시를 위해 ICP(Internet Cache Protocol), HTCP(Hypertext Cache Protocol)을 확장했다. 이는 20장에서 다룬다.

# 7.7. 캐시 처리 단계

오늘날 상용 프락시 캐시는 상당히 복잡하게 동작한다.

## 7.7.1 단계 1: 요청 받기

캐시는 네트워크 커넥션에서의 활동을 감지하고 데이터를 읽어 들인다. 고성능 캐시는 여러 커넥션에서 동시에 읽어 들인다.

## 7.7.2 단계 2: 파싱

요청 메시지를 파싱하여 URL과 헤더 등을 꺼내 조작하기 쉬운 자료구조로 만든다.

## 7.7.3 단계 3: 검색

캐시에 복사본이 있는지 검사하고 없다면 원 서버에서 가져온다.

## 7.7.4 단계 4: 신선도 검사

캐시에 복사본이 있더라도, 원서버와 같은 데이터인지를 검사 해야한다.

## 7.7.5 단계 5: 응답 생성

캐시 서버는 원 서버에서 온 것과 다름 없게 응답을 만든다. 다만 캐시 신선도 정보를 추가 하거나 Via 헤더를 포함시킬 수 있다. 주의할 것은 Date 헤더는 변경해선 안된다.

## 7.7.6 단계 6:  전송

응답을 전송한다.

## 7.7.7 단계 7: 로깅

캐시 서버에서 어떤 트랜잭션을 처리했는지 로깅한다. 로깅하는 포맷은 스키드 로그 포맷, 넷스케이프 확장 공용 로그 포맷 등이 있다. 또한 커스텀 로그 파일도 허용한다. 이것들은 21장에서 자세히 다룰 것이다.

## 7.7.8. 캐시 처리 플로 차트

![스크린샷 2023-05-16 오후 8.24.04.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/538b4f17-6c37-47e6-9406-259019a0feb7/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-05-16_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_8.24.04.png)

# 7.8. 사본을 신선하게 유지하기

HTTP에는 문서가 만들어진 시간 또는 만료 기간을 헤더에 넣는다.

## 7.8.1. 문서 만료

만료 시간을 나태내는 것은 두 종류로 Expires 헤더와 Cache-Control: max-age 헤더이다.

![스크린샷 2023-05-16 오후 8.30.16.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/df4fb188-761f-4b40-b36a-40adb0900531/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-05-16_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_8.30.16.png)

![우선 순위는 cache-control이 expires보다 높다.](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f5825380-fa7c-47c2-98dc-1fe8e09c380a/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-05-16_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_8.31.04.png)

우선 순위는 cache-control이 expires보다 높다.

## 7.8.2. 유효기간과 나이

Cache-Control 은 Date와 비교하여 max-age 초가 지나면 만료시킨다.

Expires는 언제 만료 할지에 대해 써있다.

## 7.8.3. 서버 재검사

캐시된 문서가 만료됐다는 것은 그 문서가 더 이상 원서버의 데이터와 달라졌다를 의미하는 것이 아니라, 검사할 시간이 됐단 의미이다.

## 7.8.4. 조건부 메서드와의 재검사

재검사 로직을 효율적으로 하기 위해 캐시 서버는 If-Modified-Since와 If-Node-Match를 사용한다.

## 7.8.5. If-Modified-Since: 날짜 재검사

문서가 주어진 날짜 이후로 수정됐다면 요청을 처리하고, 그렇지 않다면 캐시에 있는 것을 가져온다.

![스크린샷 2023-05-16 오후 8.39.27.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/92492fca-0b54-497a-83a3-79368efdf3be/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-05-16_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_8.39.27.png)

## 7.8.6. If-Node-Match: 엔티티 태그 재검사

문서에서 발행번호 같이 동작하는 ETag를 비교해 다를 때만 요청을 처리한다.

![스크린샷 2023-05-16 오후 8.39.42.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9411347d-d2ff-47bf-a975-f8999232b6e6/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-05-16_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_8.39.42.png)

## 7.8.7. 약한 검사기와 강한 검사기

모든 데이터를 실시간으로 보여주는 것도 좋지만, 적당한 시간마다(예를 들면 5초) 한 번 갱신 하는 것만으로 사용성을 해치지 않으면서 네트워크의 효율을 크게 높일 수 있다. 약한 검사기는 아주 조금 바뀐 버전에 대해서는 그냥 그 버전을 사용하게 하는 것이다. 버전 앞에 `W/` 를 prefix로 사용한다.

```tsx
ETag: W/"v2.6" 
If-None-Match: W/"v2.6"
```

일반적으로는 강한 검사기가 사용된다.

## 7.8.8. **언제 언터티 태그(If-None-Match)를 사용하고 언제 일시(If-Modified-Since)를 사용할까?**

ETag가 있다면 반드시 엔터티 태그 검사기를 사용하고, 시간과 관련된 값만을 반환했다면 If-Modified-Since 검사를 사용할 수 있다. 모두 사용 가능하다면 모두 사용해야 한다.

서버는 불가능하지만 않다면 엔터티 태그 검사기를 보내는 것이 좋다.

# 7.9. 캐시 제어

Cache-Control이 사용할수 있는 값이다.

## 7.9.1. no-cache와 no-store 응답 헤더

둘 다 캐시를 하지 않는다. no-store는 응답의 사본을 만드는 것을 금지하고, no-cache는 로컬 캐시 저장소에 저장될 수 있지만, 서버와의 재검사 후 클라이언트로 제공된다.

HTTP/1.0+와 호환성을 위해 Pragma: no-cache를 줄 수 있다.

## 7.9.2. Max-Age 응답 헤더

서버로부터 온 시간으로부터 얼마나 신선한지 알려준다. s-maxage 헤더는 공유된(공용) 캐시에만 적용된다.

## 7.9.3. Expires 응답 헤더

서버에 설정된 시간에 영향을 받는다.

## 7.9.4. Must-Revalidate 응답 헤더

신선하지 않는 사본을 원 서버와의 최초의 재검사 없이 제공해서 안 된다는 것을 의미한다.

## 7.9.5. 휴리스틱 만료

## 7.9.6. 클라이언트 신선도 제약

요새 브라우저는 캐시를 강제로 지울 수 있다. 캐시 신선도 전략을 위한 다양한 요청 지시어가 있다.

[Cache-Control - HTTP | MDN](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Cache-Control)

## 7.9.7. 주의할 점

문서 만료는 완벽한 시스템이 아니다. 문서가 변할 수 있는 시간을 예측하는 것은 불가능하다.

# 7.10. 캐시 제어 설정

## 7.10.1. 아파치로 HTTP 헤더 제어하기

## 7.10.2 **HTTP-EQUIV를 통한 HTML 캐시 제어**

HTML meata 태그에 HTTP-EQUIV에 값에 넣으면 캐시 프락시에서 꺼내 헤더에 넣는다. 하지만 지원하는 서버가 별로 없다.

![스크린샷 2023-05-16 오후 9.12.04.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/28f19f80-774b-4b64-90d3-cca13c783e5b/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-05-16_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_9.12.04.png)

# 7.11. 자세한 알고리즘

# 7.12. 캐시와 광고

## 7.12.1. 광고 회사의 딜레마

캐시를 하면 원 서버에 요청이 줄기 때문에 광고가 노출되지 않는다.

## 7.12.2. 퍼블리셔의 응답

광고 회사는 캐시를 무력화 하는 방법을 사용한다. 프락시 캐시 입장에선 자신의 역할을 못하기 때문에 이런 무력화를 막기도 한다. 그런데 또 캐시를 하면 방문자 통계를 알 수 없게 한다. 

이를 해결하는 대표적인 방법으로는 요청은 원서버에 가지만 본문은 전송하지 않는 식이다.

## 7.12.3. 로그 마이그레이션

캐시의 로그를 통해 원서버로의 요청을 뽑아 내려고 하지만, 인증아나 여러 이슈 때문에 표준화 되지는 않았다.

## 7.12.4. 적중 측정과 사용량 제한

특정 URL에 대한 캐시 적중 횟수를 Meter라는 새 헤더 하나를 추가해서 넣는다. 이 방법을  HTTP를 위한 간단한 캐시 적중량 측정과 사용량 제한(Simple Hit- Metering and Usage-Limiting for HTTP)이라 한다. 이 값을 보고 캐시가 몇번이나 문서를 제공할 수 있는지 제한할 수 있다.

# 요약

클라이언트(빠른 응답을 받을 수 있다) → 캐시(DB, 메모리, 파일) → 서버(서버와 통신하는건 다 돈)

Cache Hit - 적중

- 하지만 너무 오래 된 경우에는 다시 서버로 요청을 한다.(Cache-control: max-age, Expires)
    - 서버에 물어보니 변경이 없다.(304 Not Modified)
    - 변경이 있다.(200 OK)
    - cache가 오래 됐는데 그냥 오래된거 주는 경우.

Cache Miss - 미적중

Cache-control

```tsx
const http = require('http');
const server = http.createServer((req, res) => {
  res.writeHead(200, {
    'Cache-Control': 'option1, option2, ...',
  });
  res.end('ok');
});
server.listen(8080, () => {
  console.log('on 8080 port');
})
```

- 문서
    - 
    
    [Cache-Control - HTTP | MDN](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Cache-Control)
    
- s-maxage - 프록시 캐시
- no-store - 캐시하지 마라
- no-cache - 이름을 잘못 지었다.. 캐시는 저장하고 신선도 검사를 항상 한다. 유효기간을 무시한다.
- must-revalidate - 유통기한 지나면 서버에 재확인해라
- private(기본 값) - 개인 캐시 저장소(내 브라우저 디스크 메모리)
- public - 남들과 공유된다. 주로 wiki 같은 퍼블릭하고 공유 가능한 문서. 공유 캐시. Authorization 헤더가 있으면 사용 못함.
- stale-while-revalidate - 일단 오래된거 주고, 뒤에서 서버한테 물어보고 새거 있으면 몰래 업데이트 해놔. optimistic update.
- stale-if-error - 일단 오래된거 주고, 서버가 에러가 나도 에러 표시 안하고 캐시된거 일단 준다.에러 표시 싫어서.. 엄청 특수한 전략.

캐시 신선도 검사

위에 있는건 캐시 입장, 아래는 실제 데이터 기준.

캐시에 있는 데이터와 서버의 데이터가 서로 같은지를 비교하는 방법

E-tag -

Last-modified

cache-control: max-age: 캐시를 얼마나 저장할지

Last-modified 류, 캐시랑 서버의 데이터가 같은지 확인하는 것
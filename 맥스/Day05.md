---
marp: true
paginate: true
---

# Etag를 이용한 캐싱

---

## 캐싱을 사용하지 않는다면

- [캐싱 사용 X](https://github.com/StudyPlayground/HTTP-The-Definitive-Guide/assets/75886763/738bd00e-cb60-4570-b36b-17e2ad7826f2)

---

## Cache-Control: max-age

- [max-age를 이용한 캐싱](https://github.com/StudyPlayground/HTTP-The-Definitive-Guide/assets/75886763/53e73c7d-b798-4373-a659-f6845b1714e8)

---

## Etag

- max-age가 만료되었지만 데이터가 변경되지 않는 상황
  - 불필요한 요청 발생 -> 이를 해결하고자 시간말고 버전명과 같은 표시를 사용
  - [Etag 캐싱](https://github.com/StudyPlayground/HTTP-The-Definitive-Guide/assets/75886763/26a0b689-5982-4d50-9ae4-e401aabe320a)
- Etag가 오래되었다고 판단
  - 그래도 데이터가 변경되지 않은 경우 -> 304 Not-Modified ([Etag 캐싱 304](https://github.com/StudyPlayground/HTTP-The-Definitive-Guide/assets/75886763/9d219f49-f7d0-4170-8f41-689ad604fb72))
  - 변경된 경우 -> 200 Ok ([Etag 캐싱 200](https://github.com/StudyPlayground/HTTP-The-Definitive-Guide/assets/75886763/93a3826e-83ca-44e8-ade2-b49f0e38ec52))

---

## 파일명을 변경

- Etag가 오래되지 않았다고 판단하지만, 실제 서버에서는 데이터가 변경된 경우
  - 만료 헤더값과 Etag만으로 해결 어려움
- 요청하는 파일명을 변경 -> 브라우저는 리소스의 url이 변경되면 새로운 데이터로 인식
  - [파일명을 변경](https://github.com/StudyPlayground/HTTP-The-Definitive-Guide/assets/75886763/07ec1813-ab56-43f0-be00-228ed1195915)

---

### 관련 단원
- 7장 캐시
- 7.8절 사본을 신선하게 유지하기

### 참고 링크

- [[10분 테코톡] 📸소니의 Cache](https://www.youtube.com/watch?v=NxFJ-mJdVNQ)
- [ETag (MDN)](https://developer.mozilla.org/ko/docs/Web/HTTP/Headers/ETag)

---
marp: true
paginate: true
---

# 리다이렉션 (Redirection)

---

## 리다이렉션이란

- 클라이언트가 요청한 URL이 이동되었음을 뜻하는 것

---

## 리다이렉션의 종류

> 305, 306은 MDN에서 제외됨

- 영구 리다이렉션 : 301, 308
- 일시 리다이렉션 : 302, 303, 307
- 특수 리다이렉션 : 304
  - 캐시 활용 여부

---

## 영구와 일시 차이

- 검색 엔진 크롤링 & SEO
  - 영구: 기존 URL에 대해 수집한 내용을 새 URL로 이전 + 새 URL에 대해 수집
  - 일시: 새 URL에 대한 수집하지 하지 않음
- 브라우저 캐싱
  - 영구: 자동으로 캐싱
  - 일시: Cache-control, Expire와 같은 설정을 해줘야 캐싱 가능

---

## 리다이렉션시 메서드 변경 여부 (+ 본문 사용 여부)

- [변경 O](https://user-images.githubusercontent.com/75886763/234232951-ce678923-04c1-457a-9212-0cbf57c15d5b.png) : 301, 302, 303
- [변경 X](https://user-images.githubusercontent.com/75886763/234232973-102b02c9-8645-4132-a241-fc0b16c73922.png) : 307, 308

---

## 리다이렉션 종류 (정리)

||영구|일시|
|:---|:---:|:---:|
|메소드 변경 O|301 (Moved Permanently)|302 (Found), 303(See Other)|
|메소드 변경 X|308 (Permanent Redirect)|307 (Temporary Redirect)|

---

### 관련 단원
- 3장 HTTP 메시지

### 참고 링크

- [모든 개발자를 위한 HTTP 웹 기본 지식](https://www.inflearn.com/course/http-웹-네트워크)
- [🌐 3XX (Redirection) 상태 코드 - 총정리 모음](https://inpa.tistory.com/entry/HTTP-🌐-3XX-Redirection-상태-코드-제대로-알아보기)
- [🌐 301 vs 302 상태 코드 차이점 (SEO)](https://inpa.tistory.com/entry/HTTP-🌐-301-vs-302-상태-코드-차이점-💯-완벽-정리)
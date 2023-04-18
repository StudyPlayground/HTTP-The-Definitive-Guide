---
marp: true
paginate: true
---

# 단축 URL

---

## 단축 URL 원리

> 일반적이고 쉬운 예시

- 사용자로부터 받은 `원본 URL`을 DB에 저장하고 해당 id(index)를 Base62로 인코딩하여 `단축 URL`을 만든다.
- 이후 `단축 URL`로 요청이 오면 Base62로 디코딩하여 id(index)를 알아내어 `원본 URL`을 찾아 리다이렉트한다.

---

## Base62? Base64?

- `Base64`는 바이너리 데이터를 64개의 문자 집합으로 표현하는 인코딩 체계입니다.
  - `0-9`, `A-Z`, `a-z`, `+`, `/`, `=`(패딩값: 비어있음을 뜻함)
  - 예시: `http://charsyam.wordpress.com/abc===query=abcd+/=`

---

## Base62? Base64?

- `URL Safe Base 64`는 `Base64`와 비슷한 인코딩 체계입니다.
  - `+`, `/`은 URL 예약어이므로 사용하기 부적절하여 `-`, `_`로 대체하였습니다.
- `Base62`는 바이너리 데이터를 62개의 문자 집합으로 표현하는 인코딩 체계입니다.
  - URL 단축 서비스와 같이 짧고 기억하기 쉬운 영숫자 문자열이 필요한 컴퓨터 시스템 및 애플리케이션에서 자주 사용됩니다.
  - `0-9`, `A-Z`, `a-z`

---

### 관련 단원
- 2장 URL과 리소스

### 참고 링크

- [초보 개발자 URL Shortener 서버 만들기 1편 : Base62와 춤을](https://medium.com/monday-9-pm/초보-개발자-url-shortener-서버-만들기-1편-base62와-춤을-9acc226fb7eb)
- [base64, base62?](https://velog.io/@nnagman/base64-base62)
- [[입 개발] base64 가 있는데 base62 같은걸 왜 써야 하나요?](https://www.popit.kr/입-개발-base64-가-있는데-base62-같은걸-왜-써야-하나요/)
- [URL Shortener 개발기](https://42place.innovationacademy.kr/archives/9063)
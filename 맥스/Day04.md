---
marp: true
paginate: true
---

# Proxy

1. Proxy
2. Forward Proxy
3. Reverse Proxy

---

## Proxy

- Client와 Server 사이에 위치하는 중계 서버
- 각 통신의 요청과 응답을 대리로 수행
- [사진](https://github.com/StudyPlayground/HTTP-The-Definitive-Guide/assets/75886763/f5b2ee26-dc47-49b5-ba7c-ff1ef68eaa3c)

---

## Forward Proxy

- 캐싱: Client가 요청한 내용을 캐싱
  - 불필요한 통신을 감소로 네트워크
- 익명성: Client가 보낸 요청을 감춤
  - Server가 받은 요청이 누군지 모르게함 (Proxy인 것만 암)
- [사진](https://github.com/StudyPlayground/HTTP-The-Definitive-Guide/assets/75886763/92b85c42-0fbd-46f3-944b-dc2251d7db2f)

---

## Reverse Proxy

- 캐싱: Client가 요청한 내용을 캐싱
  - 불필요한 통신을 감소로 네트워크
- 보안: Server 정보를 Client로 부터 숨김
  - Client는 Server에 요청을 보낸 것으로 알지만, 사실은 Proxy
- [사진](https://github.com/StudyPlayground/HTTP-The-Definitive-Guide/assets/75886763/c5d9e63c-20cd-461c-b81a-9537801b1fae)

---

## Load Balancing

- Reverse Proxy의 선택적 특징
- 많은 Client 요청을 분산시켜 서버의 부담을 덜게 함
  - Layer4 (TCP/IP)에서 분산 ➤ IP/PORT를 기준으로 분산
  - Layer7 (Application)에서 분산 ➤ 사용자의 요청 내용을 기준으로 분산

---

### 관련 단원
- 6장 프락시

### 참고 링크

- [[10분 테코톡] 🐿 제이미의 Forward Proxy, Reverse Proxy, Load Balancer](https://www.youtube.com/watch?v=YxwYhenZ3BE)
- [로드밸런서(Load Balancer)의 개념과 특징](https://m.post.naver.com/viewer/postView.naver?volumeNo=27046347&memberNo=2521903)

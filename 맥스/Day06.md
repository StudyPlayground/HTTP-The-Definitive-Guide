---
marp: true
paginate: true
---

# 검색 엔진의 웹 로봇

---

## 페이지가 검색결과 노출되기 까지

1. 크롤링
   - 페이지로부터 하이퍼텍스트 링크를 따라 페이지들을 탐색
   - robots.txt
2. 인덱싱
   - 탐색한 페이지들의 정보를 수집하고 분류
   - robot meta tag
3. 검색 결과에 노출

- [전체적인 흐름 사진](https://github.com/StudyPlayground/HTTP-The-Definitive-Guide/assets/75886763/3d63f971-2bc1-4774-9708-19bce7d4b01f)

---

## 궁금했던 부분

- Q) 크롤링을 막으면 인덱싱이 안 되므로 검색에 노출이 아예 안 되는 건가?
- A) robots.txt로 해당 웹페이지의 크롤링이 막혀서 생각한대로 되지만 해당 웹페이지를 다른 곳에서 링크를 걸어 크롤링 오게 되면 인덱싱의 대상이 될 수 있다. 따라서 검색 결과에 노출되고 싶지 않으면 robot meta tag를 작성하여야 한다.
- [참고](https://developers.google.com/search/docs/crawling-indexing/robots/intro?hl=ko#understand-the-limitations-of-a-robots.txt-file)

---

## (번외) Sitemap

- 검색 엔진 크롤링 로봇에게 웹 사이트에서 크롤링 해야 할 URL 을 전달한다
- 사이트 맵을 지원하는 검색 엔진은 이 정보를 사용하여 웹 사이트 크롤링을 보다 효율적으로 할 수 있게된다
- [예시: sitemap.xml](https://github.com/StudyPlayground/HTTP-The-Definitive-Guide/assets/75886763/83fa0cea-c92a-40e3-950f-0ae1dc41b1f2)
- [예시: robots.txt](https://github.com/StudyPlayground/HTTP-The-Definitive-Guide/assets/75886763/3b0a46e0-29dd-4aa5-af6b-cd85d11b29ed)

---

### 관련 단원
- 9장 웹 로봇

### 참고 링크

- [Robots.txt 소개 (Google 검색 센터)](https://developers.google.com/search/docs/crawling-indexing/robots/intro?hl=ko)
- [사이트맵 알아보기 (Google 검색 센터)](https://developers.google.com/search/docs/crawling-indexing/sitemaps/overview?hl=ko)
- [HTML - 검색엔진최적화 6 : robotstxt & sitemap (생활코딩 Youtube)](https://www.youtube.com/watch?v=hRmfZxXa3KA)
- [Robots.txt와 Sitemap.xml 제대로 설정하기](https://www.ascentkorea.com/what-is-robots-txt-sitemap-xml/)

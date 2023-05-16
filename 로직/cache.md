---
marp: true
---

## stale-while-revalidate

------

## max-age
- 응답시점으로부터 리소스가 유효한 시간
- 초단위
- ex) max-age=60
---
## stale-while-revalidate
- 상한동안 재검증
- 응답이 만료된 후에도 지정된 시간동안 리소스 제공
- max-age와 함께 쓰여야함
- ex) max-age=60, stale-while-revalidate=30
---
## max-age vs stale-while-revalidate

|  | 30 | 70 | 80 |
| --- | --- | --- | --- |
| max-age=60 | hit | miss | hit |
| max-age=90 | hit | hit | hit |
| max-age=60, stale-while-revalidate=30 | hit | miss, revalidate | hit |

65초에 데이터가 갱신되었다면?
- 응답속도: max-age=90 *==*  max-age=60, stale-while-revalidate=30
- 정확도: max-age=60 > max-age=60,stale-while-revalidate=30 > max-age=90
---
## react-query
```typescript
    const { data } = useQuery('users', getUsers, {
        staleTime: 5000,
        cacheTime: 1000 * 60 * 5,
    })
```

## 4.7.3 커넥션의 끊기의 허용, 재시도, 멱등성 (보완)

> 한 번 혹은 여러 번 실행됐는지에 상관없이 같은 결과를 반환한다면 그 트랜잭션은 멱등(idempotent)하다고 한다. GET, HEAD, PUT, DELETE, TRACE, OPTIONS 메서드들은 멱등하다고 이해하면 된다.
> 

### 멱등성

- 멱등 (수학): 연산을 여러 번 적용하더라도 결괏값이 달라지지 않는일
    
    → 같은 요청을 반복해도 **항상 동일한 결과를 보장**하는 성질 
    → 해당 요청이 서버의 상태, 데이터 변경하지 않고 단순히 데이터를 읽는 등의 작업에 해당 
    

<aside>
💡 a method where performing the same action multiple times produces **the same result as performing it only once**

</aside>

| GET | 멱등 | 단순히 서버로부터 데이터를 읽어오는 요청  |
| --- | --- | --- |
| PUT | 멱등 | 서버의 데이터를 업데이트하는 요청  |
| DELETE | 멱등 | 서버의 데이터가 변경되거나 삭제 |
| POST | 비멱등 | 서버의 상태, 데이터를 변경하는 요청  |
| PATCH | 멱등 | 서버의 상태, 데이터를 일부 변경하는 요청  |
- **HTTP 프로토콜에서 왜 중요할까?**
    - 네트워크 통신에서는 요청이 **중복될 가능성이 높음 
    →** 클라이언트가 서버에 같은 요청을 여러번 보낸다면, 중복 요청으로 중복 결제, 중복 메시지 등이 발생 가능!
    → **만약 멱등성이 보장된다면?** 
    ⇒ 동일한 요청이 여러 번 호출되어도, 동일한 결과를 반환하므로 **중복 호출에 의한 부작용을 최소화할 수 있음**

<aside>
💡 POST는 멱등성을 보장하지 않아서, 서버-클라이언트에 각각 별도로 처리를 해주어야 함

**[클라이언트]**
 한 번의 요청에 대한 응답이 완료되기 전에 다른 요청을 보내지 않도록 처리 
(async/await 등)

[**서버]**
- 서버에서 생성된 리소스의 고유 식별자를 클라이언트에게 반환
 - 클라이언트가 다시 POST 요청을 보낼 때 고유 식별자를 함께 보내도록 요청 수정을 유도함 
- 중복 요청일 경우, 이전 요청에 대한 처리 결과를 반환, 혹은 중복 요청 자체를 무시

</aside>

### HTTP METHOD

1. `POST`
    1. 서버에 새로운 자원을 생성하거나, **기존 자원을 수정**하는 메소드
    2. 같은 요청을 3번 보내면 ? 
    → (1번 요청한 것과 다르게) **서버에 다른 3개의 리소스를 생성** 
    ⇒ **멱등성 보장 X**
    
    ```jsx
    //요청
    **POST /blog-posts** HTTP/1.1
    Host: example.com
    Content-Type: application/json
    
    {
      "title": "My first blog post",
      "content": "This is my very first blog post."
    }
    ```
    
    ```jsx
    //요청1에 대한 응답 
    HTTP/1.1 201 Created
    Location: /blog-posts/12345
    Content-Type: application/json
    
    {
      "id": "**12345**",
      "title": "My first blog post",
      "content": "This is my very first blog post."
    }
    
    //요청2에 대한 응답 
    HTTP/1.1 201 Created
    Location: /blog-posts/12346
    Content-Type: application/json
    
    {
      "id": "**12346**",
      "title": "My first blog post",
      "content": "This is my very first blog post."
    }
    
    //요청3에 대한 응답
    HTTP/1.1 201 Created
    Location: /blog-posts/12347
    Content-Type: application/json
    
    {
      "id": "**12347**",
      "title": "My first blog post",
      "content": "This is my very first blog post."
    }
    ```
    
    <aside>
    💡 POST를 사용하여 멱등성을 보장하려면,
    1. 서버가 클라이언트로부터 중복 요청을 감지하고 처리
    **2. 클라이언트에서 고유한 값을 생성해서 보내야 함 
    
    before**
    
    {
    "id": 12345
    "title": "My first blog post",
    "content": "This is my very first blog post."
    }
    
    **after**
    
    {
    "**id": {UUID로 생성한 값}**
    "title": "My first blog post",
    "content": "This is my very first blog post."
    }
    
    </aside>
    
2. `DELETE`
    1. 서버에 존재하는 자원을 삭제

```jsx
//요청
**DELETE /blog-posts/1** HTTP/1.1
Host: example.com
Content-Type: application/json
```

<aside>
💡 **DELETE** 메서드의 경우,
- 첫 번째 요청은 해당 리소스가 존재하고 삭제되어 반환되는 200 상태 코드를 응답
-  두 번째 요청에서는 해당 리소스가 이미 삭제되어서 404 상태 코드를 반환
→ 이렇게 두 요청 모두 동일한 효과(=**두 요청 모두 서버의 리소스에서 idX에 해당하는 데이터를 삭제한다 = 서버의 상태를 완전히 동일한 상태로 만든다**) 를 가져와 리소스의 상태 변화가 없기 때문에 멱등성이 보장됨
= **server state will remain the same** as it was after the first request since the resource no longer exists.

</aside>

```jsx
//요청
**DELETE /blog-posts/last** HTTP/1.1
Host: example.com
Content-Type: application/json
```

<aside>
💡 DELETE 메서드를 사용해 "목록의 마지막 항목 제거" 기능을 구현해서는 안된다

For example, the first time the function is called, **the list would be changed from [1, 2, 3, 4, 5] to [1, 2, 3, 4**].
 If the same function is called again, **the list would be changed from [1, 2, 3, 4] to [1, 2, 3].** 
Each subsequent call to the function would result in a different outcome, so the function is not idempotent.

</aside>

3. `PUT`
    1. 서버에 존재하는 자원을 완전히 대체 
        - 같은 요청을 여러 번 보내도 리소스 상태는 동일함
        - **중복 요청이 일어난다면? 그 중 마지막 요청만 처리**하고 나머지는 무시, 에러 응답을 반환함
    
    ```json
    //요청
    **PUT** **/posts/123** HTTP/1.1
    Content-Type: application/json
    
    {
      "title": "My updated blog post",
      "content": "This is my updated blog post."
    }
    
    ```
    
4.  `PATCH`
    1. 서버에 존재하는 자원의 일부분만 수정

```json
//요청
**PATCH /posts/123** HTTP/1.1
Host: example.com
Content-Type: application/json

{
  "title": "My updated blog post",
  "content": "This is my updated blog post."
}

//응답
**PATCH /posts/123** HTTP/1.1
Host: example.com
Content-Type: application/json

{
  "title": "My updated blog post",
  "content": "This is my updated blog post."
}

```
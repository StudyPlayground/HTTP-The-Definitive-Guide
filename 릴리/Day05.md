![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4a63ba2e-9d17-4427-9b8f-1ec8089d2375/Untitled.png)

`Cache-Control: public`

public 응답 지시어는 응답을 공유 캐시에 저장할 수 있음을 나타낸다. `Authorization` 필드가 있는 요청에 대한 응답은 공유 캐시에 저장해서는 안되지만, 공개 지시어를 사용하면 해당 응답이 공유 캐시에 저장된다. 

일반적으로, 페이지가 Basic Auth 또는 Digest Auth 인 경우 브라우저는 `Authorization`  헤더와 함께 요청을 보낸다. 즉, 응답은 특정 사용자에 대해 액세스가 제어되며, `max-age` 가 있어도 공유 캐싱이 불가능하다. 

 이럴 경우에 `public` 지침을 사용해 해당 제한을 해제할 수 있다. `s-maxage` 나 `must-revalidate` 도 이 지침에서 풀린다. 요청에 권한 부여 헤더가 없거나 응답에 이미 s-maxage나 must-revalidate를 사용할 경우에는 이걸 사용할 필요가 없다. 

`Cache-Control: private`

private 캐시에만 저장할 수 있음을 나타낸다. 사용자 개인화 콘텐츠, 특히 로그인 후 받은 응답과 세션에 개인 지시문을 추가해야 한다. private 콘텐츠가 포함된 응답에 private 추가를 잊어버리면 해당 응답이 공유 캐시에 저장되어 여러 사용자에게 재사용될 수 있으며, 이로 인해 개인정보가 유출될 수 있다. 

- 리디 개인정보 노출 사고

['로그인하면 다른 사람 정보가'…리디, 개인정보 유출 사고](https://www.sedaily.com/NewsView/29N7RDWCX7)

- 캐시 정책

[HTTP Cache로 불필요한 네트워크 요청 방지](https://web.dev/i18n/ko/http-cache/)

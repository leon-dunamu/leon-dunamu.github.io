---
layout: post
subtitle: "HTTP vs HTTPS"
title: "frontend 면접 대비 HTTP, HTTPS 비교"
author: "Seog"
header-style: text
tags: 
  - Frontend
---

결정적으로 <span style="font-weight:bold;color:purple;">보안</span> 차이

- http방식은 네트워크상에서 정보를 누군가가 마음대로 열람과 수정이 가능하지만, https는 누가 볼 수 없도록 막음
- http방식이 https방식보다 빠름
- http방식은 민감한정보를 다룰 때 항상 변조, 해킹 가능성을 생각해야 함. https는 설치 및 인증서를 유지하는데 추가적인 비용이 발생. 따라서, 민감한 정보가 있는 페이지의 경우 https 그럴 필요가 없으면 http로 만들면 된다.

<br/><br/>

## HTTP

> <span style="font-weight:bold;color:darkgreen;">H</span>yper <span style="font-weight:bold;color:darkgreen;">T</span>ext <span style="font-weight:bold;color:darkgreen;">T</span>ransfer <span style="font-weight:bold;color:darkgreen;">P</span>rotocol <br/>
> 서로 다른 시스템들 사이에서 통신을 주고받게 해주는 가장 기초적인 프로토콜 <br/>
> HTTP 프로토콜은 일반적으로 TCP/IP 통신 위에서 동작하며 기본 포트는 80번

![image](https://user-images.githubusercontent.com/49581472/107761568-eb831a80-6d6e-11eb-8432-cf5c1995b416.png)

- 비연결성 : 클라이언트와 서버가 한 번 연결을 맺은 후, 클라이언트 요청에 대해 서버가 응답을 마치면 맺었던 연결을 끊어 버리는 성질을 말한다.
  - 장점 : 서버에서 다수의 클라이언트와 연결을 계속 유지하기엔 비용이 너무 크다. 따라서 비연결성으로 통신을 할 시 더 많은 연결을 할 수 있다.
  - 단점 : 매번 새로운 연결 시도/해제 과정을 거친다.
- 무상태 : 서버는 클라이언트를 식별할 수가 없음
  - 상태를 기억하는 방법 -
    - 쿠키
    - 세션
      - 브라우저가 아닌 서버단에서 사용자 정보를 저장하는 구조. 쿠키보다는 안전. 많이 사용하면 서버의 메모리를 차지하게 되고 과부화의 원인이 된다.
    - 토큰 (OAuth, JWT)
      - 쿠키와 세션의 문제점 보완. 보호할 데이터를 토큰으로 치환하여 원본 데이터 대신 토큰을 사용하는 기술.
- 헤더 : 클라이언트와 서버가 요청 또는 응답으로 부가적인 정보를 전송할 수 있도록 해준다.

- HTTP 1.1 vs HTTP 2.0

  가장 큰 차이는 속도이다. 2.0같은 경우는 헤더를 압축해서 보내기도 하고, 한번의 연결로 동시에 여러 메시지를 주고 받을 수도 있다.

- HTTP 쿠키 → [https://developer.mozilla.org/ko/docs/Web/HTTP/Headers/Set-Cookie](https://developer.mozilla.org/ko/docs/Web/HTTP/Headers/Set-Cookie)

- Request Method

  - <span style="font-weight:bold;color:green;">GET</span> : 존재하는 자원에 대한 **요청**
  - <span style="font-weight:bold;color:blue;">POST</span> : 새로운 자원을 **생성**
  - <span style="font-weight:bold;color:orange;">PUT</span> : 존재하는 자원에 대한 **변경**
  - <span style="font-weight:bold;color:red;">DELETE</span> : 존재하는 자원에 대한 **삭제**
  - HEAD : 서버 헤더 정보를 획득. GET과 비슷하나 Response Body를 반환하지 않음
  - OPTIONS : 서버 옵션들을 확인하기 위한 요청. [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS)에서 사용

- Response

  ### 2xx - 성공

  200번대의 상태 코드는 대부분 성공을 의미

  - <span style="font-weight:bold;color:blue;">200</span> : GET 요청에 대한 성공
  - 204 : No Content. 성공했으나 응답 본문에 데이터가 없음
  - 205 : Reset Content. 성공했으나 클라이언트의 화면을 새로 고침하도록 권고
  - 206 : Partial Conent. 성공했으나 일부 범위의 데이터만 반환

  ### 3xx - 리다이렉션

  300번대의 상태 코드는 대부분 클라이언트가 이전 주소로 데이터를 요청하여 서버에서 새 URL로 리다이렉트를 유도하는 경우

  - 301 : Moved Permanently, 요청한 자원이 새 URL에 존재
  - 303 : See Other, 요청한 자원이 임시 주소에 존재
  - 304 : Not Modified, 요청한 자원이 변경되지 않았으므로 클라이언트에서 캐싱된 자원을 사용하도록 권고. [ETag](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/ETag)와 같은 정보를 활용하여 변경 여부를 확인

  ### 4xx - 클라이언트 에러

  400번대 상태 코드는 대부분 클라이언트의 코드가 잘못된 경우. 유효하지 않은 자원을 요청했거나 요청이나 권한이 잘못된 경우 발생. 가장 익숙한 상태 코드는 404 코드. 요청한 자원이 서버에 없다라는 뜻

  - <span style="font-weight:bold;color:red;">400</span> : Bad Request, 잘못된 요청
  - 401 : Unauthorized, 권한 없이 요청. Authorization 헤더가 잘못된 경우
  - 403 : Forbidden, 서버에서 해당 자원에 대해 접근 금지
  - 405 : Method Not Allowed, 허용되지 않은 요청 메서드
  - 409 : Conflict, 최신 자원이 아닌데 업데이트하는 경우. ex) 파일 업로드 시 버전 충돌

  ### 5xx - 서버 에러

  500번대 상태 코드는 서버 쪽에서 오류가 난 경우

  - 501 : Not Implemented, 요청한 동작에 대해 서버가 수행할 수 없는 경우
  - 503 : Service Unavailable, 서버가 과부하 또는 유지 보수로 내려간 경우

<br/><br/>

## HTTPS

> <span style="font-weight:bold;color:darkgreen;">H</span>yper <span style="font-weight:bold;color:darkgreen;">T</span>ext <span style="font-weight:bold;color:darkgreen;">T</span>ransfer <span style="font-weight:bold;color:darkgreen;">P</span>rotocol <span style="font-weight:bold;color:purple;">Secure</span> <br/>
> HTTP 프로토콜에 `보안 기능을 추가`한 것 <br/>
> HTTPS 프로토콜은 <span style="font-weight:bold;color:purple;">SSL</span>(보안 소켓 계층)을 사용함으로써 이 문제를 해결

- SSL은 서버와 브라우저 사이에 안전하게 암호화된 연결을 만들 수 있게 도와주고, 서버 브라우저가 민감한 정보를 주고받을 때 이것이 도난당하는 것을 막아줌
- TLS(전송 계층 보안) 프로토콜을 통해서도 보안을 유지함. TSL은 데이터 무결성을 제공하기 때문에 데이터가 전송 중에 수정되거나 손상되는 것을 방지하며, 사용자가 자신이 의도하는 웹사이트와 통신하고 있음을 입증하는 인증 기능도 제공함.

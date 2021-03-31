---
layout: post
subtitle: "script 태그의 async, defer"
title: "frontend 면접 대비 스크립트 로드 시점 변화"
author: "Seog"
header-style: text
tags: 
  - Frontend
---

`</body>` 요소 바로 앞에 **script tag**가 존재하면, `async` 또는 `defer` 속성을 사용하는 것이 큰 효과가 없다

<br />

##### 1. 일반적인 실행

![image](https://user-images.githubusercontent.com/49581472/107726594-c2da3100-6d2c-11eb-88f2-17bfbd979059.png)

HTML구문 중지 후 로드 → script 적용 → HTML 파싱 재개

<br />

##### 2. async 속성이 추가된 실행

![image](https://user-images.githubusercontent.com/49581472/107726634-da191e80-6d2c-11eb-9c32-0ed20a75abcc.png)

**로드 중에도 파싱** → 로드 완료 후 script 적용 (이 때 **HTML 파싱 중단**) → **끝나고 재개**

<br />

##### 3. defer 속성이 추가된 실행

![image](https://user-images.githubusercontent.com/49581472/107726660-e604e080-6d2c-11eb-955a-3bfc527d17fe.png)

**로드 중에도 파싱** → HTML 파싱 **끝난 후** script 적용

<br />

#### HTTP 연결을 계속 연결 & 해제 거치면 비효율적이지 않나?

⇒ HTTP/1.1부터 **keep-alive 헤더**가 추가되어 커넥션을 맺은 체로 사용 가능

하지만, 클라이언트가 한 번 호출되는 경우라면 keep-alive유지가 더 비효율적.

어플리케이션 앞 단에 프록시 서버가 있다면 프록시와 어플리케이션은 keep-alive하는 것이 이로움.

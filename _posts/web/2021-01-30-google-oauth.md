---
layout: post
subtitle: "구글 Oauth 사용하기"
title: "구글 로그인 사용하기"
author: "Seog"
header-style: text
tags: 
  - Frontend
  - Oauth
---

구글 로그인(OAuth) 사용하기

### 1. <a href="https://console.cloud.google.com/" target="_blank">https://console.cloud.google.com/</a> 로 이동 후 프로젝트 생성

![image](https://user-images.githubusercontent.com/77267603/108161443-66ad4d80-712e-11eb-8634-6071838cfea6.png)



### 2. 햄버거메뉴 > API 및 서비스 > 사용자 인증 정보

![image](https://user-images.githubusercontent.com/77267603/108161504-8a709380-712e-11eb-86bb-cace03b1058c.png)



### 3. 상단의 +사용자 인증 정보 만들기 > OAuth 클라이언트 ID

![image](https://user-images.githubusercontent.com/77267603/111246564-d2a9a580-8649-11eb-868f-836e7d2e0156.png)


> 이후 동의화면 구성이라는 화면이 나오면 알맞는 도메인, 앱이름 등을 기입하여 완료




### 4. 웹 어플리케이션으로 지정 후, URI, 승인된 리디렉션 등 기입
URI는 프론트딴의 URI(로그인을 요청하는 곳)
리디렉션은 로그인 요청 후 토큰을 받아갈 URI로 작성




### 5. 발급 완료

![image](https://user-images.githubusercontent.com/77267603/108161616-be4bb900-712e-11eb-9ef7-f21894aabf6b.png)


### 6. 구글 로그인 버튼 생성

```html
<input type="button" id="loginBtn" value="login">
<script src="https://code.jquery.com/jquery-3.4.1.min.js"></script>
<script>
    $("#loginBtn").click(function(){
        location.href="https://accounts.google.com/o/oauth2/auth?client_id="+
        "클라이언트 ID를 여기에 넣습니다."+
        "&redirect_uri="+
        "http://localhost/redirect.html" +
        "&response_type=code&scope=https://www.googleapis.com/auth/userinfo.email&approval_prompt=force&access_type=offline";
    });
</script>
```
>예시로 작성된 index.html 임<br/>클라이언트 ID 입력 요망<br/>redirect URI는 구글 콘솔에서 허용된 목록에 존재해야만 함<br/>


### 7. 로그인 버튼 클릭 후 구글 로그인 진행

![image](https://user-images.githubusercontent.com/77267603/108161787-20a4b980-712f-11eb-83c8-ca26b24286cf.png)


> 이 작업을 통해 token발급에 필요한 code를 얻을 수 있다


### 8. 얻은 코드를 통해 token으로 변환

```javascript
function getParameter(name){
	var list = location.search.substring(1).split('&');
	for(var i=0;i<list.length;i++){
		var data = list[i].split('=');
		if(data.length === 2){
			if(data[0] === name){
				return data[1];
			}
		}
	}
	return null;
}
```

> URI의 파라미터를 통해 code를 반환 받을 수 있다

- 해당 코드와 여러 정보를 통해 POST로 token을 받을 수 있다

    ```json
    method : POST
    URI : https://accounts.google.com/o/oauth2/token
    {
        "code" : "이전 작업에서 얻은 code",
        "client_id" : "발급받은 Client ID",
        "client_secret" : "발급받은 Client Secret",
        "redirect_uri" : "토큰을 받을 URI",
        "grant_type" : "authorization_code"    // 고정?
    }
    ```


- 위의 결과는 아래와 같이 반환된다
    ```json
    {
        "access_token": "사용자의 정보를 얻을 때 사용될 토큰",
        "expires_in": 3599,
        "refresh_token": ".......",
        "scope": "https://www.googleapis.com/auth/userinfo.email openid",
        "token_type": "Bearer",
        "id_token": "......."
    }
    ```


- 위의 access_token으로 사용자의 정보를 얻을 수 있다

    ```bash
    method : GET
    URI : https://www.googleapis.com/oauth2/v1/userinfo?access_token={위의 access_token}
    ```


- 반환된 사용자의 정보

    ```json
    {
        "id": "108976569521291913440",
        "email": "dnjstjr31@gmail.com",
        "verified_email": true,
        "picture": "https://....../photo.jpg"
    }
    ```




















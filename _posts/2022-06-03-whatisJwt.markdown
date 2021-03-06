---
layout: post
title:  "JSON Web Token 소개 및 구조"
date:   2022-06-03 17:44:00 +0900
categories: SpringBoot
---

### 들어가며
MSA 프로젝트에서 인증서버를 구현하기위해 JWT를 선택했습니다. 그래서 JWT를 선택한 이유와 왜 쓰는지 그리고 구조는 어떠한지 알아보겠습니다.<br>
JWT를 선택한 이유는 권한부여와 유저의 세션을 유지할 필요가 없기때문에 서버의 자원을 낭비할 필요가 없습니다. 그리고 정보가 도중에 조작되었는지 <br>
검사가 가능해 안정성있게 정보를 교환하기에 좋은 방법입니다.

### JWT 구조
<img src="/public/img/jwtRescue.png"  width="500" height="300"/><br>
JWT는 Header, Payload, Signature의 3 부분으로 이루어지며, JSON 형태인 각 부분은 Base64로 인코딩 되어 표현됩니다. <br>
또한 각각의 부분을 이어 주기 위해 . 구분자를 사용하여 구분합니다. 추가로 Base64는 암호화된 문자열이 아니고, <br>
같은 문자열에 대해 항상 같은 인코딩 문자열을 반환합니다.
1. Header
 - Header 는 두가지의 정보를 지니고 있습니다.
 - typ: 토큰의 타입을 지정합니다. ex)JWT
 - alg: 해싱 알고리즘을 지정합니다. 해싱 알고리즘으로는 보통 HMAC SHA256 혹은 RSA 가 사용되며, 이 알고리즘은, 토큰을 검증 할 때 사용되는 signature 부분에서 사용됩니다.
 ```
 { 
 "typ": "JWT",
 "alg": "HS256"
 }
 ```
2. Payload
 - Payload 부분에는 토큰에 담을 정보가 들어있습니다. 여기에 담는 정보의 한 ‘조각’ 을 클레임(Claim) 이라고 부르고, 이는 Json(Key/Value) 형태의 한 쌍으로 이뤄져있습니다. 
   토큰에는 여러개의 클레임들을 넣을 수 있습니다. 
 - 클레임의 종류는 크게 3가지입니다. 
        - 등록된 클레임 : 등록된 클레임들은 서비스에서 필요한 정보들이 아닌, 토큰에 대한 정보들을 담기위하여 이름이 이미 정해진 클레임들입니다
            - iss: 토큰 발급자(issuer)
            - sub: 토큰 제목(subject)
            - aud: 토큰 대상자(audience)
            - exp: 토큰 만료 시간(expiration), NumericDate 형식으로 되어 있어야 함 ex) 1480849147370
            - nbf: 토큰 활성 날짜(not before), 이 날이 지나기 전의 토큰은 활성화되지 않음
            - iat: 토큰 발급 시간(issued at), 토큰 발급 이후의 경과 시간을 알 수 있음
            - jti: JWT 토큰 식별자(JWT ID), 중복 방지를 위해 사용하며, 일회용 토큰(Access Token) 등에 사용
        - 공개 클레임 : 공개 클레임들은 충돌이 방지된 (collision-resistant) 이름을 가지고 있어야 합니다. 충돌을 방지하기 위해서는, 클레임 이름을 URI 형식으로 짓습니다.
        - 비공개 클레임 : 등록된 클레임도아니고, 공개된 클레임들도 아닙니다. 양 측간에 (보통 클라이언트 <->서버) 협의하에 사용되는 클레임 이름들입니다.
        
        ```
        {
        "sub": "1234567890", // 등록된 클레임
        "name": "kimsiyong", // 비공개 클레임
        "iat": 1516239022 // 등록된 클레임
        }
        ```
3. Signature
 - 서명(Signature)은 토큰을 인코딩하거나 유효성 검증을 할 때 사용하는 고유한 암호화 코드입니다. 서명은 위에서 만든 헤더(Header)와 페이로드(Payload)의 값을 각각 BASE64로 인코딩하고, 인코딩한 값을 비밀 키를 이용해 헤더(Header)에서 정의한 알고리즘으로 해싱을 하고, 이 값을 다시 BASE64로 인코딩하여 생성합니다.

4. JWT 예시(jwt.io)
<div style="text-align:center;">
<img src="/public/img/jwtSite.png"  width="800" height="300" style="margin-left : auto; margin-right: auto;"/>
</div>

### JWT 장단점
##### 장점
 - 사용자 인증 정보는 토큰에 포함하기 때문에 별도의 저장소가 필요 없습니다.
 - 쿠키를 전달하지 않아도 되므로 쿠키를 사용함으로써 발생하는 취약점이 사라집니다.
 - URL 파라미터와 헤더로 사용, 트래픽에 대한 부담이 낮음, 내장된 만료 등이 있습니다.

##### 단점
 - Self-contained: 토큰 자체에 정보를 담고 있으므로 양날의 검이 될 수 있습니다.
 - 토큰 길이: 토큰의 페이로드(Payload)에 3종류의 클레임을 저장하기 때문에, 정보가 많아질수록 토큰의 길이가 늘어나 네트워크에 부하를 줄 수 있습니다.
 - Payload 인코딩: 페이로드(Payload) 자체는 암호화 된 것이 아니라, BASE64로 인코딩 된 것입니다. 중간에 Payload를 탈취하여 디코딩하면 데이터를 볼 수 있으므로, JWE로 암호화하거나 Payload에 중요 데이터를 넣지 않아야 합니다.
 - Stateless: JWT는 상태를 저장하지 않기 때문에 한번 만들어지면 제어가 불가능합니다. 즉, 토큰을 임의로 삭제하는 것이 불가능하므로 토큰 만료 시간을 꼭 넣어주어야 합니다.
 - Tore Token: 토큰은 클라이언트 측에서 관리해야 하기 때문에, 토큰을 저장해야 합니다.

### 마무리하며
이번에 JWT의 구조가 어떻게 만들어졌는지 제대로 알게되었고, 어떤 과정을 통해서 만들어지는지 확인했습니다.<br>
앞으로도 더 노력하고 발전하는 개발자가 되겠습니다.




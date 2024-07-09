> 참고 : https://inpa.tistory.com/559

## JWT(JSON Web Token) 란?
JWT란 **인증에 필요한 정보들을 암호화시킨 JSON 토큰**을 의미한다. 그리고 JWT 기반 인증은 **JWT 토큰** (일반적으로 Access Token을 의미)을 **HTTP 헤더에 실어** 서버가 클라이언트를 식별하는 방식이다.
JWT는 JSON 데이터를 **BASE64 URL-safe Encode** 를 통해 인코딩하여 직렬화한 것이며, 토큰 내부에는 위변조 방지를 위해 개인키를 통한 전자서명도 들어있다. 따라서 사용자가 JWT를 서버로 전송하면 서버는 서명을 검증하는 과정을 거치게 되며, 검증이 완료되면 요청한 응답을 돌려준다.

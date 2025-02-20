> 참고 : 
> - https://puleugo.tistory.com/138
> - https://inpa.tistory.com/559

일반적으로 서버가 클라이언트를 인증하는 방식은 **쿠키, 세션, 토큰** 3가지 방식이 있다.

## Cookie 인증
- Cookie는 **Key-Value 형식의 문자열** 덩어리이다.
- 클라이언트가 어떤 웹사이트를 방문할 경우, 그 사이트가 사용하고 있는 서버를 통해 **클라이언트의 브라우저에 설치되는 작은 기록 정보 파일**이다. 각 사용자마다 브라우저에 정보를 저장하니, 고유 정보 식별이 가능한 것이다.

![](../../assets/Cookie%20인증%20방식.png)
1. 브라우저(클라이언트)가 서버에 요청(접속)을 보낸다.
2. 서버는 클라이언트의 요청에 대한 응답을 작성할 때, 클라이언트 측에 저장하고 싶은 정보를 응답 헤더의 Set-Cookie에 담는다.
3. 이후 해당 클라이언트는 요청을 보낼 때마다, 매번 저장된 쿠키를 요청 헤더의 Cookie에 담아 보낸다. 서버는 쿠키에 담긴 정보를 바탕으로 해당 요청의 클라이언트가 누군지 식별하거나, 정보를 바탕으로 추천 광고를 띄우거나 한다.

### Cookie 방식의 단점
- 가장 큰 단점은 **보안에 취약**하다는 점이다.
	- 요청 시 쿠키의 값을 그대로 보내기 때문에, 유출 및 조작 당할 위험이 존재한다.
- 쿠키에는 **용량 제한**이 있다.
- 웹 브라우저마다 쿠키에 대한 지원 형태가 다르기 때문에, **브라우저간 공유가 불가능**하다.
- 쿠키의 사이즈가 커질수록 네트워크에 부하가 심해진다.


## Session 인증
- 쿠키의 보안적 이슈를 해결하기 위해, 세션은 비밀번호 등 클라이언트의 민감한 인증 정보를 브라우저가 아닌 **서버 측에 저장하고 관리**한다.
	- 서버의 메모리에 저장하기도 하고, 서버의 로컬 파일이나 데이터베이스에 저장하기도 한다.
- 핵심 골자는 민감한 정보는 클라이언트에 보내지 말고, 서버에서 모두 관리한다는 점이다.
- Session도 마찬가지로 **Key-Value** 형태의 자료 구조로 구성되어 있다.
	- Key: **SESSION ID**
	- Value: **Map** (세션 생성 시간, 마지막 접근 시간, User가 저장한 속성 등을 포함)

![](../../assets/Session%20인증%20방식.png)
1. 유저가 웹사이트에서 로그인하면, 세션이 서버 메모리(혹은 데이터베이스) 상에 저장된다. 이 때, 세션을 식별하기 위한 Session ID를 기준으로 정보를 저장한다.
2. 서버에서 브라우저의 쿠키에 Session ID를 저장한다.
3. 쿠키에 정보가 담겨있기 때문에, 브라우저는 해당 사이트에 대한 모든 Request에 Session ID를 담아 전송한다.
4. 서버는 클라이언트가 보낸 Session ID와 서버 메모리로 관리하고 있는 Session ID를 비교하여 인증을 수행한다.

### Session 방식의 단점
- 쿠키를 포함한 요청이 외부에 노출되더라도 Session ID 자체는 유의미한 개인정보를 담고 있지 않는다. 그러나 해커가 **Session ID 자체를 탈취**하여 클라이언트인척 위장할 수 있다는 보안적인 한계가 존재한다.
	- (서버에서 IP 인증 절차를 추가하면 해결이 가능한 문제이긴 하다.)
- 서버에서 세션 저장소를 사용하므로, 요청이 많아지면 서버에 부하가 심해진다.


## Token 인증
- 토큰 기반 인증 시스템은 클라이언트가 서버에 접속하면 서버에서 해당 클라이언트에게 인증되었다는 의미로 **'토큰'을 부여**한다.
	- 이 토큰은 **유일**하며, 토큰을 발급받은 클라이언트는 또 다시 서버에 요청을 보낼 때 요청 헤더에 토큰을 심어서 보낸다.
	- 그러면 서버에서는 클라이언트로부터 받은 토큰을 서버에서 제공한 토큰과의 일치 여부를 체크하여 인증 과정을 처리하게 된다.
- 기존의 **세션 기반** 인증은 서버가 파일이나 데이터베이스에 세션 정보를 가지고 있어야 하고, 이를 조회하는 과정이 필요하기 때문에 **많은 오버헤드**가 발생한다.
	- 하지만 토큰은 세션과는 달리 서버가 아닌 클라이언트에 저장되기 때문에, 메모리나 스토리지 등을 통해 세션을 관리했던 서버의 부담을 덜 수 있다.
	- 토큰 자체에 데이터가 들어있기 때문에 클라이언트에서 받아 위조되었는지 판별만 하면 되기 때문.

### 서버 기반 vs 토큰 기반
#### 서버(세션) 기반 인증 시스템
서버의 세션을 사용해 사용자 인증을 하는 방법으로, 서버측(서버 RAM or DB)에서 사용자의 인증 정보를 관리하는 것을 의미한다.
그러다 보니, 클라이언트로부터 요청을 받으면 클라이언트의 상태를 계속해서 유지해놓고 사용한다. **(Stateful)**
이는 사용자가 증가함에 따라 성능의 문제를 일으킬 수 있으며, 확장성이 떨어진다는 단점을 지닌다.

#### 토큰 기반 인증 시스템
서버 기반의 단점을 극복하기 위해서 "토큰 기반 인증 시스템"이 나타났다.
인증받은 사용자에게 토큰을 발급하고, 로그인이 필요한 작업일 경우 헤더에 토큰을 함께 보내 인증받은 사용자인지 확인한다. 이는 서버 기반 인증 시스템과 달리, 상태를 유지하지 않으므로 **Stateless** 한 특징을 갖고 있다.

![](../../assets/Token%20인증%20방식.png)

1. 사용자가 아이디와 비밀번호로 로그인을 한다.
2. 서버 측에서 사용자(클라이언트)에게 **유일한 토큰**을 발급한다.
3. 클라이언트는 서버 측에서 전달받은 토큰을 쿠키나 스토리지에 저장해두고, 서버에 요청할 때마다 해당 토큰을 HTTP 요청 헤더에 포함시켜서 전달한다.
4. 서버는 전달받은 토큰을 검증하고 요청에 응답한다.
	1. 토큰에는 요청한 사람의 정보가 담겨있기 때문에, 서버는 DB를 조회하지 않아도 누가 요청하는지 알 수 있다.

### Token 방식의 단점
- 쿠키/세션과 다르게 토큰 자체의 데이터가 길어, 인증 요청이 많아질수록 네트워크 부하가 심해질 수 있다.
- Payload 자체는 암호화 되지 않기 때문에, **중요한 정보는 담을 수 없다.**
- 토큰을 **탈취**당하면 대처하기 어렵다.
	- 사용 기간 제한을 설정하는 식으로 극복한다.
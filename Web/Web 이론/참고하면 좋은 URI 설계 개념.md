
> 출처: 인프런 "모든 개발자를 위한 HTTP 웹 기본 지식 - 김영한" 강의 중

- **문서(document)**
	- 단일 개념(파일 하나, 객체 인스턴스, <u>데이터베이스 row</u>)
	- 예) /members/100, /files/star.jpg
- **컬렉션(collection)**
	- 서버가 관리하는 리소스 디렉터리
	- 서버가 리소스의 URI를 생성하고 관리
	- 예) /members
- **스토어(store)**
	- 클라이언트가 관리하는 자원 저장소
	- 클라이언트가 리소스의 URI를 알고 관리
	- 예) /files
- **컨트롤러(controller), 컨트롤 URL**
	- 문서, 컬렉션, 스토어로 해결하기 어려운 추가 프로세스 실행
	- **동사**를 직접 사용
	- 예) /members/{id}/delete

> 참고 사이트
> https://stackoverflow.com/questions/74873899/uidevice-current-name-returns-wrong-value

![](../../assets/Device%20이름%20엑세스하기(GPT).png)


## 1. 앱에서 Device 이름을 사용하는 화면을 작성하고, 캡쳐하기
이 기능을 사용하려면 **앱에 사용자에게 디바이스 이름을 보여주는 화면**이 필요하다. 그리고 이 화면을 증거 자료로 하여 애플 개발자 사이트에 권한 신청을 한다.

## 2. 애플 개발자 사이트에서 권한 신청하기
> 애플 개발자 사이트 : https://developer.apple.com/contact/request/user-assigned-device-name/

위 사이트에 들어가서 권한을 요청한다.

## 3. 앱스토어 커넥트 - 앱 Identifier 에서 capability 추가하기
아래 절차는 **승인 완료 메일을 받은 다음**에 진행할 수 있음.

> 출처 : https://stackoverflow.com/questions/72983299/how-to-get-an-ipads-device-name-on-ios16

![](../../assets/Device%20이름%20액세스%20-%20앱%20ID.png)

## 4. info.plist 파일에 권한 추가하기

작성중
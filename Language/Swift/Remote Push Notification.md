
참고 링크들

- Silent Push 사용 관련 헤더값
	- https://medium.com/@jang.wangsu/ios-silent-push-%EC%97%90-%EB%8C%80%ED%95%98%EC%97%AC-4a2884c7d9f8
- Apple Push Notification Console 사용법
	- https://youtu.be/LNgpmhesJ0s?si=rRzmyqxapvMbLLEQ
	- https://developer.apple.com/videos/play/wwdc2023/10025/
- 코드 참고
	- https://github.com/JK0369/ExPushTest
- UIKit Docs - `application(_:didReceiveRemoteNotification:fetchCompletionHandler:)`
	- Silent Push를 캐치하는 메서드
	- https://developer.apple.com/documentation/uikit/uiapplicationdelegate/1623013-application
- UserNotification 프레임워크 - `UNUserNotificationCenter` 
	- https://developer.apple.com/documentation/usernotifications/unusernotificationcenter


- 단체방에 올린 내용 (다시 정리해서 블로그로 정리하면 좋을듯)
```
안녕하세요. 어제 질문 드렸던 background 상태일 때 Push 관련해서, 조언 주신 내용들 토대로 문제를 해결할 수 있게 됐습니다. 정말 감사드립니다.

결론먼저 말씀드리면, 제가 원했던 기능은 "Silent Push" 기능이었습니다.
제가 Remote Push 기능 자체를 이번에 처음 다뤄보면서 "Silent Push"라는 키워드를 잘 모르기도 했고, 개념을 잘못 이해하거나 FCM 설정 문제, 지레짐작으로 넘겨짚고 넘어간 부분들이 뒤죽박죽 섞이는 등 Silent Push 테스트 환경을 정상적으로 구성하지 못했던 것이 이번 삽질의 가장 큰 원인이었습니다. (질문 드려보길 정말 잘했다는 생각이 듭니다 😂)

이번에 알게된 점을 간략하게 정리해보자면,

1. 파츠님께서 말씀해주신대로, {"content-available": 1} 값을 함께 넘겨줘야만 동작하는 Silent Push 메서드는 `application(didReceiveRemoteNotification:)`가 맞습니다. 그런데 특이사항이 있습니다.

  - iOS와 macOS 간 동작에 차이가 존재합니다. iOS 에서는 해당 메서드가 "반드시 Silent Push일 때만" 동작하는데에 반해, macOS 에서는 Push가 수신되면 무조건 실행됩니다.
  - 다시 말하면, macOS 에서는 "content-available" 없이 메시지를 수신해도 해당 메서드가 동작합니다.

2. 제가 겪었던 문제의 가장 근본적인 원인은 FCM 설정과 관련되어 있었습니다,

  - FCM SDK를 프로젝트에 세팅하는 과정 중, info.plist 파일에 "FirebaseAppDelegateProxyEnabled" 값을 "NO"로 설정해주어야 한다는 내용이 있습니다. (Device Token 발급 메서드의 스위즐링 관련 설정값)
  - 인터넷의 FCM 설정 방법 설명 블로그들을 보면, 해당 값을 "String"으로 설정해야 정상 동작한다는 내용이 꽤 많습니다.
  - 하지만 현재 제 테스트 환경에서는 "Boolean"으로 값을 설정해줘야만 스위즐링이 정상 동작합니다.
    - FCM SDK가 버그였고 그게 고쳐진건지, Xcode 업데이트의 영향을 받은건지 원인은 잘 모르겠습니다 ㅜㅜ.. (저는 현재 macOS 14.1, Xcode 15.0.1 을 사용중입니다.)
  - 만약 위 설정값을 여전히 String으로 둔다면, 
    1. APNs 토큰 발급 메서드에서 에러를 print합니다.
    2. `application(didReceiveRemoteNotification)`메서드가 완전히 먹통이 되어버립니다. (그런데 `userNotificationCenter(willPresent)`, `userNotificationCenter(didReceive)` 메서드는 정상 동작합니다 ..)

어제는 2번 내용에 대한 문제를 제대로 인지하지 못해서, `application(didReceiveRemoteNotification)`가 "백그라운드에서 동작하지 않는다" 라는 내용에만 초점을 맞추고 있었습니다.
여러 방면으로 답변해주신 덕분에 더 멀리 가기 전에 해결했습니다. 정말 감사드립니다.


추가로, Push를 수신하는 각 상황마다 호출되는 메서드를 이미지로 첨부해드려봅니다.
미약하지만 도움이 되었으면 좋겠고, 혹시 또 잘못된게 있다면 지적 부탁드립니다.
아무튼 정말 감사드립니다! 😊





보류 : 

3. 사용하는 메시징 플랫폼마다 Silent Push를 사용하는 방법에 약간씩 차이가 있다는 점도 알게되었습니다. 

  - Silent Push를 발송할 때, Request Header에 ["apns-push-type": "background" / "apns-priority": 5] 두 가지 key-value 설정을 추가시켜줘야 동작한다는 내용을 찾았습니다.
    - (출처 : https://medium.com/@jang.wangsu/ios-silent-push-%EC%97%90-%EB%8C%80%ED%95%98%EC%97%AC-4a2884c7d9f8)
    - 위의 아렉스님이 캡쳐해주신 Apple의 Push Notification Console 화면을 보면 Header 값이 고정되어 있는 것을 보실 수 있습니다.
    - 만약 메시징 플랫폼으로 FCM을 사용한다면, 별도로 Header에 값을 추가할 필요 없이 payload에 "content-available" 값만 잘 전달해주면 됩니다. 
```

![](../../assets/Remote%20Push%20Notification%20method%20동작.png)
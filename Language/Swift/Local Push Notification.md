> 참고 :
> - http://yoonbumtae.com/?p=4642
> - https://velog.io/@hope1053/iOS-%EB%A1%9C%EC%BB%AC-%ED%91%B8%EC%8B%9C-%EC%95%8C%EB%A6%BCLocal-Notification-%EA%B5%AC%ED%98%84%ED%95%98%EA%B8%B0
> - https://onelife2live.tistory.com/33
> - https://2unbini.github.io/%F0%9F%93%82%20all/swift/swiftUI-Local-Notification/



## 구현

### 의존 클래스 선언

- `UNUserNotificationCenter` : Push 알림 관련 중심이 되는 개체
- `UNMutableNotificationContent` : 사용자에게 보여줄 알림의 컨텐츠(메시지, 이미지 등) 설정 개체
```swift
import UserNotifications  // 필요시에만. (SwiftUI를 import했다면 할 필요 없음.)

var userNotiCenter: UNUserNotificationCenter
let notiContent: UNMutableNotificationContent

userNotiCenter = UNUserNotificationCenter.current()
notiContent = UNMutableNotificationContent()
```


### 권한 요청

```swift
/// 권한 상태를 가져오고, 상태에 따른 동작을 수행함.
  private func getPushNotiAuthorization() {
    userNotiCenter.getNotificationSettings { settings in
      // 승인되어 있지 않은경우
      if settings.authorizationStatus != .authorized {
        // 권한 요청
        setPushNotiAuthorization()
      }
      
      // 거부되어 있는 경우
      if settings.authorizationStatus == .denied {
        showAlert.toggle()
      }
    }
  }

/// 권한 요청 Alert 표시 
  private func setPushNotiAuthorization() {
    let notiAuthOptions = UNAuthorizationOptions(arrayLiteral: [.alert, .badge, .sound])
    userNotiCenter.requestAuthorization(options: notiAuthOptions) { (success, error) in
      if let error = error {
        print(#function, error)
      }
    }
  }
```

### (거부된 경우) 설정 앱으로 이동

```swift
  // 설정 화면 Open
  private func openSettings() {
    if let settings = URL(string: UIApplication.openSettingsURLString) {
      if UIApplication.shared.canOpenURL(settings) {
        UIApplication.shared.open(settings)
      }
    }
  }
```
 
### 알림 포맷 세팅

```swift
  private func setPushMessage() {
    // Title & Message 내용 설정
    notiContent.title = "Title(test)"
    notiContent.body = "Body(test)"
    
    // User Info: 푸시 메시지에 담고싶은 정보를 [AnyHashable : Any] 타입으로 입력.
    notiContent.userInfo = ["category": "Tester", "count": 10]
    
    // 썸네일 설정
    do {
      let imageUrl = Bundle.main.url(forResource: "user", withExtension: "png")
      let attach = try UNNotificationAttachment(identifier: "Notification", url: imageUrl!, options: nil)
      notiContent.attachments.append(attach)
    } catch {
      print(error)
    }
    
    // 배지 설정 (0 이하는 배지 표시안함)
    let newNumber = UserDefaults.standard.integer(forKey: "NotiTestBadgeNumber") + 1
    UserDefaults.standard.set(newNumber, forKey: "NotiTestBadgeNumber")
    notiContent.badge = (newNumber) as NSNumber
  }
```

### 알림 메시지 Trigger 명령

```swift
  private func triggerPushMessage() {
    // Foreground 상태에서는 알람이 안보이므로, 3초 뒤에 표시되도록 설정.
    let trigger = UNTimeIntervalNotificationTrigger(timeInterval: 3, repeats: false)
    let request = UNNotificationRequest(
      identifier: "Notification",
      content: notiContent,
      trigger: trigger
    )
    
    userNotiCenter.add(request) { error in
      if let error = error {
        print(#function, error as Any)
      }
    }
  }
```


### View 코드 (SwiftUI)

```swift
  var body: some View {
    VStack {
      Button {
        // 3초뒤에 트리거되는 메시지 전송
        self.triggerPushMessage()
      } label: {
        Text("Execute Push message")
      }
    }
    .onAppear {
      // 권한 설정 상태 가져오기 & 권한 요청하기
      self.getPushNotiAuthorization()
      // 푸쉬 메시지 설정
      self.setPushMessage()
    }
    .alert(isPresented: $showAlert) {
      Alert(
        title: Text("알림 권한 요청"),
        message: Text("알림 권한이 반드시 필요함."),
        dismissButton: .default(Text("권한 설정으로 이동")) {
          directToSettings.toggle()
        }
      )
    }
    .onChange(of: directToSettings) { newValue in
      if newValue {
        DispatchQueue.main.async {
          openSettings()
        }
      }
    }
  }
```

## 전체 코드 (View 코드 한 파일에 전부 다 작성)

```swift
import SwiftUI

struct ContentView: View {
  var userNotiCenter: UNUserNotificationCenter
  let notiContent: UNMutableNotificationContent
  
  @State var showAlert: Bool
  @State var directToSettings: Bool
  
  init() {
    userNotiCenter = UNUserNotificationCenter.current()
    notiContent = UNMutableNotificationContent()
    
    showAlert = false
    directToSettings = false
  }
  
  var body: some View {
    VStack {
      Button {
        // 3초뒤에 트리거되는 메시지 전송
        self.triggerPushMessage()
      } label: {
        Text("Execute Push message")
      }
    }
    .onAppear {
      // 권한 설정 상태 가져오기 & 권한 요청하기
      self.getPushNotiAuthorization()
      // 푸쉬 메시지 설정
      self.setPushMessage()
    }
    .alert(isPresented: $showAlert) {
      Alert(
        title: Text("알림 권한 요청"),
        message: Text("알림 권한이 반드시 필요함."),
        dismissButton: .default(Text("권한 설정으로 이동")) {
          directToSettings.toggle()
        }
      )
    }
    .onChange(of: directToSettings) { newValue in
      if newValue {
        DispatchQueue.main.async {
          openSettings()
        }
      }
    }
  }
  
  private func triggerPushMessage() {
    // Foreground 상태에서는 알람이 안보이므로, 3초 뒤에 표시되도록 설정.
    let trigger = UNTimeIntervalNotificationTrigger(timeInterval: 3, repeats: false)
    let request = UNNotificationRequest(
      identifier: "Notification",
      content: notiContent,
      trigger: trigger
    )
    
    userNotiCenter.add(request) { error in
      if let error = error {
        print(#function, error as Any)
      }
    }
  }
  
  private func getPushNotiAuthorization() {
    userNotiCenter.getNotificationSettings { settings in
      // 승인되어 있지 않은경우
      if settings.authorizationStatus != .authorized {
        // 권한 요청
        setPushNotiAuthorization()
      }
      
      // 거부되어 있는 경우
      if settings.authorizationStatus == .denied {
        showAlert.toggle()
      }
    }
  }
  
  private func setPushNotiAuthorization() {
    let notiAuthOptions = UNAuthorizationOptions(arrayLiteral: [.alert, .badge, .sound])
    userNotiCenter.requestAuthorization(options: notiAuthOptions) { (success, error) in
      if let error = error {
        print(#function, error)
      }
    }
  }
  
  private func setPushMessage() {
    // Title & Message 내용 설정
    notiContent.title = "Title(test)"
    notiContent.body = "Body(test)"
    
    // User Info: 푸시 메시지에 담고싶은 정보를 [AnyHashable : Any] 타입으로 입력.
    notiContent.userInfo = ["category": "Tester", "count": 10]
    
    // 썸네일 설정
    do {
      let imageUrl = Bundle.main.url(forResource: "user", withExtension: "png")
      let attach = try UNNotificationAttachment(identifier: "Notification", url: imageUrl!, options: nil)
      notiContent.attachments.append(attach)
    } catch {
      print(error)
    }
    
    // 배지 설정 (0 이하는 배지 표시안함)
    let newNumber = UserDefaults.standard.integer(forKey: "NotiTestBadgeNumber") + 1
    UserDefaults.standard.set(newNumber, forKey: "NotiTestBadgeNumber")
    notiContent.badge = (newNumber) as NSNumber
  }
  
  // 설정 화면 Open
  private func openSettings() {
    if let settings = URL(string: UIApplication.openSettingsURLString) {
      if UIApplication.shared.canOpenURL(settings) {
        UIApplication.shared.open(settings)
      }
    }
  }
}
```
> 2023.06.30 부터 `SBPlatformDestination` type은 지원을 중단했다고 한다.
> 참고 : https://swiftybeaver.com/end-of-service.html

---
### FileDestination() 사용법
- App의 Bundle에 Log 파일을 생성하는 방식.
- macOS 앱 개발 기준
- `SwiftyBeaver` 로 log를 남기면, 다음 경로에 로그 파일이 생성된다.
	- `Users/<UserName>/Library/Containers/com.sample.myapp/Data/Documents`

##### Setting code
- `init()` or `didFinish~~`
```swift
import SwiftyBeaver

let file = FileDestination()

let url = try? FileManager.default.url(for: .documentDirectory,
									   in: .userDomainMask,
									   appropriateFor: nil,
									   create: true)

let fileURL = url?.appendingPathComponent("LogByBeaver.log")

file.logFileURL = fileURL

SwiftyBeaver.self.addDestination(file)
```

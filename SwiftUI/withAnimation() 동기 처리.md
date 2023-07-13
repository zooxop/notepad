> 참고 : [SwiftUI Animation Completion](https://medium.com/geekculture/swiftui-animation-completion-b6f0d167159e)

SwiftUI의 `withAnimation(_:_:)` 함수는 <u>**동작 완료 핸들러를 제공하지 않는다.**</u>
그래서 애니메이션이 완전히 끝난 후에 함수를 실행해야 하거나 하는 등의 **동기 처리를 하는데에 제약**이 있다.

하지만, 아래의 함수로 `withAnimation()`을 한 번 더 감싸서 사용하면 동기처리를 쉽게 할 수 있다.

---

#### 동기 처리 함수

```swift
extension View {
    func animate(duration: CGFloat, _ execute: **@escaping** () -> Void) async {
        await withCheckedContinuation { continuation in
            withAnimation(.easeInOut(duration: duration)) {
                execute()
            }

            DispatchQueue.main.asyncAfter(deadline: .now() + duration) {
                continuation.resume()
            }
        }
    }
}
```


#### 사용 예제

```swift
Task {
    // 1번 함수
    await animate(duration: 0.4) {
        for _ in 0...250 {
            self.offsetA += 1
        }
    }
    // 2번 함수
    await animate(duration: 0.6) {
        for _ in 0..<100 {
            self.offsetB += 0.01
        }
    }
}
```

위와 같이 사용하면 1번 함수의 애니메이션 동작이 완전히 끝난 다음 2번 함수를 호출할 수 있게 된다.

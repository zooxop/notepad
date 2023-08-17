
- 코드 출처
	- [Stackoverflow - SwfitUI List Make Scrolling disabled](https://stackoverflow.com/questions/58836261/swfitui-list-make-scrolling-disabled)

## in iOS 16 (or later)

**`.scrollDisabled(true)`** modifier를 `List` View에 추가해주면 간단하게 해결 가능.

```swift
struct ScrollView: View {
    var body: some View {
        List {
            
        }
        .scrollDisabled(true)
    }
}
```


## iOS 16 이전 버전에서도 사용 가능한 코드

```swift
struct ScrollView: View {
    var body: some View {
        List {
            
        }
        .simultaneousGesture(DragGesture(minimumDistance: 0), including: .all)
    }
}
```

> 참고 : https://stackoverflow.com/questions/65209314/what-does-the-underscore-mean-before-a-variable-in-swiftui-in-an-init


## 의문
PropertyWrapper로 선언된 변수에 인스턴스를 할당할 때, `_` 를 사용하는 이유는 뭘까?
단순히 포인터의 개념인걸까?

```swift
class RootViewModel: ObservableObject {
  // MARK: Dependencies
  @ObservedObject public var uiStateManager: UIStateManager
  
  init(uiStateManager: UIStateManager) {
    self._uiStateManager = ObservedObject(wrappedValue: uiStateManager)
  }
}
```

## 답변
stackoverflow 답변 글이랑 공식문서 참고해서 정리해보자.

> 밑줄이 그어진 변수 이름은 **바인딩 구조체의 기본 저장소**를 나타냅니다. 이는 **프로퍼티 래퍼라는 언어 기능의 일부**입니다.


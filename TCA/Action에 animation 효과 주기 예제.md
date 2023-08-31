
> (임시 저장) 글 정리중 ..

```swift
import ComposableArchitecture
import SwiftUI

struct SignInFeature: Reducer {
  struct State: Equatable {
    var logoOffset: Double = 0.0
    var buttonOpacity: Double = 0.0
  }
  
  enum Action {
    case changeLogoOffset
    case changeButtonOpacity
    case moveLogoUp
    case appearButton
  }
  
  func reduce(into state: inout State, action: Action) -> Effect<Action> {
    switch action {
    case .changeLogoOffset:
      return .run { send in
        try await Task.sleep(nanoseconds: 0_500_000_000)
        for _ in 0...250 {
          await send(.moveLogoUp)
        }
      }
      .animation(.easeInOut(duration: 1.5))
      
    case .changeButtonOpacity:
      return .run { send in
        try await Task.sleep(nanoseconds: 0_500_000_000)
        for _ in 0..<100 {
          await send(.appearButton)
        }
      }
      .animation(.easeInOut(duration: 1.0))
      
    case .moveLogoUp:
      state.logoOffset += 1
      return .none
      
    case .appearButton:
      state.buttonOpacity += 0.01
      return .none
      
    default:
      return .none
    }
  }
}
```


이렇게 하고, View 에서 `send(.changeLogoOffset)`을 호출하면 State 값이 Animation 효과화 함께 변경됨.
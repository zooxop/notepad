

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


`State`의 변수 값 수정은 `Reducer` 내부에서 하는것이 **기본적인 TCA의 동작 흐름**이다.

- 예시) User가 `View`의 Button을 tap하면, `State`의 변수 `value`의 값을 +1 시켜주는 기능을 작성하는 경우.
	- `View` 에서 `viewStore.send(.updateValue)` 호출.
	- `Reducer`는 `state.value += 1` 코드 실행.
	- 이 때, `Reducer`의 내부 실행 구문에서 `withAnimation`을 사용해서 값을 변경할 수는 있지만, 애니메이션 효과가 적용되지는 않는다. (왜인지는 아직 잘 모르겠음.)
	- 그래서, 해당Action에서 `.none`이 아닌 `.run { }` 을 반환하고, 동시에 `.animation()` 속성을 추가해주면 애니메이션 효과가 적용된다.
	- 그런데 한 가지 문제가 더 있다. `Reducer`가  `State`를 변수로 가져올 때, **`inout`** 타입으로 가져온다는 것. 따라서, `.run { }` closure 내부에서 변수 값을 직접 바꾸지 못한다는 것을 의미한다.
		- 바꾸려고 하면, `Mutable capture of 'inout' parameter 'state' is not allowed in concurrently-executing code` 에러가 발생한다.

위의 예시 코드처럼 **오직 변수값을 수정하는 동작만을 수행하는** `Action`을 별도로 구성하고, `.run{}` Action을 return하면 **Animation 효과와 함께 변수의 값을 수정**할 수 있다.

### 문제점
위의 예시대로 코드를 실행하면, Animation 효과는 잘 동작하는 것을 볼 수 있다. 하지만,
`.changeLogoOffset` Action이 return하는 Action 내부에는 `.moveLogoUp`을 250번 호출하는 코드가 포함되어 있다.
이는 **`Reducer`를 1.5초동안 250회 호출한다는 의미**이다. 

**너무 많은 Reducer 호출이 이루어지면 어떤 Side Effect가 생길지** 아직 자세하게는 모르겠다.

곧바로 체감할 수 있는 가장 불편한것은, Console에 남는 TCA 로그가 순식간에 250개가 쌓이게 되며 로그 보는데에 굉장히 방해가 된다는 것이다.



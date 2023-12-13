
> https://doordash.engineering/2022/05/31/how-the-swiftui-view-lifecycle-and-identity-work/

`@State`
_State_ is a source of truth for the view and it’s used when the scope of changes is limited to the view only. **By wrapping value types as transient** [State](https://developer.apple.com/documentation/swiftui/state) properties, **the framework allocates a persistent storage for this value type** and makes it a dependency, so changes to the state will automatically be reflected in the view.

State는 뷰에 대한 데이터 소스이며 변경 범위가 뷰로만 제한되는 경우에 사용됩니다. 프레임워크는 값 유형을 **임시 상태 속성으로 래핑**하여 **이 값 유형에 대한 영구 저장소를 할당**하고 종속성으로 지정하므로 상태에 대한 변경 사항이 뷰에 자동으로 반영됩니다.

> https://developer.apple.com/documentation/swiftui/state

SwiftUI manages the property’s storage. When the value changes, SwiftUI updates the parts of the view hierarchy that depend on the value.

**SwiftUI가 프로퍼티의 저장소를 관리**합니다. 값이 변경되면 SwiftUI가 값에 따라 달라지는 **뷰 계층 구조의 일부를 업데이트**합니다.


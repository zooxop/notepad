
> 참고 : https://forums.developer.apple.com/forums/thread/743399


```swift
#Preview {
  @ObservedObject var viewModel: RootView.ViewModel = .init()
  return RootView(viewModel: viewModel)
}
```
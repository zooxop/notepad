## Protocol(프로토콜) 이란

> Java의 `Interface` 와 유사한 개념으로, 특정 기능 구현에 ==**필요한 메서드와 프로퍼티를 정의만 해놓은 청사진**==을 말한다.

## 프로토콜의 사용

- **구조체, 클래스, 열거형**은 **프로토콜을 채택**해서 특정 기능을 실행하기 위한 프로토콜의 요구사항을 실제로 구현할 수 있다.
- 프로토콜은 **정의를 하고 제시를 할 뿐**, **스스로 기능을 구현하지는 않는다.**
- ==**하나의 타입으로 사용**==되기 때문에, 타입 사용이 허용되는 모든 곳에 프로토콜을 사용할 수 있다.
	- 함수, 메서드, 이니셜라이저의 매개변수, 상수, 변수 등


## Property(프로퍼티) 요구사항

- 프로토콜에 **Property(프로퍼티)** 를 명시할 때는 그 Property가 **Stored Property인지 Computed Property인지 ==명시하지 않는다.==**
- **이름과 타입, gettable, settable 여부**를 명시한다.
```swift
protocol Student {  
  var height: Double { get set }  
  var name: String { get }  
  static var schoolNumber: Int { get set }  
}
```

## Method(메서드) 요구사항

- 프로토콜에**instance method** 와 **type method(class, static)** 를 명시할 수 있다.
- 하지만, ==**매개변수에 기본값을 설정**할 수는 없다.==
- 프로토콜에는 ==**함수명과 반환 타입**==만 지정할 수 있고, `{ }`는 적지 않는다.
	- `mutating` 키워드를 사용해서 인스턴스에서 변경 가능하다는 것을 표시할 수 있다. (값 타입만 가능.) 

```swift
protocol Person {  
  static func breathing()  
  func sleeping(time: Int) -> Bool  
  mutating func running()  
}
```


###### 참고 아티클
- https://medium.com/@jgj455/%EC%98%A4%EB%8A%98%EC%9D%98-swift-%EC%83%81%EC%8B%9D-protocol-f18c82571dad
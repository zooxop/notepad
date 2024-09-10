
> 출처 : https://eunjin3786.tistory.com/233


## DIContainer class

```swift
class DIContainer {
    
    static let shared = DIContainer()
    
    private var dependencies = [String: Any]()
    
    private init() {}

    func register<T>(_ dependency: T) {
        let key = String(describing: type(of: T.self))
        dependencies[key] = dependency
    }
    
    func resolve<T>() -> T {
        let key = String(describing: type(of: T.self))
        let dependency = dependencies[key]
        
        precondition(dependency != nil, "\(key)는 register되지 않았어어요. resolve 부르기전에 register 해주세요")
        
        return dependency as! T
    }
}
```


## example

만약 모델이 다음과 같이 구성된 경우
```swift
protocol Eatable {
    var calorie: Int { get }
}

protocol CityPresentable {
    var code: String { get }
    var name: String { get }
}

struct Pizza: Eatable {
    var calorie: Int {
        return 300
    }
}

struct Seoul: CityPresentable {
    var code: String {
        return "02"
    }
    
    var name: String {
        return "서울"
    }
}

struct FoodTruck {
    let food: Eatable
    let city: CityPresentable
    
    init(food: Eatable, city: CityPresentable) {
        self.food = food
        self.city = city
    }
}
```

다음과 같이 register, resolve 해서 사용 가능.

```swift
class AppDelegate: UIResponder, UIApplicationDelegate {

    func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
        
        registerDependencies()
        
        return true
    }
    
    private func registerDependencies() {
        DIContainer.shared.register(Pizza())
        DIContainer.shared.register(Seoul())
        
        let pizza: Pizza = DIContainer.shared.resolve()
        let seoul: Seoul = DIContainer.shared.resolve()

        DIContainer.shared.register(FoodTruck(food: pizza,
                                              city: seoul))
    }
}

class ViewController: UIViewController {

    let foodTruck: FoodTruck = DIContainer.shared.resolve()
    
    override func viewDidLoad() {
        super.viewDidLoad()
        print(foodTruck)
    }
}
```


그리고 Swift 5.1에서 추가된 Property Wrapper를 만들어주면

```swift
@propertyWrapper
class Dependency<T> {
    
    let wrappedValue: T
    
    init() {
        self.wrappedValue = DIContainer.shared.resolve()
    }
}
```

더 간결해진다.

```swift
class ViewController: UIViewController {

    @Dependency var foodTruck: FoodTruck
    
    override func viewDidLoad() {
        super.viewDidLoad()
        print(foodTruck)
    }
}
```

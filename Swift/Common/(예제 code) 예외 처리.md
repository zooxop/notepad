
> 1부터 10 사이의 랜덤한 숫자를 생성한 뒤, 그 숫자를 토대로 `Error`를 `throw` 하도록 설계된 `do-catch` 예제 코드


```swift
import Foundation

/// Custom Error 타입
enum MyError: Error {
    case none
    case exceededFive
}

/// String 타입의 Custom Error
enum MyStringError: String, Error {
    case normal
    case error
}

/// 실행부 <-> 예외 발생 함수 간 bridge.
func makeError() throws {
    let number = try tryLessThanFive()
    
    print(number ?? "nil")
}

/// 1부터 10 사이의 랜덤한 숫자를 반환.
func getRandomNumber() -> Int {
    let randomNum = Int(arc4random_uniform(10)) + 1
    return randomNum
}

/// 1부터 10 사이의 랜덤한 숫자를 생성하여, 그 숫자가 6 이상인 경우 에러를 throw 한다.
func tryLessThanFive() throws -> Int? {
    let number = getRandomNumber()
    
    if number <= 5 {
        return number
    } else {
        throw MyError.exceededFive
    }
}

// MARK: 실행
do {
    try makeError()
} catch MyError.none {
    print("이와 같이 특정 에러를 핸들링할 수 있음.")
} catch let error as MyStringError {
    print("Error 타입 캐스팅을 통해, 특정 Error를 한 묶음으로 핸들링 할 수 있음. \(error)")
} catch {
    print("catch all block, error :", error)
}

```


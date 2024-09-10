
> https://phillip5094.tistory.com/216
> https://zeddios.tistory.com/1078

## `@testable` 이란?

Swift의 "접근제어자(Access Control)" 목록은 다음과 같다.
- **`open`, `public`** 
	- 프로젝트 내의 모든 `module` 해당 entity에 접근할 수 있습니다.
- **`internal`**
	- `default` 접근 제어자로, entity가 작성된 `module`에서만 접근할 수 있습니다.
- **`fileprivate`**
	- entity가 작성된 `source file`에서만 접근할 수 있도록 합니다. 서로 다른 클래스가 같은 파일안에 있고 `fileprivate`로 선언되어 있다면 둘은 서로 접근할 수 있습니다.
- **`private`**
	- 특정 객체에서만 사용할 수 있도록 하는 가장 제한적인 접근제어자입니다. `fileprivate`과 달리 같은 파일 안에 있어도 서로 다른 객체가 `private`로 선언되어 있다면 둘은 서로 접근할 수 없습니다.



앱 또는 프레임워크의 외부 모듈에서는 `internal` 레벨 이하로 설정되어 있는 속성에 접근할 수 없다.
그리고 Unit Test 모듈도 예외는 아니다.
따라서, Unit Test에서 앱의 `internal` 레벨 속성을 테스트하려면 <u>해당 속성을 `public` 또는 `open` 레벨로 변경을 해주어야 한다</u>는 말이 된다.
하지만 이러한 변경은 **Swift의 type safety 이점을 저해시키게** 된다.

그래서 이를 해결하기 위한것이 **`@testable import`** 이다.
Unit Test 코드에서 앱 모듈을 import 할 때 `@testable`  어노테이션을 붙여주면, 해당 모듈의 **`internal`** 영역까지 직접 접근할 수 있게된다.

> 참고 : 그래도 `fileprivate`, `private` 영역은 접근할 수 없음.

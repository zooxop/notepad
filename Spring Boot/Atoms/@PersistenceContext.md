> https://batory.tistory.com/497

## `@PersistenceContext` 란?
> `EntityManager` 를 빈으로 주입받을 때 사용하는 어노테이션이다.

`EntityManager`는 스프링에서 **영속성**을 관리할 때 사용하는 클래스이다. 스프링 컨테이너가 시작될 때 `EntityManager`를 만들어서 빈으로 등록해둔다.
이 때, **`@PersistenceContext`** 를 사용하면 스프링이 만들어둔 `EntityManager`를 주입받을 수 있다.

스프링이 다음 두가지 방법 중 한가지로 `@PersistenceContext`로 지정된 Property에 `EntityManager`를 주입해준다.
- `EntityManagerFactory`에서 새로운 `EntityManager`를 생성
- `Transaction`에 의해 기존에 생성된 `EntityManager`를 반환

## 사용하는 이유
> 동시성 문제에서 벗어나기 위해

같은 `EntityManager`에 여러 Thread가 동시에 **접근**하면 **동시성 문제가 발생**한다.
일반적으로 스프링의 빈은 **Singleton**으로 **모든 쓰레드가 공유**한다. 따라서 `Entitymanager`에 직접 접근하도록 코드를 작성하면 동시성 문제가 발생할 가능성이 높아진다.

그러나 `@PersistenceContext`로 `EntityManager`를 주입받으면 동시성 문제가 발생하지 않는다. 그 이유는 다음과 같다.
- 스프링 컨테이너가 초기화되면서 `@PersistenceContext`로 주입받은 `EntityManager`를 **Proxy로 감싼다.**
- 이 Proxy는 `EntityManager`가 호출될 때 마다 Thread Safe를 보장하면서 `EntityManager`를 반환하도록 설계되어 있다.

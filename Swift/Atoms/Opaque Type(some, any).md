
> 자료 출처 (설명이 굉장히 잘 되어 있음) : https://byeon.is/swift-5-7-opaque-type-some-any/

## Some 키워드란?

==**특정 프로토콜을 준수하는 것들을 담고 있는 불투명한 타입(Opaque Type)**==을 만들어내기 위해 사용된다.
==**Swift 5.1**== 에서 부터 소개되었음.


## Any키워드란?

특정 **프로토콜**을 **준수**하는한, 그 안에 ==**어떠한 콘크리트 타입이라도 저장할 수 있도록**== 해주는 역할을 해준다.
**==Swift 5.6==** 에서 소개되었음.

---

## Swift Book
> [Swift Programming Language - Opaque and Boxed Types](https://docs.swift.org/swift-book/documentation/the-swift-programming-language/opaquetypes)

### 번역

#### Opaque and Boxed Types (불투명 타입 및 박스 타입)
값의 유형에 대한 구현 **세부 정보**를 ==**숨깁니다.**==

Swift는 값의 유형에 대한 세부 정보를 숨기는 두 가지 방법, 즉 ==**불투명 유형**==과 ==**박스형 프로토콜 유형**==을 제공합니다. 유형 정보를 숨기는 것은 **반환 값의 기본 유형이 비공개로 유지될 수 있기 때문에**, **모듈과 모듈을 호출**하는 ==**코드 사이의 경계**==에서 유용합니다.

A function or method that returns an opaque type hides its return value's type information. Instead of providing a concrete type as function's return type, the return value is described in terms of the protocols it supports.

불투명 타입을 반환하는 함수(또는 메서드)는 반환값의 유형 정보를 숨깁니다. 함수의 반환 유형으로 구체적인 유형을 제공하는 대신, 반환 값은 지원하는 프로토콜의 관점에서 설명됩니다.
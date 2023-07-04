---
title: Swift closure와 Objective c block의 차이
tags: closure block swift objc capturelist
---

늘 헷갈리는 Swift closure와 Objective c block의 외부(closure 또는 block 밖) 변수 copy 방식을 정리해봤다.\
참조 타입의 변수 copy 방식은 reference copy로 동일하다. 그러나 값 타입의 변수 copy 방식은 두 언어의 기본 동작이 *반대로* 되어있다.

헷갈리니 코드를 통해 살펴보자.

## Swift closure의 값 타입 변수 copy 방식
#### 기본 동작: reference copy
```swift
var someInt: Int = 10

let closure: () -> Void = {
    print(someInt) // reference copy default
}

someInt = 30
closure() // print 30
```
Swift에서는 reference copy가 기본이므로 30이 출력된다.

#### capture list를 통한 value copy
```swift
var someInt: Int = 10

let closure: () -> Void = { [someInt] in
    print(someInt)
}

someInt = 30
closure() // print 10
```
`[someInt]`로 표현된 capture list를 쓰면 value copy가 수행되고, closure 직전의 값인 10이 출력된다.
여러 개의 capture list는 `[aVar, bVar, cVar, ...]`와 같이 사용할 수 있다.


## Objective C block의 값 타입 변수 copy 방식
#### 기본 동작: value copy
```objc
int someInt = 10;
void (^closure)(void) = ^{
    NSLog(@"%d", someInt);
};

someInt = 30;
closure(); // prints 10
```
objc는 value copy가 기본 동작으로, 캡쳐하는 block 직전의 값인 10이 출력된다.


#### \_\_block을 통한 reference copy
```objc
__block int someInt = 10;
void (^closure)(void) = ^{
    NSLog(@"%d", someInt);
};

someInt = 30;
closure(); // prints 30
```
reference copy를 하고 싶다면 변수 앞에 `__block`을 붙여주면 된다. 

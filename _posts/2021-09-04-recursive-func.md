---
title: Optimizing recursive function
tags: iOS, swift, optimization, compiler, recursive
---

재귀 함수의 최적화 방법을 알아봅니다.
{:.info}

## Recursive Function

컴퓨터 과학에서 재귀란 *자신을 정의할 때, 자기 자신을 재참조하는 방법*을 뜻합니다. 
- [위키백과: 재귀](https://ko.wikipedia.org/wiki/%EC%9E%AC%EA%B7%80_(%EC%BB%B4%ED%93%A8%ED%84%B0_%EA%B3%BC%ED%95%99)){:target="_blank"}
- [Wikepedia: Recursion](https://en.wikipedia.org/wiki/Recursion_(computer_science)){:target="_blank"}

## Dangers of Recursion

재귀 함수에는 크게 2가지 위험 요소가 있다고 합니다.
1. Infinite Loop
    - 재귀 함수의 반환 조건을 잘못 설정하는 경우, 무한 루프에 빠지게 됩니다.
2. Stack Overflow
    - stack은 heap에 비해 훨씬 적은 메모리 공간을 할당 받습니다. 따라서 재귀 호출이 많아질 수록 stack에 push되는 메모리들이 많아지면서 stack overflow가 발생할 수 있습니다.

stack overflow는 어떻게 해결할 수 있을까요?

## Tail Call Optimization (TCO)

꼬리 재귀 최적화(Tail Call Optimization, TCO)를 사용하면 stack overflow를 해결할 수 있습니다. 다만, 컴파일러가 TCO를 지원할 때 가능합니다.

먼저 일반적인 재귀 함수를 보겠습니다.

    func factorial<T: Numeric>(n: T) -> T {
        if n == 0 {
            return 1
        } else {
            return n * factorial(n: n-1)
        }
    }

여기서 stack이 쌓일 수 밖에 없는 부분은 바로 **return n * factorial(n: n-1)** 때문입니다. 오른쪽 피연산자인 factorial(n: n-1)가 return되기 전까지 n과의 곱하기 연산을 할 수 없기 때문에 이 함수는 stack에서 pop되지 못하고 stack은 쌓여만 갑니다..

이를 꼬리 재귀로 해결해 보겠습니다.

    func factorial<T: Numeric>(n: T) -> T {
        func factIter(product: T, n: T) -> T {
            guard n != 1 else { return product }
            return factIter(product: product*(n-1), n: n-1)
        }
        
        return factIter(product: n, n: n)
    }

여기서 **func factIter(product:,n:)**가 꼬리 재귀 함수입니다.

앞에서 살펴본 재귀 함수는 return이 n * fact(n:)이어서 즉각 종료되지 못했지만, 꼬리 재귀에서는 곱셈 값이 꼬리 재귀 함수의 파라미터(product)로 넘겨지면서 return이 factIter(product: product*(n-1), n: n-1)로 **완전한 하나의 함수로 대체**되었습니다. 따라서 새로운 함수를 호출하는 것과 같으므로, stack에 기존 함수를 유지할 필요없이 새로운 함수로 대체하면서 재귀 호출이 반복됩니다(컴파일러가 지원할 경우에 한함.).

Swift compiler는 TCO를 지원하며, 이를 위해서는 Build Setting에서 Optimization Level을 지정해 주어야 합니다. [Code Size Optimization](https://swift.org/blog/osize/){:target="_blank"}
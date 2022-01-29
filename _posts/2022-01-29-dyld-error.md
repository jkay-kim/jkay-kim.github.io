---
title: Resolving dyld error
tags: xcode dyld package library
---

## Dyld error

원소스에서 module화를 위한 작업을 하면서 Xcode에서 특정 layer에 해당하는 코드 뭉치들을 package로 분리하였고, 프로젝트에 분리한 package를 추가한 뒤 Simulator로 실행해보니 빌드는 성공했지만, 실행과 동시에 에러가 났다.

```
dyld[94985]: Library not loaded: @rpath/XCTest.framework/XCTest
``` 


이를 해결해 보고자 `Project > Target > Frameworks, Libraries, and Embedded Content`에 `XCTest.framework`을 추가한 후 실행해보니 이번에도 빌드는 성공했지만 이런 에러를 뱉어낸다.

```
dyld[15620]: Library not loaded: @rpath/libXCTestSwiftSupport.dylib
```

## Solution

구글링 해보니 [애플로부터 받은 답변](https://github.com/CocoaPods/CocoaPods/issues/9165#issuecomment-550273696){:target="_blank"}이 공유되어 있어 기록해둔다.

> After reviewing your feedback, we have some additional information for you.\
> Since Xcode 11, you should be linking against libXCTestSwiftSupport.dylib instead of the old libswiftXCTest.dylib which is obsolete. The set of symbols exported by the new library should be nearly identical.

Which means your "Other linker flags" should be -weak_framework XCTEST -weak-lXCTestSwiftSupport\
And you should add $(PLATFORM_DIR)/Developer/usr/lib to your "Library search path"

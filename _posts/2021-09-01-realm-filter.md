---
title: Comparing filters for Realm Results
tags: iOS, Swift, Realm, filter
---

Realm Results에 사용 가능한 Swift filter와 Realm filter를 비교해 봅니다.
{:.info}

## Function Definition

[Apple Developer Documentation: Swift filter](https://developer.apple.com/documentation/swift/sequence/3018365-filter){:target="_blank"}

    func filter(_ isIncluded: (Self.Element) throws -> Bool) rethrows -> [Self.Element]

[Realm Documentation: Realm filter](https://docs.mongodb.com/realm-legacy/docs/swift/latest/index.html#queries){:target="_blank"}

    func filter(_ predicate: NSPredicate) -> Results<Element>

Swift filter는 isIncluded closure가 true인 값을 필터링하는 반면, Realm filter는 predicate로 전달받은 **쿼리 구문**으로 필터링을 수행합니다.

## Filtering Realm Results

    // LazyFilterSequence<Results<T>>
    let swiftFiltered = realm.objects(Model.self).filter { $0.property == true }

    // Results<T>
    let realmFiltered = realm.objects(Model.self).filter("property = true")

- 공통점: realm.object(_:)의 return type인 Results\<T\>는 LazySequence이므로 2개 구문 모두 거의 즉각 반환됩니다.
- 차이점: LazySequence는 값에 접근할 때 비로소 filtering을 수행하는데, 이때 *성능* 차이가 발생합니다.
  - Swift filter: 값에 접근할 때 **O(n)의 time complexity**를 갖습니다.
  - Realm filter: property가 indexedProperty라면 **O(1)의 time complexity**를 갖습니다. (indexProperty가 아닌 경우 time complexity O(n))
- 고찰: property를 indexing하면 write에 overhead가 좀 더 발생하지만, filter 등의 탐색 수행 시 시간 복잡도 측면에서 더 나은 성능을 보여주므로, 잦은 탐색 조건에 해당하는 property는 indexing 후 NSPredicate를 사용하여 filter하는 것이 성능 측면에서 더 나은 선택으로 판단됩니다.
- 기타: 여기서는 Results가 LazySequence이므로 filter 또한 lazy하게 이루어지지만, 일반적인 Sequence(Array, Set, Dictionary 등)를 lazy하게 사용하고 싶으시다면 Sequence의 instance property인 lazy를 사용하시면 됩니다. 고차 함수(map, filter 등)를 수행할 때만 Lazy하게 실행됩니다. [Apple Developer Documentation: lazy](https://developer.apple.com/documentation/swift/sequence/1641562-lazy){:target="_blank"}
  - 예) let lazilyFiltered = array.lazy.filter { condition }

<!--more-->

---
title: Filters available on Realm Results
tags: iOS, Swift, Realm, filter
---

Realm Results에 Swift filter와 Realm filter를 사용할 때 어떤 차이가 있는지 알아봅니다.
{:.info}

## Function Definition

**Link:** [Swift filter](https://developer.apple.com/documentation/swift/sequence/3018365-filter)

    func filter(_ isIncluded: (Self.Element) throws -> Bool) rethrows -> [Self.Element]

**Link:** [Realm filter](https://docs.mongodb.com/realm-legacy/docs/swift/latest/index.html#queries)

    func filter(_ predicate: NSPredicate) -> Results<Element>

## Filtering Realm Results

    // swift filtered results
    let swiftFiltered = realm.objects(Model.self).filter { $0.property == true }
    // realm filtered results
    let realmFiltered = realm.objects(Model.self).filter("property = true")

<!--more-->

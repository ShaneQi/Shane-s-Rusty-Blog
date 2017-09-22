---
title: Diff between map and flatMap
permalink: map-and-flatmap-diff-16-09-15
date: 2016-09-15 19:43
---

Firstly, we have to know that there are two kinds of `flatMap`. It depends on the transformer function you pass in.

If transformation produces an optional value, then you would just receive an array of non-optional values.

```swift
let possibleNumbers = ["1", "2", "three", "///4///", "5"]

let mapped = possibleNumbers.map({ Int($0) })
//  [1, 2, nil, nil, 5]

let flatMapped = possibleNumbers.flatMap({ Int($0) })
//  [1, 2, 5]
```

if transformation produces a sequence or collection for each element, then you would receive a single-level collection.

```swift
let numbers = [1, 2, 3, 4]

let mapped = numbers.map { Array(repeating: $0, count: $0) }
// [[1], [2, 2], [3, 3, 3], [4, 4, 4, 4]]

let flatMapped = numbers.flatMap { Array(repeating: $0, count: $0) }
// [1, 2, 2, 3, 3, 3, 4, 4, 4, 4]
```

---
title: Adding Thousand Separator to Int in Swift
permalink: thousand-separator-int-16-05-12
date: 2016-05-12 15:16
---


Straight forward:

```swift
import Foundation

let fmt = NSNumberFormatter()
fmt.numberStyle = .DecimalStyle
fmt.stringFromNumber(2358000) 
// "2,358,000"
```

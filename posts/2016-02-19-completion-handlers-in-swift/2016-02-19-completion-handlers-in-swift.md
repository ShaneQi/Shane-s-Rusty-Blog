---
title: Completion Handlers in Swift
permalink: completion-handlers-in-swift-16-02-19
date: 2016-02-19 16:13
---


> Based on Xcode 7.2.1(7C1002)

If we want to do something only after a function is completed, we use completion handlers.


# Declare:

```swift
func foo(name :String, completion: ((result:String?) -> Void)) {
	// Do something. 
	completion (result: "Hello, \(name)!") 
}
```

# Use:

```swift
foo("Shane") {
	result in print("Got back: \(result)") 
}
```

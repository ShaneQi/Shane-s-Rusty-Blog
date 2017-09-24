---
title: Send and Receive Messages Through NSNotificationCenter
permalink: nsnotificationcenter-16-03-24
date: 2016-03-24 22:43
---

Some function in class A sends a notification named `ISentANotification`:

```swift
class A {
	func sendNotification() {
		NotificationCenter.default.post(name: Notification.Name(rawValue: "ISentANotification"), object: self)
	}
}
```

When class B receives the notification named `ISentANotification`, calls the function `IReceiveANotification`:

```swift
class B: UIViewController {

	override func viewWillAppear(_ animated: Bool) {
		super.viewWillAppear(animated);
		NotificationCenter.default.addObserver(self, selector: #selector(B.IReceiveANotification), name: Notification.Name(rawValue: "ISentANotification"), object: nil)
	}

	override func viewWillDisappear(_ animated: Bool) {
		NotificationCenter.default.removeObserver(self)
	}

	@objc func IReceiveANotification() {
		print("I received a notification!")
	}

}
```

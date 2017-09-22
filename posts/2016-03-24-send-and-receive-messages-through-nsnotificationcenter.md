---
title: Send and Receive Messages Through NSNotificationCenter
permalink: nsnotificationcenter-16-03-24
date: 2016-03-24 22:43
---


Some function in class A sends a notification named `ISentANotification`:

 class A { func sendNotification() { NSNotificationCenter.defaultCenter().postNotificationName("ISentANotification", object: self) } }

When class B receives the notification named `ISentANotification`, calls the function `IReceiveANotification`:

 class B: UIViewController { override func viewWillAppear(animated: Bool) { super.viewWillAppear(animated) NSNotificationCenter.defaultCenter().addObserver(self, selector: #selector(B.IReceiveANotification), name: "ISentANotification", object: nil) } override func viewWillDisappear(animated: Bool) { NSNotificationCenter.defaultCenter().removeObserver(self) } func IReceiveANotification() { print("I received a notification!") } }




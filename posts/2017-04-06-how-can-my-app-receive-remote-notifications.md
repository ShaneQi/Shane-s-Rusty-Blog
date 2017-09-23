---
title: How Can My App Receive Remote Notifications
permalink: how-to-receive-remote-notifications-17-04-06
date: 2017-04-06 06:36
---

## TL;DR
* App Capabilities
* Notification Payload
* User's Permission

## App Capabilities
Both `Push Notifications` and `Remote Notifications` need to be turned on in Xcode.

![img1](https://server.shaneqi.com/public/storage/Bpin0CetMzQ7.png)

## Notification Payload
The payload’s `aps` dictionary must include the `content-available` key with a value of `1`.

```
{
	"aps": {
		"content-available": 1,
		...
	},
	...
}
```

## User's Permission

The app has to acquire as least one of these four permissions: `alert`, `sound`, `badge`, `background app refresh`.

This can be check in app by: `UIApplication.shared.isRegisteredForRemoteNotifications` (iOS 8.0 and later).

![img2](https://server.shaneqi.com/public/storage/3ESMpyF3X5Mu.png)

---
title: Remember to Set `dateFormatter.locale` Properly For Fixed Format Date Formatting
permalink: remember-to-set-dateFormatter-locale-properly-for-fixed-format-date-formatting-19-09-18

date: 2019-09-18 13:25
---

> [The Documentation of NSDateFormatter](https://developer.apple.com/documentation/foundation/nsdateformatter)

```swift
let dateFormatter = DateFormatter()
dateFormatter.dateFormat = "MMM d"
let dateString = dateFormatter.string(from: Date())
print(dateString)
```

What output do you expect from the above code? I personally expected `Sep 18` as output, because `MMM` refers to The shorthand name of the month, and `d` refers to The day of the month. But it's not always the case.

The output depends on the locale of the device that the code is run on. For example, if the device's locale is "ja_JP", the output will be `9月 18`, and if the device's locale is "fr_FR", the output will be "sept. 18".

It's straight forward, locale plays an important role in date formatting even though you provide the date format.

__When we need to pay extra attention to is when we want to format dates to a fixed format.__

For example, my app needs to send a time to the server and the server requires the time to be in 12 hour format ("05:32 PM"). The proper way to do this is:

```swift
let dateFormatter = DateFormatter()
dateFormatter.dateFormat = "hh:mm a"
dateFormatter.locale = Locale(identifier: "en_US_POSIX") // IMPORTANT!
// You might also want to set timezone.
let stringToSendToServer = dateFormatter.string(from: Date())
```

The line to set `dateFormatter.locale` is very important, because without it, the devices' locales will pay roles in the date formatting process which will lead to different formats strings on different devices. For example, if the the device's locale uses 12-hour format, the string would be `"17:48"`, but what the server wants is `"05:48 PM"`.

So do remember to set `dateFormatter.locale` properly for fixed format date formatting.


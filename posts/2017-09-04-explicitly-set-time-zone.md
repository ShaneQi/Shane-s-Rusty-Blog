---
title: Explicitly Set Time Zone Before Parsing Date from String in Swift
permalink: explicitly-set-time-zone-17-09-04
date: 2017-09-04 12:00
---

> `Date` represents a single point in time. A `Date` is independent of a particular calendar or time zone.

Let's say the API returned a date string to me: "2017-09-04 14:00:00". __And the API document tells me that this date string is in UTC.__

Now I'm going to parse a Date from the string and store the date in my app's local database:

```swift
// CAUTION: this is a bad practice. 
let dateFormatter = DateFormatter()
dateFormatter.dateFormat = "yyyy-MM-dd HH:mm:ss"
let date = dateFormatter.date(from: "2017-09-04 14:00:00")
```

This snippet of code is a bad practice in most cases, because the date string describes the date in UTC time zone, but the code parse date from it with the system time zone. (DateFormatter has system time zone as the default time zone)

This bad practice turns into a bug when I format this date into string again:

```swift
// `date` is from code above.
let dateFormatter = DateFormatter()
dateFormatter.dateFormat = "yyyy-MM-dd HH:mm:ss"
let dateString = dateFormatter.string(from: date)
// dateString = "2017-09-04 14:00:00", but I'm expecting 
// "2017-09-04 09:00:00" becuase I'm in -0500 time zone.
```

The formatting code doesn't have any problem, it uses a date formatter with system time zone to format a date. But the result is wrong because the date was parsed improperly at the first place.

The solution is quite simple, explicitly set time zone before parsing date from string. The time zone should be the same as the one in which the date string describes date. Such that the date object I parsed stores the correct point in time.

```swift
// SHOULD DO
let dateFormatter = DateFormatter()
// Because API doc told me the date string is in UTC.
dateFormatter.timeZone = TimeZone(secondsFromGMT: 0)
dateFormatter.dateFormat = "yyyy-MM-dd HH:mm:ss"
let date = dateFormatter.date(from: "2017-09-04 14:00:00")
// date: Date? = 2017-09-04 14:00:00 UTC, as I expected.
```

Then when I want to present the date to user, I just use whichever time zone I need to format the date:

```swift
// `date` is from code above.
let dateFormatter = DateFormatter()
dateFormatter.dateFormat = "yyyy-MM-dd HH:mm:ss"
// Not necessary to explicitly set time zone, because it has system time zone as default.
// dateFormatter.timeZone = TimeZone(secondsFromGMT: -18000)
let dateString = dateFormatter.string(from: date)
// dateString = "2017-09-04 09:00:00", as I expected.
```

> Edited on March 20th, 2019

If the date string that the server gives has timezone info, e.g. `"2017-09-04 14:00:00+0000"` (`"+0000"` indicates UTC), then we don't need to set timezone on the date formatter. We just need to make sure the date formatter is reading the timezone info by including timezone format in date format.

```swift
let dateString = "2017-09-04 14:00:00+0000"
let dateFormatter = DateFormatter()
dateFormatter.dateFormat = "yyyy-MM-dd HH:mm:ssxx" // the "xx" is timezone format
// Not necessary to explicitly set time zone, because it's reading timezon info.
// dateFormatter.timeZone = TimeZone(secondsFromGMT: 0)
let date = dateFormatter.date(from: dateString)
// date: Date? = 2017-09-04 14:00:00 UTC, as I expected.
```

### Takeaways

1. A timedate can't be defined without the timezone.
2. If the date string doesn't have a timezone in it, find it out and set it on the date formatter.
3. If the date string has a timezone in it, make sure it's read by the date formatter.
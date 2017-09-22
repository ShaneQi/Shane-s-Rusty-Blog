---
title: Get Interval Between Two Dates with Javascript
permalink: date-interval-javascript-16-01-17
date: 2016-01-17 05:14
---

Above all, there are no functions that can calculate the interval between two dates directly. So we need to do the math. It’s math, simply.

Firstly, convert two dates to milliseconds:  

```js
var date1 = new Date();
var date2 = new Date(2016, 10, 6); // Date(2016, 10, 6) means Nov. 6, 2016
var data1_ms = date1.getTime();
var data2_ms = date2.getTime();
```

Then, calculate the milliseconds interval between the two dates, which is just a minus operation:  
`var interval_ms = date2_ms - date1_ms;`

Finally, convert the internval back to days in which we should divide the milliseconds by one day’s milliseconds:  

```js
var one_day = 1000 * 60 * 60 * 24;
return Math.ceil(interval_ms/one_day); // Math.ceil() makes the difference rational
```
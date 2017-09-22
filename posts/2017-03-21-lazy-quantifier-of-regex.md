---
title: Lazy Quantifier of Regex
permalink: regex-lazy-quantifier-17-03-21
date: 2017-03-21 21:28
---

This is a snippet of text:

```html
<a href="https://google.com">hello</a><a href="https://google.com">hello</a>
```

When using this regex `<a href=.*>.*<\/a>`, the match result is only one, from the beginning all the way to the end. Even though there is a `</a>` in the middle of the text, regex just greedily takes them all together.

When using this regex `<a href=.*?>.*?<\/a>`, the match results are two like what I want. The magic is the question mark `?`.

The question mark makes this regex lazy, which means it will take as few as possible.

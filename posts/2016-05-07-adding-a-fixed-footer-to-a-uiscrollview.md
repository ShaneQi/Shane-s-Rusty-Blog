---
title: Adding a Fixed Footer to a UIScrollView
permalink: fixed-footer-in-uiscrollview-16-05-07
date: 2016-05-07 23:58
---


The approach I’m gonna introduce is just simply updating the fixed footer’s frame, making sure the fixed view is located where it should be, whenever the `UIScrollView` is scrolled.

The trick is overriding `layoutSubviews()` of `UIScrollView`:

```swift
... 
/* Our fixed footer is declared here. */ 
var footerBar: UIView! 
... 
/* This method is called when subviews have to be layouted. */ 
override func layoutSubviews() {
    super.layoutSubviews() 
    /* Where the fixed view should be. */ 
    let location = CGPointMake(0, contentOffset.y - footerBar.frame.height + self.frame.height) 
    /* Update footerBar's location. */
    footerBar.frame.origin = location 
} 
...
```



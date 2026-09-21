---
title: Get Rid of Touching Delay Caused by UITapGestureRecognizer
permalink: touching-delay-caused-by-uitapgesturerecognizer-16-05-07
date: 2016-05-07 00:01
---


I’ve been fucked by a touching delay for several days.

By touching delay, I mean: I have a `UICollectionView` subviewing to some views, and when I selected a cell of the `UICollectionView`, it didn’t go to the `didSelectItemAtIndexPath` delegate method. After hundreds of clicking cells, I’ve noticed that only if I long press a cell, it goes to `didSelectItemAtIndexPath`. I asserted that my select-touching was delayed by something.

It turned out that it is a `UITapGestureRecognizer` that causes the delay. The `UITapGestureRecognizer` was assigned to the `UICollectionView`’s some superview, in order to dismiss keyboard when user touch anywhere within the screen. So when a touch comes, the `UITapGestureRecognizer` would block the touch temporarily until it decides whether this touch should be handled by it(`UITapGestureRecognizer`). Only after the touch is released by `UITapGestureRecognizer`, it goes to delegate. Deciding takes 1 sec estimated, which leads to a delay.

How I get rid of this touching delay is add a single UIGestureRecognizerDelegate method:

```swift
/* This delegate decides whether the gesture recognizer would see this touch. */ 
func gestureRecognizer(gestureRecognizer: UIGestureRecognizer, shouldReceiveTouch touch: UITouch) -> Bool { 
/* Get the view that the touch landed on. */ 
  if let touchView = touch.view {
    /* If the landed view is a cell of my UICollectionView, forbid the UIGestureRecognizer receive this touch. */ 
    if (touchView.frame.size.height + touchView.frame.size.width) == 130 {
        return false 
      } 
  } 
  /* Otherwise let UIGestureRecognizer handle the touch. */ 
  return true 
}
```

Remember assign delegate to your `UITapGestureRecognizer`, and this might also works for getting rid of touching delay caused by all kinds of `UIGestreRecognizer` (Tap, Pan, Pinch, Swipe, etc).




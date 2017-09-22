---
title: Autolayout scrollView Properly
permalink: autolayout-scrollview-properly-16-09-12
date: 2016-09-12 18:45
---

When I have to use a scrollView, I prefer put another scrollView inside the main view of viewController, even the scrollView is the only view in the main view of viewController. And I strongly suggests others use it this way other than make main view of viewController a scrollView. This approach is scalable.

What I was trying to solve is that when I show this scrollView viewController from a navigationController, the navigation bar overlaps some content of my scrollView.

I figured out there are two ways to do such things:

- Set pushed viewController's`Adjust Scroll View Insets` property to `false` and set scrollView's Y constraints as these two:

```
    scrollView.top = TopLayoutGuide.bottom
    scrollView.bottom = BottomLayoutGuide.top
```

- Set pushed viewController's`Adjust Scroll View Insets` property to `true` and set scrollView's Y constraints as these two:

```
    scrollView.top = superview.top
    scrollView.bottom = BottomLayoutGuide.top
```

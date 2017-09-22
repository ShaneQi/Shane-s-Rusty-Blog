---
title: Using Autolayout on UIScrollView
permalink: autolayout-on-uiscrollview-16-06-14
date: 2016-06-14 16:00
---


UIScrollView has two key sizes I have to point out:

1. `frame.size`: this is a property of UIView, which defines the UIScrollView’s box outline size.
2. `contentSize`: when content size is bigger then frame size, the UIScrollView is scrollable.

Programmatically, you can set the two key sizes making a UIScrollView work well. However, if you want to use Autolayout on a UIScrollView, it’d be a little tricky.

For `frame.size`, it’s just as usual as any other UIView, just give enough constrains to the UIScrollView.

For `contentSize`, there must have a UIView (or UIView’s sub view) as a child of the UIScrollView, and the UIView must have enough constrains to explicitly tell the UIScrollView what contentSize it should be. Which means the UIView must have these 6 constrains (missing any one will cause a “content size ambiguous” warning):

1. Leading space to the UIScrollView
2. Trailing space to the UIScrollView
3. Top space to the UIScrollView
4. Bottom space to the UIScrollView
5. X position relative to the UIScrollView
6. Y position relative to the UIScrollView

However, just because these 6 constrains must exist doesn’t mean they must have the highest “required(1000)” priority.

For example, in one of my cases, my content grows in the vertical axis to the bottom, so that I set the Y position and bottom space constrains as “high(750)” priority. If I don’t do so, there might be constrains conflict in runtime and the UIScrollView might perform abnormally.




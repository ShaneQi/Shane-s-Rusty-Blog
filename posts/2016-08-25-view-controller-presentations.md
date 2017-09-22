---
title: View Controller Presentations
permalink: view-controller-presentations-16-08-25
date: 2016-08-25 01:43
---


There are four kinds of segue in interface builder:

### **Show (Push):**

This type of segue just presents the new content modally over the source view controller. Some view controllers specifically override the method and use it to implement different behaviors. For example, a navigation controller pushes the new view controller onto its navigation stack.

**use:**  
1. Set `modalPresentationStyle` and `modalTransitionStyle` property.
1. Call `func showViewController(_ vc: UIViewController, sender sender: AnyObject?)`.

### **Show Detail (Replace):**
This segue is relevant only for view controllers embedded inside a UISplitViewController object. With this segue, a split view controller replaces its second child view controller (the detail controller) with the new content. Most other view controllers present the new content modally.   
**(difference from Show(push): no override in case of navigation controller)**

**use:**  
1. Set `modalPresentationStyle` and `modalTransitionStyle` property.
1. Call `func showDetailViewController(_ vc: UIViewController, sender sender: AnyObject?)`.

### **Present Modally:**

This segue displays the view controller modally using the specified presentation and transition styles. The view controller that defines the appropriate presentation context handles the actual presentation.

**use:**  
1. Set `modalPresentationStyle` and `modalTransitionStyle` property.
1. Call `func presentViewController(_ viewControllerToPresent: UIViewController, animated flag: Bool, completion completion: (() -> Void)?)`.

### **Present as Popover:**

In a horizontally regular environment, the view controller appears in a popover. In a horizontally compact environment, the view controller is displayed using a full-screen modal presentation.

**use:**  
[Presenting a View Controller in a Popover](https://developer.apple.com/library/ios/featuredarticles/ViewControllerPGforiPhoneOS/PresentingaViewController.html#//apple_ref/doc/uid/TP40007457-CH14-SW13)

- - - - - -

Reference:

16. [Presenting a View Controller](https://developer.apple.com/library/ios/featuredarticles/ViewControllerPGforiPhoneOS/PresentingaViewController.html#//apple_ref/doc/uid/TP40007457-CH14-SW1)
17. [Using Segues](https://developer.apple.com/library/ios/featuredarticles/ViewControllerPGforiPhoneOS/UsingSegues.html#//apple_ref/doc/uid/TP40007457-CH15-SW1)



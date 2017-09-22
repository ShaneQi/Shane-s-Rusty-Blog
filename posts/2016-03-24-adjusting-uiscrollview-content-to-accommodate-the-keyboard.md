---
title: Adjusting UIScrollView Content to Accommodate the Keyboard
permalink: accommodating-uiscrollview-to-keyboard-16-03-24
date: 2016-03-24 22:30
---


override func viewWillAppear(animated: Bool) { super.viewWillAppear(animated) /* Add keyboard notification observer. */ NSNotificationCenter.defaultCenter().addObserver(self, selector: #selector(ItemDetailsViewController.keyboardWillShow), name: UIKeyboardWillShowNotification, object: nil) NSNotificationCenter.defaultCenter().addObserver(self, selector: #selector(ItemDetailsViewController.keyboardWillHide), name: UIKeyboardWillHideNotification, object: nil) } override func viewWillDisappear(animated: Bool) { /* Remove keyboard notification observer. */ NSNotificationCenter.defaultCenter().removeObserver(self) } func keyboardWillShow(notification: NSNotification) { /* Get keyboard height. */ let userInfo:NSDictionary! = notification.userInfo let keyboardHeight = userInfo.objectForKey(UIKeyboardFrameBeginUserInfoKey)!.CGRectValue().size.height /* Add bottom inset. */ scrollView.contentInset.bottom = keyboardHeight scrollView.scrollIndicatorInsets.bottom = keyboardHeight /* If text field is hidden by keyboard, scroll it so it's visible */ var viewport = scrollView.frame viewport.size.height -= keyboardHeight let textFieldOrigin = CGPoint(x: bidForm.frame.origin.x + bidForm.textFieldPosition.x, y: bidForm.frame.origin.y + bidForm.textFieldPosition.y) if CGRectContainsPoint(viewport, textFieldOrigin) { let scrollTo = CGPointMake(0, textFieldOrigin.y + keyboardHeight) scrollView.setContentOffset(scrollTo, animated: true) } } func keyboardWillHide() { /* Reset bottom inset. */ scrollView.contentInset.bottom = 0 scrollView.scrollIndicatorInsets.bottom = 0 }




---
title: Things about UIWebView
permalink: things-about-uiwebview-16-02-11
date: 2016-02-11 17:11
---

> Based on Xcode 7.2.1(7C1002)

1. Unsafe url request may be blocked. In order to load that kind of requests, add key `NSAppTransportSecurity` into `Info.plist` and append a sub-key ` NSAllowsArbitraryLoads` into it, then set it to `true`.
2. UIWebView Delegate (4 in total, all optional):  // When the UIWebView is supposed to start load a request(referred by 'request'), this delegate function decide if the UIWebView should load the request. optional func webView(_ webView: UIWebView, shouldStartLoadWithRequest request: NSURLRequest, navigationType navigationType: UIWebViewNavigationType) -> Bool // When the UIWebView starts loading a frame, it would call this function. // One page may have many frames, which means loading a full page, this function may be called for many times. optional func webViewDidStartLoad(_ webView: UIWebView) // When the UIWebView finishes loading a frame, it would call this function. // Also, this function may be called for many times loading a single page. optional func webViewDidFinishLoad(_ webView: UIWebView) // Sent if a web view failed to load a frame. optional func webView(_ webView: UIWebView, didFailLoadWithError error: NSError?)
3. Get if the page is fully loaded:  // This function will return a Bool value which indicates whether the page is fully loaded. // Actually it uses javascript. webView.stringByEvaluatingJavaScriptFromString("document.readyState")
4. Connect a UIProgressView with a UIWebView:  // Declare parameters in ViewController. // Add this progress view via Interface Builder (IBOutlet) or programmatically. let myProgressView: UIProgressView var theBool: Bool = false var myTimer: NSTimer = NSTimer()

 // Declare functions in ViewController and call them when they are supposed to be call. func funcToCallWhenStartLoadingYourWebview() { self.myProgressView.hidden = false self.myProgressView.progress = 0.0 self.theBool = false // 0.01667(1/60) means the UIProgressView will be 60FPS. self.myTimer = NSTimer.scheduledTimerWithTimeInterval(0.01667, target: self, selector: "timerCallback", userInfo: nil, repeats: true) } func funcToCallCalledWhenUIWebViewFinishesLoading() { self.theBool = true } func timerCallback() { if self.theBool { if self.myProgressView.progress >= 1 { self.myProgressView.hidden = true self.myTimer.invalidate() } else { self.myProgressView.progress += 0.1 } } else { self.myProgressView.progress += 0.05 if self.myProgressView.progress >= 0.95 { self.myProgressView.progress = 0.95 } } }




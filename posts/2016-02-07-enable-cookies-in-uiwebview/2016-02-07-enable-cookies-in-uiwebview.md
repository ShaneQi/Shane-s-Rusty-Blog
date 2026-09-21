---
title: Enable Cookies in UIWebView
permalink: enable-cookies-in-uiwebview-16-02-07
date: 2016-02-07 22:52
---


> Based on Xcode 7.2.1(7C1002)

UIWebView does store the cookies, but it only store the cookies temporarily by default.

Storting the cookies temporarily means when you kill the app and relaunch it, the cookies are gone.

Well, we can manually save the cookies when the app enters background and terminates, and load the cookies when the app enters foreground and launches.

Firstly, declare save and load functions in `AppDelegate.swift`:

func loadHTTPCookies() { let cookieDict : NSMutableArray? = NSUserDefaults.standardUserDefaults().valueForKey("cookieArray") as? NSMutableArray if cookieDict != nil { for c in cookieDict! { let cookies = NSUserDefaults.standardUserDefaults().valueForKey(c as! String) as! NSDictionary let cookie = NSHTTPCookie(properties: cookies as! [String : AnyObject]) NSHTTPCookieStorage.sharedHTTPCookieStorage().setCookie(cookie!) } } } func saveHTTPCookies() { let cookieArray = NSMutableArray() let savedC = NSHTTPCookieStorage.sharedHTTPCookieStorage().cookies for c : NSHTTPCookie in savedC! { let cookieProps = NSMutableDictionary() cookieArray.addObject(c.name) cookieProps.setValue(c.name, forKey: NSHTTPCookieName) cookieProps.setValue(c.value, forKey: NSHTTPCookieValue) cookieProps.setValue(c.domain, forKey: NSHTTPCookieDomain) cookieProps.setValue(c.path, forKey: NSHTTPCookiePath) cookieProps.setValue(c.version, forKey: NSHTTPCookieVersion) cookieProps.setValue(NSDate().dateByAddingTimeInterval(2629743), forKey: NSHTTPCookieExpires) NSUserDefaults.standardUserDefaults().setValue(cookieProps, forKey: c.name) NSUserDefaults.standardUserDefaults().synchronize() } NSUserDefaults.standardUserDefaults().setValue(cookieArray, forKey: "cookieArray") }

Then call the save function in:

- `func applicationDidEnterBackground(application: UIApplication)`
- `func applicationWillTerminate(application: UIApplication)`

Call the load function in:

- `func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject: AnyObject]?)`
- `func applicationWillEnterForeground(application: UIApplication)`

That’s it.




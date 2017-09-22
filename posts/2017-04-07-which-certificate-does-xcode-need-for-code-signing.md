---
title: Which Certificate does Xcode Need for Code-signing?
permalink: which-certificate-need-for-code-signing-17-04-07
date: 2017-04-07 02:42
---


You have to do pretty much two steps before you have the `ipa` file: *archive* then *export*. There are code-signing actions in both steps.

### Archive

You create an archive of your app regardless of the type of distribution method you select. Code-signing happens roughly after building. 
Which certificate is needed here really depends on your project config: archive needs the type of  certificate you specify in `building settings` with respect to the `build configuration` specified in your schema.
So yes, if you specify `Don't Code Sign` in building settings, archive won't do code-signing.

![](/content/images/2017/04/Untitled.png)

### Export 

What kind of certificate is needed when exporting from archive to `ipa` is straight-forward: depending on what kind of export you are doing.


![](/content/images/2017/04/Screen-Shot-2017-04-06-at-9.39.13-PM.png)

---
title: Make Xcode Auto Increment the Build Number
permalink: xcode-auto-increment-build-number-16-09-22
date: 2016-09-22 17:28
---

```bash
#!/bin/bash
buildNumber=$(/usr/libexec/PlistBuddy -c "Print CFBundleVersion" "$INFOPLIST_FILE")
buildNumber=$(($buildNumber + 1))
/usr/libexec/PlistBuddy -c "Set :CFBundleVersion $buildNumber" "$INFOPLIST_FILE"
```

![](/content/images/2016/09/gzOsd.png)

Don't forget to change the build number found under General in the Identity section to Integer.

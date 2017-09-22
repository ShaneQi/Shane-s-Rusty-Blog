---
title: Add a Permanent Alias to OS X
permalink: permanent-alias-os-x-16-01-18
date: 2016-01-18 02:10
---

If I want to map from command `ll` to `ls -laxo`, for example.

For temporary alias, this would work: `$ alias ll='ls -laxo'`

To add a permanent alias, we should create `.bash_profile` file in the user’s home directory if it doesn't exist, and then add the alias announce to the file:  

```bash
$ echo "alias ll='ls -laxo' >> ~/.bash_profile"
```

Don’t forget to quit Terminal and reopen it.

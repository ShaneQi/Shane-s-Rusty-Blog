---
title: Why does Gym Fail at `[CP] Embed Pods Frameworks`
permalink: why-does-gym-fail-at-cp-embed-pods-frameworks-19-03-07
date: 2019-03-07 20:48
---

If you are using [Match](https://docs.fastlane.tools/actions/match/) and [Gym](https://docs.fastlane.tools/actions/gym/) on a CI/CD machine, and Gym failed at `[CP] Embed Pods Frameworks`, you might have run into the the issue that this blog is going to talk about.

#### TL;DR

1. If [Match](https://docs.fastlane.tools/actions/match/) has already installed the certs and private keys, delete them from the keychain. ([Match](https://docs.fastlane.tools/actions/match/)'s magic won't happen if certs and private keys already exist)
2. Make sure you give [Match](https://docs.fastlane.tools/actions/match/) the keychain password. (by the parameter or the environment variable)
3. Run [Match](https://docs.fastlane.tools/actions/match/) again. If you see logs like 'Setting key partition list...', the problem is likely solved.

##### Why did [Gym](https://docs.fastlane.tools/actions/gym/) fail?

[Match](https://docs.fastlane.tools/actions/match/) downloads and installs certs w/ private keys and provision profiles for you. Certs and keys are stored in the keychain and protected by the keychain.

At that moment, [Gym](https://docs.fastlane.tools/actions/gym/) invoked `codesign` to sign the pod frameworks. `codesign` needed the cert and private key, but macOS didn't want to give out the cert and the key without your consent.

So if you were doing this on your local machine, you would see that macOS prompts for password to ask your consent to give out the cert and the key:

![prompt-password-access-key](https://server.shaneqi.com/public/storage/0NMNEEg1Of1n.png)

But if it was the CI/CD machine that's running [Gym](https://docs.fastlane.tools/actions/gym/), it can't prompt an UI permission popup. Nor does `codesign` ask for the password in command line. So it failed.

#### How to prevent the UI permission popup?

Before Sierra, we can give `codesign` the permission to access this key when importing the key:

```bash
security import {{certPath}} -k {{keychainPath}} -P {{certPass}} -T /usr/bin/codesign -T /usr/bin/security
```

Starting with Sierra (as of Mojave 10.14.3), the `-T` won't work. You need to use this command, after importing the cert and before running `codesign`:

```bash
security set-key-partition-list -S apple-tool:,apple: -k {{keychainPass}} {{keychainName}}
```

More details: [security / codesign in Sierra: Keychain ignores access control settings and UI-prompts for permission
](https://stackoverflow.com/questions/39868578/security-codesign-in-sierra-keychain-ignores-access-control-settings-and-ui-p)

#### Isn't [Match](https://docs.fastlane.tools/actions/match/) supposed to take care of this?

The answer is yes. [Match](https://docs.fastlane.tools/actions/match/) takes care of it:

![keychain_importe](https://server.shaneqi.com/public/storage/YC3TQHlDGilb.png)

#### Why did [Gym](https://docs.fastlane.tools/actions/gym/) still fail if [Match](https://docs.fastlane.tools/actions/match/) handled it?

It's probably because the machine has Sierra or newer OS, and you need to use `set-key-partition`), but [Match](https://docs.fastlane.tools/actions/match/) failed to set partition list.

![keychain_importer_1](https://server.shaneqi.com/public/storage/7cyiPaDSKRyb.png)

Looking at the [Fastlane](https://docs.fastlane.tools)'s source code, only if the cert importing succeeds will [Match](https://docs.fastlane.tools/actions/match/) set partition list. 

1. So one of the possible reasons that [Match](https://docs.fastlane.tools/actions/match/) won't set partition list is that the cert importing failed because the cert already exists.
1. **In my case, [Match](https://docs.fastlane.tools/actions/match/) didn't set partition list for me because the keychain was locked and I didn't give keychain password to [Match](https://docs.fastlane.tools/actions/match/).** After I gave [Match](https://docs.fastlane.tools/actions/match/) the keychain password (by setting env var `MATCH_KEYCHAIN_PASSWORD="123abc"`), I also needed to delete the imported certs and private keys first, then run [Match](https://docs.fastlane.tools/actions/match/) again to set partition list. (if I didn't delete the certs and private keys, [Match](https://docs.fastlane.tools/actions/match/) wasn't going to set partition list for me because of the reason 1).
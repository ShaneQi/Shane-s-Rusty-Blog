---
title: Certificates and Provisioning Profiles Management with Fastlane Match
permalink: certificates-and-provisioning-profiles-management-with-fastlane-match-20-01-27
date: 2020-01-27 15:28
---

> This is from the documentation I wrote when I was working for Axxess.

## Tool

__Fastlane Match__

repo: \<REDACTED\>

Passphrase: \<REDACTED\>

## Certificates Managers (as of Jan. 2020)

\<REDACTED\>

\<REDACTED\>

## Day to Day Usage Examples

##### Running an app on a real device

`fastlane match development --readonly` then enter the bundle id of the app when the prompt shows (don't forget the bundle id of any extensions the app has, e.g. share extension).

##### Uploading a build to App Store Connect

`fastlane match appstore --readonly` then enter the bundle id of the app when the prompt shows (don't forget the bundle id of any extensions the app has, e.g. share extension).

##### Fails to download certificates or provisioning profiles

Contact one of the certificates managers to check:

1. If the certs or profiles exist in the repo.
1. If don't exist, ask one of the certificates managers to create for you.
1. If exist, check if certificate has expired and if the profiles are valid.

##### Running an app on new devices

Ask the one of the certificates managers to update the provisioning profiles to include the new devices (documented below), then:

`fastlane match development --readonly` then enter the bundle id of the app when the prompt shows (don't forget the bundle id of any extensions the app has, e.g. share extension), to download the updated provisioning profile.

##### (Certificates Managers Only) Updating provisioning profiles to include new devices

`fastlane match development --force_for_new_devices` then enter the bundle id of the app when the prompt shows (don't forget the bundle id of any extensions the app has, e.g. share extension).

## Parameter Explanation

##### `development`/`appstore`/`adhoc`

Indicating the provisioning profile type.

`development` is mainly for running an app on a real device from Xcode (running on Simulators doesn't require anything) and archiving an app (you can use development cert & profile to archive, but will need adhoc/appstore (aka production) cert & profile to export/upload).

`appstore` (aka production) is for exporting/uploading an app to the App Store Connect.

`adhoc` is for exporting an app to .ipa file (the most common use case of adhoc is distributing testing build via Crashlytics Beta).

##### `--readonly`

Indicating the operation only downloads certificates or provisioning profiles from the repo.

If this parameter is used, Apple ID won't be asked, because it doesn't need it to update/create/remove certificates and provisioning profiles.

If you forgot to add this parameter and entered your Apple ID, the operation is probably going to fail because your account doesn't have the permission of the changes (except certificates managers).

## (Certificates Managers Only) What to do When Certificates Expire

1. Nuke the expired certificates and all the provisioning profiles that are associated with the expired certificates.

    - `fastlane match nuke development` for cleaning development cert & profiles.
    - `fastlane match nuke distribution` for cleaning appstore (aka production) AND adhoc cert & profiles.

4. Make sure the correct Xcode command line tool is selected.

    - If any app needs to be built with Xcode 10.x or lower, a Xcode 10.x or lower command line tool should be selected.
    - Because if use Xcode 11 or newer command line tool, the certificate it creates won't be compatible with Xcode 10.x or lower.
    - use `xcode-select -s <PATH_TO_XCODE>` to switch to the correct Xcode command line tool. (e.g. `xcode-select -s "/Applications/Xcode_10.3.app"`)

1. Create new certificates with `fastlane match development/appstore` and push ENTER directly when prompted for bundle id (because you only want the certificate, don't need to create any provisioning profiles).
1. Create new provisioning profile for the team as needed.

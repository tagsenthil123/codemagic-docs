---
description: Find out how to overcome frequent issues with building your Flutter app on Codemagic. 
title: Common issues
weight: 1
---

## iOS code signing troubleshooting

This is the list of the most common issues that may cause iOS code signing errors during a CI build.

* **The uploaded certificate is in a wrong format or corrupt.** Codemagic looks for a certificate in Personal Information Exchange (`.p12`) format. See [how to export the certificate](../code-signing/ios-code-signing/#exporting-signing-certificate-and-provisioning-profile).

* **The uploaded certificate and provisioning profile do not match.** For example, you're using a development certificate and a distribution profile to sign the build, or the certificate used for signing is not included in the provisioning profile.

* **You don’t have the required entitlements enabled for your app in Apple Developer portal.** In such cases, you will often see an error message similar to this one:

        Code Signing Error: “Runner” requires a provisioning profile with the Push Notifications feature. Select a provisioning profile in the Signing & Capabilities editor.

    Check your app’s entitlements by going to **Apple Developer portal > Certificates, identifier & profiles > Identifiers > App ID**.

* **You haven't specified the iOS scheme to be used for the `archive` action of Xcode build.**  This applies when your app has custom iOS schemes. By default, Codemagic builds the `Runner` scheme, but you can use the `FCI_FLUTTER_SCHEME` [environment variable](..\building\environment-variables) to specify another scheme.

* **The bundle ID you have entered in automatic code signing setup on Codemagic does not match the bundle ID in the build configuration that is used for archiving the app with Xcode.** Codemagic assigns provisioning profiles to the build targets and configurations before building the iOS app. That assignment is based on the bundle ID match in both provisioning profile and the build configuration. In the case signing configuration is not assigned to the build target/configuration that is used for archiving, the build will fail.

## iOS build hangs at `Xcode build done`

**Description**:
When building for iOS, the build gets stuck after showing `Xcode build done` in the log but does not finish and eventually times out.

**Log output**: 

    == Building for iOS ==

    == /usr/local/bin/flutter build ios --release --no-codesign ==
    Warning: Building for device with codesigning disabled. You will have to manually codesign before deploying to device.
    Building net.butterflyapp.trainer for device (ios-release)...
    Running pod install...                                              3.7s
    Running Xcode build...                                          
    Xcode build done.                                           203.6s

**Flutter**: `1.7.8+hotfix.3`, `1.7.8+hotfix.4`, `1.9.1+hotfix.2`, `1.9.1+hotfix.4`

**Xcode**: N/A

**Solution**: This is a known issue that occurs randomly and can be traced back to Flutter:
https://github.com/flutter/flutter/issues/28415
https://github.com/flutter/flutter/issues/35988

This issue is known to be fixed on the `master` channel.
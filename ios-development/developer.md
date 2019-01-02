# Developer

## Certificates, Identifiers & Profiles

### Certificates

To use your certificates, you must have the intermediate signing certificate in your system keychain. This is automatically installed by Xcode: [Worldwide Developer Relations Certificate Authority](https://developer.apple.com/certificationauthority/AppleWWDRCA.cer).

To manually generate a Certificate, you need a Certificate Signing Request \(CSR\) file from your Mac. When your CSR file is created, a public and private key pair is automatically generated. Your private key is stored on your computer. On a Mac, it is stored in the login Keychain by default and can be viewed in the Keychain Access app under the "Keys" category. Your requested certificate is the public half of your key pair. See: [Developer Account Help](https://help.apple.com/developer-account/).

### Profiles

Provisioning profiles allow you to install apps onto your iOS devices. A provisioning profile includes signing certificates, device identifiers, and an App ID. Development provisioning profiles are used to build and install versions of your app during the development cycle, while distribution provisioning profiles are used to submit your apps to the App Store and distribute them to beta testers.

## Symbolicating Crash Reports

[Symbolication](https://developer.apple.com/library/archive/technotes/tn2151/_index.html) is the process of resolving backtrace addresses to source code method or function names, known as symbols. As the compiler translates your source code into machine code, it also generates debug symbols which map each machine instruction in the compiled binary back to the line of source code from which it originated.

By default, debug builds of an application store the debug symbols inside the compiled binary while release builds of an application store the debug symbols in a companion **dSYM** file to reduce the binary size. The Debug Symbol file and application binary are tied together on a per-build-basis by the build UUID.

Your users can retrieve crash reports from their device and send them to you via email by following [these instructions](https://developer.apple.com/library/archive/qa/qa1747/_index.html). If you have distributed your application via AdHoc or Enterprise distribution, this is the **only** way to acquire crash reports from your users.

If the user has opted to share diagnostic data with Apple, or if the user has installed a beta version of your application through TestFlight, the crash report is uploaded to the App Store. The symbolicated crash reports are made available to you in Xcode's Crashes organizer.


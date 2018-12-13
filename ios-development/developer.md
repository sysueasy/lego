# Developer

## Certificates, Identifiers & Profiles

### Certificates

To use your certificates, you must have the intermediate signing certificate in your system keychain. This is automatically installed by Xcode: [Worldwide Developer Relations Certificate Authority](https://developer.apple.com/certificationauthority/AppleWWDRCA.cer).

To manually generate a Certificate, you need a Certificate Signing Request \(CSR\) file from your Mac. When your CSR file is created, a public and private key pair is automatically generated. Your private key is stored on your computer. On a Mac, it is stored in the login Keychain by default and can be viewed in the Keychain Access app under the "Keys" category. Your requested certificate is the public half of your key pair. See: [Developer Account Help](https://help.apple.com/developer-account/).

### Profiles

Provisioning profiles allow you to install apps onto your iOS devices. A provisioning profile includes signing certificates, device identifiers, and an App ID. Development provisioning profiles are used to build and install versions of your app during the development cycle, while distribution provisioning profiles are used to submit your apps to the App Store and distribute them to beta testers.


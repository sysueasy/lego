# Firebase

As of April 10, 2018, Google has deprecated GCM. [Firebase Cloud Messaging](https://firebase.google.com/docs/cloud-messaging/) \(FCM\) inherits the reliable and scalable GCM infrastructure, plus many new features. FCM is a **cross-platform** messaging lets you reliably deliver messages at no cost to your Android, iOS, or Web app.

## iOS Client

The FCM SDK performs **method swizzling** in two key areas: _mapping your APNs token_ to the FCM registration token and _capturing analytics data_ during downstream message callback handling.

Generally, you'll need to [upload your APNs authentication key](https://firebase.google.com/docs/cloud-messaging/ios/client#upload_your_apns_authentication_key) and [register for remote notifications](https://firebase.google.com/docs/cloud-messaging/ios/client#register_for_remote_notifications).

By default, the FCM SDK generates a registration token for the client app instance on app launch. Similar to the APNs device token, this token allows you to send targeted notifications to any particular instance of your app. If you have disabled method swizzling, you'll need to explicitly map your APNs token to the FCM registration token.


# Firebase

As of April 10, 2018, Google has deprecated GCM. [Firebase Cloud Messaging](https://firebase.google.com/docs/cloud-messaging/) \(FCM\) inherits the reliable and scalable GCM infrastructure, plus many new features. FCM is a **cross-platform** messaging lets you reliably deliver messages at no cost to your Android, iOS, or Web app.

## iOS Client

The FCM SDK performs **method swizzling** in two key areas: _mapping your APNs token to the FCM registration token_ and _capturing analytics data_ during downstream message callback handling.

If you **disable** method swizzling, you have to implement the following methods yourself:

```swift
// mapping your APNs token to the FCM registration token
func application(application: UIApplication,
                 didRegisterForRemoteNotificationsWithDeviceToken deviceToken: NSData) {
    Messaging.messaging().apnsToken = deviceToken
    // ... other codes
}
// Receive displayed notifications for iOS 10 devices.
func userNotificationCenter(_ center: UNUserNotificationCenter,
                              willPresent notification: UNNotification,
    withCompletionHandler completionHandler: @escaping (UNNotificationPresentationOptions) -> Void) {
    // With swizzling disabled you must let Messaging know about the message, for Analytics
    Messaging.messaging().appDidReceiveMessage(userInfo)
    // ... other codes
}
```

Generally, you'll need to [upload your APNs authentication key](https://firebase.google.com/docs/cloud-messaging/ios/client#upload_your_apns_authentication_key) and [register for remote notifications](https://firebase.google.com/docs/cloud-messaging/ios/client#register_for_remote_notifications).

By default, the FCM SDK generates a registration token for the client app instance on app launch. Similar to the APNs device token, this token allows you to send targeted notifications to any particular instance of your app. If you have disabled method swizzling, you'll need to explicitly map your APNs token to the FCM registration token.


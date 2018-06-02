# Support Universal Links

Internet Standard: The Internet Engineering Task Force \(**IETF**\) is the premier Internet standards body, developing open standards through open processes. Memos in the [**RFC**](https://www.ietf.org/standards/rfcs/)\(Request for Comments\) document series contain technical and organizational notes about the Internet.

The term "Uniform Resource Locator" \(**URL**\) refers to the subset of **URI** \(Uniform Resource Identifier\) that, in addition to identifying a resource, provide a means of locating the resource by describing its primary access mechanism \(e.g., its network "location"\).

## Support Universal Links

When you support **universal links**, iOS users can tap a link to your website and get seamlessly redirected to your installed app without going through Safari. If your app isn’t installed, tapping a link to your website opens your website in Safari.

Universal links let users open your app when they tap links to your website within WKWebView and UIWebView views and Safari pages, in addition to links that result in a call to openURL:, such as those that occur in Mail, Messages, and other apps.

After you specify your associated domains, adopt the [`UIApplicationDelegate`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate) methods for Handoff \(specifically [`application:continueUserActivity:restorationHandler:`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/1623072-application)\) so that your app can receive a link and handle it appropriately.

For detail informations, see [Support Universal Links](https://developer.apple.com/library/content/documentation/General/Conceptual/AppSearch/UniversalLinks.html#//apple_ref/doc/uid/TP40016308-CH12-SW1).

## Using URL Schemes to Communicate with Apps

A URL scheme lets you communicate with other apps through a protocol that you define. 

* To communicate with an app that implements such a scheme, you must create an appropriately formatted URL and ask the system to open it. 
* To implement support for a custom scheme, you must declare support for the scheme and handle incoming URLs that use the scheme. 

To register a URL type for your app, include the _CFBundleURLTypes_ key in your app’s Info.plist file. The CFBundleURLTypes key contains an array of dictionaries, each of which defines CFBundleURLName and CFBundleURLSchemes.

```text
facetime://user@example.com
```

For information about the built-in schemes supported by apple, see [_Apple URL Scheme Reference_](https://developer.apple.com/library/content/featuredarticles/iPhoneURLScheme_Reference/Introduction/Introduction.html#//apple_ref/doc/uid/TP40007899).

All URLs are passed to your **app delegate**, the system calls the delegate’s `application:openURL:sourceApplication:annotation:`to check the URL and open it.

For detail information, see [Handling URL Requests](https://developer.apple.com/library/content/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/Inter-AppCommunication/Inter-AppCommunication.html#//apple_ref/doc/uid/TP40007072-CH6-SW1).


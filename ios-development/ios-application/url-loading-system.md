# URL Loading System

## **Overview**

The URL Loading System provides access to resources identified by URLs, using standard protocols like https or custom protocols you create. Loading is performed **asynchronously**, so your app can remain responsive and handle incoming data or errors as they arrive.

The _completion_ _handler_ is called on a different Grand Central Dispatch queue than the one that created the task. Therefore, any work that uses data or error to update the UI should be explicitly placed on the main queue, as shown here.

```swift
func startLoad() {
    let url = URL(string: "https://www.example.com/")!
    let task = URLSession.shared.dataTask(with: url) { 
        data, response, error in
        if let error = error {
            self.handleClientError(error)
            return
        }
        guard let httpResponse = response as? HTTPURLResponse,
            (200...299).contains(httpResponse.statusCode) else {
            self.handleServerError(response)
            return
        }
        if let mimeType = httpResponse.mimeType, mimeType == "text/html",
            let data = data,
            let string = String(data: data, encoding: .utf8) {
            DispatchQueue.main.async {
                self.webView.loadHTMLString(string, baseURL: url)
            }
        }
    }
    //Tasks are created in a suspended state, 
    //and can be started by calling resume().
    task.resume()
}
```

1. First Step

1.1  You use a [`URLSession`](https://developer.apple.com/documentation/foundation/urlsession) instance to create one or more [`URLSessionTask`](https://developer.apple.com/documentation/foundation/urlsessiontask) instances, which can _fetch and return data to your app, download files, or upload data and files_ to remote locations. To configure a session, you use a [`URLSessionConfiguration`](https://developer.apple.com/documentation/foundation/urlsessionconfiguration) object, which controls behavior like how to use caches and cookies, or whether to allow connections on a cellular network.

2. Request and Response

2.1 [`URLRequest`](https://developer.apple.com/documentation/foundation/urlrequest) encapsulates two basic data elements of a load request: the URL to load, and the [**policy**](https://developer.apple.com/documentation/foundation/nsurlrequest/cachepolicy) to use when consulting the URL content cache made available by the implementation.

2.2 The related [`HTTPURLResponse`](https://developer.apple.com/documentation/foundation/httpurlresponse) class is a commonly used subclass of `URLResponse` whose objects represent a response to an HTTP URL load request and store additional protocol-specific information such as the response headers. Whenever you make an HTTP request, the `URLResponse` object you get back is actually an instance of the [`HTTPURLResponse`](https://developer.apple.com/documentation/foundation/httpurlresponse) class.

3. Cache Behaviour

The [`URLCache`](https://developer.apple.com/documentation/foundation/urlcache) class is used for caching responses from network resources. Your app can directly access the shared cache instance by using the shared property of `URLCache`. Or, you can create your own caches for different purposes, setting distinct caches on your [`URLSessionConfiguration`](https://developer.apple.com/documentation/foundation/urlsessionconfiguration) objects.

4. Authentication and Credentials

When your app makes a request with a [`URLSessionTask`](https://developer.apple.com/documentation/foundation/urlsessiontask), the server may respond with one or more demands for credentials before continuing. The session task attempts to handle this for you. If it can’t, it calls your session’s delegate to handle the challenges. If you don’t implement a delegate, your request may be denied by the server, and you receive a response with HTTP status code 401 \(Forbidden\) instead of the data you expect.

## Reference

* Ref: [URL Loading System](https://developer.apple.com/documentation/foundation/url_loading_system)
* Ref: [Working with JSON in Swift](https://developer.apple.com/swift/blog/?id=37)
* Ref: [Alamofire](https://github.com/Alamofire/Alamofire) is an HTTP networking library written in Swift.


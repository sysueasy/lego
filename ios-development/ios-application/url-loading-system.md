# URL Loading System

Ref: [https://developer.apple.com/documentation/foundation/url\_loading\_system](https://developer.apple.com/documentation/foundation/url_loading_system)

The URL Loading System provides access to resources identified by URLs, using standard protocols like https or custom protocols you create. Loading is performed **asynchronously**, so your app can remain responsive and handle incoming data or errors as they arrive.

The completion handler is called on a different Grand Central Dispatch queue than the one that created the task. Therefore, any work that uses data or error to update the UI should be explicitly placed on the main queue, as shown here.

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

You use a [`URLSession`](https://developer.apple.com/documentation/foundation/urlsession) instance to create one or more [`URLSessionTask`](https://developer.apple.com/documentation/foundation/urlsessiontask) instances, which can fetch and return data to your app, download files, or upload data and files to remote locations. To configure a session, you use a [`URLSessionConfiguration`](https://developer.apple.com/documentation/foundation/urlsessionconfiguration) object, which controls behavior like how to use caches and cookies, or whether to allow connections on a cellular network.

You can use one session repeatedly to create tasks. For example, a web browser might have separate sessions for regular and private browsing use.

Each session is associated with a **delegate** to receive periodic updates \(or errors\). The default delegate calls a completion handler block that you provide; if you choose to provide your own custom delegate, this block is not called.

You can configure a session to run in the **background**, so that while the app is suspended, the system can download data on its behalf and wake up the app to deliver the results.








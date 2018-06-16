# URL Loading System

Ref: [https://developer.apple.com/documentation/foundation/url\_loading\_system](https://developer.apple.com/documentation/foundation/url_loading_system)

```swift
func startLoad() {
    let url = URL(string: "https://www.example.com/")!
    let task = URLSession.shared.dataTask(with: url) { data, response, error in
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
    task.resume() //Tasks are created in a suspended state, and can be started by calling resume().
}
```

**Task**: fetch and return data to your app, download files, or upload data and files to remote locations.

You can use one **session** repeatedly to create tasks. For example, a web browser might have separate sessions for regular and private browsing use.

You can configure a session to run in the **background**, so that while the app is suspended, the system can download data on its behalf and wake up the app to deliver the results.

{% hint style="info" %}
The **completion** handler is called on a different Grand Central Dispatch queue than the one that created the task. Therefore, any work that uses data or error to update the **UI** should be explicitly placed on the main queue, as shown here.
{% endhint %}






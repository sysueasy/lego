# Frameworks

## Foundation

The [Foundation](https://developer.apple.com/documentation/foundation) framework provides a base layer of functionality for apps and frameworks, including:

* text processing, date and time calculations, sorting and filtering, The [Swift Standard Library](https://developer.apple.com/documentation/swift/swift_standard_library) provides many of the same types available in the Foundation framework.
* data storage and persistence
* networking, e.g. [URL Loading System](url-loading-system.md).
* XPC, Object Runtime, [Process and Threads](operation.md) \(e.g. RunLoop,  Operation\), 

The classes, protocols, and data types defined by Foundation are used throughout the macOS, iOS, watchOS, and tvOS SDKs.

## Core Animation

[Core Animation](https://developer.apple.com/documentation/quartzcore) provides high frame rates and smooth animations without burdening the CPU and slowing down your app. Most of the work required to draw each frame of an animation is done for you.

Layers are often used to provide the backing store for views but can also be used without a view to display content. 

```swift
let layer = self.view.layer
```

A layer’s main job is to manage the visual content that you provide but the layer itself has visual attributes that can be set, such as a background color, border, and shadow. In addition to managing visual content, the layer also maintains information about the geometry of its content \(such as its position, size, and transform\) that is used to present that content onscreen. Modifying the properties of the layer is how you initiate animations on the layer’s content or geometry.

If the layer object was created by a view, the view typically assigns itself as the layer’s **delegate** automatically, and you should not change that relationship.

## Reference

* Design: [Human Interface Guidelines](https://developer.apple.com/design/human-interface-guidelines/)
* Ref: [https://gist.github.com/bwhiteley/049e4bede49e71a6d2e2](https://gist.github.com/bwhiteley/049e4bede49e71a6d2e2)




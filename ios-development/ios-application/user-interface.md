# User Interface

## UIViewController

Each view controller manages a view hierarchy. The size and position of the root view is determined by the object that owns it, which is either a parent view controller or the app’s window. The view controller that is owned by the window is the app’s root view controller and its view is sized to fill the window.

View controllers load their views **lazily**. Accessing the view property for the first time loads or creates the view controller’s views. There are several ways to specify the views for a view controller:

* Specify the view controller and its views in your app’s [Storyboard](https://developer.apple.com/library/archive/documentation/General/Conceptual/Devpedia-CocoaApp/Storyboard.html#//apple_ref/doc/uid/TP40009071-CH99). \(preferred\)
* Specify the views for a view controller using a [Nib file](https://developer.apple.com/library/archive/documentation/General/Conceptual/DevPedia-CocoaCore/NibFile.html#//apple_ref/doc/uid/TP40008195-CH34).
* Specify the views for a view controller using the [`loadView()`](https://developer.apple.com/documentation/uikit/uiviewcontroller/1621454-loadview) method. In that method, create your view hierarchy programmatically and assign the root view of that hierarchy to the view controller’s [`view`](https://developer.apple.com/documentation/uikit/uiviewcontroller/1621460-view) property.

When using a storyboard to define your view controller and its associated views, you never initialize your view controller class directly. Instead, view controllers are instantiated by the storyboard either automatically when a segue is triggered or programmatically when your app calls the `instantiateViewController(withIdentifier:)` method. When instantiating a view controller from a storyboard, iOS initializes the new view controller by calling its `init(coder:)` method and sets the nibName property to a nib file stored inside the storyboard.

### Rotate

As of iOS 8, **all rotation-related methods are deprecated**. Instead, **rotations** are treated as a change in the size of the view controller’s view and are therefore reported using the `viewWillTransition(to:with:)` method. When the interface orientation changes, UIKit calls this method on the window’s root view controller. That view controller then notifies its child view controllers, propagating the message throughout the view controller hierarchy. The `viewWillLayoutSubviews()` method is also called after the view is resized and positioned by its parent.

The system intersects the view controller's supported orientations with the **app's** supported orientations \(as determined by the Info.plist file or the app delegate's `application(_:supportedInterfaceOrientationsFor:)` method\) and the **device's** supported orientations to determine whether to rotate. For example, the `.portraitUpsideDown` orientation is not supported on iPhone X.

When the user changes the device orientation, the system calls `supportedInterfaceOrientations` on the **root** view controller or the **topmost presented** view controller that fills the window. If the view controller supports the new orientation, the window and view controller are rotated to the new orientation. This method is only called if the view controller's `shouldAutorotate` method returns true.

You can override the `preferredInterfaceOrientationForPresentation` for a view controller that is intended to be presented full screen in a specific orientation.

### Container View Controller

A custom `UIViewController` subclass can also act as a **container** view controller. A container view controller manages the presentation of content of other view controllers it owns, also known as its **child** view controllers. Your container view controller must associate a child view controller with itself before adding the child's root view to the view hierarchy. This allows iOS to properly route events to child view controllers and the views those controllers manage. 

### **State Preservation and Restoration**

The property [`restorationIdentifier`](https://developer.apple.com/documentation/uikit/uiviewcontroller/1621499-restorationidentifier) indicates whether the view controller and its contents should be preserved and is used to identify the view controller during the restoration process. Assigning a string object to the property lets the system know that the view controller should be saved. In addition, the contents of the string are your way to identify the purpose of the view controller.

During subsequent launches, UIKit asks your app for help in recreating the view controllers that were installed the last time your app ran. When it asks for a specific view controller, UIKit provides your app with this restoration identifier and the restoration identifiers of any parent view controllers in the view controller hierarchy. Your app must use this information to create or locate the appropriate view controller object.

## UIView

A view object renders content within its bounds rectangle and handles any interactions with that content. A view is a subclass of UIResponder and can respond to touches and other types of events.

Views can adjust the size and position of their subviews. Use Auto Layout to define the rules for resizing and repositioning your views in response to changes in the view hierarchy.

By default, when a subview’s visible area extends outside of the bounds of its superview, no clipping of the subview's content occurs. Use the `clipsToBounds` property to change that behavior.

The geometry of each view is defined by its **frame** and **bounds** properties. The frame property defines the origin and dimensions of the view in the **coordinate** system of its superview. The bounds property defines the internal dimensions of the view as it sees them and _is used almost exclusively in custom drawing code_.

### Animation

Changes to several view properties can be animated: frame, bounds, center, transform, alpha, backgroundColor. To animate your changes, create a [`UIViewPropertyAnimator`](https://developer.apple.com/documentation/uikit/uiviewpropertyanimator) object and use its handler block to change the values of your view's properties. The animation support provided by _Core Animation_ is fast and easy to use, makes visible changes to a view without requiring you to subclass and implement complex drawing code. 

### Subclassing

Although there are many good reasons to **subclass** `UIView`, it is recommended that you do so only when the basic `UIView` class or the standard system views do not provide the capabilities that you need. Subclassing requires more work on your part to implement the view and to tune its performance.

Image-based backgrounds - consider using a `UIImageView` object with gesture recognizers instead of subclassing and drawing the image yourself. Alternatively, you can also use a generic `UIView` object and assign your image as the content of the view’s `CALayer` object.

Rather than draw your content using a `draw(_:)` method, embed image and label subviews with the content you want to present.

### Resizing

If you want a given view to size itself to its parent view, you should add it to the parent view before calling `sizeToFit()`.

### LayoutSubviews

You should not call `layoutSubviews()` directly. If you want to force a layout update, call the `setNeedsLayout()` method instead to do so. This method makes a note of the request and returns immediately. Because this method does not force an immediate update, but instead waits for the next update cycle, you can use it to **invalidate** the layout of multiple views before any of those views are updated. This behavior allows you to consolidate all of your layout updates to one update cycle, which is usually better for performance.

 If you want to update the layout of your views immediately, call the `layoutIfNeeded()` method. When using Auto Layout, the layout engine updates the position of views as needed to satisfy changes in constraints. Using the view that receives the message as the root view, this method lays out the view subtree starting at the root. If no layout updates are pending, this method exits without modifying the layout or calling any layout-related callbacks.

### The View Drawing Cycle

The default implementation of `draw(_:)` does nothing. Subclasses that use technologies such as Core Graphics and UIKit to draw their view’s content should override this method and implement their drawing code there. You do not need to override this method if your view sets its content in other ways. For example, you do not need to override this method if your view just displays a background color or if your view sets its content directly using the underlying layer object.

UIKit creates and configures a graphics context for drawing and adjusts the transform of that context so that its origin matches the origin of your view’s bounds rectangle. You can get a reference to the graphics context using the [`UIGraphicsGetCurrentContext()`](https://developer.apple.com/documentation/uikit/1623918-uigraphicsgetcurrentcontext) function, but do not establish a strong reference to the graphics context because it can change between calls to the `draw(_:)` method.

This method is called when a view is first displayed or when an event occurs that invalidates a visible part of the view. You should never call this method directly yourself. To invalidate part of your view, and thus cause that portion to be redrawn, call the [`setNeedsDisplay()`](https://developer.apple.com/documentation/uikit/uiview/1622437-setneedsdisplay) instead. These methods let the system know that it should update the view during the next drawing cycle.

## CALayer

**Core Animation** provides high frame rates and smooth animations without burdening the CPU and slowing down your app. Most of the work required to draw each frame of an animation is done for you.

Layers are often used to provide the backing store for views but can also be used without a view to display content. 

```swift
let layer = self.view.layer // the view's core animation layer used for rendering
```

A layer’s main job is to manage the visual content that you provide but the layer itself has visual attributes that can be set, such as a background color, border, and shadow. In addition to managing visual content, the layer also maintains information about the geometry of its content \(such as its position, size, and transform\) that is used to present that content onscreen. Modifying the properties of the layer is how you initiate animations on the layer’s content or geometry.

If the layer object was created by a view, the view typically assigns itself as the layer’s **delegate** automatically, and you should not change that relationship.

## UIBlurEffect

```swift
let blurEffect = UIBlurEffect(style: UIBlurEffectStyle.dark)
let blurEffectView = UIVisualEffectView(effect: blurEffect)
blurEffectView.frame = self.bounds
blurEffectView.autoresizingMask = [.flexibleWidth, .flexibleHeight]
self.addSubview(blurEffectView)
```

## Reference

* Design: [Human Interface Guidelines](https://developer.apple.com/design/human-interface-guidelines/)
* Ref: [https://gist.github.com/bwhiteley/049e4bede49e71a6d2e2](https://gist.github.com/bwhiteley/049e4bede49e71a6d2e2)




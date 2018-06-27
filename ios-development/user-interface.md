# User Interface

[https://developer.apple.com/design/human-interface-guidelines/ios/overview/themes/](https://developer.apple.com/design/human-interface-guidelines/ios/overview/themes/)

UILabel: Use "Minimum font size" OR "numberOfLines = 0"

UIView:

```swift
textView.sizeToFit()
textView.layoutIfNeeded()
```

## Scrolling Vertically

First, add a new View Controller. In the **Size Inspector** replace Fixed with **Freeform** for the Simulated Size, and enter a width of 340 and a height of 800. You’ll notice the layout of the controller gets narrower and longer, simulating the behavior of a long vertical content.

Add a Scroll View. Add **top, bottom, leading, trailing** with constants of 0.

Add a View as a child of the Scroll View, and rename its storyboard Label to Container View. Add top, leading, trailing constraints and height constraint to 1000 \(temporary\).

Add child views into ContainerView, add a constraint between the last child view and the ContainerView's bottom.

Fix the auto layout error and finish.


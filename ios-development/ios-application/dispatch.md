# Dispatch

## Overview

Execute code **concurrently** on **multicore** hardware by submitting work to **dispatch queues** managed by the system. **GCD** \(**Grand Central Dispatch**\), operating at the system level, can better accommodate the needs of all running applications, matching them to the available system resources in a balanced fashion.

GCD provides and manages FIFO queues to which your application can submit tasks in the form of block objects. Work submitted to dispatch queues are executed on a **pool of threads** fully managed by the system. _No guarantee_ is made as to the thread on which a task executes.

A dispatch queue can be either **serial**, so that work items are executed _one at a time_, or it can be **concurrent**, so that work items are _dequeued in order,_ but _run all at once and can finish in any order_. Both serial and concurrent queues process work items in first in, first-out \(FIFO\) order.

Each work item can be executed either **synchronously** or **asynchronously**. When a work item is executed synchronously with the sync method, the program waits until execution finishes before the method call returns. When a work item is executed asynchronously with the async method, the method call **returns** immediately.

When an app launches, the system automatically creates a special queue called the **main** _queue_. Work items enqueued to the main queue execute serially on your app’s **main thread**. Attempting to **synchronously** execute a work item on the main queue results in **deadlock**.

```swift
let queue = DispatchQueue.main
```

In addition to the serial main queue, the system also creates a number of **global concurrent dispatch queues**. You can access the global concurrent queue that best matches a specified quality of service:

```swift
let queue = DispatchQueue.global(qos: .default)
```

You can also create **custom queue** if you will.

## DispatchQueue

To create a queue is as simple as:

```swift
let queue = DispatchQueue(label: "com.yianzhou.whatsnew")
```

A string **label** to attach to the queue to _uniquely_ identify it in debugging tools such as _Instruments_, _sample_, _stackshots_, and _crash reports_. Because applications, libraries, and frameworks can all create their own dispatch queues, a _reverse-DNS naming style_ \(com.example.myqueue\) is recommended.

One can create a queue with explicit QoS:

```swift
let queue = DispatchQueue(label: "com.yianzhou.myqueue1", qos: .background)
```

You create a **concurrent** queue by:

```swift
let queue = DispatchQueue(label: "com.yianzhou.whatsnew", attributes: .concurrent)
```

Any pending blocks submitted to a queue **hold a reference** to that queue, so the queue is not deallocated until all pending blocks have completed.

Submits a block to a dispatch queue for synchronous execution and waits until that block completes:

```swift
queue.sync {
    // do sth
}
```

This function does not return until the block has finished. Calling this function and targeting the current queue results in **deadlock**. As an optimization, this function invokes the block on the current thread when possible.

Unlike with _async_, no `retain` is performed on the target queue. Because calls to this function are synchronous, it "borrows" the reference of the caller. Moreover, no `copy` is performed on the block.

## DispatchWorkItem

[`DispatchWorkItem`](https://developer.apple.com/documentation/dispatch/dispatchworkitem) encapsulates work that can be performed. Work items allow you to configure properties of individual work units, for the purposes of waiting for their completion, getting notified about their completion, and/or canceling them. A work item can be dispatched onto a `DispatchQueue` and within a `DispatchGroup`. A `DispatchWorkItem` can also be set as a `DispatchSource` event, registration, or cancel handler.

A work item is as simple as a closure:

```swift
let workItem = DispatchWorkItem {
    // do something
}
workItem.perform() // will dispatch on the main thread
```

You can notify your main queue \(or any other queue\) when a work item is dispatched:

```swift
let queue = DispatchQueue.global()
queue.async(execute: workItem)
workItem.notify(queue: DispatchQueue.main) {
    // work item has been done
}
```

To cancel a work item is simple: `workItem.cancel()`, be aware that you can only cancel an item before it reaches the head of a queue and starts executing.

Like [immutable object](../../java/concurrency.md#immutable-objects) in Java, constants in Swift is read-only and **thread-safe**. However, collection types like `Array` and `Dictionary` are not thread-safe when declared mutable. There's no such concern in a serial dispatch queue because tasks are executed one by one. But thread interference and memory consistency errors can occur in a concurrent dispatch queue.

You set `.barrier` flag to a `DispatchWorkItem` before submit it to a concurrent queue to indicate that it should be the **only** item executed on the specified queue for that particular time.

```swift
let workItem = DispatchWorkItem(flags: [.barrier]) {
    // do something
}
```

## DispatchQoS

[`DispatchQoS`](https://developer.apple.com/documentation/dispatch/dispatchqos) encapsulates quality of service classes. A **quality of service** \(QoS\) class categorizes work to be performed on a `DispatchQueue`. By specifying a QoS to work, you indicate its importance, and the system prioritizes it and schedules it accordingly.

Because higher priority work is performed more quickly and with more resources than lower priority work, it typically requires more **energy** than lower priority work. Accurately specifying appropriate QoS classes for the work your app performs ensures that your app is responsive and energy efficient. `DispatchQoS.QoSClass` encapsulates quality of service classes: 

* userInteractive: highest priority
* userInitiated
* 'default'
* utility
* background: lowest priority
* unspecified

Of course, tasks running on the main thread have always the highest priority, as the main queue also deals with the UI and keeps the app responsive.

## DispatchGroup

[`DispatchGroup`](https://developer.apple.com/documentation/dispatch/dispatchgroup) allows for aggregate synchronization of work. You can use them to submit multiple different work items and track when they all complete, even though they might run on different queues. This behavior can be helpful when progress can’t be made until all of the specified tasks are complete.

```swift
let group = DispatchGroup()
group.enter()
URLSession.shared.dataTask(with: url) { 
    data, response, error in
    // do something
    group.leave()
}
group.notify(queue: .main) { // all task completed
    self.updateUI()
}
```

## Reference

* [https://developer.apple.com/documentation/dispatch](https://developer.apple.com/documentation/dispatch)
* Java [Concurrency](../../java/concurrency.md)
* [https://www.appcoda.com/grand-central-dispatch/](https://www.appcoda.com/grand-central-dispatch/)
* [https://www.raywenderlich.com/148513/grand-central-dispatch-tutorial-swift-3-part-1](https://www.raywenderlich.com/148513/grand-central-dispatch-tutorial-swift-3-part-1%20
  )
* [https://www.raywenderlich.com/148515/grand-central-dispatch-tutorial-swift-3-part-2](https://www.raywenderlich.com/148515/grand-central-dispatch-tutorial-swift-3-part-2)




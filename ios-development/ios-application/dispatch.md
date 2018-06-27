# Dispatch

* Ref: [https://developer.apple.com/documentation/dispatch](https://developer.apple.com/documentation/dispatch)
* Ref: Java [Concurrency](../../java/concurrency.md)

## Overview

Execute code **concurrently** on **multicore** hardware by submitting work to **dispatch queues** managed by the system. GCD \(Grand Central Dispatch\), operating at the system level, can better accommodate the needs of all running applications, matching them to the available system resources in a balanced fashion.

GCD provides and manages FIFO queues to which your application can submit tasks in the form of block objects. Work submitted to dispatch queues are executed on a pool of threads fully managed by the system.

Each work item can be executed either **synchronously** or **asynchronously**. When a work item is executed synchronously with the sync method, the program waits until execution finishes before the method call returns. When a work item is executed asynchronously with the async method, the method call returns immediately.

A dispatch queue can be either **serial**, so that work items are executed _one at a time_, or it can be **concurrent**, so that work items are _dequeued in order,_ but _run all at once and can finish in any order_. Both serial and concurrent queues process work items in first in, first-out \(FIFO\) order.

When an app launches, the system automatically creates a special queue called the **main** _queue_. Work items enqueued to the main queue execute serially on your app’s **main thread**. Attempting to **synchronously** execute a work item on the main queue results in **dead-lock**. In addition to the serial main queue, the system also creates a number of global concurrent dispatch queues. You can access the global concurrent queue that best matches a specified quality of service.

## Classes

[`DispatchQueue`](https://developer.apple.com/documentation/dispatch/dispatchqueue) manages the execution of work items. Each work item submitted to a queue is processed on a pool of threads managed by the system.

[`DispatchWorkItem`](https://developer.apple.com/documentation/dispatch/dispatchworkitem) encapsulates work that can be performed. Work items allow you to configure properties of individual work units, for the purposes of waiting for their completion, getting notified about their completion, and/or canceling them. A work item can be dispatched onto a `DispatchQueue` and within a `DispatchGroup`. A `DispatchWorkItem` can also be set as a `DispatchSource` event, registration, or cancel handler.

[`DispatchQoS`](https://developer.apple.com/documentation/dispatch/dispatchqos) encapsulates quality of service classes. A **quality of service** \(QoS\) class categorizes work to be performed on a DispatchQueue. By specifying a QoS to work, you indicate its importance, and the system prioritizes it and schedules it accordingly. Because higher priority work is performed more quickly and with more resources than lower priority work, it typically requires more energy than lower priority work. Accurately specifying appropriate QoS classes for the work your app performs ensures that your app is responsive and energy efficient. `DispatchQoS.QoSClass` encapsulates quality of service classes: 

* userInteractive: highest priority
* userInitiated
* 'default'
* utility
* background: lowest priority
* unspecified

[`DispatchGroup`](https://developer.apple.com/documentation/dispatch/dispatchgroup) allows for aggregate synchronization of work. You can use them to submit multiple different work items and track when they all complete, even though they might run on different queues. This behavior can be helpful when progress can’t be made until all of the specified tasks are complete.




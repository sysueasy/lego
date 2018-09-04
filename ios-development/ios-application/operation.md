# Process and Threads

## RunLoop

A `RunLoop` object processes input for sources such as mouse and keyboard events from the window system, [`Port`](https://developer.apple.com/documentation/foundation/port) objects, and [`NSConnection`](https://developer.apple.com/documentation/foundation/nsconnection) objects. A `RunLoop` object also processes [`Timer`](https://developer.apple.com/documentation/foundation/timer) events.

Your application neither creates or explicitly manages `RunLoop` objects. Each [`Thread`](https://developer.apple.com/documentation/foundation/thread) object—including the application’s _main_ thread—has an `RunLoop` object automatically created for it as needed. If you need to access the current thread’s run loop, you do so with the class method `current`.

```swift
print(Thread.current.description)
```

The `RunLoop` class is generally not considered to be **thread-safe** and its methods should only be called within the context of the current thread. You should never try to call the methods of an `RunLoop` object running in a different thread, as doing so might cause unexpected results.

## Operation

Because the `Operation` class is an abstract class, you do not use it directly but instead subclass or use one of the system-defined subclasses to perform the actual task:

* The `NSInvocationOperation` implements a **non-concurrent** operation.
* The `BlockOperation` manages the **concurrent** execution of one or more blocks. You can use this object to execute several blocks at once without having to create separate operation objects for each. When executing more than one block, the operation itself is considered finished only when all blocks have finished executing. Blocks added to a block operation are dispatched with default **priority** to an appropriate work queue.

An operation object is a single-shot object—that is, it executes its task once and cannot be used to execute it again. You typically execute operations by adding them to an operation queue \(an instance of the `OperationQueue` class\). An operation queue executes its operations either directly, by running them on secondary threads, or indirectly using the `libdispatch` library \(also known as **Grand Central Dispatch**\).

```swift
let operation1 = BlockOperation {
    print("operation1...")
}
operation1.addExecutionBlock {
    print("operation1.1...")
}
        
let operation2 = BlockOperation {
    print("operation2...")
}
operation2.addDependency(operation1)
        
operationQueue.addOperations([operation1, operation2], waitUntilFinished: false)
// If true, the current thread is blocked until all of the specified operations finish executing.
// If false, the operations are added to the queue and control returns immediately to the caller.
```

Dependencies are a convenient way to execute operations in a **specific order**. By default, an operation object that has dependencies is not considered ready until all of its dependent operation objects have finished executing. The dependencies make no distinction about whether a dependent operation finished successfully or unsuccessfully. \(In other words, canceling an operation similarly marks it as finished.\)

The `NSOperation` class is itself **multicore** aware. It is therefore safe to call the methods of an `NSOperation` object from multiple threads without creating additional locks to synchronize access to the object.

## OperationQueue

An operation queue executes its queued `Operation` objects based on their priority and readiness. After being added to an operation queue, an operation remains in its queue until it reports that it is finished with its task. You can’t directly remove an operation from a queue after it has been added.

Operation queues retain operations until they're finished, and queues themselves are retained until all operations are finished. Suspending an operation queue with operations that aren't finished can result in a **memory leak**.

## Reference

* Source Code: [DemoOperation.playground](https://github.com/sysueasy/whatsnew/tree/master/playgrounds/DemoOperation.playground)
* Ref: [https://developer.apple.com/documentation/foundation/operation](https://developer.apple.com/documentation/foundation/operation)
* Ref: [https://developer.apple.com/documentation/foundation/operationqueue](https://developer.apple.com/documentation/foundation/operationqueue)
* Ref: [https://agostini.tech/2017/08/20/dispatchgroup-vs-operationqueue-in-swift/](https://agostini.tech/2017/08/20/dispatchgroup-vs-operationqueue-in-swift/)


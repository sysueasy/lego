# Operation

## Operation

Because the `Operation` class is an abstract class, you do not use it directly but instead subclass or use one of the system-defined subclasses to perform the actual task:

* The `NSInvocationOperation` implements a **non-concurrent** operation.
* The `BlockOperation` manages the **concurrent** execution of one or more blocks. You can use this object to execute several blocks at once without having to create separate operation objects for each. When executing more than one block, the operation itself is considered finished only when all blocks have finished executing. Blocks added to a block operation are dispatched with default priority to an appropriate work queue.

An operation object is a single-shot object—that is, it executes its task once and cannot be used to execute it again. You typically execute operations by adding them to an operation queue \(an instance of the `OperationQueue` class\). An operation queue executes its operations either directly, by running them on secondary threads, or indirectly using the `libdispatch` library \(also known as **Grand Central Dispatch**\).

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


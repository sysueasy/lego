# Operation

## Operation

Because the `Operation` class is an abstract class, you do not use it directly but instead subclass or use one of the system-defined subclasses \(`NSInvocationOperation` or `BlockOperation`\) to perform the actual task.

An operation object is a single-shot object—that is, it executes its task once and cannot be used to execute it again. You typically execute operations by adding them to an operation queue \(an instance of the `OperationQueue` class\). An operation queue executes its operations either directly, by running them on secondary threads, or indirectly using the `libdispatch` library \(also known as **Grand Central Dispatch**\).

If you do not want to use an operation queue, you can execute an operation yourself by calling its `start()` method directly from your code. Executing operations manually does put more of a burden on your code, because starting an operation that is not in the ready state triggers an exception. The `isReady` property reports on the operation’s readiness.

Dependencies are a convenient way to execute operations in a **specific order**. By default, an operation object that has dependencies is not considered ready until all of its dependent operation objects have finished executing.

The dependencies supported by `NSOperation` make no distinction about whether a dependent operation finished successfully or unsuccessfully. \(In other words, canceling an operation similarly marks it as finished.\)

The `NSOperation` class is itself **multicore** aware. It is therefore safe to call the methods of an `NSOperation` object from multiple threads without creating additional locks to synchronize access to the object. This behavior is necessary because an operation typically runs in a separate thread from the one that created and is monitoring it.



## OperationQueue

An operation queue executes its queued `Operation` objects based on their priority and readiness. After being added to an operation queue, an operation remains in its queue until it reports that it is finished with its task. You can’t directly remove an operation from a queue after it has been added.

Operation queues retain operations until they're finished, and queues themselves are retained until all operations are finished. Suspending an operation queue with operations that aren't finished can result in a **memory leak**.

## Reference

* Ref: [https://developer.apple.com/documentation/foundation/operation](https://developer.apple.com/documentation/foundation/operation)
* Ref: [https://developer.apple.com/documentation/foundation/operationqueue](https://developer.apple.com/documentation/foundation/operationqueue)
* Ref: [https://agostini.tech/2017/08/20/dispatchgroup-vs-operationqueue-in-swift/](https://agostini.tech/2017/08/20/dispatchgroup-vs-operationqueue-in-swift/)

## KVC and KVO

When an object is key-value coding \(KVC\) compliant, its properties are addressable via string parameters through a concise, uniform messaging interface.

```objectivec
@interface BankAccount : NSObject
@property (nonatomic) NSNumber* currentBalance;              // An attribute
@property (nonatomic) Person* owner;                         // A to-one relation
@property (nonatomic) NSArray< Transaction* >* transactions; // A to-many relation
@end
```

Because the `BankAccount` class is key-value coding compliant, it recognizes the keys `owner`, `currentBalance`, and `transactions`, which are the names of its properties. Instead of calling the `setCurrentBalance:` method, you can set the value by its key:

`[myAccount setValue:@(100.0) forKey:@"currentBalance"];`.

Key-value observing \(KVO\) provides a mechanism that allows objects to be notified of changes to specific properties of other objects.


# Processes and Threads

## RunLoop

A `RunLoop` object processes input for sources such as mouse and keyboard events from the window system, [`Port`](https://developer.apple.com/documentation/foundation/port) objects, and [`NSConnection`](https://developer.apple.com/documentation/foundation/nsconnection) objects. A `RunLoop` object also processes [`Timer`](https://developer.apple.com/documentation/foundation/timer) events.

Your application neither creates or explicitly manages `RunLoop` objects. Each [`Thread`](https://developer.apple.com/documentation/foundation/thread) object—including the application’s _main_ thread—has an `RunLoop` object automatically created for it as needed. If you need to access the current thread’s run loop, you do so with the class method [`current`](https://developer.apple.com/documentation/foundation/runloop/1412291-current).

The `RunLoop` class is generally not considered to be **thread-safe** and its methods should only be called within the context of the current thread. You should never try to call the methods of an `RunLoop` object running in a different thread, as doing so might cause unexpected results.  



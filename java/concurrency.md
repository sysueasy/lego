# 5. Concurrency

The Java platform is designed from the ground up to support concurrent programming. Since version 5.0, the Java platform has also included high-level concurrency APIs in the `java.util.concurrent` packages.

## Processes and Threads

A **process** generally has a complete, private set of basic run-time resources; in particular, each process has its own memory space. Processes are often seen as synonymous with programs or applications.

However, what the user sees as a single application may in fact be a set of cooperating processes. To facilitate communication between processes, most operating systems support Inter Process Communication \(IPC\) resources, such as pipes and sockets.

**Threads** exist within a process — every process has at least one. From the application programmer's point of view, you start with just one thread, called the **main** thread. This thread has the ability to create additional threads.

Single-core devices can achieve concurrency through **time-slicing**. They would run one thread, perform a context switch, then run another thread. Multi-core devices on the other hand, execute multiple threads at the same time via **parallelism**.

## Thread Objects

There are two basic strategies for using Java Thread objects to create a concurrent application:

To directly control thread creation and management, simply instantiate [`Thread`](https://docs.oracle.com/javase/tutorial/essential/concurrency/runthread.html) each time the application needs to initiate an asynchronous task.

```java
public class HelloRunnable implements Runnable {
    public void run() {
        System.out.println("Hello from a thread!");
    }
    public static void main(String args[]) {
        (new Thread(new HelloRunnable())).start();
    }
}
```

To abstract thread management from the rest of your application, pass the application's tasks to an [executor](concurrency.md#executors).

`Thread.sleep` causes the current thread to suspend execution for a specified period. This is an efficient means of making processor time available to the other threads of an application or other applications that might be running on a computer system.

## Synchronization

Threads communicate primarily by sharing access to fields and the objects reference fields refer to. This form of communication is extremely efficient, but makes two kinds of errors possible:

* **Thread** **Interference** happens when two operations, running in different threads, but acting on the same data, **interleave**.
* **Memory consistency errors** occur when different threads have inconsistent views of what should be the same data.

The tool needed to prevent these errors is **synchronization**. The Java programming language provides two basic synchronization idioms: synchronized methods and synchronized statements.

```java
public class SynchronizedCounter {
    private int c = 0;
    public synchronized void increment() {
        c++;
    }
}
```

When a thread invokes a synchronized method, it automatically acquires the intrinsic lock for that method's object and releases it when the method returns. The lock release occurs even if the return was caused by an uncaught exception. Making these methods synchronized has two effects:

* First, it is not possible for two invocations of synchronized methods on the same object to **interleave**. When one thread is executing a synchronized method for an object, all other threads that invoke synchronized methods for the same object block \(suspend execution\) until the first thread is done with the object.
* Second, when a synchronized method exits, it automatically establishes a **happens-before** relationship with any subsequent invocation of a synchronized method for the same object. This guarantees that changes to the state of the object are visible to all threads.

### Liveness

However, synchronization can introduce **thread contention**, which occurs when two or more threads try to access the same resource simultaneously and cause the Java runtime to execute one or more threads more slowly, or even suspend their execution.

A concurrent application's ability to execute in a timely manner is known as its **liveness**. The most common kind of liveness problem is **deadlock**. Deadlock describes a situation where two or more threads are blocked forever, waiting for each other. Two other much less common problems are starvation and livelock. Starvation and livelock are forms of thread contention.

* **Starvation** describes a situation where a thread is unable to gain regular access to shared resources and is unable to make progress. This happens when shared resources are made unavailable for long periods by "greedy" threads.
* A thread often acts in response to the action of another thread. If the other thread's action is also a response to the action of another thread, then **livelock** may result.

### Atomic Access

In programming, an **atomic** action is one that effectively happens all at once. An atomic action cannot stop in the middle: it either happens completely, or it doesn't happen at all.

There are actions you can specify that are atomic:

* Reads and writes are atomic for _reference_ variables and for most _primitive_ variables \(all types except _long_ and _double_\).
* Reads and writes are atomic for all variables declared **volatile** \(including long and double variables\).

The [`java.util.concurrent.atomic`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/atomic/package-summary.html) package defines classes that support atomic operations on single variables.

### Guarded Blocks

Threads often have to coordinate their actions. The most common coordination idiom is the [guarded block](https://docs.oracle.com/javase/tutorial/essential/concurrency/guardmeth.html). Such a block begins by polling a condition that must be true before the block can proceed. 

The invocation of `Object.wait` does not return until another thread has issued a notification `Object.notifyAll` that some special event may have occurred — though not necessarily the event this thread is waiting for. Let's use guarded blocks to create a [Producer-Consumer](https://docs.oracle.com/javase/tutorial/essential/concurrency/guardmeth.html) ****application.

```java
public class Drop {
    private String message;
    private boolean empty = true;
    
    public synchronized String take() {
        while (empty) {
            try {
                wait();
            } catch (InterruptedException e) {}
        }
        empty = true;
        notifyAll();
        return message;
    }

    public synchronized void put(String message) {
        while (!empty) {
            try { 
                wait();
            } catch (InterruptedException e) {}
        }
        empty = false;
        this.message = message;
        notifyAll();
    }
}
```

### Immutable Objects

Immutable objects are particularly useful in concurrent applications. Since they cannot change state, they cannot be corrupted by thread interference or observed in an inconsistent state. Maximum reliance on immutable objects is widely accepted as a sound strategy for creating simple, reliable code.

## High Level Concurrency Objects

### Lock Objects

[Lock objects](https://docs.oracle.com/javase/tutorial/essential/concurrency/newlocks.html) work very much like the implicit locks used by synchronized code. As with implicit locks, only one thread can own a Lock object at a time. Lock objects also support a wait/notify mechanism, through their associated Condition objects.

### Executors

In all of the previous examples, there's a close connection between the task being done by a new thread, as defined by its `Runnable` object, and the thread itself, as defined by a `Thread` object. This works well for small applications, but in large-scale applications, it makes sense to separate thread management and creation from the rest of the application. Objects that encapsulate these functions are known as **executors**.

The `Executor` interface does not strictly require that execution be asynchronous. In this simplest case, an executor can run the submitted task immediately in the caller's thread:

```java
 class DirectExecutor implements Executor {
   public void execute(Runnable r) {
     r.run();
   }
}
```

More typically, tasks are executed in some thread other than the caller's thread. The executor below spawns a new thread for each task.

```java
class ThreadPerTaskExecutor implements Executor {
   public void execute(Runnable r) {
     new Thread(r).start();
   }
}
```

Most of the executor implementations in `java.util.concurrent` use **thread pools**, which consist of **worker threads**, one is [`ThreadPoolExecutor`](https://docs.oracle.com/javase/7/docs/api/java/util/concurrent/ThreadPoolExecutor.html). This kind of thread exists separately from the `Runnable` and `Callable` tasks it executes and is often used to execute multiple tasks. Using worker threads minimizes the overhead due to thread creation. Thread objects use a significant amount of memory, and in a large-scale application, allocating and deallocating many thread objects creates a significant memory management overhead.

One common type of thread pool is the **fixed thread pool**. An important advantage of the fixed thread pool is that applications using it degrade gracefully. To understand this, consider a web server application where each HTTP request is handled by a separate thread. If the application simply creates a new thread for every new HTTP request, and the system receives more requests than it can handle immediately, the application will suddenly stop responding to all requests when the overhead of all those threads exceed the capacity of the system.

### Concurrent Collections

[`BlockingQueue`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/BlockingQueue.html) defines a first-in-first-out data structure that blocks or times out when you attempt to add to a full queue, or retrieve from an empty queue.

[`ConcurrentMap`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/ConcurrentMap.html) is a subinterface of `java.util.Map` that defines useful atomic operations. Making these operations atomic helps avoid synchronization. The standard general-purpose implementation of ConcurrentMap is `ConcurrentHashMap`, which is a concurrent analog of `HashMap`.

All of these [collections](https://docs.oracle.com/javase/tutorial/essential/concurrency/collections.html) help avoid memory consistency errors by defining a happens-before relationship between an operation that adds an object to the collection with subsequent operations that access or remove that object.


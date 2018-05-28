# Concurrency

The Java platform is designed from the ground up to support concurrent programming. Since version 5.0, the Java platform has also included high-level concurrency APIs in the java.util.concurrent packages.

## Processes and Threads

A **process** generally has a complete, private set of basic run-time resources; in particular, each process has its own memory space. Processes are often seen as synonymous with **programs** or **applications**.

However, what the user sees as a single application may in fact be a set of cooperating processes. To facilitate communication between processes, most operating systems support **Inter Process Communication** \(IPC\) resources, such as **pipes** and **sockets**. IPC is used not just for communication between processes on the same system, but processes on different systems.

**Threads** exist within a process — every process has at least one. From the application programmer's point of view, you start with just one thread, called the **main** thread. This thread has the ability to create additional threads.

## Thread Objects

There are two basic strategies for using Java **Thread** objects to create a concurrent application:

* To directly control thread creation and management, simply instantiate **Thread** each time the application needs to initiate an asynchronous task.
* To abstract thread management from the rest of your application, pass the application's tasks to an **executor**.

The Runnable interface defines a single method, **run**, meant to contain the code executed in the thread.

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

Thread.**sleep** causes the current thread to suspend execution for a specified period. This is an efficient means of making processor time available to the other threads of an application or other applications that might be running on a computer system.

The **join** method allows one thread to wait for the completion of another.

## Synchronization

Threads communicate primarily by sharing access to fields and the objects reference fields refer to. This form of communication is extremely efficient, but makes two kinds of errors possible: **thread interference** and **memory consistency errors**. The tool needed to prevent these errors is synchronization.

However, synchronization can introduce **thread contention**, which occurs when two or more threads try to access the same resource simultaneously and cause the Java runtime to execute one or more threads more slowly, or even suspend their execution. **Starvation** and **livelock** are forms of thread contention.

_Interference_ happens when two operations, running in different threads, but acting on the same data, **interleave**. 

_Memory consistency errors_ occur when different threads have inconsistent views of what should be the same data. The key to avoiding memory consistency errors is understanding the **happens-before** relationship.

### S**ynchronized methods**

The Java programming language provides two basic synchronization idioms: **synchronized methods** and synchronized statements.

```java
public class SynchronizedCounter {
    private int c = 0;
    public synchronized void increment() {
        c++;
    }
}
```

Making these methods synchronized has two effects:

* First, it is not possible for two invocations of synchronized methods on the same object to interleave. When one thread is executing a synchronized method for an object, all other threads that invoke synchronized methods for the same object block \(suspend execution\) until the first thread is done with the object.
* Second, when a synchronized method exits, it automatically establishes a happens-before relationship with any subsequent invocation of a synchronized method for the same object. This guarantees that changes to the state of the object are visible to all threads.

Synchronized methods enable a simple strategy for preventing thread interference and memory consistency errors: if an object is visible to more than one thread, all reads or writes to that object's variables are done through synchronized methods. This strategy is effective, but can present problems with **liveness**.

### Intrinsic lock

When a thread invokes a synchronized method, it automatically acquires the **intrinsic lock** for that method's object and releases it when the method returns. The lock release occurs even if the return was caused by an uncaught exception.

### Atomic Access

In programming, an **atomic** action is one that effectively happens all at once. An atomic action cannot stop in the middle: it either happens completely, or it doesn't happen at all.

There are actions you can specify that are atomic:

* Reads and writes are atomic for _reference_ variables and for most _primitive_ variables \(all types except _long_ and _double_\).
* Reads and writes are atomic for all variables declared **volatile** \(including long and double variables\).

## Liveness

A concurrent application's ability to execute in a timely manner is known as its **liveness**. The most common kind of liveness problem is deadlock, two other  much less common problems are starvation and livelock.

**Deadlock** describes a situation where two or more threads are blocked forever, waiting for each other.

**Starvation** describes a situation where a thread is unable to gain regular access to shared resources and is unable to make progress. This happens when shared resources are made unavailable for long periods by "greedy" threads.

A thread often acts in response to the action of another thread. If the other thread's action is also a response to the action of another thread, then **livelock** may result.

## Guarded Blocks

Threads often have to coordinate their actions. The most common coordination idiom is the **guarded block**. Such a block begins by polling a condition that must be true before the block can proceed. 

```java
public synchronized void guardedJoy() {
    // This guard only loops once for each special event, 
    // which may not be the event we're waiting for.
    while(!joy) {
        try {
            wait();
        } catch (InterruptedException e) {}
    }
    System.out.println("Joy have been achieved!");
}
```

The invocation of **Object.wait** does not return until another thread has issued a notification that some special event may have occurred — though not necessarily the event this thread is waiting for.

When wait is invoked, the thread releases the lock and suspends execution. At some future time, another thread will acquire the same lock and invoke **Object.notifyAll**, informing all threads waiting on that lock that something important has happened.

Let's use guarded blocks to create a **Producer-Consumer** application.




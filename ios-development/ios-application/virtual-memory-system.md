# Virtual Memory System

> [Memory Usage Performance Guidelines](https://developer.apple.com/library/archive/documentation/Performance/Conceptual/ManagingMemory/Articles/AboutMemory.html)

Programs must be loaded into memory before they can run and, while running, they allocate additional memory \(both explicitly and implicitly\) to store and manipulate program-level data. 

Minimizing memory usage not only decreases your application’s memory footprint, but also reduce the amount of CPU time it consumes.

Both OS X and iOS include a fully-integrated, always-on **virtual memory system**. System provides approximately **18 EB** of addressable space for 64-bit processes.

The virtual memory manager creates a **logical address space** \(or “virtual” address space\) for each process and divides it up into uniformly-sized chunks of memory called _**pages**_. The processor and its **memory management unit** \(MMU\) maintain a _**page table**_ to **map** pages in the program’s logical address space to hardware addresses in the computer’s RAM.

As far as a program is concerned, addresses in its logical address space are always available. However, if an application accesses an address on a memory page that is not currently in physical RAM, a _**page fault**_ occurs. When that happens, the virtual memory system invokes a special page-fault handler to respond to the fault immediately. The page-fault handler stops the currently executing code, locates a free page of physical memory, loads the page containing the needed data from disk, updates the page table, and then returns control to the program’s code, which can then access the memory address normally. This process is known as _**paging**_.

If there are no free pages available in physical memory, the handler must first release an existing page to make room for the new page. How the system release pages depends on the platform. In OS X, the virtual memory system often writes pages to the backing store. The _**backing store**_ is a disk-based repository containing a copy of the memory pages used by a given process. Moving data from physical memory to the backing store is called _**paging out**_ \(or “swapping out”\); moving data from the backing store back in to physical memory is called _**paging in**_ \(or “swapping in”\). 

In iOS, there is no backing store and so data are never paged out to disk, but **read-only data** are still be **paged in** from disk as needed. Instead, if the amount of free memory drops below a certain threshold, the system asks the running applications to free up memory voluntarily. Applications that fail to free up enough memory are terminated.

A9 systems \(1st appeared at iPhone 6S\) expose **16KB pages** backed by 16-kilobyte physical pages. These sizes determine how many kilobytes the system reads from disk when a page fault occurs.




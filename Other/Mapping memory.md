Mapping memory means requesting the Operating System to add to your program memory space more memory.

An example on this is the `brk/sbrk` `libc` function, or the `mmap` syscall.
# Reference
> [!cite] Credits
> [The Goat](https://discord.com/channels/273534239310479360/273541522815713281/1566496663940501505) on discord for the knowledge

every program on an operating system that supports memory protection runs in its own virtual address space
this address space is a virtualization of a flat memory region that is 4 GiB in size on 32-bit systems and 16 EiB in size on 64-bit systems, and starts off mostly empty, i.e. you get a general protection fault if you try to access it, which the operating system translates into `STATUS_ACCESS_VIOLATION` or `SIGSEGV`, commonly known as a segfault

when memory is allocated, system calls are made to create mappings into this virtual address space that relate actual physical memory to the virtual address space, making it no longer a hole that just gives you segfaults, but instead an area of memory that can be read from and perhaps written to

on program startup, parts of the executable file of the program itself, present in memory in the form of disk cache, are mapped into the newly created virtual address space: code is mapped read-execute, read-only global variables (`static`s) are mapped read-only, and mutable global variables (`static mut`, `UnsafeCell`s in `static`s, etc.) are mapped read-write; in addition to those memory mappings, a read-write mapping for the main thread stack is created, and when the C runtime starts executing, its memory allocator also creates a mapping for its heap
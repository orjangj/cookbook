# Memory Management

The Linux memory management subsystem is responsible for managing the memory in the system, and
includes (but not limited to):

- Virtual memory
- Demand paging
- Memory allocation (both for kernel internal structures and user space programs)
- Mapping of files into processes address space
- Etc.

Other:

- Memory hotplug
- Swap memory
- Out Of Memory (OOM)
- MMU (memory management unit? Memory controller)

If you're not familiar with the jargon of Linux Memory Management for system administrators, you
should have a look at https://docs.kernel.org/admin-guide/mm/concepts.html.

## Configuration

The Linux memory management is a complex system with many configurable settings [1], many of which
are available via `/proc` filesystem and adjustable through use of the `sysctl` tool.

## Virtual Memory

The virtual memory abstracts the details of physical memory from the application software, allows
keeping only needed information in the physical memory (demand paging) and provides a mechanism for
the protection and controlled sharing of data between processes.

Why dealing directly with physical memory is complex:

- The physical memory is not necessarily contiguous
- The memory might be accessible as a set of distinct address ranges
- Different CPU architectures, and even different implementations of the same architectures have
  different view of how these address ranges are defined.

To overcome these complexities, the concept of virtual memory was developed.

The physical system memory is divided into page frames, or pages. The size of each page is
architecture specific. Some architectures allow selection of the page size from several supported
values; this selection is performed at the kernel build time by setting an appropriate kernel
configuration option.

## Out-of-Memory (OOM)

Debug tools:

- valgrind?
- pmap?

### Debugging

See https://access.redhat.com/solutions/3120401

## Resources

- [1]: [The Linux Kernel Memory Management](https://docs.kernel.org/admin-guide/mm/index.html)
- [2]: [How to select the right memory configuration for embedded systems](https://www.qt.io/embedded-development-talk/memory-options-for-embedded-systems-how-to-select-the-right-memory-configuration)

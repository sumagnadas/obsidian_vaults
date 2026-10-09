To achieve virtualization of memory, we need a mechanism to manage the memory in a way to isolate multiple processes from interfering with each other while running. This is nowadays done with hardware and software using a MMU or Memory Management Unit on the hardware side. The MMU generally supports two ways to manage memory. 
***Note***: Read and remember about CHERI.
## MMU
---
An MMU generally does a lot of stuff including but not limited to:-
- Address translation from virtual to physical. This can be a associative 1:1 set or a paging-like setup, where a table is set up/used to calculate the address/id of the page/unit of discrete memory block. 
- Checking bounds and limits of access for that process. The bounds can set as fixed (like in the case of paging) or variable (segment bounds). Access can be read-only or executable or not executable.
- Checking privilege level. Is this memory protected from the user? Can someone even access or read this page/segment?
## Segmentation
---
Segmentation is one of the first ways to isolate and protect memory in an OS. In this method, address space of a process is divided into multiple logical segments, such as code, data segments, etc. These segments are described by **base** (base address in memory) and **bounds** (the size of the segment). It can also be **Readable** and/or **Writable** and/or **Executable**. It can also only be accessed by certain **privilege levels**. 
This is widely used from the 1960s and at some point of time after that, got changed back to a flat memory model for paging.
## Paging
---
Physical memory is divided into pages of fixed size, for example 4KiB, and these pages are allocated to an address when needed. Every virtual address in a program has mainly two parts, the part which identifies the Physical Page Number/Frame and the Offset in that specific page.
Page Tables are used to find the mapping from virtual page number to physical page number. This is a one-to-one map where a physical page number is stored against a virtual one, which replaces the initial part of the address to locate the page and then the offset is used to find the actual byte.  Each address space gets their own highest level page table.
This method prevents external fragmentation entirely but might result in internal fragmentation although for a very small amount. (A few KiBs lost to internal fragmentation)
#### Single level
For only one level of paging, each virtual page number is mapped to a physical page number. This Virtual Page number may be created inside a process and mapped to a physical page in a per-process page table.
Each table takes a lot of memory. A page table is allocated for the whole address space (which in this case is whole of the physical memory), whether we use all of the pages or not. 
Assuming each page is of 4KiB,
- **32bit**: $2^{20}$ entries and each entry is of 4B -> about 4MB per process. These would scale to 400MB for only 100 processes.
- **48bt**: $2^{36}$ entries and each entry is of 8B -> about 512GB per process. Not possible to fit in a personal computer.
## Multi-level
 ![[Pasted image 20261008165310.png]]
This is just for reference, an idea. Depending on which level we find a leaf, the size of the page increases. Level 4 gives 4KiB pages, Level 3 gives 2 MiB pages, Level 2 gives 1 GiB pages and Level 1 doesn't contain any leaf entries.
Now as seen here, the PML 4 contains one entry to the base of a specific page directory pointer(PDP) table. This PDP contains a entry to the base of a specific Page Directory table. Now the Page Directory table contains a entry to the base of a specific Page table, which contains the actual Physical Page Number.

The table at each level contains a maximum number of entries which does not the whole address space. For example, level 4/Page table maps only 2 MB worth of pages i.e. 512 entries, taking about 2KiB. Now all of tables are never allocated unless required. 
At minimum, Each level contains only one 1 table. For 4 level paging and each table taking about 4KiB of space, that amounts to about 16KiB and assuming that each level can have a leaf entry, thats a lot of memory that can be mapped in that small of a memory.

**Worst case (everything mapped):** each 4KiB PT maps 2MiB, so the PT level costs 4KiB / 2MiB ≈ **0.2%** of the memory it maps. Each PD costs 4KiB per 1GiB mapped, and the upper levels are smaller still. Mapping 1GiB fully costs about 2MiB of page tables, still a rounding error compared to 512GiB.

**Overall**: Multi-level paging trades space for time. It becomes slow due to multiple memory references for each level jumping. Single level is fast but takes a lot of space. Apart from that, paging introduces an extra memory reference each time memory needs to be accessed slowing down processes in general.
## Free Space Management 
---
Both of the previous techniques are for how the memory is laid out and handled at the hardware level. It does not say how free space is allocated or allocated space is freed or how any kind of bookkeeping is done to manage allocated memory for processes. That is done by the OS.
### Free lists
---
- Free list is one of the ways to manage and represent free space in memory in a data structure like linked list. Each node represents a contiguous block of free memory, with `size` and a `next` pointer pointing to either NULL (end of free memory blocks) or start of another node/memory block. 
- In this kind of set-up, the allocated memory is represented as a header+actual memory block. This header contains the size of the memory as well as a magic number. The header is present in memory while the address of starting of the memory block is sent back to the allocator. When a memory is freed by the allocator using something like `free(ptr)`, OS checks the header before the `ptr` and uses it to free the memory. 
- Generally freeing an allocated memory block/space means adding it back to the free list. Apart from that, once the node is added back, they may be coalesced into a singular node representing a singular contiguous memory block if they are next to each other. 
- Due to allocation, there may be external fragmentation. This situation is minimized via allocation methods like best-fit, first-fit, etc. When memory is allocated, a node may be split into two parts, one becoming the free node and the other the allocated memory. After that, the node allocated is given a header of size and some magic number(?).
- On a side note, the free list repurposes the free memory to make the linked list. The first few bytes of each block is used to make a node representing the memory block.
### Slab allocation
---
- Free list has a lot of overhead for an allocation technique, with setting headers, splitting nodes. After that, there is also the overhead of initialization as the allocated memory is filled with garbage values.
- Slab allocation is a technique where fixed size storage is allocated for storing a specific object, removing the overhead of initialization and header setting. Imagine it as an already defined array of objects, where the type of the object is also fixed. 
- The algorithm works mostly with Cache and Slabs. Cache is the bookkeeping entry. It keeps metadata with rules on how to store the slabs, functions to initialize, size of struct, etc. Slabs are the actual storage units. They store the free lists of the objects as well as the count and memory address. 
  ```
  // General structure of cache and slab
  struct kmem_cache {
    const char *name;             // e.g. "task_struct"
    size_t object_size;           // sizeof(struct task_struct)
    void (*constructor)(void *);  // the init function, run once per object
    struct list_head slabs_full;  // slabs with zero free objects
    struct list_head slabs_partial; // slabs with some free objects
    struct list_head slabs_empty;   // slabs with zero allocated objects
    // ... locks, stats, per-CPU magazine pointers, etc.
  };
  struct slab {
    void *memory;         // pointer to the actual page(s)
    void *free_list;      // linked list threading through free object slots
    int in_use_count;
  };
  ```
- The general idea remains same, with the addition of one-time initialization for the objects when allocated and some amount of overhead decreases due to lesser amount of header setting (fixed size) and no node splitting. Freeing is also just adding the object slot to the free list and nothing more. This makes it O(1) for alloc and freeing.
- Structures like semaphores, process context blocks, etc represented in the OS are generally allocated in this way, which are requested many times during runtime and has a fixed-size.
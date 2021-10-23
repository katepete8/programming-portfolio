
## Project Highlights

### Building malloc in C

#### Overview

Through Carnegie Mellon University, I wrote the `malloc` command for allocating memory in C. In the implementation, I used linked lists to allocate and free blocks of memory. The project optimizes memory allocation through coalescing free memory blocks, systematically extending the heap, and improving search with multiple linked lists for different block sizes.

### Skills used
- C language
- Data structures (linked lists, stacks, queues, etc.)
- Systems level programming

### Code sample
This sample shows off the main `malloc` function used to allocate a new block of a certain size. Please note that I cannot share the entire project due to university policy. 

<details>
  <summary>Click to view</summary>
  <p>
    
  ```C
  void *malloc(size_t size) {

    dbg_requires(mm_checkheap(__LINE__));
    size_t asize;      // Adjusted block size
    size_t extendsize; // Amount to extend heap if no fit is found
    block_t *block;
    void *bp = NULL;

    // Initialize heap if it isn't initialized
    if (heap_start == NULL) {
        mm_init();
    }

    // Ignore spurious request
    if (size == 0) {
        dbg_ensures(mm_checkheap(__LINE__));
        return bp;
    }

    // Adjust block size to include header and to meet alignment requirements
    asize = round_up(size + wsize, dsize);
    if (asize < min_block_size) {
        asize = min_block_size;
    }

    // Search the free list for a fit
    block = find_fit(asize);
    // If no fit is found, request more memory, and then and place the block
    if (block == NULL) {
        // Always request at least chunksize
        extendsize = max(asize, chunksize);
        block = extend_heap(extendsize);
        // extend_heap returns an error
        if (block == NULL) {
            return bp;
        }
    }
  ```
                               
</p>
</details>


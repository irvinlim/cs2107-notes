# CS2107 Lecture 9

## Montgomery ladder
Software design to ensure that code runs in constant time regardless of inputs. Works by performing redundant operations or reordering code.

Need to go further to ensure constant time for memory fetches as well, by making cache identical hits/misses regardless of branch taken, so that cache fetch timing is exactly the same regardless.

## Rainbow tables
1. Hashes can be reversed by generating rainbow tables, which store successive hash results after a fixed number of hash operations. 
2. Compute a hash chain from the hash to be reversed.
  1. For each hash result in the hash chain, compare with rainbow table chain ends.
  2. Repeat until a match is found.
3. For the matched chain end, take the start of the chain in the rainbow table, and recompute the hash chain until the initial hash is found.
4. The reversed hash is the result found just before the initial hash was found.

## Memory usage and security

### Virtual memory
Each process is allocated an individual address space in memory. The process sees it as starting from address 0, although the translation unit maps the virtual address to the real physical address.

### Stack buffer overflow attacks
Attack mechanism:
- Find a program that accepts input from us (or our attacking program), and uses a buffer without checking bounds.
- Deliver EGG to it to overflow buffer
- Overwrite stack return address with address of malicious code
- Program then runs malicious code

#### Mitigations
- **Data Execution Prevention (DEP)**: System-level memory protection feature built into Windows since XP. DEP enables the system to mark one or more pages of memory as non-executable.
  - More generally termed as **executable space prevention**.
- **Address space layout randomization (ASLR)**: Randomly arranges starting memory addresses for stack, heap, libraries, etc., in order to prevent attackers from reliably jumping to an exploited function in memory.
- **Stack canaries**: Adding a value just below the return address in the stack. If the canary is altered, program execution is terminated immediately.
- Always check range of parameters to prevent stack buffer overflows.

### Heap buffer overflow attacks
Makes use of the OS garbage collector (GC) to write the address of the malicious code into a return address, hence causing programs to execute malicious code.

### OpenSSL Heartbleed
Example of buffer over-read.
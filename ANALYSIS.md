Honggfuzz:
    References:
        -    https://github.com/google/honggfuzz/blob/master/docs/FeedbackDrivenFuzzing.md
    Coverages collectable:
        - Edge
        - Block Coverage
        - Dataflow Coverage (comparisons and constants)
    Supports a bunch of different coverage metric collection options:
        -   (Linux) Hardware-based counters (instructions, branches) [WE DON'T CARE, only count, not list]

        -   (Linux) Intel BTS code coverage (kernel >= 4.2)
        -   (Linux) Intel PT code coverage (kernel >= 4.2)  (looks like it only tracks indirect branches (thanks @unicornisaurous :) ))
        -   Sanitizer-coverage instrumentation (-fsanitize-coverage=bb)
        -   Compile-time instrumentation (-finstrument-functions or -fsanitize-coverage=trace-pc[-guard],indirect-calls,trace-cmp or both)
        -   Function hooking: `wrap=`-function hooking for dataflow and constant extraction

    Coverage output:
        multiple bitmaps in shared memory mappings via `mmap` with HUGE page sizes, one file descriptor passed to child process for each

WinAFL:
    -   Intel PT
    -   DBI coverage using DynamoRIO
    -   Static instrumentation using Syzygy (BB, Branch, function, compare)-coverages, only for WINDOWS

Jackalope:
    References:
        -   https://github.com/googleprojectzero/Jackalope
    Coverage collection:
        -   (Linux)                     Sanitizer Coverage (-trace-pc-guard)
        -   (Windows & partially MacOS) TinyInst

LibAFL:
    Coverages:
        - Basic block
        - Edge
        - Dataflow (comparisons)
    Coverage Collection options:
        - LLVM sanitizer coverage (pc-guard, pc-guard-hitcounts, cmplog, value_profile, sancov_8bit)
        - Frida (CmpLog, Edge)
        - QEMU (CmpLog, Edge)

FuzzILI:
    LLVM trace-pc-guard

LLVM:
    Coverage collection flags:
        -   -fprofile-arcs and -ftest-coverage (or --coverage for both)
        -   SanitizerCoverage:

            -fsanitize-coverage=trace-pc-guard
                - for each edge:        __sanitizer_cov_trace_pc_guard(&guard_variable)
                - initialization:       __sanitizer_cov_trace_pc_guard_init(uint32_t *start, uint32_t *stop);

            -fsanitize-coverage=trace-pc,indirect-calls
                - for each ind. call:   __sanitizer_cov_trace_pc_indirect(void *callee)

            -fsanitize-coverage=inline-8bit-counters
                - for each edge:        compile 8-bit counter increment for edge guard
                - initially:            __sanitizer_cov_8bit_counters_init(char *start, char *end)
                    -   [start,end) is array of 8-bit counters for current object, store, read out counts on exit
                - WRAPPED ADDITION

            -fsanitize-coverage=inline-bool-flag
                - for each edge:        compile setting the current edge bool to true
                - initially:            same as inline-8bit-counters

            -fsanitize-coverage=pc-table
                - outputs PC table mapping index to basic block address
                - initially:            void __sanitizer_cov_pcs_init(const uintptr_t *pcs_beg, const uintptr_t *pcs_end)
                    -   store this array for later translation of PCs

            -fsanitize-coverage=trace-pc
                - for each edge:        call __sanitizer_cov_trace_pc()

            -fsanitize-coverage=trace-pc,indirect-calls
                - for each ind. call:   __sanitizer_cov_trace_pc_indirect(void* callee)

        -   Sanitizer Coverage offers different levels of instrumentation.

            -   edge (default): edges are instrumented (see below).
            -   bb: basic blocks are instrumented.
            -   func: only the entry block of every function will be instrumented.

            - Use these flags together with trace-pc-guard or trace-pc, like this: -fsanitize-coverage=func,trace-pc-guard.

        - Sanitizer Dataflow Coverage

            -   require one of -fsanitize-coverage={trace-pc,inline-8bit-counters,inline-bool}
            -   ```C
// Called before a comparison instruction.
// Arg1 and Arg2 are arguments of the comparison.
void __sanitizer_cov_trace_cmp1(uint8_t Arg1, uint8_t Arg2);
void __sanitizer_cov_trace_cmp2(uint16_t Arg1, uint16_t Arg2);
void __sanitizer_cov_trace_cmp4(uint32_t Arg1, uint32_t Arg2);
void __sanitizer_cov_trace_cmp8(uint64_t Arg1, uint64_t Arg2);

// Called before a comparison instruction if exactly one of the arguments is constant.
// Arg1 and Arg2 are arguments of the comparison, Arg1 is a compile-time constant.
// These callbacks are emitted by -fsanitize-coverage=trace-cmp since 2017-08-11
void __sanitizer_cov_trace_const_cmp1(uint8_t Arg1, uint8_t Arg2);
void __sanitizer_cov_trace_const_cmp2(uint16_t Arg1, uint16_t Arg2);
void __sanitizer_cov_trace_const_cmp4(uint32_t Arg1, uint32_t Arg2);
void __sanitizer_cov_trace_const_cmp8(uint64_t Arg1, uint64_t Arg2);

// Called before a switch statement.
// Val is the switch operand.
// Cases[0] is the number of case constants.
// Cases[1] is the size of Val in bits.
// Cases[2:] are the case constants.
void __sanitizer_cov_trace_switch(uint64_t Val, uint64_t *Cases);

// Called before a division statement.
// Val is the second argument of division.
void __sanitizer_cov_trace_div4(uint32_t Val);
void __sanitizer_cov_trace_div8(uint64_t Val);

// Called before a GetElemementPtr (GEP) instruction
// for every non-constant array index.
void __sanitizer_cov_trace_gep(uintptr_t Idx);

// Called before a load of appropriate size. Addr is the address of the load.
void __sanitizer_cov_load1(uint8_t *addr);
void __sanitizer_cov_load2(uint16_t *addr);
void __sanitizer_cov_load4(uint32_t *addr);
void __sanitizer_cov_load8(uint64_t *addr);
void __sanitizer_cov_load16(__int128 *addr);
// Called before a store of appropriate size. Addr is the address of the store.
void __sanitizer_cov_store1(uint8_t *addr);
void __sanitizer_cov_store2(uint16_t *addr);
void __sanitizer_cov_store4(uint32_t *addr);
void __sanitizer_cov_store8(uint64_t *addr);
void __sanitizer_cov_store16(__int128 *addr);
```

    Coverage outputs:
        - .gcda and .gcno
        - .sancov and .lcov files

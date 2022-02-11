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


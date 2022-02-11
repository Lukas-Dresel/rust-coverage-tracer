Dynamic Binary:
    - DynamoRIO
    - Intel PIN
    - Mesos (https://github.com/gamozolabs/mesos) (Windows Only)
    - TinyInst (Windows and partially MacOS only)
    - Frida
    - DynInst (https://dyninst.org/) (only basic block coverage)

Static Binary:
    - TrapFuzz (https://github.com/googleprojectzero/p0tools/tree/master/TrapFuzz)
    - Syzygy (https://github.com/google/syzygy)

Compiled:
    - LLVM Sanitizer Coverage

Hardware:
    - Intel Performance counters (unusable, don't report coverage, only counts)
    - Intel BTS (not available on my machine, 40x slowdown according to wikipedia)
    - Intel PT (super fast, might only track indirect branches, unless disassembly capabilities are present)
        - LibXDC for decoding (superfast, supposedly, gives full trace info into a bitmap)
        - Honeybee for decoding (supposedly faster lol, gives full hit edge & basic block sets)
            -   https://blog.trailofbits.com/2021/03/19/un-bee-lievable-performance-fast-coverage-guided-fuzzing-with-honeybee-and-intel-processor-trace/
            -   https://github.com/trailofbits/Honeybee

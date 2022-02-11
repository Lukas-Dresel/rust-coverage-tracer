Dynamic Binary:
    - DynamoRIO
    - Intel PIN
    - Mesos (https://github.com/gamozolabs/mesos) (Windows Only)
    - TinyInst (Windows and partially MacOS only)
    - Frida

Static Binary:
    - TrapFuzz (https://github.com/googleprojectzero/p0tools/tree/master/TrapFuzz)
    - Syzygy (https://github.com/google/syzygy)

Compiled:
    - LLVM Sanitizer Coverage

Hardware:
    - Intel Performance counters (unusable, don't report coverage, only counts)
    - Intel PT (super fast, might only track indirect branches, unless disassembly capabilities are present)
    - Intel BTS (not available on my machine, 40x slowdown according to wikipedia)
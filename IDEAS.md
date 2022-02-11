INTEL PT:
    With a bitmap: we can compute the index via `hash(last TIP packet IP + the following branch conditions)`

    locally like path coverage since it cannot detect merge points in the CFG
    ```rust
    if bla {

    }
    else {

    }
    if blub {

    }
    else {

    }
    ```

    Without a bitmap: we're gonna have to do  disassembly to compute the condition

From TinyInst's LiteCov:
    Special feature of the coverage module is that the coverage buffer in the target process is initially allocated as read-only, causing an exception the first time new coverage is encountered. Combined with an option to ignore a certain subset of coverage, this enables quickly querying if running the target with a given input resulted in new coverage or not.

From @Zommiommy:
    -   If you just need **new** coverage a way to get coverage is to compile with symbols and add breakpoints to every function. The nice thing is that it has basically 0 overhead because once you hit it you can remove it.
    -   Basically full speed fuzzing, try to find a way to incorporate it with sampling counts (maybe only instrument previously hit targets sometimes)

From @alkali9:
    - use yaxpeax (https://github.com/iximeow/yaxpeax-dis) instead of capstone for SPEEEEEEEEEEEEEEEEEEED if disassembly needed

From afl-sancov:
    - maybe have differential coverage reports (as in, only report branches not seen in every(most?) execution)
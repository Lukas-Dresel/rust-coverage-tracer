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
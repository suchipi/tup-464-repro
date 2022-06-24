# tup gh issue 464 repro

This repo demonstrates an undesirable difference in behavior between `tup 0.7.8-3 [debian]` and `tup v0.7.11-84-gc8bca8d8`.

## Overview

For this build:

- Everything lives under a "build-linux" variant
- `gen-header.c` gets compiled into a program called `gen-header`
- `gen-header` is run in order to create `generated-header.h`
- `generated-header.h` is used by `myprog.c`
- `myprog.c` gets compiled into a program called `myprog`

It seems that tup 0.7.8 and tup 0.7.11 treat the `./gen-header` command differently; 0.7.8 uses the program from the variant folder, but 0.7.11 tries to find the program outside of the variant folder (in the src folder).

## Reproducing the bug

git clone this, cd into it, then:

```
# set PATH such that `tup` refers to tup 0.7.8.3, then run:
$ ./run.sh
# set PATH such that `tup` refers to tup 0.7.11, then run:
$ ./run.sh
```

Expected behavior is that `run.sh` succeeds with both tup 0.7.8 and tup 0.7.11.

Actual behavior: succeeds with tup 0.7.8, but fails with 0.7.11, because it can't find `./gen-header` (when it attempts to run the command).

---
tags:
    - rust
---

# What is Rust?

- statically compiled language
    - [[rustc]] uses [[LLVM]]
- supports multiple platforms and architectures:
    - x86, ARM, WebAssembly
    - Linux, Mac, Windows

## Benefits of Rust

- *Compile time [[memory safety]]*: whole classes of memory bugs are prevented at compile
time
– No uninitialized variables.
– No double-frees.
– No use-after-free.
– No NULL pointers.
– No forgotten locked mutexes.
– No data races between threads.
– No iterator invalidation.

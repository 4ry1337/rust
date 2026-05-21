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

- *Compile time [[memory safety]]* - whole classes of memory bugs are prevented at compile
time
    - No uninitialized variables.
    - No double-frees.
    - No use-after-free.
    - No NULL pointers.
    - No forgotten locked mutexes.
    - No data races between threads.
    - No iterator invalidation.
- No undefined runtime behavior - what a Rust statement does is never left unspecified
    - Array access is bounds checked.
    - Integer overflow is defined (panic or wrap-around).
- Modern language features - as expressive and ergonomic as higher-level languages
    - Enums and [[pattern matching]].
    - [[Generics]].
    - No overhead [[FFI]].
    - [[Zero-cost abstractions]].
    - Great compiler errors.
    - Built-in dependency manager.
    - Built-in support for testing.
    - Excellent [[Language Server Protocol]] support.

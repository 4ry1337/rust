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
    - Enums and pattern matching.
    - [[10. generics|Generics]].
    - No overhead [[FFI]].
    - Zero-cost abstractions.
    - Great compiler errors.
    - Built-in dependency manager - cargo.
    - Built-in support for testing - cargo test.
    - Excellent [[Language Server Protocol]] support.

## Table of Content

```folder-overview
id: 98fe2c6e-4a3f-468a-950a-b8939f87b68f
folderPath: ""
title: "{{folderName}} overview"
showTitle: false
depth: 3
includeTypes:
  - folder
  - markdown
style: list
disableFileTag: false
sortBy: name
sortByAsc: true
showEmptyFolders: false
onlyIncludeSubfolders: false
storeFolderCondition: true
showFolderNotes: false
disableCollapseIcon: true
alwaysCollapse: false
autoSync: true
allowDragAndDrop: true
hideLinkList: true
hideFolderOverview: false
useActualLinks: false
fmtpIntegration: false
titleSize: 1
isInCallout: false
```

```dataview
table without id
    file.link as Title
FROM
    "Literature Notes/rust"
WHERE
    file.name != "rust"
```

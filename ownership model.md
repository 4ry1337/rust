---
tags:
    - rust
---

> [!INFO] Ownership
> A set of rules that govern how a [rust] program [[memory management|manages memory]][^1].

Rules:

- each value in Rust has a owner.
- there can only be one owner a time.
- when the owner foes out of scope, the value will be dropped.

[^1]: [RUST BOOK](https://doc.rust-lang.org/stable/book/ch04-01-what-is-ownership.html)
[^2]: [RUST REFERENCE](https://doc.rust-lang.org/reference)

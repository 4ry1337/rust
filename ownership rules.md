---
tags:
    - rust
---

> [!INFO] Ownership
> A set of rules that govern how a [rust] program [[memory management|manages memory]][^1].

Rules:

- each value in Rust has a owner.
- there can only be one owner a time.
- when the owner goes out of scope, the value will be dropped.

These rules are enforced through the [[borrow checker]].

[^1]: [RUST BOOK](https://doc.rust-lang.org/stable/book/ch04-01-what-is-ownership.html)
[^2]: [RUST REFERENCE](https://doc.rust-lang.org/reference)
[^3]: [Rust for Rustaceans](https://learning.oreilly.com/library/view/rust-for-rustaceans/9781098129828/)

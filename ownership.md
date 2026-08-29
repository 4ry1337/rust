---
tags:
    - rust
---

> [!INFO] Ownership
> A set of rules that govern how a [rust] program [[memory management|manages memory]][^1].

Rules:

1. each value in Rust has a owner.
2. there can only be one owner a time.
3. when the owner goes out of scope, the value will be dropped.

These rules are enforced through the [[borrow checker]].

## Mutability

*Mutability* of data can be changed when ownership is transferred.

## Move

*A shallow copy* that invalidates the previous owner is called *a move*.

## Clone

*A deep copy* is achieved explicitly by calling the `.clone()` method on a type that implements the `Clone` trait.


[^1]: [RUST BOOK](https://doc.rust-lang.org/stable/book/ch04-01-what-is-ownership.html)
[^2]: [RUST REFERENCE](https://doc.rust-lang.org/reference)
[^3]: [Rust for Rustaceans](https://learning.oreilly.com/library/view/rust-for-rustaceans/9781098129828/)

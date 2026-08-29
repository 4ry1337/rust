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

## Move-semantics

When doing assignments 
```rust
let x = y
```
or passing function arguments by value (foo(x)), the ownership of the resources is transferred.

In Rust-speak, this is known as a *move*.

### Mutability

*Mutability* of data can be changed when ownership is transferred.


[^1]: [RUST BOOK](https://doc.rust-lang.org/stable/book/ch04-01-what-is-ownership.html)
[^2]: [RUST REFERENCE](https://doc.rust-lang.org/reference)
[^3]: [Rust for Rustaceans](https://learning.oreilly.com/library/view/rust-for-rustaceans/9781098129828/)

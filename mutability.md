# Shareable mutable containers.

Memory safety is based on this rule: Given an object `T`, it is only possible to have one of the following:

- Several immutable references (`&T`) to the object (also known as *aliasing*).
- One mutable reference (`&mut T`) to the object (also known as *mutability*).

This is enforced by the Rust compiler. However, there are situations where this rule is not flexible enough. Sometimes it is required to have multiple references to an object and yet mutate it.

[^docs]: https://doc.rust-lang.org/std/cell/index.html
[^boook-cell]: https://doc.rust-lang.org/core/cell/index.html

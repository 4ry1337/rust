---
tags:
    - rust
    - references
    - borrowing
---

## Shared reference

A reference provides a way to access another value without taking ownership of the value, and is also called *borrowing*.

```rust
let a = 'A';
let b = 'B';

let mut r: &char = &a;
dbg!(r);
r = &b;
dbg!(r);
// *r = 'X' // immutable
```

- references are read-only
- referenced data is *immutable*
- references can never be null

## Exclusive reference (Mutable reference)

Exclusekk

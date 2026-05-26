---
tags:
    - rust
    - variables
    - let
    - constants
    - const
---

# Variables

Variables, by default, are immutable.

```rust
let x = 6;
x = 5; // error[E0384]: cannot assign twice to immutable variable `x`
```

To make them *mutable* add `mut` in front.

```rust
let mut x = 6;
x = 5;
```

## const

Constants are evaluated at compile time and their values are inlined wherever they are used.
Constants are valid for the entire time a program runs, within the scope in which they were declared.

- always immutable
- type must be annotated

## static

Static variables are global variables.

- immutable
- can only store references with `'static` [[lifetime]]

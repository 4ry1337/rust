---
tags:
    - rust
    - references
    - borrowing
    - slice
---

## Shared reference

A shared reference, `&T`, provides a way to access another value without taking ownership of the value, and is also called *borrowing*.

```rust
let a = 'A';
let b = 'B';

let mut r: &char = &a;
dbg!(r);
r = &b;
dbg!(r);
// *r = 'X' // immutable
```

- referenced data is *immutable* or *read-only*
- references can not outlive the referenced data 
- references can never be null

## Mutable reference (Exclusive reference)

Mutable reference, also known as exclusive references, `&mut T`, allow changing the value they refer to.

```rust
let mut point = (1, 2);
let x_coord = &mut point.0;
*x_coord = 20;
dbg!(x_coord);
```

Mutable references have one big restriction: *If you have a mutable reference to a value, you can have no other references to that value.* That why they are exclusive.

## Slice

Slices let you *reference* a contiguous sequence of elements in a collection.

A slice is a kind of reference, so it does not have ownership.

```rust
let a: [i32; 6] = [10, 20, 30, 40, 50, 60];
println!("a: {a:?}");

let s: &[i32] = &a[2..4];
println!("s: {s:?}");
```

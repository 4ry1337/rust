---
tags: [rust, patterns, references]
---

# Reference Pattern

A **reference pattern** uses `&` or `&mut` on the *left side* of a binding to destructure a reference — the inverse of using `&` on the right side to construct one. `&` plays two roles depending on which side of `=` it appears on:

- **Value side** (right of `=`): builds a reference. Like C's `&`.
- **Pattern side** (left of `=`, in `match`, function params, `for`, `if let`): unwraps a reference.

```rust
let x: i32 = 5;
let r: &i32 = &x;   // & as operator: build reference
let &y = r;         // & as pattern: unwrap reference, y: i32
```

The pattern shape mirrors the type shape. Type `&i32` → pattern `&name`.

## Why the binding is a copy, not an alias

A pattern destructures shape and binds pieces as fresh locals. Unwrapping a reference means producing the value behind it as a standalone binding — which requires either **copying** (for `Copy` types) or **moving** (when you own the source).

```rust
let mut x = 5;
let r: &mut i32 = &mut x;
let &mut mut y = r;
y = 99;
// x is still 5 — y is a separate local
```

To mutate the original, don't destructure. Keep the reference and write through it with `*`:

```rust
*r = 99;   // x is now 99
```

## When it compiles

The rule is just normal binding rules applied to the unwrapped value:

| Reference | Pointee | Works? |
|---|---|---|
| `&T` | `T: Copy` | ✓ binds by copy |
| `&mut T` | `T: Copy` | ✓ binds by copy (write-through lost) |
| `&T` or `&mut T` | `T: !Copy` | ✗ can't move out from behind borrow |
| `&T` | `T: !Sized` (`str`, `[U]`, `dyn Trait`) | ✗ can't have unsized local |

The errors aren't about reference patterns per se — they're about whether the resulting binding is legal. Locals must be `Sized`; non-`Copy` values can't be moved from behind a borrow.

## Three identical contexts

The same pattern works anywhere patterns appear:

```rust
// let
let &a = some_ref;                          // a: T

// for loop
for &b in &vec_of_int { /* b: i32 */ }

// if let
if let Some(&c) = option_of_ref { /* c: T */ }
```

In each case the right side yields `&T`, the pattern `&name` peels the reference, and the binding is `T`.

## Common idiom: iterating with reference patterns

`for x in &vec` yields `&T` because `IntoIterator for &Vec<T>` has `type Item = &T`. The `&` pattern gives back values:

```rust
fn sum(p: &[i32]) -> i32 {
    let mut total = 0;
    for &i in p {       // i: i32, copied
        total += i;
    }
    total
}
```

For non-`Copy` elements, drop the `&` and work through references:

```rust
fn print_all(p: &[String]) {
    for s in p {        // s: &String
        println!("{s}");
    }
}
```

## The `&mut` pattern trap

`&mut x` as a pattern is almost always a bug. It throws away the write capability:

```rust
// Doesn't mutate v — each x is a fresh copy
for &mut x in v.iter_mut() { /* ... */ }

// Mutates v — x is &mut i32, *x writes through
for x in v.iter_mut() { *x *= 10; }
```

## The escape hatch: `ref`

`ref` is the inverse on the pattern side — it *creates* a reference at binding time instead of consuming one. Useful in nested patterns where you want to peel the outer `&` but keep inner pieces borrowed:

```rust
let pair: &(String, i32) = &("hi".into(), 5);
let &(ref s, n) = pair;   // s: &String (no move), n: i32 (copy)
```

## Match ergonomics

Since Rust 2018, you can often omit the `&` pattern and the compiler inserts it ([RFC 2005](https://rust-lang.github.io/rfcs/2005-match-ergonomics.html)):

```rust
for (a, b) in pairs_ref.iter() {
    // a, b are &T each — ergonomics inserted the & pattern implicitly
}
```

Explicit `&` patterns disable ergonomics and give you values directly.

## Mental model

> A reference pattern is a **destructuring read**. It peels one layer of reference and produces a fresh local. After that point, the binding obeys ordinary ownership rules — `Sized` to live on the stack, `Copy` or owned to exist at all.

If you want to write through the reference, don't pattern-match it. Keep it as a reference and use `*` on the value side.

## References

- [The Rust Reference — Reference patterns](https://doc.rust-lang.org/reference/patterns.html#reference-patterns)
- [The Rust Reference — Patterns (grammar)](https://doc.rust-lang.org/reference/patterns.html)
- [The Rust Book §18.3 — Pattern syntax](https://doc.rust-lang.org/book/ch18-03-pattern-syntax.html)
- [RFC 2005 — Match ergonomics](https://rust-lang.github.io/rfcs/2005-match-ergonomics.html)
- [`std::ops::Deref` and auto-deref](https://doc.rust-lang.org/std/ops/trait.Deref.html)

## Related

- [[ownership-and-borrowing]]
- [[copy-vs-clone]]
- [[match-ergonomics]]
- [[sized-trait]]

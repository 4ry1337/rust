---
related: "[[Rust]]"
---
# Closures in APIs

There is a subtrait relationship `Fn` : `FnMut` : `FnOnce`. That means a closure that implements `Fn` also implements `FnMut` and `FnOnce`. Likewise a closure that implements `FnMut` also implements `FnOnce`.

## From a call site perspective

| Signature | Function `g` can call ... | Function `g` accepts ... |
| --- | --- | --- |
| `g<F: FnOnce()>(f: F)` | `f()` at most once. | `Fn`, `FnMut`, `FnOnce` |
| `g<F: FnMut()>(mut f: F)` | `f()` multiple times. | `Fn`, `FnMut` |
| `g<F: Fn()>(f: F)` | `f()` multiple times. | `Fn` |

> Notice how **asking** for a `Fn` closure as a function is most restrictive for the caller; but **having** a `Fn` closure as a caller is most compatible with any function.

## From the perspective of someone defining a closure

| Closure | Implements [^1] | Comment |
| --- | --- | --- |
| `\|\| { moved_s; }` | `FnOnce` | Caller must give up ownership of `moved_s`. |
| `\|\| { &mut s; }` | `FnOnce`, `FnMut` | Allows `g()` to change caller's local state `s`. |
| `\|\| { &s; }` | `FnOnce`, `FnMut`, `Fn` | May not mutate state; but can share and reuse `s`. |

[^1]: Rust [prefers capturing](https://doc.rust-lang.org/stable/reference/expressions/closure-expr.html) by reference (resulting in the most "compatible" `Fn` closures from a caller perspective), but can be forced to capture its environment by copy or move via the `move || {}` syntax.

## Advantages and disadvantages

| Requiring | Advantage | Disadvantage |
| --- | --- | --- |
| `F: FnOnce` | Easy to satisfy as caller. | Single use only, `g()` may call `f()` just once. |
| `F: FnMut` | Allows `g()` to change caller state. | Caller may not reuse captures during `g()`. |
| `F: Fn` | Many can exist at the same time. | Hardest to produce for caller. |

---
Source: [cheats.rs - Coding Guides](https://cheats.rs/#coding-guides)

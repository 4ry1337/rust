---
related: "[[Rust]]"
---
# Adversarial Code

*Adversarial* code is *safe* 3rd party code that compiles but does not follow API *expectations*, and might interfere with your own (safety) guarantees.

| You author | User code may possibly ... |
| --- | --- |
| `fn g<F: Fn()>(f: F) { … }` | Unexpectedly panic. |
| `struct S<X: T> { … }` | Implement `T` badly, e.g., misuse `Deref`, ... |
| `macro_rules! m { … }` | Do all of the above; call site can have *weird* scope. |

## Risk patterns

| Risk Pattern | Description |
| --- | --- |
| `#[repr(packed)]` | Packed alignment can make reference `&s.x` invalid. |
| `impl std::… for S {}` | Any trait `impl`, esp. `std::ops`, may be broken. In particular ... |
| `impl Deref for S {}` | May randomly `Deref`, e.g., `s.x != s.x`, or panic. |
| `impl PartialEq for S {}` | May violate equality rules; panic. |
| `impl Eq for S {}` | May cause `s != s`; panic; must not use `s` in `HashMap` & co. |
| `impl Hash for S {}` | May violate hashing rules; panic; must not use `s` in `HashMap` & co. |
| `impl Ord for S {}` | May violate ordering rules; panic; must not use `s` in `BTreeMap` & co. |
| `impl Index for S {}` | May randomly index, e.g., `s[x] != s[x]`; panic. |
| `impl Drop for S {}` | May run code or panic at end of scope `{}`, during assignment `s = new_s`. |
| `panic!()` | User code can panic *any* time, resulting in abort or unwind. |
| `catch_unwind(|| s.f(panicky))` | Also, caller might force observation of broken state in `s`. |
| `let … = f();` | Variable name can affect order of `Drop` execution. [^1] |

[^1]: Notably, when you rename a variable from `_x` to `_` you will also change `Drop` behavior since you change semantics. A variable named `_x` will have `Drop::drop()` executed at the end of its scope, a variable named `_` can have it executed immediately on "apparent" assignment ("apparent" because a binding named `_` means **wildcard** *discard this*, which will happen as soon as feasible, often right away)!

> [!NOTE] Implications
> - Generic code **cannot be safe if safety depends on type cooperation** w.r.t. most (`std::`) traits.
> - If type cooperation is needed you must use `unsafe` traits (probably implementing your own).
> - You must consider random code execution at unexpected places (e.g., re-assignments, scope end).
> - You may still be observable after a worst-case panic.
>
> As a corollary, *safe*-but-deadly code (e.g., `airplane_speed<T>()`) should probably also follow these guides.

---
Source: [cheats.rs - Coding Guides](https://cheats.rs/#coding-guides)

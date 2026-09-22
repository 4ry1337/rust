---
related: "[[Rust]]"
---
# Idiomatic Rust

If you are used to Java or C, consider these.

| Idiom | Code |
| --- | --- |
| **Think in Expressions** | `y = if x { a } else { b };` |
| | `y = loop { break 5 };` |
| | `fn f() -> u32 { 0 }` |
| **Think in Iterators** | `(1..10).map(f).collect()` |
| | `names.iter().filter(|x| x.starts_with("A"))` |
| **Test Absence with `?`** | `y = try_something()?;` |
| | `get_option()?.run()?` |
| **Use Strong Types** | `enum E { Invalid, Valid { … } }` over `ERROR_INVALID = -1` |
| | `enum E { Visible, Hidden }` over `visible: bool` |
| | `struct Charge(f32)` over `f32` |
| **Illegal State: Impossible** | `my_lock.write().unwrap().guaranteed_at_compile_time_to_be_locked = 10;` [^1] |
| | `thread::scope(|s| { /* Threads can't exist longer than scope() */ });` |
| **Avoid *Global* State** | Being depended on in multiple versions can secretly duplicate statics. |
| **Provide Builders** | `Car::builder().name("Model T").hp(20).build();` |
| **Make it Const** | Where possible mark fns. `const`; where feasible run code inside `const {}`. |
| **Don't Panic** | Panics are *not* exceptions, they suggest immediate process abortion! |
| | Only panic on programming error; use `Option<T>` or `Result<T,E>` otherwise. |
| | If clearly user requested, e.g., calling `obtain()` vs. `try_obtain()`, panic ok too. |
| | Inside `const { NonZero::new(1).unwrap() }` panic becomes compile error, ok too. |
| **Generics in Moderation** | A simple `<T: Bound>` (e.g., `AsRef<Path>`) can make your APIs nicer to use. |
| | Complex bounds make it impossible to follow. If in doubt don't be creative with generics. |
| **Split Implementations** | Generics like `Point<T>` can have separate `impl` per `T` for some specialization. |
| | `impl<T> Point<T> { /* Add common methods here */ }` |
| | `impl Point<f32> { /* Add methods only relevant for Point<f32> */ }` |
| **Unsafe** | Avoid `unsafe {}`, often safer, faster solution without it. |
| **Implement Traits** | `#[derive(Debug, Copy, …)]` and custom `impl` where needed. |
| **Tooling** | Run **clippy** regularly to significantly improve your code quality. |
| | Format your code with **rustfmt** for consistency. |
| | Add **unit tests** (`#[test]`) to ensure your code works. |
| | Add **doc tests** (`` `my_api::f()` `` in doc comments) to ensure docs match code. |
| **Documentation** | Annotate your APIs with doc comments that can show up on [docs.rs](https://docs.rs). |
| | Don't forget to include a **summary sentence** and the **Examples** heading. |
| | If applicable: **Panics**, **Errors**, **Safety**, **Abort** and **Undefined Behavior**. |

[^1]: In most cases you should prefer `?` over `.unwrap()`. In the case of locks however the returned `PoisonError` signifies a panic in another thread, so unwrapping it (thus propagating the panic) is often the better idea.

> [!TIP]
> We highly recommend you also follow the [API Guidelines](https://rust-lang.github.io/api-guidelines/) and the [Pragmatic Rust Guidelines](https://microsoft.github.io/rust-guidelines/).

---
Source: [cheats.rs - Coding Guides](https://cheats.rs/#coding-guides)

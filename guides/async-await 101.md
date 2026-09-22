---
related: "[[Rust]]"
---
# Async-Await 101

If you are familiar with async / await in C# or TypeScript, here are some things to keep in mind.

## Basics

| Construct | Explanation |
| --- | --- |
| `async` | Anything declared `async` always returns an `impl Future<Output=_>`. |
| `async fn f() {}` | Function `f` returns an `impl Future<Output=()>`. |
| `async fn f() -> S {}` | Function `f` returns an `impl Future<Output=S>`. |
| `async { x }` | Transforms `{ x }` into an `impl Future<Output=X>`. |
| `let sm = f();` | Calling `f()` that is `async` will **not** execute `f`, but produce state machine `sm`. [^1] [^2] |
| `sm = async { g() };` | Likewise, does **not** execute the `{ g() }` block; produces a state machine. |
| `runtime.block_on(sm);` | Outside an `async {}`, schedules `sm` to actually run. Would execute `g()`. [^3] [^4] |
| `sm.await` | Inside an `async {}`, run `sm` until complete. Yields to runtime if `sm` not ready. |

[^1]: Technically `async` transforms the following code into an anonymous, compiler-generated state machine type; `f()` instantiates that machine.
[^2]: The state machine always `impl Future`, possibly `Send` & co, depending on types used inside `async`.
[^3]: The state machine is driven by a worker thread invoking `Future::poll()` via the runtime directly, or a parent `.await` indirectly.
[^4]: Rust doesn't come with a runtime, you need an external crate instead, e.g., [tokio](https://crates.io/crates/tokio). Also, more helpers in the [futures crate](https://github.com/rust-lang-nursery/futures-rs).

## Execution Flow

At each `x.await`, the state machine passes control to the subordinate state machine `x`. At some point a low-level state machine invoked via `.await` might not be ready. In that case the worker thread returns all the way up to the runtime so it can drive another `Future`. Some time later the runtime:

- **might** resume execution. It usually does, unless `sm` / `Future` was dropped.
- **might** resume with the previous worker **or another** worker thread (depends on the runtime).

Simplified diagram for code written inside an `async` block:

```
       consecutive_code();           consecutive_code();           consecutive_code();
START --------------------> x.await --------------------> y.await --------------------> READY
// ^                          ^     ^                               Future<Output=X> ready -^
// Invoked via runtime        |     |
// or an external .await      |     This might resume on another thread (next best available),
//                             |     or NOT AT ALL if Future was dropped.
//                             |
//                             Execute `x`. If ready: just continue execution; if not, return
//                             this thread to runtime.
```

## Caveats

With the execution flow in mind, some considerations when writing code inside an `async` construct:

| Constructs [^5] | Explanation |
| --- | --- |
| `sleep_or_block();` | Definitely bad, never halt the current thread, it clogs the executor. |
| `set_TL(a); x.await; TL();` | Definitely bad, `await` may return on another thread, thread local invalid. |
| `s.no(); x.await; s.go();` | Maybe bad, `await` will not return if `Future` is dropped while waiting. [^6] |
| `Rc::new(); x.await; rc();` | Non-`Send` types prevent `impl Future` from being `Send`; less compatible. |

[^5]: Here `s` is any non-local that could temporarily be put into an invalid state; `TL` is any thread local storage, and the `async {}` containing the code is written without assuming executor specifics.
[^6]: Since [`Drop`](https://doc.rust-lang.org/std/ops/trait.Drop.html) is run in any case when a `Future` is dropped, consider using a drop guard that cleans up / fixes application state if it has to be left in a bad condition across `.await` points.

---
Source: [cheats.rs - Coding Guides](https://cheats.rs/#coding-guides)

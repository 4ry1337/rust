---
related: "[[Rust]]"
---
# Performance Tips

"My code is slow" sometimes comes up when porting microbenchmarks to Rust, or after profiling.

| Rating | Name | Description |
| --- | --- | --- |
| 🚀🍼 | **Release Mode** | Always do `cargo build --release` for massive speed boost. |
| 🍼⚠️ | **Target Native CPU** | Add `rustflags = ["-Ctarget-cpu=native"]` to `config.toml`. |
| 🍼⚖️ | **Codegen Units** | Codegen units `1` may yield faster code, slower compile. |
| 🍼 | **Reserve Capacity** | Pre-allocation of collections reduces allocation pressure. |
| 🍼 | **Recycle Collections** | Calling `x.clear()` and reusing `x` prevents allocations. |
| 🍼 | **Append to Strings** | Using `write!(&mut s, "{}")` can prevent extra allocation. |
| 🍼⚖️ | **Global Allocator** | On some platforms an external allocator (e.g., **mimalloc**) is faster. |
| | **Bump Allocations** | Cheaply gets *temporary*, dynamic memory, esp. in hot loops. |
| | **Batch APIs** | Design APIs to handle multiple similar elements at once, e.g., slices. |
| 🚀🚀⚖️ | **SoA** / **AoSoA** | Beyond that consider *struct of arrays* (SoA) and similar. |
| 🚀⚖️ | **SIMD** (nightly) | Inside (math heavy) batch APIs using SIMD can give 2x - 8x boost. |
| | **Reduce Data Size** | Small types (e.g., `u8` vs `u32`, niches) and data have better cache use. |
| | **Keep Data Nearby** | Storing often-used data *nearby* can improve memory access times. |
| | **Pass by Size** | Small (2-3 words) structs best passed by value, larger by reference. |
| 🚀🚀⚖️ | **Async-Await** | If *parallel waiting* happens a lot (e.g., server I/O) `async` is a good idea. |
| | **Threading** | Threads allow you to perform *parallel work* on multiple items at once. |
| 🚀 | ... in app | Often good for apps, as lower wait times means better UX. |
| 🚀🚀⚖️ | ... inside libs | Opaque threading use *inside* a lib is often not a good idea, can be too opinionated. |
| 🚀 | ... for lib callers | However, allowing *your user* to process *you* in parallel is an excellent idea. |
| 🚀🚀⚖️ | **Avoid Locks** | Locks in multi-threaded code kill parallelism. |
| 🚀🚀⚖️ | **Avoid Atomics** | Needless atomics (e.g., `Arc` vs `Rc`) impact other memory access. |
| 🚀🚀⚖️ | **Avoid False Sharing** | Make sure data read/written by different CPUs is at least 64 bytes apart. |
| 🚀🍼 | **Buffered I/O** | Raw `File` I/O is highly inefficient without buffering. |
| 🍼⚠️ | **Faster Hasher** | Default `HashMap` hasher is DoS attack-resilient but slow. |
| 🍼⚠️ | **Faster RNG** | If you use a crypto RNG, consider swapping for a non-crypto one. |
| 🚀🚀⚖️ | **Avoid Trait Objects** | Trait objects reduce code size, but increase memory indirection. |
| 🚀🚀⚖️ | **Defer Drop** | Dropping *heavy* objects in a dump-thread can free up the current one. |
| 🍼⚠️ | **Unchecked APIs** | If you are 100% confident, `unsafe { unchecked_… }` skips checks. |

> Legend: 🚀 often comes with a massive (>2x) performance boost · 🍼 easy to implement even after-the-fact · ⚖️ might have costly side effects (e.g., memory, complexity) · ⚠️ has special risks (e.g., security, correctness).

> [!TIP] Profiling Tips
> Profilers are indispensable to identify hot spots in code. For the best experience add this to your `Cargo.toml`:
> ```toml
> [profile.release]
> debug = true
> ```
> Then do a `cargo build --release` and run the result with [Superluminal](https://superluminal.eu/rust/) (Windows) or [Instruments](https://en.wikipedia.org/wiki/Instruments_%28software%29) (macOS).
> That said, there are many performance opportunities profilers won't find, but that need to be *designed in*.

---
Source: [cheats.rs - Coding Guides](https://cheats.rs/#coding-guides)

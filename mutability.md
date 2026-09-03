# Shareable Mutable Containers

Sometimes multiple references to an object must still mutate it — the [[6. pointers and borrowing#Rules of References|exclusivity rule]] is too strict for this.

*Shareable mutable containers* permit controlled mutability despite aliasing.

- Multi-threaded: [[19. synchronization#Mutex|Mutex<T>]], [[19. synchronization#RwLock|RwLock<T>]], [[19. synchronization#OnceLock|OnceLock<T>]], [[19. synchronization#Atomics|atomic types]]
- Single-threaded (no `Sync`): `Cell<T>`, `RefCell<T>`, `OnceCell<T>` — provide *interior mutability*

^interior-mutability



[^docs]: https://doc.rust-lang.org/std/cell/index.html
[^boook-cell]: https://doc.rust-lang.org/core/cell/index.html

# Shareable Mutable Containers

Sometimes multiple references to an object must still mutate it — the [[6. pointers and borrowing#Rules of References|exclusivity rule]] is too strict for this.

*Shareable mutable containers* exist to permit mutability in a controlled manner, even in the presence of aliasing.

[[19. synchronization#Mutex|Mutex<T>]], [[19. synchronization#RwLock|RwLock<T>]], [[19. synchronization#OnceLock|OnceLock<T>]] or [[19. synchronization#Atomics|atomic types]] allow doing this among multiple threads 

Cell<T>, RefCell<T>, and OnceCell<T> allow doing this in a single-threaded way—they do not implement Sync. Cell types provide *interior mutability*

^interior-mutability



[^docs]: https://doc.rust-lang.org/std/cell/index.html
[^boook-cell]: https://doc.rust-lang.org/core/cell/index.html

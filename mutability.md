# Shareable Mutable Containers

Sometimes multiple references to an object must still mutate it — the [[6. pointers and borrowing#Rules of References|exclusivity rule]] is too strict for this.

*Shareable mutable containers* permit controlled mutability despite aliasing.

- Multi-threaded: [[19. synchronization#Mutex|Mutex<T>]], [[19. synchronization#RwLock|RwLock<T>]], [[19. synchronization#OnceLock|OnceLock<T>]], [[19. synchronization#Atomics|atomic types]]
- Single-threaded (no `Sync`): `Cell<T>`, `RefCell<T>`, `OnceCell<T>` — provide *interior mutability*

^interior-mutability

## Cell<T>

- Implements interior mutability by moving values in/out — `&T` to the inner value cannot be obtained.
- Value cannot be directly obtained without replacing it with something else.

Methods:
- `get` — retrieves current value by copy (requires `T: Copy`)
- `take` — replaces current value with `Default::default()`, returns replaced value (requires `T: Default`)
- `replace` — replaces current value, returns replaced value
- `into_inner` — consumes the `Cell<T>`, returns interior value
- `set` — replaces interior value, drops the replaced value

> [!EXAMPLE]
> ```rust
> use std::cell::Cell;
> 
> fn main() {
>     let c = Cell::new(5);
>     let old = c.get();            // copy out: 5
>     c.set(10);                    // replace, drop old value
>     let replaced = c.replace(20); // returns 10, cell now holds 20
> 
>     println!("{old} {replaced} {}", c.get()); // 5 10 20
> 
>     let d: Cell<String> = Cell::new(String::from("hello"));
>     let taken = d.take();     // replaces with String::default() (""), returns "hello"
>     println!("{taken} / {}", d.into_inner()); // hello / (empty)
> }
> ```

Preferred over `RefCell` for small, cheap-to-copy types (e.g. numbers). `RefCell` better for larger/non-`Copy` types.

## RefCell<T>

- Implements "dynamic borrowing" — temporary, exclusive, mutable access via lifetimes.
- Borrows tracked at runtime, unlike native references (tracked statically at compile time).

Access:
- `borrow` — obtains `&T`
- `borrow_mut` — obtains `&mut T`
- Both verify borrow rules at call time: any number of immutable borrows, OR a single mutable borrow, never both.
- Violating the rule → thread panics.

> [!EXAMPLE]
> ```rust
> use std::cell::RefCell;
> 
> fn main() {
>     let c = RefCell::new(5);
>     {
>         let r1 = c.borrow();
>         let r2 = c.borrow(); // multiple immutable borrows OK
>         println!("{} {}", *r1, *r2);
>     } // borrows dropped here
> 
>     {
>         let mut m = c.borrow_mut();
>         *m += 10;
>     } // mutable borrow dropped here
> 
>     println!("{}", c.borrow()); // 15
> 
>     // let a = c.borrow();
>     // let b = c.borrow_mut(); // would panic: already borrowed
> }
> ```

`Sync` equivalent: [[19. synchronization#RwLock|RwLock<T>]].


[^docs]: https://doc.rust-lang.org/std/cell/index.html
[^boook-cell]: https://doc.rust-lang.org/core/cell/index.html

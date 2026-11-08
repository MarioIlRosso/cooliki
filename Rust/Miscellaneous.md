# Intro
These are facts that are interesting that either:
- Don't belong to a file currently
- Are too unimportant to put in a file

## Variable initialization
Variables can be initialized much later (even after the declaration) without making them mutable

Using variables that aren't yet initialized makes the compiler error with code [E0381](https://doc.rust-lang.org/stable/error_codes/E0381.html).
### Example
```rust
let x: String;
let y: u64;

// `x` and `y` don't need to be `mut`.
x = String::from("Hello, World!");
y = 5;
```

```rust
let x: bool;
function_that_takes_in_a_bool(x); // COMPILER ERROR!
x = true;
```
## Mutex poisoning
A mutex is poisoned when a thread that unlocked it panics before re-locking it, after a mutex was poisoned, it's not possible to access its contents anymore, as the mutex recognizes it's tained.
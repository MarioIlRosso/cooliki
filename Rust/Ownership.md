# Definition
Ownership is a way to avoid memory safety bugs in rust. For example use after free.
## Rules
Rust's ownership system has 3 rules.

**1.** Every value has an owner:

This is pretty simple.

For example:
```rust
// Here, `a` is the owner, and the string is the value
let a = String::new();
```

**2.** When an owner goes out of scope, its value is dropped

("dropped" means cleaned up, deallocated...)

**3.** Every value can only have one owner.
This is to avoid use-after-free bugs.

Because the owner of a value is responsible for dropping it, if there were two owners, a value could potentially be dropped multiple times.
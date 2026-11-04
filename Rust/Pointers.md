# Definition
A pointer in rust is generally similar to pointers in other languages.

A pointer points to a pointee (that can even point to other pointers), and allows to dereference it, and read/write it's value, in an `unsafe` context.
## Size
A size of a pointer is the same size of `usize`.
that's because pointers differ in size based on the current platform:

> [!warning] Title
> Rust guarantees `usize` to be the same size of a pointer in the target platform, but that doesn't mean that, for example, an `x64` target always has `u64` pointers, so the size of `usize` should be treated as platform dependant.
## Creating a pointer
You can create a pointer to any value in the same way [[References#Definition|references]] do it, except that the type annotation differs.
```rust
let x: u64 = 4;
let x_ptr: *const u64 = &x;
```

In this example:
- `*` means that we're declaring a pointer.
- `const` means that the pointee is immutable.
- `u64` is the type of the pointee.

A property of pointers is that they can be declared mutable, while pointing to an immutable value:
```rust
let x: u64 = 42;
let x_ptr: *mut u64 = &x as *const u64 as *mut u64;
```

instead of `const` we're using `mut` here, declaring that the pointer can mutate the pointee (even if `x` is immutable), and we're casting `&x` as an immutable pointer, and casting into a mutable one.
The previous snippet is completely safe rust, because the unsafe operation is actually dereferencing or reading and writing.

If we tried to do the same with references, it won't work:
```rust
let x: u64 = 42;
let x_ptr: &mut u64 = &x as &mut u64;
```

This is because an immutable reference can't be casted as a mutable one, this fails with error code [E0605](https://doc.rust-lang.org/stable/error_codes/E0605.html#error-code-e0605)

You should always prefer using a smart pointer instead of using raw pointers, unless it's needed.
## Dereferencing
A pointer can't be dereferenced in a safe context, the reasons are trivial:
- The compiler can't be certain that the pointer isn't dangling.
- Dereferencing might violate rust's aliasing rules.

```rust
let x: *const u64 = std::ptr::null();
let x_ptr: *mut *const u64 = &x as *const *const u64 as *mut *const u64;

let y: u64 = 0;
let y_ptr: *const u64 = &y;

unsafe {
    *x_ptr = y_ptr;
}
```

This will fail to compile, as rust notices that we're actually dereferencing the pointer, and can't know that it's safe or not.
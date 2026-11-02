# Definition
A reference is *usually* a pointer to a value (see [[References#Dynamically Sized Types|DSTs]] for more information).
All references in rust have a [[Lifetimes|Lifetime]].

A size of a reference is the size of `usize`, that's because pointers differ in size based on the current platform: On a `64x` target, pointers will be 8 bytes wide (64 bits), while on a `32x` target, they will be 4 bytes wide (32 bits), other architectures may differ.
## Invariants
Rust references are never dangling, and can't be null.
## Examples
```rust
let x: u32 = 5;
let reference: &u32 = &x;
```
# Mutability
## Immutable reference
Also known as a shared reference.

An immutable reference is a read-only view of a value, that doesn't [[Ownership|own it]].
Any value can be referenced as an immutable reference, even references themselves (see [[Common Misconceptions#`T` is always owned, `&T` is always a reference, `&mut T` is always a mutable reference|this]] for an example)

Immutable references can't mutate the value they're referencing.

To create an immutable reference, use the `&` operator behind a value.
## Mutable reference
Also known as exclusive references.

A mutable reference is a mutable view of a value, that doesn't [[Ownership|own it]].
Any mutable value can be referenced as a mutable reference.

To create a mutable reference, use the `&` operator with the `mut` modifier.
### Other
Rust allows declaring a mutable and immutable reference to a value, but if they're actively used at the same time, the compiler will return error [E0502](https://doc.rust-lang.org/stable/error_codes/E0502.html).

You can only create a mutable reference to a value if that value is mutable, else, the compiler will return error [E0596](https://doc.rust-lang.org/stable/error_codes/E0596.html).
# Dynamically Sized Types
References (specifically pointers) are also used to work with `DSTs`.
## Size
In the special cases of references to `DSTs`, instead of being pointers, the references are fat pointers, holding the pointer to the value and its length.

a dynamically sized type is a type that has no stable size.
## Examples
String views (`str`), Slices (`[T]`), and trait objects are all `DSTs`
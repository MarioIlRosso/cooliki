# Definition
Higher ranked trait bounds (commonly shortened to HRTB), is a way to make a trait bound valid for every lifetime.
## Example
```rust
for <'a> F: Fn(&'a str) -> &'a usize,
```
would roughly mean:
"F is a function that takes in a `str` behind a reference, of **every** lifetime, and outputs an `usize` behind a reference, of the same lifetime as the input"

This effectively would *hypothetically* generate a list of infinite lifetimes, and makes `F` work for every lifetime.

But the rust compiler doesn't actually generate infinite lifetimes, instead it just proves that every possible lifetime works, using a set of constraint, and proves that there's a solution.
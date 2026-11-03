This is a group of common misconceptions that get addressed. A misconception could be a summary of another article (even external), and if so, it will be linked.
## `T` is always owned, `&T` is always a reference, `&mut T` is always a mutable reference
`T` can actually be defined as a set.

let `Ref` be a set of all possible references to `T`:
`Ref = { &T, &&T, ..., &&mut T, ... }`

let `MutRef` be a set of all possible mutable references to `T`:
`MutRef = { &mut T, &mut &mut T, ..., &mut &T, ... }`

`Ref != MutRef`

`T` is a set, containing `Ref` and `MutRef`.

`Ref contains Ref, MutRef`
#### [Reference](https://github.com/pretzelhammer/rust-blog/blob/master/posts/common-rust-lifetime-misconceptions.md#1-t-only-contains-owned-types)
From "Common Rust Lifetime Misconceptions"

## `&'a T` == `T: 'a`
While `&'a T` means "a value `T` with lifetime `'a`", `T: 'a'` means "a value `T` that can live at least as long as `'a`".

Although they're not equal, `&'a T` implies that `T: 'a`, that is the case because "`T` is of lifetime `'a`" and "`T` lives as long as `'a`" implying each other.
#### [Reference](https://github.com/pretzelhammer/rust-blog/blob/master/posts/common-rust-lifetime-misconceptions.md#3-a-t-and-t-a-are-the-same-thing)
From "Common Rust Lifetime Misconceptions"
## If statements
If statements are expressions, in rust, if a piece of code returns a value, it's deemed an "expression".

So saying "if statement" is technically incorrect
# Definition
In Rust, a lifetime is a way for the borrow checker to understand the lifetime of a reference at compile-time, making sure that a reference can never outlive the lifetime of the value it's referencing.
## Examples
Suppose we have this function in rust, a lifetime is the period of time between the creation of the borrow of the value, and it's destruction.

```rust
fn pretty_print(s: &str) {
	// Here, &s starts.
	println!("This is value {}", s);
	// Here, &s ends.
}
```

Lifetimes in rust are denoted as an apostrophe, and are usually followed by a lowercase ASCII letter.
e.g: `<'a>`, `<'b>`, `<'g>`, `<'d>`,

**NOTE**: This naming convention is not enforced by the compiler.

In the previous example, we didn't have to use lifetimes, as the compiler can understand the lifetime of `&s` automatically, this is called lifetime elision.

An example of actual usage of lifetimes is the following:
```rust
struct Data<'a> {
	string: &'a str,
	name: &'a str,
}
```

Here we need to specify the lifetimes, as the compiler can't automatically infer them.

In the struct `Data`, we are telling the compiler that `Data` should live as long as the references of fields `string` and `name` (basically, `Data` can't outlive `'a`).

This means that if any of the fields inside `Data` are destroyed, it would violate the lifetime contract the compiler imposed, as the lifetime of the destroyed field is shorter than the lifetime of `Data`.
# Relationship between lifetimes
Sometimes, there can be more than one lifetime, let's analyze this struct:

```rust
struct Pair<'a, 'b> {
	left: &'a str,
	right: &'b str,
}
```

This is essentially telling the compiler that `Pair` assumes two lifetimes: `'a` and `'b`.
This however, doesn't guarantee the two lifetimes will be *equal*.

`'a` here could outlive `'b`, and it would still be valid, as we only require `'a` and `'b` to live as long as `Pair`.

When we say that a lifetime `'a` outlives lifetime `'b`, it's written as
$$
'a: 'b
$$
## Variance
Variance is a way to express the relationship between two types.
### Covariance
Let `T` be any type.

Automatically, `&'a T` becomes covariant over `'a`.

That is to say, if `'a` can be replaced by a longer lifetime and shrink down to suit `'a`, then `&'a T` can also be replaced by said lifetime.

For example, given two lifetimes (`'long` and `'short`), because we can shrink `'long` down to suit `'short`, this means that any reference with any type that has the lifetime `'long` can also shrink down to suit `'short`.

In short, whenever the word "covariant" is used, it simply means "`&'a T`'s lifetime can be shrinked like lifetime `'a` can"

```rust
'long: 'short
`&'long T: &'short T`
```
### Invariance
Let `T` be any type.

Automatically, `&mut 'a T` becomes invariant over the lifetime of `'a`. We can't modify the lifetime `'a` in anyway (for example, using covariance to shrink it).

For understanding why this is the case, let's implement a function
```rust
fn assign<T>(old_value: &mut T, new_value: T) {
	*old_value = new_value;
}

fn main() {
    let mut hello: &'static str = "hello";
    let world = String::from("world");
    assign(&mut hello, &world);
    println!("{hello}");
}

/// This code was inspired by the rustonomicon (Chapter 3.8)
```

This won't compile, the problem is that we are telling the compiler that `hello` is of a `'static` lifetime, and then we assign another variable which is `world`.

Then we pass a reference of both `hello` and `world` into `assign`.

The problem is that, because `'static: 'world`, this means that `'static` would outlive world, and so we're assigning a shorter lifetime to a `'static` one.

Because any mutable reference with lifetime `'a` is invariant over `'a`, this is a problem, for example, if we did assign `hello` to the lifetime of `world`, then this means that it would be dropped way before, that isn't what `'static` promises.

In short, whenever the word "invariant" is used, it simply means "`&mut 'a T`'s lifetime can't be shrinked like lifetime `'a` can"
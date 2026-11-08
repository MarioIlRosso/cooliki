# Definition
In Rust, a lifetime is a way for the borrow checker to understand the lifetime of a reference at compile-time, making sure that a reference can never outlive the lifetime of the value it's referencing.
## Examples
Suppose we have this function in rust, a lifetime is the period of time between the creation of the borrow of the value, and it's destruction.

```rust
fn pretty_print(s: &str) {
	println!("This is value {}", s);
}

// With lifetimes (expanded)
fn pretty_print<'a>(s: &'a str) {
	// Here, 'a starts.
	println!("This is value {}", s);
	// Here, 'a ends.
}
```

Lifetimes in rust are denoted as an apostrophe, and are usually followed by a lowercase ASCII letter.
e.g: `<'a>`, `<'b>`, `<'g>`, `<'d>`,

**NOTE**: This naming convention is not enforced by the compiler.

In the previous example, we didn't have to use lifetimes, as the compiler can understand the lifetime of `&s` automatically, this is called lifetime elision.
# Lifetime elision
Most of the time, we don't need to annotate lifetimes in functions, this is because the rust compiler can infer them, it uses a simple process:

**1.** Every input that is a reference gets an unique lifetime
**2.** If there's one input lifetime, it gets applied to all outputs
**3.** If there are multiple input lifetimes, but one of them is `&self` or `&mut self`, then the lifetime of `&self` is applied to all outputs
**4.** Lifetime elision can't infer lifetimes, and they need to be annotated.
# Lifetimes in structs
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
## Subtyping
To understand the following concepts, we first need to define what subtyping is:

> [!warning] Warning
> The following section assumes a Rust context, but does use math.

Subtyping is a way to define a relation ship between lifetimes in rust.
We can define all lifetimes of a program as a set, containing all lifetimes of the minimum value, containing more sets with larger value, which includes a set `'static`, containing the lifetimes with the largest amount of lifetime.

All lifetimes in rust follow this simple property:
lets define `'long` and `'short` as two lifetimes.
if `'long <: 'short`, then you can replace `'short` with `'long`, but not the other way.

`'long <: 'short` means that `'long` is a subtype of `'short`, that is to say, `'long` is contained in set `'short`.

Note that the `<:` symbol doesn't define the size of a lifetime here by itself, but it defines the relationship in the set.
## Variance
Before this point, we only talked about lifetimes, but lifetimes are always related to some type `T` that is a reference or pointer.

> [!warning] Warning
> This section requires reading [[Common Misconceptions#`T` is always owned, `&T` is always a reference, `&mut T` is always a mutable reference|this]]

Variance, in rust, defines the relationship of two types (a sub type, and a super type), when inside a container.

Let's define `T` and `U` as types, and we'll also define a container type `F` that contains these two.

### Covariance
If the relationship between `T` and `U` is preserved when both are contained in `F`, then we say that `F<T>` is covariant over `U` (or `F<T>` is covariant with respect of `U`).

For example, here we use a reference as a container:
```rust
'long: 'short (or 'long <: 'short)
&'long T: &'short T (or &'long T <: &'short T)
```

because `&T` is covariant over `T` (and by extension `&T`), the subtyping of the lifetimes is also valid when they're behind an immutable reference.

The definition of covariant is, simply put: "`F<T>` is covariant over `F<U>` when the relationship of `T` and `U` is preserved."

### Examples of covariance
`&T`, `Vec<T>`, and `Box<T>` are all covariant over `T`.
## Invariance
If there's a relationship between `T` and `U`, but it's not preserved when both are contained in `F`, and there's no relationship between `F<T>` and `F<U>`, then we say that `F<T>` is invariant over `U` (or `F<T>` is invariant with respect of `U`).

For example, here we use a mutable reference as a container:
```rust
'long: 'short (or 'long <: 'short)
// "!:" is the reverse of ":"
// "!<:" means "not in subtype" (reverse of "<:")
&'long T !: &'short T (or &'long T !<: &'short T)
```

The definition of covariant is, simply put: "`F<T>` is covariant over `F<U>` when the relationship of `T` and `U` isn't preserved."
# Reference
> [!cite] Credits
> Thanks to [zacryol](https://discord.com/users/693815604368506911), [harudagondi](https://discord.com/users/599833361565089802), and many others for helping me understand lifetimes.
# Definition
Conversions happen when you need to translate a type into another.
for example, converting an `i32` into an `i64`.

There are various type of conversions:
## Coercions
The safest and implicit type of conversions are **coercions**.

a coercion is simply a harmless way to translate a type into another.

The reason coercions exist is not have to manually convert trivial things.
### Example
an example is coercing a mutable reference:
`&mut T => &T`

**NOTE**: Coercions aren't necessarily bijective, this means that they aren't always two way conversions, with the same example, you *can't* transform a `&T` into a `&mut T`.

Another trivial example of a coercion is `T` into a `T`, an *identity coercion*.

To get a full list of all coercions, [click here](https://doc.rust-lang.org/reference/type-coercions.html#coercion-types)
## Casts
Casting is another way to reinterpret bits into another type.

To cast, simply follow this syntax:
`[value] as [type]`

A common type of cast is converting an `i32` into a `i64`.
```
let num: i32 = 10;
let bigger_num: i64 = num as i64;
```

**NOTE**: Casting is *not* always safe, for example, casting an `i64` into an `i32` could lead to unexpected results, as it could truncate data.
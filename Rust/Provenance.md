# Definition
[[Pointers|Pointer]] provenance (or simply provenance for the rest of the file), is a compiler-only concept, that is useful for statistically optimizing a program.

In a low-level perspective, pointers are just an `usize` that represents a memory address.
But in compilers, and optimizers, pointers also have some extra data called provenance.

An example of provenance would also be storing the allocation of the pointer, and it's offset based on the allocation.
# Reference
> [!cite] Credits
> All the information here is mostly taken from:
> - [Pointers Are Complicated, or: What's in a Byte?](https://www.ralfj.de/blog/2018/07/24/pointers-and-bytes.html)
> - [Pointers Are Complicated II, or: We need better language specs](https://www.ralfj.de/blog/2020/12/14/provenance.html)
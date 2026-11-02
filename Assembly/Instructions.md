> [!warning]
> **WARNING**: This document is specifically for the `x86-64` architecture.

> [!todo] Work in progress
> This file is a work in progress, don't expect everything to be correct.
# Definition
An assembly instruction is a direct way to tell the CPU what to do.

[[Instructions#List of instructions|List of x86_64 instructions]].

An instruction is composed of a mnemonic and it's arguments (being a maximum of 4), arguments are separated with commas:
`instr op, op, op, op`

An assembly instruction can be written in any casing.
## Example
```asm
mov rax, 42
```
# List of instructions

|  Name  |  Arg 1  |  Arg 2  |  Arg 3  | Arg 4 | Documentation                                                                                                                                                                            |
| :----: | :-----: | :-----: | :-----: | :---: | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `mov`  |  `dst`  |  `src`  |    -    |   -   | Copies the contents of `src` into `dst`                                                                                                                                                  |
| `add`  | `base`  |  `add`  |    -    |   -   | Equivalent to a high-level `base += add`                                                                                                                                                 |
| `sub`  | `base`  |  `sub`  |    -    |   -   | Equivalent to a high-level `base -= sub`                                                                                                                                                 |
| `mul`  | `base`  |    -    |    -    |   -   | Equivalent to a high-level `base *= RAX/EAX/AX/AL`. This only works for unsigned numbers, if overflowing, sets the carry flag                                                            |
| `imul` | `base`  |    -    |    -    |   -   | Equivalent to a high-level `base *= RAX/EAX/AX/AL`. This only works for signed numbers, if overflowing, sets the carry flag                                                              |
| `imul` | `base`  |  `mul`  |    -    |   -   | Equivalent to a high-level `base *= mul`. This only works for signed numbers, if overflowing, sets the carry flag                                                                        |
| `imul` |  `dst`  | `base`  | `const` |   -   | Equivalent to a high-level `dst = base * const`. This only works for signed numbers, if overflowing, sets the carry flag                                                                 |
| `shl`  | `base`  | `shift` |    -    |   -   | Moves all bits `shift`-step left. (`x << shift`)                                                                                                                                         |
| `div`  | `base`  |    -    |    -    |   -   | Equivalent to a high-level `base /= RAX/EAX/AX/AL`. This only works for unsigned numbers. Stores the quotient in `EAX`, and the remainder in `EDX`., if overflowing, sets the carry flag |
| `idiv` |  todo   |  todo   |  todo   | todo  | todo                                                                                                                                                                                     |
| `shr`  | `base`  | `shift` |    -    |   -   | Moves all bits `shift`-step right. (`x >> shift`)                                                                                                                                        |
| `jmp`  | `label` |    -    |    -    |   -   | Sets `rip` to the address of `label`                                                                                                                                                     |
| `cmp`  | `left`  | `right` |    -    |   -   | Sets different CPU flags depending on the comparison between `left` and `right`, see more about [[Instructions#`cmp`\|cmp]]                                                              |
|  `je`  | `label` |    -    |    -    |   -   | if `ZF` == 1, jump to `label` (`==`)                                                                                                                                                     |
| `jne`  | `label` |    -    |    -    |   -   | if `ZF` != 1, jump to `label` (`!=`)                                                                                                                                                     |
|  `jz`  | `label` |    -    |    -    |   -   | alias of `je`                                                                                                                                                                            |
| `jnz`  | `label` |    -    |    -    |   -   | alias of `jne`                                                                                                                                                                           |
|  `ja`  | `label` |    -    |    -    |   -   | if `CF and ZF` == 0, jump to `label` (`>`), works only for unsigned numbers                                                                                                              |
| `jae`  | `label` |    -    |    -    |   -   | if `CF` == 0, jump to `label` (`>=`), works only for unsigned numbers                                                                                                                    |
|  `jb`  | `label` |    -    |    -    |   -   | if `CF` == 1, jump to `label` (`<`), works only for unsigned numbers                                                                                                                     |
| `jbe`  | `label` |    -    |    -    |   -   | if `CF and ZF` == 1, jump to `label` (`=<`), works only for unsigned numbers                                                                                                             |
|  `jg`  | `label` |    -    |    -    |   -   | if `ZF` == 0 and `SF == OF`, jump to `label` (`>`), works only for signed numbers                                                                                                        |
| `jge`  | `label` |    -    |    -    |   -   | if `SF` == `OF`, jump to `label` (`>=`), works only for signed numbers                                                                                                                   |
|  `jl`  | `label` |    -    |    -    |   -   | if `SF` != `OF`, jump to `label` (`=<`), works only for signed numbers                                                                                                                   |
| `jle`  | `label` |    -    |    -    |   -   | if `ZF` == 1 or `SF` != `OF`, jump to `label` (`<`), works only for signed numbers                                                                                                       |
## `cmp`
Compares the value of the two operands provided.

let `L` be the left operand, and `R` be the right operand.

| **Condition**     | **Consequence (Flags)**          |
| ----------------- | -------------------------------- |
| L == R            | `ZF` = 1                         |
| L != R            | `ZF` = 0                         |
| L > R (unsigned)  | `CF` = 0 **and** `ZF` = 0        |
| L > R (signed)    | `SF` = `OF` **and** `ZF = 0`     |
| L >= R (unsigned) | `CF` = 0                         |
| L >= R (signed)   | `SF` = `OF`                      |
| L < R (unsigned)  | `CF` = 1                         |
| L < R (signed)    | `SF` != `OF`                     |
| L <= R (unsigned) | `CF` = 1 **or** `ZF` = 1         |
| L <= R (signed)   | (`SF` != `OF`) **or** (`ZF` = 1) |

# Definition
A Compiler is a software that turns source code into an executable that the CPU can actually understand.

By source code, i mean code that is understandable by us humans.

# Examples
An example of a compiler is `gcc`, `g++`, `rustc`, `clang`.

# How it works
A Compiler works by usually doing a 3 step process.

## The Front-End
This step analyzes your source code, and checks errors that don't follow the rules imposed by the compiler.

By the end of this step, your source code ultimately becomes an `AST` (Abstract Syntax Tree), and then an `IR` (Intermediate Representation).

## The Middle-End
The Compiler then optimizes the `IR`, it does this by for example removing unused variables or functions, and doing tricks to optimize runtime costs, binary sizes, and memory usage.

## The Back-End
The Compiler finally turns the `IR` into machine code the CPU can understand, and it can also optimize things for the target architecture.
# Definition
The process memory is a space where a program can store variables, and use the stack and heap to operate.
## Layout
A process memory at start looks like this

|   **Region**   |                                     **Description**                                     | **Other** |
| :------------: | :-------------------------------------------------------------------------------------: | :-------: |
|  Stack Start   |                                 The start of the stack                                  |     -     |
| Stack Metadata |                  Contains metadata such as `envp`, `argv`, and `argc`                   |     -     |
|   Stack End    |                                  The end of the stack.                                  |   `RSP`   |
|  Empty Space   | Between the Stack's end and Heap end, there's empty space for the stack and heap to use |     -     |
|    Heap End    |                                  The end of the heap.                                   | `sbrk(0)` |
|   Heap Start   |                                  The start of the heap                                  |     -     |
# Reference
> [!cite] Credits
> (Thanks to T-Dark on [discord](https://discord.com/channels/273534239310479360/274215136414400513/1567173000493797377) for the reference!)
> This is apparently how memory of a process could look like at a random point of execution.

Typically like this
```c
[Pre-Stack metadata] // This is sometimes said to be at the start of the stack, 
|                    // and includes things like command-line arguments and env vars
[Stack Start]
|
[Stack End] // This is where the stack pointer goes
|
| // This space is unused. It'll be used as you grow the stack or allocate more on the heap.
|
[Heap End] // This is where the "program break" goes
|
[Heap Start]
```
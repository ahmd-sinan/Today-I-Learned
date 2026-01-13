# Command Line Arguments: `argc` and `argv` 

**Date:** 13-Jan-2026


Today I learned that the `main` function doesn't have to be empty (`void`). We can use it to accept inputs directly from the command line when the program starts.

## The Syntax
Instead of `int main(void)`, we write:
```c
int main(int argc, char *argv[])
1. argc (Argument Count) 
It is an integer representing the count of arguments passed to the program.
Important: The count includes the program's own name!

If you run `./hello`, argc is 1.
If you run `./hello Bro`, argc is 2.

2. argv (Argument Vector) 
It is an array of strings (technically an array of character pointers) containing the actual arguments.

Indexing:
argv[0]: The name of the program itself (e.g., "./hello").
argv[1]: The first real input provided by the user (e.g., "David").
argv[2]: The second input, and so on.
argv[argc]: This is always a NULL pointer (identifying the end of the array).
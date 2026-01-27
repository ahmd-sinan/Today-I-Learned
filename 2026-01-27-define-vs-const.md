# Constants: `#define` vs. `const` 🥊

**Date:** 2026-01-27
**Category:** C Programming
**Tags:** #CProgramming #Preprocessor #Constants #Macros

Today I learned that there are two ways to create "unchangeable" values in C, but they work very differently under the hood.

## 1. The Preprocessor Way: `#define` 🔨
```c
#define PI 3.14
```

- What happens: Before the code is even compiled, the Preprocessor runs through the file. It literally finds every instance of the text PI and pastes 3.14 over it.
- Type: None. It is just a text fragment.
- Memory: It does not take up memory as a variable. It is "hardcoded" into the instruction logic.
- Scope: It ignores scope (curly braces {}). It is global from the point it is defined until the end of the file.

## 2. The Compiler Way: `const`
```C
const float pi = 3.14;
```

- What happens: The Compiler handles this. It creates a real variable in memory but flags it as "Read-Only."
- Type: It has a specific data type (float). The compiler checks if you are using it correctly.
- Memory: It occupies actual memory storage (usually in the "Read-Only Data" segment).- Scope: It respects scope. If defined inside a function, it only exists inside that function.

## 3. Comparison Table ⚔️

| Feature | `#define` (Macro) | `const` (Variable) |
| :--- | :--- | :--- |
| Processed By | Preprocessor (Text substitution) | Compiler (Syntax/Type checking) |
| Type Safety | No (Just text) | Yes (Strongly typed) |
| Debugging | Harder (Symbol disappears) | Easier (Symbol exists in debugger) |
| Memory | Code Segment (Immediate value) | Data Segment (Memory Address) |
| Semicolon | No semicolon (`#define MAX 10`) | Needs semicolon (`const int max = 10;`) | 

### Which one to use?
- Use `#define` for simple configuration settings or "Magic Numbers" (e.g., Array sizes in older C standards).
- Use `const` when type safety is important or when dealing with complex data types.

- `#define` is a "Find & Replace" command. `const` is a "Do Not Touch" sign on a variable.
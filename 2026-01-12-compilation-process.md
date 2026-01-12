# The Compilation Process: From Source to Machine Code ⚙️

**Date:** 12-Jan-2026


Today I learned that "compiling" isn't just one step. It is actually a chain of four distinct processes that turn human-readable code (`source code`) into computer-readable binary (`machine code`).

**The Command:**
When we run `make hello` or `clang -o hello hello.c`, these four steps happen automatically:

## 1. Preprocessing 🔨
* **Input:** Source Code (`.c` file)
* **Action:** The preprocessor handles lines starting with `#` (like `#include`).
    * It literally "copy-pastes" the contents of header files (like `stdio.h`) into your program.
    * It replaces defined constants (macros).
* **Output:** Preprocessed Source Code.

## 2. Compiling 📝
* **Input:** Preprocessed Source Code.
* **Action:** The compiler translates the C code into **Assembly Code**.
    * Assembly is low-level code closer to the CPU's instruction set, but still readable by humans (barely!).
* **Output:** Assembly Code (`.s` file).

## 3. Assembling 🤖
* **Input:** Assembly Code.
* **Action:** The assembler translates the assembly instructions into **Machine Code** (binary instructions: 0s and 1s).
    * This result is called "Object Code".
* **Output:** Object Code (`.o` file).

## 4. Linking 🔗
* **Input:** Object Code + Libraries (like standard libraries).
* **Action:** The linker combines your program's object code with the object code of the libraries you used (e.g., the actual binary code for `printf`).
    * It merges everything into one single file.
* **Output:** The Final Executable (e.g., `a.out` or `hello.exe`).

---
### Visual Summary
`hello.c` ➡️ **Preprocessing** ➡️ **Compiling** ➡️ **Assembling** ➡️ **Linking** ➡️ `./hello` (Executable)
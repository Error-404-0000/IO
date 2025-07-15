# Assembly Console Input/Output + Arithmetic (x64)

This project is a complete console app written in x64 Assembly. It handles string input/output, string-to-integer parsing, integer-to-string conversion, memory-safe storage, basic console control, and loops, all without using C runtime.

I wrote it to get a deep understanding of what high-level languages are abstracting away, and how simple tasks like `cin >>`, `cout <<`, or `std::stoi()` actually work under the hood.

---

## 💡 What It Does

* Takes two numbers as string input from the user
* Validates input (basic zero-check)
* Converts both strings to integers manually
* Adds them together
* Converts the result back to string
* Outputs the result to the console

---

## 🔍 How It Works (Internals)

### Main.asm

This is the entry point. It:

* Prints a prompt using `WriteConsole`
* Reads a line using `ReadLine`
* Calls `StringToInt64` to convert input to integer
* Stores values in `LocalStorage` (allocated on the stack)
* Performs addition directly with `add`
* Converts result back to string via `IntToString`
* Prints result using `WriteConsole`

### IO.asm

Handles console I/O directly with Win32 API:

* `WriteConsole` writes strings using `WriteConsoleA`
* `ReadLine` uses `ReadConsoleA`, manages a fixed-size input buffer (`LastInput`), and ensures null-termination

### intToString.asm

Implements a manual 64-bit integer to ASCII string conversion:

* Divides by 10 in a loop
* Stores digits in reverse
* Handles zero as a special case

### StringToInt.asm

Manually parses an ASCII string to 64-bit integer:

* Loops character-by-character
* Builds the number by multiplying previous value by 10 and adding digit

### While.asm

A `while`-like construct that accepts a function pointer to repeatedly call until a memory value equals a given target. Implemented manually using register state and a loop.

### String.asm

* `GetLength`: measures length of null-terminated ASCII strings
* `StringConcatenation`: performs byte-by-byte copy of two strings into newly allocated memory (using `CoTaskMemAlloc`)

### StringManagement.asm

* `CreateNewConsole`: calls `AllocConsole`
* `JoinConsole`: attempts to join another console session with `AttachConsole`

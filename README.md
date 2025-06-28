# Computer Programming and Programming Languages 2 (Hardware Software Interface)
# C to Assembly Transpiler

## Table of Contents

- [Overview](#overview)
- [Conventions](#conventions)
- [Operations](#operations)

## Overview

The aim of this project is to implement a small transpiler that converts simple C code snippets into basic assembly instructions.
The assignment served as a way of familiarising myself with Assembly language mnemonics and of understanding how high-level constructs 
map to low-level operations.

This transpiler is intentionally minimalistic and should be treated as such. Thus, features such as checking
the input before performing any operations are not considered relevant enough for the goal of this project. The translation is 
as simple as possible while covering basic arithmetic operations, register usage, data movement, and control flow constructs.

## Conventions

- **Basic Register Mapping:**
  - `A` → `eax`
  - `B` → `ebx`
  - `C` → `ecx`
  - `D` → `edx`

- **Data Types**
  - All of the data types are considered to be **4 bytes** (including pointers)
  - Every number will be treated as an **int** (4 bytes), in order to avoid using smaller registers (`al`, `ah`, `ax`, etc.)
  - When performing arithmetic operations, all will be considered on **32 bits** (4 bytes) for consistency

- **Handling Equivalent Instructions**
  - If an instruction has an equivalent, contracted form in C, the more **explicit and simple** one will be used
  - Example: `a = a + 12` will be used instead of `a += 12`
  - This is to avoid overcomplicating the program and defeating the purpose of the project

## Operations

Only the most common operations have been chosen for this project, given its purpose. Each subsection will explain any potential restrictions.
The examples will be given as input versus output, for a better understanding of the implementation decisions made for each section.

### MOV

The mov instructions is the simplest of all. As the name says, it moves the data from one place to another.

ASM Syntax: `MOV destination, source`

| **C Code**    | **ASM Code**          |
|------------   |----------------       |
| `a = 1;`      | `MOV eax, 1`          |
| `b = a;`      | `MOV ebx, eax`        |

>**Note:** There will be no `MOV 2, eax` as that is an invalid operation.

### Logical Operations

The three common bitwise logical operations are included.

ASM Syntax: `AND destination, source` | `OR destination, source` | `XOR destination, source`

| **C Code**        | **ASM Code**    |
|------------       |---------------- |
| `a = a & 0xFF;`   | `AND eax, 0xFF` |
| `b = b \| a`      | `OR ebx, eax`   |
| `c = a ^ c;`      | `XOR ecx, eax`  |

### Shift Operations

The other two bitwise operations can also be used by the program.

ASM Syntax: `SHL destination, num_bits_shifted` | `SHR destination, num_bits_shifted`

| **C Code**        | **ASM Code**    |
|-------------      |---------------- |
| `a = a << 1;`     | `SHL eax, 1`    |
| `b = b >> 2;`     | `SHR ebx, 2`    |

### Arithmetic Operations

The four fundamental arithmetic operations are also included. Given the technical talk required for multiplication and division,
this pair will be treated after the easier one.

#### Addition and Subtraction

The two straightforward arithmetic operations require a register, an assignment operator and the expression to be assigned.
Inputs with the plus and assign (`+=`) operator or similar ones are not taken into consideration here.

ASM Syntax: `ADD destination, source` | `SUB destination, source`

| **C Code**        | **ASM Code**      |
|------------       |----------------   |
| `a = a + 5;`      | `ADD eax, 5`      |
| `b = b - a;`      | `SUB ebx, eax`    |

#### Multiplication and Division

The other two basic arithmetic operations depend very much on data type in Assembly so as to avoid overflows, exception and incorrect results.
For the purpose of this program, only the 32-bit multiplication and division rules will be used, but with a few notes:
- The first factor of the multiplication must be found in the `eax` register
- The second factor can be found in another 32-bit register or can be a number
- The result of the multiplication should be stored in `edx:eax` (high:low); only `eax` will be used this time
- The dividend should be stored in `edx:eax` (high:low); similarly, only `eax` will be used here
- The divisor can be found in a 32-bit register other than `eax`
- Attempting to divide by 0 will result in a **Divide Error** exception

ASM Syntax: `MUL source` | `DIV divisor`

| **C Code**        | **ASM Code**    |
|------------       |---------------- |
| `a = a * 3;`      | `MUL 3`         |
| `b = b * c;`      | `MOV eax, ebx`  |
|                   | `MUL ecx`       |
|                   | `MOV ebx, eax`  |
| `a = a / 3;`      | `MOV eax, a`    |
|                   | `DIV 3`         |
|                   | `MOV a, eax`    |
| `b = b / c;`      | `MOV eax, ebx`  |
|                   | `DIV ecx`       |
|                   | `MOV ebx, eax`  |

### CMP

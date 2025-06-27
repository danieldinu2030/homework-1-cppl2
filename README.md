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

- **Data types**
  - All of the data types are considered to be **4 bytes** (including pinters)
  - Every number will be treated as an **int** (4 bytes), in order to avoid using smaller registers (`al`, `ah`, `ax`, etc.)

## Operations

Only the most common operations have been chosen for this project, given its purpose. Each subsection will explain any potential restrictions.
The examples will be given as input versus output, for a better understanding of the implementation decisions made.

### MOV

The mov instructions is the simplest of all. As the name says, it moves the data from one place to another.

Syntax: `MOV destination, source`

| **C Code**    | **ASM Code**          |
|------------   |----------------       |
| `a = 1;`      | `MOV eax, 1`          |
| `b = a;`      | `MOV ebx, eax`        |

>**Note:** There will be no `MOV 2, eax` as that is an invalid operation.

### Logical Operations

Also known as bitwise operations, all three logical operations are included.

Syntax: `AND destination, source` | `OR destination, source` | `XOR destination, source`

| **C Code**        | **ASM Code**    |
|------------       |---------------- |
| `a = a & 0xFF;`   | `AND eax, 0xFF` |
| `b = b \| a`      | `OR ebx, eax`   |
| `c = a ^ c;`      | `XOR ecx, eax`  |

### Arithmetic Operations

The four fundamental arithmetic operations are also included. Given the technical talk required for multiplication and division,
this pair will be treated after the easier one.

#### Addition and Subtraction

Syntax: `ADD destination, source` | `SUB destination, source`

| **C Code**        | **ASM Code**      |
|------------       |----------------   |
| `a = a + 5;`      | `ADD eax, 5`      |
| `b = b - a;`      | `SUB ebx, eax`    |

#### Multiplication and Division

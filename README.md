# CPP Module 06

This repository contains my implementation of **C++ Module 06**, developed as part of the 42 School curriculum. This module explores the different types of casts available in C++, including static_cast, dynamic_cast, reinterpret_cast, and const_cast, through practical exercises involving type conversion, serialization, and type identification.

**"C++ casts"**

## Table of Contents

- [Overview](#overview)
- [General Rules](#general-rules)
- [Additional Rule](#additional-rule)
- [Exercises](#exercises)
  - [Exercise 00: Conversion of scalar types](#exercise-00-conversion-of-scalar-types)
  - [Exercise 01: Serialization](#exercise-01-serialization)
  - [Exercise 02: Identify real type](#exercise-02-identify-real-type)
- [Project Structure](#project-structure)
- [Installation](#installation)
- [Usage](#usage)
- [Submission and Peer Evaluation](#submission-and-peer-evaluation)
- [Acknowledgments](#acknowledgments)
- [Disclaimer](#disclaimer)
- [License](#license)

## Overview

C++ is a general-purpose programming language created by **Bjarne Stroustrup** as an extension of the C programming language, or "C with Classes". This module focuses on the various casting operators introduced in C++ to provide safer and more explicit type conversions compared to C-style casts.

Key topics covered in this module:
- **static_cast** - compile-time type conversion for related types
- **dynamic_cast** - runtime type identification and conversion in polymorphic hierarchies
- **reinterpret_cast** - low-level reinterpretation of bit patterns
- **const_cast** - adding or removing const/volatile qualifiers
- **Type detection** - identifying types from string literals
- **Serialization** - converting pointers to integers and back
- **Type identification** - determining actual types at runtime without typeinfo

All exercises must be written in **C++98** and compiled using the appropriate flags.

## General Rules

- **Compilation:**
  - Compile with `c++ -Wall -Wextra -Werror`
  - Code must still compile with `-std=c++98`

- **Formatting & Naming:**
  - Exercises are stored in directories `ex00`, `ex01`, `ex02`, …  
  - Class names in `UpperCamelCase` (e.g., `ScalarConverter`, `Serializer`)  
  - Source files: `ClassName.cpp`  
  - Header files: `ClassName.hpp` or `ClassName.h`  

- **Allowed / Forbidden:**
  - Allowed: most of the C++ standard library
  - Forbidden: external libraries, C++11 or newer, Boost libraries
  - Forbidden functions: `*printf()`, `*alloc()`, `free()`
  - Forbidden keywords: `using namespace <ns_name>`, `friend` (unless explicitly stated)
  - STL containers and algorithms are only allowed starting from Module 08

- **Design Requirements:**
  - All classes must follow the Orthodox Canonical Form (unless stated otherwise)
  - Avoid memory leaks when using `new` / `delete`
  - Each header must be self-contained and protected by include guards
  - No function implementations inside headers (except templates)

## Additional Rule

**IMPORTANT:** For each exercise, type conversion must be handled using a **specific type of casting**. Your choice of cast type will be reviewed during defense. Use the appropriate C++ cast operator (static_cast, dynamic_cast, reinterpret_cast, const_cast) rather than C-style casts.

## Exercises

### Exercise 00: Conversion of scalar types

**Directory:** `ex00/`  
**Files to turn in:** `Makefile`, `*.cpp`, `*.{h,hpp}`  
**Allowed functions:** Any function to convert from string (to help, but won't do the whole job)

Create a `ScalarConverter` class with a single static method that converts string representations of C++ literals to scalar types.

**Class design:**
- Only one static method: `convert(const std::string& literal)`
- Must NOT be instantiable by users (no public constructor)

**Supported literal types:**
- **char literals:** `'c'`, `'a'`, etc.
- **int literals:** `0`, `-42`, `42`, etc.
- **float literals:** `0.0f`, `-4.2f`, `4.2f`, plus pseudo-literals (`-inff`, `+inff`, `nanf`)
- **double literals:** `0.0`, `-4.2`, `4.2`, plus pseudo-literals (`-inf`, `+inf`, `nan`)

**Output format:**
The program must display conversions to all four types:
```
char: <result>
int: <result>
float: <result>
double: <result>
```

**Special cases:**
- Non-displayable chars: print "Non displayable"
- Impossible conversions: print "impossible"
- Overflow: print "impossible"

**Examples:**
```sh
./convert 0
char: Non displayable
int: 0
float: 0.0f
double: 0.0

./convert nan
char: impossible
int: impossible
float: nanf
double: nan

./convert 42.0f
char: '*'
int: 42
float: 42.0f
double: 42.0
```

**Key Learning:** Type detection from strings, explicit type conversion, handling special values and edge cases, static class design.

### Exercise 01: Serialization

**Directory:** `ex01/`  
**Files to turn in:** `Makefile`, `*.cpp`, `*.{h,hpp}`

Implement a `Serializer` class that converts pointers to unsigned integers and back.

**Class design:**
- Not initializable by users
- Two static methods only

**Static methods:**
```cpp
uintptr_t serialize(Data* ptr);
```
Converts a pointer to unsigned integer type `uintptr_t`.

```cpp
Data* deserialize(uintptr_t raw);
```
Converts an unsigned integer back to a `Data*` pointer.

**Requirements:**
- Create a non-empty `Data` structure (with data members)
- Test by serializing a Data object's address, then deserializing
- Verify that the deserialized pointer equals the original pointer

**Example:**
```cpp
Data* original = new Data();
uintptr_t raw = Serializer::serialize(original);
Data* deserialized = Serializer::deserialize(raw);
// original == deserialized (must be true)
```

**Key Learning:** Using `reinterpret_cast` for pointer-to-integer conversions, understanding `uintptr_t`, round-trip conversion verification.

### Exercise 02: Identify real type

**Directory:** `ex02/`  
**Files to turn in:** `Makefile`, `*.cpp`, `*.{h,hpp}`  
**Forbidden:** `std::typeinfo` header

Create a type identification system without using typeinfo.

**Class hierarchy:**
- `Base` - class with public virtual destructor only
- `A`, `B`, `C` - three empty classes that inherit publicly from Base

*Note: These classes don't need to follow Orthodox Canonical Form.*

**Functions to implement:**

```cpp
Base* generate(void);
```
Randomly instantiates A, B, or C and returns as Base pointer.

```cpp
void identify(Base* p);
```
Prints the actual type of the object pointed to by p: "A", "B", or "C".

```cpp
void identify(Base& p);
```
Prints the actual type of the object referenced by p: "A", "B", or "C".
**Using a pointer inside this function is forbidden.**

**Constraints:**
- Cannot include `<typeinfo>` header
- Cannot use `typeid` or `std::type_info`
- Must identify types using other mechanisms (e.g., dynamic_cast)

**Key Learning:** Runtime type identification using `dynamic_cast`, understanding the difference between pointer and reference semantics, working with polymorphic hierarchies without typeinfo.

## Project Structure

```
CPP06/
├── ex00/
│   ├── Makefile
│   ├── main.cpp
│   ├── ScalarConverter.hpp
│   └── ScalarConverter.cpp
├── ex01/
│   ├── Makefile
│   ├── main.cpp
│   ├── Serializer.hpp
│   ├── Serializer.cpp
│   ├── Data.hpp
│   └── Data.cpp
├── ex02/
│   ├── Makefile
│   ├── main.cpp
│   ├── Base.hpp
│   ├── Base.cpp
│   ├── A.hpp
│   ├── B.hpp
│   └── C.hpp
└── README.md
```

## Installation

1. **Clone the Repository:**

   ```sh
   git clone https://github.com/marco-ht/CPP06.git
   cd CPP06
   ```

2. **Build the Desired Exercise:**

   ```sh
   cd ex00 && make
   ```

## Usage

Each exercise produces its own executable. Navigate to the exercise directory and run:

**Exercise 00:**
```sh
./convert <literal>
./convert 42
./convert 42.0f
./convert 'c'
./convert nan
```

**Exercise 01:**
```sh
./serializer
```
The program demonstrates serialization and deserialization of Data pointers.

**Exercise 02:**
```sh
./identify
```
The program generates random Base-derived objects and identifies their actual types.

## Submission and Peer Evaluation

- Submit your project through the designated Git repository.
- Only the files within your Git repository will be evaluated during defense.
- Double-check that all file names and structures match the subject requirements.
- Ensure your Makefile compiles without relinking and includes all required rules.
- **Be prepared to explain your choice of cast operators** for each exercise during defense.

## Acknowledgments

- Thanks to the 42 community, mentors, and fellow students for their guidance and support.
- Special thanks to resources on C++ type casting and type safety.

## Disclaimer

**IMPORTANT**:
This project was developed solely as part of the educational curriculum at 42 School. The code in this repository is published only for demonstration purposes to showcase my programming and C++ development skills.

**Academic Integrity Notice**:
It is strictly prohibited to copy or present this code as your own work in any academic submissions at 42 School. Any misuse or uncredited use of this project will be considered a violation of 42 School's academic policies.

## License

This repository is licensed under the Creative Commons - NonCommercial-NoDerivatives (CC BY-NC-ND 4.0) license. You are free to share this repository as long as you:

- Provide appropriate credit.
- Do not use it for commercial purposes.
- Do not modify, transform, or build upon the material.

For further details, please refer to the CC BY-NC-ND 4.0 license documentation.

Developed with dedication and in strict adherence to the high standards of 42 School.

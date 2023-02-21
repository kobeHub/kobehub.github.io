---
layout: post
title: C++ Preprocess Directives
date: 2023-02-16 10:00:00
description: Common-used preprocess directives in c++.
tags: cpp
categories: cpp, programming
giscus_comments: true
---

I have recently joined a company that specializes in Quantitative Trading, where the primary languages used for development are C++ and Rust. Although I have not had prior experience working on a commercial C++ program, I wanted to share my recent encounter with C++ preprocess directives in this post.

## 1. #include and #include_next

#include is the most common-used directive in c/c++. It just searches the file and add targeted file's content to current file. Normally, we include all header files(.h and .hpp) in our source files (.cc, .cpp, .cxx). Basic usage:∞

```c++
#include <path-spec>
#include "path-spec"
```

The *path-spec* is a file name that may optionally be preceded by a directory specification. The file name must name an existing file. The syntax of the *path-spec* depends on the operating system on which the program is compiled.

`#include_next` is a supportive directive in GUN c++. `#include_next` does not distinguish between \<file> and "file" inclusion, nor does it check that the file you specify has the same name as the current file. It simply looks for the file named, starting with the directory in the search path after the one where the current file was found.

The use of `#include_next' can lead to great confusion. We recommend it be used only when there is no other alternative. In particular, it should not be used in the headers belonging to a specific program; it should be used only to make global corrections along the lines of fixincludes.

Here are some scenarios we can use `#include_next`:

+ If we want to replace a default header with our own making. If we want to replace `stblib.h`, we create a new file in our project and it contains a header "stdlib.h". We can create a our lib in this way.

  ```c++
  #include_next "stdlib.h"
  int my_lib_func();
  ```

  And the compiler will not include *your* stdlib.h again recursively, as would be the case with plain a #include, but rather continue in other directories for a file named "stdlib.h".

+ It's handy if you're supporting multiple versions of something. For example, I'm writing code that supports PostgreSQL 9.4 and 9.6. A number of internal API changes exist, mostly new arguments to existing functions. We can use the most up-to-date version header with `#include_next`.

## 2. #define and #undef

The **#define** creates a *macro*, which is the association of an identifier or parameterized identifier with a token string. After the macro is defined, the compiler can substitute the token string for each occurrence of the identifier in the source file.

The **#define** directive causes the compiler to substitute *token-string* for each occurrence of *identifier* in the source file. The *identifier* is replaced only when it forms a token. That is, *identifier* is not replaced if it appears in a comment, in a string, or as part of a longer identifier.

One or more white-space characters must separate *token-string* from *identifier*. This white space is not considered part of the substituted text, nor is any white space that follows the last token of the text. But any token after token string will use to replace text;

```c++
#define TEST 3;
int a = 1 + TEST // a = 1 + 3;
```

A `#define` without a *token-string* removes occurrences of *identifier* from the source file. The *identifier* remains defined and can be tested by using the `#if defined` and `#ifdef` directives.

The second syntax form defines a function-like macro with parameters. This form accepts an optional list of parameters that must appear in parentheses. After the macro is defined, each subsequent occurrence of *identifier*( *identifier*opt, ..., *identifier*opt ) is replaced with a version of the *token-string* argument that has actual arguments substituted for formal parameters.

```cpp
#define max(a,b) ( (a) > (b) ? (a) : (b) )    // We need to add parentheses to each argument to ensure expression is correct.
int m = 3, n = 4;
int c = max( m, n );   
```

`#undef` removes one identifier's defination.

## 3. Conditions: #if, #ifdef

There is a series of preprocess directives to control workflow. Conditional directives are mostly used in standard libraries to detect platforms and environment.

```c++
#if constant-expression  // #ifdef or #ifndef
      code
#elif constant-expression
      code
#else
      code
#endif
```

 The *constant-expression* is an integer constant expression with these additional restrictions:

- Expressions must have integral type and can include only integer constants, character constants, and the **defined** operator.
- The expression can't use **`sizeof`** or a type-cast operator.
- The target environment may be unable to represent all ranges of integers.
- The translation represents type **`int`** the same way as type **`long`**, and **`unsigned int`** the same way as **`unsigned long`**.
- The translator can translate character constants to a set of code values different from the set for the target environment. To determine the properties of the target environment, use an app built for that environment to check the values of the *LIMITS.H* macros.
- The expression must not query the environment, and must remain insulated from implementation details on the target computer.

In the following example, the **#if** and **#endif** directives control compilation of one of three function calls:

```cpp
#if defined(CREDIT)
    credit();
#elif defined(DEBIT)
    debit();
#else
    printerror();
#endif
```

The conditional compilation statements in the following example assume a previously defined symbolic constant named `DLEVEL`.

```cpp
#if DLEVEL > 5
    #define SIGNAL  1
    #if STACKUSE == 1
        #define STACK   200
    #else
        #define STACK   100
    #endif
#else
    #define SIGNAL  0
    #if STACKUSE == 1
        #define STACK   100
    #else
        #define STACK   50
    #endif
#endif
#if DLEVEL == 0
    #define STACK 0
#elif DLEVEL == 1
    #define STACK 100
#elif DLEVEL > 5
    display( debugptr );
#else
    #define STACK 200
#endif
```

## 4. #pragma

Pragma directives specify machine-specific or operating system-specific compiler features. A line that starts with `#pragma` specifies a pragma directive. 

```cpp
#pragma token-string
__pragma( token-string ) // two leading underscores - Microsoft-specific extension
_Pragma( string-literal ) // C99
```

Each implementation of C and C++ supports some features unique to its host machine or operating system. Some programs, for example, must exercise precise control over the location of data in memory, or control the way certain functions receive parameters. The **`#pragma`** directives offer a way for each compiler to offer machine- and operating system-specific features, while maintaining overall compatibility with the C and C++ languages.

### 4.1 #pragma region

This directive specifies the packing alignment for structure, union and class members.

+ `#pragma pack(show)`: shows current byte value for packing alignment. The value is displayed by a warning message.
+ `#pragma pack(push, n)`: pushes the current packimg alignment value to internal compiler stack and set current packing alignment to `n`. If n is not specified, current packing alignment value is pushed.
+ `#pragma pack(pop, n)`: Removes the record from the top of internal compiler stack. If `n` isn't specified with `pop`, then current packing alignment value is the top of the stack. If `n` is specified, `n` becomes the new packing alignment value. 
+ `#pragma pack(n)`: specifies the packing alignment value. The default value is 8, available values include 1, 2, 4, 8, and 16. The alignment of a member is on a boundary that's either a multiple of *`n`*, or a multiple of the size of the member, whichever is smaller.

As for the push and pop, we can also assign one identifier to it. If you pop using an *`identifier`*, for example, `#pragma pack(pop, r1)`, then all records on the stack are popped until the record that has *`identifier`* is found. That record gets popped, and the packing value associated with the record found on the top of the stack becomes the new packing alignment value. If you pop using an *`identifier`* that isn't found in any record on the stack, then the **`pop`** is ignored.

```c++
// pragma_directives_pack_2.cpp
// compile with: /W1 /c
#pragma pack()   // n defaults to 8; equivalent to /Zp8
#pragma pack(show)   // C4810
#pragma pack(4)   // n = 4
#pragma pack(show)   // C4810
#pragma pack(push, r1, 16)   // n = 16, pushed to stack
#pragma pack(show)   // C4810

// pop to the identifier and then set
// the value of the current packing alignment:
#pragma pack(pop, r1, 2)   // n = 2 , stack popped
#pragma pack(show)   // C4810
```

### 4.1 `#pragma once`

This directive specifies that the compiler includes this header file only once, while compiling a source file.

The use of `#pragma once` can reduce build times, as the compiler won't open and read the file again after the first `#include` of the file in the translation unit. It's called the *multiple-include optimization*. We can also use include guard idiom, which puts around the entirety of the header file:

```c++
// point.hpp
#ifndef MYPROJECT_POINT_HPP_GUARD
#define MYPROJECT_POINT_HPP_GUARD
struct point {
    int x, y;
};
#endif
```

We recommend the `#pragma once` directive for new code because it doesn't pollute the global namespace with a preprocessor symbol. It requires less typing, it's less distracting, and it can't cause *symbol collisions*. Symbol collisions are errors caused when different header files use the same preprocessor symbol as the guard value. It isn't part of the C++ Standard, but it's implemented portably by several common compilers.


## 5. \__attribute\__

## 6. \__declspec


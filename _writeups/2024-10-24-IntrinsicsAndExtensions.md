---
title: "Intrinsics and ISA Extensions"
layout: post
---


When we write a C/C++ program, we will compile it using either gcc/clang/msvc/intel compiler. During compilation the cpp code is converted to assembly using the definitions provided in .cpp files.
These compilers can also provide some extra 'functions' which can be used by our program but whose definitions is not a part of our program or any library our program uses but compiler has access to/knows the assembly language definition of these functions. Or a third party can provide its own interface (a header file) and machine language impelentation of the functions which compiler will just inline. The functions available can depend on the compiler we are using and the architecture for which the program is being compiled.

Often CPU vendors provide these intrisnics which can be included via a header file. Intel has its own set of intrinsics for its CPUs. (x86,  amd64/x86-64 include "immintrin.h"), so does ARM ( include ??). You will have to check if there are compiler specific compatibility issues and what headers to include in your program to use these intrinsics.

#### Extensions and Relation between Intrinsics and Extensions

What kind of functions are provided by cpu vendors separately ? It so happens that a RISC/CISC/ARM processor might support additional instructions than defined in the RISC/CISC/ARM standard. This capability to support addditional instructions is totally vendor and model specific. These instructions are not something compiler can output as they are not part of standard. The intrinsics provided by the cpu vendor are functions that rely on these extensions. For example Intel provides Advanced Vector Extensions, and corresponding intrinsics functions which rely on these extension.




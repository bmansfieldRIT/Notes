## Clang Cheatsheet

#### About Clang:
* compiler front end
* supports C, C++, Obj C, Obj C++, OpenMP, OpenCL, CUDA
* uses LLVM as a backend
* has been part of LLVM release cycle since LLVM 2.6
* designed as a drop-in replacement for GCC, supporting most compiler flags
* contributors include Apple, MSFT, GOOG, ARM, Sony, Intel, AMD
* released under UofIllinois / NCSA license
* Clang project includes Clang Front end, Clang Static Analyzer, and several code analysis tools
* Clang Static Analyzer - implemented as a C++ library, finds bugs in C/C++/Obj C programs

#### History:
* 2005, Apple begins to heavily use LLVM in iOS SDK, and Xcode IDE
* LLVM was originally intended to use GCC front end, but GCC was too large and cumbersome
* Apple software uses Objective C, but Obj C front end in GCC is low priority
* GCC published under GNU GPL, requires source code for any GCC forks be publicly available
* LLVM has BSD license, no source release required
* Apple chooses to develop new compiler front end, supporting C/Obj C/C++, open sourced in July 2007
* In 2012, Clang become default compiler in MINIX, FreeBSD
* In 2016, Clang becomes default compiler for Android
* In 2018, Clang used to build Chrome for windows

#### Writing Clang Tools:
* LibClang
	* stable high level C Interface to clang, usually desired interface
	* use for interface from other languages than C++, want powerful high level abstractions
	* do not use when you want full control over Clang AST
* Clang Plugins
	* allow you to run additional actions on the AST as part of compilation
	* can use to rerun tools on dependency changes, when you need full control over clang AST
	* do not use for running tools outside build environment, controlling how Clang is set up
* LibTooling
	* C++ interface for writing standalone tools
	* use for writing simple syntax checker, refactoring tools, when you want to run tools over a single file independent of build system, when you want full control over Clang AST
	* do not use if you don’t want to write tools in C++

#### Design:
* Intended to work on top of LLVM
* Main goal of Clang is to provide library based architecture, allows compiler to be more tightly tied to tools that interact with source code, like IDE
* compiler for C languages only, for other languages LLVM remains dependent on GCC of other compiler front end
* designed to be highly compatible with GCC
* Clang can run some benchmarks faster than GCC, though GCC has often still faster compilation speeds
* compilers are broadly seen as comparable in speed and memory footprint

#### Clang Vs GCC
* Clang = Library based architecture, vs GCC = classic compile-link-debug cycle
	* makes integration with other tools like IDE easier
	* vendors using GCC usually use other tools for syntax highlighting, autocomplete, etc
* Clang retains more info during compiling process than GCC, preserves overall form of code
	useful for automated code refactoring, syntax checking, better error messages
* GCC supports more targets than LLVM
* GCC supports more languages than Clang
* GCC has old codebase, Clang designs for understandable AST
* GCC is a monolith static compiler, Clang is built to be modular
* Clang does not implicitly simplify code as it parses (x-x -> 0), better for refactoring, for example

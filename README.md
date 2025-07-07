# CoreObject
Object Oriented Programming ABI compatible with CoreFoundation

## Introduction

CoreObject aims to be provide a small C API and a stable ABI for exposing modern OOP concepts like classes, dynamically dispatched methods and properties.

The project uses CoreFoundation as a base for its ABI layout, and extends it with dynamically dispatched methods, properties and optional reflection (disabled for embedded targets). While this design sacrifices the speed of directly calling C functions like CoreFoundation provides, it allows an API to provide a stable ABI and C interface for distribution with operating systems, compiled libraries, and interoperability with different programming languages. It aims to reimplement some features of Objective-C and Java, while targetting embedded systems and resource constrained devices, including WASM. One of the advantages of its dynamic nature is the ability to implement types in any language, on any platform, and have executables link against this C API, and not break when your API changes. A framework could be implemented in Swift, and consumed from C, C++, Rust or Java. Likewise, a Swift application could be built against this C API, and implemented in Rust or Java. No generated C shims or headers are neccesary, and reflection capabilities allow scripting languages to leverage your APIs as well. 

## ABI

This library provides a small set of functions for object type metadata and dynamic dispatch. Internally, objects extend `__CFRuntimeClass` with additional metadata for object properties and methods, with a vtable similar to COM for dynamic method dispatch. CoreFoundation is used for memory management and class registration. The only function clients of your APIs will be required to link against will be the provider of your CFType and call `_CFRuntimeRegisterClass`, e.g. `CFUUIDGetTypeID()`. This differs from CoreFoundation where a function like `CFUUIDCreate()` or `CFArraySetValueAtIndex()` introduces an ABI requirement that Darwin platforms have had to adhere to since the 90s.

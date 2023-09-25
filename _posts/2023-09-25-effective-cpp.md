---
layout: post
title: "Effective C++"
---
These are my notes for the book Effective C++ by Scott Meyers you can see my simliar writing his two useful books called [Effective STL](https://quynhdinh.github.io/notes/2023/09/25/effective-stl) and [Modern Effective C++](https://quynhdinh.github.io/notes/2023/09/25/effective-modern-cpp)

#### Chapter 1.  Accustoming Yourself to C++
- [x] 1: View C++ as a federation of languages

Those are STL, the good old C, and the OOP C++.
- [ ] 2: Prefer consts, enums, and inlines to `#defines`

Before your source code actually goes to the compiler, it will come to the symbol table, by using #defines instead of others, there are no meaningful exceptions throw when errors came
- [ ] 3: Use const whenever possible
- [ ] 4: Make sure that objects are initialized before they're used

#### Chapter 2.  Constructors, Destructors, and Assignment Operators
- [ ] 5: Know what functions C++ silently writes and calls
- [ ] 6: Explicitly disallow the use of compiler-generated functions you do not want
- [ ] 7: Declare destructors virtual in polymorphic base classes
- [ ] 8: Prevent exceptions from leaving destructors
- [ ] 9: Never call virtual functions during construction or destruction
- [ ] 10: Have assignment operators return a reference to `*this`
- [ ] 11: Handle assignment to self in operator=
- [ ] 12: Copy all parts of an object

#### Chapter 3.  Resource Management
- [ ] 13: Use objects to manage resources.
- [ ] 14: Think carefully about copying behavior in resource-managing classes.
- [ ] 15: Provide access to raw resources in resource-managing classes.
- [ ] 16: Use the same form in corresponding uses of new and delete.
- [ ] 17: Store newed objects in smart pointers in standalone statements.

#### Chapter 4.  Designs and Declarations
- [ ] 18: Make interfaces easy to use correctly and hard to use incorrectly
- [ ] 19: Treat class design as type design
- [ ] 20: Prefer pass-by-reference-to-const to pass-by-value
- [ ] 21: Don't try to return a reference when you must return an object
- [ ] 22: Declare data members private
- [ ] 23: Prefer non-member non-friend functions to member functions
- [ ] 24: Declare non-member functions when type conversions should apply to all parameters
- [ ] 25: Consider support for a non-throwing swap

#### Chapter 5.  Implementations
- [x] 26: Postpone variable definitions as long as possible.

This is classic. Same advice from Bloch's book.
- [ ] 27: Minimize casting.
- [ ] 28: Avoid returning "handles" to object internals.
- [ ] 29: Strive for exception-safe code.
- [ ] 30: Understand the ins and outs of inlining.
- [ ] 31: Minimize compilation dependencies between files.

#### Chapter 6.  Inheritance and Object-Oriented Design
- [ ] 32: Make sure public inheritance models "is-a."
- [ ] 33: Avoid hiding inherited names
- [ ] 34: Differentiate between inheritance of interface and inheritance of implementation
- [ ] 35: Consider alternatives to virtual functions
- [ ] 36: Never redefine an inherited non-virtual function
- [ ] 37: Never redefine a function's inherited default parameter value
- [ ] 38: Model "has-a" or "is-implemented-in-terms-of" through composition
- [ ] 39: Use private inheritance judiciously
- [ ] 40: Use multiple inheritance judiciously

#### Chapter 7.  Templates and Generic Programming
- [ ] 41: Understand implicit interfaces and compile-time polymorphism
- [ ] 42: Understand the two meanings of typename
- [ ] 43: Know how to access names in templatized base classes
- [ ] 44: Factor parameter-independent code out of templates
- [ ] 45: Use member function templates to accept "all compatible types."
- [ ] 46: Define non-member functions inside templates when type conversions are desired
- [ ] 47: Use traits classes for information about types
- [ ] 48: Be aware of template metaprogramming

### Chapter 8.  Customizing new and delete
- [ ] 49: Understand the behavior of the new-handler
- [ ] 50: Understand when it makes sense to replace new and delete
- [ ] 51: Adhere to convention when writing new and delete
- [ ] 52: Write placement delete if you write placement new

#### Chapter 9.  Miscellany
- [ ] 53: Pay attention to compiler warnings.
- [ ] 54: Familiarize yourself with the standard library, including TR1
- [ ] 55: Familiarize yourself with Boost.

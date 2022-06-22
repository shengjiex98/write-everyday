- [Learning languages](#learning-languages)
  - [Julia](#julia)
- [COMP 524](#comp-524)
  - [What a genius solution!](#what-a-genius-solution)
  - [Lexical (tokenization)](#lexical-tokenization)
  - [Syntactic (parsing)](#syntactic-parsing)
  - [Semantic (evaluation)](#semantic-evaluation)
    - [Applicative order](#applicative-order)
    - [Non-applicative order](#non-applicative-order)
    - [Environment & scope](#environment--scope)
  - [Unification](#unification)

# Learning languages

## Julia

- Julia's functions **does not** belong to objects. I.e., use `enqueue!(q, item)` instead of `q.enqueue!(item)`. This has the advantage that all arguments of a function call are used to determine which implementation of the function (*method*) is used instead of just the `self` argument like in other OOP. This procress of determining which method to use is called *dispatch*.
- Julia stores n-d arrays with **first dimension** consecutive in memory. Check out [this](https://discourse.julialang.org/t/for-i-1-m-j-1-n-discrepancy-loop-vs-comprehension-generator/75659/25) discussion on the Julia discourse (and [my answer](https://discourse.julialang.org/t/for-i-1-m-j-1-n-discrepancy-loop-vs-comprehension-generator/75659/26?u=shengjiex98)). Also [Why column major?](https://discourse.julialang.org/t/why-column-major/24374/30).

# COMP 524

## What a genius solution!
Append two lists in SML
```SML
fun append([], ls2) = ls2
  | append(h::t, ls2) = h::append(t, ls2)
```

## Lexical (tokenization)

## Syntactic (parsing)

Shape/form of the language.

To use a recursive descent parser (RDP), the grammar needs to be unambiguous. LL(1) grammar is such a grammar that satisfies the following requirements

- No left recursion
- No common prefixes

## Semantic (evaluation)

Meaning of the language. An operator is either a

- special form,
- macro, or
- procedure

### Applicative order

1. Evaluate the operator
2. Evaluate the operands from left to right
3. Invoke (apply) the procedure on the arguments

### Non-applicative order

Applicative order always evaluate all arguments. In contrast, *normal order of evaluation* or *normal semantics* are used by special forms (like `if`) and macros.

- Special forms: `if`, `and`, `or`, `define` and `lambda`
- Macros

### Environment & scope

- Lexical scoping: extend the *defining* environment of function upon invocation
- Dynamic scoping: extend the *current* environment of function upon invocation

## Unification

Type inference summary
Here are the important things that you need to know about type inference.

The type inference algorithm involves 3 steps:

1. assign type variables to the unknown types of various subexpressions
2. use the typing semantics and associated rules to create equations involving the type variables
3. solving the system of equations for the unknowns (i.e. the type variables) using the unification algorithm

Unification can have several outcomes:

- an invalid equation, which implies a type conflict, which means the given program has invalid types
- a type equation with a variable alone on the LHS and the RHS containing that same variable but not alone, which implies circularity, which means that the program does not have enough information to be type-checked, so the type checking fails
- a valid set of substitutions, with every type variable having a fully concrete type (i.e. no type variables in the RHS of any substitution), which means that the program is validly typed with no type parameters
- a valid set of substitutions, with at least some type variables occuring in the RHS of some substitutions, which means that the program is validly typed with at least 1 type parameter

The unification algorithm works like this:

1. if there are no remaining equations, terminate and conclude the program is validly typed
2. examine the top equation
    - if there are any type variables that we have a substitution for, apply the substitution to the equation and restart Step 2
    - if the LHS and RHS are both function types, replace the equation with two equations, one for the input types and one for the output types, and restart Step 2
    - if the RHS is a lone type variable, swap the LHS and RHS and restart Step 2
    - if the LHS is a lone type variable and the RHS contains the same type variable, fail: your types have circularity so your program cannot be type checked
    - if the LHS is a lone type variable, add it to the substitutions, and apply that substitution to all existing substitutions and go to Step 1
    - otherwise, fail: you have a type conflict
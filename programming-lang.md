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
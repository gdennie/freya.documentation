# Overview of the Freya stack

Freya is made up of quite a few distinct components which are designed to work in harmony -- but it's also designed to be a set of components where you can mix and match which parts of Freya you want to use.

It's referred to as a stack as it's built as a set of libraries which build on lower level libraries, providing more specific and more focused functionality and abstractions the higher in the stack you go.

You can take only the low level libraries and use them to build your own systems on top (defining your own higher level abstractions), or you can use some of the higher level libraries to build systems where more abstractions are predefined, and you can start thinking at a higher level.

To make full use of the Freya stack it's useful to understand how the stack fits together and what is provided by the different libraries. That way you can decide what you want to build and what level of abstraction suits you.

## Low Level Libraries

### Freya.Core

__Freya.Core__ is one of the lowest level libraries in the Freya stack. Along with Freya.Types, it's one of two libraries which have no dependencies within the Freya stack, and it serves as a basic functional abstraction over [OWIN][owin]. The chief functionality provided by __Freya.Core__ is fully-featured computation expression for working with OWIN compatible web requests/reposnses, wrapping the raw dictionary abstraction of OWIN with a more idiomatically F# interface to that data.

This computation expression (simply called `freya`) comes with a set of operators and functions for working with the data inside the OWIN state, and some useful functions for working in a request/response-centric way, such as memoising computation expressions for the lifetime of a request.

For more on __Freya.Core__ see the in-depth guides on the [Core Overview][core-overview], [Core Abstraction][core-abstraction] and [Core Syntax Options][core-syntax].

### Freya.Types.*

The __Freya.Types.*__ family of libraries (__Freya.Types.Cors__, __Freya.Types.Http__, __Freya.Types.Language__, __Freya.Types.Uri__) provides strongly typed representations of common data types that are used in web programming. The representations closely mirror the relevant specifications (such as the IETF RFCs which define much of the modern web infrastructure).

The types additionally comes with parsers and formatters, allowing them to easily be used where string representations of data are provided by lower level abstractions such as OWIN.

The __Freya.Types.Cors__ and __Freya.Types.Http__ libraries also provide sets of lenses which integrate with the Freya.Core abstraction, providing strongly typed, safe access to all of the data within the OWIN state.

For more on __Freya.Types.*__ see the in-depth guide to the [Types Family][types-family].

## Mid-Level Libraries

### Freya.Pipeline

__Freya.Pipeline__ addresses the desire for composability in the Freya stack. It provides a simple set of types which define a Freya pipeline -- simply put, a core Freya abstraction which returns either Next or Halt, and a way of chaining these pipeline components together logically.

For more on __Freya.Pipeline__ see the in-depth guide to the [Pipeline][pipeline].



[OWIN]: http://owin.org

[core-overview]: ./core-overview.md
[core-abstraction]: ./core-understanding-the-abstraction.md
[core-syntax]: ./core-computation-expressions-or-operators.md

[types-family]: ./types-understanding-the-family.md

[pipeline]: ./pipeline-composition.md

# Overview of the Freya stack

Freya is made up of quite a few distinct components which are designed to work in harmony -- but it's also designed to be a set of components where you can mix and match which parts of Freya you want to use.

It's referred to as a stack as it's built as a set of libraries which build on lower level libraries, providing more specific and more focused functionality and abstractions the higher in the stack you go.

You can take only the low level libraries and use them to build your own systems on top (defining your own higher level abstractions), or you can use some of the higher level libraries to build systems where more abstractions are predefined, and you can start thinking at a higher level.

To make full use of the Freya stack it's useful to understand how the stack fits together and what is provided by the different libraries. That way you can decide what you want to build and what level of abstraction suits you.

This overview of the libraries which make up Freya is non-exhaustive, excluding libraries which are not intended for "end user" use in building systems using Freya, but support aspects of Freya functionality.

## Low-Level Libraries

### Freya.Core

_Freya.Core_ is one of the lowest level libraries in the Freya stack. Along with _Freya.Types_, it's one of two libraries which have no dependencies within the Freya stack, and it serves as a basic functional abstraction over [OWIN][owin]. The chief functionality provided by _Freya.Core_ is a fully-featured computation expression (and set of supporting functions) for working with OWIN compatible web requests/reposnses, wrapping the simple dictionary abstraction of OWIN with a more idiomatically F# interface to that data.

See the guides [Freya.Core Overview][core-overview] and [Core Syntax Options][core-syntax] (though [Core Syntax Options][core-syntax] can be safely deferred until you're comfortable with the Freya stack).

### Freya.Types.*

The _Freya.Types.*_ family of libraries (_Freya.Types.Cors_, _Freya.Types.Http_, _Freya.Types.Language_, _Freya.Types.Uri_) provides strongly typed representations of common data types that are used in web programming. The representations closely mirror the relevant specifications (such as the IETF RFCs which define much of the modern web infrastructure).

The types additionally comes with parsers and formatters, allowing them to easily be used where string representations of data are provided by lower level abstractions such as OWIN.

The _Freya.Types.Cors_ and _Freya.Types.Http_ libraries also provide sets of lenses which integrate with the _Freya.Core_ abstraction, providing strongly typed, safe access to all of the data within the OWIN state.

See the guide [Understanding the Freya.Types.* family][types-family].

## Mid-Level Libraries

### Freya.Pipeline

_Freya.Pipeline_ addresses the desire for composability in the Freya stack. It provides a simple set of types which define a Freya pipeline -- simply put, a core Freya abstraction which returns either Next or Halt, and a way of chaining these pipeline components together in a logical form.

See the guide [Composition with Freya.Pipeline][pipeline].

## High-Level Libraries

### Freya.Machine

### Freya.Router

[owin]: http://owin.org
[core-overview]: ./core-overview.md
[core-syntax]: ./core-computation-expressions-or-operators.md
[types-family]: ./types-understanding-the-family.md
[pipeline]: ./pipeline-composition.md

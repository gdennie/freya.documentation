# Freya.Core Overview

_Freya.Core_ provides a basic abstraction over [OWIN][owin], allowing web programming using OWIN to be done in a more idiomatically (and pleasantly) F# way.

While larger systems will normally use some (or all) of the higher level libraries that Freya provides (such as _Freya.Machine_), those higher level libraries are all built on the basic abstraction from _Freya.Core_. Even if you're using the higher level libraries, you'll usually end up writing code that uses the basic _Freya.Core_ abstraction for configuration, customization, etc.

You can write code that uses only _Freya.Core_ though, and it's a good place to start understanding what _Freya.Core_ provides.

## Hello World with Freya.Core

Here's our very simple Hello World using _Freya.Core_. It's just a console program, and we're using [Katana][katana] as our server (Freya doesn't include a web server -- it's designed to be used with an OWIN compatible server).

```fsharp
open System
open System.IO
open System.Text
open Freya.Core
open Microsoft.Owin.Hosting

let helloWorld =
    freya {
        let! state = getM

        let response = state.Environment.["owin.ResponseBody"] :?> Stream
        let helloWorld = Encoding.UTF8.GetBytes ("Hello World!")

        response.Write (helloWorld, 0, helloWorld.Length) }

type HelloWorld () =
    member __.Configuration () =
        OwinAppFunc.fromFreya (helloWorld)

[<EntryPoint>]
let main _ = 
    let _ = WebApp.Start<HelloWorld> ("http://localhost:7000")
    let _ = Console.ReadLine ()
    0
```
The first reaction here might be "Oh... That doesn't look very elegant at all. Do we really have to write stuff like that all the time?"

Thankfully the answer to that is no - it's not very common to end up writing much code that looks like this. That's what the higher level libraries are for. But it's good to follow this through and see what we're doing, as understanding this makes it easier to work out what's going on later.

### The Boilerplate

Let's deal with what the basic boilerplate parts of this are for. and then move on to the more interesting Freya.Core abstraction.

First of all, our `HelloWorld` type. Katana expects a .NET type with a `Configuration` method returning an OWIN `AppFunc` to be passed to the `WebApp.Start` method. _Freya.Core_ gives you an easy way to create a function you can return from that -- `OwinAppFunc.fromFreya`.

Secondly, our server itself. It's just a console application, so we call Katana and then block, so we don't immediately close our server down. Of course, as Freya sits on top of OWIN, it doesn't have to be a console application. You can host OWIN apps on many servers, including IIS, so applications built with the Freya stack are very portable.

### The Freya Computation Expression

Right, now we can move on to the useful parts. Minus the boilerplate that only ever gets written once per application (essentially the entry point to a Freya application) we're left with the interesting part:

```fsharp
let helloWorld =
    freya {
        let! state = getM

        let response = state.Environment.["owin.ResponseBody"] :?> Stream
        let helloWorld = Encoding.UTF8.GetBytes ("Hello World!")

        response.Write (helloWorld, 0, helloWorld.Length) }
```

The first thing to note here is that it's a computation expression. The `freya` computation expression crops up in many places, so it's worth getting used to. The `freya` computation expression has the following type:

```fsharp
type Freya<'a> = FreyaState -> Async<'a * FreyaState>
```
It's a function which takes a `FreyaState` (which is effectively a wrapper around the OWIN Environment -- a dictionary of data) and returns (asynchronously) a tuple of some value of type `'a` and a `FreyaState` (either the `FreyaState` provided, or a modified `FreyaState`). If you're familiar with common types of monad, it's an AsyncState monad, where the state is of type `FreyaState`. If that means nothing to you, don't worry -- Freya will still make sense and it's very simple to use when you're familiar with it.

For now all we really need to know is if you're asked to provide (perhaps as an argument to some function) a value of type `Freya<string>` the following would work:

```fsharp
let someFunction (required: Freya<string) =
    ...

let myRequired =
    freya {
        return "Hello World!" }

someFunction (myRequired) // This is fine
someFunction (freya { return "Hello World!" }) // This is also fine
```
### Reading/Writing Data

Now we get to the point of this computation expression - reading and writing the OWIN Environment.

If you're not familiar with the implementation of OWIN, it's quite simple. An OWIN compatible server exposes the content of a request, along with the content of a response (yet to be sent). This is exposed as a mutable dictionary of data (the Environment), which can be read and written, and which is referenced by the server. For example, the path of the request can be accessed at the key "owin.RequestPath". The type of the OWIN Environment (available within the FreyaState as the Environment property) is `IDictionary<string,obj>` so objects must be unboxed and boxed when reading and writing. The server is responsible for the transport between client and server, etc. leaving software built on OWIN responsible solely for manipulating the OWIN Environment to achieve responses to requests.

At this point, we should know enough to be able to make sense of our Hello World example. Here it is again, annotated:

```fsharp
let helloWorld =
    freya {
        // Get the state (FreyaState)
        let! state = getM

        // Get the response body and cast it to a Stream
        let response = state.Environment.["owin.ResponseBody"] :?> Stream

        // Encode our intended response body
        let helloWorld = Encoding.UTF8.GetBytes ("Hello World!")

        // Write our response to the stream
        response.Write (helloWorld, 0, helloWorld.Length) }
```
We've introduced one function here -- `getM`. It's a function of type `Freya<FreyaState>` which simply returns the current state. There's two companion functions available -- `setM` which sets the state to a new value and returns `unit`, and modM, which takes a function of `FreyaState -> FreyaState` and modifies the state "in place". You'll very rarely need to use any of these functions, they're very low level, but for this very basic example we needed `getM`.

### Using Lenses

The `getM / setM / modM` functions are rarely used as they require us to work at the level of the entire state. We don't usually care about this. In the previous example, we only wanted to deal with the response body.

Freya uses lenses to provide access to focused parts of the state data structure. If you're not familiar with lenses, you may want to read the [Introduction][aether-intro] and [Guide][aether-guide] to [Aether][aether] (the lens library that's used extensively throughout the Freya stack).

Let's see what our example looks like when we update it to use lenses.

```fsharp
let helloWorld =
    freya {
        // Get the response body as a Stream
        let response = getLM (environmentKeyLens<Stream> "owin.ResponseBody")

        // Encode our intended response body
        let helloWorld = Encoding.UTF8.GetBytes ("Hello World!")

        // Write our response to the stream
        response.Write (helloWorld, 0, helloWorld.Length) }
```
We can see that the process of getting the response stream is now a single operation, using the `getLM` function (note the additional "L" for lens). This function gets a part of the state, using a lens such as the argument shown. It also has companion functions `setLM` and `modLM`, which set and modify the state using a lens as you might expect.

In this case, we're using the `environmentKeyLens<'a>` function to create a lens. This function takes an argument (the key within the OWIN Environment to fetch) and a type parameter, indicating what type the resulting object should be unboxed as. This simplifies the process of working with data quite significantly.

### Partial Lenses

As noted in the Aether [Introduction][aether-intro], Aether supports partial lenses and these are supported in Freya through the use of the `getPLM`, `setPLM` and `modPLM` functions (note the additional "P" for partial). The `environmentKeyPLens<'a>` function is also provided, to create partial lenses for when the dictionary may not be guaranteed to contain the data. This is common within OWIN, as some data is optional.

### Higher Level Lenses

While the lenses for working with basic aspects of the OWIN Environment are useful, they're still quite low level, requiring us to work with aspects of HTTP (such as collections of headers, paths, etc.) as plain strings, or dictionaries of strings. This is possible, but tedious and error prone. To help with this the Freya stack provides the _Freya.Types.*_ family of libraries, which provides an extensive set of types and lenses to help work with the content of HTTP requests and responses in a typed, safe manner.

## Next Steps

Suggested next reads:

* [Understanding the Freya.Types.* family][types] -- a good introduction to the higher level lenses, and further examples of using lenses with the `freya` computation expression.
* [Computation Expressions or Operators][core-syntax] -- a look at alternative syntax for working with the `Freya<'a>` type using operators in contrast to computation expression syntax.

Further reading:

* [OWIN][owin] -- it's worth understanding the underlying OWIN abstraction as it's present throughout Freya and many other .NET Web frameworks/tools.
* [Aether Guide][aether-guide] -- referenced earlier in this guide, familiarity with the Aether style of lenses (and morphisms) will be useful when working with the Freya stack. 

[owin]: http://owin.org
[katana]: https://katanaproject.codeplex.com
[aether]: https://github.com/xyncro/aether
[aether-intro]: http://kolektiv.github.io/fsharp/aether/2014/08/10/aether/
[aether-guide]: http://kolektiv.github.io/fsharp/aether/2014/08/13/aether-guide/
[types]: ./types-understanding-the-family.md
[core-syntax]: ./core-computation-expressions-or-operators.md

# C-Sharp-Notes

https://mva.microsoft.com/en-us/training-courses/programming-in-c-jump-start-14254?l=j0iuozSfB_6900115888


### What is an Object?

- An object typically models a concept: – An object usually “is” something –i.e. a customer
- An object usually “has” data – i.e. the customer’s first name
- An object usually “performs” actions – i.e. make a customer “preferred

### What is Object Oriented Programming?

To be object oriented, a language is designed around the concept of objects. that are something, have certain properties and exhibit certain behaviors. This means the language generally includes support for:

- Encapsulation
- Inheritance
- Polymorphism
    
### What is a Managed Language?

- Managed languages depend on services provided by a runtime environment.
- C# is one of many languages that compile into managed code.
- Managed code is executed by the Common Language Runtime (CLR).
- The CLR provides features such as:
    - Automatic memory management
    - Exception Handling
    - Standard Types
    - Security
 
**Note: just like java gets compiled into bytecode first and excuted by JVM**

### Why Standard Types

- Foundational building block is a Type
  - Metadata about space allocation
  - Metadata for compile-time type checking

- All types have a common base 
  - Object–Object defines `ToString`, so every object has a `ToString` function
  
- There are 3 categories of types:
  - Value types - these directly store values
  - Reference types – or objects, store a reference to data
  - Pointer types – only available in unsafe code

### Encapsulation

- Encapsulation means to create a boundary around an object to separate its external (public) behavior from its internal (private) implementation. 
- Consumers of an object should only concern themselves with what an object does, not how it does it
- C# supports encapsulation via:
  - Unified Type System
  - Classes and Interfaces
  - Properties, Methods and Event
  
### Inheritance

- C# implements Inheritance in two ways:
  - A class may inherit from a single base class
  - A class may implement zero or more Interface
  
### Polymorphism

- A class can be used as its own type, cast to any base types or interface types it implements. 
- Objects may methods as virtual; virtual methods may be overridden in a derived type. These are executed instead of the base implementation.

### Developer Productivity

- From the outset, C# focused on making it easier for developers to solve complex tasks without compromising elegance.
- Examples:
  - var - simplifies variable definition while retaining strong typing
  - LINQ - language integrated query
  - Lambdas - a further refinement of anonymous methods used extensively in LINQ
  
### C# Syntax

- C# syntax is based on the C & C++ syntax.
- Identifiers are names of classes, methods, variables, and so on: 
  - `Lion`, `Sound`, `MakeSound()`, `Console`, `WriteLine()`
- Keywords are compiler reserved words:
  - `public`, `class`, `string`, `get`, `set`, `void`
  
```c#
public class Lion() {
    public string Sound {get; set}
    
    public void MakeSound() {
        Console.writeLine(Sound);
    }
}
```
### Namespace

Namespaces have the following properties:
  - They organize large code projects.
  - They are delimited by using the . operator.
  - The using directive obviates the requirement to specify the name of the namespace for every class.
  - The global namespace is the "root" namespace: global::System will always refer to the .NET Framework namespace System.

**Namespace Aliases**

The using Directive can also be used to create an alias for a namespace. For example, if you are using a previously written namespace that contains nested namespaces, you might want to declare an alias to provide a shorthand way of referencing one in particular, as in the following example:

```c#
using Co = Company.Proj.Nested;  // define an alias to represent a namespace
```

**Using Namespaces to control scope**

```c#
namespace SampleNamespace
{
    class SampleClass
    {
        public void SampleMethod()
        {
            System.Console.WriteLine(
              "SampleMethod inside SampleNamespace");
        }
    }

    // Create a nested namespace, and define another class.
    namespace NestedNamespace
    {
        class SampleClass
        {
            public void SampleMethod()
            {
                System.Console.WriteLine(
                  "SampleMethod inside NestedNamespace");
            }
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            // Displays "SampleMethod inside SampleNamespace."
            SampleClass outer = new SampleClass();
            outer.SampleMethod();

            // Displays "SampleMethod inside SampleNamespace."
            SampleNamespace.SampleClass outer2 = new SampleNamespace.SampleClass();
            outer2.SampleMethod();

            // Displays "SampleMethod inside NestedNamespace."
            NestedNamespace.SampleClass inner = new NestedNamespace.SampleClass();
            inner.SampleMethod();
        }
    }
}
```

First, remember that a namespace declaration with periods, like:

```c#
namespace MyCorp.TheProduct.SomeModule.Utilities
{
    ...
}
```

is entirely equivalent to:

```c#
namespace MyCorp
{
    namespace TheProduct
    {
        namespace SomeModule
        {
            namespace Utilities
            {
                ...
            }
        }
    }
}
```

If you wanted to, you could put using directives on all of these levels. (Of course, we want to have  usings in only one place, but it would be legal according to the language.)

The rule for resolving which type is implied, can be loosely stated like this: First search the inner-most "scope" for a match, if nothing is found there go out one level to the next scope and search there, and so on, until a match is found. If at some level more than one match is found, if one of the types are from the current assembly, pick that one and issue a compiler warning. Otherwise, give up (compile-time error).

Now, let's be explicit about what this means in a concrete example with the two major conventions.

1. With usings outside:

```c#
using System;
using System.Collections.Generic;
using System.Linq;
//using MyCorp.TheProduct;  <-- uncommenting this would change nothing
using MyCorp.TheProduct.OtherModule;
using MyCorp.TheProduct.OtherModule.Integration;
using ThirdParty;

namespace MyCorp.TheProduct.SomeModule.Utilities
{
    class C
    {
        Ambiguous a;
    }
}
```

In the above case, to find out what type Ambiguous is, the search goes in this order:

    1. Nested types inside `C` (including inherited nested types)
    2. Types in the current namespace `MyCorp.TheProduct.SomeModule.Utilities`
    3. Types in namespace `MyCorp.TheProduct.SomeModule`
    4. Types in `MyCorp.TheProduct`
    5. Types in `MyCorp`
    6. Types in the `global` namespace (the global namespace)
    7. Types in `System`, `System.Collections.Generic`, `System.Linq`, `MyCorp.TheProduct.OtherModule`, `MyCorp.TheProduct.OtherModule.Integration`, and `ThirdParty`
    
The other convention:

2. With usings inside:

```c#
namespace MyCorp.TheProduct.SomeModule.Utilities
{
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using MyCorp.TheProduct;                           // MyCorp can be left out; this using is NOT redundant
    using MyCorp.TheProduct.OtherModule;               // MyCorp.TheProduct can be left out
    using MyCorp.TheProduct.OtherModule.Integration;   // MyCorp.TheProduct can be left out
    using ThirdParty;

    class C
    {
        Ambiguous a;
    }
}
```

Now, search for the type Ambiguous goes in this order:

    1. Nested types inside `C` (including inherited nested types)
    2. Types in the current namespace `MyCorp.TheProduct.SomeModule.Utilities`
    3. Types in `System`, `System.Collections.Generic`, `System.Linq`, `MyCorp.TheProduct`, `MyCorp.TheProduct.OtherModule`,        `MyCorp.TheProduct.OtherModule.Integration`, and `ThirdParty`
    4. Types in namespace `MyCorp.TheProduct.SomeModule`
    5. Types in `MyCorp`
    6. Types in the `global` namespace (the global namespace)
    
(Note that `MyCorp.TheProduct` was a part of "3." and was therefore not needed between "4." and "5.".)

**Concluding remarks**

No matter if you put the usings inside or outside the namespace declaration, there's always the possibility that someone later adds a new type with identical name to one of the namespaces which have higher priority.

Also, if a nested namespace has the same name as a type, it can cause problems.

It is always dangerous to move the usings from one location to another because the search hierarchy changes, and another type may be found. Therefore, choose one convention and stick to it, so that you won't have to ever move usings.

Visual Studio's templates, by default, put the usings outside of the namespace (for example if you make VS generate a new class in a new file).

One (tiny) advantage of having usings outside is that you can then utilize the using directives for a global attribute, for example [assembly: ComVisible(false)] instead of [assembly: System.Runtime.InteropServices.ComVisible(false)].

### `using` statement inside and outside the namespace

There is actually a (subtle) difference between the two. Imagine you have the following code in **File1.cs**:

```c#
// File1.cs
using System;
namespace Outer.Inner
{
    class Foo
    {
        static void Bar()
        {
            double d = Math.PI;
        }
    }
}
```

Now imagine that someone adds another file (**File2.cs**) to the project that looks like this:

```c#
// File2.cs
namespace Outer
{
    class Math
    {
    }
}
```

The compiler searches Outer before looking at those using statements outside the namespace, so it finds Outer.Math instead of System.Math. Unfortunately (or perhaps fortunately?), Outer.Math has no PI member, so **File1** is now broken.

This changes if you put the using inside your namespace declaration, as follows:

```c#
// File1b.cs
namespace Outer.Inner
{
    using System;
    class Foo
    {
        static void Bar()
        {
            double d = Math.PI;
        }
    }
}
```

Now the compiler searches `System` before searching Outer, finds `System.Math`, and all is well.

Some would argue that Math might be a bad name for a user-defined class, since there's already one in `System`; the point here is just that there is a difference, and it affects the maintainability of your code.

It's also interesting to note what happens if Foo is in namespace `Outer`, rather than `Outer.Inner`. In that case, adding `Outer.Math` in **File2** breaks **File1** regardless of where the using goes. This implies that the compiler searches the innermost enclosing namespace before it looks at any `using` statements.

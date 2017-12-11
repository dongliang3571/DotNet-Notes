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


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
  - Pointer types – only available in unsafe cod

# C-Sharp-Notes

### .NET Standard, .NET Core and .NET Framework 

https://stackoverflow.com/questions/42939454/what-is-the-difference-between-net-core-and-net-standard-class-library-project

Creative answer

```c#
IAnimal == .NetStandard (General)
IBird == .NetCore (Less General)
IEagle == .NetFramework (Specific / oldest and has the most features)
```

### C Sharp Documentation

https://mva.microsoft.com/en-us/training-courses/programming-in-c-jump-start-14254?l=j0iuozSfB_6900115888

### Package Management


**An introduction to NuGet**

https://docs.microsoft.com/en-us/nuget/what-is-nuget

An essential tool for any modern development platform is a mechanism through which developers can create, share, and consume useful libraries of code. Such libraries are typically referred to as "packages" because they can contain compiled code (as DLLs) along with other content that might be needed in the projects that consume those libraries.+

For .NET, the mechanism for sharing code is **NuGet**, which defines how packages for .NET are created, hosted, and consumed, and provides the tools for each of those roles.

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
  
Structs do not support inheritance, but they can implement interfaces. For more information, see [Interfaces]https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/interfaces/index).

**`override`, `virtual`**

If a member class or member variable need to be overidden, you need to have keyword `virtual` in the memeber variable or method in the base class, and `override` is needed in derived class in order to override an variable or method.

This example defines a base class named `Employee`, and a derived class named `SalesEmployee`. The `SalesEmployee` class includes an extra property, `salesbonus`, and overrides the method `CalculatePay` in order to take it into account.

It is an error to use the `virtual` modifier on a static property

A virtual inherited property can be overridden in a derived class by including a property declaration that uses the `override` modifier.

An override method provides a new implementation of a member that is inherited from a base class. The method that is overridden by an override declaration is known as the overridden base method. The overridden base method must have the same signature as the override method. For information about inheritance, see Inheritance.

You cannot use the virtual modifier with the static, abstract, private, or override modifiers. The following example shows a virtual property

You cannot override a non-virtual or static method. The overridden base method must be `virtual`, `abstract`, or `override`.

An `override` declaration cannot change the accessibility of the `virtual` method. Both the `override` method and the `virtual` method must have the same access level modifier(`public`, `private`, etc.).

You cannot use the `new`, `static`, or `virtual` modifiers to modify an override method.

An overriding property declaration must specify exactly the same access modifier, type, and name as the inherited property, and the overridden property must be `virtual`, `abstract`, or `override`.

```c#
class TestOverride
{
    public class Employee
    {
        public string name;

        // Basepay is defined as protected, so that it may be 
        // accessed only by this class and derrived classes.
        protected decimal basepay;

        // Constructor to set the name and basepay values.
        public Employee(string name, decimal basepay)
        {
            this.name = name;
            this.basepay = basepay;
        }

        // Declared virtual so it can be overridden.
        public virtual decimal CalculatePay()
        {
            return basepay;
        }
    }

    // Derive a new class from Employee.
    public class SalesEmployee : Employee
    {
        // New field that will affect the base pay.
        private decimal salesbonus;

        // The constructor calls the base-class version, and
        // initializes the salesbonus field.
        public SalesEmployee(string name, decimal basepay, 
                  decimal salesbonus) : base(name, basepay)
        {
            this.salesbonus = salesbonus;
        }

        // Override the CalculatePay method 
        // to take bonus into account.
        public override decimal CalculatePay()
        {
            return basepay + salesbonus;
        }
    }

    static void Main()
    {
        // Create some new employees.
        SalesEmployee employee1 = new SalesEmployee("Alice", 
                      1000, 500);
        Employee employee2 = new Employee("Bob", 1200);

        Console.WriteLine("Employee4 " + employee1.name + 
                  " earned: " + employee1.CalculatePay());
        Console.WriteLine("Employee4 " + employee2.name + 
                  " earned: " + employee2.CalculatePay());
    }
}
/*
    Output:
    Employee4 Alice earned: 1500
    Employee4 Bob earned: 1200
*/
```

**`abstract`**

The `abstract` modifier indicates that the thing being modified has a missing or incomplete implementation. The abstract modifier can be used with classes, methods, properties, indexers, and events. Use the `abstract` modifier in a class declaration to indicate that a class is intended only to be a base class of other classes. Members marked as abstract, or included in an abstract class, must be implemented by classes that derive from the abstract class.

Abstract classes have the following features:

  - An abstract class cannot be instantiated.

  - An abstract class may contain abstract methods and accessors.

  - It is not possible to modify an abstract class with the sealed modifier because the two modifers have opposite meanings. The `sealed` modifier prevents a class from being inherited and the `abstract` modifier requires a class to be inherited.

  - A non-abstract class derived from an abstract class must include actual implementations of all inherited abstract methods and accessors.

  - Use the `abstract` modifier in a method or property declaration to indicate that the method or property does not contain implementation.

  - Abstract methods have the following features:

  - An abstract method is implicitly a virtual method.

  - Abstract method declarations are only permitted in abstract classes.

  - Because an abstract method declaration provides no actual implementation, there is no method body; the method declaration simply ends with a semicolon and there are no curly braces ({ }) following the signature. For example:

In this example, the class `Square` must provide an implementation of `Area` because it derives from `ShapesClass`:

```c#
abstract class ShapesClass
{
    abstract public int Area();
}
class Square : ShapesClass
{
    int side = 0;

    public Square(int n)
    {
        side = n;
    }
    // Area method is required to avoid
    // a compile-time error.
    public override int Area()
    {
        return side * side;
    }

    static void Main() 
    {
        Square sq = new Square(12);
        Console.WriteLine("Area of the square = {0}", sq.Area());
    }

    interface I
    {
        void M();
    }
    abstract class C : I
    {
        public abstract void M();
    }

}
// Output: Area of the square = 144
```

**`sealed`**

When applied to a class, the sealed modifier prevents other classes from inheriting from it. In the following example, class B inherits from class A, but no class can inherit from class B.

```c#
class A {}      
sealed class B : A {}
```

You can also use the `sealed` modifier on a method or property that overrides a virtual method or property in a base class. This enables you to allow classes to derive from your class and prevent them from overriding specific virtual methods or properties

In the following example, `Z` inherits from `Y` but `Z` cannot override the virtual function `F` that is declared in `X` and sealed in `Y`.

```c#
class X
{
    protected virtual void F() { Console.WriteLine("X.F"); }
    protected virtual void F2() { Console.WriteLine("X.F2"); }
}
class Y : X
{
    sealed protected override void F() { Console.WriteLine("Y.F"); }
    protected override void F2() { Console.WriteLine("Y.F2"); }
}
class Z : Y
{
    // Attempting to override F causes compiler error CS0239.
    // protected override void F() { Console.WriteLine("C.F"); }

    // Overriding F2 is allowed.
    protected override void F2() { Console.WriteLine("Z.F2"); }
}
```

When you define new methods or properties in a class, you can prevent deriving classes from overriding them by not declaring them as virtual.

It is an error to use the abstract modifier with a sealed class, because an abstract class must be inherited by a class that provides an implementation of the abstract methods or properties.

structs are implicitly sealed, they cannot be inherited.

**`readonly`**

The `readonly` keyword is a modifier that you can use on fields. When a field declaration includes a `readonly` modifier, assignments to the fields introduced by the declaration can only occur as part of the declaration or in a constructor in the same class.

In this example, the value of the field year cannot be changed in the method ChangeYear, even though it is assigned a value in the class constructor:

```c#
class Age
{
    readonly int _year;
    Age(int year)
    {
        _year = year;
    }
    void ChangeYear()
    {
        //_year = 1967; // Compile error if uncommented.
    }
}
```

You can assign a value to a readonly field only in the following contexts:

 - When the variable is initialized in the declaration, for example:
 
   ```
   public readonly int y = 5; 
   ```
 - For an instance field, in the instance constructors of the class that contains the field declaration, or for a static field, in the static constructor of the class that contains the field declaration. These are also the only contexts in which it is valid to pass a readonly field as an out or ref parameter.

**Note:**

The `readonly` keyword is different from the `const` keyword. A `const` field can only be initialized at the declaration of the field. A `readonly` field can be initialized either at the declaration or in a constructor. Therefore, `readonly` fields can have different values depending on the constructor used. Also, while a `const` field is a compile-time constant, the `readonly` field can be used for runtime constants as in the following example:

```c#
public static readonly uint timeStamp = (uint)DateTime.Now.Ticks; 
```

```c#
public class ReadOnlyTest
{
   class SampleClass
   {
      public int x;
      // Initialize a readonly field
      public readonly int y = 25;
      public readonly int z;

      public SampleClass()
      {
         // Initialize a readonly instance field
         z = 24;
      }

      public SampleClass(int p1, int p2, int p3)
      {
         x = p1;
         y = p2;
         z = p3;
      }
   }

   static void Main()
   {
      SampleClass p1 = new SampleClass(11, 21, 32);   // OK
      Console.WriteLine("p1: x={0}, y={1}, z={2}", p1.x, p1.y, p1.z);
      SampleClass p2 = new SampleClass();
      p2.x = 55;   // OK
      Console.WriteLine("p2: x={0}, y={1}, z={2}", p2.x, p2.y, p2.z);
   }
}
/*
 Output:
    p1: x=11, y=21, z=32
    p2: x=55, y=25, z=24
*/
```

**`const`**

You use the `const` keyword to declare a constant field or a constant local. Constant fields and locals aren't variables and may not be modified. Constants can be numbers, Boolean values, strings, or a null reference. Don’t create a constant to represent information that you expect to change at any time. For example, don’t use a constant field to store the price of a service, a product version number, or the brand name of a company. These values can change over time, and because compilers propagate constants, other code compiled with your libraries will have to be recompiled to see the changes

`const` implies `static` (you don't need an instance to reference the const value).

I want to also add this important point: When you link against (reference) an assembly with a public const, that value is copied into your assembly. So if the const value in the referenced assembly changes, your assembly will still have the originally compiled-in value.

If this behavior is not acceptable, then you should consider making the field a `public static readonly` field.

```c#
public class Foo {
    public const int HATS = 42;
    public static readonly int GLOVES = 33;
}
```

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

### Some rules

If you use the built-in templates to add classes to a folder, it will by default be put in a namespace that reflects the folder hierarchy.

The classes will be easier to find and that alone should be reasons good enough.

The rules we follow are:

- Project/assembly name is the same as the root namespace, except for the .dll ending
- Only exception to the above rule is a project with a .Core ending, the .Core is stripped off
- Folders equals namespaces
- One type per file (class, struct, enum, delegate, etc.) makes it easy to find the right file

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

### Attributes

https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/attributes/index

### Difference between struct and class in C sharp

Use structs if:

  - instances of the type are small
  - the struct is commonly embedded in another type
  - the structlogically represent a single value
  - the values don't change (immutable)
  - It is rarely "boxed"
  
structs are value types. (Always pass by values)

For example, say you have a class SimpleClass with two properties, A and B. You instantiate a copy of this class, initialize A and B, and then pass the instance to another method. That method further modifies A and B. Back in the calling function (the one that created the instance), your instance's A and B will have the values given to them by the called method.

Now, you make it a struct. The properties are still mutable. You perform the same operations with the same syntax as before, but now, A and B's new values aren't in the instance after calling the method. What happened? Well, your class is now a struct, meaning it's a value type. If you pass a value type to a method, the default (without an out or ref keyword) is to pass "by value"; a shallow copy of the instance is created for use by the method, and then destroyed when the method is done leaving the initial instance intact.

This becomes even more confusing if you were to have a reference type as a member of your struct (not disallowed, but extremely bad practice in virtually all cases); the class would not be cloned (only the struct's reference to it), so changes to the struct would not affect the original object, but changes to the struct's subclass WILL affect the instance from the calling code. This can very easily put mutable structs in very inconsistent states that can cause errors a long way away from where the real problem is.

**Always make your structures immutable**

Use class if:
  - you need a reference type
  - instances of the type are big
 
 Classes can optionally be declared as:
   - static - cannot ever be instantiated
   - abstracct - incomplete classs; must be completed in a derived class
   - sealed - cannot be inherited from

### Partial class

Each class in C# resides in a separate physical file with a .cs extension. C# provides the ability to have a single class implementation in multiple .cs files using the partial modifier keyword. The partial modifier can be applied to a class, method, interface or structure.

For example, the following MyPartialClass splits into two files, PartialClassFile1.cs and PartialClassFile2.cs:

```c#
// PartialClassFile1.cs

public partial class MyPartialClass
{
    public MyPartialClass()
    {
    }

    public void Method1(int val)
    {
        Console.WriteLine(val);
    }
}
```

```c#
public partial class MyPartialClass
{
    public void Method2(int val)
    {
        Console.WriteLine(val);
    }
}
```

MyPartialClass in PartialClassFile1.cs defines the constructor and one public method, Method1, whereas PartialClassFile2 has only one public method, Method2. The compiler combines these two partial classes into one class as below:

```c#
public class MyPartialClass
{
    public MyPartialClass()
    {
    }
        
    public void Method1(int val)
    {
        Console.WriteLine(val);
    }

    public void Method2(int val)
    {
        Console.WriteLine(val);
    }
}
```

Partial Class Requirements:

  - All the partial class definitions must be in the same assembly and namespace.
  - All the parts must have the same accessibility like public or private, etc.
  - If any part is declared abstract, sealed or base type then the whole class is declared of the same type.
  - Different parts can have different base types and so the final class will inherit all the base types.
  - The Partial modifier can only appear immediately before the keywords class, struct, or interface.
  - Nested partial types are allowed.

Advantages of Partial class:

  - Multiple developers can work simultaneously with a single class in separate files.
  - When working with automatically generated source, code can be added to the class without having to recreate the source file. For example, Visual Studio separates HTML code for the UI and server side code into two separate files: .aspx and .cs files.
  
The following example shows that nested types can be partial, even if the type they are nested within is not partial itself.

```c#
class Container
{
    partial class Nested
    {
        void Test() { }
    }
    partial class Nested
    {
        void Test2() { }
    }
}
```

At compile time, attributes of partial-type definitions are merged. For example, consider the following declarations:

```c#
[SerializableAttribute]
partial class Moon { }

[ObsoleteAttribute]
partial class Moon { }
```

They are equivalent to the following declarations:

```c#
[SerializableAttribute]
[ObsoleteAttribute]
class Moon { }
```

**Inheritance in partial class**

That's a single class defined across multiple declarations, not two different classes. You only need to define the inheritance model in a single declaration, e.g.:

```c#
public class Foo { }

//Bar extends Foo
public partial class Bar : Foo { }

public partial class Bar {  }
```

However, if you were to try the following, you'd generate a compiler error of "Partial declarations of 'Bar' must not specify different base classes":

```c#
public class Foo { }

public partial class Bar : Foo { }

public partial class Bar : object {  }
```

**Partial Methods:**

A partial class or struct may contain partial methods. A partial method must be declared in one of the partial classes. A partial method may or may not have an implementation. If the partial method doesn't have an implementation in any part then the compiler will not generate that method in the final class. For example, consider the following partial method with a partial keyword:

```c#
// PartialClassFile1.cs

public partial class MyPartialClass
{
    partial void PartialMethod(int val);

    public MyPartialClass()
    {
            
    }

    public void Method2(int val)
    {
        Console.WriteLine(val);
    }
}
```

```c#
// PartialClassFile2.cs
public partial class MyPartialClass
{
    public void Method1(int val)
    {
        Console.WriteLine(val);
    }

    partial void PartialMethod(int val)
    {
        Console.WriteLine(val);
    }
}
```

PartialClassFile1.cs contains the declaration of the partial method and PartialClassFile2.cs contains the implementation of the partial method.

Partial method requirements:
  - The partial method declaration must began with the partial modifier.
  - The partial method can have a ref but not an out parameter.
  - Partial methods are implicitly private methods.
  - Partial methods can be static methods.
  - Partial methods can be generic.

### Access Modifiers

All types and members have an accessibility level:

- public - The type or member can be accessed by any other code in the same assembly or another assembly that references it.
- private - The type or member can be accessed only by code in the same class or struct.
- protected - The type or member can be accessed only by code in the same class, or in a class that is derived from that class.
- internal - The type or member can be accessed by any code in the same assembly, but not from another assembly.
- protected internal - The type or member can be accessed by any code in the assembly in which it is declared, or from within a derived class in another assembly
- private protected - The type or member can be accessed only within its declaring assembly, by code in the same class or in a type that is derived from that class.

Access modifiers are not allowed on namespaces. Namespaces have no access restrictions.

Depending on the context in which a member declaration occurs, only certain declared accessibilities are permitted. If no access modifier is specified in a member declaration, a default accessibility is used.

Top-level types, which are not nested in other types, can only have `internal` or `public` accessibility. The default accessibility for these types is `internal`.

Nested types, which are members of other types, can have declared accessibilities as indicated in the following table.

|members of	| Default member accessibility	| Allowed declared accessibility of the member |
|-----------|-------------------------------|----------------------------------------------|
|enums      |public                         | None|
|class | private|public, protected, internal, private, protected internal, private protected|
|interface| public | None|
|struct| private| public, internal, private|

### Method parameters

**`params`**

By using the `params` keyword, you can specify a method parameter that takes a variable number of arguments.

You can send a comma-separated list of arguments of the type specified in the parameter declaration or an array of arguments of the specified type. You also can send no arguments. If you send no arguments, the length of the `params` list is zero.

No additional parameters are permitted after the `params` keyword in a method declaration, and only one `params` keyword is permitted in a method declaration.

The following example demonstrates various ways in which arguments can be sent to a `params` parameter.

```c#
public class MyClass
{
    public static void UseParams(params int[] list)
    {
        for (int i = 0; i < list.Length; i++)
        {
            Console.Write(list[i] + " ");
        }
        Console.WriteLine();
    }

    public static void UseParams2(params object[] list)
    {
        for (int i = 0; i < list.Length; i++)
        {
            Console.Write(list[i] + " ");
        }
        Console.WriteLine();
    }

    static void Main()
    {
        // You can send a comma-separated list of arguments of the 
        // specified type.
        UseParams(1, 2, 3, 4);
        UseParams2(1, 'a', "test");

        // A params parameter accepts zero or more arguments.
        // The following calling statement displays only a blank line.
        UseParams2();

        // An array argument can be passed, as long as the array
        // type matches the parameter type of the method being called.
        int[] myIntArray = { 5, 6, 7, 8, 9 };
        UseParams(myIntArray);

        object[] myObjArray = { 2, 'b', "test", "again" };
        UseParams2(myObjArray);

        // The following call causes a compiler error because the object
        // array cannot be converted into an integer array.
        //UseParams(myObjArray);

        // The following call does not cause an error, but the entire 
        // integer array becomes the first element of the params array.
        UseParams2(myIntArray);
    }
}
/*
Output:
    1 2 3 4
    1 a test
           
    5 6 7 8 9
    2 b test again
    System.Int32[]
*/
```

**`ref`**

https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/ref

The `ref` keyword indicates a value that is passed by reference. It is used in three different contexts:

  - In a method signature and in a method call, to pass an argument to a method by reference. See **Passing an argument by reference** for more information.

  - In a method signature, to return a value to the caller by reference. See **Reference return values** for more information.

  - In a member body, to indicate that a reference return value is stored locally as a reference that the caller intends to modify. See **Ref locals** for more information.

**Passing an argument by reference**

When used in a method's parameter list, the `ref` keyword indicates that an argument is passed by reference, not by value. The effect of passing by reference is that any change to the argument in the called method is reflected in the calling method. For example, if the caller passes a local variable expression or an array element access expression, and the called method replaces the object to which the ref parameter refers, then the caller’s local variable or the array element now refers to the new object when the method returns.

**Note:**

Do not confuse the concept of passing by reference with the concept of reference types. The two concepts are not the same. A method parameter can be modified by ref regardless of whether it is a value type or a reference type. There is no boxing of a value type when it is passed by reference.

To use a `ref` parameter, both the method definition and the calling method must explicitly use the ref keyword, as shown in the following example.

```c#
class RefExample
{
    static void Method(ref int i)
    {
        i = i + 44;
    }

    static void Main()
    {
        int val = 1;
        Method(ref val);
        Console.WriteLine(val);
        // Output: 45
    }
}
```

An argument that is passed to a `ref` parameter must be initialized before it is passed. This differs from out parameters, whose arguments do not have to be explicitly initialized before they are passed.

Members of a class can't have signatures that differ only by `ref` and `out`. A compiler error occurs if the only difference between two members of a type is that one of them has a `ref` parameter and the other has an `out` parameter. The following code, for example, doesn't compile.

```c#
class CS0663_Example
{
    // Compiler error CS0663: "Cannot define overloaded 
    // methods that differ only on ref and out".
    public void SampleMethod(out int i) { }
    public void SampleMethod(ref int i) { }
}
```

However, methods can be overloaded when one method has a `ref` or `out` parameter and the other has a value parameter, as shown in the following example.

```c#
class RefOverloadExample
{
    public void SampleMethod(int i) { }
    public void SampleMethod(ref int i) { }
}
```

**Passing an argument by reference: An example**

The previous examples pass value types by reference. You can also use the ref keyword to pass reference types by reference. Passing a reference type by reference enables the called method to replace the object to which the reference parameter refers in the caller. The storage location of the object is passed to the method as the value of the reference parameter. If you change the value in the storage location of the parameter (to point to a new object), you also change the storage location to which the caller refers. The following example passes an instance of a reference type as a ref parameter.

```c#
class RefExample2
{
    static void Main()
    {
        // Declare an instance of Product and display its initial values.
        Product item = new Product("Fasteners", 54321);
        System.Console.WriteLine("Original values in Main.  Name: {0}, ID: {1}\n",
            item.ItemName, item.ItemID);

        // Pass the product instance to ChangeByReference.
        ChangeByReference(ref item);
        System.Console.WriteLine("Back in Main.  Name: {0}, ID: {1}\n",
            item.ItemName, item.ItemID);
    }

    static void ChangeByReference(ref Product itemRef)
    {
        // Change the address that is stored in the itemRef parameter.   
        itemRef = new Product("Stapler", 99999);

        // You can change the value of one of the properties of
        // itemRef. The change happens to item in Main as well.
        itemRef.ItemID = 12345;
    }
}

class Product
{
    public Product(string name, int newID)
    {
        ItemName = name;
        ItemID = newID;
    }

    public string ItemName { get; set; }
    public int ItemID { get; set; }
}
// Output: 
//        Original values in Main.  Name: Fasteners, ID: 54321
//    
//        Back in Main.  Name: Stapler, ID: 12345
```

**`out`**

The `out` keyword causes arguments to be passed by reference. It is like the ref keyword, except that ref requires that the variable be initialized before it is passed. To use an `out` parameter, both the method definition and the calling method must explicitly use the out keyword. For example:

```c#
using System;

class OutExample
{
   static void Method(out int i)
   {
      i = 44;
   }
   
   static void Main()
   {
      int value;
      Method(out value);
      Console.WriteLine(value);     // value is now 44
   }
}
```

**Declaring `out` arguments**

Declaring a method with out arguments is useful when you want a method to return multiple values. The following example uses out to return three variables with a single method call. Note that the third argument is assigned to null. This enables methods to return values optionally

```c#
class OutReturnExample
{
    static void Method(out int i, out string s1, out string s2)
    {
        i = 44;
        s1 = "I've been returned";
        s2 = null;
    }

    static void Main()
    {
        int value;
        string str1, str2;
        Method(out value, out str1, out str2);
        // value is now 44
        // str1 is now "I've been returned"
        // str2 is (still) null;
    }
}
```

The **Try pattern**(`Int32.TryParse(string str, out int val)`) involves returning a `bool` to indicate whether an operation succeeded and failed, and returning the value produced by the operation in an `out` argument. A number of parsing methods, such as the DateTime.TryParse method, use this pattern

**Calling a method with an `out` argument**

In C# 6 and earlier, you must declare a variable in a separate statement before you pass it as an out argument. The following example declares a variable named number before it is passed to the Int32.TryParse method, which attempts to convert a string to a number.

```c#
using System;

public class Example
{
   public static void Main()
   {
      string value = "1640";

      int number;
      if (Int32.TryParse(value, out number))
         Console.WriteLine($"Converted '{value}' to {number}");
      else
         Console.WriteLine($"Unable to convert '{value}'");   
   }
}
// The example displays the following output:
//       Converted '1640' to 1640
```

Starting with C# 7, you can declare the out variable in the argument list of the method call, rather than in a separate variable declaration. This produces more compact, readable code, and also prevents you from inadvertently assigning a value to the variable before the method call. The following example is like the previous example, except that it defines the number variable in the call to the Int32.TryParse method.

```c#
using System;

public class Example
{
   public static void Main()
   {
      string value = "1640";

      if (Int32.TryParse(value, out int number))
         Console.WriteLine($"Converted '{value}' to {number}");
      else
         Console.WriteLine($"Unable to convert '{value}'");   
   }
}
// The example displays the following output:
//       Converted '1640' to 1640
```

In the previous example, the number variable is strongly typed as an int. You can also declare an implicitly typed local variable, as the following example does.

```c#
using System;

public class Example
{
   public static void Main()
   {
      string value = "1640";

      if (Int32.TryParse(value, out var number))
         Console.WriteLine($"Converted '{value}' to {number}");
      else
         Console.WriteLine($"Unable to convert '{value}'");   
   }
}
// The example displays the following output:
//       Converted '1640' to 1640
```

### Asynchronous programming with async and await (C#)

https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/async/index

## Types

The C# typing system contains the following categories:

  - Value types

  - Reference types

  - Pointer types

### Value Types

The value types consist of two main categories:

  - Structs

  - Enumerations

Structs fall into these categories:

  - Numeric types

  - Integral types

  - Floating-point types

  - decimal

  - bool

  - User defined structs.
  
All built-in value types:

  - bool
  - byte
  - char
  - decimal
  - double
  - enum
  - float
  - int
  - long
  - sbyte
  - short
  - struct
  - uint
  - ulong
  - ushort

### Reference Types

The following keywords are used to declare reference types:

  - class

  - interface

  - delegate

C# also provides the following built-in reference types:

  - dynamic

  - object

  - string

**`string`**

The `string` type represents a sequence of zero or more Unicode characters. `string` is an alias for String in the .NET Framework.

Although `string` is a reference type, the equality operators (`==` and `!=`) are defined to compare the values of `string` objects, not references. This makes testing for string equality more intuitive. For example:

```c#
string a = "hello";  
string b = "h";  
// Append to contents of 'b'  
b += "ello";  
Console.WriteLine(a == b);  // Return True
Console.WriteLine((object)a == (object)b);  // return False
```

This displays "True" and then "False" because the content of the strings are equivalent, but a and b do not refer to the same string instance.

The `+` operator concatenates strings:

```c#
string a = "good " + "morning";
```

This creates a string object that contains "good morning".

Strings are immutable--the contents of a string object cannot be changed after the object is created, although the syntax makes it appear as if you can do this. For example, when you write this code, the compiler actually creates a new string object to hold the new sequence of characters, and that new object is assigned to b. The string "h" is then eligible for garbage collection.

```c#
string b = "h";  
b += "ello";
```

The [] operator can be used for readonly access to individual characters of a string:

```c#
string str = "test";  
char x = str[2];  // x = 's';
```

String literals are of type string and can be written in two forms, quoted and @-quoted. Quoted string literals are enclosed in double quotation marks ("):

```c#
"good morning"  // a string literal
```

String literals can contain any character literal. Escape sequences are included. The following example uses escape sequence `\\` for backslash, `\u0066` for the letter `f`, and `\n` for newline.

```c#
string a = "\\\u0066\n";  
Console.WriteLine(a);
```

Verbatim string literals start with @ and are also enclosed in double quotation marks. For example:

```c#
@"good morning"  // a string literal
```

The advantage of verbatim strings is that escape sequences are not processed, which makes it easy to write, for example, a fully qualified file name

```c#
@"c:\Docs\Source\a.txt"  // rather than "c:\\Docs\\Source\\a.txt"
```

To include a double quotation mark in an @-quoted string, double it:

```c#
@"""Ahoy!"" cried the captain." // "Ahoy!" cried the captain
```

Another use of the @ symbol is to use referenced (/reference) identifiers that are C# keywords.

**Best practices for using strings**

https://docs.microsoft.com/en-us/dotnet/standard/base-types/best-practices-strings

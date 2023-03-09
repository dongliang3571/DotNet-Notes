# DotNet-Notes

### binding redirects

Why are binding redirects needed at all? Suppose you have application A that references library B, and also library C of version 1.1.2.5. Library B in turn also references library C, but of version 1.1.1.0. Now we have a conflict, because you cannot load different versions of the same assembly at runtime. To resolve this conflict you might use binding redirect, usually to the new version (but can be to the old too). You do that by adding the following to app.config file of application A, under configuration > runtime > assemblyBinding section (see here for an example of full config file):

```
<dependentAssembly>
    <assemblyIdentity name="C"  
                      publicKeyToken="32ab4ba45e0a69a1"  
                      culture="en-us" />  

    <bindingRedirect oldVersion="1.1.1.0" newVersion="1.1.2.5" />  
</dependentAssembly>
```

You can also specify a range of versions to map:

```
<bindingRedirect oldVersion="0.0.0.0-1.1.1.0" newVersion="1.1.2.5" />  
```

Now library B, which was compiled with reference to C of version 1.1.1.0 will use C of version 1.1.2.5 at runtime. Of course, you better ensure that library C is backwards compatible or this might lead to unexpected results.

You can redirect any versions of libraries, not just major ones

### WaitHandle, ManualSetEvent, AutoSetEvent

https://jonskeet.uk/csharp/threads/waithandles.html

### Entity Framework

**Eagerly loading**

https://docs.microsoft.com/en-us/ef/ef6/querying/related-data

### Problems frequently Occur

**1.**

```
// When use this line to check if services has started, make sure you intialization takes less than the TimeSpan you defined
controller.WaitForStatus(ServiceControllerStatus.Running, TimeSpan.FromSeconds(10));
```

you might often see:

  - Service start() failed: Time out has expired and the operation has not been completed.
  - Cannot start service <service_name> on computer '.'.
  - Error 1053: The service did not respond to the start or control request in a timely fashion (when you start it on service manager)

Solution:

  - Remove that line of code 
  - or make sure intialization takes less than the timespan you defined

**2.**

The "obj" folder is used to store temporary object files and other files used in order to create the final binary during the compilation process.

The "bin" folder is the output folder for complete binaries (assemblies).

**3.**

`List<T>.Add(T)`

`.Add(T)` will only add references of objects. there if original obj was changed, element in the List changes as well

```c#
class Animal
{
    public int age;
    public Animal(int age)
    {
        this.age = age; 
    }
}

class Program
{
    public static void Main(string[] args)
    {
        Animal one = new Animal(4);
        List<Animal> ani = new List<Animal>();
        ani.Add(one);
        
        Console.WriteLine(one.age); // output: 4
        Console.WriteLine(ani[0].age); // output: 4
        
        one.age = 5;
        
        Console.WriteLine(one.age); // output: 5
        Console.WriteLine(ani[0].age); // output: 5
    }
}
```

**4.**

Each project in a solution is consider an assembly. Each assembly will read it's App.config(After build, its name becomes `<AppName>.exe.config` or `<AppName>Test.dll.config`), this behavior includes Unit Test project.

In order to have service like email or logging which depend on configs to work, we need to create it's own App.config for each project.

Same thing for unit testing project, the email won't work if testing project doesn't have its own email config

**5.**

**Moq issues**

When you change/add/remove a field in a Model, re-run tests, you will get errors like `The model backing the 'EFDbContextProxy' context has changed`

```
StackTrace: 
    at System.Data.Entity.CreateDatabaseIfNotExists`1.InitializeDatabase(TContext context)
       at System.Data.Entity.Internal.InternalContext.<>c__DisplayClassf`1.<CreateInitializationAction>b__e()
```

basically there is cached temp database, you want to delele them and re-run test should be fine

Cache files are located in `/Users/<username>/TigraDBConnection.mdf` and `/Users/<username>/TigraDBConnection_log.Idf`

Some helpful information: https://stackoverflow.com/questions/31564127/the-model-backing-the-efdbcontextproxy-context-has-changed-testing-cache#answer-54406602

### .NET Standard, .NET Core and .NET Framework 

https://stackoverflow.com/questions/42939454/what-is-the-difference-between-net-core-and-net-standard-class-library-project

Creative answer

```c#
IAnimal == .NetStandard (General)
IBird == .NetCore (Less General)
IEagle == .NetFramework (Specific / oldest and has the most features)
```

### Mock framework

https://docs.microsoft.com/en-us/ef/ef6/fundamentals/testing/mocking

### Unit testing

https://msdn.microsoft.com/en-us/library/ms182532.aspx

https://docs.microsoft.com/en-us/aspnet/core/mvc/controllers/testing?view=aspnetcore-3.1

### Respository and unit of work pattern

The repository and unit of work patterns are intended to create an abstraction layer between the data access layer and the business logic layer of an application. Implementing these patterns can help insulate your application from changes in the data store and can facilitate automated unit testing or test-driven development (TDD).

https://docs.microsoft.com/en-us/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application

### handle concurrency

https://docs.microsoft.com/en-us/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application

**Initializer and CleanUp**

https://msdn.microsoft.com/en-us/library/microsoft.visualstudio.testtools.unittesting.testcleanupattribute.aspx

`TestInitialize` runs before every test that is declared on the the same class where the attribute is declared.

`ClassInitialize` runs only on the initialization of the class where the attribute is declared. In other words it won't run for every class. Just for the class that contains the ClassInitialize method.

If you want a method that will run once before all tests or classes' initialization use the `AssemblyInitialize`.


**Access private memeber for testing purpose**

https://msdn.microsoft.com/en-us/library/ms243741.aspx


```c#
using Microsoft.VisualStudio.TestTools.UnitTesting;

var obj = new object();
var wrapper = new PrivateObject(obj);
wrapper.Invoke(method_name, parameters_array);
```

### C Sharp Documentation

https://mva.microsoft.com/en-us/training-courses/programming-in-c-jump-start-14254?l=j0iuozSfB_6900115888

### Package Management

**An introduction to NuGet**

https://docs.microsoft.com/en-us/nuget/what-is-nuget

An essential tool for any modern development platform is a mechanism through which developers can create, share, and consume useful libraries of code. Such libraries are typically referred to as "packages" because they can contain compiled code (as DLLs) along with other content that might be needed in the projects that consume those libraries.+

For .NET, the mechanism for sharing code is **NuGet**, which defines how packages for .NET are created, hosted, and consumed, and provides the tools for each of those roles.

### Configurations, `appSettings`, `applicationSettings`

https://msdn.microsoft.com/en-us/library/ms228154(v=vs.100).aspx

```xml
  <appSettings>
    <add key="key1" value="value"/>
    <add key="key2" value="5000"/>
  </appSettings>
  <applicationSettings>
    <SFCasesSvc.Properties.Settings>
      <setting name="value1" serializeAs="String">
        <value>http://something1/</value>
      </setting>
      <setting name="value2" serializeAs="String">
        <value>http://something2/</value>
      </setting>
      <setting name="value3" serializeAs="String">
        <value>http://something3/</value>
      </setting>
      <setting name="value4" serializeAs="String">
        <value>http://something4/</value>
      </setting>
    </SFCasesSvc.Properties.Settings>
  </applicationSettings>
  <userSettings>
    <SFCasesSvc.Properties.Settings>
      <setting name="somename" serializeAs="String">
        <value>http://something4/</value>
      </setting>
      <setting name="somename" serializeAs="String">
        <value />
      </setting>
    </SFCasesSvc.Properties.Settings>
  </userSettings>
```

**using application settings and user settings**

https://docs.microsoft.com/en-us/dotnet/framework/winforms/advanced/using-application-settings-and-user-settings

**How `App.Debug.config`, `App.Release.config` work**

For example, you have Three files, `App.config`, `App.Debug.config`, and `App.Release.config`

`App.config`

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <appSettings>
    <add key="key1" value="5000"/>
    <add key="key2" value="5000"/>
  </appSettings>
</configuration>
```

`App.Debug.config`

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <appSettings>
    <add key="key1" value="900000" xdt:Transform="Replace" xdt:Locator="Match(key)"/>
    <add key="key2" value="5000"/>
  </appSettings>
</configuration>
```

`App.Release.config`

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <appSettings>
    <add key="key1" value="800000" xdt:Transform="Replace" xdt:Locator="Match(key)"/>
    <add key="key2" value="5000"/>
  </appSettings>
</configuration>
```

When it's in Debug mode, the final generated config file aka. `ProjectName.exe.config` will have

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <appSettings>
    <add key="key1" value="900000"/> <-- it's replaced by DEBUG value -->
    <add key="key2" value="5000"/>
  </appSettings>
</configuration>
```

When it's in Release mode, the final generated config file aka. `ProjectName.exe.config` will have

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <appSettings>
    <add key="key1" value="800000"/> <-- it's replaced by Release value -->
    <add key="key2" value="5000"/>
  </appSettings>
</configuration>
```

Note: `xdt:Transform="Replace"` is called transformation syntax, see also https://msdn.microsoft.com/en-us/library/dd465326(v=vs.110).aspx


### Entity Framework - Object-Relational Mapper (ORM)

https://msdn.microsoft.com/en-us/library/gg696172(v=vs.103).aspx
https://www.codeproject.com/Articles/363040/An-Introduction-to-Entity-Framework-for-Absolute-B

### Debug a Windows service application

https://docs.microsoft.com/en-us/dotnet/framework/windows-services/how-to-debug-windows-service-applications

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
  
Structs do not support inheritance, but they can implement interfaces. For more information, see [Interfaces](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/interfaces/index).

**`base`**

The `base` keyword is used to access members of the base class from within a derived class:

  - Call a method on the base class that has been overridden by another method.

  - Specify which base-class constructor should be called when creating instances of the derived class.

A base class access is permitted only in a constructor, an instance method, or an instance property accessor.

It is an error to use the base keyword from within a static method.

**Calling base constructor**

```c#
class A
{
    public A()
    {
        Console.WriteLine("Calling A");
    }
}


class B: A
{
    public B()
    {
        Console.WriteLine("Calling B");
    }
}

public static void Main(string[] args)
{
    var n = new B();
}

// output:
// Calling A
// Calling B
```

Even if you don't use `base`, it still calls the default base class construct. However if you have more than one constructor, you can use `base` to call the one you want.

```c#
public class BaseClass
{
    int num;

    public BaseClass()
    {
        Console.WriteLine("in BaseClass()");
    }

    public BaseClass(int i)
    {
        num = i;
        Console.WriteLine("in BaseClass(int i)");
    }

    public int GetNum()
    {
        return num;
    }
}

public class DerivedClass : BaseClass
{
    // This constructor will call BaseClass.BaseClass()
    public DerivedClass() : base()
    {
    }

    // This constructor will call BaseClass.BaseClass(int i)
    public DerivedClass(int i) : base(i)
    {
    }

    static void Main()
    {
        DerivedClass md = new DerivedClass();
        DerivedClass md1 = new DerivedClass(1);
    }
}
/*
Output:
in BaseClass()
in BaseClass(int i)
*/
```

Another example calling non-constructot menthods

```c#
public class Person
{
    protected string ssn = "444-55-6666";
    protected string name = "John L. Malgraine";

    public virtual void GetInfo()
    {
        Console.WriteLine("Name: {0}", name);
        Console.WriteLine("SSN: {0}", ssn);
    }
}
class Employee : Person
{
    public string id = "ABC567EFG";
    public override void GetInfo()
    {
        // Calling the base class GetInfo method:
        base.GetInfo();
        Console.WriteLine("Employee ID: {0}", id);
    }
}

class TestClass
{
    static void Main()
    {
        Employee E = new Employee();
        E.GetInfo();
    }
}
/*
Output
Name: John L. Malgraine
SSN: 444-55-6666
Employee ID: ABC567EFG
*/
```

```c#
abstract class A
{
    public A()
    {
        Console.WriteLine("Calling A");
    }
    
    public static void cc()
    {
        Console.WriteLine("Calling static cc");
    }
    
    public static B dd()
    {
        return new B();
    }
}

class B: A
{
    public B()
    {
        Console.WriteLine("Calling B");
    }
}

public void static Main(string[] args)
{
    B bInstance = A.dd(); // This will work
}
```

Above example is useful in a standard library `[System.Net.WeRequest.Create()]`(https://docs.microsoft.com/en-us/dotnet/api/system.net.webrequest.create?view=netframework-4.7.1#System_Net_WebRequest_Create_System_String_)

`System.Net.WeRequest` is an `abstract` class too, `System.Net.WeRequest.Create()` is a static method that returns `HttpWebRequest` which is subclass of `WeRequest`.

### Modifiers

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

**static constructor**

https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/static-constructors


static contructor is thread safe

http://team4talent.be/Blog/Article/e698fb20-3003-4b28-a063-28bfbca8a5f4

### Finalize Method and Destructors

https://msdn.microsoft.com/library/0s71x931(v=vs.110).aspx

**Note:**

Empty destructors should not be used. When a class contains a destructor, an entry is created in the Finalize queue. When the destructor is called, the garbage collector is invoked to process the queue. If the destructor is empty, this just causes a needless loss of performance.

 - Destructors cannot be defined in structs. They are only used with classes.

 - A class can only have one destructor.

 - Destructors cannot be inherited or overloaded.

 - Destructors cannot be called. They are invoked automatically.

 - A destructor does not take modifiers or have parameters.

**Example**

The following example creates three classes that make a chain of inheritance. The class First is the base class, Second is derived from First, and Third is derived from Second. All three have destructors. In Main(), an instance of the most-derived class is created. When the program runs, notice that the destructors for the three classes are called automatically, and in order, from the most-derived to the least-derived.

```c#
class First
{
    ~First()
    {
        System.Diagnostics.Trace.WriteLine("First's destructor is called.");
    }
}

class Second : First
{
    ~Second()
    {
        System.Diagnostics.Trace.WriteLine("Second's destructor is called.");
    }
}

class Third : Second
{
    ~Third()
    {
        System.Diagnostics.Trace.WriteLine("Third's destructor is called.");
    }
}

class TestDestructors
{
    static void Main()
    {
        Third t = new Third();
    }

}
/* Output (to VS Output Window):
    Third's destructor is called.
    Second's destructor is called.
    First's destructor is called.
*/
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

### Properties

```c#
class TimePeriod
{
    private double seconds;

    public double Hours
    {
        get { return seconds / 3600; }
        set { seconds = value * 3600; }
    }
}


class Program
{
    static void Main()
    {
        TimePeriod t = new TimePeriod();

        // Assigning the Hours property causes the 'set' accessor to be called.
        t.Hours = 24;

        // Evaluating the Hours property causes the 'get' accessor to be called.
        System.Console.WriteLine("Time in hours: " + t.Hours);
    }
}
// Output: Time in hours: 24
```

**Empty setter and getter**

public field can be inherited but not overridable, whereas public property can be inherited and overidable. That is, in inheriting class you can have `virtual` keyword for property but not for field. In inhertied class you can have `override` keyword for property but not for fields.

```c#
public class Foo {
  public virtual int MyField = 1; // Nope, this can't

  public virtual int Bar {get; set; }
}

public class MyDerive : Foo {
  public override MyField; // Nope, this can't

  public override int Bar {
    get {
      //do something;
    }
    set; }
}
```

### Attributes

https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/attributes/index

**Difference between attribute and interface**

A long time ago in a galaxy far, far away... There were no Attributes or compiler support for class metadata, so the developers tried to implement their own. One of the methods our ancestors worked out were to declare Marker Interfaces .

So, to answer your question: custom attributes are an "evolution" of marker interfaces. You can use both. But note that, if you want to enforce that your object implement specific methods, you are using an interface, plain and simple. That's how IDisposable works, it forces you to implement a method named Dispose(). [Serializable] (and probably ISerializable on your C++ example) does not force you to implement anything, as the runtime will just read that declaration and do its task (i.e., serialize the object).

Note that C# also have a ISerializable interface... It is meant to let you write your custom serialization code, which will then be called by the runtime. Note that it is NOT a marker interface nor replacement for the [Serializable] attribute, as you still need to mark your class with the attribute for the serialization to work.

**Marker interface**

The marker interface pattern is a design pattern in computer science, used with languages that provide run-time type information about objects. It provides a means to associate metadata with a class where the language does not have explicit support for such metadata.

To use this pattern, a class implements a marker interface[1] (also called tagging interface), and methods that interact with instances of that class test for the existence of the interface. Whereas a typical interface specifies functionality (in the form of method declarations) that an implementing class must support, a marker interface need not do so. The mere presence of such an interface indicates specific behavior on the part of the implementing class. Hybrid interfaces, which both act as markers and specify required methods, are possible but may prove confusing if improperly used.

**An example of the application of marker interfaces from the Java programming language is the `Serializable` interface. A class implements this interface to indicate that its non-transient data members can be written to an `ObjectOutputStream`. The `ObjectOutputStream` private method `writeObject0(Object,boolean)` contains a series of `instanceof` tests to determine writeability, one of which looks for the `Serializable` interface. If any of these tests fails, the method throws a NotSerializableException.**

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

The default access for everything in C# is **"the most restricted access you could declare for that member"**

for example:

```c#
namespace MyCompany
{
    class Outer
    {
        void Foo() {}
        class Inner {}
    }
}
```

is equivalent to

```c#
namespace MyCompany
{
    internal class Outer
    {
        private void Foo() {}
        private class Inner {}
    }
}
```

The one sort of exception to this is making one part of a property (usually the setter) more restricted than the declared accessibility of the property itself:

```c#
public string Name
{
    get { ... }
    private set { ... } // This isn't the default, have to do it explicitly
}
```

**Top-level types**, which are not nested in other types, can only have `internal` or `public` accessibility. The default accessibility for these types is `internal`.

Note: 

**Top-level types are defined directly inside a namespac**

```c#
namespace Foo
{
    class Bar {} // top-level class
}
```

**Nested types are defined inside another class or struct**

```c#
namespace Foo
{
    class Bar
    {
        class Baz {} // nested type
    }
}
```

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

### Named Arguments

Named arguments free you from the need to remember or to look up the order of parameters in the parameter lists of called methods. The parameter for each argument can be specified by parameter name. For example, a function that prints order details (such as, seller name, order number & product name) can be called in the standard way by sending arguments by position, in the order defined by the function.

```c#
PrintOrderDetails("Gift Shop", 31, "Red Mug");
```

If you do not remember the order of the parameters but know their names, you can send the arguments in any order.

```c#
PrintOrderDetails(orderNum: 31, productName: "Red Mug", sellerName: "Gift Shop");

PrintOrderDetails(productName: "Red Mug", sellerName: "Gift Shop", orderNum: 31);
```

**Example**

The following code implements the examples from this section along with some additional ones.

```c#
class NamedExample
{
    static void Main(string[] args)
    {
        // The method can be called in the normal way, by using positional arguments.
        PrintOrderDetails("Gift Shop", 31, "Red Mug");

        // Named arguments can be supplied for the parameters in any order.
        PrintOrderDetails(orderNum: 31, productName: "Red Mug", sellerName: "Gift Shop");
        PrintOrderDetails(productName: "Red Mug", sellerName: "Gift Shop", orderNum: 31);

        // Named arguments mixed with positional arguments are valid
        // as long as they are used in their correct position.
        PrintOrderDetails("Gift Shop", 31, productName: "Red Mug");
        PrintOrderDetails(sellerName: "Gift Shop", 31, productName: "Red Mug");    // C# 7.2 onwards
        PrintOrderDetails("Gift Shop", orderNum: 31, "Red Mug");                   // C# 7.2 onwards

        // However, mixed arguments are invalid if used out-of-order.
        // The following statements will cause a compiler error.
        // PrintOrderDetails(productName: "Red Mug", 31, "Gift Shop");
        // PrintOrderDetails(31, sellerName: "Gift Shop", "Red Mug");
        // PrintOrderDetails(31, "Red Mug", sellerName: "Gift Shop");
    }

    static void PrintOrderDetails(string sellerName, int orderNum, string productName)
    {
        if (string.IsNullOrWhiteSpace(sellerName))
        {
            throw new ArgumentException(message: "Seller name cannot be null or empty.", paramName: nameof(sellerName));
        }

        Console.WriteLine($"Seller: {sellerName}, Order #: {orderNum}, Product: {productName}");
    }
}
```

### Optional Arguments

https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/named-and-optional-arguments

The definition of a method, constructor, indexer, or delegate can specify that its parameters are required or that they are optional. Any call must provide arguments for all required parameters, but can omit arguments for optional parameters.

Each optional parameter has a default value as part of its definition. If no argument is sent for that parameter, the default value is used. A default value must be one of the following types of expressions:

  - a constant expression;

  - an expression of the form new ValType(), where ValType is a value type, such as an enum or a struct;

  - an expression of the form default(ValType), where ValType is a value type.

Optional parameters are defined at the end of the parameter list, after any required parameters. If the caller provides an argument for any one of a succession of optional parameters, it must provide arguments for all preceding optional parameters. Comma-separated gaps in the argument list are not supported. For example, in the following code, instance method ExampleMethod is defined with one required and two optional parameters.

```c#
public void ExampleMethod(int required, string optionalstr = "default string", int optionalint = 10)
```

The following call to ExampleMethod causes a compiler error, because an argument is provided for the third parameter but not for the second.

```c#
anExample.ExampleMethod(3, ,4);
```

However, if you know the name of the third parameter, you can use a named argument to accomplish the task

```c#
anExample.ExampleMethod(3, optionalint: 4);
```

**Example**

In the following example, the constructor for `ExampleClass` has one parameter, which is optional. Instance method `ExampleMethod` has one required parameter, required, and two optional parameters, `optionalstr` and `optionalint`. The code in Main shows the different ways in which the constructor and method can be invoked.

```c#
namespace OptionalNamespace
{
    class OptionalExample
    {
        static void Main(string[] args)
        {
            // Instance anExample does not send an argument for the constructor's
            // optional parameter.
            ExampleClass anExample = new ExampleClass();
            anExample.ExampleMethod(1, "One", 1);
            anExample.ExampleMethod(2, "Two");
            anExample.ExampleMethod(3);

            // Instance anotherExample sends an argument for the constructor's
            // optional parameter.
            ExampleClass anotherExample = new ExampleClass("Provided name");
            anotherExample.ExampleMethod(1, "One", 1);
            anotherExample.ExampleMethod(2, "Two");
            anotherExample.ExampleMethod(3);

            // The following statements produce compiler errors.

            // An argument must be supplied for the first parameter, and it
            // must be an integer.
            //anExample.ExampleMethod("One", 1);
            //anExample.ExampleMethod();

            // You cannot leave a gap in the provided arguments. 
            //anExample.ExampleMethod(3, ,4);
            //anExample.ExampleMethod(3, 4);

            // You can use a named parameter to make the previous 
            // statement work.
            anExample.ExampleMethod(3, optionalint: 4);
        }
    }

    class ExampleClass
    {
        private string _name;

        // Because the parameter for the constructor, name, has a default
        // value assigned to it, it is optional.
        public ExampleClass(string name = "Default name")
        {
            _name = name;
        }

        // The first parameter, required, has no default value assigned
        // to it. Therefore, it is not optional. Both optionalstr and 
        // optionalint have default values assigned to them. They are optional.
        public void ExampleMethod(int required, string optionalstr = "default string",
            int optionalint = 10)
        {
            Console.WriteLine("{0}: {1}, {2}, and {3}.", _name, required, optionalstr,
                optionalint);
        }
    }

    // The output from this example is the following:
    // Default name: 1, One, and 1.
    // Default name: 2, Two, and 10.
    // Default name: 3, default string, and 10.
    // Provided name: 1, One, and 1.
    // Provided name: 2, Two, and 10.
    // Provided name: 3, default string, and 10.
    // Default name: 3, default string, and 4.

}
```

### Delegate

https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/delegates/how-to-declare-instantiate-and-use-a-delegate

In C# 1.0 and later, delegates can be declared as shown in the following example.

```c#
// Declare a delegate.
delegate void Del(string str);

// Declare a method with the same signature as the delegate.
static void Notify(string name)
{
    Console.WriteLine("Notification received for: {0}", name);
}

// Create an instance of the delegate.
Del del1 = new Del(Notify);
```

C# 2.0 provides a simpler way to write the previous declaration, as shown in the following example.

```c#
// C# 2.0 provides a simpler way to declare an instance of Del.
Del del2 = Notify;
```

In C# 2.0 and later, it is also possible to use an anonymous method to declare and initialize a delegate, as shown in the following example.

```c#
// Instantiate Del by using an anonymous method.
Del del3 = delegate(string name)
    { Console.WriteLine("Notification received for: {0}", name); };
```

In C# 3.0 and later, delegates can also be declared and instantiated by using a lambda expression, as shown in the following example

```c#
// Instantiate Del by using a lambda expression.
Del del4 = name =>  { Console.WriteLine("Notification received for: {0}", name); };
```

### Asynchronous programming with async and await (C#)

https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/async/index

https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/await

https://blog.stephencleary.com/2012/02/async-and-await.html


The `async` keyword enables the `await` keyword in that method and changes how method results are handled. That’s all the async keyword does! It does not run this method on a thread pool thread, or do any other kind of magic. The `async` keyword only enables the `await` keyword (and manages the method results).

The beginning of an async method is executed just like any other method. That is, it runs synchronously until it hits an “await” (or throws an exception).

`await` waits Task(Awaitable), not the method

```c#
public async Task NewStuffAsync()
{
  // Use await and have fun with the new stuff.
  await ...
}

public Task MyOldTaskParallelLibraryCode()
{
  // Note that this is not an async method, so we can't use await in here.
  ...
}

public async Task ComposeAsync()
{
  // We can await Tasks, regardless of where they come from.
  await NewStuffAsync();
  await MyOldTaskParallelLibraryCode();
}
```

Async methods returning `Task` or `void` do not have a return value. Async methods returning `Task<T>` must return a value of type `T`:

```c#
public async Task<int> CalculateAnswer()
{
  await Task.Delay(100); // (Probably should be longer...)

  // Return a type of "int", not "Task<int>"
  return 42;
}
```

It's very similar to Swift closure

```swift
// In Swft you have:

// declare function definition
func download(completion: @escape (() -> Void)) {
  downloading(completionHandler: {
    completion() // this is executed after downloading is finished
  })
}

// use function
download(completion: {
  // do something
})
```

```c#

public async Task download() {
  await downloading();
  
  completion(); // this is execute after downloading is finished
}

```

**Async Composition**

So far, we’ve only considered serial composition: an async method waits for one operation at a time. It’s also possible to start several operations and await for one (or all) of them to complete. You can do this by starting the operations but not awaiting them until later:

```c#
public async Task DoOperationsConcurrentlyAsync()
{
  Task[] tasks = new Task[3];
  tasks[0] = DoOperation0Async();
  tasks[1] = DoOperation1Async();
  tasks[2] = DoOperation2Async();

  // At this point, all three tasks are running at the same time.

  // Now, we await them all.
  await Task.WhenAll(tasks);
}

public async Task<int> GetFirstToRespondAsync()
{
  // Call two web services; take the first response.
  Task<int>[] tasks = new[] { WebService1Async(), WebService2Async() };

  // Await for the first one to respond.
  Task<int> firstTask = await Task.WhenAny(tasks);

  // Return the result.
  return await firstTask;
}
```

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

### C# Singleton. `lock` keyword, `volatile` keyword 

http://csharpindepth.com/Articles/General/Singleton.aspx

**Potentially unsafe double-checked locking. Use `volatile` variable(s) or synchronization primitives to avoid this.**

https://www.viva64.com/en/w/v3054/

The analyzer detected a possible error related to unsafe use of the "double-checked locking" pattern. This software design pattern is used to reduce the overhead of acquiring a lock by first testing the locking criterion without actually acquiring the lock. Only if the locking criterion check indicates that locking is required, does the actual locking logic proceed. That is, locking will be performed only if really needed.

Consider the following example of unsafe implementation of this pattern in C#:

```c#
private MyClass _singleton = null;
public MyClass Singleton
{
    get
    {
        if(_singleton == null)
            lock(_locker)
            {
                if(_singleton == null)
                {
                    MyClass instance = new MyClass();
                    instance.Initialize();
                    _singleton = instance;
                }
            }
        return _singleton;
    }
}
```

In this example, the pattern is used to implement "lazy initialization" – that is, initialization is delayed until a variable's value is needed for the first time. This code will work correctly in a program that uses a singleton object from one thread. To ensure safe initialization in a multithreaded program, a construct with the lock statement is usually used. However, it's not enough in our example.

Note the call to method `Initialize()` of the 'Instance' object. When building the program in Release mode, the compiler may optimize this code and invert the order of assigning the value to the `_singleton` variable and calling to the `Initialize()` method. In that case, another thread accessing `Singleton` at the same time as the initializing thread may get access to the object before initialization is over.

Here's another example of using the double-checked locking pattern:

```c#
private MyClass _singleton = null;
bool _initialized = false;
public MyClass Singleton;
{
    get
    {
        if(!_initialized)
            lock(_locker)
            {
                if(!_initialized)
                {
                    _singleton = new MyClass();
                    _initialized = true;
                }
            }
        return _singleton;
    }
}
```

Like in the previous example, compiler optimization of the order of assigning values to variables `_singleton` and `_initialized` may cause errors. That is, the `_initialized` variable will be assigned the value 'true' first, and only then will a new object, `MyClass()`, be created and the reference to it be assigned to `_singleton`.

Such inversion may cause an error when accessing the object from a parallel thread. It turns out that the '_singleton' variable will not be specified yet while the '_initialized' flag will be already set to 'true'.

One of the dangers of these errors is the seeming correctness of the program's functioning. Such false impression occurs because this problem won't occur very often and will depend on the architecture of the processor used, CLR version, and so on.

There are several ways to ensure thread-safety when using the pattern. The simplest way is to mark the variable checked in the if condition with the `volatile` keyword:

```c#
private volatile MyClass _singleton = null;
public MyClass Singleton
{
    get
    {
        if(_singleton == null)
            lock(_locker)
            {
                if(_singleton == null)
                {
                    MyClass instance = new MyClass();
                    instance.Initialize();
                    _singleton = instance;
                }
            }
        return _singleton;
    }
}
```

The `volatile` keyword will prevent the variable from being affected by possible compiler optimizations related to swapping write/read instructions and caching its value in processor registers.

For performance reasons, it's not always a good solution to declare a variable as volatile. In that case, you can use the following methods to access the variable: 'Thread.VolatileRead', 'Thread.VolatileWrite', and 'Thread.MemoryBarrier'. These methods will put barriers for reading/writing memory only where necessary.

Finally, you can implement "lazy initialization" using the Lazy<T> class, which was designed specifically for this purpose and is available in .NET starting with version 4.

### Anonymous types

https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/anonymous-types

```c#
// The following example shows an anonymous type that is initialized with two properties named Amount and Message.

var v = new { Amount = 108, Message = "Hello" };  
  
// Rest the mouse pointer over v.Amount and v.Message in the following  
// statement to verify that their inferred types are int and string.  
Console.WriteLine(v.Amount + v.Message);  
```

### default value expressions

A default value expression produces the default value for a type. Default value expressions are particularly useful in generic classes and methods. One issue that arises using generics is how to assign a default value to a parameterized type `T` when you do not know the following in advance:

  - Whether `T` is a reference type or a value type.
  - If `T` is a value type, whether is a numeric value or a user-defined struct.

Given a variable t of a parameterized type T, the statement t = null is only valid if T is a reference type. The assignment t = 0 only works for numeric value types but not for structs. The solution is to use a default value expression, which returns null for reference types (class types and interface types) and zero for numeric value types. For user-defined structs, it returns the struct initialized to the zero bit pattern, which produces 0 or null for each member depending on whether that member is a value or reference type. For nullable value types, default returns a `System.Nullable<T>`, which is initialized like any struct.
  
The `default(T)` expression is not limited to generic classes and methods. Default value expressions can be used with any managed type. Any of these expressions are valid:

```c#
var s = default(string);
var d = default(dynamic);
var i = default(int);
var n = default(int?); // n is a Nullable int where HasValue is false.
```

The following example from the `GenericList<T>` class shows how to use the `default(T)` operator in a generic class. For more information, see Generics Overview.

```c#
namespace ConsoleApplication1
{
    class Program
    {
        static void Main(string[] args)
        {
            // Test with a non-empty list of integers.
            GenericList<int> gll = new GenericList<int>();
            gll.AddNode(5);
            gll.AddNode(4);
            gll.AddNode(3);
            int intVal = gll.GetLast();
            // The following line displays 5.
            System.Console.WriteLine(intVal);

            // Test with an empty list of integers.
            GenericList<int> gll2 = new GenericList<int>();
            intVal = gll2.GetLast();
            // The following line displays 0.
            System.Console.WriteLine(intVal);

            // Test with a non-empty list of strings.
            GenericList<string> gll3 = new GenericList<string>();
            gll3.AddNode("five");
            gll3.AddNode("four");
            string sVal = gll3.GetLast();
            // The following line displays five.
            System.Console.WriteLine(sVal);

            // Test with an empty list of strings.
            GenericList<string> gll4 = new GenericList<string>();
            sVal = gll4.GetLast();
            // The following line displays a blank line.
            System.Console.WriteLine(sVal);
        }
    }

    // T is the type of data stored in a particular instance of GenericList.
    public class GenericList<T>
    {
        private class Node
        {
            // Each node has a reference to the next node in the list.
            public Node Next;
            // Each node holds a value of type T.
            public T Data;
        }

        // The list is initially empty.
        private Node head = null;

        // Add a node at the beginning of the list with t as its data value.
        public void AddNode(T t)
        {
            Node newNode = new Node();
            newNode.Next = head;
            newNode.Data = t;
            head = newNode;
        }

        // The following method returns the data value stored in the last node in
        // the list. If the list is empty, the default value for type T is
        // returned.
        public T GetLast()
        {
            // The value of temp is returned as the value of the method. 
            // The following declaration initializes temp to the appropriate 
            // default value for type T. The default value is returned if the 
            // list is empty.
            T temp = default(T);

            Node current = head;
            while (current != null)
            {
                temp = current.Data;
                current = current.Next;
            }
            return temp;
        }
    }
}
```

**default literal and type inference**

Beginning with C# 7.1, the default literal can be used for default value expressions when the compiler can infer the type of the expression. The default literal produces the same value as the equivalent default(T) where T is the inferred type. This can make code more concise by reducing the redundancy of declaring a type more than once. The default literal can be used in any of the following locations:

  - variable initializer
  - variable assignment
  - declaring the default value for an optional parameter
  - providing the value for a method call argument
  - return statement (or expression in an expression bodied member)
  
The following example shows many usages of the default literal in a default value expression

```c#
public class Point
{
    public double X { get; }
    public double Y { get; }

    public Point(double x, double y)
    {
        X = x;
        Y = y;
    }
}

public class LabeledPoint
{
    public double X { get; private set; }
    public double Y { get; private set; }
    public string Label { get; set; }

    // Providing the value for a default argument:
    public LabeledPoint(double x, double y, string label = default)
    {
        X = x;
        Y = y;
        this.Label = label;
    }

    public static LabeledPoint MovePoint(LabeledPoint source, 
        double xDistance, double yDistance)
    {
        // return a default value:
        if (source == null)
            return default;

        return new LabeledPoint(source.X + xDistance, source.Y + yDistance, 
        source.Label);
    }

    public static LabeledPoint FindClosestLocation(IEnumerable<LabeledPoint> sequence, 
        Point location)
    {
        // initialize variable:
        LabeledPoint rVal = default;
        double distance = double.MaxValue;

        foreach (var pt in sequence)
        {
            var thisDistance = Math.Sqrt((pt.X - location.X) * (pt.X - location.X) +
                (pt.Y - location.Y) * (pt.Y - location.Y));
            if (thisDistance < distance)
            {
                distance = thisDistance;
                rVal = pt;
            }
        }

        return rVal;
    }

    public static LabeledPoint ClosestToOrigin(IEnumerable<LabeledPoint> sequence)
        // Pass default value of an argument.
        => FindClosestLocation(sequence, default);
}
```

### Interface

**Difference between implement interface implicitly and explicitly**

Basically:

Implicit: you access the interface methods and properties as if they were part of the class.
Explicit: you can only access methods and properties when treating the class as the implemented interface.
Code examples:

```c#
public class Test : ITest
{
    public string Id // Generated by Implement Interface Implicitly
    {
        get { throw new NotImplementedException(); }
    }

    string ITest.Id // Generated by Implement Interface Explicitly
    {
        get { throw new NotImplementedException(); }
    }
}
```

Implicit:

```c#
Test t = new Test();
t.Id; // OK
((ITest)t).Id; // OK
```

Explicit:

```c#
Test t = new Test();
t.Id; // Not OK
((ITest)t).Id; // OK
```

### Extension methods

The following example shows an extension method defined for the System.String class. Note that it is defined inside a non-nested, non-generic static class:

```c#
namespace ExtensionMethods
{
    public static class MyExtensions
    {
        public static int WordCount(this String str)
        {
            return str.Split(new char[] { ' ', '.', '?' }, 
                             StringSplitOptions.RemoveEmptyEntries).Length;
        }
    }   
}
```

**Inheritance with extension method**

```c#
//Rextester.Program.Main is the entry point for your code. Don't change it.
//Compiler version 4.0.30319.17929 for Microsoft (R) .NET Framework 4.5

using System;
using System.Collections.Generic;
using System.Linq;
using System.Text.RegularExpressions;

namespace Rextester
{
    class Te
    {
        
    }
    
    static class Test
    {
        public static void extend(this Te source) // add a extension method to Te
        {
            Console.WriteLine("Extend");
        }
    }
    
    class Tes: Te // subclass from Te
    {
        public void extend<T>() // create a method with same name but it's generic
        {
            Console.WriteLine("Extendddddddddddddddddddddddddd");
        }
    }
    public class Program
    {
        public static void Main(string[] args)
        {   
            Te test1 = new Te();
            test1.extend(); // outputs: Extend
            
            Tes test2 = new Tes();
            test2.extend(); // outputs: Extend
            test2.extend<int>(); // outputs: Extendddddddddddddddddddddddddd
        }
    }
    
    
}
```

### `DllImportAttrubute`

Indicates that the attributed method is exposed by an unmanaged dynamic-link library (DLL) as a static entry point

https://msdn.microsoft.com/en-us/library/system.runtime.interopservices.dllimportattribute(v=vs.110).aspx

https://msdn.microsoft.com/en-us/library/system.runtime.interopservices.callingconvention(v=vs.110).aspx

```c#
using System;
using System.Runtime.InteropServices;

class Example
{
    // Use DllImport to import the Win32 MessageBox function.
    [DllImport("user32.dll", CharSet = CharSet.Unicode)]
    public static extern int MessageBox(IntPtr hWnd, String text, String caption, uint type);

    static void Main()
    {
        // Call the MessageBox function using platform invoke.
        MessageBox(new IntPtr(0), "Hello World!", "Hello Dialog", 0);
    }
}
```

The following example demonstrates how to replace MessageBoxA with MsgBox in your code by using the EntryPoint field.

```c#
using System.Runtime.InteropServices;  

public class Win32 {  
    [DllImport("user32.dll", EntryPoint="MessageBoxA")]  
    public static extern int MsgBox(int hWnd, String text, String caption,  
                                    uint type);  
}
```

## ASP.NET

https://dotnettutorials.net/lesson/asp-dot-net-mvc-viewbag/

### MVC

**Example of ViewBag in ASP.NET MVC:**

Let us see an example to understand how to use the new dynamic type ViewBag in ASP.NET MVC to pass data from a controller action method to a view. We are going to work with the same example that we worked in our previous article with ViewData. So, modify the Index action method of HomeController class as shown below.

```c#
using FirstMVCDemo.Models;
using System.Web.Mvc;
namespace FirstMVCDemo.Controllers
{
    public class HomeController : Controller
    {
        public ActionResult Index()
        {
            EmployeeBusinessLayer employeeBL = new EmployeeBusinessLayer();
            Employee employee = employeeBL.GetEmployeeDetails(101);
            
            ViewBag.Employee = employee;
            ViewBag.Header = "Employee Details";
            
            return View();
        }
    }
}
```

**Accessing the ViewBag in a View in ASP.NET MVC**

Now we will see how to access the ViewBag data within an ASP.NET MVC view. So, modify the Index.cshtml view file as shown below.

```cshtml
@{
    Layout = null;
}
<!DOCTYPE html>
<html>
<head>
    <meta name="viewport" content="width=device-width" />
    <title>Page Title</title>  
</head>
<body>
    @{
        var employee = ViewBag.Employee;
    }
    <h2>@ViewBag.Header</h2>
    <table style="font-family:Arial">
        <tr>
            <td>Employee ID:</td>
            <td>@employee.EmployeeId </td>
        </tr>
        <tr>
            <td>Name:</td>
            <td>@employee.Name</td>
        </tr>
        <tr>
            <td>Gender:</td>
            <td>@employee.Gender</td>
        </tr>
        <tr>
            <td>City:</td>
            <td>@employee.City</td>
        </tr>
        <tr>
            <td>Salary:</td>
            <td>@employee.Salary</td>
        </tr>
        <tr>
            <td>Address:</td>
            <td>@employee.Address</td>
        </tr>
    </table>
</body>
</html>
```

**Note:** The ViewBag is a dynamic property that is also resolved at runtime like ViewData; as a result, here also it will not provide compile-time error checking as well as intelligence support. For example, if we miss-spell the property names of the ViewBag, then we wouldn’t get any compile-time error rather we came to know about the error at runtime.

**ViewBag**

https://stackoverflow.com/questions/16949468/how-does-viewbag-in-asp-net-mvc-work-behind-the-scenes

ViewBag is a property of ControllerBase. It is defined as follows:

```c#
public Object ViewBag { get; }
Note that this signature is actually incorrect. Here's what the source code actually looks like:

public dynamic ViewBag {
    get {
        if (_dynamicViewDataDictionary == null) {
            _dynamicViewDataDictionary = new DynamicViewDataDictionary(() => ViewData);
        }
        return _dynamicViewDataDictionary;
    }
}
    
```
`_dynamicViewDataDictionary` is an `ExpandoObject`; you can `add properties to it at runtime`. Its lifetime is the same as that of the controller, which is the lifetime of the HTTP request.

**ViewBag, ViewData,TempDate**

https://www.infragistics.com/community/blogs/b/dhananjay_kumar/posts/what-are-viewdata-viewbag-and-tempdata-in-asp-net-mvc
https://www.infragistics.com/community/blogs/b/dhananjay_kumar/posts/what-are-viewdata-viewbag-and-tempdata-in-asp-net-mvc

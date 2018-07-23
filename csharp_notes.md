C# Notes
========

References:

* [Microsoft: C# Tour].

Tidbits
=======

non-nullable vs nullable value types
------------------------------------

* non-nullable: That type only (eg. `int` can only contain integers).
* nullable: That type or `null`. (eg. `int` can contain integers or `null`).

Accessibility
-------------

* `public`
* `protected`: This or derived classes.
* `internal`: Current assembly (`.exe`, `.dll`, etc).
* `private`: This class only.

Inheritance
-----------

Inheritance is done by adding the class you want to inherit from to your class
with a colon. eg. `public class AdditionalClass: MyBaseClass {...}`.

You can use `: base(correct, number, of, args)` to reference the base classes
constructor, so that you only have to call your additional changes in your
constructor.

The base class type can be used when creating a variable that is referencing an
instance of the same class type or an instance of a derived class type.

Glossary
========

* REPL: Read-Eval-Print Loop interface. Interactive code prompt.
* BYOC: Bring your own Container.
* Boxing: Converting from a Value Type to a Reference Type Object. The `object`
  instance is the _"box"_ that holds a copy of that value.

```c#
int i = 123;
object o = i;  // Boxing
```

* Unboxing: Converting from a Reference Type Object, back to a Value Type. Type
  checks are done with a copy being made if the checks succeed.

```c#
int i = 123;
object o = i;  // Boxing
int j = (int)o;  // Unboxing
```

* Non-nullable value type _(only that type)_.
* Nullable value type _(That type or `null` value)_.
* Object reference type _(`null` reference, object reference, reference to a
  boxed value type)_.
* Class reference type _( `null` reference, class instance reference, derived
  class instance reference)_.
* Interface reference type _(`null` reference, class instance reference that
  implements interface, reference to a boxed value type that implements
  interface)_.
* Array reference type _(`null` reference, array instance reference
  (same/compatible array type)_.
* Delegate reference type _(`null` reference, compatible delegate instance
  reference)_.
* Assembly: Library/package/module. Built as an `.exe` executable or `.dil`
  library. They contain Intermediate Language (IL) instructions & metadata.
* `.dll`: Dynamic?? Interchangeable Library. Built Assembly
  (Module/package/library) that can be referenced by an application via `using`
  and a reference flag at build time.

```bash
csc /r MyCustom.dll MySource.cs
```

* JIT (Just-In-Time): compiler of .NET Common Language Runtime.
* Generic class type: Class with angle brackets (`<`, `>`) with type
  parameters within.
* Constructed type: A generic type with arguments provided.

Class Members
=============

* Classes are stored on the heap.
* objects created per instance.

Field Arguments
---------------

Fields denote storage locations.

* `readonly`: Constant with value calculated at Runtime. JIT compiler
  optimisations when paired with `static`. See [SO: readonly benefits].
* `const`: Constant with value calculated at Compile time. See
  [SO: readonly benefits]. Slightly better performance than `readonly`, but
  issue if dll A has `const`, dll B uses dll A. dll A changes `const`
  value. dll B needs recompile before new value is used.
* `static`: Single class value (no copies per class instance). Does not require
  a class instance to access.

Properties
----------

Similar to a field, but runs `accessors` instead of storage locations.

* Use `{` & `}` delimiters instead of `;`.
* May contain a `get` and/or `set` accessor.
* Property can be defined `static`.
* Accessors can be defined `virtual`, `abstract`, `override`.
* Example property construction:

```c#
public int Thing
{
    // `stuff` is initialized as a class or constructor variable.
	// `value` is the input arg when used.
    get { return stuff }
	set { stuff = value + 5 }
}
```

* Example property usage:

```c#
myClass.Thing = 100;  // Invokes set accessor.
int i = myClass.Thing;  // Invokes get accessor.
```

Indexers
--------

Same as a property except name member is followed by a list parameter, to allow
objects to be indexed as a list (Think Python `__iter__` function to make it
iterable).

* Only need 1 per-class to make it iterable.
* Can overloaded for multiple indexer declarations, as long as there is no
  type/number overlaps for parameters.

```c#
public T this[int index]
{
    get { return items[index]; }
    set
    {
        items[index] = value;
        OnChanged();  // Method call that marks an event on the class instance.
    }
}
```

Event
-----

Field declaration which your class does work/changes that should be notified to
the User of your class instance.

* Example event construction:

```c#
// `Changed.Invoke()` is called elsewhere when work is done and an event needs
//  to be fired off.
public event EventHandler Changed;
```

* Example event usage:

```c#
static void MyFuncThatDoesSomethingWhenEventFires(object sender, EventArgs e)
{ ... }

// Add an Event Listener.
myClass.Changed += new EventHandler(MyFuncThatDoesSomethingWhenEventFires);
// Event fires and calls `MyFuncThatDoesSomethingWhenEventFires`.
myClass.DoSomething(...);
// Remove Event Listener with `-=`.
```

* Event declaration can explicitly provide `add`, `remove` accessors.
* Add/remove event listeners with `+=`, `-=`.

Operators
---------

`operator` members can apply an expression to a class. eg. Comparator
expression to a class for `instanceA == instanceB` or `instanceA !=
instanceB`. To avoid reference comparisons.

* Example operator construction:

```c#
public static bool operator ==(MyClass<T> a, MyClass<T> b) =>
    Equals(a, b);
```

* Example operator usage:

```c#
MyClass<int> a = new MyClass<int>();
MyClass<int> b = new MyCLass<int>();
// manipulate both class instances...
Console.WriteLine(a == b);  // Outputs either `True`, `False`.
```
* See: [Microsoft: Lambda Operator `=>`].

Finalizers
----------

* Like a de-constructor for the class.
* No parameters, no accessibility modifiers, cannot be explicitly invoked.
* Called by garbage collector on **any** thread at a non-deterministic time.
* `using` provides better destruction. i.e. **don't use finalizers!!**

Methods
-------

* Parameters: used to pass values/references into a method.
* Method: function.
* Overload: Multiple methods with the same name but different signature of
  input parameters (Types/Argument numbers).
* `ref`: reference parameter to pass arguments by reference into a method.

```c#
using System;

class RefExample
{
    static void Swap(ref int x, ref int y)
    {
        int temp = x;
        x = y;
        y = temp;
    }

public static void SwapExample()
    {
        int i = 1, j = 2;
        Swap(ref i, ref j);
        Console.WriteLine($"{i} {j}");  // Outputs "2 1"
    }
}
```

* `out`: output parameter to get arguments out of a method without a
  `return`. This is defined in the method parameters.

```c#
using System;

class OutExample
{
    static void Divide(int x, int y, out int result, out int remainder)
    {
        result = x / y;
        remainder = x % y;
    }

    public static void OutUsage()
    {
        Divide(10, 3, out int res, out int rem);
        Console.WriteLine("{0} {1}", res, rem); // Outputs "3 1"
    }
}
```

* `param`: Parameter array. The last parameter of a method may be an array. Can
  be invoked with a single array or multiple arguments of the Array Type. eg.
  `public static void Write(string fmt, params object() args) {...}`.
* `virtual`: runtime Type of the invoked instance will determine the
  implementation. If absent (non-virtual), done at compile time.
* `override`: Override an inherited `virtual` method, with the same signature.
* `abstract`: `virtual` method with no implementation (used in
  interfaces/abstract classes), therefore must be overridden.

Structs
=======

* Like classes but stores the data directly on the `struct`.
* Struct's are Value Types, so copies instead of references (beware of swapping
  between classes/structs if you do/don't want the side-effect of updating
  references from multiple sources).
* Structs copying is more expensive when passing assignment/parameters around,
  compared to a reference.
* Cannot reference a `struct`, without explicit `in`, `ref`, `out` parameters.

Arrays
======

* Array elements all must have the same element type.
* Array types are reference types.
* Array declarations are done at runtime via the `new` operator.
* Array's are fixed length!
* Use comma's to define multidimensional arrays:
  `int[,,] a = new int[10, 5, 2]`.
* Jagged Array: a nested array, where the nested arrays may be of different
  lengths.

```c#
int[][] a = new int[3][];
a[0] = new int[10];
a[1] = new int[5];
a[2] = new int[20];
```

* Array initializer: `int[] a = new int[] {1, 2, 3};` or:
  `int[] a = {1, 2, 3};`. Length is inferred.
* For dynamic lists see: [MSDN: Generic Lists], [SO: dynamic array].

Interfaces
==========

* Multiple inheritance: `interface IMyInterface: IThing1, IThing2 {...}`.
* Classes/Structs can implement multiple interfaces:
  `public class MyClass: IMyInterface, IAnotherOne {...}`.
* Can use the interface as the type for that class/struct.
* Can use dynamic type cats if instance is not statically known to implement
  the interface.

```c#
MyClass myClass = new MyClass();
IMyInterface myInterface = (IMyInterface)myClass;
```

* (explicit) Interface Member Implementations: allows class/struct to avoid
  public members by using the fully qualified interface member name.

```c#
public class MyClass: IMyInterface, IAnother {
    void IMyInterface.thing() {...}
	void IAnother.thing2(ThingType t) {...}
}
```

Enums
=====

* Enum creation:

```c#
enum Colour
{
	Red,
	Green,
	Blue
}
```

* Underlying type: or Integral Type is `int` if not defined. eg.
  `int i = (int)Colour.Blue;  // int i = 2;` or
  `Colour c = (Colour)2 // Colour c = Colour.Blue;`
* Enum's are initialised from `0`.
* Enum with defined type:

```c#
enum Alignment: sbyte
{
	Left = -1,
	Center = 0,
	Right = 1
}
```

Delegate
========

* `delegate` Type: reference to method with a particular parameter list &
  return type.
* Like object-oriented & type-safe function pointers.
* If the defined delegate variable is in your methods parameter list, then you
  can pass in any uncalled method in and then call it within your method.

```c#
delegate double DeleFunc(double x);

class MyClass {
	static double[] DoStuff(double[] a, DeleFunc f) {
		double result = new double[a.Length];
		for (int i = 0; i < a.Length; i++) {
			result[i] = f(a[i]);
		}
		return result;
	}

	static void Main() {
		double[] a = {0.0, 0.5, 1.0};
		double[] things = DoStuff(a, OtherFunc);
		double[] things2 = DoStuff(a, SecondFunc);
		// inline method, or Anonymous function. Think lambda's.
		double[] things3 = DoStuff(a, (double x) => x * 2.0);
	}
}
```

Attributes
==========

* `Attribute` classes allow additional declarative information to be tied to a
  class and access at runtime.
* `Attribute` is the base class.
* Mandatory information is in the constructor.
* Additional information added by properties.
* If Attribute is the suffix of your class, this can be ignored when using it.
* Attribute classes are called within square brackets above the class/function
  definitions.
* NOTE: don't use them like a python decorator. It's apparently bad form to do
  functional work via an attribute. eg. [SO: Timer via Attribute].
* `Attribute` class creation:

```c#
using System;

public class HelpAttribute: Attribute {
	string url;
	string topic;

	public HelpAttribute(string url) {
		this.url = url;
	}

	public string Url => url;

	public string Topic {
		get { return topic; }
		set { topic = value; }
	}
}
```

* `Attribute class usage:

```c#
[Help("https://url")]
public class MyClass {
	[Help("https:://func/url"), Topic = "Func")]
	public void Func(string text) {...}
}
```

Anonymous Functions
===================

Anonymous Methods
-----------------

* See: [Microsoft: Anonymous Methods].
* Superseded by Lambda Expressions in all cases except when you want a no
  parameters anonymous function passed into a `delegate`. eg.

```c#
void StartThread()
{
    System.Threading.Thread t1 = new System.Threading.Thread
      (delegate()
            {
                System.Console.Write("Hello, ");
                System.Console.WriteLine("World!");
            });
    t1.Start();
}
```

Lambda Expressions
------------------

* See: [Microsoft: Lambda Operator `=>`].
* Uses the format:
  `([Type] parameter [, [Type] parameter2, ...]) => Loop-Expression;`. eg.

```c#
string[] words = { "thing", "tree", "people" };
int minWord = words.Min(word => word.Length);
```

* Multiple parameters:

```c#
string[] digits = {
	"zero", "one", "two", "three", "four", "five", "six", "seven", "eight",
	"nine" };

Console.WriteLine("Example that uses a lambda expression:");
var shortDigits = digits.Where((digit, index) => digit.Length < index);
foreach (var sD in shortDigits)
{
	Console.WriteLine(sD);
}
```

Expression Body Definition
--------------------------

* See: [Microsoft: Lambda Operator `=>`].
* Uses the format: `member => expression;`
* This is a short hand for a function. eg. Both are equivalent:

```c#
public override string ToString() => $"{fname} {lname}".Trim();

public override string ToString()
{
	return $"{fname} {lname}".Trim();
}
```

Asynchronous Programming
========================

* See: [Microsoft: Async Concepts].
* Seems to be a **lot** simpler than the Twisted Python I used to write.
* Method signature's. NOTE: convention for an `Async` suffix:
	* `async Task<Type> MyMethodAsync()`.
	* `async Task NoReturnStatementOrReturnStatementWithNoOperandAsync()`.
	* `async void MyEventHandlerAsync()`.
	* C# 7.0 `async <Type> MyNonTaskMethodAsync()`. Type used requires a
      `GetAwaiter` method!
* Code inside looks synchronous as normal, unless anything downstream is async
  as well (getter `Task<Type>` or `await` calls).
* Method uses `return` as normal (**not `yield`!!**).
* The caller of the method: `<Type> RetVal = await MyMethodAsync()`.
* You cannot `await` or catch exceptions from an `async void MethodAsync()
  method.

Maintains the same I.O.U. basics as other languages, but instead of Deferred's
& Callbacks, there is `async` to create your callback method and `await` which
sits on the deferred I.O.U.'s (Generally a `Task<Type>` instead of a
defer). Awaited tasks (un-fired defers) are in _"suspension"_, which allows the
compiler to return control back to the caller of the async method.

* Async (_work_ to do) + `Task.Run` (coordinating _work_ onto the threadpool)
  is better & simpler (no race condition guarding) than `BackgroundWorker`
  class for I/O-bound operations
    * **INVESTIGATE:** `BackgroundWorker` if I see it anywhere. Assuming a
      legacy way of writing code !?
* In C# 7.1 you can do: `static async Task Main(string[] args)`. Previously to
  `await` on tasks in `Main()` you had to do the following as per
  [SO: Async Main]:
* C# 7.0 Added `ValueTask<TResult>`, where any standard Type can be
  `TResult`. Requires `System.Threading.Tasks.Extensions` NuGet package.

```c#
static void Main(string[] args)
{
    MainAsync(args).GetAwaiter().GetResult();
}

static async Task MainAsync(string[] args) {... // await call(s) }
```
* Use `<Type> blah = await Task.WhenAll(arrayOfTasks);` to run tasks in
  parallel and continue when all have completed. See: [LINQ](#linq) for a Task
  query.

LINQ
====

* See: [MSDN: LINQ].
* Formalised Querying of relational databases and XML. Appears to be very
  _"Database"_ like, rather than pythons universal _"read/write"_ or CRUD
  (Create Read Update Delete) mentality that allows easy swapping of the
  underlying source (database, file, socket, mock, dictionary, etc).
* You can use queries to create a list of async tasks eg.

```c#
// Create the query.
IEnumerable<Task<Type>> myTasksQuery = from thing in things select SomeMethodAsync(thing);
// Fire off & wait on the query.
Task<Type>[] MyTasks = MyTasksQuery.ToArray();
<Type> result = await Task.WhenAll(MyTasks);
```

.NET Libraries
==============

* See: [Microsoft: Libraries], [Microsoft: Packages].
* **Also: [Microsoft: .NET API Browser] for API reference.**
* Also: [Github: mono] for cross-platform & open source implementation of the
  .NET framework.
* _".NET Standard"_ is the base package of BCL's (Base Class Libraries) that
  all other packages build upon. ~40 libraries.
* Target _".NET Standard 4.0"_ for the widest user base. Done in your `.csproj`
  file with a `<TargetFramework>` node in the `<PropertyGroup>`.
* Multitarget if you need different library versions for; OS / Compile Time
  (`#if` directives) for conditional compilation / Different API
  calls. Requires `<TargetFrameworks>` and `<ItemGroup Condition>` nodes.
* _".NET Core"_ builds off _".NET Standard"_ with ~20 more libraries for CLI
  applications.
* [Nuget] is the .NET package repository (think mvnrepository).
* Multiple Projects: Used to separate code from test or, core logic from public
  API's that can be consumed by a .NET language. eg.

```
MyLibrary
 |- MyLibrary.Core     # Business logic.
 |- MyLibrary.CSharp   # Public APIs for C# consumption.
 |- MyLibrary.FSharp   # Public APIs for F# consumption.
```

* Above example is built via:

```bash
mkdir MyLibrary && cd MyLibrary
dotnet new sln
mkdir MyLibrary.Core && cd MyLibrary.Core && dotnet new classlib
cd ..
mkdir MyLibrary.CSharp && cd MyLibrary.CSharp && dotnet new classlib && dotnet add reference ../MyLibrary.Core/MyLibrary.Core.csproj
cd ..
mkdir MyLibrary.FSharp && cd MyLibrary.FSharp && dotnet new classlib -lang F# && dotnet add reference ../MyLibrary.Core/MyLibrary.Core.csproj
cd ..
dotnet sln add MyLibrary.Core/MyLibrary.Core.csproj
dotnet sln add MyLibrary.CSharp/MyLibrary.CSharp.csproj
dotnet sln add MyLibrary.FSharp/MyLibrary.FSharp.fsproj
```

* The solution allows top of project `dotnet restore`/`dotnet build`, but
  `dotnet test` still needs to be done within the test folder(s).

The following is a list of the key NuGet packages for .NET Core:

* `System.Runtime` - The most fundamental .NET Core package, including
  `Object`, `String`, `Array`, `Action`, and `IList<T>`.
* `System.Collections` - A set of (primarily) generic collections, including
  `List<T>` and `Dictionary<TKey,TValue>`.
* `System.Net.Http` - A set of types for HTTP network communication, including
  `HttpClient` and `HttpResponseMessage`.
* `System.IO.FileSystem` - A set of types for reading and writing to local or
  networked disk-based storage, including `File` and `Directory`.
* `System.Linq` - A set of types for querying objects, including `Enumerable`
  and `ILookup<TKey,TElement>`.
* `System.Reflection` - A set of types for loading, inspecting, and activating
  types, including `Assembly`, `TypeInfo` and `MethodInfo`.

The above plus more packages make up the MetaPackage that represents the .NET
Standard Library Framework.

* You can reference packages individually or the MetaPackage (if you need
  several packages).

JSON
----

There's a handful of JSON libraries for C#. See: [json.org] for the list for
all languages.

* [Microsoft: JSON tutorial] - based on using:
  [Microsoft: System.Runtime.Serialization.Json]. _Opinions online point to
  this being slower than other libraries._
* [Github: Newtonsoft.Json (Json.NET)]. _Opinions point to this being the
  preferred choice of library.__
* [json2csharp] - The C# equivalent of the Java: [jsonschema2pojo], for quickly
  creating a schema class from JSON.

Logging
-------

There seems to be multiple logging libraries:

* [Microsoft: ASP.NET logging] - Looks to be easy ASP.NET way to do
  logging. INVESTIGATE if you can use this in .NET core.
* [MSDN: Microsoft.Extensions.Logging] - Still being updated on Nuget. Looks
  like the above.
* [Microsoft: System.Diagnostics.Trace] - builtin logging, but looks to be
  messier (Partway between `Console.*` and test assertions, with additional
  method calls to change indentation).
* [log4net] - Seems to be well regarded. Apparently it is a port/fork of Java's
  log4j, so need an external XML config file (instead of in-code config).
* [NLog] - The other well regarded logging framework. Supports both XML or
  programmatic config. See: [Github: NLog Tutorial].
* [Serilog] - Looks the most pythonic when compared to [log4net] &
  [NLog]. Still active according to [Github: serilog]. Programmatic config.
* [TheObjectGuy: .NET Logging Framework] - **PAID** - Looks like a dead project
  from 2009. No updates to copyright or twitter reviews past 2010. XML config.

Deployments
===========

* See:
	* [Microsoft: .NET Core application deployment].
    * [Microsoft: CLI application deployment].
	* [Microsoft: runtime patch selection].
* **Save `.pdb` (program database) files so you can debug release builds!!**
* External dependencies must be in `.csproj` within a `<PackageReference>` node
  uder `<ItemGroup>` and then pulled down locally from [Nuget] via `dotnet
  restore`.
* `--no-restore` on a `build`/`run`/`publish` call will explicitly stop the
  latest package versions from being pulled down to your local cache.
* `RuntimeFrameworkVersion` in the `.csproj` programmatically  sets the
  required package versions.

FDD (Framework-dependent deployment)
------------------------------------

* FDD (Framework-dependent deployment): Application built with 3rd party /
  external to .NET Core libraries. Assumption is that .NET Core is installed on
  Target. Contains `.dll` files (`dotnet app.dll`).
* .NET Core apps default to FDD.
* Pro: small size and absence of creating a Platform-specific binary.
* Con is Library mismatch issues & library pre-requisite.
* Publish eg: `dotnet publish -f netcoreapp1.1 -c Release`, generates artefacts
  under `bin/publish/`.
* ASP.NET Core Apps:
	* Typically FDD.
	* Typically assume prior Libraries deployed from a manifest file(s) to
      create a [Microsoft: runtime store]. ie. Host with libraries
      pre-deployed, which may run multiple FDD apps.
    * Version mismatches will highlight the target manifest that called for the
      runtime package store assembly.

SCD (Self-contained deployment)
-------------------------------

* SCD (Self-contained deployment): Built executable (`app.exe` for platform
  specific .NET Core host) and `.dll` (`app.dll` is the actual application)
  with all required libraries (including .NET Core) and .NET Core runtime
  included.
* Pro: allows library/platform control, host executable digital signature.
* Cons: larger packages & installs (silo-ed .NET Core libraries), specifying
  platforms for deployment.
* Publish eg: repeat for each platform: `dotnet publish -c Release -r
  <runtime_identifier>`.
* Requires `<RuntimeIdentifiers` node under `<ProjectGroup>` in your `.csproj`
  file.
* `TargetLatestRuntimePatch` propert set to `true` in `.csproj` prevents
  implicit restores from grabbing latest packages during `dotnet publish`.

Docker
------

* See: [Microsoft: Docker], for loads of examples for various frameworks.
* Microsoft maintains a space on [DockerHub: Microsoft].
* [DockerHub: microsoft/aspnetcore] is for ASP.NET Core (Web) apps.
* [DockerHub: microsoft/dotnet] is for .NET Core (Console) apps. eg. Batch
  processes, Azure WebJobs, etc..
* Visual Studio support for development; Visual Studio Tools for Docker.
* BYOC: Bring your own Container.
* Change environment by injecting config/environment variables into the image.
* Dev/Test/Build/Deploy using containers for consistency & stable environment
  at each stage of the pipeline.
* Example Docker files:
	* [Docker Docs: Dockerise .NET].
	* [Github: .NET Docker Unittesting] from [Github: .NET Docker].
	* [MSDN: .NET Docker].

Testing
=======

xUnit
-----

Annotations:

* `[Fact]` - standard testcase.
* `[Theory]` - allows multi-testcase generation by feeding in arguments into
  the methods parameter list. Done via: `[InlineData]`, `[PropertyData]`,
  `[ClassData]`, `[MemberData]`.
* `[Trait]` - allows selective filtering of tests by category/priority. Usage:
  `dotnet test --filter <blah>`. See: [Microsoft: selective unittests].
* Test published artefact: `dotnet vstest <PublishedTests>.dll`. Requires
  `--Framework:"<framework>,Version=vN.N"` if a non `netcoreapp`.

Filtering:

* See: [Github: vstest/dotnet filtering].
* `dotnet test --filter DisplayName~MyTestClass` - Filter tests to class
  name. `~` does a _contains_ lookup.

Code Snippets
=============

* String to/from bytes conversion:

```c#
byte bytes = Encoding.UTF8.GetBytes(someString);
string someString = Encoding.UTF8.GetString(bytes);
```


TODO
====

Coding Concepts
---------------

* async/await.
* LINQ
* Generics
* asp.NET
* Core web api

Test Frameworks
---------------

* Unittesting:
	* [xUnit]
	* nUnit
	* MStest
	* _[Robolectric](http://robolectric.org) - Android JVM for your desktop to
      run Android unittests locally. Needs explicit Annotation. Also is Java
      only??_
	* https://en.wikipedia.org/wiki/List_of_unit_testing_frameworks#.NET_programming_languages
* Mocking:
	* [Github: Moq] - **Active** - looks like a good framework from the
      examples. See: [NuDoq: Moq] for API docs. Seems to be popular along with
      [NSubstitute] in my searches so far. _- Trying to actively play with it._
	* [NSubstitute] - **Active** - Looks good and is in active development
      according to [Github: NSubstitute]. _- INVESTIGATE !!_
	* [Github: FakeItEasy] - **Active** - Also looks pretty simple to use. Docs
      are very good from a quick glance at: [FakeItEasy]. _- INVESTIGATE !!_
	* [Telerik: JustMock] - **Paid (tiered) ** - solution. Not dug into it at
      all.
	* [TypeMock] - **Paid (tiered)** - solution. Not dug into it at all.
	* [SourceForge: EasyMock.NET] - **Dead** - Looks to be a dead project
      (Readonly CVS code base, by SourceForge's CVS deprecation policy. No
      activity in 3 years) with no docs. Was a fork of the Java EasyMock
      framework.
	* [NMock3] - **Dead** - Another dead project ([Nuget: NMock3] show's last
	  build in 2013. All weblinks circle back to main page, except the zip
	  download). Seems to a progression from [SourceForge: NMock] v1 & v2.
	* [RhinoMocks] - **Dead** - Looks okay from a quick glance, but also looks
      dead ([Github: rhino-mocks] shows no activity in 8 years).
* Mocking Additionals:
	* [Kzu: Record/Replay/Verify mocking model] - Seems like an odd concept
      (compared to python mock frameworks) that complicates test
      code. INVESTIGATE: if I'm not using Moq.
	* [Github: Testing.HttpClient] - **Active** - Angular style HttpClient-only
      mock. _- Not tried yet. See: [HttpClient and unit testing] for an
      overview._
	* 
* WebUI:
    * [Selenium] - Is a pretty good WebUI framework. Used it in the past
      pre-WebDriver and Grid days, on their Python bindings. Also had fun
      transitioning our tests over to WebDriver. My main feeling is that if the
      WebUI is one of many UI's that you have (eg. CLI, GUI), then try to make
      your wrapper code for each UI look the same. Makes it easier to point the
      same test at all of your UI's. Selenium is so good at testing WebUI's,
      that you need to be diligent in making sure your UI has minimal business
      logic in it (i.e. the API's provided from your business logic in the
      backend is feature full enough to support a thin UI layer). Also
      impressed at how much more support there is now that I'm looking back:
	    * [Selenium: C# API].
		* [Selendroid] - android support. [Github: selendroid] - Java-based
          with a Java Client. Python example show's a simple test from scratch,
          that would be similar for C#. Works against pre-existing emulators or
          hardware.
		* [ios-driver].
		* Additional: Blackberry, Windows Phone, QT, Winium (Windows
          Desktop/Store apps (Previously we had to use [Sikuli] for this)),
          [Appium].
* Devices:
    * Android:
	    * [Android Dev: UIAutomator] Has testing guides.
	    * [Android Dev: UI Automator] for testing against the Android Web
          driver for System or UI testing. Used by [Appium] or [Selendroid].
		* [Android Dev: Espresso] is another UI test tool.
    * iOS:
	    * [Apple Dev: XCTest] covers unit/performance/UI tests for Xcode
          projects. Used by [Appium].
	* Windows:
	    * [Github: WinAppDriver] covers UI testing against Windows 10
          (UWP/Win32 apps). Used by [Appium].
	* [Appium] - Andorid/iOS/Windows(Desktop/Mac(Desktop) cross-platform
      mobile-automation Library. Looks like it wraps up platform-specific test
      frameworks (UIAutomator, UIAutomator2, Instrumentation / XCUITest,
      UIAutomation / WinAppDriver - respectively) with [Selenium] so that tests
      are written to a common HTTP REST Client and then translated to the
      framework via the JSON Wire Protocol. [Github: Appium]. Appium extends
      regular [Selenium: C# API] Client Driver, so should use their C# driver
      instead: [Github: appium-dotnet-driver].
	* [Sikuli]: Automation via image recognition. Like Selenium, but can be
      used on desktop apps. Same issues of Selenium with UI changes (Typically
      non-backend and sometimes non-frontend code changes) can cause test
      failures until the tests are updated (should be definition of done, to
      update as part of the feature change).
* Load Testing:
	* [Microsoft: Visual Studio based Load testing] - Has; _Scenarios, Counter
      Sets, Run Settings_, for Web/unit testing multiple simulated Users. All
      done within Visual Studio Enterprise (ie. How do you use this in CI or
      cross-platform ??). **INVESTIGATE:** further if I'm going to be using VS
      Enterprise edition in the future.
	* [tc] - Traffic Control is great if you can control the network that
      contains your SUT (System Under Test). Linux tool that does network
      traffic shaping (ie. dropped/delayed packets, bandwidth). There are also
      similar tools on managed switches (eg. Cisco).

Pipelines
---------

* Code Coverage:
	* Seems to be a mess with no cross-platform programs due to Microsoft not
      completing the effort to provide the necessary API's. All non-Windows
      tools are either free/paid reverse engineered projects.
	* [MiniCover](https://github.com/lucaslorentz/minicover) Linux based
      coverage. Outputs Clover/JSON/NCover/CoverAll formats. Requires some faff
      to instal/build.
	* [vstest](https://github.com/Microsoft/vstest-docs/blob/master/docs/analyze.md#coverage) -
      Windows Visual Studio based coverage. The coverage data **can** be built
      from CLI, but is still consumed by Visual Studio.
	* [Coverlet](https://github.com/tonerdo/coverlet) Assume cross-platform
      (works on Linux). Outputs JSON/LCOV/opencover/cobertura. Very easy
      install, simple `nuget test` CLI switches (eg. `nuget test
      /p:CollectCoverage=true /p:CoverletOutputFormat=lcov
      /p:Threshold=90`). Also actively developed. _- Prefer it to MiniCover!!_
	* [AltCover](https://github.com/SteveGilham/altcover) is
      cross-platform. Called via `dotnet fake run ./Build/build.fsx`, so new
      package setup and additional build scripts in your project.
	* [OpenCover](https://github.com/OpenCover/opencover) Not cross-platform
      yet [issue #703](https://github.com/OpenCover/opencover/issues/703). Like
      Coverlet, OpenCover seems to extend the `dotnet test` calls with
      additional flags. May be one to watch when it goes
      cross-platform. Previously called (or forked from) PartCover.
	* [ReportGenerator](https://github.com/danielpalme/ReportGenerator)
      Generates human readable reports from most XML based coverage outputs
      (OpenCover, PartCover, dotCover, NCover, Visual Studio, Covertura, Mono's
      mprof-report).
	* Non-investigated coverage tools:
		* dotCover.
		* NCover (Paid solution).
		* Covertura.
		* Visual Studio (Currently running a Linux only laptop).
		* NDepend (Paid solution).
		* NCrunch (Paid solution).
* Static Analysis:
	* [Coverity] - Now has C# support as of July 2017. Used this in my old
      place for our C/C++ code. We didn't use it for our Python code
      though. Cross-platform Java client for running static analysis as part of
      your build pipeline. Can take hours on a large codebase (8GB ~10hr+ from
      clean compile). Think [Github: findbugs] or [Github: pmd] in Java, where
      it finds the common issues in your code that could lead to a bug (leaks,
      dereferenced NULL pointers, error handing/concurrency/flow control
      issues, etc...). Free for opensource, or paid at [Synopsys].
	* <style checking> ??
	* [IWYU](https://include-what-you-use.org) ??
	* https://stackoverflow.com/questions/38635/what-static-analysis-tools-are-available-for-c#100350
	* [Github: StyleCop] - Enforces Style on a codebase. Looks to be similar to
      Python's [pep8] or Java's [Github: CheckStyle].
* Documentation:
	* Sphinx ?
	* Doxygen
	* ?
* Build Tools:
	* `dotnet`
	* `msbuild`
		* https://docs.microsoft.com/en-us/visualstudio/msbuild/msbuild-concepts
		* https://docs.microsoft.com/en-us/dotnet/core/tools/cli-msbuild-architecture
	* `csc`
* CI:
	* [Travis: C#].
* IDE/Editors:
	* [Emacs] - Cross-platform cross-language CLI/GUI editor (comparable to
      VIM). Great if you buddy code remote via an SSH terminal + [Github: Tmux]
      for a low-bandwidth an bi-directional buddy coding session. Can also
      compile/debug code.
	    * [Github: omnisharp-emacs] provides IDE features via [Github:
          omnisharp-roslyn] server.
	    * [Github: company-mode] is a completion engine that supports [Github:
          omnisharp-emacs].
		* [Github: flycheck] is an on-the-fly syntax checker.
	* [MonoDevelop] cross-platform C#/F#/VB .NET/Vala IDE.
    * [Microsoft: Xamarin] - Windows/Mac C# IDE (fork or plugin to [Microsoft:
      Visual Studio] ??) that supports build targets of Android/iOS/Windows via
      wrapping native APIs.
	    * [Github: xamarin-macios] for all apple product native APIs.
		* [Github: xamarin-android].
		* [Github: Xamarin.Forms] for iOS/Android/Windows native UIs in a
          single/shared code base.
	* [Microsoft: Visual Studio] - Windows/Mac
      C++/Node.js/Python/R/.NET/JavaScript IDE.


[Microsoft: C# Tour]: https://docs.microsoft.com/en-us/dotnet/csharp/tour-of-csharp/index
[SO: readonly benefits]: https://stackoverflow.com/questions/277010/what-are-the-benefits-to-marking-a-field-as-readonly-in-c#277117
[MSDN: Generic Lists]: https://msdn.microsoft.com/en-us/library/6sh2ey19.aspx "MSDN: Generic Lists (dynamic lists)"
[SO: dynamic array]: https://stackoverflow.com/questions/594853/dynamic-array-in-c-sharp
[SO: Timer via Attribute]: https://stackoverflow.com/questions/4479329/c-sharp-time-a-function-using-attribute#4479372
[Microsoft: Anonymous Methods]: https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/statements-expressions-operators/anonymous-methods
[Microsoft: Lambda Operator `=>`]: https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/operators/lambda-operator
[Microsoft: Async Concepts]: https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/async/
[SO: Async Main]: https://stackoverflow.com/questions/9208921/cant-specify-the-async-modifier-on-the-main-method-of-a-console-app#24601591

[MSDN: LINQ]: https://msdn.microsoft.com/en-us/library/bb308959.aspx "MSDN: LINQ (.NET Language-Integrate Query)"
[Microsoft: Libraries]: https://docs.microsoft.com/en-us/dotnet/core/tutorials/libraries
[Microsoft: Packages]: https://docs.microsoft.com/en-us/dotnet/core/packages
[Microsoft: .NET API Browser]: https://docs.microsoft.com/en-gb/dotnet/api/
[Github: mono]: https://github.com/mono/mono
[Nuget]: https://www.nuget.org "Nuget: The .NET package repository"
[json.org]: http://json.org
[Microsoft: JSON tutorial]: https://docs.microsoft.com/en-us/dotnet/framework/wcf/feature-details/support-for-json-and-other-data-transfer-formats
[Microsoft: System.Runtime.Serialization.Json]: https://docs.microsoft.com/en-us/dotnet/api/system.runtime.serialization.json?view=netframework-4.7.2
[Github: Newtonsoft.Json (Json.NET)]: https://github.com/JamesNK/Newtonsoft.Json
[json2csharp]: http://json2csharp.com/
[jsonschema2pojo]: http://www.jsonschema2pojo.org

[Microsoft: ASP.NET logging]: https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1
[MSDN: Microsoft.Extensions.Logging]: https://msdn.microsoft.com/en-us/magazine/mt694089.aspx
[Microsoft: System.Diagnostics.Trace]: https://docs.microsoft.com/en-gb/dotnet/api/system.diagnostics.trace?view=netframework-4.7.2
[log4net]: http://logging.apache.org/log4net/
[NLog]: http://nlog-project.org
[Github: NLog Tutorial]: https://github.com/nlog/nlog/wiki/Tutorial
[Serilog]: https://serilog.net
[Github: serilog]: https://github.com/serilog/serilog
[TheObjectGuy: .NET Logging Framework]: http://dotnetlog.theobjectguy.com

[Microsoft: .NET Core application deployment]: https://docs.microsoft.com/en-us/dotnet/core/deploying/index
[Microsoft: CLI application deployment]: https://docs.microsoft.com/en-us/dotnet/core/deploying/deploy-with-cli
[Microsoft: runtime patch selection]: https://docs.microsoft.com/en-us/dotnet/core/deploying/runtime-patch-selection
[Microsoft: runtime store]: https://docs.microsoft.com/en-us/dotnet/core/deploying/runtime-store
[Microsoft: Docker]: https://docs.microsoft.com/en-us/dotnet/core/docker/intro-net-docker
[DockerHub: Microsoft]: https://hub.docker.com/u/microsoft/
[DockerHub: microsoft/aspnetcore]: https://hub.docker.com/r/microsoft/aspnetcore/
[DockerHub: microsoft/dotnet]: https://hub.docker.com/r/microsoft/dotnet/
[Docker Docs: Dockerise .NET]: https://docs.docker.com/engine/examples/dotnetcore/
[Github: .NET Docker]: https://github.com/dotnet/dotnet-docker/tree/master/samples
[Github: .NET Docker Unittesting]: https://github.com/dotnet/dotnet-docker/blob/master/samples/dotnetapp/dotnet-docker-unit-testing.md
[MSDN: .NET Docker]: https://blogs.msdn.microsoft.com/dotnet/2018/06/13/using-net-and-docker-together-dockercon-2018-update/

[Github: vstest/dotnet filtering]: https://github.com/Microsoft/vstest-docs/blob/master/docs/filter.md
[Microsoft: selective unittests]: https://docs.microsoft.com/en-us/dotnet/core/testing/selective-unit-tests
[xUnit]: https://xunit.github.io

[Github: Moq]: https://github.com/moq/moq4
[NuDoq: Moq]: http://www.nudoq.org/#!/Projects/Moq
[Github: FakeItEasy]: https://github.com/FakeItEasy/FakeItEasy
[FakeItEasy]: https://fakeiteasy.github.io
[Telerik: JustMock]: https://www.telerik.com/products/mocking.aspx
[SourceForge: EasyMock.NET]: https://sourceforge.net/projects/easymocknet/
[NMock3]: https://archive.codeplex.com/?p=nmock3
[SourceForge: NMock]: http://nmock.sourceforge.net
[Nuget: NMock3]: https://www.nuget.org/packages/NMock3
[TypeMock]: https://www.typemock.com
[RhinoMocks]: http://hibernatingrhinos.com/oss/rhino-mocks
[Github: rhino-mocks]: https://github.com/hibernating-rhinos/rhino-mocks
[NSubstitute]: http://nsubstitute.github.io
[Github: NSubstitute]: https://github.com/nsubstitute/NSubstitute

[Kzu: Record/Replay/Verify mocking model]: http://blogs.clariusconsulting.net/kzu/whats-wrong-with-the-recordreplyverify-model-for-mocking-frameworks/
[Github: Testing.HttpClient]: https://github.com/dfederm/Testing.HttpClient
[HttpClient and unit testing]: https://dfederm.com/httpclient-and-unit-testing/

[Selenium]: https://docs.seleniumhq.org
[Selenium: C# API]: http://seleniumhq.github.io/selenium/docs/api/dotnet/index.html
[Selendroid]: http://selendroid.io
[Github: selendroid]: https://github.com/selendroid/selendroid
[ios-driver]: http://ios-driver.github.io/ios-driver/

[Android Dev: Testing]: https://developer.android.com/training/testing/
[Android Dev: UIAutomator]: https://developer.android.com/training/testing/ui-automator
[Android Dev: Espresso]: https://developer.android.com/training/testing/espresso/
[Apple Dev: XCTest]: https://developer.apple.com/documentation/xctest
[Github: WinAppDriver]: https://github.com/microsoft/winappdriver
[Appium]: http://appium.io
[Github: Appium]: https://github.com/appium/appium
[Github: appium-dotnet-driver]: https://github.com/appium/appium-dotnet-driver

[Microsoft: Visual Studio based Load testing]: https://docs.microsoft.com/en-us/visualstudio/test/quickstart-create-a-load-test-project
[tc]: https://linux.die.net/man/8/tc "Linux Traffic Control"
[Sikuli]: http://www.sikuli.org "Sikuli: Automation via image recognition"

[Coverity]: https://scan.coverity.com
[Github: findbugs]: https://github.com/findbugsproject/findbugs
[Github: pmd]: https://github.com/pmd/pmd
[Synopsys]: https://www.synopsys.com/software-integrity.html
[Github: StyleCop]: https://github.com/StyleCop/StyleCop
[pep8]: https://www.python.org/dev/peps/pep-0008/
[Github: CheckStyle]: https://github.com/checkstyle/checkstyle

[Travis: C#]: https://docs.travis-ci.com/user/languages/csharp/

[Emacs]: https://www.gnu.org/software/emacs/
[Github: omnisharp-emacs]: https://github.com/OmniSharp/omnisharp-emacs
[Github: omnisharp-roslyn]: https://github.com/OmniSharp/omnisharp-roslyn
[Github: flycheck]: https://github.com/flycheck/flycheck
[Github: Tmux]: https://github.com/tmux/tmux "Terminal multiplexer (better than screen)"
[MonoDevelop]: http://www.monodevelop.com
[Microsoft: Xamarin]: https://visualstudio.microsoft.com/xamarin/
[Github: xamarin-macios]: https://github.com/xamarin/xamarin-macios
[Github: xamarin-android]: https://github.com/xamarin/xamarin-android
[Github: Xamarin.Forms]: https://github.com/xamarin/Xamarin.Forms
[Microsoft: Visual Studio]: https://visualstudio.microsoft.com

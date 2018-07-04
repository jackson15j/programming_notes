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

LINQ
====

* See: [MSDN: LINQ].
* Formalised Querying of relational databases and XML. Appears to be very
  _"Database"_ like, rather than pythons universal _"read/write"_ or CRUD
  (Create Read Update Delete) mentality that allows easy swapping of the
  underlying source (database, file, socket, mock, dictionary, etc).

.NET Libraries
==============

* See: [Microsoft: Libraries], [Microsoft: Packages].
* **Also: [Microsoft: .NET API Browser] for API reference.**
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
* 




TODO
====

Coding Concepts
---------------

* async/await.
* LINQ
* Generics
* asp.NET
* Core web api
* [Xamarin] - Builds core C# business logic + UI to Android/iOS/Windows.

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
	* Moq
* WebUI:
    * Selenium (C# bindings)
* Devices:
    * Android ??
    * iOS ??
	* [Appium] - Andorid/iOS/Windows cross-platform mobile-automation
      Library. Looks like it wraps up platform-specific test frameworks
      (UIAutomator, UIAutomator2, Instrumentation / XCUITest, UIAutomation /
      WinAppDriver - respectively) with Selenium so that tests are written to a
      common HTTP REST Client and then translated to the framework via the JSON
      Wire Protocol.
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
      install, simple `nuget test` CLI switches. Also actively developed. _-
      Prefer it to MiniCover!!_
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
	* coverity ?
	* <style checking> ??
	* [IWYU](https://include-what-you-use.org) ??
	* https://stackoverflow.com/questions/38635/what-static-analysis-tools-are-available-for-c#100350
* Documentation:
	* Sphinx ?
	* Doxygen
	* ?
* Build Tools:
	* dotnet
	* msbuild
		* https://docs.microsoft.com/en-us/visualstudio/msbuild/msbuild-concepts
		* https://docs.microsoft.com/en-us/dotnet/core/tools/cli-msbuild-architecture
	* csc
* CI:
	* [Travis: C#].


[Microsoft: C# Tour]: https://docs.microsoft.com/en-us/dotnet/csharp/tour-of-csharp/index
[SO: readonly benefits]: https://stackoverflow.com/questions/277010/what-are-the-benefits-to-marking-a-field-as-readonly-in-c#277117
[MSDN: Generic Lists]: https://msdn.microsoft.com/en-us/library/6sh2ey19.aspx "MSDN: Generic Lists (dynamic lists)"
[SO: dynamic array]: https://stackoverflow.com/questions/594853/dynamic-array-in-c-sharp
[Microsoft: Anonymous Methods]: https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/statements-expressions-operators/anonymous-methods
[Microsoft: Lambda Operator `=>`]: https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/operators/lambda-operator
[Microsoft: Async Concepts]: https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/async/

[MSDN: LINQ]: https://msdn.microsoft.com/en-us/library/bb308959.aspx "MSDN: LINQ (.NET Language-Integrate Query)"
[Microsoft: Libraries]: https://docs.microsoft.com/en-us/dotnet/core/tutorials/libraries
[Microsoft: Packages]: https://docs.microsoft.com/en-us/dotnet/core/packages
[Microsoft: .NET API Browser]: https://docs.microsoft.com/en-gb/dotnet/api/
[Nuget]: https://www.nuget.org "Nuget: The .NET package repository"
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

[Microsoft: selective unittests]: https://docs.microsoft.com/en-us/dotnet/core/testing/selective-unit-tests
[xUnit]: https://xunit.github.io
[Microsoft: Visual Studio based Load testing]: https://docs.microsoft.com/en-us/visualstudio/test/quickstart-create-a-load-test-project
[tc]: https://linux.die.net/man/8/tc "Linux Traffic Control"
[Sikuli]: http://www.sikuli.org "Sikuli: Automation via image recognition"

[Appium]: http://appium.io
[Xamarin]: https://visualstudio.microsoft.com/xamarin/
[Travis: C#]: https://docs.travis-ci.com/user/languages/csharp/

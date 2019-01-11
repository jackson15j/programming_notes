C++ Notes
=========

* [C++ Standards Committee: ISOCPP] tracks the state of current and proposed
  libraries. [Github: cplusplus] is their Github presence.
* Tutorial sites:
    * [LearnCpp].
    * [CppReference].


Libraries
=========

[Boost]
-------

* See:
    * [Github boost] super-project for all of the libraries.
    * [Boost: Getting started guide].
* [Boost] is a _"well regarded"_ collection of cross-platform libraries.
* Some Boost libraries have been migrated into the C++ Standard Libraries.
* See: [Test Frameworks](#test-frameworks) for [Boost.Test] notes.


Build Tools
===========

[Boost.Build]
-------------

* See:
    * [Boost.Build: Tutorial].
* [Boost.Build] is the [Boost] way of making it easy to build projects in a
  compiler independent way.
* In arch, [Boost.Build] is separated out from the `pacman` boost package and
  is instead in AUR (`yaourt`).
* `b2` is the interpreter for your build tool of choice..
    * `Jamroot.jam` config file should be at the base of your project.
    * `Jamfile.jam` config file can be in any sub-module of your project. If
      `b2` is executed in a folder with a `Jamfile.jam`, it will search upwards
      for a `Jamroot.jam` to get project global config.
    * `Jamroot.jam`/`Jamfile.jam` naming convention is for `b2`'s searching
      benefit.
    * `b2` does nothing if no `.jam` file at point of execution.
    * `b2` converts `.jam` configs to flags for your compiler of choice (`gcc`,
      `clang`, ...).
    * All tokens in `.jam` files must have a space around them!


**INVESTIGATE**.
----------------

* [CMake]
* [Clang]
* [GCC]


Test Frameworks
===============

* [Boost.Test]
    * Like most frameworks, It can do manual/auto testcase registration at a
      method/class/suite level.
    * Testcase generation either by it's (slightly weird) dataset class, or via
      an iterator wrapped up in a boost data set call. Note: that the latter
      requires `ostream` definitions for the type and iterator objects. eg.

      ```cpp
      #define BOOST_TEST_DYN_LINK
      #include <boost/test/unit_test.hpp>
      #include <boost/test/data/test_case.hpp>

      namespace bdata = boost::unit_test::data;

      BOOST_AUTO_TEST_SUITE(MyBoostGeneratedTest)

      struct MyTestObj
      {
      std::string stringUnderTest;
      bool expResult;
      };

      /** Define how to print out the MyTestObj struct. */
      std::ostream& operator << (std::ostream& o, const MyTestObj& myTestObj) {
      o << "[MyTestObj]" << " - [stringUnderTest] \"" << myTestObj.stringUnderTest << "\" - [expResult] " << myTestObj.expResult;
      return o;
      }

      /** Vector of MyTestObj's */
      std::vector<MyTestObj> MyVectorDataset {
      {"Thing to Test: pass", true},
      {"This should fail", false},
      };

      /** Define how to print out a std::vector of MyTestObj's structs. This is
      required by Boost's testcase generation on testcase failure.
      */
      std::ostream& operator <<(std::ostream& o, const std::vector<MyTestObj>& v) {
      for(auto const& item: v) {
      o << item << std::endl;
      }
      return o;
      }

      /** Generates testcases from the MyVectorDataset. */
      BOOST_DATA_TEST_CASE(ads_dataset_test, bdata::make(MyVectorDataset), myTestObj)
      {
      BOOST_TEST(ClassUnderTest::MethodUnderTest(myTestObj.stringUnderTest) == myTestObj.expResult);
      }

      BOOST_AUTO_TEST_SUITE_END();
      ```

**INVESTIGATE**

* [Github: Catch]
* [Github: Google Test]

Debugging
=========

* [gprof & gcov profilers].
* GDB.

Random Tips
===========

* Use `const` for all read-only variables (Mentality: _"It's not python!"_
  Start `const` first and remove as required).
* C++-11: Ranged based `for` loops. See: [CppReference: Range For].
* `\n` is an OS-agnostic newline character (Traditionally. Linux=`\n`,
  Windows=`\r\n`). See: [CppReference: Escape Sequences].
* [RAII] - a set of guidelines that get you to a safer place of de-allocating
  resources on failure. Originally thought it was like [Python's `with` Context
  Manager], however, it is a manual process.
    * Encapsulate into a class: Constructor creates resources or throws
      exceptions, Destructor releases resources but never excepts.
    * Use resource via a RAII-class instance: Lifetime bound to lifetime of the
      object.
* `g++` is _apparently_ preferred to: `gcc -lstdc++` to pull in the standard
  C++ libraries. See: [StackOverflow: Undefined Std library reference].

Editor Config
=============

* [tudho: c-ide].
* [Boston Uni: emacs programing].


[C++ Standards Committee: ISOCPP]: http://www.open-std.org/JTC1/SC22/WG21/
[Github: cplusplus]: https://github.com/cplusplus
[LearnCpp]: https://www.learncpp.com
[CppReference]: https://en.cppreference.com/w/

[Boost]: https://www.boost.org
[Github boost]: https://github.com/boostorg/boost
[Boost: Getting started guide]: https://www.boost.org/more/getting_started/index.html
[Boost.Build]: https://boostorg.github.io/build/
[Boost.Build: Tutorial]: https://boostorg.github.io/build/tutorial.html
[Boost.Test]: https://www.boost.org/doc/libs/1_68_0/libs/test/doc/html/index.html

[CMake]: https://cmake.org
[Clang]: https://clang.llvm.org
[GCC]: https://gcc.gnu.org

[Github: Catch]: https://github.com/catchorg/Catch2
[Github: Google Test]: https://github.com/google/googletest

[gprof & gcov profilers]: https://alex.dzyoba.com/blog/gprof-gcov/

[CppReference: Range For]: https://en.cppreference.com/w/cpp/language/range-for
[CppReference: Escape Sequences]: https://en.cppreference.com/w/cpp/language/escape

[RAII]: https://en.cppreference.com/w/cpp/language/raii
[Python's `with` Context Manager]: https://docs.python.org/3/reference/compound_stmts.html#with
[StackOverflow: Undefined Std library reference]: https://stackoverflow.com/questions/28236870/undefined-reference-to-stdcout#28236905

[tudho: c-ide]: https://tuhdo.github.io/c-ide.html
[Boston Uni: emacs programing]: https://www.cs.bu.edu/teaching/tool/emacs/programming/

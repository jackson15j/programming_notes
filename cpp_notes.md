C++ Notes
=========

* [C++ Standards Committee: ISOCPP] tracks the state of current and proposed
  libraries. [Github: cplusplus] is their Github presence.


Libraries
=========

[Boost]
-------

* See:
    * [Github boost] super-project for all of the libraries.
	* [Boost: Getting started guide].
* [Boost] is a _"well regarded"_ collection of cross-platform libraries.
* Some Boost libraries have been migrated into the C++ Standard Libraries.


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

**INVESTIGATE**

* [Github: Catch]
* [Github: Google Test]


[C++ Standards Committee: ISOCPP]: http://www.open-std.org/JTC1/SC22/WG21/
[Github: cplusplus]: https://github.com/cplusplus

[Boost]: https://www.boost.org
[Github boost]: https://github.com/boostorg/boost
[Boost: Getting started guide]: https://www.boost.org/more/getting_started/index.html
[Boost.Build]: https://boostorg.github.io/build/
[Boost.Build: Tutorial]: https://boostorg.github.io/build/tutorial.html

[CMake]: https://cmake.org
[Clang]: https://clang.llvm.org
[GCC]: https://gcc.gnu.org

[Github: Catch]: https://github.com/catchorg/Catch2
[Github: Google Test]: https://github.com/google/googletest

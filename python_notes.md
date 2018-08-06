Python Notes
============

A dump of my python notes.

Future
======

* **INVESTIGATE**
* Look at some of the libraries coming out of the new function annotation stuff
  in python 3. _Apparently_ there is some good use cases for them now.
* For keeping up on trends:
    * [The Hitchhiker’s Guide to Python!].
    * [Python Packaging User Guide].
* [mypy]: Statically typed python.


Web Frameworks
==============

[WSGI] ((Python) Web Server Gateway Interface)
----------------------------------------------

[WSGI] is an interface specification for Web Server/Gateway (may not by Python)
to (Python) Application/Framework communication, which is detailed in [PEP
3333] (a Python3 extension to the Python2 [PEP 333]). This is the equivalent of
Java's _"servlet"_ API.

* WSGI Server: Thin layer that invokes the Application (context passed to it)
  callable on each Client request. It returns the Application response back to
  the Client.
* WSGI Application: provides callables (function/method/class with
  `__call__()`) for the Server to call.
* WSGI Middleware: Implements both sides of the Server/Application.

[Flask]
-------

* [Flask]: A thin framework that exposes a REST API on your application via
  decorators.
* Dependencies:
    * [Jinja2]: ([Django] inspired templating engine).
    * [Werkzeug]: [WSGI] utility library.
* `/static`: contains static files. eg. CSS.
* `/templates`: contains templates for programmatic population. eg. HTML.
* Supports [WSGI] Middlewares.
* XSS (cross-site scripting) is covered by the [Jinja2] templating engine.
* Works with [Twisted].
* [Github: flask].

[Django]
--------

* An _"opinionated"_ framework (like Java's Spring Boot) that also creates base
  project structure (like C#'s `dotnet` CLI's `new` command). Feels like an
  EcoSystem.
*_"Hand holding"_ to create a whole web site.
* Definitions (eg. default database type) can be changed via python file
  configuration + User addition of required binding libraries.
* Been recommended to buy: [Django Design Patterns and Best Practices].

[Tornado]
---------

* Both a Web Framework and an Async Networking library.
* Not [WSGI] based, but has some support (they suggest not to use it).
* Typically 1 thread per process.
* Path definitions are inside the function. Personally prefer [Flask]'s
  decorator paths for the clarity.


Async Libraries
===============

Typically used for external connections (network/databases) to:

* Prevent Application blocking on delays/waits on external connections or long
  running internal computations.
* Push thread management out of the Application and into the hands of the OS
  for CPU time-slicing and multiple core usage of parallelised tasks.

[Twisted]
---------

* Python2/3 compatible async library.
* Supports full async code (`callbacks`) and decorated blocking code
  (`@defer.inlineCallbacks`).
    * Remember debugging `inlineCallbacks` to be an absolute pain with stack
      traces pointing at the reactor, when it finally is torn down. Always
      better to change the code to be fully async.
	* Remember Async & Blocking code looks vastly different. Especially
      disappointing after recently playing with C#'s `async`/`await` that are
      tacked onto blocking code with minimal changes.

[AsyncI/O]
----------

* **INVESTIGATE**

[Tornado]
---------

* Async coroutines, similar to Python3.5 `async def`.
* Coroutines allows code to look more synchronous, compared to passing
  Callbacks around.
* Supports Python3.5's `async`/`await` calls (_"native coroutines"_) as it's
  own backwards compatible decorator/`yield` coroutines via
  `tornado.gem.coroutine`.
* Looks very C#-ish, since my last bit of python async was [Twisted]. I'm
  assuming this was a Python3 stop-gap between [Twisted]'s slow Python3 support
  and [AsyncI/O] getting into the language.


Task Queuing
============

* **INVESTIGATE**



Test Frameworks
===============

[unittest]
----------

* **INVESTIGATE**

[PyTest]
--------

* **INVESTIGATE**

[Lettuce]
---------

* **INVESTIGATE**
* BDD (Business Driven Development) test framework (PM/PO User
  Story/requirements with functions behind the words, which create the tests
  piecemeal)
* Python version of: [Cucumber] ([Github: cucumber]).
* When we tried it years ago, it was great for simple test cases, but a
  nightmare of duplication for matrix tests (if you want to display
  granularity). Also couldn't get PM to commit to give concrete enough
  requirements for such tests, as well as their commitment to write these
  sentences. ie. They cared more on acceptance filtered from engineering, than
  test result minutiae.

[tox]
-----

* **INVESTIGATE**
* _Apparently_ is on the way out for [Pipenv].


Tooling
=======

* **INVESTIGATE**

[Pipenv]
--------

* Replacement of: [virtualenv].
* **INVESTIGATE**

> It's the recommended tool from the python org now for managing environment,
> and it definitely plugs a big hole in requirment tracking, especially for
> real deployments does a lot of stuff with locking down exactly what packages
> are being used in a deployment, without specifying stuff you don't need. Plus
> has super useful `pipenv check` which will tell you if your dependencies have
> any known security bugs.

CLI
===

[plac]
------

* **INVESTIGATE**
* CLI parser.

[click]
-------

* **INVESTIGATE**
* CLI UI generation (Like [Flask] for the CLI).


[The Hitchhiker’s Guide to Python!]: https://docs.python-guide.org
[Python Packaging User Guide]: https://packaging.python.org
[mypy]: http://mypy-lang.org

[WSGI]: http://wsgi.tutorial.codepoint.net
[PEP 3333]: https://www.python.org/dev/peps/pep-3333/ "PEP 3333 -- Python Web Server Gateway Interface v1.0.1"
[PEP 333]: https://www.python.org/dev/peps/pep-0333/ "PEP 333 -- Python Web Server Gateway Interface v1.0"

[Flask]: http://flask.pocoo.org/docs/1.0/
[Github: flask]: https://github.com/pallets/flask/
[Jinja2]: http://jinja.pocoo.org/docs/2.10/
[Werkzeug]: http://werkzeug.pocoo.org/docs/0.14/

[Django]: https://www.djangoproject.com
[Django Design Patterns and Best Practices]: https://www.amazon.co.uk/dp/B00VIBPW0I/ref=dp-kindle-redirect?_encoding=UTF8&btkr=1

[Tornado]: http://www.tornadoweb.org/en/stable/

[Twisted]: https://twistedmatrix.com/trac/wiki

[Celery]: http://www.celeryproject.org

[PyTest]: https://docs.pytest.org/en/latest/

[Lettuce]: http://lettuce.it
[Cucumber]: https://docs.cucumber.io
[Github: cucumber]: https://github.com/cucumber/cucumber

[tox]: https://tox.readthedocs.io/en/latest/

[Pipenv]: https://docs.pipenv.org
[virtualenv]: https://virtualenv.pypa.io/en/stable/


[plac]: http://micheles.github.io/plac/
[click]: http://click.pocoo.org/5/

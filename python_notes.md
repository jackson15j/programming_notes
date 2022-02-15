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

* [Flask]: A thin, synchronous framework that exposes a REST API on your
  application via decorators.
* Dependencies:
    * [Jinja2]: ([Django] inspired templating engine).
    * [Werkzeug]: [WSGI] utility library.
* `/static`: contains static files. eg. CSS.
* `/templates`: contains templates for programmatic population. eg. HTML.
* Supports [WSGI] Middlewares.
* XSS (cross-site scripting) is covered by the [Jinja2] templating engine.
* Works with [Twisted].
* [Github: flask].
* An async package was built (but is no longer maintained or production ready):
  [Flask-aiohttp], [Github: Flask-aiohttp].

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

[Klein]
-------

* Web Framework built on [Twisted] and [Werkzeug].
* Have used this in a [Flask]/[Twisted] based python2.7
  application. **INVESTIGATE** _- Is this still worth using in Python3 when
  compared to something like [Tornado]?_

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
* See: [PEP 492] - Coroutines with async and await syntax.

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
* See: [Github: Tornado Wiki link dump].

Task Queuing
============

* **INVESTIGATE**
* [Celery] - Can use various system provided queue brokers (RabbitMQ (default),
  Redis, other) for queuing tasks and result backends (RPC (RabbitMQ/AMQP),
  Redis, SQLAchemy/Django ORM, Memcached, other) for tracking state.
      * Looks like [Flask] with the `app` definition and decorators.
      * Python configuration file.
      * See: [Flask: Celery Background Tasks].


Test Frameworks
===============

Mocking
-------

* [Alex Marandon: Python Mock Gotchas].
* [Toptal: An introduction to mocking in Python].

[unittest]
----------

* Builtin testing library.
* Classic style of per-testcase/class setup & teardown functions and testcase
  functions.
* Generating multiple testcases against a dataset is a custom job. -
  **INVESTIGATE** _- This was the case back in the day, is this still true?_

[PyTest]
--------

* An alternative to [unittest].
* A bit C#-like ([C# Notes: xUnit]) with the use of _"fixtures"_ to pass in
  datasets for setup/teardown into a testcase/class/module, to generate
  multiple tests on the fly.
* uses stock `assert`, but provides useful failure messages & code snippets
  eg. `assert a == b`.  Seems to be an improvement to [unittest] which needed a
  custom failure message in the `assert.assert*` to provide context.

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

[Locust]
--------

* API/URL scale testing with worker nodes + human readable graphs.

[Schemathesis]
--------------

* Python version of [Dredd] to do API testing.

Tooling
=======

* **INVESTIGATE**
* [Black] - is a PEP8/Flake8-based CLI syntax re-formatting tool. Seems to be a
  modern version of the older PEP8 ones, which could also do (sometimes
  destructive) refactoring.
* [NovemberFive: PyPI repo on AWS S3].
* [Pip: Requirement Specifiers] - add platform info inline in a requirements
  file.
* [Sphinx: function definitions].
* [Python Docs: Profilers] - eg. `cprofile`.

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

* `pipenv check` (check packages for security issues) fails with API key
  warning:
    * [Github: pipenv issue: Your API Key is invalid #4188].
    * Workaround: `export PIPENV_PYUP_API_KEY=""`.

[logging]
---------

* List all loggers: `logging.root.manager.loggerDict`.

[ipython]
---------

* Dump history from current running window: `%history -g -f <filename>`

UI
==

### [pyperclip] ###

* Cross-platform copy/paste to/from System's clipboard.

CLI
---

### [plac] ###

* **INVESTIGATE**
* CLI parser.

### [click] ###

* **INVESTIGATE**
* CLI UI generation (Like [Flask] for the CLI).

Packaging
=========

* [setuptools] - Packaging `eggs`.

Databases
=========

[ORM]
-----

* [ORM] Object Relational Mappers, are libraries that transfer between
  relational databases and (python) objects.
* ie. high-level abstraction; CRUD (Create Read Update Delete), instead of
  direct database calls/queries.
* eg. [Flask]'s ORM is [SQLAlchemy], which uses a database connector
  `MySQL-python`/`psycopg` for MySQL/PostgreSQL.

Additional Notes
================

* Export `PYTHONPATH`, Windows vs Linux:

  ```bash
  # Windows:
  set PYTHONPATH=%PYTHONPATH%;C:\path\to\export
  # Linux:
  export PYTHONPATH=$PYTHONPATH:/path/to/export
  ```

* `subprocess` command but use virtualenv python interpreter.

  ```python
  import sys
  from subprocess import run
  run([sys.executable, "-m", "<command/module>", ...])
  ```

[The Hitchhiker’s Guide to Python!]: https://docs.python-guide.org
[Python Packaging User Guide]: https://packaging.python.org
[mypy]: http://mypy-lang.org

[WSGI]: http://wsgi.tutorial.codepoint.net
[PEP 3333]: https://www.python.org/dev/peps/pep-3333/ "PEP 3333 -- Python Web Server Gateway Interface v1.0.1"
[PEP 333]: https://www.python.org/dev/peps/pep-0333/ "PEP 333 -- Python Web Server Gateway Interface v1.0"

[Flask]: http://flask.pocoo.org/docs/1.0/
[Github: flask]: https://github.com/pallets/flask/
[Flask-aiohttp]: https://flask-aiohttp.readthedocs.io/en/latest/index.html
[Github: Flask-aiohttp]: https://github.com/Hardtack/Flask-aiohttp
[Jinja2]: http://jinja.pocoo.org/docs/2.10/
[Werkzeug]: http://werkzeug.pocoo.org/docs/0.14/

[Django]: https://www.djangoproject.com
[Django Design Patterns and Best Practices]: https://www.amazon.co.uk/dp/B00VIBPW0I/ref=dp-kindle-redirect?_encoding=UTF8&btkr=1
[Klein]: https://klein.readthedocs.io/en/latest/

[Tornado]: http://www.tornadoweb.org/en/stable/
[Github: Tornado Wiki link dump]: https://github.com/tornadoweb/tornado/wiki/Links
[AsyncI/O]: https://docs.python.org/3/library/asyncio.html
[PEP 492]: https://www.python.org/dev/peps/pep-0492/ "PEP 492 -- Coroutines with async and await syntax"
[Twisted]: https://twistedmatrix.com/trac/wiki

[Celery]: http://www.celeryproject.org
[Flask: Celery Background Tasks]: http://flask.pocoo.org/docs/1.0/patterns/celery/

[PyTest]: https://docs.pytest.org/en/latest/
[C# Notes: xUnit]: csharp_notes.md#xunit

[Alex Marandon: Python Mock Gotchas]: http://alexmarandon.com/articles/python_mock_gotchas/
[Toptal: An introduction to mocking in Python]: https://www.toptal.com/python/an-introduction-to-mocking-in-python
[Lettuce]: http://lettuce.it
[Cucumber]: https://docs.cucumber.io
[Github: cucumber]: https://github.com/cucumber/cucumber

[tox]: https://tox.readthedocs.io/en/latest/

[Locust]: https://locust.io
[Schemathesis]: https://schemathesis.readthedocs.io/en/stable/
[Dredd]: https://dredd.org/en/latest/index.html

[Black]: https://pypi.org/project/black/
[NovemberFive: PyPI repo on AWS S3]: https://novemberfive.co/blog/opensource-pypi-package-repository-tutorial/
[Pip: Requirement Specifiers]: https://pip.pypa.io/en/stable/reference/pip_install/#requirement-specifiers
[Sphinx: function definitions]: https://pythonhosted.org/an_example_pypi_project/sphinx.html#function-definitions
[Python Docs: Profilers]: https://docs.python.org/3/library/profile.html

[Pipenv]: https://docs.pipenv.org
[virtualenv]: https://virtualenv.pypa.io/en/stable/
[Github: pipenv issue: Your API Key is invalid #4188]: https://github.com/pypa/pipenv/issues/4188

[logging]: https://docs.python.org/3/library/logging.html

[ipython]: https://ipython.org

[pyperclip]: https://pyperclip.readthedocs.io/en/latest/introduction.html

[plac]: http://micheles.github.io/plac/
[click]: http://click.pocoo.org/5/

[setuptools]: https://setuptools.readthedocs.io/en/latest/setuptools.html

[ORM]: https://www.fullstackpython.com/object-relational-mappers-orms.html
[SQLAlchemy]: https://www.fullstackpython.com/sqlalchemy.html

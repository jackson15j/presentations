Unittest Hello_world
====================

_Or: What happens when you read up on old C++ unittest frameworks..._

---

So...

---

hello_world.py
==============

Let's write hello world:

    !python
    #! /usr/bin/env python3
    print("hello world")

Presenter Notes
===============

* Python 3 example (simplicity).
* Barest form of printing a string to stdout.
* Let's prove it works...

---

hello_world.py
==============

Let's write hello world:

    !python
    #! /usr/bin/env python3
    print("hello world")

Bash output:

    !bash
    $> ./hello_world.py
    hello world
    $>

Presenter Notes
===============

* Nice and simple.
* It just works.
* Now lets unittest it...
* how to test? Lets pipe it to a file and read it in...

---

How to Test?
============

Let's just pipe it into a file:

    !bash
    $> ./hello_world.py | python3 test_hello_world.py
    hello world
    My test assert passed.
    $>

Presenter Notes
===============

* Piped hello_world's stdout into our test program.
* Test program reads stdout and checks for: "hello world".
* Does an assert.
* Says assert passsed.
* Lets look at our test code...

---

test_hello_world.py
===================

Our test code:

    !python
    import sys

    input = sys.stdin.read()
    assert("hello world"t in input)
    print('My test assert passed.')

Presenter Notes
===============

* Read stdin.
* Assert "hello world" is in input.
* Print message if no exception.
* That was easy. Lets look at Pro's/Con's...

---

Pro's
=====

* No code changes to main program.
* Done our test.
* Could've got someone else to write test.

Presenter Notes
===============

* Could throw over wall to qa.

---

Con's
=====

* We used `in` instead of `==`.
* Using `==` would have lead to line ending gotcha's.
* "Black box" test.
* Crap... this is an integration test! _Lets try again..._

Presenter Notes
===============

* Line endings: Linux: \n. Windows: CRLF.
* "Black Box": miss lots of internal testing.
* Not a unittest (main aim).
* Round 2...

---

Round 2

---

hello_world.py
==============

Let's write hello world:

    !python
    #! /usr/bin/env python3
    print("hello world")

Presenter Notes
===============

* Boom! We've got our hello_world code.
* Lets try again.
* As a dev I don't want to change my program for testing.
* What can I do?
* Boom! Monkey Patching...

---

Monkey Patching sys.stdout Test
===============================

YES!!

    !python
    import sys
    import io

    default_stdout = sys.stdout
    stdout_variable = io.StringIO()
    sys.stdout = stdout_variable
    import hello_world
    sys.stdout = default_stdout

    assert("hello world" in stdout_variable.getvalue())
    print('My test assert passed.')


Presenter Notes
===============

* Boom! Look at my Mad Dev skills!
* Balls so big I can space-hopper around the office on them!
* I monkey patched print()!
* No code changes to my program.
* I'm a super dev!
* Test output...

---

Monkey Patching sys.stdout Test
===============================

Test Bash Output:

    !bash
    $> python3 monkey_patched_test_hello_world.py
    hello world
    My test assert passed.
    $>

Presenter Notes
===============

* Pro's...

---

Pro's
=====

* Mad "Space-Hopper Balls" Dev skills!
* Monkey Patching is awesome!
* It's a unittest!

Presenter Notes
===============

* Awesome.
* Are there any Con's?...

---

Con's
=====

* You Monkey Patched `sys.stdout`!!
* We've swallowed all sys.stdout calls until we un-Monkey Patch.
* Cyclic death if you try this in a Python Interpreter.
* Do we need to test Third Party code?
* There's got to be an easier way??

Presenter Notes
===============

* If you're Monkey Patching, are you sure you **really** need to Monkey Patch?
* Monkey Patching is mind bendingly hard at the best of times.
* Is there an easier way?
* Lets Revise the problem...

---

Hello World Criteria
====================

Print `"hello world"` to stdout.

Presenter Notes
===============

* So what's our Test critera?...

---

Test Criteria
=============

Test that `"hello world"` **is** printed to stdout.

Presenter Notes
===============

* Do we need to test the `print()` function?
* Print is manually tested by **every** single python user!
* Assume that `print()` has "good enough" automated testing.
* So lets reword that statement a bit...

---

Test Criteria
=============

Test that `"hello world"` **will** be printed to stdout.

Presenter Notes
===============

* Subtle but important change.
* Trust `print()` will do it's job if fed appropriate arguments.
* Test what we are concerned with.
* Leave testing of items outside our concern to their maintainer/producer.
* Lets take another stab...
* Global variable...
* Round 3...

---

Round 3

---

hello_world.py
==============

Let's write hello world:

    !python
    #! /usr/bin/env python3
    HELLO_WORLD = "hello world"
    print(HELLO_WORLD)

Presenter Notes
===============

* Boom! We've got our hello_world code. Again
* Third time lucky.
* As a dev I need to modify my code to be testable.
* Lets make lest effort change.
* Test Code...

---

unittest_hello_world.py
=======================

Our test code:

    !python
    from testable_hello_world import hello_world
    assert(hello_world.HELLO_WORLD == "hello world")
    print('My test assert passed.')

Test output:

    !bash
    $> python3 unittest_hello_world.py
    hello world
    My test assert passed.
    $>

Presenter Notes
===============

* That wasn't a horrific change to our hello_world program.
* Now test that our global variable == `"hello world"`.
* Pro's...

---

Pro's
=====

* "White Box" testing.
* Only testing changes in our "Realm".
* Mitigates issues with distance from "Black Box" Input/Output testing.
* Allows refactor with test safety net.
* Can be part of CI that runs on Save/Commit/Remote-sharing.
* Trust Third Party code/tests/API.

Presenter Notes
===============

* "White Box" == unittest.
* Testing isolated components.
* Mitigates issues with integration tests.
* Con's...

---

Con's
=====

* Trust Third Party code/tests/API.
* Had to change Program to make it unittest-able.

Presenter Notes
===============

* Potential for unittests passing but solution level issues.
* Need mix of unit/integration tests (better unittest heavy).
* Making code testable after design is costly.
* Thanks...

---

Thank You
=========

* Presentation: https://github.com/jackson15j/presentations
* Code: https://github.com/jackson15j/unittest_helloworld
* Presentation renderer: https://github.com/adamzap/landslide

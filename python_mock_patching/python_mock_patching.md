Monkey Patching is Great!! (How _not_ to use it)
================================================

* Mocking too much.
* Monkey Patching hard to test code.
* _"Code Smells"_
* Baby Steps to componetisation.

.notes: Let's see some code.

---

Mail Queue Example
==================

```python
import ExternalQueue


class MailQueueHandler():
    def get_mail(self):
        # Potential implementation specific DB read Exception.
        ExternalQueue.get_mail_from_queue()

    def mail_runner(self):
        '''Daemon process to read emails off the queue and send them.'''
        while True:
            # Potential Exception will break the loop.
            mail_queue = self.get_mail()
            for mail in mail_queue()
                # Send mail.
```

.notes: Daemon function that should never exit the loop.
.notes: No Exception Handling == Bugs.
.notes: No Exception Handling == exit the loop.

---

Problem: Buggy Code
===================

* `mail_runner()` should never break out of the `while` loop.
* Code needs exception handling.
* De-risk change with unittests.

.notes: Let's add some Exception Handling.

---

Mail Queue Example: Exception Handling
======================================

```python
import ExternalQueue


class MailQueueHandler():
    def get_mail(self):
        try:
            ExternalQueue.get_mail_from_queue()
        except ImplentationSpecificDbError:
            return []


    def mail_runner(self):
        '''Daemon process to read emails off the queue and send them.'''
        while True:
            try:
                mail_queue = self.get_mail()
                for mail in mail_queue()
                    # Send mail.
            except AnyOtherErrors:
                log.exception("<msg>")
```

.notes: Added Exception handling, so bug is fixed
.notes: Need proof == unittests.
.notes: Need to mock out ExternalQueue.get_mail_from_queue().
.notes: Let's look at that.

---

Mail Queue Example: Exception Handling
======================================

```python
import ImplementationSpecificDb

class ExternalQueue():
    # Other methods/variables ...

    @staticmethod
    def get_mail_from_queue()
        # Potential implementation specific DB read Exception.
        return implementation_specific_db.read()
```

.notes: Implementation specific DB code & Exception.
.notes: Outside boundary of the function I want to test.
.notes: Lets mock it, but how?
.notes: No hard boundary between my component and external action (DB access).
.notes: Need to find an entrypoint.
.notes: Monkey Patching.

Problem: How to Unittest?
=========================

---

Problem: Mocking too much
=========================

.notes: Mocked `get_mail()` == Tests have to recreate production logic.
.notes: Mocked `get_mail()` == Mock all external logic.
.notes: Mocked `get_mail()` == masks verification of broken code in production.
.notes: Requires thorough & explicit UT of every code path in `get_mail()`.

Patching external concerns away
===============================

.notes: Patch usage (imported) point, not original implementation.
.notes: Gotach: Patching too late (post-initialisation).

Summary: How to Unittest?
=========================

* Hard to test code (incestuous, poorly defined boundaries) can lead to:
    * Unittests: mocking out too much.
    * Unittests: bringing in too much.
    * Integration/Solution tests instead of unittests.
* Patching is the scalpel cut for a unittest entrypoint.

Problem: _"Code Smell"_
=======================

Solution: Pass in external dependencies to functions
====================================================

.notes: An improvement, but still a code smell.

Solution: Pass in external dependencies to class
================================================

.notes: Much better.
.notes: Testable class (mock external dependencies.
.notes: Start of enforcing code boundaries.

Summary: _"Code Smell"_
=======================

* Patching highlighted lack of defined boundaries between Our class and
  external dependencies.
* Also highlighted difficulty to test Our class in isolation.
* Fixed by defining external dependencies at class initialisation.
* Removed patching from tests, now we can pass in a mock instance.

Improvements: Harden external boundary via an Interfacet
======================================

.notes: Interface via ABC.

Interface Example
=================

Mail Queue Example: Using External Interface
============================================

Mail Queue Test Example: Using External Interface
=================================================

Improvements: Custom Exceptions on Interface
============================================

.notes: Wrap implementation Exceptions with a defined boundary Exception.
.notes: **Bug if implementation Exceptions leak out!**

Summary: Interface
==================

* Interface is a defined contract.
* Implementation details are kept within the implementation class. ie.
    * Downstream does not see implementation Exceptions.
    * Downstream does not need to care about implementation decisions (DB type,
      network connections, library usage, etc.).
* Test against the interface.
* Interface enforces behaviour.

Summary: Wrapping up
====================

* Be careful of mocking too much (and only testing your mock).
* Patching is great to test gnarly code.
* Patching highlights _code smells_.
* If your problem is poorly defined boundaries, try moving to class
  initialisation.
* Interfaces are an easy refactor to define a hard boundary contract.
* Patching is bad, don't do patching.

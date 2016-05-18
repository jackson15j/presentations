Lets just do it
===============

_Or: Justifying fixing Technical Debt with napkin maths..._

---

The Build takes too long...

...SDP Parser and that new-fangled Boost takes forever to build...

---

The SDP Parser was made to be built in isolation and has it's own set of unittests.

---

It *still* builds in isolation today.

---

So why don't we just build it separately and link the artefacts as part of the main Build process?

---

Surely that would speed up the builds of everyone?

---

Ney Sayers
==========

> The Build system is too complicated.

> It'll never be signed off.

> That's just how long it takes to build stuff.

---

Defining the Problem
====================

* A clean Build takes ~1hr (on my IT dev VM).
* Building the SDP Parser in isolation takes ~20mins.

Napkin maths = ~15-20mins savings if SDP Parser was pre-built & linked.

---

Problem 1
=========

To Build the SDP Parser in isolation would require:

* A Jenkins job to do the Build.
* A downstream job to run It's unittests.
* A subsequent downstream job to store Build artefacts.

---

Rational of Benefit
===================

SDP Parser churns ~X times an iteration, therefore most engineers will need to rebuild it after updating their local feature/bug branches.

---

Problem 2
=========

Time to modify Build system to support pre-built binaries.

> TODO: get quotes from people.

---

Ney Sayers
==========

> We can't justify Y days of a Developers time (not spent on Money earning features work).

---

Napkin Maths
============

We have ~100 Developers across UK/GWY/BGL/USA working on Expressway (TODO: check numbers).

---

Napkin Maths
============

If it takes a Developer 2 Iterations to modify the Build system:

    2 * 10 * 8 = 160 hours

---

Napkin Maths
============

How many *clean* Builds must each Developer do to *buy* back this work:

    20 mins * 100 / 60 = ~33 hours
    160 / 33 = ~5 times

---

Napkin Maths
============

That's right!

If each Developer does 5 *clean* Builds post implementation this is paid for.

---

Napkin Maths
============

Now how many *clean* Builds does a Developer do each iteration?

* 1?
* 2?
* 3+?

So maybe 2 iterations is a sensible minimum ballpark guess to pay off the work.

---

Napkin Maths
============

How about CI *clean* Builds?

Currently we do ~30 *clean* Builds a day.

So CI will pay this work off in 5 days!

---

So why have we not done this yet?

---

What other justifications can we think of?

---

Further Justifications
======================

* Path to *Scalability via Modularity*.
* Enables future Componetisation such as breaking out Ivy.
* Enforces Code/API boundaries.
* Decreases reach of incestuous shotgunned "common" code.
* Reduces risk.
* Increases amount of feature (money) work each (~100) Developer can do.
* Reduces CI time to return a *Green*/*Red* Build status.
* Increases Build throughput of our CI system.

---

Now we've glossed over quite a few details, but as a Napkin Maths exercise it should highlight that there is some merit in taking this non-money earning Techincal Debt piece forwards.

---

Thank You
=========

* Presentation: https://github.com/jackson15j/presentations
* Presentation renderer: https://github.com/adamzap/landslide

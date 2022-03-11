 +++

title = "Software Engineering Module 3: Introduction to agile and DevOps"
description = "Introduction to agile and DevOps, a case from the literature, SCRUM"
outputs = ["Reveal"]

+++

# Software Engineering
### **(for Intelligent Distributed Systems)**
# Module 3: DevOps

## *Software dependencies, Build automation*

---

# The build "life cycle"
#### (Not to be confused with the system development life cycle (SDLC))

The process of creating *tested deployable software artifacts*
<br/>
from *source* code

May include, depending on the system specifics:
* *Source code manipulation* and generation
* Source code *quality assurance*
* *Dependency management*
* *Compilation*, linking
* *Binary manipulation*
* *Test execution*
* Test *quality assurance* (e.g., coverage)
* API *documentation*
* *Packaging*
* *Delivery*

---

# Lifecycle styles

* **Custom**: select some phases that the product needs and perform them.
    * *Flexible and configurable*: tailored on each project's needs
    * *Hard to adapt and port*

* **Standard**: run a sequence of pre-defined actions/phases.
    * *Portable and easy to understand*: replicated on every product
    * *Limited configuration options*

---

# Build automation

Automation of the build lifecycle

* In principle, the lifecycle could be executed manually
* In reality *time is precious* and *repetitivy is boring*

$\Rightarrow$ Create software that automates the building of some software!

* All those concerns that hold for sofware creation hold for build systems creation...

---

## Build automation: basics and styles

Different lifecycle types generate different build automation **styles**

**Imperative**: write a script that tells the system what to do to get
from code to artifacts
* *Examples*: make, cmake, Apache Ant
* *Abstraction gap*: verbose, repetitive
* Configuration (*declarative*) and actionable (*imperative*) logics *mixed* together
* Highly *configurable*

**Declarative**: adhere to some convention, customizing some settings
* *Examples*: Apache Maven
* Separation between *what* to do and *how* to do it
  * The build system decides how to do the stuff
* *Configuration limited* by the provided options

---

## Hybrid automators

Create a *declarative infrastructure* upon an *imperative basis*, and
*allow easy access to the underlying machinery*

**Domain-Specific Languages** are helpful in this context: they can "hide" imperativity without ruling it out

Still, many challenges remain open:
* How to reuse the build logic?
    * within a project, and among projects
automatiche 
---

Many modern languages (such as Rust) come with a *build automator* as *part of their distribution*.

## In Python

Python is an *interpreted* language
* An arguably old one, too (1991)
* Initially used mainly for scripting
* No need to compile (as far as the project is pure Python)
* Build systems were a less pressing concerns than with other platforms...

The need for build systems in Python emerged with more complex use cases
* Since there were none, now there are several tools that do build-related jobs:
    * [PyBuilder](https://pybuilder.io/)
    * [Conda](https://docs.conda.io/en/latest/index.html)
    * [Miniconda](https://docs.conda.io/en/latest/miniconda.html)
    * [Anaconda](https://www.anaconda.com/)
    * [pip](https://pypi.org/project/pip/)
    * [PyEnv](https://github.com/pyenv/pyenv)
    * [virtualenv](https://virtualenv.pypa.io/en/latest/#)
* The feature set varies wildly
* They are meant to solve different problems!

---

## A simple financial application

Build an applicatio that performs a simple graphical MACD analysis of a financial product:

![app screenshot](app.png)

* How many Non-Comment Lines of Code (NCLoC)?

---

{{< mentimeter "12dc23978d6774050055d681b9cd72c9/ddac3c5150cf" >}}

---

## A possible solution

{{< github repo="python-finance-plot" path="macd.py" >}}

[about 45 NCLoC](https://github.com/DanySK/python-finance-plot)

---

## The trick: using a few libraries

* **yfinance**
    * Financial data from Yahoo! Finance
* **Pandas**
    * Data in tabular format
* **Pandas-TA**
    * Pandas technical analysis enhancement
* **PyQt5**
    * Graphical interface

---

## Actual dependency tree

```
+--- commons-io:commons-io:+ -> 2.8.0
+--- com.uwetrottmann.thetvdb-java:thetvdb-java:+ -> 2.4.0
|    +--- com.squareup.retrofit2:retrofit:2.6.2
|    |    \--- com.squareup.okhttp3:okhttp:3.12.0
|    |         \--- com.squareup.okio:okio:1.15.0
|    \--- com.squareup.retrofit2:converter-gson:2.6.2
|         +--- com.squareup.retrofit2:retrofit:2.6.2 (*)
|         \--- com.google.code.gson:gson:2.8.5
\--- org.jooq:jool-java-8:+ -> 0.9.14
```

* three *direct* dependencies
* six *transitive* dependencies

In large projects, *transitive* dependencies often dominate

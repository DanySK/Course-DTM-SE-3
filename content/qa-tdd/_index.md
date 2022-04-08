 +++

title = "Software Engineering Module 3: Introduction to agile and DevOps"
description = "Distributed version control systems, basics of Git"
outputs = ["Reveal"]

+++

# Software Engineering
### **(for Intelligent Distributed Systems)**
# Module 3: DevOps

## *Quality assurance, reproducibility, test-driven development*

---

## Quality assurance

As any engineered product, software should be *subject to quality assurance control*.

* Would you drive a car that has not been succefully passed its *quality control*?
* What does it mean that a car **passed** its *quality control checks*?
  * What does it mean for a bridge?
  * What does it mean for a chair?
  * What does it mean for a robot?
  * What does it mean for a medical software?

---

## Testing: critieria

Verify that the software meets quality criteria.

* **Functional** criteria:
  * Does *what* we expect it to do?
    * Does the software produce the expected results?
* **Non-functional** criteria:
  * Does it do it *how* we want it?
    * Is it secure?
    * Are performance acceptable?

---

## Automated vs. manual

Running an application manually is a form of testing: *exploratory testing*.
* Done **without a plan**

If there is a plan that can be followed step-by-step, then *there is a program that can do it for you*
* If a program can do it for you, the it *should* do it for you

---

## Testing scope

As any engineering product, software can be tested at different levels of abstraction.
* **Unit** testing: test *single software components*
  * Is this `class` behavior the expected one?
  * Is this *suspension spring* behavior the expected one?
  * Is this *steel rod* mechanical properties the expected ones?
* **Integration** testing: test *an entire subsystem*, with *multiple components*
  * Is this *OCR server* behavior the expected ones?
  * Is this *engine*  working as expected?
  * Is this *span of a bridge* working correctly?
* **End-to-end** (or **acceptance**) testing: test *an entire system*
  * Is this whole application functional?
  * Is the car lapping under 2 minutes?
  * Does the bridge work nominally with high traffic and strong wind?

A well-maintained engineering product must have tests at all granularity levels

---

## Reproducibility

**Reproducibility** is **central** for testing

(true for any engineering, but in particular for software)

* *Tests should always provide the same results*
  * Tests that work sometimets but sometimes not are called *flaky tests*
* Tests should be *self-contained* (they should not depend on the results of previous tests)
* *Random* events can be tested by *seeding* the random generators

Would you be comfortable with a car that passes the crash test 99.9% of time, but on the 0.1% of the cases fails unexplicably?

---

## Test plan

Testing should be *planned for in advance*.

A good test plan can guide the development, and should be ready *early* in the project.

When designing cars,
the crash testing procedure,
the engine test bench,
and so on are prepared well before the car prototype is ready!

---

## Test-Driven Development (TDD)

The practice of:
* converting requirements to (executable) test cases
* preparing tests before development
* define the expected behavior via test cases
* track all development by always testing all cases

---

## Development cycle in TDD

1. *Capture* a requirement into an executable *test*
2. Run the test suite, the _new test should **fail**_
3. Fix the code so that the new test passes
4. Re-run the whole test suite, all tests should pass
5. Improve the quality as needed (refactor, style, duplication...)

---

## Injecting tests into projects started without tests

Developing without testing is *unsustainable*

Yet many software projects have no or minimal tests, as:

> We do not have time (or money) for testing

Beware: **testing saves times in the long run**, not testing is a *cost*!
* Untested software components are likely sources of *technical debt*

---

## A much better quotation

> we never have the money to do it *right* but somehow we always have the fucking money to do it *twice*

$---$ UserInputSucks (@UserInputSucks) [May 27, 2019](https://twitter.com/UserInputSucks/status/1132904286415929345)

---

## Tackling bugs and regressions

When a new **bug** (or a **regression**, namely, a feature that was working and it is now compromised) is discovered,
*resist the temptation to "fix" the issue right away*

* A fix without a test could be *insufficient*
* The "fix" could break another feature (create a *regression*)

A more **robust approach**:
1. Reproduce the issue in a *minimal context*
2. Create a new *test case* that *correctly fails*
3. *Fix* the issue, and make sure that the test now *passes*

Writing the test *after* the fix is much less effective.

---

## Coverage

---

## Stubbing, mocking, spying

---

## Quality Assurance

*"It works"* **is _not_ good enough**

(besides, the very notion of "it works" is debatable)

* Software quality should be *continuously assessed*
* The assessment should *automatic* whenever possible
* **QA should be integrated in the build system!**
  * It is fine to *fail the build* if quality criteria are not met

---

## Quality Assurance: levels

* *Style* and *coherence*
* *Flawed programming* patterns
* Violations of the *DRY* principle
* **Testing**
  * Multifaceted issue
  * To be executed along the whole software lifecycle
  * $\Rightarrow$ Plenty of detail in upcoming lectures

---

## Quality Assurance: style and coherence

Automated checkers are also called *linters*, often provide an auto-formatting tool

**Idiomatic** and **standardized** code:
* reduces *complexity*
* improves *understandandability*
* prevents *style-changing commits* with *unintelligible diffs*
* lowers the *maintenance* burden and related *costs*
* simplifies *code reviews*

In *Java*:
[Checkstyle](https://checkstyle.sourceforge.io/),
[PMD](https://pmd.github.io/)

In *Kotlin*:
[IDEA Inspection](https://github.com/JetBrains/inspection-plugin),
[Ktlint](https://ktlint.github.io/)

In *Scala*:
[Scalafmt](https://scalameta.org/scalafmt/),
[Scalastyle](http://www.scalastyle.org/rules-1.0.0.html)

---

## Quality Assurance: flawed programming patterns

Identification and reporting of *patterns* known to be *problematic*

* Early-interception of *potential bugs*
* Enforce *good programming principles*
* Improves *performance*
* Reduces *complexity*
* Reduces *maintenance cost*

In *Java*:
[PMD](https://pmd.github.io/),
[SpotBugs](https://spotbugs.github.io/)

In *Kotlin*:
[Detekt](https://detekt.github.io/detekt/)
[IDEA Inspection](https://github.com/JetBrains/inspection-plugin)

In *Scala*:
[Scalafix](htthttps://scalacenter.github.io/scalafix/),
[Wartremover](http://www.wartremover.org/)

---

## Quality Assurance: violations of the DRY principle

Code *replicated* rather than *reused*

* improves *understandandability*
* Reduces *maintenance cost*
* simplifies *code reviews*

General advice: **never copy/paste** your code
* If you need to copy something, you probably need to *refactor* something

Multi-language tool: [Copy/Paste Detector (CPD)](https://pmd.github.io/latest/pmd_userdocs_cpd.html) (part of PMD)

---

## Quality Assurance: testing and coverage

**Automated** software verification
* Unit level
* Integration testing
* End-to-end testing

Extension of testing can be evaluated via **coverage**.
* Coverage tells you *how much code is **untested**, not how much is tested*

Several frameworks, recommended ones:

* Testing for *all JVM languages*: [Junit/Jupiter (JUnit 5)](https://junit.org/junit5/docs/current/user-guide/)
* Testing for *Kotlin*: [Kotest](https://kotest.io/)
* Testing for *Scala*: [Scalatest](https://www.scalatest.org/)
* Coverage for all JVM languages: [JaCoCo](https://www.eclemma.org/jacoco/)
* Coverage for Scala: [Scoverage](http://scoverage.org/)

---

## Quality Assurance: JUnit + Gradle

`src/main/scala/it/unibo/test/Test.scala`

{{< github owner="APICe-at-DISI" repo="PPS-ci-examples" path="04-junit/src/main/scala/it/unibo/test/Test.scala">}}

---

## Quality Assurance: JUnit + Gradle

`src/test/scala/MyTest.scala`

{{< github owner="APICe-at-DISI" repo="PPS-ci-examples" path="04-junit/src/test/scala/MyTest.scala">}}

---

## Quality Assurance: JUnit + Gradle

`build.gradle.kts`

{{< github owner="APICe-at-DISI" repo="PPS-ci-examples" path="04-junit/build.gradle.kts">}}

---

## Quality Assurance: Scalatest + Scoverage

Let's switch *testing framework* and enable *coverage*

`src/main/scala/it/unibo/test/Test.scala`

{{< github owner="APICe-at-DISI" repo="PPS-ci-examples" path="03-scalatest/src/main/scala/it/unibo/test/Test.scala">}}

---

## Quality Assurance: Scalatest + Scoverage

`src/test/scala/Test.scala`

{{< github owner="APICe-at-DISI" repo="PPS-ci-examples" path="03-scalatest/src/test/scala/Test.scala">}}

---

## Quality Assurance: Scalatest + Scoverage

`build.gradle.kts`

{{< github owner="APICe-at-DISI" repo="PPS-ci-examples" path="03-scalatest/build.gradle.kts">}}

---

## Gradle: QA execution

* The `java` plugin (applied by the `scala` plugin under the hood) also introduces:
  * `test`: a task that runs all tests
  * `check`: a task that runs the whole quality assurance suite

---

## Additional checks and reportings

There exist a number of recommended services that provide additional QA and reports.

Non exhaustive list:
* [Codecov.io](https://codecov.io/)
    * Code coverage
    * Supports Jacoco XML reports
    * Nice data reporting system
* [Sonarcloud](https://sonarcloud.io/)
    * Multiple measures, covering reliability, security, maintainability, duplication, complexity...
* [Codacy](https://www.codacy.com/)
    * Automated software QA for several languages
* [Code Factor](https://www.codefactor.io/)
    * Automated software QA

---

## Unit testing in Python

An example repository: [https://github.com/DanySK/python-testing-101/tree/master/example-py-unittest](https://github.com/DanySK/python-testing-101/tree/master/example-py-unittest)

Try the following:
1. Clone the repository
2. Move into `example-py-unittest` folder
3. Run the tests with `python -m unittest discover` (option `-m` runs a module as script, `discover` is an option that instructs `unittest` to find and run all tests), observe the results
4. Introduce a bug in `calc.py` and re-run the tests. Observe the behavior

---

### Do it yourself!
1. Based on the project structure of the examplar, prepare a `complex.py` implementing a complex number
2. The class should support methods for adding, subtracting, multiplying, and dividing complex numbers. Create the number and implement them with `pass`
3. Prepare the test cases to verify that the behaviour is the intended one
4. Implement the methods!

**Notes**:
* A complex number can be modelled as a couple of real numbers, one for the real part, one for the imaginary part.
* Try to emulate the behavior of a number [operator overloading](https://docs.python.org/3/reference/datamodel.html#emulating-numeric-types)!


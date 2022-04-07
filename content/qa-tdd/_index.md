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

## Some common elements

Verify that the software meets quality criteria.

* **Functional** criteria:
  * Does *what* we expect it to do?
* **Non-functional** criteria:
  * Does it do it *how* we want it?

---

## Testing



---

## Testing in Python

---

## Bugs / reproducibility

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



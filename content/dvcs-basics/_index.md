 +++

title = "Software Engineering Module 3: Introduction to agile and DevOps"
description = "Distributed version control systems, basics of Git"
outputs = ["Reveal"]

+++

# Software Engineering
### **(for Intelligent Distributed Systems)**
# Module 3: DevOps

## *Distributed version control basics*

---

## Tracking changes

Did you ever need to *roll back* some project or assignment to a previous version?

How did you track the *history* of the project?

### Classic way

1. find a naming convention for files/folders
2. make a copy every time there is some relevant progress
3. make a copy every time an ambitious but risky development begins

**Inefficient!**
* Consumes a lot of resources
* Requires time
* How to tell what was in some previous releases?
* How to cherry-pick some changes?

---

## Fostering collaborative workflows

Did you ever need to develop some project or assignment *as a team*?

How did you organize the work to *maximize the productivity*?

### Classic ways

* *One screen, many heads*
  * a.k.a. one works, the other ones sleep
* *Locks*: "please do not touch section 2, I'm working on that"
  * probability of arising conflicts close to 100%
* *Realtime-sharing* (like google docs or overleaf)
  * okay in many cases for text documents (but with a risk of frankestein-ization)
  * disruptive with code (inconsistencies are much less tolerable in formal languages)

---

## Version control systems

Tools meant to support the development of projects by:
* Tracking the project *history*
* Allowing *roll-backs*
* Collecting *meta-information* on the changes
  * Authors, dates, notes...
* *Merging* information produced at different stages
* (in some cases) *facilitate parallel workflows*
* Also called Source Content Management (SCM)

**Distributed**: *Every copy* of the repository contains
(i.e., every developer locally have)
*the entire history*.

**Centralized**: A *reference copy* of the repository contains the whole history;
developers work on a subset of such history


---

## Short history

* **Concurrent Versioning System (CVS)** (1986): client-server (*centralized* model, the truth is on the server), operates on single files or repository-level, history stored in a hidden directory, uses delta compression to save space.
* **Apache Subversion (SVN)** (2000): successor to CVS, still largely used (especially in businesses that struggle to renovate their processes). *Centralized* model (similar to CVS). Improved binary file management. Improved concurrency for the operation, still cumbersome for parallel workflows.
* **Mercurial** and **Git** (both April 2005): *decentralized* version control systems (DVCSs), no "special" copy of the repository, each client stores the whole history. Highly scalable. Foster parallel work by allowing easy branching and merging. Very similar conceptually (when two succesful tools emerge at the same time with a similar model independently, it is an indication that the underlying model is "the right one" for the context).

**Git** is now the dominant DVCS (although Mercurial is still in use, e.g., for Python, Java, Facebook).

---

## Google trends to {{< today >}}

<script type="text/javascript" src="https://ssl.gstatic.com/trends_nrtr/2884_RC01/embed_loader.js"></script>
<script type="text/javascript">
trends.embed.renderExploreWidget(
  "TIMESERIES",
  {"comparisonItem":
    [
      {"keyword":"/m/05vqwg","geo":"","time":"2004-01-01 {{% today %}}"},
      {"keyword":"/m/012ct9","geo":"","time":"2004-01-01 {{% today %}}"},
      {"keyword":"/m/08441_","geo":"","time":"2004-01-01 {{% today %}}"},
      {"keyword":"/m/09d6g","geo":"","time":"2004-01-01 {{% today %}}" }
    ],
    "category":0,
    "property":""
  },
  {"exploreQuery":"date=all&q=%2Fm%2F05vqwg,%2Fm%2F012ct9,%2Fm%2F08441_,%2Fm%2F09d6g","guestPath":"https://trends.google.com:443/trends/embed/"}
);
</script>

---

## Intuition: the history of a project

1. Create a new project
```mermaid
%%{init: { width: '20' 'gitGraph': {'showBranches': false, 'showCommitLabel': false}} }%%
gitGraph
  commit id: "Initialize project"
```

---

2. Make some changes

```mermaid
gitGraph
  commit id: "Initialize project"
  commit id: "Make some changes"
```

---

3. Then more and more, until the project is ready

```mermaid
gitGraph
  commit id: "Initialize project"
  commit id: "Make some changes"
  commit id: "Make more changes"
  commit id: "It's finished!" type: HIGHLIGHT
```

At a first glance, the history of a project *looks like* a **line**.

---

## Except that, in the real world...

> Anything that can go wrong will go wrong
> <br><cite>$1^{st}$ Murphy's law</cite>

> If anything simply cannot go wrong, it will anyway
> <cite>$5^{th}$ Murphy's law</cite>

---

# ...things go wrong

```mermaid
gitGraph
  commit id: "Initialize project"
  commit id: "Make some changes"
  commit id: "Make more changes"
  commit id: "It's *not* finished! :(" type: REVERSE tag: "fail"
```

---

## Rolling back changes

Go *back in time* to a previous state where things work

```mermaid
gitGraph
  commit id: "init"
  commit id: "okay" type: HIGHLIGHT
  commit id: "error" type: REVERSE
  commit id: "reject" type: REVERSE
```

---

## Get the previous version and fix

Then fix the mistake

```mermaid
gitGraph
  commit id: "init"
  commit id: "okay"
  branch fix-issue
  checkout master
  commit id: "error" type: REVERSE
  commit id: "reject" type: REVERSE
  checkout fix-issue
  commit id: "improve"
  commit id: "finished" type: HIGHLIGHT
```

If you consider rollbacks, history is a **tree**!

---

# Collaboration: diverging

Alice and Bob work together for some time, then they go home and work separately, in parallel

They have a *diverging history*!

```mermaid
%%{init: { 'gitGraph': { 'mainBranchName': 'alice'}} }%%
gitGraph
  commit id: "together-1"
  commit id: "together-2"
  commit id: "together-3"
  branch bob
  commit id: "bob-4"
  checkout alice
  commit id: "alice-4"
  checkout bob
  commit id: "bob-5"
  checkout alice
  commit id: "alice-5"
  checkout bob
  commit id: "bob-6"
  checkout alice
  commit id: "alice-6"
```

---

# Collaboration: reconciling

```mermaid
%%{init: { 'gitGraph': { 'mainBranchName': 'alice'}} }%%
gitGraph
  commit id: "together-1"
  commit id: "together-2"
  commit id: "together-3"
  branch bob
  commit id: "bob-4"
  checkout alice
  commit id: "alice-4"
  checkout bob
  commit id: "bob-5"
  checkout alice
  commit id: "alice-5"
  checkout bob
  commit id: "bob-6"
  checkout alice
  commit id: "alice-6"
  merge bob tag: "reconciled!"
  commit id: "together-7"
```

If you have the possibility to *reconcile diverging developments*, the history becomes a **graph**!

Reconciling diverging developments is usually referred to as **merge**

---

## DVCS concepts and terminology: *Repository*

Project **meta-data**. Includes the whole project history
* information on how to *roll back* changes 
* *authors* of changes
* *dates*
* *differences* between different points in time
* and so on

Usually, stored in a hidden folder in the *root folder* of the project

---

## DVCS concepts and terminology: *Working Tree*

(or *worktree*, or *working directory*)

the collection of **files** (usually, inside a *root folder*) that constitute the project,
excluding the *meta-data*.

---

## DVCS concepts and terminology: *Commit*

A **saved status** of the project.
* Collects the *changes* required to transform the previous (*parent*) commit into the current (*differential tracking*)
* Creates a *snapshot* of the status of the worktree (snapshotting).
* Records metadata: *parent commit*, *author*, *date*, a *message* summarizing the changes, and a *unique identifier*.
* A commit with no parent is an *initial commit*.
* A commit with multiple parents is a *merge commit*.

```mermaid
%%{init: { 'gitGraph': { 'mainBranchName': 'alice'}} }%%
gitGraph
  commit tag: "initial"
  commit
  commit
  branch bob
  commit
  checkout alice
  commit
  checkout bob
  commit
  checkout alice
  commit
  checkout bob
  commit
  checkout alice
  commit
  merge bob tag: "merge"
  commit
```

---

## DVCS concepts and terminology: *Branch*

A **named sequence of commits**

```mermaid
%%{init: { 'gitGraph': { 'mainBranchName': 'default-branch'}} }%%
gitGraph
  commit
  commit
  commit
  branch branch2
  commit
  checkout default-branch
  commit
  checkout branch2
  branch branch3
  commit
  checkout default-branch
  branch branch4
  commit
  commit
  commit
  commit
  checkout branch3
  merge branch4
  commit
  commit
  commit
  checkout default-branch
  merge branch3
  checkout branch2
  commit
  commit
  merge default-branch
```

If no branch has been created at the first commit, a default name is used.

---

## DVCS concepts and terminology: *Commit references*

To be able to go *back in time* or *change branch*, we need to **refer to commits**

* Every commit has a **unique identifier**, which is a valid reference
* A **branch name** is a valid commit reference (points to the *last commit of that branch*)
* A special commit name is  **HEAD**, which refers to the *current commit*
  * When committing, the **HEAD** moves forward to the new commit

Commit references are also referred to as `tree-ish`es

### Absolute and relative references

Appending `~` and a number `i` to a valid tree-ish means "`i-th` parent of this tree-ish"

```mermaid
%%{init: { 'gitGraph': { 'mainBranchName': 'master', 'showCommitLabel': false}} }%%
gitGraph
  commit tag: "HEAD~8"
  commit tag: "HEAD~7"
  commit tag: "HEAD~6"
  commit tag: "HEAD~5"
  commit tag: "HEAD~4"
  commit tag: "HEAD~3"
  commit tag: "HEAD~2"
  commit tag: "HEAD~1"
  commit tag: "HEAD"
  commit
  commit
```

```mermaid
%%{init: { 'gitGraph': { 'mainBranchName': 'b0', 'showCommitLabel': false}} }%%
gitGraph
  commit tag: "b0~8"
  commit tag: "b0~7"
  commit tag: "b0~6"
  commit tag: "b0~5"
  commit tag: "b0~4"
  commit tag: "b0~3"
  commit tag: "b0~2"
  commit tag: "b0~1"
  commit tag: "b0"
```

---

## DVCS concepts and terminology: *Checkout*

The operation of **moving to another commit**
* Moving to *another branch*
* Moving *back in time*

Moves the `HEAD` to the specified *target tree-ish*

---

## DVCS concepts and terminology: *Head*

A pointer to the **current commit**

```mermaid
%%{init: { 'gitGraph': { 'mainBranchName': 'default-branch'}} }%%
gitGraph
  commit
  commit
  commit
  branch branch2
  commit
  checkout default-branch
  commit
  checkout branch2
  branch branch3
  commit
  checkout default-branch
  branch branch4
  commit
  commit
  commit
  commit
  checkout branch3
  merge branch4
  commit
  commit
  commit
  checkout default-branch
  merge branch3
  checkout branch2
  commit
  commit
  merge default-branch
```

If no branch has been created at the first commit, a default name is used.

---

## Project evolution example

Let us try to see what happens when ve develop some project, step by step.

---


1. first commit

```mermaid
flowchart RL
  HEAD{{HEAD}}
  b1(default-branch)

  C1([1])

  HEAD -.-> C1
  HEAD --"fas:fa-link"--o b1
  b1 -.-> C1

  class HEAD head
  class b1 branch
  class C1 commit
```

2. second commit

```mermaid
flowchart RL
  HEAD{{HEAD}}
  b1(default-branch)

  C2([2]) --> C1([1])

  HEAD -.-> C2
  HEAD --"fas:fa-link"--o b1
  b1 -.-> C2

  class HEAD head;
  class b1,b2 branch;
  class C1,C2 commit;
```

---

![four commits later](4commitslater.png)

---

```mermaid
flowchart RL
  HEAD{{HEAD}}
  b1(default-branch)

  C6([6]) --> C5([5]) --> C4([4]) --> C3([3]) --> C2([2]) --> C1([1])

  b1 -.-> C6

  HEAD -.-> C6
  HEAD --"fas:fa-link"--o b1

  class HEAD head;
  class b1,b2 branch;
  class C1,C2,C3,C4,C5,C6 commit;
```

Oh, no, there was a mistake! We need to roll back!

---

## *checkout of C4*

```mermaid
flowchart RL
  HEAD{{HEAD fas:fa-unlink}}
  b1(default-branch)

  C6([6]) --> C5([5]) --> C4([4]) --> C3([3]) --> C2([2]) --> C1([1])

  b1 -.-> C6

  HEAD -.-> C4

  class HEAD head;
  class b1,b2 branch;
  class C1,C2,C3,C4,C5,C6 commit;
```

* No information is lost, we can get back to `6` whenever we want to.
* what if we commit now?

---

## Branching!


```mermaid
flowchart RL
  HEAD{{"HEAD"}}
  b1(default-branch)
  b2(new-branch)

  C6([6]) --> C5([5]) --> C4([4]) --> C3([3]) --> C2([2]) --> C1([1])
  C7([7]) --> C4([4])
  
  b1 -.-> C6
  b2 -.-> C7

  HEAD -.-> C7
  HEAD --"fas:fa-link"--o b2

  class HEAD head;
  class b1,b2 branch;
  class C1,C2,C3,C4,C5,C6,C7,C8 commit;
```

* Okay, but there was useful stuff in `5`, I'd like to have it into `new-branch`

---

## Merging!

```mermaid
flowchart RL
  HEAD{{"HEAD"}}
  b1(default-branch)
  b2(new-branch)

  C6([6]) --> C5([5]) --> C4([4]) --> C3([3]) --> C2([2]) --> C1([1])
  C7([7]) --> C4
  C8([8]) --> C7
  C8 --> C5
  
  b1 -.-> C6
  b2 -.-> C8

  HEAD -.-> C8
  HEAD --"fas:fa-link"--o b2

  class HEAD head;
  class b1,b2 branch;
  class C1,C2,C3,C4,C5,C6,C7,C8 commit;
```

**Notice that:**
* we have two branches
* `8` is a merge commit, as it has two parents: `7` and `5`
* the situation is the same regardless that is a *single developer going back on the development* or *multiple developers working in parallel*!
* this is possible because *every copy of the repository contains the entire history*!

---

## Reference DVCS: Git

De-facto reference distributed version control system

* *Distributed*
* Born in *2005* to replace BitKeeper as SCM for the Linux kernel
  * Performance was a major concern
  * Written in C
* Developed by Linus Torvalds
  * Now maintained by others
* *Unix-oriented*
  * Tracks Unix file permissions
* Very *fast*
  * At conception, 10 times faster than Mercurial¹, 100 times faster than Bazaar

¹ Less difference now, Facebook vastly improved Mercurial

---

## Funny historical introduction

{{< youtube id="4XpnKHJAok8" autoplay="false" title="Linus Torvalds introduces Git at Google">}}

---

## Approach: terminal-first

#### (actually: terminal-only)

**Git is a command line tool**

Although graphical interfaces exsist, it makes no sense to learn a GUI:
* they are more prone to future changes than the CLI
* they add a level of interposition between you and the tool
* unless they are incomplete, they expose *more complexity* than what we can deal with in this course
  * what do you do with a checkbox labeled "squash when merging"?
  * and what about *recursively checkout submodules*?
* as soon as you learn *the CLI*, you become so proficient that you get *slower* when there is a graphical interface in-between

**I am assuming minimal knowledge of the shell, please let me know NOW if you've never seen it**

---

## Terminal cheat sheet

| Operation | <i class="fab fa-linux"></i> <i class="fab fa-apple"></i> *nix | <i class="fab fa-windows"></i> win |
| --- | --- | --- |
| Print the current directory location | `pwd` | `echo %cd%`
| Remove the file `foo` (does not work with directories) | `rm foo` | `del foo` |
| Remove directory `bar` | `rm -r bar` | `del bar` |
| Change disk (e.g., switch to `D:`) | n.a., single root (`/`) | `D:` |
| Move to the subdirectory `baz` | `cd baz` | `cd baz` |
| Move to the parent directory | `cd ..` | `cd..` |
| Move (rename) file `foo` to `baz` | `mv foo baz` | `move foo baz` |
| Copy file `foo` to `baz` | `cp foo baz` | `copy foo baz` |
| Create a directory named `bar` | `mkdir bar` | `md bar` |

---

## Initializing a repository

### `git init`
* Initializes a new repository *inside the current directory*
* Reified in the `.git` folder
* The location of the `.git` folder marks the root of the repository
  * Do not nest repositories inside repositories, it is fragile
  * Nested projects are realized via *submodules* (not discussed in this course)
* **Beware of the place where you issue the command!**
  * First use `cd` to locate yourself inside the folder that contains (or will containe the project)
    * (possibly, first create the folder with `mkdir`)
  * **Then** issue `git init`
  * if something goes awry, you can delete the repository by deleting the `.git` folder.

---

## Staging

Git has the concept of *stage* (or *index*).
* Changes must be added to the stage to be committed.
* Commits save the *__changes__ included in the stage*
  * Files changed after being added to the stage neet to be re-staged
* `git add <files>` moves the current state of the files into the stage as *changes*
* `git reset <files>` removes currently staged *changes* of the files from stage
* `git commit` creates a new *changeset* with the contents of the stage

{{< image src="staging.png" >}}

---

## Observing the repository status

It is extremely important to understand *clearly* what the current state of affairs is
* Which *branch* are we working on?
* Which *files* have been *modified*?
* Which *changes* are already *staged*?

`git status` prints the current state of the repository, example output:

```git
❯ git status
On branch master
Your branch is up to date with 'origin/master'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   content/_index.md
        new file:   content/dvcs-basics/_index.md
        new file:   content/dvcs-basics/staging.png

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   layouts/shortcodes/gravizo.html
        modified:   layouts/shortcodes/today.html
```

---

## Committing

* Requires an **author** and an **email**
  * They can be configured *globally* (at the *computer level*):
    * `git config --global user.name 'Your Real Name'`
    * `git config --global user.email 'your@email.com'`
  * The global settings can be *overridden* at the *repository level*
    * e.g., you want to commit with a different email between work and personal projects
    * `git config user.name 'Your Real Name'`
    * `git config user.email 'your@email.com'`
* Requires a **message**, using appropriate messages is **extremely important**
  * If unspecified, the commit does not get performed
  * it can be specified inline with `-m`, otherwise Git will pop up the *default editor*
    * `git commit -m 'my very clear and explanatory message'`
* The *date* is recorded automatically
* The *commit identifier* (a cryptographic hash) is generated automatically

---

## Default branch

At the first commit, there is no branch and no `HEAD`.

Depending on the version of Git, the following behavior may happen upon the first commit:
* Git creates a *new branch* named `master`
  * *legacy behavior*
  * the name is inherited from the default branch name in *Bitkeeper*
* Git creates a *new branch* named `master`, but warns that it is a deprecated behavior
  * although coming from the Latin "*magister*" (teacher) and not from the "master/slave" model of asymmetric communication control, many recently prefer `main` as seen as more inclusive
* Git refuses to commit until a default branch name is specified
  * *modern behavior*
  * Requires configuration: `git config --global init.defaultbranch default-branch-name`

---

## Ignoring files

In general, we do not want to track *all* the files in the repository folder:
* Some files could be *temporary* (e.g., created by the editor)
* Some files could be *regenerable* (e.g., compiled binaries and application archives)
* Some files could contain *private* information

Of course, we could just not `add` them, but the error is around the corner!

It would be much better to just tell Git to ignore some files.

This is achieved through a *special `.gitignore` file*.
  * the file must be named `.gitignore`, names like `foo.gitignore` or `gitignore.txt` won't work
    * A good way to create/append to this file is via `echo whatWeWantToIgnore >> .gitignore` (multiplatform command)
  * it is a list of paths that git will ignore (unless `git add` is called with the `--force` option)
  * it is possible to add exceptions

---

## `.gitignore` example

```ignore-list
# ignore the bin folder and all its contents
bin/
# ignore every pdf file
*.pdf
# rule exception (beginning with a !): pdf files named 'myImportantFile.pdf' should be tracked
!myImportantFile.pdf
```

---

## Dealing with removal and renaming of files

* The removal of a file is a legit *change*
* As we discussed, `git add` adds a *change* to the stage
* **the change can be a removal!**

`git add someDeletedFile` is a correct command, that will stage the fact that `someDeletedFile` does not exist anymore, and its deletion must be registered at the next `commit`.

* File *renaming* is *equivalent to file deletion and file creation* where, incidentally, the new file has the same content of the deleted file
* To stage the rinomination of file `foo` into `bar`:
  * `git add foo bar`
  * it records that `foo` has been deleted and `bar` has been created
  * Git is smart enough to understand that it is a name change, and will deal with it *efficiently*

---

## Visualizing the history

Of course, it is useful to visualize the history of commits.
Git provides a dedicated sub-command:

`git log`

* opens a *navigable interactive view* of the history from the `HEAD` commit (the current commit) backwards
  * Press <kbd>Q</kbd>
* *compact* visualization: `git log --oneline`
* visualization of *all branches*: `git log --all`
* visualization of a lateral *graph*: `git log --graph`
* compact visualization of all branches with a graph: `git log --oneline --all --graph`

---

### example output of `git log --oneline --all --graph`

```text
* d114802 (HEAD -> master, origin/master, origin/HEAD) moar contribution
| * edb658b (origin/renovate/gohugoio-hugo-0.94.x) ci(deps): update gohugoio/hugo action to v0.94.2
|/  
* 4ce3431 ci(deps): update gohugoio/hugo action to v0.94.1
* 9efa88a ci(deps): update gohugoio/hugo action to v0.93.3
* bf32a8b begin with build slides
* b803a65 lesson 1 looks ready
* 6a85f8f ci(deps): update gohugoio/hugo action to v0.93.2
* b474d2a write more on the introductory lesson
* 8a7105e ci(deps): update gohugoio/hugo action to v0.93.1
* 6e40642 begin writing the first lesson
```

---

## Referring to commits: `<tree-ish>`es

In git, a reference to a commit is called `<tree-ish>`. Valid `<tree-ish>`es are:
* Full *commit hashes*, such as `b82f7567961ba13b1794566dde97dda1e501cf88`.
* *Shortened commit hashes*, such as `b82f7567`.
* *Branch names*, in which case the reference is to the last commit of the branch.
* `HEAD`, a special name referring to the current commit (the head, indeed).
* *Tag names* (we will discuss what a tag is later on).

---

## Relative references

It is possible to build *relative references*, e.g., "get me the commit before this `<tree-ish>`",
by following the commit `<tree-ish>` with a tilde (`~`) and with the number of parents to get to:
* `<tree-ish>~STEPS` where `STEPS` is an integer number produces a reference to the `STEPS-th` parent of the provided `<tree-ish>`:
  * `b82f7567~1` references the *parent* of commit `b82f7567`.
  * `some_branch~2` refers to the *parent of the parent* of the last commit of branch `some_branch`.
  * `HEAD~3` refers to the *parent of the parent of the parent* of the current commit.

* In case of merge commits (with multiple parents), `~` selects the first one
* Selection of parents can be performed with caret in case of multiple parents (`^`)
  * We won't go in depth here, but:
    * The [`git rev-parse` reference on specifying revision](https://git-scm.com/docs/git-rev-parse#_specifying_revisions) is publicly available
    * A [much more readable explanation can be found on Stack overflow](https://stackoverflow.com/a/2222920/1916413)

---

## Visualizing the differences

We want to see which *differences* a commit introduced, or what we modified in some files of the work tree

Git provides support to visualize the changes in terms of *modified lines* through `git diff`:
* `git diff` shows the difference between the *stage* and the *working tree*
  * namely, what you would stage if you perform a `git add`
* `git diff --staged` shows the difference between `HEAD` and the *working tree*
* `git diff <tree-ish>` shows the difference between `<tree-ish>` and the *working tree* (*stage excluded*)
* `git diff --staged <tree-ish>` shows the difference between `<tree-ish>` and the *working tree*, *including staged changes*
* `git diff <from> <to>`, where `<from>` and `<to>` are `<tree-ish>`es, shows the differences between `<from>` and `<to>`

---

### `git diff` Example output:

```diff
diff --git a/.github/workflows/build-and-deploy.yml b/.github/workflows/build-and-deploy.yml
index b492a8c..28302ff 100644
--- a/.github/workflows/build-and-deploy.yml
+++ b/.github/workflows/build-and-deploy.yml
@@ -28,7 +28,7 @@ jobs:
           # Idea: the regex matcher of Renovate keeps this string up to date automatically
           # The version is extracted and used to access the correct version of the scripts
           USES=$(cat <<TRICK_RENOVATE
-          - uses: gohugoio/hugo@v0.94.1
+          - uses: gohugoio/hugo@v0.93.3
           TRICK_RENOVATE
           )
           echo "Scripts update line: \"$USES\""
```

The output is compatible with the Unix commands `diff` and `patch`

Still, *binary files are an issue*! Tracking the right files is paramount.

---

## Navigating the history

Navigation of the history concretely means to move the head (in Git, `HEAD`) to arbitrary points of the history

In Git, this is performed with the `checkout` commit:
* `git checkout <tree-ish>`
  * Unless there are changes that could get lost, *moves* `HEAD` to the provided `<tree-ish>`
  * Updates all tracked files to their version at the provided `<tree-ish>`

The command can be used to selectively checkout a file from another revision:
* `git checkout <tree-ish> -- foo bar baz`
  * Restores the status of files `foo`, `bar`, and `baz` from commit `<tree-ish>`, and adds them to the stage (unless there are uncommitted changes that could be lost)
  * Note that `--` is surrounded by whitespaces, it is not a `--foo` option, it is just used as a separator between the `<tree-ish>` and the list of files
    * the files could be named as a `<tree-ish>` and we need disambiguation

---

## Detached head

Git does **not** allow *multiple heads per branch*
(other DVCS do, in particular Mercurial):
for a commit to be valid, `HEAD` must be at the "end" of a branch (on its last commit), as follows:

```mermaid
flowchart RL
  HEAD{{"HEAD"}}
  b1(master)

  C10([10]) --> C9([9]) --> C8([8]) --> C7([7]) --> C6([6]) --> C5([5]) --> C4([4]) --> C3([3]) --> C2([2]) --> C1([1])
  b1 -.-> C10

  HEAD -.-> C10
  HEAD --"fas:fa-link"--o b1

  class HEAD head;
  class b1,b2 branch;
  class C1,C2,C3,C4,C5,C6,C7,C8,C9,C10 commit;
```

When an old commit is checked out this condition doesn't hold!

If we run `git checkout HEAD~4`:

```mermaid
flowchart RL
  HEAD{{"HEAD fas:fa-unlink"}}
  b1(master)

  C10([10]) --> C9([9]) --> C8([8]) --> C7([7]) --> C6([6]) --> C5([5]) --> C4([4]) --> C3([3]) --> C2([2]) --> C1([1])

  b1 -.-> C10

  HEAD -.-> C6

  class HEAD head;
  class b1,b2 branch;
  class C1,C2,C3,C4,C5,C6,C7,C8,C9,C10 commit;
```

The system enters a special workmode called *detached head*.

When **in detached head**, Git allows to make **commits**, but they **are lost**!

(Not really, but to retrieve them we need `git reflog` and `git cherry-pick`, that we won't discuss)

---

### Creating branches

To store our commits, we need to *create* a **branch**, then attach the `HEAD` by checking it out.

In Git, new branches are created with `git branch branch_name`

```mermaid
flowchart RL
  HEAD{{"HEAD fas:fa-unlink"}}
  b1(master)

  C10([10]) --> C9([9]) --> C8([8]) --> C7([7]) --> C6([6]) --> C5([5]) --> C4([4]) --> C3([3]) --> C2([2]) --> C1([1])

  b1 -.-> C10

  HEAD -.-> C6

  class HEAD head;
  class b1,b2 branch;
  class C1,C2,C3,C4,C5,C6,C7,C8,C9,C10 commit;
```

⬇️ `git branch new-experiment` ⬇️

```mermaid
flowchart RL
  HEAD{{"HEAD fas:fa-unlink"}}
  b1(master)
  b2("new-experiment")

  C10([10]) --> C9([9]) --> C8([8]) --> C7([7]) --> C6([6]) --> C5([5]) --> C4([4]) --> C3([3]) --> C2([2]) --> C1([1])

  b1 -.-> C10

  HEAD -.-> C6
  b2 -.-> C6

  class HEAD head;
  class b1,b2 branch;
  class C1,C2,C3,C4,C5,C6,C7,C8,C9,C10 commit;
```

---

```mermaid
flowchart RL
  HEAD{{"HEAD fas:fa-unlink"}}
  b1(master)
  b2("new-experiment")

  C10([10]) --> C9([9]) --> C8([8]) --> C7([7]) --> C6([6]) --> C5([5]) --> C4([4]) --> C3([3]) --> C2([2]) --> C1([1])

  b1 -.-> C10

  HEAD -.-> C6
  b2 -.-> C6

  class HEAD head;
  class b1,b2 branch;
  class C1,C2,C3,C4,C5,C6,C7,C8,C9,C10 commit;
```

⬇️ `git checkout -b new-experiment` ⬇️

```mermaid
flowchart RL
  HEAD{{"HEAD"}}
  b1(master)
  b2("new-experiment")

  C10([10]) --> C9([9]) --> C8([8]) --> C7([7]) --> C6([6]) --> C5([5]) --> C4([4]) --> C3([3]) --> C2([2]) --> C1([1])

  b1 -.-> C10

  HEAD -.-> C6
  HEAD --"fas:fa-link"--o b2
  b2 -.-> C6

  class HEAD head;
  class b1,b2 branch;
  class C1,C2,C3,C4,C5,C6,C7,C8,C9,C10 commit;
```

---

## One-shot branch creation

As you can imagine, creating a *new branch* and *attaching `HEAD`* to the freshly created branch is pretty common

Very common, a short-hand is provided: `git checkout -b new-branch-name`
* Creates `new-branch-name` from the current position of `HEAD`
* Attaches `HEAD` to `new-branch-name`

```mermaid
flowchart RL
  HEAD{{"HEAD fas:fa-unlink"}}
  b1(master)

  C10([10]) --> C9([9]) --> C8([8]) --> C7([7]) --> C6([6]) --> C5([5]) --> C4([4]) --> C3([3]) --> C2([2]) --> C1([1])

  b1 -.-> C10

  HEAD -.-> C6

  class HEAD head;
  class b1,b2 branch;
  class C1,C2,C3,C4,C5,C6,C7,C8,C9,C10 commit;
```

⬇️ `git checkout -b new-experiment` ⬇️

```mermaid
flowchart RL
  HEAD{{"HEAD"}}
  b1(master)
  b2("new-experiment")

  C10([10]) --> C9([9]) --> C8([8]) --> C7([7]) --> C6([6]) --> C5([5]) --> C4([4]) --> C3([3]) --> C2([2]) --> C1([1])

  b1 -.-> C10

  HEAD -.-> C6
  HEAD --"fas:fa-link"--o b2
  b2 -.-> C6

  class HEAD head;
  class b1,b2 branch;
  class C1,C2,C3,C4,C5,C6,C7,C8,C9,C10 commit;
```

---

## Merging branches

Reunifying diverging development lines is *much trickier* than spawning new development lines

In other words, *merging* is **much trickier** than *branching*

* Historically, with *centralized* version control systems, merging was considered extremely delicate and difficult
* The *distributed* version control systems promoted *frequent*, *small-sized* merges, much easier to deal with
* **Conflicts** *can still arise!*
  * what if we change the same line of code in two branches differently?

In Git, `git merge target` merges the branch named `target` into the current branch (`HEAD` must be attached)

---

## Merge visual example

```mermaid
flowchart RL
  HEAD{{"HEAD"}}
  b1(master)
  b2("new-experiment")

  C10([10]) --> C9([9]) --> C8([8]) --> C7([7]) --> C6([6]) --> C5([5]) --> C4([4]) --> C3([3]) --> C2([2]) --> C1([1])
  C12([12]) --> C11([11]) --> C6 

  b1 -.-> C10

  HEAD -.-> C12
  HEAD --"fas:fa-link"--o b2
  b2 -.-> C12

  class HEAD head;
  class b1,b2 branch;
  class C1,C2,C3,C4,C5,C6,C7,C8,C9,C10,C11,C12 commit;
```

⬇️ `git merge master` ⬇️

```mermaid
flowchart RL
  HEAD{{"HEAD"}}
  b1(master)
  b2("new-experiment")

  C10([10]) --> C9([9]) --> C8([8]) --> C7([7]) --> C6([6]) --> C5([5]) --> C4([4]) --> C3([3]) --> C2([2]) --> C1([1])
  C13([13]) --> C12([12]) --> C11([11]) --> C6 
  C13 --> C10

  b1 -.-> C10

  HEAD -.-> C13
  HEAD --"fas:fa-link"--o b2
  b2 -.-> C13

  class HEAD head;
  class b1,b2 branch;
  class C1,C2,C3,C4,C5,C6,C7,C8,C9,C10,C11,C12,C13 commit;
```


---

## Fast forwarding

Consider this situation:

```mermaid
flowchart RL
  HEAD{{"HEAD"}}
  b1(master)
  b2("new-experiment")

  C10([10]) --> C9([9]) --> C8([8]) --> C7([7]) --> C6([6]) --> C5([5]) --> C4([4]) --> C3([3]) --> C2([2]) --> C1([1])

  b1 -.-> C10

  HEAD -.-> C6
  HEAD --"fas:fa-link"--o b2
  b2 -.-> C6

  class HEAD head;
  class b1,b2 branch;
  class C1,C2,C3,C4,C5,C6,C7,C8,C9,C10,C11,C12,C13 commit;
```

* We want `new-experiment` to also have the changes in `C5` and `C6` (to be up to date with `master`)
* `master` contains all the commits of `new-experiment`
* We don't really need a merge commit, we can just move `new-experiment` to point it to `C6`
* $\Rightarrow$ This is called a **fast-forward**
  * It is the *default behavior* in Git when merging branches where the target is the head plus something


```mermaid
flowchart RL
  HEAD{{"HEAD"}}
  b1(master)
  b2("new-experiment")

  C10([10]) --> C9([9]) --> C8([8]) --> C7([7]) --> C6([6]) --> C5([5]) --> C4([4]) --> C3([3]) --> C2([2]) --> C1([1])

  b1 -.-> C10

  HEAD -.-> C10
  HEAD --"fas:fa-link"--o b2
  b2 -.-> C10

  class HEAD head;
  class b1,b2 branch;
  class C1,C2,C3,C4,C5,C6,C7,C8,C9,C10,C11,C12,C13 commit;
```

---

## Merge conflicts

Git tries to resolve most conflicts by *itself*
* It's *pretty good* at it
* but things can still require *human intervention*

In case of conflict on one or more files, Git marks the subject files as *conflicted*, and modifies them adding *merge markers*:

```text
<<<<<<< HEAD
Changes made on the branch that is being merged into,
this is the branch currently checked out (HEAD).
=======
Changes made on the branch that is being merged in.
>>>>>>> other-branch-name
```

* The user should *change the conflicted files* so that they reflect the *final desired status*
* The (now fixed) files should get added to the stage with `git add`
* The merge operation can be concluded through `git commit`
  * In case of merge, the message is pre-filled in

---

## Good practices

**Avoiding merge conflicts is *much* better than solving them**

Although they are unavoidable in some cases, they can be *minimized* by following a few *good practices*:

* **Do not** *track files that can be generated*
  * This is harmful under many points of view, and merge conflicts are one
* **Do** *make many small commits*
  * Each coherent change should be reified into a commit
  * Even very small changes, like modification of the whitespaces
  * Smaller commits help Git better figure out what changed and in which order,
  generally leading to finer grained (and easier to solve) conflicts
* **Do** *enforce style rules* across the team
  * Style changes are legitimate changes
  * Style is often enforced at the IDE level
  * Minimal logical changes may cause widespread changes due to style modifications
* **Do** *pay attention to newlines*
  * Different OSs use different newline characters
  * Git tries to be smart about it, often failing catastrophically

---

## Going to a new line is more complicated than it seems

Going to a new line is a two-phased operation:
1. Bring the cursor back to the begin of the line
2. Bring the cursor down one line

In *electromechanic teletypewriters* (and in typewriters, too), they were two distinct operations:
1. *Carriage Return* (bringing the carriage to its leftmost position)
2. *Line Feed* (rotating the carriage of one step)

---

## A teletypewriter

![teletypewriter](https://upload.wikimedia.org/wikipedia/commons/thumb/9/93/Telescrivente_CEP_-_Calcolatrice_Elettronica_Pisana.jpg/1080px-Telescrivente_CEP_-_Calcolatrice_Elettronica_Pisana.jpg)

* driving them text without drivers required to *explicitly send* *carriage return* and *line feed* commands

---

## Newlines in the modern world

Terminals were designed to behave like virtual teletypewriters
* Indeed, they are still called **TTY** (**T**ele**TY**pewriter)
* In Unix-like systems, they are still implemented as *virtual devices*
  * If you have MacOS X or Linux, you can see which virtual device backs your current terminal using `tty`
* At some point, Unix decided that `LF` was sufficient in virtual TTYs to go to a new line
  * Probably *inspired by the C language*, where `\n` means "newline"
  * The behaviour can still be disabled
```text
we would get
            lines
                 like these
```

####  Consequence:
* Windows systems go to a new line with a `CR` character followed by an `LF` character: `\r\n`
* Unix-like systems go to a new line with an `LF` character: `\n`
* Old Mac systems used to go to a new line with a `CR` character: `\r`
  * Basically they decided to use a single character like Unix did, but made the opposite choice
  * MacOS X is POSIX-compliant, uses `\n`

---

## Newlines and version control

If your team uses *multiple OSs*, it is likely that, by default, the text editors use either `LF` (on Unix) or `CRLF`

It is also very likely that, upon saving, the whole file gets rewritten with the "*locally* correct" *line endings*

* This however would result in *all the lines being changed*!
* The differential would be huge
* *Conflicts would arise everywhere*!

Git tries to tackle this issue by converting the line endings so that they match the initial line endings of the file,
resulting in repositories with *illogically mixed line endings*
(depending on who created a file first)
and loads of warnings about `LF`/`CRLF` conversions.

Line endings should instead be **configured per file type!**

---

## `.gitattributes`

* A sensible strategy is to use `LF` everywhere, but for Windows scripts (`bat`, `cmd`, `ps1`)
* Git can be configured through a `.gitattributes` file in the repository root
  * It can do [much more than enforcing line endings](https://git-scm.com/docs/gitattributes), actually
* Example: 
```text
* text=auto eol=lf
*.[cC][mM][dD] text eol=crlf
*.[bB][aA][tT] text eol=crlf
*.[pP][sS]1 text eol=crlf
```

---

## Associating symbolic names to commits

It is often handful to associate some commits with a *symbolic name*,
most of the time to assign *versions*.
* e.g., identify commit `8d400c0` as version `1.2.3`

Although in principle *branches* could be used to do so, their nature is of *moving labels*:
when `HEAD` is attached, new commits move the branch forward.
We would like to have *branches to which `HEAD` cannot attach* (hence, they can't be moved from their creation point).

```mermaid
flowchart RL
  HEAD{{"HEAD"}}
  master(master)

  C10([10]) --> C9([9]) --> C8([8]) --> C7([7]) --> C6([6]) --> C5([5]) --> C4([4]) --> C3([3]) --> C2([2]) --> C1([1])

  master -.-> C10

  HEAD -.-> C10
  HEAD --"fas:fa-link"--o master

  class HEAD head;
  class master branch;
  class C1,C2,C3,C4,C5,C6,C7,C8,C9,C10,C11,C12,C13 commit;
```

⬇️ `git checkout C4 && git branch 1.2.3 && git checkout master` ⬇️

```mermaid
flowchart RL
  HEAD{{"HEAD"}}

  master(master)
  tag1(1.2.3)

  C10([10]) --> C9([9]) --> C8([8]) --> C7([7]) --> C6([6]) --> C5([5]) --> C4([4]) --> C3([3]) --> C2([2]) --> C1([1])

  master -.-> C10

  HEAD -.-> C10
  HEAD --"fas:fa-link"--o master
  tag1 --o C4

  class HEAD head;
  class master,tag1 branch;
  class C1,C2,C3,C4,C5,C6,C7,C8,C9,C10,C11,C12,C13 commit;
```

---

## Branches as attachable (and movable) labels

```mermaid
flowchart RL
  HEAD{{"HEAD"}}

  master(master)
  tag1(1.2.3)

  C10([10]) --> C9([9]) --> C8([8]) --> C7([7]) --> C6([6]) --> C5([5]) --> C4([4]) --> C3([3]) --> C2([2]) --> C1([1])

  master -.-> C10

  HEAD -.-> C10
  HEAD --"fas:fa-link"--o master
  tag1 --o C4

  class HEAD head;
  class master,tag1 branch;
  class C1,C2,C3,C4,C5,C6,C7,C8,C9,C10,C11,C12,C13 commit;
```

Looks good, but if we do something like: ⬇️ `git checkout 1.2.3` [some changes] `git commit` ⬇️

```mermaid
flowchart RL
  HEAD{{"HEAD"}}

  master(master)

  tag1(1.2.3)

  C10([10]) --> C9([9]) --> C8([8]) --> C7([7]) --> C6([6]) --> C5([5]) --> C4([4]) --> C3([3]) --> C2([2]) --> C1([1])
  C11([11]) --> C4
  master -.-> C10

  HEAD -.-> C10
  HEAD --"fas:fa-link"--o master
  tag1 --o C11

  class HEAD head;
  class master,tag1 branch;
  class C1,C2,C3,C4,C5,C6,C7,C8,C9,C10,C11,C12,C13 commit;
```

Our version **moved**, *we never want this to happen*!

---

## Tagging

The `tag` subcommand to create *permanent labels* attached to commits.
Tags come in two fashions:
* **Lightweight** *tags* are very similar to a "permanent branch": *pointers to commits that never change*
* **Annotated** *tags*  (option `-a`) store additional information: a *message*, and, optionally, a *signature* (option `-s`/`-u`)

```mermaid
flowchart RL
  HEAD{{"HEAD"}}
  master(master)

  C10([10]) --> C9([9]) --> C8([8]) --> C7([7]) --> C6([6]) --> C5([5]) --> C4([4]) --> C3([3]) --> C2([2]) --> C1([1])

  master -.-> C10

  HEAD -.-> C10
  HEAD --"fas:fa-link"--o master

  class HEAD head;
  class master branch;
  class C1,C2,C3,C4,C5,C6,C7,C8,C9,C10,C11,C12,C13 commit;
```

⬇️ `git checkout C4 && git tag 1.2.3` ⬇️

```mermaid
flowchart RL
  HEAD{{"HEAD fas:fa-unlink"}}

  master(master)
  tag1>1.2.3]

  C10([10]) --> C9([9]) --> C8([8]) --> C7([7]) --> C6([6]) --> C5([5]) --> C4([4]) --> C3([3]) --> C2([2]) --> C1([1])

  master -.-> C10

  HEAD -.-> C4
  tag1 --o C4

  class HEAD head;
  class master branch;
  class tag1 tag;
  class C1,C2,C3,C4,C5,C6,C7,C8,C9,C10,C11,C12,C13 commit;
```

`HEAD` cannot attach to tags!

---

## Branches as labels: deletion

As we discussed, *branches* work like *special labels* that **move** if a commit is performed when `HEAD` is **attached**.

Also, the *history* tracked by git is a *directed acyclic graph* (each commit has a reference to its parents)

$\Rightarrow$ *Branches can be removed without information loss*, as far as there is at least *another branch* from which *all the commits* of the deleted branch are *reachable*

*Safe* branch deletion is performed with `git branch -d branch-name` (fails if there is information loss).

---

## Branch deletion example

```mermaid
flowchart RL
  HEAD{{"HEAD"}}
  master(master)
  bug22(fix/bug22)
  serverless(feat/serverless)

  C10([10]) --> C9([9]) --> C8([8]) --> C7([7]) --> C6([6]) --> C5([5]) --> C4([4]) --> C3([3]) --> C2([2]) --> C1([1])
  C11([11]) --> C7

  master -.-> C10
  bug22 -.-> C3
  serverless -.-> C11

  HEAD -.-> C10
  HEAD --"fas:fa-link"--o master

  class HEAD head;
  class master,bug22,serverless branch;
  class C1,C2,C3,C4,C5,C6,C7,C8,C9,C10,C11,C12,C13 commit;
```

{{% fragment %}}
⬇️ `git branch -d fix/bug22` ⬇️
{{% /fragment %}}

{{% fragment %}}
```mermaid
flowchart RL
  HEAD{{"HEAD"}}
  master(master)
  serverless(feat/serverless)

  C10([10]) --> C9([9]) --> C8([8]) --> C7([7]) --> C6([6]) --> C5([5]) --> C4([4]) --> C3([3]) --> C2([2]) --> C1([1])
  C11([11]) --> C7

  master -.-> C10
  serverless -.-> C11

  HEAD -.-> C10
  HEAD --"fas:fa-link"--o master

  class HEAD head;
  class master,bug22,serverless branch;
  class C1,C2,C3,C4,C5,C6,C7,C8,C9,C10,C11,C12,C13 commit;
```
{{% /fragment %}}

{{% fragment %}}
No commit is lost, branch `fix/bug22` is removed
* `git branch -d feat/serverless` would **fail** with an error message, as `C7` would be lost
{{% /fragment %}}

---

## Importing a repository

* We can initialize an **emtpy** repository with `git init`
* But most of the time we want to start from a *local copy* of an **existing** repository

Git provides a `clone` subcommand that copies *the whole history* of a repository locally
* `git clone URI destination` creates the folder `destination` and clones the repository found at `URI`
  * If `destination` is not empty, fails
  * if `destination` is omitted, a folder with the same namen of the last segment of `URI` is created
  * `URI` can be remote or local, Git supports the `file://`, `https://`, and `ssh` protocols
      * `ssh` *recommended* when available
* The `clone` subcommand checks out the remote branch where the `HEAD` is attached (*default branch*)

Examples:
```bash
# creates a local folder called `destination` and copies the repository from the local directory
git clone /some/repository/on/my/file/system destination
# creates a local folder called `myfolder` and copies the repository located at the specified `URL`
git clone https://somewebsite.com/someRepository.git myfolder
# creates a local folder called `SomeRepo` and copies the repository located at the specified `URL`
git clone user@sshserver.com:SomePath/SomeRepo.git
```

---

## Remotes

* Remotes are the *known copies* of the repository that exist somewhere (usually in the Internet)
* Each remote has a *name* and a *URI*
* When a repository is created via `init`, no remote is known.
* When a repository is imported via `clone`, a remote called `origin` is created automatically

*Non-local branches can be referenced* as `remoteName/branchName`

The `remote` subcommand is used to inspect and manage remotes:
* `git remote -v` *lists* the known remotes    C3 -> "fix/bug22";

* `git remote add a-remote URI` *adds* a new remote named `a-remote` and pointing to `URI`
* `git remote show a-remote` displays *extended information* on `a-remote`
* `git remote remove a-remote` *removes* `a-remote` (it does not delete information on the remote, it *locally* forgets that it exits)

---

## Upstream branches

Remote branches can be *associated* with local branches, with the intended meaning that the local and the remote branch are *intended to be two copies of the same branch*

* A remote branch associated to a local branch is its **upstream branch**
* upstream branches can be configured via `git branch --set-upstream-to=remote/branchName`
  * e.g.: `git branch --set-upstream-to=origin/develop` sets the current branch upstream to `origin/develop`
* When a repository is initialize by `clone`, its default branch is checked out locally with the same name it has on the remote, and the remote branch is automatically set as *upstream*

---

### Actual result of `git clone git@somesite.com/repo.git`

```mermaid
flowchart RL

subgraph somesite.com/repo.git
  direction RL
  HEAD{{"HEAD"}}
  master(master)
  serverless(feat/serverless)

  C10([10]) --> C9([9]) --> C8([8]) --> C7([7]) --> C6([6]) --> C5([5]) --> C4([4]) --> C3([3]) --> C2([2]) --> C1([1])
  C12([12]) --> C11([11]) --> C7

  master -.-> C10
  serverless -.-> C12

  HEAD -.-> C10
  HEAD --"fas:fa-link"--o master
end

subgraph local
  direction RL
  origin[(origin)]

  HEADL{{"HEAD"}}
  masterl(master)

  CL10([10]) --> CL9([9]) --> CL8([8]) --> CL7([7]) --> CL6([6]) --> CL5([5]) --> CL4([4]) --> CL3([3]) --> CL2([2]) --> CL1([1])

  masterl -.-> CL10
  masterl ==o master

  HEADL -.-> CL10
  HEADL --"fas:fa-link"--o masterl
end

origin ==o somesite.com/repo.git

class local,somesite.com/repo.git repo;
class origin remote;
class HEAD,HEADL head;
class master,masterl,bug22,serverless branch;
class C1,C2,C3,C4,C5,C6,C7,C8,C9,C10,C11,C12,C13,CL1,CL2,CL3,CL4,CL5,CL6,CL7,CL8,CL9,CL10,CL11,CL12,CL13 commit;
```

* `git@somesite.com/repo.git` is saved as `origin`
* The main branch (the branch where `HEAD` is attached, in our case `master`) on `origin` gets checked out locally with the same name
* The local branch `master` is set up to track `origin/master` as upstream
* Additional branches are *fetched* (they are known locally), but they are not checked out

---

## Importing remote branches

`git branch` (or `git checkout -b`) can checkout remote branches locally *once they have been fetched*.

```mermaid
flowchart RL

subgraph somesite.com/repo.git
  direction RL
  HEAD{{"HEAD"}}
  master(master)
  serverless(feat/serverless)

  C10([10]) --> C9([9]) --> C8([8]) --> C7([7]) --> C6([6]) --> C5([5]) --> C4([4]) --> C3([3]) --> C2([2]) --> C1([1])
  C12([12]) --> C11([11]) --> C7

  master -.-> C10
  serverless -.-> C12

  HEAD -.-> C10
  HEAD --"fas:fa-link"--o master
end

subgraph local
  direction RL
  origin[(origin)]

  masterl(master)

  HEADL{{"HEAD"}}

  CL10([10]) --> CL9([9]) --> CL8([8]) --> CL7([7]) --> CL6([6]) --> CL5([5]) --> CL4([4]) --> CL3([3]) --> CL2([2]) --> CL1([1])

  masterl -.-> CL10
  masterl ==o master

  HEADL -.-> CL10
  HEADL --"fas:fa-link"--o masterl
end

origin ==o somesite.com/repo.git

class local,somesite.com/repo.git repo;
class origin remote;
class HEAD,HEADL head;
class master,masterl,bug22,serverless branch;
class C1,C2,C3,C4,C5,C6,C7,C8,C9,C10,C11,C12,C13,CL1,CL2,CL3,CL4,CL5,CL6,CL7,CL8,CL9,CL10,CL11,CL12,CL13 commit;
```

➡️ `git checkout -b imported-feat origin/feat/serverless` ➡️

---

⬇️ `git checkout -b imported-feat origin/feat/serverless` ⬇️


```mermaid
flowchart RL

subgraph somesite.com/repo.git
  direction RL
  HEAD{{"HEAD"}}
  master(master)
  serverless(feat/serverless)

  C10([10]) --> C9([9]) --> C8([8]) --> C7([7]) --> C6([6]) --> C5([5]) --> C4([4]) --> C3([3]) --> C2([2]) --> C1([1])
  C12([12]) --> C11([11]) --> C7

  master -.-> C10
  serverless -.-> C12

  HEAD -.-> C10
  HEAD --"fas:fa-link"--o master
end

subgraph local
  direction RL
  origin[(origin)]

  masterl(master)
  imported(imported-feat)
  HEADL{{"HEAD"}}

  CL12([12]) --> CL11([11]) --> CL7
  CL10([10]) --> CL9([9]) --> CL8([8]) --> CL7([7]) --> CL6([6]) --> CL5([5]) --> CL4([4]) --> CL3([3]) --> CL2([2]) --> CL1([1])

  masterl -.-> CL10
  masterl ==o master
  imported -.-> CL12
  imported ==o serverless

  HEADL -.-> CL12
  HEADL --"fas:fa-link"--o imported
end

origin ==o somesite.com/repo.git

class local,somesite.com/repo.git repo;
class origin remote;
class HEAD,HEADL head;
class master,masterl,bug22,serverless,imported branch;
class C1,C2,C3,C4,C5,C6,C7,C8,C9,C10,C11,C12,C13,CL1,CL2,CL3,CL4,CL5,CL6,CL7,CL8,CL9,CL10,CL11,CL12,CL13 commit;
```

* A new branch `imported-feat` is created locally, and `origin/feat/new-client` is set as its *upstream*

---

## Importing remote branches

* It is customary to reuse the upstream name if there are no conflicts
  * `git checkout -b feat/new-client origin/feat/new-client`
* Modern versions of Git automatically checkout remote branches if there are no ambiguities:
  * `git checkout feat/new-client`
  * creates a new branch `feat/new-client` with the upstream branch set to `origin/feat/new-client` if:
    * there is **no** *local branch* named `feat/new-client`
    * there is **no** *ambiguity* with remotes
  * Quicker if you are working with a single remote (pretty common)

---

## Example with multiple remotes

```mermaid
flowchart RL

subgraph somesite.com/repo.git
  direction RL
  HEAD{{"HEAD"}}
  master(master)
  serverless(feat/serverless)

  C4([4]) --> C3([3]) --> C2([2]) --> C1([1])
  C7([7]) --> C6([6]) --> C5([5]) --> C2 

  master -.-> C4
  serverless -.-> C7

  HEAD -.-> C4
  HEAD --"fas:fa-link"--o master
end

subgraph somewherelse.org/repo.git
  direction RL

  masterl(master)
  HEADL{{"HEAD"}}

  CL10([10]) --> CL9([9]) --> CL8([8]) --> CL4([4]) --> CL3([3]) --> CL2([2]) --> CL1([1])

  masterl -.-> CL10

  HEADL -.-> CL10
  HEADL --"fas:fa-link"--o masterl
end

class local,somesite.com/repo.git,somewherelse.org/repo.git repo;
class origin remote;
class HEAD,HEADL head;
class master,masterl,bug22,serverless,imported branch;
class C1,C2,C3,C4,C5,C6,C7,C8,C9,C10,C11,C12,C13,CL1,CL2,CL3,CL4,CL5,CL6,CL7,CL8,CL9,CL10,CL11,CL12,CL13 commit;
```

➡️ Next: `git clone git@somesite.com/repo.git` ➡️

---

⬇️ `git clone git@somesite.com/repo.git` ⬇️

```mermaid
flowchart RL

subgraph somesite.com/repo.git
  direction RL
  HEAD{{"HEAD"}}
  master(master)
  serverless(feat/serverless)

  C4([4]) --> C3([3]) --> C2([2]) --> C1([1])
  C7([7]) --> C6([6]) --> C5([5]) --> C2 

  master -.-> C4
  serverless -.-> C7

  HEAD -.-> C4
  HEAD --"fas:fa-link"--o master
end

subgraph somewherelse.org/repo.git
  direction RL

  masterl(master)
  HEADL{{"HEAD"}}

  CL10([10]) --> CL9([9]) --> CL8([8]) --> CL4([4]) --> CL3([3]) --> CL2([2]) --> CL1([1])

  masterl -.-> CL10

  HEADL -.-> CL10
  HEADL --"fas:fa-link"--o masterl
end

subgraph local
  direction RL

  origin[(origin)]

  HEADa{{"HEAD"}}
  mastera(master)

  C4a([4]) --> C3a([3]) --> C2a([2]) --> C1a([1])

  mastera -.-> C4a
  mastera ==o master

  HEADa -.-> C4a
  HEADa --"fas:fa-link"--o mastera
end

origin ==o somesite.com/repo.git

class local,somesite.com/repo.git,somewherelse.org/repo.git repo;
class origin remote;
class HEAD,HEADL,HEADa head;
class master,masterl,mastera,bug22,serverless,imported branch;
class C1,C2,C3,C4,C5,C6,C7,C8,C9,C10,C11,C12,C13,C1a,C2a,C3a,C4a,C5a,C6a,C7a,C8a,C9a,C10a,C11a,C12a,C13a,CL1,CL2,CL3,CL4,CL5,CL6,CL7,CL8,CL9,CL10,CL11,CL12,CL13 commit;
```

➡️ Next: `git checkout -b feat/serverless origin/feat/serverless` ➡️

---

⬇️ `git checkout -b feat/serverless origin/feat/serverless` ⬇️

```mermaid
flowchart RL

subgraph somesite.com/repo.git
  direction RL
  HEAD{{"HEAD"}}
  master(master)
  serverless(feat/serverless)

  C4([4]) --> C3([3]) --> C2([2]) --> C1([1])
  C7([7]) --> C6([6]) --> C5([5]) --> C2 

  master -.-> C4
  serverless -.-> C7

  HEAD -.-> C4
  HEAD --"fas:fa-link"--o master
end

subgraph somewherelse.org/repo.git
  direction RL

  masterl(master)
  HEADL{{"HEAD"}}

  CL10([10]) --> CL9([9]) --> CL8([8]) --> CL4([4]) --> CL3([3]) --> CL2([2]) --> CL1([1])

  masterl -.-> CL10

  HEADL -.-> CL10
  HEADL --"fas:fa-link"--o masterl
end

subgraph local
  direction RL

  origin[(origin)]

  HEADa{{"HEAD"}}
  mastera(master)
  serverlessa(feat/serverless)

  C4a([4]) --> C3a([3]) --> C2a([2]) --> C1a([1])
  C7a([7]) --> C6a([6]) --> C5a([5]) --> C2a 

  mastera -.-> C4a
  mastera ==o master

  serverlessa -.-> C7a
  serverlessa ==o serverless

  HEADa -.-> C7a
  HEADa --"fas:fa-link"--o serverlessa
end

origin ==o somesite.com/repo.git

class local,somesite.com/repo.git,somewherelse.org/repo.git repo;
class origin remote;
class HEAD,HEADL,HEADa head;
class master,masterl,mastera,bug22,serverless,serverlessa,imported branch;
class C1,C2,C3,C4,C5,C6,C7,C8,C9,C10,C11,C12,C13,C1a,C2a,C3a,C4a,C5a,C6a,C7a,C8a,C9a,C10a,C11a,C12a,C13a,CL1,CL2,CL3,CL4,CL5,CL6,CL7,CL8,CL9,CL10,CL11,CL12,CL13 commit;
```

➡️ Next: `git remote add other git@somewhereelse.org/repo.git` ➡️

---

⬇️ `git remote add other git@somewhereelse.org/repo.git` ⬇️

```mermaid
flowchart RL

subgraph somesite.com/repo.git
  direction RL
  HEAD{{"HEAD"}}
  master(master)
  serverless(feat/serverless)

  C4([4]) --> C3([3]) --> C2([2]) --> C1([1])
  C7([7]) --> C6([6]) --> C5([5]) --> C2 

  master -.-> C4
  serverless -.-> C7

  HEAD -.-> C4
  HEAD --"fas:fa-link"--o master
end

subgraph somewherelse.org/repo.git
  direction RL

  masterl(master)
  HEADL{{"HEAD"}}

  CL10([10]) --> CL9([9]) --> CL8([8]) --> CL4([4]) --> CL3([3]) --> CL2([2]) --> CL1([1])

  masterl -.-> CL10

  HEADL -.-> CL10
  HEADL --"fas:fa-link"--o masterl
end

subgraph local
  direction RL

  origin[(origin)]
  other[(other)]

  HEADa{{"HEAD"}}
  mastera(master)
  serverlessa(feat/serverless)

  C4a([4]) --> C3a([3]) --> C2a([2]) --> C1a([1])
  C7a([7]) --> C6a([6]) --> C5a([5]) --> C2a 

  mastera -.-> C4a
  mastera ==o master

  serverlessa -.-> C7a
  serverlessa ==o serverless

  HEADa -.-> C7a
  HEADa --"fas:fa-link"--o serverlessa
end

origin ==o somesite.com/repo.git
other ==o somewherelse.org/repo.git

class local,somesite.com/repo.git,somewherelse.org/repo.git repo;
class origin,other remote;
class HEAD,HEADL,HEADa head;
class master,masterl,mastera,bug22,serverless,serverlessa,imported branch;
class C1,C2,C3,C4,C5,C6,C7,C8,C9,C10,C11,C12,C13,C1a,C2a,C3a,C4a,C5a,C6a,C7a,C8a,C9a,C10a,C11a,C12a,C13a,CL1,CL2,CL3,CL4,CL5,CL6,CL7,CL8,CL9,CL10,CL11,CL12,CL13 commit;
```

➡️ Next: `git checkout -b other-master other/master` ➡️

---

⬇️ `git checkout -b other-master other/master` ⬇️

```mermaid
flowchart RL

subgraph somesite.com/repo.git
  direction RL
  HEAD{{"HEAD"}}
  master(master)
  serverless(feat/serverless)

  C4([4]) --> C3([3]) --> C2([2]) --> C1([1])
  C7([7]) --> C6([6]) --> C5([5]) --> C2 

  master -.-> C4
  serverless -.-> C7

  HEAD -.-> C4
  HEAD --"fas:fa-link"--o master
end

subgraph somewherelse.org/repo.git
  direction RL

  masterl(master)
  HEADL{{"HEAD"}}

  CL10([10]) --> CL9([9]) --> CL8([8]) --> CL4([4]) --> CL3([3]) --> CL2([2]) --> CL1([1])

  masterl -.-> CL10

  HEADL -.-> CL10
  HEADL --"fas:fa-link"--o masterl
end

subgraph local
  direction RL

  HEADa{{"HEAD"}}
  mastera(master)
  serverlessa(feat/serverless)
  othermaster(other-master)

  C10a([10]) --> C9a([9]) --> C8a([8]) --> C4a([4]) --> C3a([3]) --> C2a([2]) --> C1a([1])
  C7a([7]) --> C6a([6]) --> C5a([5]) --> C2a 

  mastera -.-> C4a
  mastera ==o master

  serverlessa -.-> C7a
  serverlessa ==o serverless

  othermaster -.-> C10a
  othermaster ==o masterl

  HEADa -.-> C10a
  HEADa --"fas:fa-link"--o othermaster

  origin[(origin)]
  other[(other)]
end

origin ==o somesite.com/repo.git
other ==o somewherelse.org/repo.git

class local,somesite.com/repo.git,somewherelse.org/repo.git repo;
class origin,other remote;
class HEAD,HEADL,HEADa head;
class master,masterl,mastera,bug22,serverless,serverlessa,imported,othermaster branch;
class C1,C2,C3,C4,C5,C6,C7,C8,C9,C10,C11,C12,C13,C1a,C2a,C3a,C4a,C5a,C6a,C7a,C8a,C9a,C10a,C11a,C12a,C13a,CL1,CL2,CL3,CL4,CL5,CL6,CL7,CL8,CL9,CL10,CL11,CL12,CL13 commit;
```

You can operate with *multiple remotes*! Just remember: *branch names* must be *unique* for every repository
  * If you want to track `origin/master` and `anotherRemote/master`, you need to branches with diverse names

---

## Fetching updates

To check if a *remote* has any *update* available, git provides th `git fetch` subcommand.
* `git fetch a-remote` checks if `a-remote` has any new information. If so, it downloads it.
  * **Note**: *it does **not** merge* it anywhere, it just memorizes if there is any change
* `git fetch` without a remote specified
  * if `HEAD` is *attached* and the *current branch* has an *upstream*, then the *remote* that is hosting the *upstream branch* is fetched
  * otherwise, `origin` is fetched, if present
* To apply the updates, is then necessary to use `merge`

The new *information fetched* includes new *commits*, *branches*, and *tags*.

---

## Fetch + merge example

{{< gravizo width=100 >}}
digraph G {
  fontname="Helvetica,Arial,sans-serif"
  fontsize="20"
  node [fontname="Helvetica,Arial,sans-serif"]
  edge [fontname="Helvetica,Arial,sans-serif"]
  rankdir=LR;
  compound=true
  subgraph cluster_remote {
    color=black
    label="somesite.com/repo.git"
    # Commits
    C0r [label=C0]
    C1r [label=C1]
    C2r [label=C2]
    C3r [label=C3]
    C4r [label=C4]
    C0r -> C1r -> C2r -> C3r -> C4r [dir=back]
    # Branches
    node [style="filled,solid", shape=box, fillcolor=orange];
    edge [dir=back, penwidth=4, color=orange]
    master_r [label=master]
    C4r -> master_r
    # Head
    HEAD_r [label=HEAD]
    C4r -> HEAD_r
    edge [dir=forward, arrowhead=tee, penwidth=2, color=red]
    HEAD_r -> master_r [arrowhead=tee, penwidth=2, color=red, label="attached"]
  }
  subgraph cluster_local {
    label="Local"
    color=black
    # Commits
    C0 -> C1 -> C2 -> C3 -> C4 [dir=back]
    # Branches
    node [style="filled,solid", shape=box, fillcolor=orange]
    edge [dir=back, penwidth=4, color=orange]
    C4 -> master
    # Head
    C4 -> HEAD
    edge [dir=forward, arrowhead=tee, penwidth=2, color=red]
    HEAD -> "master" [label="attached"]
    # Upstreams
    edge [arrowhead=dot, dir=forward, penwidth=2, color=blue]
    master -> master_r [label="upstream"]
    # Remotes
    node [style="filled,solid", shape=box, fillcolor=aquamarine3]
    edge [arrowhead=dot, dir=forward, penwidth=3, color=aquamarine3]
    origin -> C0r [lhead=cluster_remote]
  }
}
{{< /gravizo >}}

⬇️ Changes happen on `somesite.com/repo.git` and on our repository concurrently ⬇️

{{< gravizo width=100 >}}
digraph G {
  fontname="Helvetica,Arial,sans-serif"
  fontsize="20"
  node [fontname="Helvetica,Arial,sans-serif"]
  edge [fontname="Helvetica,Arial,sans-serif"]
  rankdir=LR;
  compound=true
  subgraph cluster_remote {
    color=black
    label="somesite.com/repo.git"
    # Commits
    C0r [label=C0]
    C1r [label=C1]
    C2r [label=C2]
    C3r [label=C3]
    C4r [label=C4]
    C5r [label=C5]
    C6r [label=C6]
    C0r -> C1r -> C2r -> C3r -> C4r -> C5r -> C6r [dir=back]
    # Branches
    node [style="filled,solid", shape=box, fillcolor=orange]
    edge [dir=back, penwidth=4, color=orange]
    master_r [label=master]
    C6r -> master_r
    # Head
    HEAD_r [label=HEAD]
    C6r -> HEAD_r
    edge [dir=forward, arrowhead=tee, penwidth=2, color=red]
    HEAD_r -> master_r [arrowhead=tee, penwidth=2, color=red, label="attached"]
  }
  subgraph cluster_local {
    label="Local"
    color=black
    # Commits
    C0 -> C1 -> C2 -> C3 -> C4 -> C7 [dir=back]
    # Branches
    node [style="filled,solid", shape=box, fillcolor=orange]
    edge [dir=back, penwidth=4, color=orange]
    C7 -> master
    # Head
    C7 -> HEAD
    edge [dir=forward, arrowhead=tee, penwidth=2, color=red]
    HEAD -> "master" [label="attached"]
    # Upstreams
    edge [arrowhead=dot, dir=forward, penwidth=2, color=blue]
    master -> master_r [label="upstream"]
    # Remotes
    node [style="filled,solid", shape=box, fillcolor=aquamarine3]
    edge [arrowhead=dot, dir=forward, penwidth=3, color=aquamarine3]
    origin -> C0r [lhead=cluster_remote]
  }
}
{{< /gravizo >}}

---

## Fetch + merge example

{{< gravizo width=100 >}}
digraph G {
  fontname="Helvetica,Arial,sans-serif"
  fontsize="20"
  node [fontname="Helvetica,Arial,sans-serif"]
  edge [fontname="Helvetica,Arial,sans-serif"]
  rankdir=LR;
  compound=true
  subgraph cluster_remote {
    color=black
    label="somesite.com/repo.git"
    # Commits
    C0r [label=C0]
    C1r [label=C1]
    C2r [label=C2]
    C3r [label=C3]
    C4r [label=C4]
    C5r [label=C5]
    C6r [label=C6]
    C0r -> C1r -> C2r -> C3r -> C4r -> C5r -> C6r [dir=back]
    # Branches
    node [style="filled,solid", shape=box, fillcolor=orange]
    edge [dir=back, penwidth=4, color=orange]
    master_r [label=master]
    C6r -> master_r
    # Head
    HEAD_r [label=HEAD]
    C6r -> HEAD_r
    edge [dir=forward, arrowhead=tee, penwidth=2, color=red]
    HEAD_r -> master_r [arrowhead=tee, penwidth=2, color=red, label="attached"]
  }
  subgraph cluster_local {
    label="Local"
    color=black
    # Commits
    C0 -> C1 -> C2 -> C3 -> C4 -> C7 [dir=back]
    # Branches
    node [style="filled,solid", shape=box, fillcolor=orange]
    edge [dir=back, penwidth=4, color=orange]
    C7 -> master
    # Head
    C7 -> HEAD
    edge [dir=forward, arrowhead=tee, penwidth=2, color=red]
    HEAD -> "master" [label="attached"]
    # Upstreams
    edge [arrowhead=dot, dir=forward, penwidth=2, color=blue]
    master -> master_r [label="upstream"]
    # Remotes
    node [style="filled,solid", shape=box, fillcolor=aquamarine3]
    edge [arrowhead=dot, dir=forward, penwidth=3, color=aquamarine3]
    origin -> C0r [lhead=cluster_remote]
  }
}
{{< /gravizo >}}

⬇️ `git fetch && git merge origin/master` (assuming no conflicts or conflicts resolved) ⬇️

{{< gravizo width=100 >}}
digraph G {
  fontname="Helvetica,Arial,sans-serif"
  fontsize="20"
  node [fontname="Helvetica,Arial,sans-serif"]
  edge [fontname="Helvetica,Arial,sans-serif"]
  rankdir=LR;
  compound=true
  subgraph cluster_remote {
    color=black
    label="somesite.com/repo.git"
    # Commits
    C0r [label=C0]
    C1r [label=C1]
    C2r [label=C2]
    C3r [label=C3]
    C4r [label=C4]
    C5r [label=C5]
    C6r [label=C6]
    C0r -> C1r -> C2r -> C3r -> C4r -> C5r -> C6r [dir=back]
    # Branches
    node [style="filled,solid", shape=box, fillcolor=orange]
    edge [dir=back, penwidth=4, color=orange]
    master_r [label=master]
    C6r -> master_r
    # Head
    HEAD_r [label=HEAD]
    C6r -> HEAD_r
    edge [dir=forward, arrowhead=tee, penwidth=2, color=red]
    HEAD_r -> master_r [arrowhead=tee, penwidth=2, color=red, label="attached"]
  }
  subgraph cluster_local {
    label="Local"
    color=black
    # Commits
    C0 -> C1 -> C2 -> C3 -> C4 -> C5 -> C6 -> C8 [dir=back]
    C4 -> C7 -> C8 [dir=back]
    # Branches
    node [style="filled,solid", shape=box, fillcolor=orange]
    edge [dir=back, penwidth=4, color=orange]
    C8 -> master
    # Head
    C8 -> HEAD
    edge [dir=forward, arrowhead=tee, penwidth=2, color=red]
    HEAD -> "master" [label="attached"]
    # Upstreams
    edge [arrowhead=dot, dir=forward, penwidth=2, color=blue]
    master -> master_r [label="upstream"]
    # Remotes
    node [style="filled,solid", shape=box, fillcolor=aquamarine3]
    edge [arrowhead=dot, dir=forward, penwidth=3, color=aquamarine3]
    origin -> C0r [lhead=cluster_remote]
  }
}
{{< /gravizo >}}

If there had been no updates locally, we would have experienced a *fast-forward*

---

## `git pull`

*Fetching* the remote with the upstream branch and then *merging* is *extremely common*,
so common that there is a special subcommand that operates.

`git pull` is equivalent to `git fetch && git merge FETCH_HEAD`
* `git pull remote` is the same as `git fetch remote && git merge FETCH_HEAD`
* `git pull remote branch` is the same as `git fetch remote && git merge remote/branch`

`git pull` is more commonly used than `git fetch` + `git merge`,
still, it is important to understand that *it is not a primitive operation*

---

## Sending local changes

Git provides a way to *send* changes to a remote: `git push remote branch`
* sends the current branch changes to `remote/branch`, and updates the remote `HEAD`
* if the branch or the remote is omitted, then the *upstream* branch is used
* `push` *requires writing rights to the remote repository*
* `push` *fails* if the pushed branch is not a *descendant* of the destination branch, which means:
  * the destination branch has *work that is not present* in the local branch
  * the destination branch *cannot be fast-forwarded* to the local branch
  * the commits on the destination branch *are not a subset* of the ones on the local branch

#### Pushing tags

By default, `git push` does not send *tags*
* `git push --tags` sends only the tags
* `git push --follow-tags` sends commits and then tags

---

## Example with git pull and git push

{{< gravizo width=90 >}}
digraph G {
  fontname="Helvetica,Arial,sans-serif"
  fontsize="20"
  node [fontname="Helvetica,Arial,sans-serif"]
  edge [fontname="Helvetica,Arial,sans-serif"]
  rankdir=LR;
  compound=true
  subgraph cluster_remote {
    color=black
    label="somesite.com/repo.git"
    # Commits
    C0r [label=C0]
    C1r [label=C1]
    C2r [label=C2]
    C3r [label=C3]
    C0r -> C1r -> C2r -> C3r [dir=back]
    # Branches
    node [style="filled,solid", shape=box, fillcolor=orange]
    edge [dir=back, penwidth=4, color=orange]
    master_r [label=master]
    C3r -> master_r
    # Head
    HEAD_r [label=HEAD]
    C3r -> HEAD_r
    edge [dir=forward, arrowhead=tee, penwidth=2, color=red]
    HEAD_r -> master_r [arrowhead=tee, penwidth=2, color=red, label="attached"]
  }
  subgraph cluster_local {
    label="Local"
    color=black
    # Commits
    C0 -> C1 -> C2 -> C3 [dir=back]
    # Branches
    node [style="filled,solid", shape=box, fillcolor=orange]
    edge [dir=back, penwidth=4, color=orange]
    C3 -> master
    # Head
    C3 -> HEAD
    edge [dir=forward, arrowhead=tee, penwidth=2, color=red]
    HEAD -> "master" [label="attached"]
    # Upstreams
    edge [arrowhead=dot, dir=forward, penwidth=2, color=blue]
    master -> master_r [label="upstream"]
    # Remotes
    node [style="filled,solid", shape=box, fillcolor=aquamarine3]
    edge [arrowhead=dot, dir=forward, penwidth=3, color=aquamarine3]
    origin -> C0r [lhead=cluster_remote]
  }
}
{{< /gravizo >}}

⬇️ [some changes] `git add . && git commit` ⬇️

{{< gravizo width=90 >}}
digraph G {
  fontname="Helvetica,Arial,sans-serif"
  fontsize="20"
  node [fontname="Helvetica,Arial,sans-serif"]
  edge [fontname="Helvetica,Arial,sans-serif"]
  rankdir=LR;
  compound=true
  subgraph cluster_remote {
    color=black
    label="somesite.com/repo.git"
    # Commits
    C0r [label=C0]
    C1r [label=C1]
    C2r [label=C2]
    C3r [label=C3]
    C0r -> C1r -> C2r -> C3r [dir=back]
    # Branches
    node [style="filled,solid", shape=box, fillcolor=orange]
    edge [dir=back, penwidth=4, color=orange]
    master_r [label=master]
    C3r -> master_r
    # Head
    HEAD_r [label=HEAD]
    C3r -> HEAD_r
    edge [dir=forward, arrowhead=tee, penwidth=2, color=red]
    HEAD_r -> master_r [arrowhead=tee, penwidth=2, color=red, label="attached"]
  }
  subgraph cluster_local {
    label="Local"
    color=black
    # Commits
    C0 -> C1 -> C2 -> C3 -> C4 [dir=back]
    # Branches
    node [style="filled,solid", shape=box, fillcolor=orange]
    edge [dir=back, penwidth=4, color=orange]
    C4 -> master
    # Head
    C4 -> HEAD
    edge [dir=forward, arrowhead=tee, penwidth=2, color=red]
    HEAD -> "master" [label="attached"]
    # Upstreams
    edge [arrowhead=dot, dir=forward, penwidth=2, color=blue]
    master -> master_r [label="upstream"]
    # Remotes
    node [style="filled,solid", shape=box, fillcolor=aquamarine3]
    edge [arrowhead=dot, dir=forward, penwidth=3, color=aquamarine3]
    origin -> C0r [lhead=cluster_remote]
  }
}
{{< /gravizo >}}

---

## Example with git pull and git push

{{< gravizo width=85 >}}
digraph G {
  fontname="Helvetica,Arial,sans-serif"
  fontsize="20"
  node [fontname="Helvetica,Arial,sans-serif"]
  edge [fontname="Helvetica,Arial,sans-serif"]
  rankdir=LR;
  compound=true
  subgraph cluster_remote {
    color=black
    label="somesite.com/repo.git"
    # Commits
    C0r [label=C0]
    C1r [label=C1]
    C2r [label=C2]
    C3r [label=C3]
    C0r -> C1r -> C2r -> C3r [dir=back]
    # Branches
    node [style="filled,solid", shape=box, fillcolor=orange]
    edge [dir=back, penwidth=4, color=orange]
    master_r [label=master]
    C3r -> master_r
    # Head
    HEAD_r [label=HEAD]
    C3r -> HEAD_r
    edge [dir=forward, arrowhead=tee, penwidth=2, color=red]
    HEAD_r -> master_r [arrowhead=tee, penwidth=2, color=red, label="attached"]
  }
  subgraph cluster_local {
    label="Local"
    color=black
    # Commits
    C0 -> C1 -> C2 -> C3 -> C4 [dir=back]
    # Branches
    node [style="filled,solid", shape=box, fillcolor=orange]
    edge [dir=back, penwidth=4, color=orange]
    C4 -> master
    # Head
    C4 -> HEAD
    edge [dir=forward, arrowhead=tee, penwidth=2, color=red]
    HEAD -> "master" [label="attached"]
    # Upstreams
    edge [arrowhead=dot, dir=forward, penwidth=2, color=blue]
    master -> master_r [label="upstream"]
    # Remotes
    node [style="filled,solid", shape=box, fillcolor=aquamarine3]
    edge [arrowhead=dot, dir=forward, penwidth=3, color=aquamarine3]
    origin -> C0r [lhead=cluster_remote]
  }
}
{{< /gravizo >}}

⬇️ [some changes] `git push` ⬇️

{{< gravizo width=85 >}}
digraph G {
  fontname="Helvetica,Arial,sans-serif"
  fontsize="20"
  node [fontname="Helvetica,Arial,sans-serif"]
  edge [fontname="Helvetica,Arial,sans-serif"]
  rankdir=LR;
  compound=true
  subgraph cluster_remote {
    color=black
    label="somesite.com/repo.git"
    # Commits
    C0r [label=C0]
    C1r [label=C1]
    C2r [label=C2]
    C3r [label=C3]
    C4r [label=C4]
    C0r -> C1r -> C2r -> C3r -> C4r [dir=back]
    # Branches
    node [style="filled,solid", shape=box, fillcolor=orange]
    edge [dir=back, penwidth=4, color=orange]
    master_r [label=master]
    C4r -> master_r
    # Head
    HEAD_r [label=HEAD]
    C4r -> HEAD_r
    edge [dir=forward, arrowhead=tee, penwidth=2, color=red]
    HEAD_r -> master_r [arrowhead=tee, penwidth=2, color=red, label="attached"]
  }
  subgraph cluster_local {
    label="Local"
    color=black
    # Commits
    C0 -> C1 -> C2 -> C3 -> C4 [dir=back]
    # Branches
    node [style="filled,solid", shape=box, fillcolor=orange]
    edge [dir=back, penwidth=4, color=orange]
    C4 -> master
    # Head
    C4 -> HEAD
    edge [dir=forward, arrowhead=tee, penwidth=2, color=red]
    HEAD -> "master" [label="attached"]
    # Upstreams
    edge [arrowhead=dot, dir=forward, penwidth=2, color=blue]
    master -> master_r [label="upstream"]
    # Remotes
    node [style="filled,solid", shape=box, fillcolor=aquamarine3]
    edge [arrowhead=dot, dir=forward, penwidth=3, color=aquamarine3]
    origin -> C0r [lhead=cluster_remote]
  }
}
{{< /gravizo >}}

* Everything okay! `origin/master` is a *subset* of `master`
* The remote `HEAD` can be *fast-forwarded*

---

## Example with git pull and git push

{{< gravizo width=90 >}}
digraph G {
  fontname="Helvetica,Arial,sans-serif"
  fontsize="20"
  node [fontname="Helvetica,Arial,sans-serif"]
  edge [fontname="Helvetica,Arial,sans-serif"]
  rankdir=LR;
  compound=true
  subgraph cluster_remote {
    color=black
    label="somesite.com/repo.git"
    # Commits
    C0r [label=C0]
    C1r [label=C1]
    C2r [label=C2]
    C3r [label=C3]
    C4r [label=C4]
    C0r -> C1r -> C2r -> C3r -> C4r [dir=back]
    # Branches
    node [style="filled,solid", shape=box, fillcolor=orange]
    edge [dir=back, penwidth=4, color=orange]
    master_r [label=master]
    C4r -> master_r
    # Head
    HEAD_r [label=HEAD]
    C4r -> HEAD_r
    edge [dir=forward, arrowhead=tee, penwidth=2, color=red]
    HEAD_r -> master_r [arrowhead=tee, penwidth=2, color=red, label="attached"]
  }
  subgraph cluster_local {
    label="Local"
    color=black
    # Commits
    C0 -> C1 -> C2 -> C3 -> C4 [dir=back]
    # Branches
    node [style="filled,solid", shape=box, fillcolor=orange]
    edge [dir=back, penwidth=4, color=orange]
    C4 -> master
    # Head
    C4 -> HEAD
    edge [dir=forward, arrowhead=tee, penwidth=2, color=red]
    HEAD -> "master" [label="attached"]
    # Upstreams
    edge [arrowhead=dot, dir=forward, penwidth=2, color=blue]
    master -> master_r [label="upstream"]
    # Remotes
    node [style="filled,solid", shape=box, fillcolor=aquamarine3]
    edge [arrowhead=dot, dir=forward, penwidth=3, color=aquamarine3]
    origin -> C0r [lhead=cluster_remote]
  }
}
{{< /gravizo >}}

⬇️ [someone else pushes a change, local change] `git add . && git commit` ⬇️

{{< gravizo width=90 >}}
digraph G {
  fontname="Helvetica,Arial,sans-serif"
  fontsize="20"
  node [fontname="Helvetica,Arial,sans-serif"]
  edge [fontname="Helvetica,Arial,sans-serif"]
  rankdir=LR;
  compound=true
  subgraph cluster_remote {
    color=black
    label="somesite.com/repo.git"
    # Commits
    C0r [label=C0]
    C1r [label=C1]
    C2r [label=C2]
    C3r [label=C3]
    C4r [label=C4]
    C5r [label=C5]
    C0r -> C1r -> C2r -> C3r -> C4r -> C5r [dir=back]
    # Branches
    node [style="filled,solid", shape=box, fillcolor=orange]
    edge [dir=back, penwidth=4, color=orange]
    master_r [label=master]
    C5r -> master_r
    # Head
    HEAD_r [label=HEAD]
    C5r -> HEAD_r
    edge [dir=forward, arrowhead=tee, penwidth=2, color=red]
    HEAD_r -> master_r [arrowhead=tee, penwidth=2, color=red, label="attached"]
  }
  subgraph cluster_local {
    label="Local"
    color=black
    # Commits
    C0 -> C1 -> C2 -> C3 -> C4 -> C6 [dir=back]
    # Branches
    node [style="filled,solid", shape=box, fillcolor=orange]
    edge [dir=back, penwidth=4, color=orange]
    C6 -> master
    # Head
    C6 -> HEAD
    edge [dir=forward, arrowhead=tee, penwidth=2, color=red]
    HEAD -> "master" [label="attached"]
    # Upstreams
    edge [arrowhead=dot, dir=forward, penwidth=2, color=blue]
    master -> master_r [label="upstream"]
    # Remotes
    node [style="filled,solid", shape=box, fillcolor=aquamarine3]
    edge [arrowhead=dot, dir=forward, penwidth=3, color=aquamarine3]
    origin -> C0r [lhead=cluster_remote]
  }
}
{{< /gravizo >}}

---

## Example with git pull and git push

{{< gravizo width=90 >}}
digraph G {
  fontname="Helvetica,Arial,sans-serif"
  fontsize="20"
  node [fontname="Helvetica,Arial,sans-serif"]
  edge [fontname="Helvetica,Arial,sans-serif"]
  rankdir=LR;
  compound=true
  subgraph cluster_remote {
    color=black
    label="somesite.com/repo.git"
    # Commits
    C0r [label=C0]
    C1r [label=C1]
    C2r [label=C2]
    C3r [label=C3]
    C4r [label=C4]
    C5r [label=C5]
    C0r -> C1r -> C2r -> C3r -> C4r -> C5r [dir=back]
    # Branches
    node [style="filled,solid", shape=box, fillcolor=orange]
    edge [dir=back, penwidth=4, color=orange]
    master_r [label=master]
    C5r -> master_r
    # Head
    HEAD_r [label=HEAD]
    C5r -> HEAD_r
    edge [dir=forward, arrowhead=tee, penwidth=2, color=red]
    HEAD_r -> master_r [arrowhead=tee, penwidth=2, color=red, label="attached"]
  }
  subgraph cluster_local {
    label="Local"
    color=black
    # Commits
    C0 -> C1 -> C2 -> C3 -> C4 -> C6 [dir=back]
    # Branches
    node [style="filled,solid", shape=box, fillcolor=orange]
    edge [dir=back, penwidth=4, color=orange]
    C6 -> master
    # Head
    C6 -> HEAD
    edge [dir=forward, arrowhead=tee, penwidth=2, color=red]
    HEAD -> "master" [label="attached"]
    # Upstreams
    edge [arrowhead=dot, dir=forward, penwidth=2, color=blue]
    master -> master_r [label="upstream"]
    # Remotes
    node [style="filled,solid", shape=box, fillcolor=aquamarine3]
    edge [arrowhead=dot, dir=forward, penwidth=3, color=aquamarine3]
    origin -> C0r [lhead=cluster_remote]
  }
}
{{< /gravizo >}}

⬇️ `git push` ⬇️

**ERROR**

```text
To somesite.com/repo.git
 ! [rejected]        master -> master (fetch first)
error: failed to push some refs to 'somesite.com/repo.git'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

* `master` is not a *superset* of `origin/master`: commit `C5` prevents us from pushing
* How to solve?
  * (Git's error explains it pretty well)

---

## Example with git pull and git push

{{< gravizo width=90 >}}
digraph G {
  fontname="Helvetica,Arial,sans-serif"
  fontsize="20"
  node [fontname="Helvetica,Arial,sans-serif"]
  edge [fontname="Helvetica,Arial,sans-serif"]
  rankdir=LR;
  compound=true
  subgraph cluster_remote {
    color=black
    label="somesite.com/repo.git"
    # Commits
    C0r [label=C0]
    C1r [label=C1]
    C2r [label=C2]
    C3r [label=C3]
    C4r [label=C4]
    C5r [label=C5]
    C0r -> C1r -> C2r -> C3r -> C4r -> C5r [dir=back]
    # Branches
    node [style="filled,solid", shape=box, fillcolor=orange]
    edge [dir=back, penwidth=4, color=orange]
    master_r [label=master]
    C5r -> master_r
    # Head
    HEAD_r [label=HEAD]
    C5r -> HEAD_r
    edge [dir=forward, arrowhead=tee, penwidth=2, color=red]
    HEAD_r -> master_r [arrowhead=tee, penwidth=2, color=red, label="attached"]
  }
  subgraph cluster_local {
    label="Local"
    color=black
    # Commits
    C0 -> C1 -> C2 -> C3 -> C4 -> C6 [dir=back]
    # Branches
    node [style="filled,solid", shape=box, fillcolor=orange]
    edge [dir=back, penwidth=4, color=orange]
    C6 -> master
    # Head
    C6 -> HEAD
    edge [dir=forward, arrowhead=tee, penwidth=2, color=red]
    HEAD -> "master" [label="attached"]
    # Upstreams
    edge [arrowhead=dot, dir=forward, penwidth=2, color=blue]
    master -> master_r [label="upstream"]
    # Remotes
    node [style="filled,solid", shape=box, fillcolor=aquamarine3]
    edge [arrowhead=dot, dir=forward, penwidth=3, color=aquamarine3]
    origin -> C0r [lhead=cluster_remote]
  }
}
{{< /gravizo >}}

⬇️ `git pull` (assuming no merge conflicts, or after conflict resolution) ⬇️

{{< gravizo width=90 >}}
digraph G {
  fontname="Helvetica,Arial,sans-serif"
  fontsize="20"
  node [fontname="Helvetica,Arial,sans-serif"]
  edge [fontname="Helvetica,Arial,sans-serif"]
  rankdir=LR;
  compound=true
  subgraph cluster_remote {
    color=black
    label="somesite.com/repo.git"
    # Commits
    C0r [label=C0]
    C1r [label=C1]
    C2r [label=C2]
    C3r [label=C3]
    C4r [label=C4]
    C5r [label=C5]
    C0r -> C1r -> C2r -> C3r -> C4r -> C5r [dir=back]
    # Branches
    node [style="filled,solid", shape=box, fillcolor=orange]
    edge [dir=back, penwidth=4, color=orange]
    master_r [label=master]
    C5r -> master_r
    # Head
    HEAD_r [label=HEAD]
    C5r -> HEAD_r
    edge [dir=forward, arrowhead=tee, penwidth=2, color=red]
    HEAD_r -> master_r [arrowhead=tee, penwidth=2, color=red, label="attached"]
  }
  subgraph cluster_local {
    label="Local"
    color=black
    # Commits
    C0 -> C1 -> C2 -> C3 -> C4 -> C6 -> C7[dir=back]
    C4 -> C5 -> C7
    # Branches
    node [style="filled,solid", shape=box, fillcolor=orange]
    edge [dir=back, penwidth=4, color=orange]
    C7 -> master
    # Head
    C7 -> HEAD
    edge [dir=forward, arrowhead=tee, penwidth=2, color=red]
    HEAD -> "master" [label="attached"]
    # Upstreams
    edge [arrowhead=dot, dir=forward, penwidth=2, color=blue]
    master -> master_r [label="upstream"]
    # Remotes
    node [style="filled,solid", shape=box, fillcolor=aquamarine3]
    edge [arrowhead=dot, dir=forward, penwidth=3, color=aquamarine3]
    origin -> C0r [lhead=cluster_remote]
  }
}
{{< /gravizo >}}

* Now `master` is a *superset* of `origin/master`! (all the commits in `origin/master`, plus `C6` and `C7`)

---

## Example with git pull and git push

{{< gravizo width=90 >}}
digraph G {
  fontname="Helvetica,Arial,sans-serif"
  fontsize="20"
  node [fontname="Helvetica,Arial,sans-serif"]
  edge [fontname="Helvetica,Arial,sans-serif"]
  rankdir=LR;
  compound=true
  subgraph cluster_remote {
    color=black
    label="somesite.com/repo.git"
    # Commits
    C0r [label=C0]
    C1r [label=C1]
    C2r [label=C2]
    C3r [label=C3]
    C4r [label=C4]
    C5r [label=C5]
    C0r -> C1r -> C2r -> C3r -> C4r -> C5r [dir=back]
    # Branches
    node [style="filled,solid", shape=box, fillcolor=orange]
    edge [dir=back, penwidth=4, color=orange]
    master_r [label=master]
    C5r -> master_r
    # Head
    HEAD_r [label=HEAD]
    C5r -> HEAD_r
    edge [dir=forward, arrowhead=tee, penwidth=2, color=red]
    HEAD_r -> master_r [arrowhead=tee, penwidth=2, color=red, label="attached"]
  }
  subgraph cluster_local {
    label="Local"
    color=black
    # Commits
    C0 -> C1 -> C2 -> C3 -> C4 -> C6 -> C7[dir=back]
    C4 -> C5 -> C7
    # Branches
    node [style="filled,solid", shape=box, fillcolor=orange]
    edge [dir=back, penwidth=4, color=orange]
    C7 -> master
    # Head
    C7 -> HEAD
    edge [dir=forward, arrowhead=tee, penwidth=2, color=red]
    HEAD -> "master" [label="attached"]
    # Upstreams
    edge [arrowhead=dot, dir=forward, penwidth=2, color=blue]
    master -> master_r [label="upstream"]
    # Remotes
    node [style="filled,solid", shape=box, fillcolor=aquamarine3]
    edge [arrowhead=dot, dir=forward, penwidth=3, color=aquamarine3]
    origin -> C0r [lhead=cluster_remote]
  }
}
{{< /gravizo >}}


⬇️ `git push` ⬇️

{{< gravizo width=90 >}}
digraph G {
  fontname="Helvetica,Arial,sans-serif"
  fontsize="20"
  node [fontname="Helvetica,Arial,sans-serif"]
  edge [fontname="Helvetica,Arial,sans-serif"]
  rankdir=LR;
  compound=true
  subgraph cluster_remote {
    color=black
    label="somesite.com/repo.git"
    # Commits
    C0r [label=C0]
    C1r [label=C1]
    C2r [label=C2]
    C3r [label=C3]
    C4r [label=C4]
    C5r [label=C5]
    C6r [label=C6]
    C7r [label=C7]
    C0r -> C1r -> C2r -> C3r -> C4r -> C5r -> C7r [dir=back]
    C4r -> C6r -> C7r  [dir=back]
    # Branches
    node [style="filled,solid", shape=box, fillcolor=orange]
    edge [dir=back, penwidth=4, color=orange]
    master_r [label=master]
    C7r -> master_r
    # Head
    HEAD_r [label=HEAD]
    C7r -> HEAD_r
    edge [dir=forward, arrowhead=tee, penwidth=2, color=red]
    HEAD_r -> master_r [arrowhead=tee, penwidth=2, color=red, label="attached"]
  }
  subgraph cluster_local {
    label="Local"
    color=black
    # Commits
    C0 -> C1 -> C2 -> C3 -> C4 -> C6 -> C7[dir=back]
    C4 -> C5 -> C7
    # Branches
    node [style="filled,solid", shape=box, fillcolor=orange]
    edge [dir=back, penwidth=4, color=orange]
    C7 -> master
    # Head
    C7 -> HEAD
    edge [dir=forward, arrowhead=tee, penwidth=2, color=red]
    HEAD -> "master" [label="attached"]
    # Upstreams
    edge [arrowhead=dot, dir=forward, penwidth=2, color=blue]
    master -> master_r [label="upstream"]
    # Remotes
    node [style="filled,solid", shape=box, fillcolor=aquamarine3]
    edge [arrowhead=dot, dir=forward, penwidth=3, color=aquamarine3]
    origin -> C0r [lhead=cluster_remote]
  }
}
{{< /gravizo >}}

The push suceeds now!

---

## Best practices

* The **CLI** is your *truth*
  * Beware of the GUIs
* Prepare an *ignore list* early
  * And *maintain it*
  * And maybe prepare it manually and don't copy/paste it
* When you have untracked files, *decide whether you want to track them or ignore them*
* Be very careful with *what* you track
* Prepare an *attribute file*
* *Pull* before pushing

---

## Centralized Version Control Systems

{{< image src="2021-04-14-centralized-vcs.svg" max-h="65">}}

---

## Decentralized VCS

{{< image src="2021-04-14-decentralized-vcs.svg" max-h="65">}}

---

## Real-world DVCS

{{< image src="2021-04-14-dvcs-sink.svg" max-h="65">}}

---

## Git repository hosting

Several services allow the creation of *shared repositories on the cloud*.
They *enrich* the base git model with services built around the tool:

* **Forks**: copies of a repository associated to different users/organizations
* **Pull requests** (or **Merge requests**): formal requests to *pull* updates from *forks*
  * repositories do not allow pushes from everybody
  * what if we want to contribute to a project we cannot push to?
    * *fork* the repository (we *own* that copy)
    * write the contribution and push to our *fork*
    * ask the maintainers of the *original repository* to *pull from* our fork
* **Issue tracking**

---

## Most common services

* **GitHub**
  * Replaced Sourceforge as the *de-facto standard* for open source projects hosting
  * *Academic plan*
* **GitLab**
  * Available for free as *self-hosted*
  * Userbase grew when Microsoft acquired GitHub
* **Bitbucket**
  * From Atlassian
  * Well integrated with other products (e.g., Jira)


---

## GitHub

* *Hosting* for git repositories
* *Free for open source*
* *Academic accounts*
* *De-facto standard* for open source projects
* One *static website* per-project, per-user, and per-organization
  * (a feature exploited by these slides)

---

## Exercise:

* *Fork* the repository at: https://github.com/APICe-at-DISI/OOP-git-merge-conflict-test
* *Clone* the repository locally
* *Merge* the branch `feature` into `master`
* There will be a *conflict*!
  * If you know Java, solve it in such a way that the program prints both the author and the number of processors
  * If you don't, edit the file to make it appear like something that could work
* Once the file has the look you want, *complete the merge*
* *Push* to your fork
* *Observe* that you can open a pull request
  * Do not open it, or at least not towards the original repository
  * (I won't pull anyway ;) )

---

# DVCS: Workflows

> with great power comes great responsibility

and also

> power is nothing without control

Elements to consider:
* How *large* is the team?
* How *complex* is the project?
* Do team members work *together* (in spacetime)?
* Do team members *trust* each other?

---

## Trunk-based development(-like)

Single branch, shared truth repository, frequent merges
<br>
{{< image src="2021-04-14-dvcs-sink.svg" max-h="55">}}

* *Small* teams, *low-complexity* projects, *colocated* teams, *high* trust
* Typical of small company projects

---

## Git flow (classic)

Multiple branches, shared truth repository
<br>
{{< image src="2021-04-14-dvcs-flow-sink.svg" max-h="55">}}

* *Large* teams, *high-complexity* projects, *preferably colocated* teams, *high* trust
* Typical of large company projects

---

## Git flow structure

{{< image src="2021-04-13-gitflow.svg" max-h="55">}}

---

## Forks versus branches

* In Git, separate *development lines* are separate *branches*
* However, everyone has a *copy* of the *same repository*
* Git *hosting services* can *identify copies* of the same project belonging to different users

These copies are called **forks**

* Branches on one fork can be requested to be merged on another fork
  * With **merge request** (also called **pull request**, depending on the host)
* Pull requests enable easier code review
  * Necessary when the developer *does not trust* the contributor
  * But very useful anyway
* Working with pull requests is **not part of git** and *requires host support*
  * GitHub, GitLab, and Bitbucket all support pull requests

---

## Single branch, multiple forks

* Single branch, multiple independent repository copies

<p>
{{< image src="2021-04-14-dvcs-fork.svg" max-h="55">}}
</p>

* *Unknown* team size, *low-complexity* projects, *sparse* teams, *low* trust
* Typical of small open-source projects

---

## Git flow over multiple forks

* Single branch, multiple independent repository copies

<p>
{{< image src="2021-04-14-dvcs-flow-fork.svg" max-h="55">}}
</p>

* *Unknown* team size, *high-complexity* projects, *sparse* teams, *low* trust
* Typical of complex open-source projects

---

# Documenting projects using GitHub

Documentation of a project is **part of the project**

* **Documentation** must stay in the same repository of the project
* However, it should be *accessible to non-developers*

Meet GitHub Pages

* GitHub provides an *automated* way to publish *webpages from **Markdown*** text
* Markdown is a *human readable markup language*, [easy to learn](https://learnxinyminutes.com/docs/markdown/)
  * These slides are written in Markdown
  * (generation is a bit richer, but just to make the point)
* Supports *Jekyll* (a Ruby framework for static website generation) out of the box
  * We will not discuss it today
  * But maybe in LSS...

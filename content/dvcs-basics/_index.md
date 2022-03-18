 +++

title = "Software Engineering Module 3: Introduction to agile and DevOps"
description = "Introduction to agile and DevOps, a case from the literature, SCRUM"
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
* **Distributed** vs. *Centralized*
  * **Every developer has a whole copy of the entire history**
  * *There exist a central point of synchronization*
* Also called Source Content Management (SCM)

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

## Distributed version control: concepts

* **Repository**: project *meta-data*.
  Includes the information about the project history, how to roll changes back,
  authors, dates, differences between "saves", and so on.
* **Working Tree** (or worktree, or working directory): the collection of *files* (usually, inside a folder) that constitute the project.
  The version control system tracks changes to the working tree.
* **Commit**: a *saved status* of the project.
  Depending on the operation, it can be seen as the collection of *changes* required to transform the previous (*parent*) save into the current (differential tracking),
  or as a *snapshot* of the status of the worktree (snapshotting).
  It includes metadata about the author, the date, and a *message* summarizing the changes, and a unique *identifier*.
  A commit with no parent is an *initial commit*.
  A commit with multiple parents is a *merge commit*.
* **Branch**: a *sequence of commits* (more precisely, a directed acyclic graph)
* **Head**: a pointer to the *current commit*: when a new commit is performed,
  the head becomes the parent commit of the newly created commit,
  and then moves to point to the new commit.
* **Checkout**: *moves the head* to a commit, possibly updating the some or all the files in the working tree.
* **Merge**: *fusion of divergent branches*, generates a *merge commit*

---

## Visual representation

A *single branch*, linear development.

* every oval is a *commit*
* black arrows point to the *parent commit*
* orange boxes are *labels* (symbolic names of commits)

---

1. first commit
{{< gravizo width="100" >}}
  digraph G {
    fontname="Helvetica,Arial,sans-serif"
   	node [fontname="Helvetica,Arial,sans-serif"]
  	edge [fontname="Helvetica,Arial,sans-serif"]
    rankdir=LR;
    "HEAD -> branch_name" [style="filled,solid", shape=box, fillcolor=orange];
    C1 -> "HEAD -> branch_name" [dir=back, penwidth=4, color=orange];
  }
{{< /gravizo >}}

2. second commit

{{< gravizo width="30" >}}
  digraph G {
    fontname="Helvetica,Arial,sans-serif"
   	node [fontname="Helvetica,Arial,sans-serif"]
  	edge [fontname="Helvetica,Arial,sans-serif"]
    rankdir=LR;
    C1 -> C2 [dir=back];
    "HEAD -> branch_name" [style="filled,solid", shape=box, fillcolor=orange];
    C2 -> "HEAD -> branch_name" [dir=back, penwidth=4, color=orange];
  }
{{< /gravizo >}}

---

![four commits later](4commitslater.png)

---

{{< gravizo >}}
  digraph G {
    fontname="Helvetica,Arial,sans-serif"
   	node [fontname="Helvetica,Arial,sans-serif"]
  	edge [fontname="Helvetica,Arial,sans-serif"]
    rankdir=LR;
    C1 -> C2 -> C3 -> C4 -> C5 -> C6 [dir=back];
    "HEAD -> branch_name" [style="filled,solid", shape=box, fillcolor=orange];
    C6 -> "HEAD -> branch_name" [dir=back, penwidth=4, color=orange];
  }
{{< /gravizo >}}

Oh, no, there was a mistake! We need to roll back!

---

## *checkout of C4*

{{< gravizo >}}
  digraph G {
    fontname="Helvetica,Arial,sans-serif"
   	node [fontname="Helvetica,Arial,sans-serif"]
  	edge [fontname="Helvetica,Arial,sans-serif"]
    rankdir=LR;
    C1 -> C2 -> C3 -> C4 -> C5 -> C6 [dir=back];
    HEAD, branch_name [style="filled,solid", shape=box, fillcolor=orange];
    C4 -> HEAD [dir=back, penwidth=4, color=orange];
    C6 -> branch_name [dir=back, penwidth=4, color=orange];
  }
{{< /gravizo >}}

* No information is lost, we can get back to `C6` whenever we want to.
* what if we commit now?

---

## Branching!

{{< gravizo >}}
  digraph G {
    fontname="Helvetica,Arial,sans-serif"
   	node [fontname="Helvetica,Arial,sans-serif"]
  	edge [fontname="Helvetica,Arial,sans-serif"]
    rankdir=LR;
    C1 -> C2 -> C3 -> C4 -> C5 -> C6 [dir=back];
    C4 -> C7 [dir=back]
    "HEAD -> another_branch", branch_name [style="filled,solid", shape=box, fillcolor=orange];
    C7 -> "HEAD -> another_branch" [dir=back, penwidth=4, color=orange];
    C6 -> branch_name [dir=back, penwidth=4, color=orange];
  }
{{< /gravizo >}}

* Okay, but there was useful stuff in `C5`, I'd like into `another_branch`

---

## Merging!

{{< gravizo >}}
  digraph G {
    fontname="Helvetica,Arial,sans-serif"
   	node [fontname="Helvetica,Arial,sans-serif"]
  	edge [fontname="Helvetica,Arial,sans-serif"]
    rankdir=LR;
    C1 -> C2 -> C3 -> C4 -> C5 -> C6 [dir=back];
    C4 -> C7 [dir=back]
    C5 -> C7 [dir=back]
    "HEAD -> another_branch", branch_name [style="filled,solid", shape=box, fillcolor=orange];
    C7 -> "HEAD -> another_branch" [dir=back, penwidth=4, color=orange];
    C6 -> branch_name [dir=back, penwidth=4, color=orange];
  }
{{< /gravizo >}}

**Notice that:**
* we have two branches
* merge `C7` is a merge commit: it has two parents `C4` and `C5`
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

```lisp
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

* Requires an *author* and an *email*
  * They can be configured globally (at the computer level):
    * `git config --global user.name 'Your Real Name'`
    * `git config --global user.email 'your@email.com'`
  * The global settings can be overridden at the repository level
    * e.g., you want to commit with a different email between work and personal projects
    * `git config user.name 'Your Real Name'`
    * `git config user.email 'your@email.com'`
* Requires a **message**, using appropriate messages is **extremely important**
  * If unspecified, the commit does not get performed
  * it can be specified inline with `-m`, otherwise Git will pop up the default editor
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

```gitignore
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

example output of `git log --oneline --all --graph`

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

Example output:
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

{{< gravizo >}}
  digraph G {
    fontname="Helvetica,Arial,sans-serif"
   	node [fontname="Helvetica,Arial,sans-serif"]
  	edge [fontname="Helvetica,Arial,sans-serif"]
    rankdir=LR;
    # Commits
    C0 -> C1 -> C2 -> C3 -> C4 -> C5 -> C6 [dir=back];
    # Branches
    node [style="filled,solid", shape=box, fillcolor=orange];
    edge [dir=back, penwidth=4, color=orange];
    C6 -> HEAD;
    C6 -> master;
    # Head
    edge [dir=forward, arrowhead=tee, penwidth=2, color=red];
    HEAD -> master [label="attached"];
  }
{{< /gravizo >}}

When an older commit is checked out, however, this condition doesn't occur, if we run `git checkout HEAD~4`:

{{< gravizo >}}
  digraph G {
    fontname="Helvetica,Arial,sans-serif"
   	node [fontname="Helvetica,Arial,sans-serif"]
  	edge [fontname="Helvetica,Arial,sans-serif"]
    rankdir=LR;
    # Commits
    C0 -> C1 -> C2 -> C3 -> C4 -> C5 -> C6 [dir=back];
    # Branches
    node [style="filled,solid", shape=box, fillcolor=orange];
    edge [dir=back, penwidth=4, color=orange];
    C2 -> HEAD;
    C6 -> master;
    # Head
    edge [dir=forward, arrowhead=tee, penwidth=2, color=red];
    nowhere [style=invis,shape=point]
    HEAD -> nowhere [label="detached"];
  }
{{< /gravizo >}}

The system enters a special workmode called *detached head*. When in detached head:
* It is possible to make changes and experiments
* Git allows to make commits, but they are lost!
  * Not really, but to retrieve them we need `git reflog` and `git cherry-pick`, that we won't discuss

---

### Creating branches

To be able to store our commits, we need to create a **branch**, then checkout the branch, so that `HEAD` returns attached.

In Git, new branches are created with `git branch branch_name`

{{< gravizo width=60 >}}
  digraph G {
    fontname="Helvetica,Arial,sans-serif"
   	node [fontname="Helvetica,Arial,sans-serif"]
  	edge [fontname="Helvetica,Arial,sans-serif"]
    rankdir=LR;
    # Commits
    C0 -> C1 -> C2 -> C3 -> C4 -> C5 -> C6 [dir=back];
    # Branches
    node [style="filled,solid", shape=box, fillcolor=orange];
    edge [dir=back, penwidth=4, color=orange];
    C2 -> HEAD;
    C6 -> master;
    # Head
    edge [dir=forward, arrowhead=tee, penwidth=2, color=red];
    nowhere [style=invis,shape=point]
    HEAD -> nowhere [label="detached"];
  }
{{< /gravizo >}}


⬇️ `git branch new-experiment` ⬇️

{{< gravizo width=60 >}}
  digraph G {
    fontname="Helvetica,Arial,sans-serif"
   	node [fontname="Helvetica,Arial,sans-serif"]
  	edge [fontname="Helvetica,Arial,sans-serif"]
    rankdir=LR;
    # Commits
    C0 -> C1 -> C2 -> C3 -> C4 -> C5 -> C6 [dir=back];
    # Branches
    node [style="filled,solid", shape=box, fillcolor=orange];
    edge [dir=back, penwidth=4, color=orange];
    C2 -> HEAD;
    C6 -> master;
    C2 -> "new-experiment";
    # Head
    edge [dir=forward, arrowhead=tee, penwidth=2, color=red];
    nowhere [style=invis,shape=point]
    HEAD -> nowhere [label="detached"];
  }
{{< /gravizo >}}

⬇️ `git checkout new-experiment` ⬇️

{{< gravizo width=60 >}}
  digraph G {
    fontname="Helvetica,Arial,sans-serif"
   	node [fontname="Helvetica,Arial,sans-serif"]
  	edge [fontname="Helvetica,Arial,sans-serif"]
    rankdir=LR;
    # Commits
    C0 -> C1 -> C2 -> C3 -> C4 -> C5 -> C6 [dir=back];
    # Branches
    node [style="filled,solid", shape=box, fillcolor=orange];
    edge [dir=back, penwidth=4, color=orange];
    C2 -> HEAD;
    C6 -> master;
    C2 -> "new-experiment";
    # Head
    edge [dir=forward, arrowhead=tee, penwidth=2, color=red];
    HEAD -> "new-experiment" [label="attached"];
  }
{{< /gravizo >}}

---

## One-shot branch creation

As you can imagine, creating a *new branch* and *attaching `HEAD`* to the freshly created branch is pretty common

When an operation is very common, usually a short-hand is provided:
* `git checkout -b new-branch-name`
* Creates `new-branch-name` from the current position of `HEAD`
* Attaches `HEAD` to `new-branch-name`

{{< gravizo width=60 >}}
  digraph G {
    fontname="Helvetica,Arial,sans-serif"
   	node [fontname="Helvetica,Arial,sans-serif"]
  	edge [fontname="Helvetica,Arial,sans-serif"]
    rankdir=LR;
    # Commits
    C0 -> C1 -> C2 -> C3 -> C4 -> C5 -> C6 [dir=back];
    # Branches
    node [style="filled,solid", shape=box, fillcolor=orange];
    edge [dir=back, penwidth=4, color=orange];
    C2 -> HEAD;
    C6 -> master;
    # Head
    edge [dir=forward, arrowhead=tee, penwidth=2, color=red];
    nowhere [style=invis,shape=point]
    HEAD -> nowhere [label="detached"];
  }
{{< /gravizo >}}

⬇️ `git checkout -b new-experiment` ⬇️

{{< gravizo width=60 >}}
  digraph G {
    fontname="Helvetica,Arial,sans-serif"
   	node [fontname="Helvetica,Arial,sans-serif"]
  	edge [fontname="Helvetica,Arial,sans-serif"]
    rankdir=LR;
    # Commits
    C0 -> C1 -> C2 -> C3 -> C4 -> C5 -> C6 [dir=back];
    # Branches
    node [style="filled,solid", shape=box, fillcolor=orange];
    edge [dir=back, penwidth=4, color=orange];
    C2 -> HEAD;
    C6 -> master;
    C2 -> "new-experiment";
    # Head
    edge [dir=forward, arrowhead=tee, penwidth=2, color=red];
    HEAD -> "new-experiment" [label="attached"];
  }
{{< /gravizo >}}


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

{{< gravizo >}}
  digraph G {
    fontname="Helvetica,Arial,sans-serif"
   	node [fontname="Helvetica,Arial,sans-serif"]
  	edge [fontname="Helvetica,Arial,sans-serif"]
    rankdir=LR;
    # Commits
    C0 -> C1 -> C2 -> C3 -> C4 -> C5 -> C6 [dir=back];
    C4 -> C7 -> C8 [dir=back];
    # Branches
    node [style="filled,solid", shape=box, fillcolor=orange];
    edge [dir=back, penwidth=4, color=orange];
    C8 -> HEAD;
    C6 -> master;
    C8 -> "new-experiment";
    # Head
    edge [dir=forward, arrowhead=tee, penwidth=2, color=red];
    HEAD -> "new-experiment" [label="attached"];
  }
{{< /gravizo >}}

⬇️ `git merge master` ⬇️

{{< gravizo >}}
  digraph G {
    fontname="Helvetica,Arial,sans-serif"
   	node [fontname="Helvetica,Arial,sans-serif"]
  	edge [fontname="Helvetica,Arial,sans-serif"]
    rankdir=LR;
    # Commits
    C0 -> C1 -> C2 -> C3 -> C4 -> C5 -> C6 -> C9 [dir=back];
    C4 -> C7 -> C8 -> C9 [dir=back];
    # Branches
    node [style="filled,solid", shape=box, fillcolor=orange];
    edge [dir=back, penwidth=4, color=orange];
    C9 -> HEAD;
    C6 -> master;
    C9 -> "new-experiment";
    # Head
    edge [dir=forward, arrowhead=tee, penwidth=2, color=red];
    HEAD -> "new-experiment" [label="attached"];
  }
{{< /gravizo >}}


---

## Fast forwarding

Consider this situation:

{{< gravizo >}}
  digraph G {
    fontname="Helvetica,Arial,sans-serif"
   	node [fontname="Helvetica,Arial,sans-serif"]
  	edge [fontname="Helvetica,Arial,sans-serif"]
    rankdir=LR;
    # Commits
    C0 -> C1 -> C2 -> C3 -> C4 -> C5 -> C6 [dir=back];
    # Branches
    node [style="filled,solid", shape=box, fillcolor=orange];
    edge [dir=back, penwidth=4, color=orange];
    C4 -> HEAD;
    C6 -> master;
    C4 -> "new-experiment";
    # Head
    edge [dir=forward, arrowhead=tee, penwidth=2, color=red];
    HEAD -> "new-experiment" [label="attached"];
  }
{{< /gravizo >}}

* We want `another_branch` to also have the changes in `C5` and `C6`
  * namely, to be up to date with `some_branch`
* But `some_branch` contains all the commits of `another_branch`
* We don't really need a merge commit, we can just move the `another_branch` to point it to `C6`
* $\Rightarrow$ This is called a **fast-forward**
  * It is the *default behavior* in Git when merging branches where the target is the same as the head plus something

{{< gravizo >}}
  digraph G {
    fontname="Helvetica,Arial,sans-serif"
   	node [fontname="Helvetica,Arial,sans-serif"]
  	edge [fontname="Helvetica,Arial,sans-serif"]
    rankdir=LR;
    # Commits
    C0 -> C1 -> C2 -> C3 -> C4 -> C5 -> C6 [dir=back];
    # Branches
    node [style="filled,solid", shape=box, fillcolor=orange];
    edge [dir=back, penwidth=4, color=orange];
    C6 -> master;
    C6 -> HEAD;
    C6 -> "new-experiment";
    # Head
    edge [dir=forward, arrowhead=tee, penwidth=2, color=red];
    HEAD -> "new-experiment" [label="attached"];
  }
{{< /gravizo >}}

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

## Newlines and Git

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

{{< gravizo width=60 >}}
  digraph G {
    fontname="Helvetica,Arial,sans-serif"
   	node [fontname="Helvetica,Arial,sans-serif"]
  	edge [fontname="Helvetica,Arial,sans-serif"]
    rankdir=LR;
    # Commits
    C0 -> C1 -> C2 -> C3 -> C4 -> C5 -> C6 [dir=back];
    # Branches
    node [style="filled,solid", shape=box, fillcolor=orange];
    edge [dir=back, penwidth=4, color=orange];
    C6 -> master;
    C6 -> HEAD;
    # Head
    edge [dir=forward, arrowhead=tee, penwidth=2, color=red];
    HEAD -> master [label="attached"];
  }
{{< /gravizo >}}


⬇️ `git checkout C4 && git branch 1.2.3 && git checkout master` ⬇️

{{< gravizo width=60 >}}
  digraph G {
    fontname="Helvetica,Arial,sans-serif"
   	node [fontname="Helvetica,Arial,sans-serif"]
  	edge [fontname="Helvetica,Arial,sans-serif"]
    rankdir=LR;
    # Commits
    C0 -> C1 -> C2 -> C3 -> C4 -> C5 -> C6 [dir=back];
    # Branches
    node [style="filled,solid", shape=box, fillcolor=orange];
    edge [dir=back, penwidth=4, color=orange];
    C6 -> master;
    C6 -> HEAD;
    C4 -> "1.2.3"
    # Head
    edge [dir=forward, arrowhead=tee, penwidth=2, color=red];
    HEAD -> master [label="attached"];
  }
{{< /gravizo >}}

Appears good, but if by mistake we do something like: ⬇️ `git checkout 1.2.3` [some changes] `git commit` ⬇️

{{< gravizo width=60 >}}
  digraph G {
    fontname="Helvetica,Arial,sans-serif"
   	node [fontname="Helvetica,Arial,sans-serif"]
  	edge [fontname="Helvetica,Arial,sans-serif"]
    rankdir=LR;
    # Commits
    C0 -> C1 -> C2 -> C3 -> C4 -> C5 -> C6 [dir=back];
    C4 -> C7 [dir=back];
    # Branches
    node [style="filled,solid", shape=box, fillcolor=orange];
    edge [dir=back, penwidth=4, color=orange];
    C6 -> master;
    C6 -> HEAD;
    C7 -> "1.2.3"
    # Head
    edge [dir=forward, arrowhead=tee, penwidth=2, color=red];
    HEAD -> master [label="attached"];
  }
{{< /gravizo >}}


Our version **moved**, *we never want this to happen*!

---

## Tagging

Git provides a dedicated `tag` subcommand to create *permanent labels* attached to commits.
Tags come in two fashions:
* **Lightweight** *tags* are very similar to a "permanent branch": *pointers to commits that never change*
* **Annotated** *tags* store additional information: a *message* (often used for logging changes), and, optionally, a *signature*
  * created by adding the `-a` option (or `-s`/`-u` for signed tags)

{{< gravizo width=60 >}}
  digraph G {
    fontname="Helvetica,Arial,sans-serif"
   	node [fontname="Helvetica,Arial,sans-serif"]
  	edge [fontname="Helvetica,Arial,sans-serif"]
    rankdir=LR;
    C0 -> C1 -> C2 -> C3 -> C4 -> C5 -> C6 [dir=back];
    "HEAD -> master" [style="filled,solid", shape=box, fillcolor=orange];
    C6 -> "HEAD -> master" [dir=back, penwidth=4, color=orange];
  }
{{< /gravizo >}}

⬇️ `git checkout C4 && git tag 1.2.3` ⬇️

{{< gravizo width=60 >}}
  digraph G {
    fontname="Helvetica,Arial,sans-serif"
   	node [fontname="Helvetica,Arial,sans-serif"]
  	edge [fontname="Helvetica,Arial,sans-serif"]
    rankdir=LR;
    C0 -> C1 -> C2 -> C3 -> C4 -> C5 -> C6 [dir=back];
    "HEAD (detached)", "master", "1.2.3" [style="filled,solid", shape=box, fillcolor=orange];
    C6 -> "master" [dir=back, penwidth=4, color=orange];
    C4 -> "1.2.3" [dir=back, penwidth=4, color=orange];
    C4 -> "HEAD (detached)" [dir=back, penwidth=4, color=orange];
  }
{{< /gravizo >}}

`HEAD` cannot attach to tags!

---

## Branches as labels: deletion

As we discussed, *branches* in Git work like *special labels* that move forward if `HEAD` is attached to them and commit is performed.

Also, the *history* tracked by git is a *directed acyclic graph* (each commit has a reference to its parents)

$\Rightarrow$ *Branches can be removed without information loss*, as far as there is at least *another branch* from which *all the commits* of the deleted branch are *reachable*

Branch deletion is performed with `git branch -d branch-name`. It is *safe* (fails if there is potential information loss).

{{< gravizo width=60 >}}
  digraph G {
    fontname="Helvetica,Arial,sans-serif"
   	node [fontname="Helvetica,Arial,sans-serif"]
  	edge [fontname="Helvetica,Arial,sans-serif"]
    rankdir=LR;
    C0 -> C1 -> C2 -> C3 -> C4 -> C5 -> C6 [dir=back];
    C4 -> C7 [dir=back];
    "HEAD -> master", "fix/bug22", "feat/serverless" [style="filled,solid", shape=box, fillcolor=orange];
    C6 -> "HEAD -> master" [dir=back, penwidth=4, color=orange];
    C7 -> "feat/serverless" [dir=back, penwidth=4, color=orange];
    C3 -> "fix/bug22" [dir=back, penwidth=4, color=orange];
  }
{{< /gravizo >}}


⬇️ `git branch -d fix/bug22` ⬇️

{{< gravizo width=60 >}}
  digraph G {
    fontname="Helvetica,Arial,sans-serif"
   	node [fontname="Helvetica,Arial,sans-serif"]
  	edge [fontname="Helvetica,Arial,sans-serif"]
    rankdir=LR;
    C0 -> C1 -> C2 -> C3 -> C4 -> C5 -> C6 [dir=back];
    C4 -> C7 [dir=back];
    "HEAD -> master", "feat/serverless" [style="filled,solid", shape=box, fillcolor=orange];
    C6 -> "HEAD -> master" [dir=back, penwidth=4, color=orange];
    C7 -> "feat/serverless" [dir=back, penwidth=4, color=orange];
  }
{{< /gravizo >}}

No commit is lost, branch `fix/bug22` is removed
* `git branch -d feat/serverless` would **fail** with an error message, as `C7` would be lost

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
* The `clone` subcommand checks out the remote branch where the `HEAD` is attached
  * the "default branch"

Examples:
* `git clone /some/repository/on/my/file/system destination`
  * creates a local folder called `destination` and copies the repository from the local directory
* `git clone https://somewebsite.com/someRepository.git myfolder`
  * creates a local folder called `myfolder` and copies the repository located at the specified `URL`
* `git clone user@sshserver.com:SomePath/SomeRepo.git`
  * creates a local folder called `SomeRepo` and copies the repository located at the specified `URL`

---

## Remotes

* Remotes are the *known copies* of the repository that exist somewhere (usually in the Internet)
* Each remote has a *name* and a *URI*
* When a repository is created via `init`, no remote is known.
* When a repository is imported via `clone`, a remote called `origin` is created automatically

*Non-local branches can be referenced* as `remoteName/branchName`

The `remote` subcommand is used to inspect and manage remotes:
* `git remote -v` *lists* the known remotes
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

## Actual result of `git clone somesite.com/repo.git`

{{< gravizo width=100 >}}
  digraph pippo {
    fontname="Helvetica,Arial,sans-serif"
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
      C0r -> C1r -> C2r -> C3r -> C4r [dir=back];
      
      # Branches
      node [style="filled,solid", shape=box, fillcolor=orange];
      master_r [label=master]
      HEAD_r [label=HEAD]
      C4r -> HEAD_r [dir=back, penwidth=4, color=orange];
      C4r -> master_r [dir=back, penwidth=4, color=orange];
      HEAD_r -> master_r [arrowhead=tee, penwidth=2, color=red, label="attached"];
    }

    subgraph cluster_local {
      label="Local"
      color=black
      
      # Commits
      C0 -> C1 -> C2 -> C3 -> C4 [dir=back];
      
      # Branches
      node [style="filled,solid", shape=box, fillcolor=orange];
      C4 -> HEAD [dir=back, penwidth=4, color=orange];
      C4 -> master [dir=back, penwidth=4, color=orange];
      HEAD -> master [arrowhead=tee, penwidth=2, color=red, label="attached"];
      master -> master_r [arrowhead=dot, penwidth=2, color=blue, label="upstream"];
      
      # Remotes
      node [style="filled,solid", shape=box, fillcolor=green];
      origin -> C4r [lhead=cluster_remote]
    }

  }
{{< /gravizo >}}

---

---

## Distributed version control with git: a recap

### *Remote*
* (possibly remote) locations hosting copies of branches of this repository exist

### `git remote`
* Configures the *remotes*

### *upstream*
* The default *remote* for network operations.

---

## Distributed version control with git: a recap

### `git clone`
* Copies a repository from a possibly remote location.
* Alternative to `init`
* Automatically sets the local branch *upstream* to the cloned location.

---

## Distributed version control with git: a recap

### `git fetch <remote>`
* **Updates** the state of `<remote>`
* If remote is omitted, updates the state of the branch *upstream*'s *remote*

### `git pull <remote> <branch>`
* **Shortcut** for `git fetch && git merge FETCH_HEAD`

### `git push <remote> <branch>`
* Sends local changes *remote* *branch*
* Requires branches to **share a root**
* If remote and branch are omitted, updates are sent to the *upstream*

---

--follow-tags

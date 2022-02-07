# Basics

```{objectives}
- Learn to create Git repositories and make commits.
- Get a grasp of the structure of a repository.
- Learn how to inspect the project history.
- Learn how to write useful commit log messages.
```

```{instructor-note}
- 20 min teaching/type-along
- 15 min exercise
```


## What is Git, and what is a Git repository?

- Git is a *version control system*: can record snapshots and track the content of a folder as it changes over time.
- Every time we **commit** a snapshot, Git records a snapshot of the **entire project**, saves it, and assigns it a version.
- These snapshots are kept inside a sub-folder called `.git`.
- If we remove `.git`, we remove the repository and history (but keep the working directory!).
- `.git` uses relative paths - you can move the whole thing somewhere else and it will still work
- Git doesn't do anything unless you ask it to (it does not record anything automatically).
- Multiple interfaces to Git exist (command line, graphical interfaces, web interfaces).


## Recording a snapshot with Git

- Git takes snapshots only if we request it.
- We will record changes always in two steps (we will later explain why this is a recommended practice):

```console
$ git add somefile.txt
$ git commit

$ git add file.txt anotherfile.txt
$ git commit
```

- We first focus (`git add`, we "stage" the change), then shoot (`git commit`):

```{figure} img/git_stage_commit.svg
:alt: Git staging
:width: 100%

Git staging and committing.
```

```{discussion} Question for the more advanced participants
What do you think will be the outcome if you
stage a file and then edit it and stage it again, do this several times and
at the end perform a commit? (think of focusing several scenes and pressing the
shoot button only at the end)
```


## Before we start we need to configure Git

If you haven't already configured Git, please follow the instructions in the
[Git refresher lesson](https://coderefinery.github.io/git-refresher/01-setup/#configuring-git)


## Type-along: Tracking a guacamole recipe with Git

We will learn how to initialize a Git repository, how to track changes, and how
to make delicious guacamole!

This example is inspired by [Byron Smith](http://blog.byronjsmith.com), for original reference, see
[this thread](http://lists.software-carpentry.org/pipermail/discuss/2016-May/004529.html).
The motivation for taking a cooking recipe instead of a program is that everybody can relate to cooking
but not everybody may be able to relate to a program written in e.g. Python or another language.

Let us start.
One of the basic principles of Git is that it is **easy to create repositories**:

```console
$ mkdir recipe
$ cd recipe
$ git init
```

That's it! We have now created an empty Git repository.

We will use `git status` a lot to check out what is going on:

```console
$ git status

On branch master

No commits yet

nothing to commit (create/copy files and use "git add" to track)
```

We will make sense of this information during this morning.

Let us now **create two files**.

One file is called `instructions.txt` and contains:

```shell
* chop avocados
* chop onion
* squeeze lime
* add salt
* and mix well
```

The second file is called `ingredients.txt` and contains:

```shell
* 2 avocados
* 1 lime
* 2 tsp salt
```

As mentioned above, in Git you can always check the status of files in your repository using
`git status`. It is always a safe command to run and in general a good idea to
do when you are trying to figure out what to do next:

```console
$ git status

On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)

	ingredients.txt
	instructions.txt

nothing added to commit but untracked files present (use "git add" to track)
```

The two files are untracked in the repository (directory). You want to **add the files** (focus the camera)
to the list of files tracked by Git. Git does not track
any files automatically and you need make a conscious decision to add a file. Let's do what
Git hints at and add the files:


```console
$ git add ingredients.txt
$ git add instructions.txt
$ git status

On branch master

Initial commit

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

	new file:   ingredients.txt
	new file:   instructions.txt
```

Now this change is *staged* and ready to be committed.

Let us now commit the change to the repository:

```console
$ git commit -m "adding ingredients and instructions"

[master (root-commit) aa243ea] adding ingredients and instructions
 2 files changed, 8 insertions(+)
 create mode 100644 ingredients.txt
 create mode 100644 instructions.txt
```

Right after we query the status to get this useful command into our muscle memory:

```console
$ git status
```

What does the `-m` flag mean? Let us check the help page for that command:

```console
$ git help commit
```

You should see a very long help page as the tool is very versatile (press q to quit).
Do not worry about this now but keep in mind that you can always read the help files
when in doubt. Searching online can also be useful, but choosing search terms
to find relevant information takes some practice and discussions in some
online threads may be confusing.
Note that help pages also work when you don't have a network connection!


### Git history and log

Now try `git log`:

```console
$ git log

commit d619bf848a3f83f05e8c08c7f4dcda3490cd99d9
Author: Radovan Bast <bast@users.noreply.github.com>
Date:   Thu May 4 15:02:56 2017 +0200

    adding ingredients and instructions
```

- We can browse the development and access each state that we have committed.
- The long hashes uniquely label a state of the code.
- They are not just integers counting 1, 2, 3, 4, ... (why?).
- Output is in reverse chronological order, i.e. newest commits on top.
- We will use them when comparing versions and when going back in time.
- `git log --oneline` only shows the first 7 characters of the commit hash and is good to get an overview.
- If the first characters of the hash are unique it is not necessary to type the entire hash.
- `git log --stat` is nice to show which files have been modified.


## Exercise: record changes

````{exercise}
  Add 1/2 onion to `ingredients.txt` and also the instruction
  to "enjoy!" to `instructions.txt`. Do not stage the changes yet.
 
  When you are done editing the files, try `git diff`:
 
  ```console
  $ git diff
  ```
 
  ```
  diff --git a/ingredients.txt b/ingredients.txt
  index 2607525..ec0abc6 100644
  --- a/ingredients.txt
  +++ b/ingredients.txt
  @@ -1,3 +1,4 @@
   * 2 avocados
   * 1 lime
   * 2 tsp salt
  +* 1/2 onion
  diff --git a/instructions.txt b/instructions.txt
  index 6a8b2af..f7dd63a 100644
  --- a/instructions.txt
  +++ b/instructions.txt
  @@ -3,3 +3,4 @@
   * squeeze lime
   * add salt
   * and mix well
  +* enjoy!
  ```
 
  Now first stage and commit each change separately (what happens when we leave out the `-m` flag?):
 
  ```console
  $ git add ingredients.txt
  $ git commit -m "add half an onion"
  $ git add instructions.txt
  $ git commit                   # <-- we have left out -m "..."
  ```
 
  When you leave out the `-m` flag, Git should open an editor where you can edit
  your commit message. This message will be associated and stored with the
  changes you made. This message is your chance to explain what you've done and
  convince others (and your future self) that the changes you made were
  justified.  Write a message and save and close the file.
 
  When you are done committing the changes, experiment with these commands:
 
  ```console
  $ git log    # show commit logs
  $ git show   # show various types of objects
  $ git diff   # show changes
  ```
````


## Optional exercises

```{exercise} Comparing and showing commits
1. Inspect differences between commit hashes with `git diff <hash1> <hash2>`.
2. Have a look at specific commits with `git show <hash>`.
```

```{exercise} Renaming and removing files
1. Create a new file, `git add` and `git commit` the file.
2. Rename the file with `git mv` (you will need to `git commit` the rename).
3. Use `git log --oneline` and `git status`.
4. Remove the file with `git rm` (again you need to `git commit` the change).
5. Inspect the history with `git log --stat`. Can you recover the removed file from the Git history?
   Hint: You can try with a web search for "git checkout removed file from past".
```

````{exercise} Visual diff tools
  - Make further modifications and experiment with `git difftool` (requires installing one of the [visual diff tools](https://coderefinery.github.io/installation/difftools/)):
 
  On Windows or Linux:
  ```
  $ git difftool --tool=meld
  ```
 
  On macOS:
  ```
  $ git difftool --tool=opendiff
  ```
 
  ```{figure} img/meld.png
  :alt: Git difftool using meld
  :width: 100%

  Git difftool using meld.
  ```
 
  You probably want to use the same visual diff tool every time and
  you can configure Git for that:
  ```
  $ git config --global diff.tool meld
  ```
````


## Writing useful commit messages

Using `git log --oneline` we understand that the first line of the commit message is very important.

Good example:

```
increase threshold alpha to 2.0

the motivation for this change is
to enable ...
...
```

Convention: **one line summarizing the commit, then one empty line,
then paragraph(s) with more details in free form, if necessary**.

- Bad commit messages: "fix", "oops", "save work", "foobar", "toto", "qppjdfjd", "".
- [http://whatthecommit.com](http://whatthecommit.com)
- Write commit messages in English that will be understood
  15 years from now by someone else than you.
- Many projects start out as projects "just for me" and end up to be successful projects
  that are developed by 50 people over decades.
- [Commits with multiple authors](https://help.github.com/articles/creating-a-commit-with-multiple-authors/)

Good references:

- ["My favourite Git commit"](https://fatbusinessman.com/2019/my-favourite-git-commit)
- ["On commit messages"](https://who-t.blogspot.com/2009/12/on-commit-messages.html)
- ["How to Write a Git Commit Message"](https://chris.beams.io/posts/git-commit/)


## Ignoring files and paths with .gitignore

- Should we add and track all files in a project?
- How about generated files?
- Why is it considered a bad idea to commit compiled binaries to version control?
- What types of generated files do you know?

As a general rule compiled files are not
committed to version control. There are many reasons for this:

- Your code could be run on different platforms.
- These files are automatically generated and thus do not contribute in any meaningful way.
- The number of changes to track per source code change can increase quickly.
- When tracking generated files you could see differences in the code although you haven't touched the code.

For this we use `.gitignore` files. Example:

```
# ignore compiled python 2 files
*.pyc
# ignore compiled python 3 files
__pycache__
```

`.gitignore` uses something called a
[shell glob syntax](https://en.wikipedia.org/wiki/Glob_(programming)) for
determining file patterns to ignore. You can read more about the syntax in the
[documentation](https://git-scm.com/docs/gitignore).

An example taken from [documentation](https://git-scm.com/docs/gitignore):

```
# ignore objects and archives, anywhere in the tree.
*.[oa]
# ignore generated html files,
*.html
# except foo.html which is maintained by hand
!foo.html
# ignore everything under build directory
build/
```

You can have `.gitignore` files in lower level directories and they affect the paths
relatively.

`.gitignore` should be part of the repository (why?).


### Clean working area

- Use `git status` a lot.
- Untracked files belong to .gitignore.
- **All files should be either tracked or ignored**.


## Graphical user interfaces

We have seen how to make commits directly via the GitHub website, and also via command line.
But it is also possible to work from within a Git graphical user interface (GUI):

- [GitHub Desktop](https://desktop.github.com)
- [SourceTree](https://www.sourcetreeapp.com)
- [List of third-party GUIs](https://git-scm.com/downloads/guis)


## Summary

Now we know how to save snapshots:

```console
$ git add <file(s)>
$ git commit
```

And this is what we do as we program.

Every state is then saved and later we will learn how to go back to these "checkpoints"
and how to undo things.

```console
$ git init    # initialize new repository
$ git add     # add files or stage file(s)
$ git commit  # commit staged file(s)
$ git status  # see what is going on
$ git log     # see history
$ git diff    # show unstaged/uncommitted modifications
$ git show    # show the change for a specific commit
$ git mv      # move tracked files
$ git rm      # remove tracked files
```

Git is not ideal for large binary files
(for this consider [http://git-annex.branchable.com](http://git-annex.branchable.com)).

````{challenge} Test your understanding
  Which command(s) below would save the changes of `myfile.txt`
  to my local Git repository?
 
  1. ```console
     $ git commit -m "my recent changes"
     ```
  2. ```console
     $ git init myfile.txt
     $ git commit -m "my recent changes"
     ```
  3. ```console
     $ git add myfile.txt
     $ git commit -m "my recent changes"
     ```
  4. ```console
     $ git commit -m myfile.txt "my recent changes"
     ```
  ```{solution}
   
  1. Would only create a commit if files have already been staged.
  2. Would try to create a new repository.
  3. Is correct: first add the file to the staging area, then commit.
  4. Would try to commit a file "my recent changes" with the message myfile.txt.
  ```
````

```{keypoints}
- "Initializing a Git repository is simple: `git init`"
- Commits should be used to tell a story.
- Git uses the .git folder to store the snapshots.
```
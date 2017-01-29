#HSLIDE

## GIT INSIGHTS
##### Inner workings

#HSLIDE

## TOC
- About git
- Different areas files are in
- Basic CLI commands
- What are commits
- Branches
- Merging algoritms
- Conflicts
- Git log
- Git bisect
- Git workflows

#HSLIDE

## About git

#VSLIDE

## Kinds of version control

- local version control  <!-- .element: class="fragment" -->
- centralised version control  <!-- .element: class="fragment" -->
  - perforce
  - CVS
  - Subversion
- distributed version control  <!-- .element: class="fragment" -->
  - Git
  - Mercurial
  - DARCS

#VSLIDE

## CENTRALISED
<img src="https://git-scm.com/book/en/v2/images/centralized.png" width=400 />

#VSLIDE

## DISTRIBUTED
<img src="https://git-scm.com/book/en/v2/images/distributed.png" width=400 />

#VSLIDE

## Key requirements when developing GIT

- Speed
- Simple design
- Strong support for non-linear development (thousands of parallel branches)
- Fully distributed
- Able to handle large projects like the Linux kernel efficiently (speed and data size)

#VSLIDE

## Traditional VCSses store differences
CVS, Subversion, Perforce, Bazaar, and so on
![Image-Relative](https://git-scm.com/book/en/v2/images/deltas.png)

#VSLIDE

## Git stores snapshots
Huge benefits when you start to branch
![Image-Relative](https://git-scm.com/book/en/v2/images/snapshots.png)

#VSLIDE

## Other advantages of git
- Nearly everything is local <!-- .element: class="fragment" -->
- Git Has Integrity <!-- .element: class="fragment" -->
  - corruption detected immediately
  - Everything is referenced by SHA1-hashes of contents
- Git almost exclusively adds data <!-- .element: class="fragment" -->
  - Once you commit it very difficult to lose code

#HSLIDE

## stages

- modified  <!-- .element: class="fragment" -->
- staged  <!-- .element: class="fragment" -->
- committed  <!-- .element: class="fragment" -->

#VSLIDE
## main sections of a git project
- working tree  <!-- .element: class="fragment" -->
- staging area  <!-- .element: class="fragment" -->
- .git repository  <!-- .element: class="fragment" -->

#VSLIDE
![Image-Relative](https://git-scm.com/book/en/v2/images/areas.png)

#HSLIDE

## Basic CLI commands
- CLI: the only place where you can run ALL commands, most GUIS implement a subset  <!-- .element: class="fragment" -->
- When you know the CLI, you can work with any GUI  <!-- .element: class="fragment" -->

#VSLIDE

## Git config
Tool for setting config variables
  - /etc/gitconfig file: Contains values for every user on the system and all their repositories. If you pass the option --system to git config, it reads and writes from this file specifically.
  - ~/.gitconfig or ~/.config/git/config file: Specific to your user. Pass the --global option.
  - config file in the Git directory (that is, .git/config)
  
#VSLIDE

## Git config --list
```
$ git config --list
user.name=John Doe
user.email=johndoe@example.com
color.status=auto
color.branch=auto
color.interactive=auto
color.diff=auto
...
```

#VSLIDE

## Getting help
```
$ git help <verb>
$ git <verb> --help
$ man git-<verb>
```
For example
```
git help config
```

#VSLIDE

## Git init

Initializing a Repository in an Existing Directory
(makes the .git directory and database)

## Git clone

Cloning an Existing Repository
(copies the _complete_ repository to your computer)

```
$ git clone https://github.com/libgit2/libgit2 mylibgit`
```

#VSLIDE

## Recording changes

  - untracked
  - tracked
    - unmodified
    - modified
    - staged
<img src="https://git-scm.com/book/en/v2/images/lifecycle.png" width='500' />

#VSLIDE

## Git status
Shows info about tracked and untracked files

```
$ vim CONTRIBUTING.md
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

    new file:   README
    modified:   CONTRIBUTING.md

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

    modified:   CONTRIBUTING.md
```
#VSLIDE

## Git diff
Shows lines changed in modified files only

## Git diff --staged
Shows lines changed in staged files only

#VSLIDE

```
$ git diff
diff --git a/CONTRIBUTING.md b/CONTRIBUTING.md
index 8ebb991..643e24f 100644
--- a/CONTRIBUTING.md
+++ b/CONTRIBUTING.md
@@ -65,7 +65,8 @@ branch directly, things can get messy.
 Please include a nice description of your changes when you submit your PR;
 if we have to read the whole diff to figure out why you're contributing
 in the first place, you're less likely to get feedback and have your change
-merged in.
+merged in. Also, split your changes into comprehensive chunks if your patch is
+longer than a dozen lines.

 If you are starting to work on a particular area, feel free to submit a PR
 that highlights your work in progress (and note in the PR title that it's
```

#VSLIDE

## Git add
Add a file or a chunck to the staging area

#VSLIDE

## Git commit

Add a snapshot (commit) to the git repository
Launches default editor for a commitmessage

Automatically stage all tracked files (skip add)
```
$ git commit -a 
```

```
$ git commit -m 'my commit message'

```

#VSLIDE

## git rm

Remove files from staging area and working directory

To only remove from staging area, when you accidentally staged something

```
git rm --cached <filename>
```

# VSLIDE

## Git log
```
$ git log
commit ca82a6dff817ec66f44342007202690a93763949
Author: Scott Chacon <schacon@gee-mail.com>
Date:   Mon Mar 17 21:52:11 2008 -0700

    changed the version number

commit 085bb3bcb608e1e8451d4b2432f8ecbe6306e7e7
Author: Scott Chacon <schacon@gee-mail.com>
Date:   Sat Mar 15 16:40:33 2008 -0700

    removed unnecessary test

commit a11bef06a3f659402fe7563abf99ad00de2209e6
Author: Scott Chacon <schacon@gee-mail.com>
Date:   Sat Mar 15 10:31:28 2008 -0700

    first commit
```

#VSLIDE

## Git log options
```
  - -p           show diffs along with info
  - -2           only f show last 2 commits
  - --stats      show abbreviated stats with each commits
  - --pretty     =[online|short|full|fuller|format]
  - --graph      Display an ASCII graph of the branch and merge history beside the log output.
  - -S<text>     show all commits which have searchstring in changes
  - --no-merges  leave out merge commits
```

#VSLIDE

## Example 1

```
$ git log --pretty="%h - %s" --author=gitster --since="2008-10-01" --before="2008-11-01" --no-merges -- t/
5610e3b - Fix testcase failure when extended attributes are in use
acd3b9e - Enhance hold_lock_file_for_{update,append}() API
f563754 - demonstrate breakage of detached checkout with symbolic link HEAD
d1a43f2 - reset --hard/read-tree --reset -u: remove unmerged new paths
51a94af - Fix "checkout --track -b newbranch" on detached HEAD
b0ad11e - pull: allow "git pull origin $something:$current_branch" into an unborn branch
```

#VSLIDE 

## Example 2
```
$ git log --pretty=format:"%h %s" --graph
* 2d3acf9 ignore errors from SIGCHLD on trap
*  5e3ee11 Merge branch 'master' of git://github.com/dustin/grit
|\
| * 420eac9 Added a method for getting the current branch.
* | 30e367c timeout code and tests
* | 5a09431 add timeout protection to grit
* | e1193f8 support for heads with slashes in them
|/
* d6016bc require time for xmlschema
*  11d191e Merge branch 'defunkt' into local
```

#VSLIDE 

## Example 3
```
$ git log -p -2
commit ca82a6dff817ec66f44342007202690a93763949
Author: Scott Chacon <schacon@gee-mail.com>
Date:   Mon Mar 17 21:52:11 2008 -0700

    changed the version number

diff --git a/Rakefile b/Rakefile
index a874b73..8f94139 100644
--- a/Rakefile
+++ b/Rakefile
@@ -5,7 +5,7 @@ require 'rake/gempackagetask'
 spec = Gem::Specification.new do |s|
     s.platform  =   Gem::Platform::RUBY
     s.name      =   "simplegit"
-    s.version   =   "0.1.0"
+    s.version   =   "0.1.1"
     s.author    =   "Scott Chacon"
     s.email     =   "schacon@gee-mail.com"
     s.summary   =   "A simple gem for using Git in Ruby code."

commit 085bb3bcb608e1e8451d4b2432f8ecbe6306e7e7
Author: Scott Chacon <schacon@gee-mail.com>
Date:   Sat Mar 15 16:40:33 2008 -0700

    removed unnecessary test

diff --git a/lib/simplegit.rb b/lib/simplegit.rb
index a0a60ae..47c6340 100644
--- a/lib/simplegit.rb
+++ b/lib/simplegit.rb
@@ -18,8 +18,3 @@ class SimpleGit
     end

 end
-
-if $0 == __FILE__
-  git = SimpleGit.new
-  puts git.show
-end
\ No newline at end of file
```

#VSLIDE

## undoing things

Replace the previous commit with the result of the current.
When you run this command immediately after the previous commit without changing anything, this allows you to change the commit message.
```
$ git commit --amend
```

Unstage a file:
```
$ git reset HEAD mystagedfile.js
```

Unmodifying a Modified File:
```
$ git checkout -- mystagedfile.js
```

#VSLIDE

## Other commands
- git remote
- git tag
- git alias

#VSLIDE

```
$ git config --global alias.last 'log -1 HEAD'
```

```
$ git last
commit 66938dae3329c7aebe598c2246a8e6af90d04646
Author: Josh Goebel <dreamer3@example.com>
Date:   Tue Aug 26 19:48:51 2008 +0800

    test for current head

    Signed-off-by: Scott Chacon <schacon@example.com>
```


## Commits

#HSLIDE

## Branches

#HSLIDE

## Merging algoritms

#HSLIDE

## Conflicts

#HSLIDE

## Git log

#HSLIDE

## Git bisect

#HSLIDE

## Git workflows

#HSLIDE

## Documentation

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

- killer feature van Git
- instantaneous
- non-expensive operations
- multiple branch & merge activities a day

#VSLIDE

## What is a commit made of

- metadata
- working tree
- blobs with snapshots of files

<img src="https://git-scm.com/book/en/v2/images/commit-and-tree.png" />

#VSLIDE

## Commits and their parents

<img src="https://git-scm.com/book/en/v2/images/commits-and-parents.png">

#VSLIDE

## What's a branch

- lightweight pointer to a commit
- default when creating repo: master (nothing special about it)
- common misconception: a branch is a copy from my entire repository. (Like SVN, perforce)

#VSLIDE

<img src="https://git-scm.com/book/en/v2/images/branch-and-history.png">

#VSLIDE

## $ git branch testing

<img src="https://git-scm.com/book/en/v2/images/two-branches.png">

#VSLIDE

## HEAD: local current branch you're on

<img src="https://git-scm.com/book/en/v2/images/head-to-master.png">

#VSLIDE

## $ git checkout testing

<img src="https://git-scm.com/book/en/v2/images/head-to-testing.png">

#VSLIDE

## The HEAD branch moves forward when a commit is made

<img src="https://git-scm.com/book/en/v2/images/advance-testing.png">

#VSLIDE

## $ git checkout master

<img src="https://git-scm.com/book/en/v2/images/checkout-master.png">

#VSLIDE
```
$ vim test.rb
$ git commit -a -m 'made other changes'
```

<img src="https://git-scm.com/book/en/v2/images/advance-master.png">

#VSLIDE

```
$ git log --oneline --decorate --graph --all
* c2b9e (HEAD, master) made other changes
| * 87ab2 (testing) made a change
|/
* f30ab add feature #32 - ability to add new formats to the
* 34ac2 fixed bug #1328 - stack overflow under certain conditions
* 98ca9 initial commit of my project
```

#VSLIDE

Because a branch in Git is actually a simple file that contains the 40 character SHA-1 checksum of the commit it points to, branches are cheap to create and destroy. Creating a new branch is as quick and simple as writing 41 bytes to a file (40 characters and a newline).

#HSLIDE

## Merging algoritms

#VSLIDE

## Basic branching

<img src="https://git-scm.com/book/en/v2/images/basic-branching-1.png">

#VSLIDE

## $ git checkout -b iss53

<img src="https://git-scm.com/book/en/v2/images/basic-branching-2.png">

#VSLIDE

## $ git commit -a -m 'added a new footer [issue 53]'

<img src="https://git-scm.com/book/en/v2/images/basic-branching-3.png">

#VSLIDE

## Sudden bug comes up on production site

```
$ git checkout master
Switched to branch 'master'
```

```
$ git checkout -b hotfix
Switched to a new branch 'hotfix'
$ vim index.html
$ git commit -a -m 'fixed the broken email address'
[hotfix 1fb7853] fixed the broken email address
 1 file changed, 2 insertions(+)
```

#VSLIDE

## NEW GRAPH

<img src="https://git-scm.com/book/en/v2/images/basic-branching-4.png">

#VSLIDE

## Fast-forward merge

```
$ git checkout master
$ git merge hotfix
Updating f42c576..3a0874c
Fast-forward
 index.html | 2 ++
 1 file changed, 2 insertions(+)
```

#VSLIDE

## FAST FORWARD ?

When you try to merge one commit with a commit that can be reached by following the first commit’s history, 
Git simplifies things by moving the pointer forward because there is no divergent work to merge together – this is called a “fast-forward.”

#VSLIDE

<img src="https://git-scm.com/book/en/v2/images/basic-branching-5.png">

#VSLIDE

## let's go back to our issue

```
$ git branch -d hotfix
Deleted branch hotfix (3a0874c).
```

```
$ git checkout iss53
Switched to branch "iss53"
$ vim index.html
$ git commit -a -m 'finished the new footer [issue 53]'
[iss53 ad82d7a] finished the new footer [issue 53]
1 file changed, 1 insertion(+)
```

#VSLIDE

## NEW GRAPH

<img src="https://git-scm.com/book/en/v2/images/basic-branching-6.png">

#VSLIDE

## Recursive strategy

```
$ git checkout master
Switched to branch 'master'
$ git merge iss53
Merge made by the 'recursive' strategy.
index.html |    1 +
1 file changed, 1 insertion(+)
```

#VSLIDE

## Treeway merge

In this case, your development history has diverged from some older point. Because the commit on the branch you’re on isn’t a direct ancestor of the branch you’re merging in, Git has to do some work. In this case, Git does a simple three-way merge, using the two snapshots pointed to by the branch tips and the common ancestor of the two.

#VSLIDE

## Common anchestors + branch tips

<img src="https://git-scm.com/book/en/v2/images/basic-merging-1.png">

#VSLIDE

## Tadaaa! A merge commit

<img src="https://git-scm.com/book/en/v2/images/basic-merging-2.png">

#VSLIDE

## $git branch

```
$git branch
$git branch -v  // last commits on each branch
$git branch --merged // branches allreadfy merged in current
$git branch --no-merged // branches not yet merged
$git branch -d brach-you-want-to-delete

```


#HSLIDE

## Conflicts

- Can occur when you merge 2 branches which have changes in the same parts of same files.
- Git doesn't do the merging, but pauses the merging process

```
$ git merge iss53
Auto-merging index.html
CONFLICT (content): Merge conflict in index.html
Automatic merge failed; fix conflicts and then commit the result.
```

#VSLIDE

## What's our status

```
$ git status
On branch master
You have unmerged paths.
  (fix conflicts and run "git commit")

Unmerged paths:
  (use "git add <file>..." to mark resolution)

    both modified:      index.html

no changes added to commit (use "git add" and/or "git commit -a")
```

#VSLIDE

## open the file, current branch at the top

```
<<<<<<< HEAD:index.html
<div id="footer">contact : email.support@github.com</div>
=======
<div id="footer">
 please contact us at support@github.com
</div>
>>>>>>> iss53:index.html
```

#VSLIDE

## Resolve, then add, then commit

Clean up the file, then add it to the stage.
Staging a file marks it as resolved
When all files are resolved, run git commit to finalize the merge commit

#VSLIDE

## Mergetools

```
$ git mergetool

This message is displayed because 'merge.tool' is not configured.
See 'git mergetool --tool-help' or 'git help config' for more details.
'git mergetool' will now attempt to use one of the following tools:
opendiff kdiff3 tkdiff xxdiff meld tortoisemerge gvimdiff diffuse diffmerge ecmerge p4merge araxis bc3 codecompare vimdiff emerge
Merging:
index.html

Normal merge conflict for 'index.html':
  {local}: modified file
  {remote}: modified file
Hit return to start merge resolution tool (opendiff):
```

#HSLIDE

## Remote branches

Remote references are references (pointers) in your remote repositories, including branches, tags, and so on.
Show them with:
```
 $ git ls-remote //list
 $ git remote show SOMEBRANCH //some info
```

#VSLIDE
<img src="https://git-scm.com/book/en/v2/images/remote-branches-1.png">

#VSLIDE
<img src="https://git-scm.com/book/en/v2/images/remote-branches-2.png">

#VSLIDE

## Git fetch

<img src="https://git-scm.com/book/en/v2/images/remote-branches-3.png">

Git pull = git fetch + git merge

# VSLIDE

## Adding another server as a remote

<img src="https://git-scm.com/book/en/v2/images/remote-branches-4.png">

Think Github forks

## Push

```
$ git push origin serverfix
Counting objects: 24, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (15/15), done.
Writing objects: 100% (24/24), 1.91 KiB | 0 bytes/s, done.
Total 24 (delta 2), reused 0 (delta 0)
To https://github.com/schacon/simplegit
 * [new branch]      serverfix -> serverfix
```

#VSLIDE

It’s important to note that when you do a fetch that brings down new remote-tracking branches, you don’t automatically have local, editable copies of them. In other words, in this case, you don’t have a new serverfix branch – you only have an origin/serverfix pointer that you can’t modify.

To merge this work into your current working branch, you can run git merge origin/serverfix. If you want your own serverfix branch that you can work on, you can base it off your remote-tracking branch:

```
$ git checkout -b serverfix origin/serverfix
Branch serverfix set up to track remote branch serverfix from origin.
Switched to a new branch 'serverfix'
```

#VSLIDE

## Tracking branches

Checking out a local branch from a remote-tracking branch automatically creates what is called a “tracking branch” (and the branch it tracks is called an “upstream branch”). Tracking branches are local branches that have a direct relationship to a remote branch. If you’re on a tracking branch and type git pull, Git automatically knows which server to fetch from and branch to merge into.

#VSLIDE

## Tracking branches: own combinations
```
$ git checkout -b sf origin/serverfix
Branch sf set up to track remote branch serverfix from origin.
Switched to a new branch 'sf'
```

#HSLIDE

## Rebasing

#HSLIDE

## Git log

#HSLIDE

## Git bisect

#HSLIDE

## Git workflows

#HSLIDE

## Documentation

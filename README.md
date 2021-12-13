C:\Users\PC home\Git\devops-netology\branching>echo.>merge.sh

C:\Users\PC home\Git\devops-netology\branching>echo.>rebase.sh

C:\Users\PC home\Git\devops-netology\branching>git add .

C:\Users\PC home\Git\devops-netology\branching>git status
On branch main
Your branch is up to date with 'origin/main'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   merge.sh
        new file:   rebase.sh


C:\Users\PC home\Git\devops-netology\branching>git commit -m "prepare for merge and rebase"
[main 1447269] prepare for merge and rebase
 2 files changed, 16 insertions(+)
 create mode 100644 branching/merge.sh
 create mode 100644 branching/rebase.sh

C:\Users\PC home\Git\devops-netology\branching>git branch git-merge

C:\Users\PC home\Git\devops-netology\branching>git branch
  fix
  git-merge
* main

C:\Users\PC home\Git\devops-netology\branching>git checkout git-merge
Switched to branch 'git-merge'

C:\Users\PC home\Git\devops-netology\branching>git status
On branch git-merge
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   merge.sh

no changes added to commit (use "git add" and/or "git commit -a")

C:\Users\PC home\Git\devops-netology\branching>git add .

C:\Users\PC home\Git\devops-netology\branching>git status
On branch git-merge
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   merge.sh


C:\Users\PC home\Git\devops-netology\branching>git commit -m "merge: @ instead *"
[git-merge 655c850] merge: @ instead *
 1 file changed, 3 insertions(+), 3 deletions(-)

C:\Users\PC home\Git\devops-netology\branching>git push main
fatal: 'main' does not appear to be a git repository
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.

C:\Users\PC home\Git\devops-netology\branching>git push origin
fatal: The current branch git-merge has no upstream branch.
To push the current branch and set the remote as upstream, use

    git push --set-upstream origin git-merge


C:\Users\PC home\Git\devops-netology\branching>git push
fatal: The current branch git-merge has no upstream branch.
To push the current branch and set the remote as upstream, use

    git push --set-upstream origin git-merge


C:\Users\PC home\Git\devops-netology\branching>git push --all
Enumerating objects: 14, done.
Counting objects: 100% (14/14), done.
Delta compression using up to 6 threads
Compressing objects: 100% (12/12), done.
Writing objects: 100% (12/12), 1.10 KiB | 563.00 KiB/s, done.
Total 12 (delta 5), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (5/5), completed with 2 local objects.
To https://github.com/AMShamsutdinov/devops-netology.git
   8b558bb..faa7e91  fix -> fix
   ae2a2d9..1447269  main -> main
 * [new branch]      git-merge -> git-merge

C:\Users\PC home\Git\devops-netology\branching>git branch
  fix
* git-merge
  main

C:\Users\PC home\Git\devops-netology\branching>git remote
bitbucket
gitlab
origin

C:\Users\PC home\Git\devops-netology\branching>git push --all gitlub
fatal: 'gitlub' does not appear to be a git repository
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.

C:\Users\PC home\Git\devops-netology\branching>git push gitlub
fatal: 'gitlub' does not appear to be a git repository
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.

C:\Users\PC home\Git\devops-netology\branching>git push -u gitlab --all
Enumerating objects: 19, done.
Counting objects: 100% (19/19), done.
Delta compression using up to 6 threads
Compressing objects: 100% (15/15), done.
Writing objects: 100% (15/15), 1.45 KiB | 494.00 KiB/s, done.
Total 15 (delta 6), reused 0 (delta 0), pack-reused 0
remote:
remote: To create a merge request for fix, visit:
remote:   https://gitlab.com/amshamsutdinov/devops-netology/-/merge_requests/new?merge_request%5Bsource_branch%5D=fix
remote:
remote:
remote: To create a merge request for git-merge, visit:
remote:   https://gitlab.com/amshamsutdinov/devops-netology/-/merge_requests/new?merge_request%5Bsource_branch%5D=git-merge
remote:
To https://gitlab.com/amshamsutdinov/devops-netology.git
   ae2a2d9..1447269  main -> main
 * [new branch]      fix -> fix
 * [new branch]      git-merge -> git-merge
Branch 'main' set up to track remote branch 'main' from 'gitlab'.
Branch 'fix' set up to track remote branch 'fix' from 'gitlab'.
Branch 'git-merge' set up to track remote branch 'git-merge' from 'gitlab'.

C:\Users\PC home\Git\devops-netology\branching>git push -u bitbucket --all
Enumerating objects: 19, done.
Counting objects: 100% (19/19), done.
Delta compression using up to 6 threads
Compressing objects: 100% (15/15), done.
Writing objects: 100% (15/15), 1.45 KiB | 494.00 KiB/s, done.
Total 15 (delta 6), reused 0 (delta 0), pack-reused 0
To https://bitbucket.org/amshamsutdinov/devops-netology.git
   ae2a2d9..1447269  main -> main
 * [new branch]      fix -> fix
 * [new branch]      git-merge -> git-merge
Branch 'main' set up to track remote branch 'main' from 'bitbucket'.
Branch 'fix' set up to track remote branch 'fix' from 'bitbucket'.
Branch 'git-merge' set up to track remote branch 'git-merge' from 'bitbucket'.

C:\Users\PC home\Git\devops-netology\branching>git checkout main
Switched to branch 'main'
Your branch is up to date with 'bitbucket/main'.

C:\Users\PC home\Git\devops-netology\branching>git status
On branch main
Your branch is up to date with 'bitbucket/main'.

nothing to commit, working tree clean

C:\Users\PC home\Git\devops-netology\branching>git push -u origin --all
Everything up-to-date
Branch 'fix' set up to track remote branch 'fix' from 'origin'.
Branch 'git-merge' set up to track remote branch 'git-merge' from 'origin'.
Branch 'main' set up to track remote branch 'main' from 'origin'.

C:\Users\PC home\Git\devops-netology\branching>git status
On branch main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean

C:\Users\PC home\Git\devops-netology\branching>git checkout main
Already on 'main'
Your branch is up to date with 'origin/main'.

C:\Users\PC home\Git\devops-netology\branching>git remote gitlab
error: Unknown subcommand: gitlab
usage: git remote [-v | --verbose]
   or: git remote add [-t <branch>] [-m <master>] [-f] [--tags | --no-tags] [--mirror=<fetch|push>] <name> <url>
   or: git remote rename <old> <new>
   or: git remote remove <name>
   or: git remote set-head <name> (-a | --auto | -d | --delete | <branch>)
   or: git remote [-v | --verbose] show [-n] <name>
   or: git remote prune [-n | --dry-run] <name>
   or: git remote [-v | --verbose] update [-p | --prune] [(<group> | <remote>)...]
   or: git remote set-branches [--add] <name> <branch>...
   or: git remote get-url [--push] [--all] <name>
   or: git remote set-url [--push] <name> <newurl> [<oldurl>]
   or: git remote set-url --add <name> <newurl>
   or: git remote set-url --delete <name> <url>

    -v, --verbose         be verbose; must be placed before a subcommand


C:\Users\PC home\Git\devops-netology\branching>
                                               git checkout git-merge
Switched to branch 'git-merge'
Your branch is up to date with 'origin/git-merge'.

C:\Users\PC home\Git\devops-netology\branching>git add merge.sh

C:\Users\PC home\Git\devops-netology\branching>git status
On branch git-merge
Your branch is up to date with 'origin/git-merge'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   merge.sh


C:\Users\PC home\Git\devops-netology\branching>git commit -m "merge: use shift"
[git-merge e4909a5] merge: use shift
 1 file changed, 3 insertions(+), 2 deletions(-)

C:\Users\PC home\Git\devops-netology\branching>git push -u origin
Enumerating objects: 7, done.
Counting objects: 100% (7/7), done.
Delta compression using up to 6 threads
Compressing objects: 100% (4/4), done.
Writing objects: 100% (4/4), 465 bytes | 465.00 KiB/s, done.
Total 4 (delta 1), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
To https://github.com/AMShamsutdinov/devops-netology.git
   655c850..e4909a5  git-merge -> git-merge
Branch 'git-merge' set up to track remote branch 'git-merge' from 'origin'.

C:\Users\PC home\Git\devops-netology\branching>git checkout main
Switched to branch 'main'
Your branch is up to date with 'origin/main'.

C:\Users\PC home\Git\devops-netology\branching>git add rebase.sh

C:\Users\PC home\Git\devops-netology\branching>git status
On branch main
Your branch is up to date with 'origin/main'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   rebase.sh


C:\Users\PC home\Git\devops-netology\branching>git commit -m "first change rebase"
[main 4a6f03f] first change rebase
 1 file changed, 4 insertions(+), 2 deletions(-)

C:\Users\PC home\Git\devops-netology\branching>git status
On branch main
Your branch is ahead of 'origin/main' by 1 commit.
  (use "git push" to publish your local commits)

nothing to commit, working tree clean

C:\Users\PC home\Git\devops-netology\branching>git push -u origin
Enumerating objects: 7, done.
Counting objects: 100% (7/7), done.
Delta compression using up to 6 threads
Compressing objects: 100% (4/4), done.
Writing objects: 100% (4/4), 399 bytes | 399.00 KiB/s, done.
Total 4 (delta 2), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (2/2), completed with 2 local objects.
To https://github.com/AMShamsutdinov/devops-netology.git
   1447269..4a6f03f  main -> main
Branch 'main' set up to track remote branch 'main' from 'origin'.

C:\Users\PC home\Git\devops-netology\branching>git log --oneline
4a6f03f (HEAD -> main, origin/main, origin/HEAD) first change rebase
1447269 (gitlab/main, bitbucket/main) prepare for merge and rebase
ae2a2d9 (tag: v0.1, tag: v0.0) Testing
c053d76 (gitlab/origin/main) Moved and deleted
34d577e Added gitignore
9515f6a Prepare to delete and move
d03fd72 First commit
f8165aa Initial commit

C:\Users\PC home\Git\devops-netology\branching>git checkout 1447269
Note: switching to '1447269'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by switching back to a branch.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -c with the switch command. Example:

  git switch -c <new-branch-name>

Or undo this operation with:

  git switch -

Turn off this advice by setting config variable advice.detachedHead to false

HEAD is now at 1447269 prepare for merge and rebase

C:\Users\PC home\Git\devops-netology\branching>git branch git-rebase

C:\Users\PC home\Git\devops-netology\branching>git status
HEAD detached at 1447269
nothing to commit, working tree clean

C:\Users\PC home\Git\devops-netology\branching>git brach
git: 'brach' is not a git command. See 'git --help'.

The most similar command is
        branch

C:\Users\PC home\Git\devops-netology\branching>git branch
* (HEAD detached at 1447269)
  fix
  git-merge
  git-rebase
  main

C:\Users\PC home\Git\devops-netology\branching>git add .

C:\Users\PC home\Git\devops-netology\branching>git status
HEAD detached at 1447269
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   rebase.sh


C:\Users\PC home\Git\devops-netology\branching>git checkout git-rebase
Switched to branch 'git-rebase'
M       branching/rebase.sh

C:\Users\PC home\Git\devops-netology\branching>git status
On branch git-rebase
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   rebase.sh


C:\Users\PC home\Git\devops-netology\branching>git commit -m "git-rebase 1"
[git-rebase 9f829a9] git-rebase 1
 1 file changed, 4 insertions(+), 2 deletions(-)

C:\Users\PC home\Git\devops-netology\branching>git add .

C:\Users\PC home\Git\devops-netology\branching>git commit -m "git-rebase 2"
[git-rebase b5279aa] git-rebase 2
 1 file changed, 1 insertion(+), 1 deletion(-)

C:\Users\PC home\Git\devops-netology\branching>git push -u origin
fatal: The current branch git-rebase has no upstream branch.
To push the current branch and set the remote as upstream, use

    git push --set-upstream origin git-rebase


C:\Users\PC home\Git\devops-netology\branching>git push -u origin git-rebase
Enumerating objects: 11, done.
Counting objects: 100% (11/11), done.
Delta compression using up to 6 threads
Compressing objects: 100% (8/8), done.
Writing objects: 100% (8/8), 780 bytes | 390.00 KiB/s, done.
Total 8 (delta 3), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (3/3), completed with 1 local object.
remote:
remote: Create a pull request for 'git-rebase' on GitHub by visiting:
remote:      https://github.com/AMShamsutdinov/devops-netology/pull/new/git-rebase
remote:
To https://github.com/AMShamsutdinov/devops-netology.git
 * [new branch]      git-rebase -> git-rebase
Branch 'git-rebase' set up to track remote branch 'git-rebase' from 'origin'.

C:\Users\PC home\Git\devops-netology\branching>git checkout main
Switched to branch 'main'
Your branch is up to date with 'origin/main'.

C:\Users\PC home\Git\devops-netology\branching> git merge git-merge
Merge made by the 'ort' strategy.
 branching/merge.sh | 7 ++++---
 1 file changed, 4 insertions(+), 3 deletions(-)

C:\Users\PC home\Git\devops-netology\branching> git push
Enumerating objects: 7, done.
Counting objects: 100% (7/7), done.
Delta compression using up to 6 threads
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 369 bytes | 369.00 KiB/s, done.
Total 3 (delta 1), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
To https://github.com/AMShamsutdinov/devops-netology.git
   4a6f03f..072d21e  main -> main

C:\Users\PC home\Git\devops-netology\branching>git checkout git-rebase
Switched to branch 'git-rebase'
Your branch is up to date with 'origin/git-rebase'.

C:\Users\PC home\Git\devops-netology\branching>git rebase -i main
hint: Waiting for your editor to close the file... emacs*: emacs*: command not found
error: There was a problem with the editor 'emacs*'.

C:\Users\PC home\Git\devops-netology\branching>git rebase -i main
hint: Waiting for your editor to close the file... emacs*: emacs*: command not found
error: There was a problem with the editor 'emacs*'.

C:\Users\PC home\Git\devops-netology\branching>git config --global core.editor "'C:\Program Files (x86)\Notepad++\notepad++.exe' -multiInst -notabbar -nosession -noPlugin"

C:\Users\PC home\Git\devops-netology\branching>git rebase -i main
error: invalid line 4:  f, fixup [-C | -c] <commit> = like "squash" but keep only the previous
You can fix this with 'git rebase --edit-todo' and then run 'git rebase --continue'.
Or you can abort the rebase with 'git rebase --abort'.

C:\Users\PC home\Git\devops-netology\branching>git rebase -i main
fatal: It seems that there is already a rebase-merge directory, and
I wonder if you are in the middle of another rebase.  If that is the
case, please try
        git rebase (--continue | --abort | --skip)
If that is not the case, please
        rm -fr ".git/rebase-merge"
and run me again.  I am stopping in case you still have something
valuable there.


C:\Users\PC home\Git\devops-netology\branching>git rebase --skip
error: invalid line 11:  f, fixup [-C | -c] <commit> = like "squash" but keep only the previous
error: please fix this using 'git rebase --edit-todo'.

C:\Users\PC home\Git\devops-netology\branching>git rebase --edit-todo
error: invalid line 4:  f, fixup [-C | -c] <commit> = like "squash" but keep only the previous

C:\Users\PC home\Git\devops-netology\branching>
C:\Users\PC home\Git\devops-netology\branching>git rebase -i main
fatal: It seems that there is already a rebase-merge directory, and
I wonder if you are in the middle of another rebase.  If that is the
case, please try
        git rebase (--continue | --abort | --skip)
If that is not the case, please
        rm -fr ".git/rebase-merge"
and run me again.  I am stopping in case you still have something
valuable there.


C:\Users\PC home\Git\devops-netology\branching>git status
interactive rebase in progress; onto 072d21e
No commands done.
Next commands to do (2 remaining commands):
   pick 9f829a9 git-rebase 1
   pick b5279aa git-rebase 2
  (use "git rebase --edit-todo" to view and edit)
You are currently editing a commit while rebasing branch 'git-rebase' on '072d21e'.
  (use "git commit --amend" to amend the current commit)
  (use "git rebase --continue" once you are satisfied with your changes)

nothing to commit, working tree clean

C:\Users\PC home\Git\devops-netology\branching>git rebase --edit-todo
error: invalid line 3:     git rebase --continue
You can fix this with 'git rebase --edit-todo' and then run 'git rebase --continue'.
Or you can abort the rebase with 'git rebase --abort'.

C:\Users\PC home\Git\devops-netology\branching>git rebase --continue
error: invalid line 29:     git rebase --continue
error: please fix this using 'git rebase --edit-todo'.

C:\Users\PC home\Git\devops-netology\branching>git rebase --edit-todo
error: invalid line 3:     git rebase --continue

C:\Users\PC home\Git\devops-netology\branching>git rebase --continue
Auto-merging branching/rebase.sh
CONFLICT (content): Merge conflict in branching/rebase.sh
error: could not apply 9f829a9... git-rebase 1
hint: Resolve all conflicts manually, mark them as resolved with
hint: "git add/rm <conflicted_files>", then run "git rebase --continue".
hint: You can instead skip this commit: run "git rebase --skip".
hint: To abort and get back to the state before "git rebase", run "git rebase --abort".
Could not apply 9f829a9... git-rebase 1

C:\Users\PC home\Git\devops-netology\branching>git add rebase.sh

C:\Users\PC home\Git\devops-netology\branching>git rebase --continue
Auto-merging branching/rebase.sh
CONFLICT (content): Merge conflict in branching/rebase.sh
error: could not apply b5279aa... git-rebase 2
hint: Resolve all conflicts manually, mark them as resolved with
hint: "git add/rm <conflicted_files>", then run "git rebase --continue".
hint: You can instead skip this commit: run "git rebase --skip".
hint: To abort and get back to the state before "git rebase", run "git rebase --abort".
Could not apply b5279aa... git-rebase 2

C:\Users\PC home\Git\devops-netology\branching>git rebase --continue
branching/rebase.sh: needs merge
You must edit all merge conflicts and then
mark them as resolved using git add

C:\Users\PC home\Git\devops-netology\branching>git add rebase.sh

C:\Users\PC home\Git\devops-netology\branching>git rebase --continue
[detached HEAD 6e8973e] git-rebase 2 fixup
 1 file changed, 1 insertion(+), 1 deletion(-)
Successfully rebased and updated refs/heads/git-rebase.

C:\Users\PC home\Git\devops-netology\branching>git status
On branch git-rebase
Your branch and 'origin/git-rebase' have diverged,
and have 5 and 2 different commits each, respectively.
  (use "git pull" to merge the remote branch into yours)

nothing to commit, working tree clean

C:\Users\PC home\Git\devops-netology\branching>git push origin git-rebase
To https://github.com/AMShamsutdinov/devops-netology.git
 ! [rejected]        git-rebase -> git-rebase (non-fast-forward)
error: failed to push some refs to 'https://github.com/AMShamsutdinov/devops-netology.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Integrate the remote changes (e.g.
hint: 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.

C:\Users\PC home\Git\devops-netology\branching>git push origin
To https://github.com/AMShamsutdinov/devops-netology.git
 ! [rejected]        git-rebase -> git-rebase (non-fast-forward)
error: failed to push some refs to 'https://github.com/AMShamsutdinov/devops-netology.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Integrate the remote changes (e.g.
hint: 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.

C:\Users\PC home\Git\devops-netology\branching>git log --graph --all
* commit 6e8973ea53af3c96eb16152b03897159204b0762 (HEAD -> git-rebase)
| Author: Artem Shamsutdinov <AMShamsutdinov@gmail.com>
| Date:   Tue Dec 14 01:25:46 2021 +0500
|
|     git-rebase 2 fixup
|
*   commit 072d21e1e75f8adeb1a0b9a7317756ae259bc25f (origin/main, origin/HEAD, main)
|\  Merge: 4a6f03f e4909a5
| | Author: Artem Shamsutdinov <AMShamsutdinov@gmail.com>
| | Date:   Tue Dec 14 01:47:33 2021 +0500
| |
| |     Merge branch 'git-merge'
| |
| * commit e4909a5c3a97637d84edf4b41c85a419fc125603 (origin/git-merge, git-merge)
| | Author: Artem Shamsutdinov <AMShamsutdinov@gmail.com>
| | Date:   Tue Dec 14 01:10:52 2021 +0500
| |
| |     merge: use shift
| |
| * commit 655c850965658f2417e498b8d7eb0266d35882ed (gitlab/git-merge, bitbucket/git-merge)
| | Author: Artem Shamsutdinov <AMShamsutdinov@gmail.com>
| | Date:   Tue Dec 14 00:48:01 2021 +0500
| |
| |     merge: @ instead *
| |
* | commit 4a6f03f9ae48858c7bbc11ac839afe8350281ea2
|/  Author: Artem Shamsutdinov <AMShamsutdinov@gmail.com>
|   Date:   Tue Dec 14 01:16:11 2021 +0500
|

C:\Users\PC home\Git\devops-netology\branching>git checkout main
Switched to branch 'main'
Your branch is up to date with 'origin/main'.

C:\Users\PC home\Git\devops-netology\branching>git merge git-rebase
Updating 072d21e..6e8973e
Fast-forward
 branching/rebase.sh | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)

C:\Users\PC home\Git\devops-netology\branching>git push origin
Enumerating objects: 7, done.
Counting objects: 100% (7/7), done.
Delta compression using up to 6 threads
Compressing objects: 100% (4/4), done.
Writing objects: 100% (4/4), 399 bytes | 399.00 KiB/s, done.
Total 4 (delta 2), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (2/2), completed with 2 local objects.
To https://github.com/AMShamsutdinov/devops-netology.git
   072d21e..6e8973e  main -> main

C:\Users\PC home\Git\devops-netology\branching>git log --graph --all
* commit 6e8973ea53af3c96eb16152b03897159204b0762 (HEAD -> main, origin/main, origin/HEAD, git-rebase)
| Author: Artem Shamsutdinov <AMShamsutdinov@gmail.com>
| Date:   Tue Dec 14 01:25:46 2021 +0500
|
|     git-rebase 2 fixup
|
*   commit 072d21e1e75f8adeb1a0b9a7317756ae259bc25f
|\  Merge: 4a6f03f e4909a5
| | Author: Artem Shamsutdinov <AMShamsutdinov@gmail.com>
| | Date:   Tue Dec 14 01:47:33 2021 +0500
| |
| |     Merge branch 'git-merge'
| |
| * commit e4909a5c3a97637d84edf4b41c85a419fc125603 (origin/git-merge, git-merge)
| | Author: Artem Shamsutdinov <AMShamsutdinov@gmail.com>
| | Date:   Tue Dec 14 01:10:52 2021 +0500
| |
| |     merge: use shift
| |
| * commit 655c850965658f2417e498b8d7eb0266d35882ed (gitlab/git-merge, bitbucket/git-merge)
| | Author: Artem Shamsutdinov <AMShamsutdinov@gmail.com>
| | Date:   Tue Dec 14 00:48:01 2021 +0500
| |
| |     merge: @ instead *
| |
* | commit 4a6f03f9ae48858c7bbc11ac839afe8350281ea2
|/  Author: Artem Shamsutdinov <AMShamsutdinov@gmail.com>
|   Date:   Tue Dec 14 01:16:11 2021 +0500
|

C:\Users\PC home\Git\devops-netology\branching>git push --all
To https://github.com/AMShamsutdinov/devops-netology.git
 ! [rejected]        git-rebase -> git-rebase (non-fast-forward)
error: failed to push some refs to 'https://github.com/AMShamsutdinov/devops-netology.git'
hint: Updates were rejected because a pushed branch tip is behind its remote
hint: counterpart. Check out this branch and integrate the remote changes
hint: (e.g. 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.

C:\Users\PC home\Git\devops-netology\branching>git push -u main git push
error: src refspec git does not match any
error: src refspec push does not match any
error: failed to push some refs to 'main'

C:\Users\PC home\Git\devops-netology\branching>git push -u main git-rebase
fatal: 'main' does not appear to be a git repository
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.

C:\Users\PC home\Git\devops-netology\branching>git push -u origin git-rebase
To https://github.com/AMShamsutdinov/devops-netology.git
 ! [rejected]        git-rebase -> git-rebase (non-fast-forward)
error: failed to push some refs to 'https://github.com/AMShamsutdinov/devops-netology.git'
hint: Updates were rejected because a pushed branch tip is behind its remote
hint: counterpart. Check out this branch and integrate the remote changes
hint: (e.g. 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.

C:\Users\PC home\Git\devops-netology\branching>git push -u origin git-rebase -f
Total 0 (delta 0), reused 0 (delta 0), pack-reused 0
To https://github.com/AMShamsutdinov/devops-netology.git
 + b5279aa...6e8973e git-rebase -> git-rebase (forced update)
Branch 'git-rebase' set up to track remote branch 'git-rebase' from 'origin'.

C:\Users\PC home\Git\devops-netology\branching>git checkout main
Already on 'main'
Your branch is up to date with 'origin/main'.

C:\Users\PC home\Git\devops-netology\branching>git merge git-rebase
Already up to date.

C:\Users\PC home\Git\devops-netology\branching>
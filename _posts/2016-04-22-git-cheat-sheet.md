---
inFeed: true
hasPage: true
inNav: false
inLanguage: null
keywords: []
description: ''
datePublished: '2016-04-27T05:28:52.846Z'
dateModified: '2016-04-27T05:28:50.387Z'
title: ''
author: []
sourcePath: _posts/2016-04-22-git-cheat-sheet.md
published: true
authors: []
publisher:
  name: null
  domain: null
  url: null
  favicon: null
starred: false
url: git-cheat-sheet/index.html
_type: Article

---
__
![](https://the-grid-user-content.s3-us-west-2.amazonaws.com/492bee29-0e46-48e1-ba0d-cfaae028d685.jpg)

__

--

_Set up a global username_

git config --global user.name "Billy Everyteen"

_Init rep_

git init

_check status_

git status

_Track a file_

git add octopus.txt

_Commit a change_

git commit -m "Add cute octocat story"

_Add all txt files_

git add '\*.txt'

_Show all the changes committed so far, in order_

git log

_Add a remote repository_

git remote add repName

[https://github.com/try-git/try\_git.git][0]_Remove a remote repository_

git remote rm repName

_Push our changes to the remote rep_

git push -u origin master

_Check for changes on our GitHub repository and pull down any new changes_

git pull origin master

_Look at what is different from our last commit_

git diff HEAD

_See the changes you just staged_

git diff --staged

_Unstage files_

git reset octofamily/octodog.txt

_Remove file from the head (head=latest commit)_

git reset HEAD filename

_Change file back to how they were at the last commit_

git checkout -- octocat.txt

_Create a branch_

git branch clean\_up

_List the branches_

git branch

_Switch branch_

git checkout clean\_up

Remove the actual files from disk, and stage the removal of the files

git rm '\*.txt'

_Switch Back to master_

git checkout master

_Tell Git to merge the clean\_up branch into master (when you have already checkout master)_

git merge clean\_up

_Delete a branch_

git branch -d clean\_up

_Push on the remote repository (when it's already checked out)_

git push

_Undo the commit and put everything back into staging (it means move to the commit before HEAD)_

git reset ---soft HEAD^

_Add a file to the latest check in_

git add filename

git commit ---amend -m "added an extra file"

_Undo last commit and all changes_

git reset ---hard HEAD^

_Undo 2 last commit2 and all changes_

git reset ---hard HEAD^^

_List all the remote reps_

git remote -v

_Push to a specific remote repository_

git push -u origin master

_Add and commit in one step_

git commit -m "single step commit" -a index.html

_Merge a branch "cat" into master_

git checkout master

git merge cat

_Create a new branch "admin" and check it out_

git checkout -b admin

_Merge commit (starts the editor when there are conflicts)_

git commit -a

_Create a remote branch from a local one "shopping\_cart" (tracks the branch)_

git push origin shopping\_cart

_List all the remote branches_

git branch -r

_Get and start working on a remote repository (gets copied and created locally)_

git checkout shopping\_cart

_Show local and remote branches and whether they are tracked or not_

git remote show origin

_Remove a remote branch_

git push origin :shopping\_cart

_Remove a local branch (will stop in pending commits)_

git branch -d :shopping\_cart

_Remove a local branch (will not stop in pending commits)_

git branch -D :shopping\_cart

_Clean up local branches that do not match any remote branch_

git remote prune origin

_Follow the changes of a file whose name has changed_

git log --follow

_List all the tags_

git tag

_Checkout a commit with a specific tag_

git checkout tagName

_Add a tag name_

git tag -a tagName -m "Tag comment"

_Push new tags to remote_

git push ---tags

_Fetch the changes without merging_

git fetch

_Local branch rebase (merge without merge commit) with branch admin_

git checkout admin

gir rebase admin

git checkout master

git merge admin (will do a fast forward merge)

_Abort the rebase_

git rebase abort

_Continue the rebased stopped due to conflicts_

git rebase --continue

_Activate the commit color_

git config --- global color.ui true

_Show one line log with how many insertion/deletion for each commit_

git log ---oneline stat

_Show a graphical representation of the merge_

git log ---oneline graph

_Specify date filter in the log_

git log ---until=1.minute ago

git log ---since=1.day ago

_Get diff between two commits (uses SHA)_

git diff SHA1..SHA2

_Get diff between two branches_

git diff branch1 branch2

_Get all the differences in one file ordered by date_

git blame index.html --date short

_Exclude a directory from git tracking (can filter also by extension or specific files)_

Put the directory in the .git/info/exclude file

_Remove file from rep_

git rm fileName

_Untrack file_

git rm ---cached filaName

_Use emacs for interactive commands_

git config ---global core.editor emacs

_Create an alias for a log format_

And then use them with git mylog or git lol

_More aliases__Switch commits_

git rebase -i commitSha (or HEAD or HEAD^, etc)

Then swap the commit order (they are shown from oldest to newest in the editor)

_Change commit message_

git rebase -i commitSha (or HEAD or HEAD^, etc)

Change the pick command for the commit to reword and change the message

_Split one commit into two_

git rebase -i commitSha (or HEAD or HEAD^, etc)

Change the key from pick to edit

(We can add more code by staging files and running git commit ---amend and git rebase ---continue)

To split the commit we need to run git reset HEAD^(to undo the commit that has been replayed).

Then we can stage one file, commit it, stage the other and commit it. And run got git rebase ---continue

_Merge different commits_

git rebase -i commitSha (or HEAD or HEAD^, etc)

Modify the pick keyword to squash (the commit will be squashed into the one above).

Change and exit the commit message

_Stash a change_

git stash save (save them in a temporary area - stash stack-, and restore the area from the previous commit)

We can meanwhile work on another branch doing git checkout otherBranch and git pull safely

You can have multiple stashes

_Resume the stash_

git checkout branchWhereTheStashIs

git stash apply

_Get a list of all stashes_

git stash list

(git stash apply will get the first stash in the list, if no name is supplied)

_Remove a stash from the list_

git stash drop

_Stash the state, but not the staging area_

git stash save ---keep-index (will stash only the unstaged files)

_Stash also the unstaged files_

git stash save ---include-untracked

_Get a detailed list of stashes_

git stash list ---stat

_Get details about a stash_

git stash show stash@{0}

_Show the file changes within the stash_

git stash show ---patch

_Add a stash message to the stash_

git stash save "stashing message"

_Create a new branch from a stash_

git stash branch branchName stash@{0}

_Remove all the stashes_

git stash clear

_Purge history in all commits, in all the branches_

git filter-branch ---tree-filter shellScript --- ---all

(e.g. git filter-branch ---tree-filter 'rm -f password.txt' --- ---all

_Purge history in all commits, in the current branch_

git filter-branch ---tree-filter 'rm -f password.txt' --- ---HEAD

_Purge history in all commits, in the current branch on the staging area (does not do a checkout first)_

git filter-branch --index-filter 'git rm --cached --ignore-unmatch master\_password.txt' HEAD (command must operate on the staging area-must be a git command)

_Remove empty commits_

git filter-branch --tree-filter --prune-empty -f HEAD

_On Unix: Change CR/LF to LF on commit (make Unix and Windows commits to work together)_

git config ---global core.autocrlf input

_On Windows: Changes LF to CR/LF on checkout and converts back to LF on commit_

git config ---global core.autocrlf true

_Pick a specific commit from another branch, not picking the others before and after_

git checkout mainBranch

git cherry-pick commitHASH

_Take commits from other branch, merge them without committing_

git cherry-pick --no-commit commit1Hash commit2Hash

_Show where the cherry picked commit is coming from_

git cherry-pick -x commitHASH (only useful on public repo)

_Show who committed the cherry picked commit_

git cherry-pick --signoff commitHASH (only useful on public repo)

_Change the commit message when cherry picking_

git cherry-pick --edit HASH

_Add a submodule to a project_

git submodule add git@example.com:css.git

git commit - "added submodule"

git push

_Update a submodule_

cd subModule

git checkout master

git commit -am "update in the submodule"

git push

then update the parent repo:

cd ..

git add subModule

git commit -am "update in the submodule from the parent repo"

git push

_When the submodules are already existing_

clone the parent project

git submodule init

git submodule update

_Pull the differences of the submodules_

git submodule update

_Modify submodules_

git checkout master

git merge commitHash (of the orphaned commit in the submodule)

_To avoid forgetting to push twice (submodules + parent repo)_

git push ---recurse-submodules=check (will abort the push if the submodules have not been pushed yet)

git push ---recurse-submodules=on-demand (push also the submodules)

_See also the deleted commits_

git reflog

_Restore a deleted commit_

git reset -hard SHADeletedCommit

_Get extra information about the reflows_

git log ---walk-reflogs

_Recreate a deleted branch from a reflog entry_

git branch branchName SHADeletedCommitg

_Move the changes to the right branch, after having made changes on the wrong one_

git stash

git checkout branch123

git stash apply

_Add all the content of a folder_

git add foldername/\\\\\*

ALIASES

c = commit -m

a = add

aa= !git add -u && git add . && git status

cob = checkout -b

up = !git fetch origin && git rebase origin/master

ir = !git rebase -i origin/master

done = !git fetch && git rebase origin/master && git checkout master && git merge @{-1} && rake && git push

ca = commit --amend -C HEAD

you want to commit changes as part of the prior commit, and want to keep the original commit message. Similar to creating a new commit and then squashing them.

rmb = !sh -c 'git branch -D $1 && git push origin :$1' -

Remove local and remote branch

who = shortlog -n -s --no-merges

Check who contributed to the project

cleanup = !git remote prune origin && git gc && git clean -dfx && git stash clear

remove remote branch references that no longer exist, cleanup unnecessary git files, remove untracked files from the working tree and clear out your stash

[0]: https://github.com/try-git/try_git.git
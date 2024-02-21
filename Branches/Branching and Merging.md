Given that you have a repository with some commits and an issue pops up, you create an issue branch and do some work:

![[Initial repository.png]]![[Repository with an issue branch.png]]
After that, you realize that there is a bug in `master`, and so you `git checkout master`, `git checkout -b hotfix` and resolve the bug:

![[hotfix branch.png]]
After resolving it, you then `git checkout master` and `git merge hotfix` to update the master on that hotfix:
![[master and hotfix merge.png]]

After quickly resolving that bug, you delete the hotfix branch and return to the previous issue. After another commit on that branch, the commit tree is looking something like this:

![[new commit issue.png]]

Note that none of the changes done during the `hotfix ` occurred in the `iss53` branch. After you're done working on that issue, you decide to `git merge master` (while you're on the `iss53` branch, of course), git uses the trick of creating a new snapshot that encompasses both branches, to look like:
![[three-way merge.png]]

![[after three way merge.png]]
This way, the `iss53` branch is no longer required, and you can delete it.


### Merge conflicts
#### Basic Merge Conflicts

In the previous scenario, all merges went smoothly because there were no conflicts between the branches. Imagine that between the last merge, you changed both files in the `master` (previously `hotfix`) and in the `iss53` branch. And it so happens to be the same function. How does Git know which change to accept as the one that you want to use? Or perhaps there is a combination of changes within the same function that you want to integrate with one another, but in those separate branches. In that case, Git starts a merge conflict, which interrupts the merge conflict.

In order to proceed, you will have to choose which file's contents you want to keep, change or remove from both commits. Something like this:

```console
$ git merge iss53
Auto-merging index.html
CONFLICT (content): Merge conflict in index.html
Automatic merge failed; fix conflicts and then commit the result.

$ git status
On branch master
You have unmerged paths.
  (fix conflicts and run "git commit")

Unmerged paths:
  (use "git add <file>..." to mark resolution)

    both modified:      index.html

no changes added to commit (use "git add" and/or "git commit -a")
```

Anything that has merge conflicts and hasn’t been resolved is listed as unmerged. Git adds standard conflict-resolution markers to the files that have conflicts, so you can open them manually and resolve those conflicts. Your file contains a section that looks something like this:

```console
<<<<<<< HEAD:index.html
<div id="footer">contact : email.support@github.com</div>
=======
<div id="footer">
 please contact us at support@github.com
</div>
>>>>>>> iss53:index.html
```

The example above can be solved like:
```html
<div id="footer">
please contact us at email.support@github.com
</div>
```

Which as you can see is not equal to any of the previous commits, but rather an amalgamation. You will also notice that `<<<<<<<`, `=======` and `>>>>>>>` lines have been removed. Since these are the indicators for the merge conflict, you can now `git add` the file(s) in question:

```console
$ git status
On branch master
All conflicts fixed but you are still merging.
  (use "git commit" to conclude merge)

Changes to be committed:

    modified:   index.html
```

As you can see, the merging conflict has been resolved, and you can now `git commit` to finalize it:

```console
Merge branch 'iss53'

Conflicts:
    index.html
#
# It looks like you may be committing a merge.
# If this is not correct, please remove the file
#	.git/MERGE_HEAD
# and try again.


# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
# On branch master
# All conflicts fixed but you are still merging.
#
# Changes to be committed:
#	modified:   index.html
#
```
If you wish to explain to others in the future how you solved this merge conflict, you can explain them in text without `#` before the line so that it doesn't become a comment.

#### Managing Branches

To list your current branches, `git branch` without any arguments might return:

```console
$ git branch
* master
  iss53
  testing
```

Where the asterisk shows which branch you are currently on, or that the branch `HEAD` points to. To see which commits are on what branch, use the `-v` flag:

```console
$ git branch -v
  iss53   93b412c Fix javascript issue
* master  7a98805 Merge branch 'iss53'
  testing 782fd34 Add scott to the author list in the readme
```

Or to list merged and non-merged branches, `--merged` and `--no-merged`:

```console
$ git branch --merged
  iss53
* master
```

Because you already merged in `iss53` earlier, you see it in your list. Branches on this list without the `*` in front of them are generally fine to delete with `git branch -d`; you’ve already incorporated their work into another branch, so you’re not going to lose anything.

To see all the branches that contain work you haven’t yet merged in, you can run `git branch --no-merged`:

```console
$ git branch --no-merged
  testing
```

This means that `testing` is being merged, and trying to delete it won't work with the `-d` flag, but rather the `-D` flag. This is to prevent accidentally deleting a commit that is being merged and losing that work.

#### Changing a branch name

`git branch --move old_branch_name new_branch_name` renames locally the `old_branch_name` to `new_branch_name`, meaning that to apply that change in the remote server, you'll have to `git push --set-upstream origin new_branch_name`. Doing `git branch --all`:

```console
$ git branch --all
* new_branch_name
  main
  remotes/origin/old_branch_name
  remotes/origin/new_branch_name
  remotes/origin/main
```

So one last step is to `git push origin --delete old_branch_name`.

#### Changing the master branch name

Be careful before doing this! You can break integrations, services, helper utilities and even build/release scripts that your repository uses. Before doing this, consult your collaborators and make sure to update all the references from the old main branch to the new main branch.

Lets say that you want to change `master` to `main`. You start by `git branch --move master main` to change it locally, and then `gitt push --set-upstream origin main`. Finally, you can `git push origin --delete master`.


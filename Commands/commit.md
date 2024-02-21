After changing files, staging them or removing them, the changes have to be commited. Files that weren't added with [[add|`git add`]] will not be commited. They'll stay modified in the disk, but not go into this commit.

To commit the changes, do `git commit`.  Now, your editor of choice must pop up. (This is set by your shell’s `EDITOR` environment variable — usually vim or emacs, although you can configure it with whatever you want using the `--global core.editor EDITOR` with [[config|git config]]).

The editor displays the following text (this example is a Vim screen):

```
# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
# On branch master
# Your branch is up-to-date with 'origin/master'.
#
# Changes to be committed:
#	new file:   README
#	modified:   CONTRIBUTING.md
#
~
~
~
```

When you exit the editor, Git creates your commit with that commit message (with the comments and diff stripped out).

Alternatively, you can type your commit message inline with the `commit` command by specifying it after a `-m` flag, like this:

```console
$ git commit -m "Story 182: fix benchmarks for speed"
[master 463dc4f] Story 182: fix benchmarks for speed
 2 files changed, 2 insertions(+)
 create mode 100644 README
```

If you want to just add all the files to the next commit, you can use the `-a` flag in the `git commit` command to skip the [[add|git add]] part.

## Undoing things

#### Changing a commit

Let's say you have staged files on a commit but you don't want them there or messed up the commit message, then you can use `--ammend` so that you can add or remove files in staging and the result of the second commit replaces the first.

#### Unstaging a staged file

So you `git add *` and then git status shows a staged file that you didn't want there. `git reset HEAD <file>` allows you to unstage. This removes modified files but doesn't remove renamed ones.

#### Unmodifying a modified file

`git checkout -- <file>` reverts a modified file until the previous commit
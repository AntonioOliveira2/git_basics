A file on a given repository has four possible states:

> Untracked, when a file exists on a repository but isn't being tracked;
> Unmodified, when a file is being tracked, but has no changes compared to its previous version;
> Modified, when a file that was previously tracked and has been changed.
> Staged, When a file has been added to be commited.

Unmodified, modified and staged files are all tracked files. 

In order to check the status of such files, `git status` shows the files that either have been modified or staged, as to give a perspective on what files are staged and would be commited if the command was executed.


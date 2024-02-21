To remove a file, simply use `git rm`. The file will be removed from the working directory, and you'll not see it as untracked.

Removing the file from the directory isn't the same as removing tracked files. The first deletes the files and doesn't stage those changes, while the latter makes it so next time you commit, those files are deleted and no longer tracked.

If there is a file that you do not want tracked but still want it in your hard drive, like compiled files, you can used `git rm --cached FILE(S)`. This way the files are not staged and you don't have to commit it, then remove the file and commit again! 
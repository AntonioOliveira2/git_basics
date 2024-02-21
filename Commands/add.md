A file or folder that wasn't previously being tracked, or a tracked file that was modified can be added via:
	`git add file(s)`

This makes it so that untracked files are tracked and staged or modified files are staged. At any time you can modify the files that you added, but in doing so you have to `git add` it again. Otherwise the commit will not have the latest change of the file, but previous one that was added.

##### Example: 
Lets say that you have a project with a README.md, untracked file, and CONTRIBUTING.md, a tracked but modified file.

After modifying both, just use `git add README.md` and `git add CONTRIBUTING.md` (or `git add .` to add all the files in the project folder if you are in that folder).

You remember to make a last minute change to `README.md`, if you do not execute `git add README.md`, making a commit will not include the latest change!



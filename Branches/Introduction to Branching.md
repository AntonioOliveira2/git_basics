Given that Git is a series of snapshots, that include not only the data but also the information of the author, message of the commit and so on, what does the structure of organising all of this looks like?

A brief example to illustrate is helpful. Lets say that you have a directory with three files, and you do the usual:

```console
$ git add <file_1> <file_2> <file_3>
$ git commit -m 'Initial commit'
```

At this point, Git checksums every subdirectory, and stores the information in a tree. Then, it also creates a commit object that has the metadata and pointer to the root project tree. 

![[Commit branch.png]]

After several commits, there is also a chain of pointers between them:

![[Commit chain.png]]

However, this isn't enough to define branches. In order to do that, there is an additional pointer that indicates on what commit, automatically pointing to the latest commit made:

![[Branch pointers.png]]
This indicates the branch's current commit, but not where you are located. An additional pointer, `HEAD,` is used that points to the pointer of each branch: 

![[HEAD pointer.png]]

A simple way to verify in which branch you are, you can use [[Viewing Commit History|git log]] with the flag `--decorate` to see where the branch pointers are pointing.

#### Switching branches

Taking the previous images, to switch from `master` to `testing`, you can use `git checkout testing` to do so. That results in:

![[HEAD master to testing.png]]
After some experiments, you decide to make a commit on the `testing` branch, resulting in a commit tree like this:

![[Commit testing branch.png]]

It is clear that the `master` branch was *left behind*, such that it isn't pointing to the new commit done in another branch. Using `git checkout master`, the `HEAD` pointer goes to `master` once again:

![[Back to master pointer.png]]
This doesn't just change to where the `HEAD` pointer is pointing, but also reverts your working directory back to the commit `master` points to. Any change that you do now diverges from the commit that you previously did on `testing`. Lets take that hypothetical and this is what happens:

![[Diverging branches.png]]

As previously mentioned, the `git log --oneline --decorate --graph --all` will print out the commits history, where the pointers are currently pointing to and where it diverged:

```console
$ git log --oneline --decorate --graph --all
* c2b9e (HEAD, master) Make other changes
| * 87ab2 (testing) Make a change
|/
* f30ab Add feature #32 - ability to add new formats to the central interface
* 34ac2 Fix bug #1328 - stack overflow under certain conditions
* 98ca9 Initial commit of my project
```

To create and change to said branch at the same time, you can use `git checkout -b <new_branch>`.
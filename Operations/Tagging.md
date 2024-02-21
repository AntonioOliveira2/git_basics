
Git has the ability to tag specific points, most often used to mark release points (`v1.0`, `v3.52`).

To list all the existing tags, use `git tag`. Some repositories might have hundreds of tags, meaning that the flag `-l` makes it able to filter. For example:

```console
$ git tag -l "v1.8.5*"
v1.8.5
v1.8.5-rc0
v1.8.5-rc1
v1.8.5-rc2
v1.8.5-rc3
v1.8.5.1
v1.8.5.2
v1.8.5.3
v1.8.5.4
v1.8.5.5
```

There are two types of tags: *lightweight* and *annotated*. Lightweight tags are just pointers to a certain commit, whereas annotated tags are their own object, with the creator's information, checksum and can even be signed with a GNU PGP.

`git tag -m "<message>"` Specifies the tagging message. Not doing so opens an editor to 
type it in. `git show` has the data tag alongside the commit that was tagged by it.

A lightweight tag, you just don't specify `-a`, `-s` or `-m`. By doing so, the tag doesn't appear in `git show`.

#### Tagging commits later

If you have several commits and you want to tag them after, you can just list them with `git log`, and take their checksum (or part of it), followed by `git tag -a <tag_name> <checksum>`.

#### Sharing tags

By default, `git push` doesn't transfer tags to remote servers. To do that, you'll have to either use `git push origin <tag_name>` or if wish to push all tags, `git push origin --tags`.
#### Deleting tags

To remove a tag, `git tag -d <tag_name>` ought to do the trick. But once again, that change isn't pushed to remote servers. To do that, there are two ways: `git push <remote> :refs/tags/<tag_name>` or `git push <remote> --delete <tag_name>`.

#### Checking out tags

Lets say that you're supporting an older version of your project, and you need to edit a certain version. `git checkout <tag_name>` changes you to a *detached HEAD* state, where you can make changes and create a commit, but the tag stays the same. However, this commit doesn't belong to any branch, and thus is unreachable except for the exact hash value. If you need to save those changes, you might want to create a branch for that with `git checkout -b <branch_name> <tag_name>`. 

However, by doing so the `branch_name` branch will be slightly different than your `tag_name`tag since it will move forward with your new changes, so do be careful.
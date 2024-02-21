In a repository, to see which remote servers you have configured, use the `git remote` command. It lists the names of the remotes that you specified. The default name Git gives the server you cloned from is *origin*. The specifier `-v` shows the URLs of those remotes.

If you have more than ore remote, the command lists them all. If you have a remote with multiple collaborators that also have their own remote, it also lists the respective users and their URLs. Since by default Git adds the origin remote, `git remote add <shortname> <url>` adds the specific remote. Then you don't have to reference the URL in future commands like `fetch`, but rather the shortname instead.

#### Fetching and Pulling from remotes

If any work has been done on the remote, after using `fetch` the new data that you don't have will be pulled. After doing this, you will have references to all of the branches from that remote, which you can merge and inspect at any time.

Despite fetching new data, `fetch` doesn't merge it automatically, that has to be done manually.  `git pull` not only pulls the data, but also merges that branch with your current branch, that is if your current branch is set up to track a remote branch. `git clone` setups the `master` (or the default branch's name) on the server that you cloned from.

#### Pushing to your remotes

To share a project at some point, you have to push it upstream. `git push <remote> <branch>`. By default names, you might want to push your `master` to the remote `origin`. This only works on a server that you cloned from and also have write access to it, and nobody pushed in the meantime. The latter has to do with merging the other pushes' modifications before you can insert your own.

#### Inspecting a remote

`git remote show <remote>` allows you to see, for example through the shortname, the characteristics of the remote's branch. It also tells you that if you're on the `master` branch, running `git pull` will automatically merge the remote's `master` branch after being fetched.

#### Renaming and removing remotes

`git remote rename <original_shortname> <new_shortname>` changes a certain remote shortname to a new one. This also changes the remote's tracking branch names too. If there is like `original_shortname/master`, it will become `new_shortname/master`.

`git remote remove <shortname>` removes a particular remote. 
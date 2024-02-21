References to remote repositories can be checked via `git remote show <remote>`, to check with more detail the given `<remote>` repository. 

These references can't be controlled locally, as they represent the state of a remote repository, so Git moves it whenever there is any network communication. 


Remote branches usually have the local name `<remote>\<branch>`, meaning that if you want to see the `master` branch of your `origin` remote repository, you would check the `origin/master` branch. Then lets say that you want to change to the `iss53` branch, simply go to `origin/iss53`.

#### Company Remote Example

![[git clone.png]]
If you do some work on your local `master` branch, and, in the meantime, someone else pushes to `git.ourcompany.com` and updates its `master` branch, then your histories move forward differently. Also, as long as you stay out of contact with your `origin` server, your `origin/master` pointer doesn’t move.

![[git fetch.png]]

Let's assume you have another internal Git server that is used only for development by one of your sprint teams. This server is at `git.team1.ourcompany.com`. You can add it as a new remote reference to the project you’re currently working on by running the `git remote add` command: 

![[multiple remotes git add.png]]

Now, you can run `git fetch teamone` to fetch everything the remote `teamone` server has that you don’t have yet. Because that server has a subset of the data your `origin` server has right now, Git fetches no data but sets a remote-tracking branch called `teamone/master` to point to the commit that `teamone` has as its `master` branch.

![[multiple remotes fetch.png]]

#### Pushing

Now given another scenario, you want to push the local branch `serverfix` to the remote `origin`. By doing `git push origin serverfix`, you're doing it with a shortcut. What the `serverfix` branch expands to is `refs/heads/serverfix:refs/heads/serverfix`, meaning that Git will take your local `serverfix` branch and update the remote's `serverfix`. 

If you wish to push to another remote branch, you can just do `git push origin serverfix:awsomebranch`, meaning that now the push will be done from the local `serverfix` branch to `remote/awsomebranch`.

The next time your collaborators fetch from the server, they'll get a reference to where the version of `serverfix` is under the remote branch of `origin/serverfix`:

```console
$ git fetch origin
remote: Counting objects: 7, done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 0), reused 3 (delta 0)
Unpacking objects: 100% (3/3), done.
From https://github.com/schacon/simplegit
 * [new branch]      serverfix    -> origin/serverfix
```

However, this does not mean that fetching remote tracking branches gives you access to a local copy of them, but rather a reference pointer that you can't modify.

To merge this work into your current working branch, you can run `git merge origin/serverfix`. If you want your own `serverfix` branch that you can work on, you can base it off your remote tracking branch:

```console
$ git checkout -b serverfix origin/serverfix
Branch serverfix set up to track remote branch serverfix from origin.
Switched to a new branch 'serverfix'
```

#### Tracking branches 

Using `git pull` automatically makes the branches fetched tracking. This means that there is a direct relationship to a remote branch, where they differ because the remote branch is also called *upstream*.

When you check out a local branch from a remote tracking branch, a tracking branch is automatically created (where the branch it tracks is called *upstream*). Running `git checkout -b <branch> <remote>/<branch>` is quite common, such that there is a shortcut to just `git checkout <branch>`.

If you want to setup a local branch with a different name from the remote branch, you can use the first version with a different local name.

If you already have a local branch and want to set it to a remote branch you just pulled down, or want to change the upstream branch you’re tracking, you can use the `-u` or `--set-upstream-to` option to `git branch` to explicitly set it at any time.

```console
$ git branch -u origin/serverfix
Branch serverfix set up to track remote branch serverfix from origin.
```

If you want to see what tracking branches you have set up, you can use the `-vv` option to `git branch`. This will list out your local branches with more information including what each branch is tracking and if your local branch is ahead, behind or both.

```console
$ git branch -vv
  iss53     7e424c3 [origin/iss53: ahead 2] Add forgotten brackets
  master    1ae2a45 [origin/master] Deploy index fix
* serverfix f8674d9 [teamone/server-fix-good: ahead 3, behind 1] This should do it
  testing   5ea463a Try something new
```

So here we can see that our `iss53` branch is tracking `origin/iss53` and is “ahead” by two, meaning that we have two commits locally that are not pushed to the server. We can also see that our `master` branch is tracking `origin/master` and is up to date. Next we can see that our `serverfix` branch is tracking the `server-fix-good` branch on our `teamone` server and is ahead by three and behind by one, meaning that there is one commit on the server we haven’t merged in yet and three commits locally that we haven’t pushed. Finally we can see that our `testing` branch is not tracking any remote branch.

Remember that these numbers are only since the last time you fetched from the server! This is just local caching.

#### Pulling

While the `git fetch` command will fetch all the changes on the server that you don’t have yet, it will not modify your working directory at all. It will simply get the data for you and let you merge it yourself. However, there is a command called `git pull` which is essentially a `git fetch` immediately followed by a `git merge` in most cases. If you have a tracking branch set up as demonstrated in the last section, either by explicitly setting it or by having it created for you by the `clone` or `checkout` commands, `git pull` will look up what server and branch your current branch is tracking, fetch from that server and then try to merge in that remote branch.

Generally it’s better to simply use the `fetch` and `merge` commands explicitly as the magic of `git pull` can often be confusing.

#### Deleting Remote Branches

Suppose you’re done with a remote branch — say you and your collaborators are finished with a feature and have merged it into your remote’s `master` branch (or whatever branch your stable codeline is in). You can delete a remote branch using the `--delete` option to `git push`. If you want to delete your `serverfix` branch from the server, you run the following:

```console
$ git push origin --delete serverfix
To https://github.com/schacon/simplegit
 - [deleted]         serverfix
```

Basically all this does is to remove the pointer from the server. The Git server will generally keep the data there for a while until a garbage collection runs, so if it was accidentally deleted, it’s often easy to recover.
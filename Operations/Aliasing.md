In order to make the usage of Git simpler, easier or more familiar, you can create aliases with [[config|git config]]. Some examples are:

```console
$ git config --global alias.co checkout
$ git config --global alias.br branch
$ git config --global alias.ci commit
$ git config --global alias.st status
```

This means that you only have to type `git co` for `git checkout` for example. 

Not only that, but it can be used to create new commands, by only using a portion of the command, for example without the arguments:

```console
$ git config --global alias.unstage 'reset HEAD --'
```

Like this, these two commands are equivalent:
```console
$ git unstage <file>
$ git reset HEAD -- <file>
```

Or to check only the latest commit:
```console
$ git config --global alias.last 'log -1 HEAD'
```

But its not limited to aliases. It can actually run commands of your own tools, just add the `!` as a prefix to the alias:
```console
$ git config --global alias.visual '!gitk'
```

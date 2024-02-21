
After some commits or a cloned repository, you can see what happened on previous commits. In order to do that, you can use`git log`.

Running `git log` command makes an output like this:

```console
$ git log
commit ca82a6dff817ec66f44342007202690a93763949
Author: Scott Chacon <schacon@gee-mail.com>
Date:   Mon Mar 17 21:52:11 2008 -0700

    Change version number

commit 085bb3bcb608e1e8451d4b2432f8ecbe6306e7e7
Author: Scott Chacon <schacon@gee-mail.com>
Date:   Sat Mar 15 16:40:33 2008 -0700

    Remove unnecessary test

commit a11bef06a3f659402fe7563abf99ad00de2209e6
Author: Scott Chacon <schacon@gee-mail.com>
Date:   Sat Mar 15 10:31:28 2008 -0700

    Initial commit
```

The most recent commits come first, and the checksum, author information and message are displayed.

`--patch NUMBER` displays the last NUMBER of commits.

`--stat` shows a summary with a list of modified files, how many files were changed, and how many lines were added/removed. 

Another really useful option is `--pretty`. This option changes the log output to formats other than the default. A few prebuilt option values are available for you to use. The `oneline` value for this option prints each commit on a single line, which is useful if you’re looking at a lot of commits. In addition, the `short`, `full`, and `fuller` values show the output in roughly the same format but with less or more information, respectively:

The most interesting option value is `format`, which allows you to specify your own log output format. This is especially useful when you’re generating output for machine parsing — because you specify the format explicitly, you know it won’t change with updates to Git:

```console
$ git log --pretty=format:"%h - %an, %ar : %s"
ca82a6d - Scott Chacon, 6 years ago : Change version number
085bb3b - Scott Chacon, 6 years ago : Remove unnecessary test
a11bef0 - Scott Chacon, 6 years ago : Initial commit
```

Useful specifiers for `git log --pretty=format` lists some of the more useful specifiers that `format` takes. Here are some useful specifiers:

|Specifier|         Description of Output
|`%H`          |        Commit hash                                        
|`%h`          |        Abbreviated commit hash
|`%T`          |        Tree hash
|`%t`          |        Abbreviated tree hash
|`%P`          |        Parent hashes
|`%p`          |        Abbreviated parent hashes
|`%an`        |        Author name
|`%ae`        |        Author email
|`%ad`        |        Author date (format respects the `--date=option`)
|`%ar`        |        Author date, relative
|`%cn`        |        Committer name
|`%ce`        |        Committer email
|`%cd`        |        Committer date
|`%cr`        |        Committer date, relative
|`%s`          |        Subject

`--graph` displays an ASCII graph with the branch and merge history:

```console
$ git log --pretty=format:"%h %s" --graph
* 2d3acf9 Ignore errors from SIGCHLD on trap
*  5e3ee11 Merge branch 'master' of https://github.com/dustin/grit.git
|\
| * 420eac9 Add method for getting the current branch
* | 30e367c Timeout code and tests
* | 5a09431 Add timeout protection to grit
* | e1193f8 Support for heads with slashes in them
|/
* d6016bc Require time for xmlschema
*  11d191e Merge branch 'defunkt' into local
```

If you want to limit the log output to a certain time interval, the option `--since=TIME` can bee used, where time can be "2.weeks" for a timespan, a specific date like "2008-01-15", or a relative date like "2 years 1 day 3 minutes ago".

Moreover, other things like `--author` or `--grep` search for a specific author or a keyword in the commits, respectively.

Another helpful filter is `-S`, that shows the commits that changed the number of times a certain string occurred. If a commit made or deleted a reference to a function, this option allows you to see which one(s).

Lastly, the option `--` followed by a path to a file shows the history of such a file

##### Example

If you want to see which commits modifying test files in the Git source code history were committed by Junio Hamano in the month of October 2008 and are not merge commits, you can run something like this:

```console
$ git log --pretty="%h - %s" --author='Junio C Hamano' --since="2008-10-01" \
   --before="2008-11-01" --no-merges -- t/
5610e3b - Fix testcase failure when extended attributes are in use
acd3b9e - Enhance hold_lock_file_for_{update,append}() API
f563754 - demonstrate breakage of detached checkout with symbolic link HEAD
d1a43f2 - reset --hard/read-tree --reset -u: remove unmerged new paths
51a94af - Fix "checkout --track -b newbranch" on detached HEAD
b0ad11e - pull: allow "git pull origin $something:$current_branch" into an unborn branch
```

Of the nearly 40,000 commits in the Git source code history, this command shows the 6 that match those criteria.
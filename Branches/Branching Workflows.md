#### Long Running Branches

The most common workflow is one where only the code that is entirely stable to be on `master`, another branch like `develop` to create code that might not be stable, but once it is gets merged to the `master`, and any other branches like `topics` or `issues` come from the `develop` branch, and not from master. Ultimately it will look like this:

![[conceptual view long running branches.png]]
But from the *pointer* perspective, it boils down to:

![[simplified view long running branches.png]]

The way that you separate branches is by stability, as mentioned before. This is especially useful when dealing with very complex projects.

#### Topic Branches

This workflow is related to the [[Branching and Merging]]  section, where you had concurrent branches like `master` and `iss53`. This way, its easier to jump from topic to topic without much dependency on what you have to resolve first:

![[topic branches before merge.png]]

Given that the numbers are the order in which the commits have been created, you can clearly see that there is no need to keep developing at any synchronicity.  Lets then say that `dumbidea` gets merged to master, and `iss91v2` is the solution to the issue at cause. Then the history will look like:

![[topic branches after merge.png]]

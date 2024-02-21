Config allows you to set/remove variables that make your workspace interoperable between repositories. The command is `git config`, and the following examples are to be appended to the command.

Check all the defined git variables:
	`--list`

Define user and email variables for all local repositories:
	`--global user.name USERNAME`, where USERNAME is your username
	`--global user.email EMAIL`, where EMAIL is your email

Check a single variable:
	`user.name`

Remove a variable:
	`--unset user.password`

Set your preferred editor when git opens a file:
	`--global core.editor EDITOR`, where EDITOR is your editor of choice
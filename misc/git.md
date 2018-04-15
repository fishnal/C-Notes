# Git

+ [.gitignore](#git-gitignore)
+ [`status`](#git-status)
+ [`add`](#git-add)
+ [`commit`](#git-commit)
+ [`remote`](#git-remote)
+ [`push`](#git-push)
+ [`pull`](#git-pull)

___

## <a id="git-gitignore"></a> .gitignore

+ Tells git to ignore files or directories that match criteria given.
+ Matches using [glob patterns](https://en.wikipedia.org/wiki/Glob_(programming)), not [regular expressions (regex)](https://en.wikipedia.org/wiki/Regular_expression)
+ Can use `!` to *not* ignore that pattern
	+ i.e. Say you want to ignore every file in directory `./foo/` except `./foo/bar.txt`, we can do

	```text
	# .gitignore
	foo/*.*
	!foo/bar.txt
	```

## <a id="git-status"></a> `status`

+ Displays files that have differences between what git's tracking
	+ i.e. You're tracking `foo.txt` and you add some words to it, `git status` will tell you that `foo.txt` had changes made to it

## <a id="git-add"></a> `add`

+ Stages new files to be tracked or new changes in currently tracked files
+ `-f` forces to add a file, regardless if it's being ignored or not

## <a id="git-commit"></a> `commit`

+ Commits/applies staged changes from `add` to index
+ `-a` only commits files that are currently being tracked (new files are not affected)

## <a id="git-remote"></a> `remote`

+ Manages remote repositories that you track
+ `add <name> <url>` adds a repo as `name` located at `url`
+ `rename <old> <new>` renames a tracked repo
+ `remove <name>` removes a tracked repo
+ `get-url <name>` gets the `url` of a tracked repo
+ `set-url <name>` sets the `url` of a tracked repo
+ Common names are `origin` and `upstream`

## <a id="git-push"></a> `push`

## <a id="git-pull"></a> `pull`
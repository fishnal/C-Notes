# Unix Commands

+ [`cp`](#unix-cp)
+ [`ls`](#unix-ls)
+ [`cd`](#unix-cd)
+ [`pwd`](#unix-pwd)
+ [`rm`](#unix-rm)
+ [`mv`](#unix-mv)
+ [`diff`](#unix-diff)
+ [`touch`](#unix-touch)
+ [`gcc`](#unix-gcc)
+ [`rmdir`](#unix-rmdir)
+ [`tar`](#unix-tar)
+ [`grep`](#unix-grep)
+ [`chmod`](#unix-chmod)

___

+ <a id="unix-cp"></a> `cp` copies files and directories
	+ Usage: `cp [opts] <SRC> <DIR>`
	+ `-r|-R|--recursive` copies directories recursively
+ <a id="unix-ls"></a> `ls` lists directory contents
	+ Usage: `ls [opts] [FILE]` lists info about file
	+ `-a` includes ALL hidden files (including `.` and `..`)
	+ `-A` includes MOST hidden files (excludes `.` and `..`)
	+ `-d|--directory` lists directories but not their contents
	+ `-R|--recursive` lists subdirectories recursively
+ <a id="unix-cd"></a> `cd` changes current working directory
	+ `.` will change to current working directory (no real effect)
	+ `..` will go up one level (if it can)
+ <a id="unix-pwd"></a> `pwd` prints name of current working directory
+ <a id="unix-rm"></a> `rm` removes files or directories
	+ Usage: `rm [opts] <FILE>`
	+ `-r|-R|--recursive` removes directories and their contents recursively
	+ `-d|--dir` removes empty directories
+ <a id="unix-mv"></a> `mv` moves/renames files
	+ Usage: `mv <SRC> <DEST>` renames `SRC` to `DEST`
	+ Usage: `mv <SRC> <DIR>` moves `SRC` to `DIR`
	+ ***how do you move a file up a directory***
+ <a id="unix-diff"></a> `diff` compares files line by line
	+ Usage: `diff [opts] <FILE1/DIR1> <FILE2/DIR1>`
	+ `-bwi` ignores whitespaces, but not empty lines
	+ `-Bwi` ignores whitespaces and empty lines
+ <a id="unix-touch"></a> `touch` changes the timestamp on a file
	+ Usage: `touch [opts] <FILE>`
	+ If used on existing file, file's timestamp changed to now
	+ If used on non-existing file, creates that file
	+ `-c|--no-create` does not create a file
+ <a id="unix-gcc"></a> `gcc` compiles C/C++ files
	+ Usage: `gcc [opts] <FILES>`
	+ `-c` compiles but does not link (output file ends with `.o`)
	+ `-o <FILE>` places output file in specified file
	+ `-g` compiles with debugging symbols
+ <a id="unix-rmdir"></a> `rmdir` removes empty directories
	+ Usage: `rmdir [opts] <dir>`
	+ `-p|--parents` removes directory and it's ancestors (if they're also empty)
+ <a id="unix-tar"></a> `tar` stores and extracts files from a tape archive
	+ Usage: `tar [opts]`
	+ `-c` creates a new archive
	+ `-z` indicates the gzip compression method
	+ `-f` indicates the archive file
	+ `-x` extracts files from an archive
	+ Creating a compressed `.tar.gz` file: `tar -czf dest.tar.gz srcdir`
	+ Extracting a compressed `.tar.gz` file: `tar -xzf src.tar.gz`
+ <a id="#unix-grep"></a> `grep` searches a file for a pattern
	+ Usage: `grep [opts] <pattern> <file>`
	+ Supports regular expressions for patterns
+ <a id="unix-chmod"></a> `chmod` changes the file mode bits (basically it's permissions)
	+ Usage: `chmod <permission> <file>`
	+ Permission is provided in as a 9-bit octal number or in `rwx` format
		+ `chmod 777 file` provides all permissions to everyone because 7 in octal is 111 in binary
		+ `chmod r---w---x file` provides read to owner, write to group, and x to others
	+ First 3 bits modify the owner's permission
	+ Second 3 bits modify group's permission
	+ Third 3 bits modify other's permission
	+ Each bit in the 3-bit sequence represents read, write, and execute permissions respectively

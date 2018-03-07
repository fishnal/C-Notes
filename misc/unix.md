# Unix Commands
+ [cp](#unix-cp)
+ [ls](#unix-ls)
+ [cd](#unix-cd)
+ [pwd](#unix-pwd)
+ [rm](#unix-rm)
+ [mv](#unix-mv)
+ [diff](#unix-diff)
+ [touch](#unix-touch)
+ [gcc](#unix-gcc)

`cp` <a id="unix-cp"></a> copies files and directories
+ Usage: `cp [opts] <SRC> <DIR>`
+ `-r|-R|--recursive` copies directories recursively

`ls` <a id="unix-ls"></a> lists directory contents
+ Usage: `ls [opts] [FILE]` lists info about file
+ `-a` includes ALL hidden files (including `.` and `..`)
+ `-A` includes MOST hidden files (excludes `.` and `..`)
+ `-d|--directory` lists directories but not their contents
+ `-R|--recursive` lists subdirectories recursively

`cd` <a id="unix-cd"></a> changes current working directory
+ `.` will change to current working directory (no real effect)
+ `..` will go up one level (if it can)

`pwd` <a id="unix-pwd"></a> prints name of current working directory

`rm` <a id="unix-rm"></a> removes files or directories
+ Usage: `rm [opts] <FILE>`
+ `-r|-R|--recursive` removes directories and their contents recursively
+ `-d|--dir` removes empty directories

`mv` <a id="unix-mv"></a> moves/renames files
+ Usage: `mv <SRC> <DEST>` renames `SRC` to `DEST`
+ Usage: `mv <SRC> <DIR>` moves `SRC` to `DIR`
+ ***how do you move a file up a directory***

`diff` <a id="unix-diff"></a> compares files line by line
+ Usage: `diff [opts] <FILE1/DIR1> <FILE2/DIR1>`
+ `-bwi` ignores whitespaces, but not empty lines
+ `-Bwi` ignores whitespaces and empty lines

`touch` <a id="unix-touch"></a> changes the timestamp on a file
+ Usage: `touch [opts] <FILE>`
+ If used on existing file, file's timestamp changed to now
+ If used on non-existing file, creates that file
+ `-c|--no-create` does not create a file

`gcc` <a id="unix-gcc"></a> compiles C/C++ files
+ Usage: `gcc [opts] <FILES>`
+ `-c` compiles but does not link (output file ends with `.o`)
+ `-o <FILE>` places output file in specified file
+ `-g` compiles with debugging symbols
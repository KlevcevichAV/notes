# Terminal
#### FILE MANAGEMENT COMMANDS

+ List information about the FILEs --`ls`

	`-l` -- use a long listing format

	`-a ` -- do not ignore entries starting with .

+ Change the shell working directory -- `cd`

+ Print the name of the current working directory -- `pwd`

+ Create the DIRECTORY(ies), if they do not already exist. -- `mkdir`

+ Remove (unlink) the FILE(s) -- `rm`
	
	`-r` remove directories and their contents recursively

+ Move (rename) files -- `mv`

+ Copy files and directories -- `cp`

+ Start a program -- `open`

+ Concatenate files and print on the standard output -- `cat`

+ Display Linux processes -- `top`
+ An interface to the on-line reference manuals -- `man`

+ Search for files in a directory hierarchy --`find`

+ Change file mode bits -- `chmod`
  + `chmod -R 777` -- change files and directories recursively
  + `chmod +x file` -- makes the file executable

#### CONSOLE COMMANDS FOR WORKING WITH TEXT

+ Viewing long texts that do not fit on one screen -- `less`

+ File editing -- `nano file`

+ Output the first part of files -- `head`
+ Output the last part of files-- `tail`

	Head выводит несколько первых строк из файла (голова), а tail выдает несколько последних строк (хвост).

+ Searches text by pattern -- `|grep`

+ Sort lines of text files -- `sort`

	`-n`(numeric) -- compare according to string numerical value

	`-r`(reverse) -- reverse the result of comparisons
+ Print newline, word, and byte counts for each file -- `wc`

+ Compare files line by line -- `diff`

#### Keyboard shortcuts

- `ctrl + c` — stop
- `ctrl + z` — freeze 
- `ctrl + d` - close the terminal
- `fg` — defrost
- `ctrl + shift + c` — copy
- `ctrl + shift + v` — insert

#### File upload
+ Text program for downloading files -- `wget link`
  + `-P prefix` -- Set directory prefix to prefix.
  + `-i file` -- Read URLs from a local or external file. 
  + `-r` -- enable recursive browsing of directories and subdirectories on the remote server.
  + `-l` -- Determine the maximum recursion depth equal to depth when browsing directories on a remote server. By default depth = 5.
  + -A <acclist>, –accept <acclist>, -R <rejlist>, –reject <rejlist> -- a comma-separated list of filenames to (accept) or not (reject) upload. Allowed to set file names by mask.

#### Archives
+ Unzip archive.zip -- `unzip archive.zip`
+ Unzip archive.gz-- `gunzip archive.gz`
+ Zip files in zip format -- `zip files`
+ Zip file in gz format -- `gzip file`
+ Zip files in gz format
```
tar -cvf archive.tar files
gzip archive.tar
```
+ Pack the listed files into archive.tar.gz -- `tor -zcxf archive.tar.gz files`

+ Unzip archive.tar -- `tar -xvf archive.tar`
+ Unzip archive.tar.gz -- `tar -xzvf archive.tar.gz`

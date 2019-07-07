# FILTERING, DATA OVERVIEW AND TRANSFORMATION

## CAT

Overview of the content of a file

When working with large files of several hundreds of lines, it is sometimes difficult to spot information.

The cat commands outputs the content of a file to the console.

```
cat filename.ext
```

However, it is not very convenient on his own to see the content of very large files.

## MORE FINE-GRAINED WAYS TO LOOK AT FILES CONTENT

Note: the following commands are equivalent to doing

```
cat filename.ext | command
```

### HEAD 

Without arguments, displays the first 10 lines of a file
```
head filename.ext
```

This will output the first 30 lines of the file
```
head -30 filename.ext
```

### TAIL

Tail is used to see the end of a file, without outputing the whole file (which might take some time when working with really large files).

This command will do the opposite of head: display the last 1 lines of filename.ext
```
tail filename.ext
```

This will display the last 20 lines of filename.ext
```
tail -20 filename.txt
```

When working with files being populated while you look at their content (ie: log files), you can use tail's -f option to watch the file as it is being updated
```
tail -f filename.ext
```
Note: press the Ctrl+c keys to exit

### MORE

The more command can be used to read a large file line by line.

This command allows you to scroll a file line by line by pressing enter to move one line forward. Press the q key to exit
```
more filename.ext
```

### LESS

This command does the same as more but with the possibility to move both forward and backward X lines as well as searching within the file with the / key for instance.
```
less filename.ext
```

## FILTERING

###GREP

The grep command allows to search within a file and filter out content.

This command will only output the lines containing the searched term.
```
grep "search term" filename.ext
```

Using the -v option, this command will do the opposite: filter out lines containing "search term"
You can also search within several files with a single command.
```
grep -v "search term"
```

This will search within file1.ext and file2.ext
```
grep "search term" file1.ext file2.ext
```

This will search within all files in the current directory having the .ext extension (the * character is used to match any character, hence filename). The matching results will be prefixed with the filename in which the matching was found. To remove the prefix, use the -h option.
```
grep "search term" *.txt
```

For case insensitive search needs the -i option
This command will return matching results for "Search TERM" as well, for instance.
```
grep -i "search term" *.txt
```

To have contextual information about the matching results, you can use the -A (after), -B (before) or -C (both before and after) followed by a number to get X contextual lines.
This command will return the matching results and the 2 preceeding lines for each.
```
grep -A 2 "search term" filename.ext
```

### SED

In the context of data extraction or filtering, sed is another powerfull tool with a lot of options.

This command will output the first line of a file.
The -n options tells sed to not output anything and then 1 is the matching pattern followed by an instruction (p being for print).
```
sed -n '1 p' file.ext
```

This command will output lines 10 to 15 of the file.
```
sed -n '10,15 p' file.ext
```

You can also use search terms to match certain lines
This command will only output lines containing "iPhone" 
```
 sed -n '/iPhone/ p' file.ext
```

To perform a case insensitive search, use the I option after the matching criteria
This command will only output lines containing "iPhone, iphone, IPhone, IPHONE, etc ..." 
```
 sed -n '/iphone/I p' file.ext
```

To match lines starting with a with a certain word, use the ^ option
```
 sed -n '/^DC/ p' file.ext
```

To filter out lines, use the d action
```
 sed '/secure/ d' file.ext
```


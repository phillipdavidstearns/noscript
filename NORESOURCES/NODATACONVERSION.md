# DATA CONVERSION

The command line interface offers many ways to transform information, from simple truncation to data conversion.

## CUT

For files with delimiters between fields, such as csv (comma) or tsv (tab), the cut command can be used to extract certain fields.

This command will extract the second field of a tab separated values file.
```
cut -f2 filename.ext
```

This command will extract the 2nd and 4th field of a tab separated values file.
```
cut -f2,4 filename.ext
```

Cut uses the tab separator by default.
When working with other separators, you can set it with the -d option.

This command will extract the 2nd and 4th field of a comma separated values file.
```
cut -d, -f2,4 filename.ext
```

This command will extract the 2nd and 4th field of a space separated values file.
Note: quotes have to be used to make the space visible to cut (otherwise treated as a separation between arguments)  
```
cut -d" " -f2,4 filename.ext
```

Fields order can also be changed.
This command will output the 4th then the 2nd fields of a comma separated values file. 
```
cut -d, -f4,2 filename.ext
```

You can also specify a range of fields with a -
This command will output the 2nd to the 5th fields of a comma separated file
```
cut -d, -f2-5 filename.ext
```

The cut command can also be used as a way to truncate lines
This command will only print from the 19th to the 38th characters of each line (included charaters of a file)
```
cut -c 19-38 filename.ext
```

Using a number followed by a - allows to remove the first X characters of each line.
This command will remove the first 19 characters of each line
```
cut -c 19- filename.ext
```

## SORT

The sort options offers a way to sort files and have various options.

By default, sort will use alphabetical order
```
sort filename.ext
```

For reverse search, use the -v option
```
sort -v filename.ext
```

Sort considers spaces as field separators by default.
You can force the separator to another one with the -t option.
This will sort the file, considering the comma as a field separator.
```
sort -t, filename.ext
```


To operate sorting on a certain field, you can use the -k otpion
This command will sort the file using the 3rd field's values using the blank space as a field separator.
```
sort -k3 filename.ext
```

It is also possible to shuffle a file content with the -R option.
This command will shuffle the content of the file on a line basis
```
sort -R filename.ext
```
Note: this is similar to using the shuf command
```
shuf filename.ext
```

The -u option allows to keep unique values (removing duplicated lines)
```
sort -u filename.ext
```
Note: this is similar to using the uniq command
```
sort filename.ext | uniq
```

## UNIQ

Uniq is a command used to remove identical consecutive lines.
Hence, those two commands will have a dfferent output if the filename.ext file is not sorted.
```
cat filename.ext | sort | uniq
cat filename.ext | uniq
```

It can also be used to know how many times a line occurs.
This command will outut the content of filename.ext with each line preceeded with its number of occurences.
```
sort filename.ext | uniq -c
```

This command will output the first 8 characters of each line, sort them, count the number of occurences and sort the results using numerical sorting.
```
cut -c 1-8 filename.ext | sort | uniq -c | sort -n -k1
```


## SHUF



## WC

## UNIQ



## TR

## SED s//




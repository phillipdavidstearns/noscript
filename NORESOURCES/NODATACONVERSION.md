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

For reverse sorting, use the -v option
```
sort -v filename.ext
```

For case insensitive sorting, use the -f option
```
sort -f filename.ext
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
This command will output the content of filename.ext with each line preceeded with its number of occurences.
```
sort filename.ext | uniq -c
```

This command will output the first 8 characters of each line, sort them, count the number of occurences and sort the results using numerical sorting.
```
cut -c 1-8 filename.ext | sort | uniq -c | sort -n -k1
```


## SHUF
Shuf is a way to operate a random sorting on a file, on a line by line basis.

This command will shuffle a file and extract a random line each time
```
shuf filename.txt | head -1
```

## WC
WC is for word count and is a way to get information about a file's content.

```
wc -l filename.txt
```
Note: this command will remove the filename from the ouput
```
cat filename.txt | wc -l
```

Without options, it will display the number of lines, words, then characters.
```
cat filename.txt | wc
```

To get the number of words, use the -w option
```
cat filename.txt | wc -w
```

To get the number of characters, use the -m option
```
cat filename.txt | wc -m
```

To get the number of bytes, use the -c option
```
cat filename.txt | wc -c
```

To get longest line lenght, use the -L option
```
cat filename.txt | wc -L
```

## TR

TR is a way to do character replacement.

This command will transform uppercase characters to lowercase.
```
cat filename.ext | tr [:upper:] [:lower:]
```

This command will replace As with 1, Bs with 2 and Cs with 3.
```
cat filename.ext | tr "ABC" "123"
```

The -d option can be used to remove characters
This command will remove both : and , characters.
```
cat filename.ext | tr -d ":,"
```
This other command will remove line breaks.
```
cat filename.ext | tr -d "\n"
```
This other command will remove punctuation.
```
cat filename.ext | tr -d [:punct:]
```
This one will remove spaces.
```
cat filename.ext | tr -d [:space:]
```

# REPLACING AND REMOVING DATA WITH SED

## SED

Apart from filtering (as seen in the NODATAFILTERING chapter), sed can also be used for text replacement.

This command will replace the first occurence of "secure" on each line with "insecure"
```
sed 's/secure/insecure/' filename.ext
```

To replace all occurences, use the g (globally option)
```
sed 's/secure/insecure/g' filename.ext
```

To ignore case, use the i (insensitive option)
```
sed 's/secure/insecure/i' filename.ext
```
Note: you can combine several options
```
sed 's/secure/insecure/gi' filename.ext
```

To replace only the 2nd occurence, use a number right after the replacethe t information
This will replace the 2nd occurence of 1 on every line with ____
```
sed 's/1/____/2' filename.ext
```

To replace every occurence from the X occurence, combine it with the g option
This will replace the 2nd occurence of 1 and the next ones with ____
```
sed 's/1/____/2g' filename.ext
```

### Exercise
Download George Orwell's 1984 book using the curl command.
Curl allows to output a webpage content to the console.
It can be redirected to a file for saving that file.
```
curl http://gutenberg.net.au/ebooks01/0100021.txt > 1984.txt 
```
Here is its equivalent with the wget command
```
wget http://gutenberg.net.au/ebooks01/0100021.txt -O 1984.txt 
```



* Have a look at the top and bottom of the file and find a way to remove the metadata using command line.
* Check the number of lines in the file
* Check the number of words
* Create a script or write a command to only keep unique words, lowercase and have information about their frequency in the text.
* Create a script or write a command to display the 10 words used the most.
* Create a script or write a command to display the words used 13 times.
* Create a script or write a command to display the words used 13 times (without the number information) and save it to a file.


### REGULAR EXPRESSIONS


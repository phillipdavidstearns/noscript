## Friday

* Review
* Getting connected

## Review

We attempted to use the process of configuring our Pis to connect to a personal hotspot as a way to learn basic bash concepts.

### Commands

* man
* pwd
* ls
* mkdir
* rmdir
* cd
* cp
* nano
* sudo
* reboot

### Concepts

* Exploring the File System
* Creating and Deleting Directories
* Copying files
* Editing files
* File privileges 

## PIPING AND REDIRECTION

### Prerequisites

We will first install command line tools needed for this chapter.

On Raspbian (the Linux flavor we're using on the Raspberry Pi), they are installed via the `apt` tool.

`apt` is short for aptitude and is the package manager tool for Debian based distributions of Linux like Raspbian. Software packages can be installed, updated, and removed using this tool.

Run the following commands to update the software repository and get the latest versions of the packages already installed

```
sudo apt update
sudo apt upgrade
```

Run the following command to install `cowsay`

```
sudo apt install cowsay
```

Run the following command to install `fortune`, `figlet`, and `lolcat`

```
sudo apt install fortune figlet lolcat 
```

### `echo`

echo is a 

### `cowsay`

`cowsay` displays ascii art characters saying something

Type this command and press enter

```
cowsay hello no school 
```

`cowsay` allows others (even custom) characters stored as text files with placeholders for the eyes and the text

You can list them with the -l argument

Type this command and press enter

```
cowsay -l
```

Typing this command and pressing enter will produce a different output

```
cowsay -f hellokitty hello no school 
```

### Fortune

Fortune is fortune cookie generator

Type this command several times, using the up arrow key to call the last command executed

```
fortune 
```

This produces various length's texts

You can limit this size by using the -s argument (short), combined with -n (number) followed by the desired max length

This command will produce outputs of no more than 20 characters

```
nostudent@192.168.1.100 $ fortune -n 20 -s
```


### Lolcat

Lolcat is a variation of the cat (catalog command used to display a file content)

Type this command and press enter

```
nostudent@192.168.1.100 $ echo hello no school | lolcat 
```

This will output a colorful hello no school
We'll give you more details about this syntax later.

### Figlet

Figlet displays ASCII banner

Type this command and press enter

```
nostudent@192.168.1.100 $ figlet hello no school
```

This will output an ASCII hello no school banner


### Echo

The echo command ouputs text to the console

Type this command and press enter

```
nostudent@192.168.1.100 $ echo hello no school 
```

### Piping

When command line tools produce an output (text displayed in the console), it can be redirected to another command to perform another operation.

This is a powerful function allowing to chain commands to perform a complicated task with simple tools.

We can combine the tools with the piping functionality, by adding the | character to send ouput of a previous command as an input for the other one.

Type these commands and press enter

```
nostudent@192.168.1.100 $ figlet hello no school
nostudent@192.168.1.100 $ figlet hello no school | lolcat
nostudent@192.168.1.100 $ echo hello no school | lolcat | figlet
nostudent@192.168.1.100 $ echo hello no school | figlet | lolcat
nostudent@192.168.1.100 $ echo hello no school | cosway | lolcat
nostudent@192.168.1.100 $ fortune -s -n 20 | cosway | lolcat
```

Exercice: try different combinations of the tools we used with various arguments


### Redirect

#### Output to a file

Console output can be redirected to files

Type this command and press enter

```
nostudent@192.168.1.100 $ fortune | figlet > test.txt
```

You won't see any output in the console because it's been redirected to a file

You can see the file content using the cat command

Type these commands and press enter after each

```
nostudent@192.168.1.100 $ cat test.txt
nostudent@192.168.1.100 $ lolcat test.txt
```

Now run these 2 commands several times (used the up arrow key to avoid having to retype them)

```
nostudent@192.168.1.100 $ fortune -s -n 20 | figlet > test.txt
nostudent@192.168.1.100 $ cat test.txt
```

Note: they can be combined in a single line using this syntax

```
nostudent@192.168.1.100 $ fortune -s -n 20 | figlet > test.txt; cat test.txt
```

Or even this syntax, using the tee command which redirects to both console and file

```
nostudent@192.168.1.100 $ fortune -s -n 20| figlet | tee test.txt
```

As you can notice, the file content is erased every time you run the command

If you want to keep the previous content, you have to use another form of redirect.

#### Append to a file

Type this command and press enter several times

```
nostudent@192.168.1.100 $ fortune -s -n 20 | figlet >> test.txt; cat test.txt
```

Note it is equivalent to using the tee command with the -a argument 

```
nostudent@192.168.1.100 $ fortune -s -n 20 | figlet | tee -a test.txt
```

As you can notice, the previous content is kept

But the file gets larger and cat shows it all in one shot so you are unable to the the first part of the document

Hopefully, there are other tools allowing to previous part of the file and read the file page by page.

## FILE VIEWING AND FILTERING

### To be continued
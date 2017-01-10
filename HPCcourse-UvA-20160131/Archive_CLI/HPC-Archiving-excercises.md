# Archiving your data

## Introduction

Welcome to the hands-on part of archiving data using tar or dmftar in combination with the Data Archive of SURFsara. After finishing the module, you will have learned how to use the archiving tools to archive your data.

The module consists of two parts:

- Archiving using tar: learn to create and extract tarballs (tar archive files)
- Archiving using dmftar: learn to create and extract dmftar archives, either locally or on the Data Archive service

Each part has several excercises and will take approximately 10 minutes each. For those who are faster, several extra bonus exercises are provided which will educate more advanced usage of the tools.

General syntax:

- tar:
```tar [OPTIONS] <tarball> <input-files..>```

- dmftar:
```dmftar [OPTIONS] <dmftar-archive> <input-files>```

General tips:

- Using the tar command: combine multiple options into a single argument, e.g. `-a -b -c` becomes `-abc`

## Part 1: Basic tar usage (10 min)

Learn how to explore the basic options of tar and to pack a file or directory.

The tar tool is invoked by entering `tar` as a command, and is available to all users and in all directories.

First go to the tar excercise directory `~/tar` and inspect its contents.

### Exercise 1.a: The options of tar

**Task: Show the options of tar**

As with every new tool, you'll need to learn how to use it and to make it do something the way you want it to do it. Therefore a first step is to investigate the tool's options by requesting additional information.

Hint: add an option to the command in order to list the possibilities of the tool.

##### Solution

Enter the command, optionally followed by an option showing additional help, e.g.:

```
tar --help
```

##### Questions

- What are the differences in output between:
 - Entering the sole command
 - Adding the `--usage` option
 - Adding the `--help` option
 - Using the `man` pages
- What is a smart strategy to look for specific functionality in a tool? 

### Exercise 1.b. Pack files into a tar file

**Task: Pack the files `data1.dat`, `data2.dat` and `data3.dat` into the tar file `data-files.dat`**

To pack some files into a single tar file, the tar command is used followed by some options and these file names as arguments.

Hints:

- Look for the create option
- Make sure the correct order of arguments are used and all required options are added.

##### Solution

Use the file names as argument:

```
tar -cf data-files.tar data1.dat data2.dat data3.dat
```

##### Questions

- Can the file names be combined into a single argument?

### Exercise 1.c. Pack a directory into a tar file

**Task: Pack the directory `data` into tar file `data-directory.tar`**

In order to pack several files stored in a directory, the tar command can be invoked by supplying the directory name as an argument.

Hint: use the command used in the previous excercise and replace the file name with a directory name.

##### Solution

Use the directory name as an argument:

```
dmftar -cf data-directory.tar data/
```

##### Questions

- What happens to the archive file if you run the command twice?

### Exercise 1. (bonus) Pack by file list

**Task: Pack the files specified in the file list named `files.lst` to tar file `data-filelist.tar`**

In some cases it might be useful to pack some files defined by a file list given by a specific file.

Hint: use the command used in the previous excercise followed by an extra argument.

##### Solution

Use the `-T` option:

```
tar -cf data-filelist.tar -T files.lst
```

##### Questions

- Can you add multiple file lists as input?

## Part 2: Extracting tarballs (10 min)

Learn how to inspect and unpack an existing archive file.

Make sure to first go to the correct directory and inspect its contents.

### Exercise 2.a: List the contents of an archive

**Task: List the contents of the tarball `data.tar`**

Before extracting any files from a tarball, it might be useful to inspect its contents.

Hint: look for the list option.

##### Solution

Use the `-t` option:

```
tar -tf data.tar
```

### Exercise 2.b. Unpack an archive

**Task: Unpack the contents of the file `data.tar` to the current directory**

Hint: look for the extract option.

##### Solution

Use the `-x` option with tar:

```
tar -xf /archive/<username>/data.tar
```

##### Questions

- Where is the extracted data stored?

### Exercise 2. (bonus)

**Task: Unpack the contents of the file `data.tar` located in the home folder of your archive service to a directory called `new`**

Before you can start copying or extracting data from the archive service directly, you need to stage the data from tape to the archive disk cache. The data migration (DM) tools can be used which are available on the Cartesius, Lisa and archive systems.

Copying and extracting will work without staging but can potentially take a huge amount of time when dealing with many files. So remember, when working directly with data stored on the archive, first stage it!

For the Lisa cluster, you first need to load a module containing the dmget tool:

```
module load rdmf
```

Hint: add an option that changes the directory.

##### Solution

Staging:

```
dmget -a /archive/<user>/data-files.dmftar
```

Extraction: use the `-C` option. First create the directory if it doesn't exist yet!

```
mkdir new
tar -xf data.tar -C new
```

##### Questions

- What happens if you extract the contents without staging the dmftar archive first?

## Part 3: Basic dmftar usage (10 min)

Learn how to pack directory contents using the dmftar tool. dmftar is only available on the archive, Cartesius and Lisa services.

The dmftar tool is invoked by entering `dmftar` as a command. It is available to all users and in all directories. On the Lisa cluster, you need to load it as a module first:

```
module load dmftar
```

Now go to the data directory for the dmftar excercises and inspect its contents.

### Exercise 3.a. Show the options of dmftar

**Task: Investigate the tool's options by requesting additional information**

Hint: add an option to the command in order to list the possibilities of the tool.

##### Solution

Enter the command, optionally followed by an option showing additional help.

```
dmftar --help
```

Questions:

- Which options are available to get additional help?
- Are there any man pages available?
- What is the difference in command structure between tar and dmftar?
- Can you combine the options to a single option? E.g. `-a -b` becomes `-ab`?
- Which option is always required?

### Exercise 3.b. Pack a directory

**Task: Pack the directory `data` in the dmftar excersises directory into a dmftar archive called `data.dmftar`**

dmftar can easily process the contents of entire directories compared to the more file-oriented nature of tar. Therefore the tool is mostly used to pack individual directories.

Hint: look for the create option.

##### Solution

Use the `-c` option:

```
dmftar -c -f data.dmftar data/
```

##### Questions

- Where is the archived data stored?

### Exercise 3. (bonus) Investigate the contents of a dmftar archive folder

**Task: List the contents of the newly created dmftar folder using standard Linux tools.**

To understand the actual contents of a dmftar archive it is useful to have a look at which files are actually stored in the folders.

##### Solution

```
ls -l data.dmftar/0000
```

##### Questions

- What is the structure of the dmftar archive?
- Which file types are created?
- What is stored in these files?
- How is the listing of the files in the dmftar archive changed?

### Exercise 3. (bonus) Verify your archive

**Task: Verify the contents of the archive folder `data.dmftar` on the bit-level**

An important aspect of data archiving is data integrity. The archiving facility of SURFsara automatically checks the data stored on tape by comparing files every once in a while.

Archived folders created using dmftar can be verified based on a checksum that is stored within it.

Hint: look for the verify option.

##### Solution

Add the `-V` option:

```
dmftar -V -f data.dmftar
```

## Part 4: Extracting dmftar archives (10 min)

Learn how to inspect and unpack an existing archive folder using dmftar.

### Exercise 4.a. List the contents of an dmftar archive

**Task: List the contents of the archive folder `archived-data.dmftar`**

Hint: look for the list option.

##### Solution

Use the `-t` option:

```
dmftar -t -f archived-data.dmftar
```

##### Questions

- What type of contents can be seen in the dmftar archive?
- What is the alternative for option `-t`?

### Exercise 4.b. Unpack an archive folder

**Task: Unpack the contents of the archive folder `archived-data.dmftar`**

Hint: look for the extract option.

##### Solution

Use the `-x` option:

```
dmftar -x -f archived-data.dmftar
```

##### Questions

- Which files and folder are created?

### Exercise 4. (bonus) Unpack only a single file from an archive folder

**Task: Unpack the file `data1-1.dat` from the archive file `archived-data.dmftar`**

Sometimes it is useful to only unpack specific files or folders from an archive file. Using dmftar this is easily accomplished by adding the file name or a name pattern as an argument to the command.

Hints:

- Use the `--help` option to learn how to add a file extraction pattern.
- Investigate in what subdirectory the file is located.

##### Solution

Add the file name and folder as the last argument in the command:

```
dmftar -x -f archived-data.dmftar data1/data1-1.dat
```

##### Questions

- Can you extract the file by only adding the file name?
- Where is the extracted file stored?

### Exercise 4. (bonus) Unpack a specific directory from an archive folder

**Task: Repeat the previous bonus excercise to extract all files in the subdirectory `data2` in the archive folder `archived-data.dmftar`**

##### Solution

Solely add the subdirectory name:

```
dmftar -x -f archived-data.dmftar data2/
```

## Part 5: Direct archive usage with dmftar (10 min)

In many cases the dmftar archives are stored in your home folder on the archive service. Either to offload the file system on Cartesius or Lisa, or to store the files for the long term.

In following excercises you'l learn to directly store and extract data files using the archive service. Since the archive service is based on a tape infrastructure, all files stored there need to be staged first before they can be used.

For the Lisa cluster, you first need to load two modules containing the tools. 

For `dmftar` enter:

```
module load dmftar
```

The tools are now available throughout the system and will remain so during your login session.

### Exercise 5.a: Archive directly to the archive service

**Task: Pack the files `data1.dat`, `data2.dat` and `data3.dat` into the dmftar archive `data-files.dmftar` directly on the archive in your home folder**

Hints:

- Make sure the right modules are loaded
- Locate your home folder on the archive service first
- Investigate how to set a destination directory for the newly created dmftar archive.

##### Solution

```
dmftar -c -f /archive/<user>/data-files.dmftar data*.dat
```

##### Questions

- How can the contents be listed in the newly created file?

### Exercise 5.b. Unpack directly from the archive service

**Task: Unpack the files from the dmftar archive `data-files-extract.dmftar` located in your home folder of the archive service**

Hints:

- If required, load the correct modules first
- Locate the dmftar archive file
- Make sure to stage the file first before unpacking it!
- Optionally list the contents of the dmftar archive

##### Solution

Extraction:

```
dmftar -x -f /archive/<user>/data-files.dmftar 
```

##### Questions

- What command can now be omitted when using dmftar compared to tar?

### Bonus excercise: using dmftar with remote servers
**Task: Unpack the files from the dmftar archive `data-files-extract.dmftar` located on the archive service using the remote path `archive.surfsara.nl`**

The dmftar tool can be used in combination with the archive service without a mounted folder. In order to do so first key-based authentication needs to be set up between your computer system and the archive.

Hints:

- This excercise involves three different actions:
 - Create a new private-public key pair
 - Copy the public key to the archive system
 - Execute the dmftar tool with the right command

##### Solution

Create the keys:

```
ssh-keygen
```

Copy the key to the archive system:

```
ssh-copy-id <username>@archive.surfsara.nl
```

Execute dmftar:

```
dmftar -x -f <username>@archive.surfsara.nl:~/data-files-extract.dmftar
```


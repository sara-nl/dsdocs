# Archiving your data

## Introduction

Welcome to the hands-on part of archiving data using tar or dmftar in combination with the Data Archive of SURFsara. After finishing the module, you will have learned how to use the archiving tools to archive your data efficiently.

The module consists of two parts:

- Archiving using tar: learn to create, list and extract tarballs (tar archive files)
- Archiving using dmftar: learn to create and extract dmftar archives, either locally or on the Data Archive service

Each part has several excercises and will take approximately 10 minutes each. For those who are faster, several extra bonus exercises are provided which will educate more advanced usage of the tools.

Most exercises provide hints and additional questions to answer for yourself. Please read these carefully so you understand what you are doing!

Every exercise's files are located in separate directories, e.g. the files of Exercise 1 are stored in folder `~/tar/exercise-1`, while the files of Exercise 4 are stored in `~/dmftar/exercise-4`. Make sure to go to these folders before starting the respective exercises. All files on the archive service are stored in your home folder (i.e. `/archive/<username>`).

#### General command syntax

- tar:
```tar [OPTIONS] <tarball> <input-files..>```

- dmftar:
```dmftar [TASK] [OPTIONS] -f <dmftar-archive> <input-files>```

#### Tips

- Using the commands: combine multiple options into a single argument, e.g. `-a -b -c` becomes `-abc`
- Please note that the `-f` option should always be right in front of the archive file, even when combining the different options (i.e. `-fc` will not work)

## Part 1: Basic tar usage (10 min)

Learn how to explore the basic options of tar and to pack a file or directory.

The tar tool is invoked by entering `tar` as a command, and is available to all users and in all directories.

First go to the tar excercise directory `~/tar/exercise-1` and inspect its contents.

### Exercise 1.a: The options of tar

**Task: Show the options of tar**

As with every new tool, you'll need to learn how to use it and to make it do something the way you want it to do it. Therefore a first step is to investigate the tool's options by requesting additional information.

Hint: add an option to the command in order to list the possibilities of the tool.

##### Questions

- What are the differences in output between:
 - Entering the sole command
 - Adding the `--usage` option
 - Adding the `--help` option
 - Using the `man` pages
- What is a smart strategy to look for specific functionality in a tool?

### Exercise 1.b. Pack files into a tarball

**Task: Pack the files `data1.dat`, `data2.dat` and `data3.dat` into tarball `data-files.dat`**

To pack some files into a single tarball, the tar command is used followed by some options and these file names as arguments.

Hints:

- Look for the create option
- Make sure the correct order of arguments are used and all required options are added.

##### Questions

- Can the file names be combined into a single argument?

### Exercise 1.c. Pack a directory into a tarball

**Task: Pack the directory `data` into tarball `data-directory.tar`**

In order to pack several files stored in a directory, the tar command can be invoked by supplying the directory name as an argument.

Hint: use the command used in the previous excercise and replace the file name with a directory name.

##### Questions

- What happens to the existing tarball if you run the command twice?

### Exercise 1. (bonus) Pack by file list

**Task: Pack the files specified in the file list named `files.lst` into tarball `data-filelist.tar`**

In some cases it might be useful to pack some files defined by a file list given by a specific file.

Hint: use the command used in the previous excercise followed by an extra argument.

##### Questions

- Can you add multiple file lists as input?

## Part 2: Extracting tarballs (10 min)

Learn how to inspect and unpack an existing archive file.

Make sure to first go to the correct exercise directory and inspect its contents.

### Exercise 2.a: List the contents of an archive

**Task: List the contents of the tarball `data.tar`**

Before extracting any files from a tarball, it might be useful to inspect its contents.

Hint: look for the list option.

##### Questions
- What happens if you add the `-v` option to your command?

### Exercise 2.b. Unpack an archive

**Task: Unpack the contents of the file `data.tar` to the current directory**

Hint: look for the extract option.

##### Questions

- Where are the extracted files now stored?

### Exercise 2. (bonus)

##### Introduction
In many cases, your data will be stored on the Data Archive service of SURFsara under your own account.

Before you can start copying or extracting data from the archive service directly, you need to stage the data from tape to the archive disk cache. The data migration (DM) tools can be used which are available on the Cartesius, Lisa and archive systems.

Copying and extracting will work without staging but can potentially take a huge amount of time when dealing with many files at the same time and potentially wasting a lot of core hours. So remember, when working directly with data stored on the archive, first stage it!

The right tool to stage your data on the archive is dmget, which has the following syntax:

```
dmget [options] <file or folder or wildcard>
```

If you use the `-a` option, the access time of the file will be updated and therefore your data will be staged for a longer time. You can therefore use it more often without requiring to stage the data again.

For the Lisa cluster, you first need to load a module containing the dmget tool:

```
module load rdmf
```

##### Exercise

**Task: Unpack the contents of the file `data-stage.tar` located in the home folder of the Data Archive to a directory called `new` in your Lisa account**

Hint: look for an option that changes the directory.

##### Questions

- What happens if you extract the contents without staging the dmftar archive first?

## Part 3: Basic dmftar usage (10 min)

Learn how to pack directory contents using the dmftar tool. dmftar is only available on the archive, Cartesius and Lisa services.

The dmftar tool is invoked by entering `dmftar` as a command. It is available to all users and in all directories. On the Lisa cluster, you need to load it as a module first:

```
module load dmftar
```

Now go to the data directory for the dmftar excercises (`~/dmftar`) and inspect its contents.

### Exercise 3.a. Show the options of dmftar

**Task: Investigate the tool's options by requesting additional information**

Hint: add an option to the command in order to list the possibilities of the tool.

##### Questions

- Which options are available to get additional help?
- Are there any man pages available?
- What is the difference in command structure between tar and dmftar?
- Can you combine the options to a single option? E.g. `-a -b` becomes `-ab`?
- Which option is always required?

### Exercise 3.b. Pack a directory

**Task: Pack the directory `data` in into a dmftar archive called `data.dmftar`**

dmftar can easily process the contents of entire directories compared to the more file-oriented nature of tar. Therefore the tool is mostly used to pack individual directories.

Hint: look for the create option.

##### Questions

- Where is the archived data stored?

### Exercise 3. (bonus) Investigate the contents of a dmftar archive

**Task: List the contents of the newly created dmftar folder using standard Linux tools.**

To understand the actual contents of a dmftar archive it is useful to have a look at which files are actually stored in the folders.

##### Questions

- What is the structure of the dmftar archive?
- Which file types are created?
- What is stored in these files?
- How is the listing of the files in the dmftar archive changed?

### Exercise 3. (bonus) Verify your archive

**Task: Verify the contents of the dmftar archive `data.dmftar` on the bit-level**

An important aspect of data archiving is data integrity. The archiving facility of SURFsara automatically checks the data stored on tape by comparing files every once in a while.

Archived folders created using dmftar can be verified based on a checksum that is stored within it.

Hint: look for the verify option.

##### Questions

- What is the ouput of the command?
- Can you be sure you data's integrity is intact?

## Part 4: Extracting dmftar archives (10 min)

Learn how to inspect and unpack an existing dmftar archive using dmftar.

When using the dmftar tool instead of tar, the underlying storage infrastructure is no longer of concern since dmftar will automatically detect whether a file is stored on disk or on tape. This saves you the burden to recall all files using separate commands and can improve the execution time of your data processing commands by several orders of magnitude.

For the Lisa cluster, you first need to load a module containing the tool:

```
module load dmftar
```

The dmftar tool is now available throughout the system and will remain so during your login session.

### Exercise 4.a. List the contents of an dmftar archive

**Task: List the contents of the dmftar archive `archived-data.dmftar`**

Hint: look for the list option.

##### Questions

- What type of contents can be seen in the dmftar archive?
- What is the alternative for option `-t`?
- Does adding the `-v` option change anything to the output?
- How can you search for a specific file in the archive?

### Exercise 4.b. Unpack a dmftar archive

**Task: Unpack the contents of the dmftar archive `archived-data.dmftar`**

Hint: look for the extract option.

##### Questions

- Which files and folder are created?

### Exercise 4. (bonus) Unpack only a single file from a dmftar archive

Sometimes it is useful to only unpack specific files or folders from an archive file. Using dmftar this is easily accomplished by adding the file name or a name pattern as an argument to the command.

**Task: Unpack the file `data1-1.dat` from the dmftar archive `archived-data.dmftar`**

Hints:

- Use the `--help` option to learn how to add a file extraction pattern.
- Investigate in what subdirectory the file is located.

##### Questions

- Can you extract the file by only adding the file name?
- In what folder is the extracted file stored?

### Exercise 4. (bonus) Unpack a specific directory from an dmftar archive

**Task: Repeat the previous bonus excercise to extract all files in the subdirectory `data2` in the dmftar archive `archived-data.dmftar`**

## Part 5: Direct archive usage with dmftar (10 min)

In many cases the dmftar archives are stored somewhere in your home folder on the Data Archive service, either to offload the file system on Cartesius or Lisa, or to store the files for the long term.

In following excercises you'll learn to directly store and extract data files stored on the Data Archive service using the dmftar tool.

Again, if not already done so, for the Lisa cluster you first need to load a module containing the tool:

```
module load dmftar
```

The dmftar tool is now available throughout the system and will remain so during your login session.

### Exercise 5.a: Archive directly to the archive service

**Task: Pack the files `data1.dat`, `data2.dat` and `data3.dat` into the dmftar archive `data-files.dmftar` directly on the archive in your home folder**

Hints:

- Make sure the right modules are loaded
- Locate your home folder on the archive service first
- Investigate how to set a destination directory for the newly created dmftar archive.

##### Questions

- How can the contents be listed in the newly created file?

### Exercise 5.b. Unpack directly from the Data Archive service

**Task: Unpack the files from the dmftar archive `data-files-extract.dmftar` located in your home folder on the archive service**

Hints:

- If required, load the correct modules first
- Locate the dmftar archive file
- Make sure to stage the file first before unpacking it!
- Optionally list the contents of the dmftar archive

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



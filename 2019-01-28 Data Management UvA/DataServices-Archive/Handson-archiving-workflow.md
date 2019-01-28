# Hands-on: Archiving your data

## Data Management Course
#### Trainers: Narges Zarrabi (SURFsara), Sharif Islam (SURFsara)

## Introduction

Welcome to the hands-on part of archiving data from Lisa using tar and dmftar tool in combination with the Data Archive of SURFsara. After finishing the hands-on, you will learn how to use the archiving tools to archive your data.

The module consists of three parts:

- Exploring the command line environment of Lisa and Archive and the DMF commands
- Archiving using tar: learn to create and extract tarballs (tar archive files).
- Archiving using dmftar: learn to create and extract dmftar archives, either locally (on Lisa or Cartesius) or on the Data Archive service.

Each part has several excercises. For those who are faster, several extra bonus exercises are provided which will educate more advanced usage of the tools.

**Requirements:**

- logins to lisa and archive (`sdemo<XXX>` accounts)

**General syntax:**

- tar:
```tar [OPTIONS] <tarball_name>.tar <input-files..>```

- dmftar:
```dmftar [OPTIONS] <dmftar_name>.dmftar <input-files..>```

**General tips:**

- Using the tar command: combine multiple options into a single argument, e.g. `-a -b -c` becomes `-abc`

## Part1: Explore the Environment

### Access the archive from HPC
To explore the environment, open a command line and try to login to the lisa service with the following command:

```
ssh <username>@lisa.surfsara.nl  
```

The Archive is mounted to Lisa via NFS mounts. From Lisa, you can see you archive directory here:


```
ls /archive/<username>/
```

<!--
### Direct access to the archive
You can also directly connect to the archive by
-->

Lets copy a directory of files to your archive directory to work with:

```
cp -r /archive/sdemo001/data-management/ /archive/<username>/
```

List the files in the directory you just copied to your archive:

```
ls -l /archive/<username>/data-management/
```


### DMF commands

DMF (Data Migration Facility) is a hierarchical storage management system for Silicon Graphics environments. Its primary purpose is to augment the economic value of storage media and stored data. DMF commands ate Tape aware and facilitate staging data from tape and putting data on tape.

If you are handling really large amounts of data, the DMF utilities come in useful. On Lisa and Cartesius the DMF utilities are system-wide installed.

The most important DMF commands are:

- `dmls` (Lists contents of directories)
- `dmput` (Migrates files from disk to tape media)
- `dmget` (retrieve files from tape to disk) 

To get more information about each command you can run the manual by:
`man dmls`

Lets see the list the contents of the data-management directory in our archive account, using`dmls` command:
```
dmls -l /archive/<username>/data-management/
```



```
dmls -l /archive/sdemo002/data-management/

drwx------  3 sdemo002    sdemo002          17 2018-01-23 14:58 (REG) data-files-extract.dmftar
-rw-------  1 sdemo002    sdemo002    52439040 2018-01-23 16:42 (DUL) data-stage.tar
```

Lets put the file `data-stage.tar` on the archive and remove it from disk. This can be done using `dmput` command. Then run the 'dmls' command again to see the change in the status of the migrated file.



```
dmput -r /archive/sdemo002/data-management/data-stage.tar
dmls -l /archive/sdemo002/data-management/

drwx------  3 sdemo002    sdemo002          17 2018-01-23 14:58 (REG) data-files-extract.dmftar
-rw-------  1 sdemo002    sdemo002    52439040 2018-01-23 16:42 (OFL) data-stage.tar
```


## Part2: Archiving using tar
Learn how to explore the basic options of tar and to pack a file or directory.

The tar tool is invoked by entering `tar` as a command, and is available to all users and in all directories.

<!--First open a terminal and login to LISA with your `sdemo<XXX>` accounts.

Login to the User Interface machine at Lisa:

```
ssh <username>@lisa.surfsara.nl 
```
-->
### Step 1: Create some data 

On Lisa, create a data directory called "data" and make it your working directory. Then create some big data files to work with. Run the truncate command to create 3 data files (data1, data2, data3) each of size 15 MB:

```
mkdir data
cd data
truncate -s 14M data1 data2 data3
```

### Step 2: Make tarballs 

First explore the options of tar command by

```
tar --help
```

To pack some files into a single tar file, the tar command is used followed by some options and these file names as arguments.

```tar [OPTIONS] <tarball_name>.tar <input-files..>```

**Exercise: Pack the files `data1`, `data2` and `data3` into the tar file `data.tar`**


Hints:

- Look for the create option
- Make sure the correct order of arguments are used and all required options are added.

**Solution:** Use the file names as argument:

```
tar -cf data.tar data1 data2 data3
```

**Exercise (BONUS): Pack the directory `data` into tar file `data-directory.tar`**

In order to pack several files stored in a directory, the tar command can be invoked by supplying the directory name as an argument.

Hint: use the command used in the previous excercise and replace the file name with a directory name.

**Solution:** First go one level up to see the data/ directory. Use the tar command with the directory name as an input argument:

```
cd ..
ls 
tar -cf data-directory.tar data/
```

### Step3: Extract tarballs

Learn how to inspect and unpack an existing archive file. Make sure that you first inspect the contents of a tar file, before extracting that.

**Exercise: List the contents of the tarball `data.tar`**

Before extracting any files from a tarball, it might be useful to inspect its contents.

Hint: look for the list option.

**Solution:** Use the `-t` option:

```
tar -tf data/data.tar
```


**Exercise: Unpack the contents of the file `data.tar` to the current directory**

Hint: look for the extract option.

**Solution:** Use the `-x` option with tar:

```
tar -xf data/data.tar
```
**Question:** Where is the extracted data stored?

**Task: Unpack the contents of the file `data.tar` located in the home folder of your archive service to a directory called `new`**

Hint: add an option that changes the directory. Use the `-C` option. First create the directory if it doesn't exist yet!

```
mkdir new
tar -xf data.tar -C new
```

### Step 4: Extract tallballs from Arcive

Before you can start copying or extracting data from the archive service directly, you need to stage the data from tape to the archive disk cache. The DMF (data migration facility) tools can be used which are available on the Lisa system.
To stage the data use the `dmget` command:

```
dmget –a <file>```

Copying and extracting will work without staging but can potentially take a huge amount of time when dealing with many files. So remember, when working directly with data stored on the archive, first stage it!


**Exercise (BONUS): Unpack the contents of the file `data-stage.tar` located in the data-management directory of your archive service to a directory called `new`**


Hint: add an option that changes the directory.

**Solution:** First list all files in your archive directory using `dmls`. Then stage the requested file from the archive to disk:

```
dmget -a /archive/<username>/data-management/data-stage.tar	
```

Extract the file `data-stage.tar` in your Archive to a new directory in your home folder on Lisa:

```
tar -xf /archive/sdemo002/data-management/data-stage.tar -C new
```


## Part 3: Archive using dmftar

Learn how to pack directory contents using the dmftar tool. dmftar is only available on the archive, Cartesius and Lisa services.

The dmftar tool is invoked by entering `dmftar` as a command. It is available to all users and in all directories. On the Lisa cluster, you need to load it as a module first:

```
module load dmftar
```
### Step 1: Create some more data

In your home directory, create a new `file` directory. Change your working directory to `file` and create some data (data4, data5, data6) using the `truncate` command. 

```
mkdir file
cd file
truncate -s 14M data4 data5 data6
```



### Step 2: Make dmftar archive files

First explore the options of dmftar command by

```
dmftar --help
```

Questions:

- Which options are available to get additional help?
- Are there any man pages available?
- What is the difference in command structure between tar and dmftar?
- Can you combine the options to a single option? E.g. `-a -b` becomes `-ab`?
- Which option is always required?

**Exercise: Pack the files `data4`, `data5` , `data6` into a dmftar archive called `data.dmftar`**

Hint: look for the create option.

**Solution:** Use the `-c` option:

```
dmftar -c -f data.dmftar data4 data5 data6
```

**Exercise: List the contents of the newly created dmftar folder using standard Linux tools.**

To understand the actual contents of a dmftar archive it is useful to have a look at which files are actually stored in the folders.

**Solution**

```
ls -l data.dmftar/0000

total 43028
-rw------- 1 narges narges 44052480 Nov  8 14:20 data.dmftar.tar
-rw------- 1 narges narges       53 Nov  8 14:20 data.dmftar.tar.chksum
-rw------- 1 narges narges      171 Nov  8 14:20 data.dmftar.tar.idx
```

**Exercise (BONUS): In your home diretory make a dmftar archive file or the tow other directories 
`data` and `file`.**

dmftar can easily process the contents of entire directories compared to the more file-oriented nature of tar. Therefore the tool is mostly used to pack individual directories.

**Solution:** You can pack the directories `data` and `file` into one dmftar archive file called `data-dir.dmftar`, by passing the directory names ad input arguments of the `dmftar` command.

```
cd ..
dmftar -c -f data-dir.dmftar data/ file/
```

### Step 3: Verify your archive

An important aspect of data archiving is data integrity. The archiving facility of SURFsara automatically checks the data stored on tape by comparing files every once in a while.

Archived folders created using dmftar can be verified based on a checksum that is stored within it.

**Exercise:  Verify the contents of the archive folder `data.dmftar` on the bit-level**

Hint: look for the verify option.

**Solution:** Add the `-V` option:

```
cd file/
dmftar -V -f data.dmftar

verifying checksum for 1 volumes
verifying checksum for volume #1
OK
```

### Step 4: Extracting dmftar archives

Learn how to inspect and unpack an existing archive folder using dmftar. This can be done without unpakking the folder.

**Exercise: List the contents of the archive folder `data.dmftar`**

Hint: look for the list option.

**Solution:** Use the `-t` option:

```
dmftar -t -f data.dmftar
```
**Questions**

- What type of contents can be seen in the dmftar archive?
- What is the alternative for option `-t`?

**Exercise: Unpack the contents of the archive folder `data.dmftar`**

Hint: look for the extract option.

**Solution:** Use the `-x` option:

```
dmftar -x -f file/data.dmftar/
```

**Questions**

- Which files and folder are created?
- Where is the dmftar file extracted?

Remove the unpacked data in your home folder:
```
rm data4 data5 data6
```


**Eercise (BONUS): Unpack only a single file from a dmftar archive folder. Unpack the file `data5` from the archive file `data.dmftar`**

Sometimes it is useful to only unpack specific files or folders from an archive file. Using dmftar this is easily accomplished by adding the file name or a name pattern as an argument to the command.

Hints:

- Use the `--help` option to learn how to add a file extraction pattern.
- Investigate in what subdirectory the file is located.

**Solution:** Add the file name and folder as the last argument in the command:

```
dmftar -x -f data.dmftar data5
```

**Questions**

- Can you extract the file by only adding the file name?
- Where is the extracted file stored?


**Exercise (BONUS): Unpack a specific directory from a dmftar archive folder. Repeat the previous bonus excercise to extract all files in the subdirectory `data` in the archive folder `data-dir.dmftar`**

**Solution:** Solely add the subdirectory name:

```
dmftar -x -f data-dir.dmftar data2/
```

### Step 5: Direct archive usage with dmftar 

You can use the `dmftar` command from Lisa to directly create dmftar archive files on the archive, or extract dmftar files from the archive.

**Exercise: Create a dmftar archive directory from the data directly on your Lisa account and directly put it on the archive**

**Solution:** 
To create dmftar archive file from Lisa on the archive, you should use the same dmftar command, but give a full path to the archive for the the dmftar file to be created 

```
dmftar -c -f /archive/<username>/data-management/data.dmftar files/
```

**BOPNUS Exercise: Unpack directly from the archive the contents of the archived folder**

Lets unpack the dmftar file `data-files-extract.dmftar` directly from the archive.

**Solution:** 
```
dmftar -x -f /archive/<username>/data-management/data-files-extract.dmftar
```



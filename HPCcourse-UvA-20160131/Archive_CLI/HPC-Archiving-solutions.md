# Archiving your data exercise solutions

## Introduction

Welcome to the hands-on part of archiving data using tar or dmftar in combination with the Data Archive of SURFsara. This file provides the solutions to the exercises found in [Archiving your data exercises](HPC-Archiving-exercises.md) module.

## Part 1: Basic tar usage

All exercises are done from the `~/tar` folder in your home folder.

### Exercise 1.a: The options of tar

Enter the command, optionally followed by an option showing additional help, e.g.:

```
tar --help
```

### Exercise 1.b. Pack files into a tarball

Use the file names as argument with the `-c` option:

```
tar -cf data-files.tar data1.dat data2.dat data3.dat
```

Alternatively use a wildcard to shorten the command:

```
tar -cf data-files.tar data*.dat
```

### Exercise 1.c. Pack a directory into a tarball

Use the directory name as an argument:

```
tar -cf data-directory.tar data/
```

### Exercise 1. (bonus) Pack by file list

Use the `-T` option:

```
tar -cf data-filelist.tar -T files.lst
```

## Part 2: Extracting tarballs (10 min)

All exercises are done from the `~/tar` folder in your home folder.

### Exercise 2.a: List the contents of a tarball

Use the `-t` option:

```
tar -tf data.tar
```

### Exercise 2.b. Unpack a tarball

Use the `-x` option with tar followed by the name of the tarball:

```
tar -xf data.tar
```

Another solution if you need specific files from the tarball:

```
tar -xf data.tar data*.dat
```

### Exercise 2. (bonus)

First stage the tarball on the archive service:

```
dmget -a /archive/<username>/data-stage.tar
```

Extraction: use the `-C` option. First create the directory if it doesn't exist yet!

```
mkdir new
tar -xf /archive/<username>/data.tar -C new
```

## Part 3: Basic dmftar usage (10 min)

All exercises are done from the `~/dmftar` folder in your home folder.

### Exercise 3.a. Show the options of dmftar

Enter the command, optionally followed by an option showing additional help.

```
dmftar --help
```

### Exercise 3.b. Pack a directory

Use the `-c` option:

```
dmftar -c -f data.dmftar data/
```

### Exercise 3. (bonus) Investigate the contents of a dmftar archive

Directly list the folder contents using the `ls` tool:

```
ls -l data.dmftar/0000
```

### Exercise 3. (bonus) Verify your archive

Add the `-V` option to the `dmftar` command:

```
dmftar -V -f data.dmftar
```

## Part 4: Extracting dmftar archives (10 min)

All exercises are done from the `~/dmftar` folder in your home folder.

### Exercise 4.a. List the contents of an dmftar archive

Use the `-t` option:

```
dmftar -t -f archived-data.dmftar
```

### Exercise 4.b. Unpack a dmftar archive

Use the `-x` option:

```
dmftar -x -f archived-data.dmftar
```

### Exercise 4. (bonus) Unpack only a single file from a dmftar archive

Add the file name and folder as the last argument in the command:

```
dmftar -x -f archived-data.dmftar data1/data1-1.dat
```

### Exercise 4. (bonus) Unpack a specific directory from a dmftar archive

Solely add the subdirectory name:

```
dmftar -x -f archived-data.dmftar data2/
```

## Part 5: Direct archive usage with dmftar (10 min)

All exercises are done from the `~/dmftar` folder in your home folder.

### Exercise 5.a: Archive directly to the archive service

Use the mounted folder `/archive/<username>`:

```
dmftar -c -f /archive/<username>/data-files.dmftar data*.dat
```

### Exercise 5.b. Unpack directly from the archive service

Extraction is done using the `-x` option with the mounted folder path `/archive/<username>`:

```
dmftar -x -f /archive/<username>/data-files.dmftar
```

### Bonus excercise: using dmftar with remote servers

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


# Data workflows and automation

## Learning objectives
- Reading external data from a repository
- Store resulting data safely at a remote site
- Explore the connection between self-written programs (here in Python) and commandline tools
 - Create folders and files and write unique filenames in Python
 - Call commandline code from within Python
- Build reusable code in Python

## Workflow that we will write in python
1. Download a large data file from **FigShare**
2. Save chunks of the data in single files
3. Create a tar-ball from the single files
4. Send the single files to the archive


## Python environment
Import python packages.
The packages *os* and *subprocess* are availbale with any python compiler. The package *pandas* is an extra package that needs to be installed separately.


```python
import os
import pandas as pd
import subprocess # python package to call commandline (bash) code
```

## 1. Downloading data from a repository
We will use python's *pandas* package to read in a remote *csv* file from an archive called **FigShare** and print out the first 20 lines.


```python
surveysDF = pd.read_csv('https://ndownloader.figshare.com/files/2292172')
print(surveysDF.head(20))

print(surveysDF.shape) # returns number of rows and number of columns
```

The data is rather large. Reading data can take a lot of time and in some cases we would like to extract chunks of the data for certain pipelines.
In the following we  will create single files for each year.

## 2. Creating directories and files from within python
With the package *os* we have access to functions that are equivalent to commandline functions e.g. *mkdir*, *ls*, *chmod*, ...
The file we downloaded contains 25 years of data and is very large. We would like to separate the data for each year into a separate file.
Let’s start by making a new directory inside the folder data to store all of these files using the module os:


```python
print(os.getcwd()) # get your current working directory
help(os.mkdir)
```


```python
os.mkdir('/home/FILL_IN/yearly_files')
print(os.listdir('/home/FILL_IN'))
```

Now we select only the data from the year 1977. We can access the column *year* and create a boolean vector that selects only rows of a specific year :


```python
print(surveysDF.year == 1977)
```


```python
# Select rows for 1977
surveys1977 = surveysDF[surveysDF.year == 1977]
print(surveys1977.tail(10)) # print the last 10 lines in the new data frame

# Write the new DataFrame to a csv file
surveys1977.to_csv('yearly_files/surveys1977.csv')
```


```python
print(os.listdir('/home/FILL_IN/yearly_files'))
```

## 3. Example: Calling commandline tools to connect to a different server
We have seen that we can download a public data file by URL from a different server. Now we will save some data in the archive which is not publicly accessible. We will call **rsync** from within python and store the file in your home-directory.


```python
p = subprocess.Popen('rsync -av yearly_files/surveys1977.csv FILL_IN@archive.surfsara.nl:', 
                     shell=True, stdout=subprocess.PIPE, stderr=subprocess.PIPE)
```

Type in your password and press **enter** twice. 
The string in the function *subprocess.Popen* is exactly what we would type on the commandline.

To see what the system did for us, type in the following command:


```python
print(p.communicate())
```

The *p.communicate* command fetches for us the logging and the error the issued command causes in the form of 

```sh 
('output', 'error')
```

## 4. Automatise the generation and remote saving of data
Now we can automatise the above steps to create single files for every year and sending them to the archive.

The string we passed to the *subprocess* function is pretty long. Parts of it can be reused when we are sending over other data. Let us safe the general parts in single variables:


```python
rsyncCmd = 'rsync -av ' #Watch out, we have a trailing space here!!
user = 'FILL_IN'
remoteSite = 'archive.surfsara.nl'
```

Now we create a list of all years that are listed in the data:


```python
years = surveysDF.year.unique()
print(years)
```

With the variables set above, we can automatise the steps we carried out previously with a for-loop and then send some of the data to the *remote site*:


```python
for year in years:
    subDF = surveysDF[surveysDF.year == year]
    subDF.to_csv('yearly_files/surveys'+str(year)+'.csv')

#Check the commandline string
#TODO send all data until the year 2000 to the archive using the command below.
cmd = syncCmd+'yearly_files/FILL_IN.csv '+user+'@'+remoteSite+':'
print(cmd)
p = subprocess.Popen(cmd, shell=True, stdout=subprocess.PIPE, stderr=subprocess.PIPE)
# To receive the output (error and logging) of the issued command we execute
print(p.communicate())
```

**Food for thought**: What is more covenient, copying the files in the loop or after the loop?
    

## Tarring of data
How would the *pprocess* command look like to tar the folder *yearly_data*? This is what we would type on the commandline:
`
tar -cf yearly_files.tar yearly_files/
`


```python
cmd = FILL_IN
print(cmd)
p = subprocess.Popen(cmd, shell=True, stdout=subprocess.PIPE, stderr=subprocess.PIPE)
print(p.communicate())
```

## 5. Building reusable and modular code with functions
We have explored the single commands line by line and we have seen how to automatise recurring parts with a for-loop. To make code reusable and generally applicable we can write functions that 

1. Create a tar-ball from a given folder
2. Send a file or a folder to remote server

### The tar function
Below you will find the function that creates a tar-ball *tarball* from a folder *directory*.
Fill in the missing parts.


```python
def tarDir(directory, tarball):
    # check whether directory exists and is a directory
    if not os.path.isdir(FILL_IN):
        print('Folder does not exist. Exiting')
        return False
    # check whether we are not overwriting an existing tar-ball
    if os.path.isfile(FILL_IN):
        print('Destination file already exists. Refuse to overwrite. Exiting')
        return False
    
    cmd = 'FILL_IN'+destination+' '+directory
    print(cmd)
    p = subprocess.Popen(cmd, shell=True, stdout=subprocess.PIPE, 
    		stderr=subprocess.PIPE)
    print(p.communicate())
    return True

```

### The rsync function
The function below sends data to a remote site. It takes two arguments

1. *source*, that can be the path to a file or a folder
2. *destination* which should have the form user@archive.surfsara.nl:destination/path

First we will check whether the destination on the remote server already exists. If that is not the case we will check whether the *source* is a folder or a file and create the respective command.

Depending on whether we would send a file or a folder we will adjust the rsync command.


```python
def syncToRemote(source, destination):
    # check whether the destination on the remote server does not exist, 
    # we do not want to overwrite existing data.
    # splitting 'destination' into the user@server part and the remote location part
    remote_host     = destination.split(':')[0]
    remote_loc      = destination.split(':')[1]

    # The bash command we would like to issue is
    # ssh <user>@archive.surfsara.nl "test -e destination"
    # 
    print('Checking destination: %s') %remote_loc 
    cmd = 'ssh '+remote_host+ ' "test -e '+remote_loc+'"'
    print(cmd)
    
    p = subprocess.Popen(cmd, shell=True, stdout=subprocess.PIPE, 
    		stderr=subprocess.PIPE)
    # p.communicate gives the same output no matter whether the destination 
    # exists or not.
    # However the function also fetches the system status of the command and 
    # saves it in p.returncode, which we can use to check the outcome.
    p.communicate() # Creates the output and sets the return code 
                    # 1 Fail (file does not exist)
                    # 0 Success (file exists)
    resp = p.returncode
    
    if resp == 0:
        print('Remote folder already exists: %s') %remote_loc
        return False
    else:
        print "Folder", remote_loc, "will be created on remote site."
        
    # check whether source exists and is a directory or a file:
    if os.path.isdir(source):
        cmd = 'rsync -var '+source+' '+destination
        print(cmd)
    elif os.path.isfile(source):
        cmd = 'rsync -va '+source+' '+destination
        print(cmd)
    else:
        print('No valid file or folder')
        return False
    p = subprocess.Popen(cmd, shell=True, stdout=subprocess.PIPE, 
    		stderr=subprocess.PIPE)
    # To receive the output (error and logging) of the issued command we execute
    print(p.communicate())
    
    return True
```

## 6. The workflow in python
With the two functions above we do not have to worry about the tarring and synchronisation to the archive server anylonger and can concentrate on the workflow itself.

```py
# Download data from FigShare
surveysDF = pd.read_csv('https://ndownloader.figshare.com/files/2292172')

# Do something with the data, some analysis
print(surveysDF.head(20))
print(surveysDF.shape) # returns number of rows and number of columns
    
# Save some results to files
myResDir = 'compute_output'
os.mkdir(myResDir)
years = surveysDF.year.unique()
for year in years:
subDF = surveysDF[surveysDF.year == year]
subDF.to_csv(myResDir+'/surveys'+str(year)+'.csv')
    
# Create a tar of the output files
tarDir(myResDir, myResDir+'.tar')
    
# Archive results
syncToRemote(myResDir+'.tar', 
	'FILL_IN@archive.surfsara.nl:+myResDir+'.tar'')
	
# Archive raw data
surveysDF.to_csv('raw_surveys.csv')
syncToRemote('raw_surveys.csv', 
	'FILL_IN@archive.surfsara.nl:+myResDir+'.tar'')
```

## 7. The full python script
The full python script for the workflow will look like the script below. This is a skeleton employing the functions above and simulating the workflow above in the function *main*.

```python
import os
import pandas as pd
import subprocess # python package to call commandline (bash) code

def tarDir(directory, tarball):
    # check whether directory exists and is a directory
    if not os.path.isdir(directory):
        print('Folder does not exist. Exiting')
        return False
    # check whether we are not overwriting an existing tar-ball
    if os.path.isfile(tarball):
        print('Destination file already exists. Refuse to overwrite. Exiting')
        return False
    
    cmd = 'tar -cf '+tarball+' '+directory
    print(cmd)
    p = subprocess.Popen(cmd, shell=True, stdout=subprocess.PIPE, 
    		stderr=subprocess.PIPE)
    print(p.communicate())
    return True


def syncToRemote(source, destination):
    # check whether the destination on the remote server does not exist, 
    # we do not want to overwrite existing data.
    # splitting 'destination' into the user@server part and the remote location part
    remote_host     = destination.split(':')[0]
    remote_loc      = destination.split(':')[1]

    # The bash command we would like to issue is
    # ssh <user>@archive.surfsara.nl "test -e destination"
    # 
    print('Checking destination: %s') %remote_loc 
    cmd = 'ssh '+remote_host+ ' "test -e '+remote_loc+'"'
    print(cmd)
    
    p = subprocess.Popen(cmd, shell=True, 
    		stdout=subprocess.PIPE, stderr=subprocess.PIPE)
    # p.communicate gives the same output no matter whether 
    # the destination exists or not.
    # However, the function also fetches the system status of 
    # the command and saves it in
    # p.returncode, which we can use to check the outcome.
    p.communicate() # Creates the output and sets the return code 
                    # 1 Fail (file does not exist)
                    # 0 Success (file exists)
    resp = p.returncode
    
    if resp == 0:
        print('Remote folder already exists: %s') %remote_loc
        return False
    else:
        print "Folder", remote_loc, "will be created on remote site."
        
    # check whether source exists and is a directory or a file:
    if os.path.isdir(source):
        cmd = 'rsync -var '+source+' '+destination
        print(cmd)
    elif os.path.isfile(source):
        cmd = 'rsync -va '+source+' '+destination
        print(cmd)
    else:
        print('No valid file or folder')
        return False
    p = subprocess.Popen(cmd, shell=True, 
    	stdout=subprocess.PIPE, stderr=subprocess.PIPE)
    # To receive the output (error and logging) of the issued command we execute
    print(p.communicate())
    
    return True
    
def main():
    # Download data from FigShare
    surveysDF = pd.read_csv('https://ndownloader.figshare.com/files/2292172')

    # Do something with the data, some analysis
    print(surveysDF.head(20))
    print(surveysDF.shape) # returns number of rows and number of columns
    
    # Save some results to files
    myResDir = 'compute_output'
    os.mkdir(myResDir)
    years = surveysDF.year.unique()
    for year in years:
        subDF = surveysDF[surveysDF.year == year]
        subDF.to_csv(myResDir+'/surveys'+str(year)+'.csv')
    
    # Create a tar of the output files
    tarDir(myResDir, myResDir+'.tar')
    
    # Archive results
    syncToRemote(myResDir+'.tar', 
    	'FILL_IN@archive.surfsara.nl:+myResDir+'.tar'')

if __name__ == "__main__":
    main()

```

# CAPP Development Module
[![Build Status](https://travis-ci.org/justincohler/capp_demo.svg?branch=master)](https://travis-ci.org/justincohler/capp_demo)

[Accompanying Slide Deck](https://docs.google.com/presentation/d/1Y7EJ8do0qUN05D7yQeSgReTaOUcA7bIBS4b_ZWVNtmo/edit?usp=sharing)

**Purpose**: This repository serves to help CAPP students refresh their skills with unix-style command line tools, file-system, and common python development setup requirements for collaborative development.

## Speed up your workflow
Before we get started, there are five basic CLI tips that will make the rest of the module (and your life) easier:

1. Tab-to-complete: the ```tab``` key will auto-complete (case-sentive) file/folder names after the first letter entered. When traversing directories, or typing long filenames, use ```tabs```. Note if several folder/files have the same beginning letters, tab twice to see the distinct matches. 
2. ```ctrl-a``` (beginning-of-line) and ```ctrl-e``` (end-of-line)
3. Tilde (```~```): represents your home directory
4. Wildcards (```*```): Looking for a file in your directory that starts with the letter 'B' or ends with "txt"? ```ls B*``` or ```ls *.txt``` to the rescue.
5. Last command (```!!```): the text of the last command. Example below:
    ```
    $> run /restricted_folder/restricted_file.py
    ERROR: Permission Denied
    $> sudo !!
    sudo run /restricted_folder/restricted_file.py
    ...
    ...
    Hello World!
    $>
    ```

**Note** - If you're curious about the details of any command going forward, use ```man <command_name>```, which brings up the linux (man)ual for that command.

***********************************************************
## Review Getting Around (Warmup)
### Cheatsheet

|     Command | Name                      | Description                              | Common arguments                                                 | Example                            |
| ----------: | :------------------------ | :--------------------------------------- | ---------------------------------------------------------------- | ---------------------------------- |
|    ```ls``` | List                      | list the contents of a directory         | ```<path>```, -a (all files, including hidden), -l (list format) | ```ls -al ~/```                    |
|   ```pwd``` | Present Working Directory | print complete path to terminal location |                                                                  | ```pwd```                          |
|    ```cd``` | Change Directory          | change terminal location                 | ```<path>```                                                     | ```cd /tmp/```                     |
|    ```cp``` | Copy                      | copy file/folder to location             | -r (recursively, for folders), ```<source>```, ```<dest>```      | ```cp -R my_folder /tmp/```        |
|    ```mv``` | Move/Rename               | move/rename a file/folder to location    | ```<source>```, ```<dest>```                                     | ```mv old_name.txt new_name.txt``` |
|    ```rm``` | Remove                    | remove a file/folder                     | -r (recursively), -f (force)                                     | ```rm -rf /tmp/my_folder```        |
| ```mkdir``` | Make Directory            | create folder(s)                         |                                                                  | ```mkdir dir1 /tmp/dir2```         |

### Exercise
* Run the following command to create an empty file named "one" in your user directory:
  ```
  touch ~/one
  ```
* Rename the file "two".
* Move it to the ```/tmp``` directory.
* Make a dirctory in ```/tmp``` called "test".
* Make a copy of the file in ```/tmp/test``` called "three" so that you have ```two``` in ```/tmp``` and ```three``` in your ```/tmp/test``` folder.
* Remove ```two``` from ```/tmp```.


***********************************************************
## Common Functions

### Viewing files (```head```,```tail```,```cat```)

Looking quickly at an entire file, or just a snippet can be done in several ways.

* ```head -n 5 my_file.txt``` - will print the first 5 lines of my_file.txt 
* ```tail -n 5 my_file.txt``` - will print the last 5 lines of my_file.txt
* ```cat my_file.txt``` - will concatenate all lines of the file to a location (stdout by default)

### Writing/Appending (```>/>>```)

Writing to a file can be done by simply appending output to a file with the ```>``` symbol. The example below prints "Hello World!" into a text file called ```hello_world.txt```.

```$> echo "Hello World!" > hello_world.txt```

Appending can be donw with double carrots (```>>```). 

#### Quick Exercise:
Add another line of text, "Hello World, again!" to the ```hello_world.txt``` file you just created.

### Aside
Sometimes you need to create a dummy file quickly. In this case, ```touch``` does just this. See a trivial example below

```touch empty_file.txt```
OR
```touch ~/Downloads/empty_file.txt```

### Piping
Piping is how we chain output from one command into the input of another command. Similar to ```%>%``` in R's tidyverse, pipes (```|```) act as a passthrough of several commands, reducing the number of intermediate variables needed to get something done.

A simple example below:
```ps -a | grep python```

This example first runs the "ps" function, which lists active processes running, then pipes the resulting processes into the "grep" function, which searches through the process list for active processes containing the term "python". You can run through the first of the two functions alone to see the raw output.

#### Exercise
* Use ```head``` and ```tail``` to print the stanza from lines 78-83 of the file ```the_raven.txt``` found in this repo. 
* Now, in one line, store that stanza in a file called ```stanza.txt``` (hint: use pipe(s))

### Killing Processes
The ```kill``` function offers the ability to kill processes that have hung. Kill has several options to kill gracefully (allowing memory to be cleaned up, connections to close, etc.) or harshly (terminating immediately). By default, kill will attempt to shutdown the process gracefully. ```kill -9 <process_name>``` will perform an immediate termination.

#### Exercise
Let's test it out! In this folder you'll find a python file named ```run_forever.py```. Take a look at the contents of this file and you'll find an infinite loop. 

* Run ```python run_forever.py &``` on the command line. This will execute the infinite loop in the backgroun (```&``` creates a background process)
* Use ```ps``` to find the hung file and then kill it. 

***********************************************************
## Bash Scripts **TODO**
if/fi
loops

***********************************************************
## Working with Paths
Paths can be confusing at first in Linux, and work slightly differently than on Mac, vastly differently than Windows. 

### Exporting variables, PATH
Exporting a variable is incredibly simple:
```
export HELLO_WORLD="Hello World!"
echo $HELLO_WORLD
```
Note that this is new variable ```HELLO WORLD``` is ephemeral, and will only last as long as your terminal session is open.

```PATH``` is a built-in variable that contains a colon-separated list of directories pointing to executable files. Adding to the path can be done as follows:
```export PATH=PATH:~/your_directory/```

### .bash_profile
There will be times when you want variables stored so that they can be used every time you open a new terminal. In this case, you want to add these variables to a file called ```.bash_profile```, located in your home directory. Let's add that ```HELLO_WORLD``` variable to our bash profile:

```
echo HELLO_WORLD="Hello World!" >> ~/.bash_profile
```

Close your terminal and reopen a new one. Verify the variable persisted by echoing it on the command line:
* ```echo $HELLO_WORLD```

### Other useful folders to acquaint yourself

* ```/tmp``` - a generic top-level folder (visible and write-able by all users on the computer) that deletes all contents on machine shutdown
*  ```/opt``` - a common location to put programs that do not auto-install (e.g. tomcat servers, etc.)
* ```/usr/local``` - where installed programs and packages reside 

***********************************************************
## Remote Shells (```ssh```)

SSH is a trust-based remote access system, one of the underlying ways of provisioning Amazon Web Services (AWS) and other cloud providers.

### Requirements
* a "public key" that can be shared with anyone (named ```id_rsa.pub``` by default)
* a "private key", never to be shared (named ```id_rsa``` by default)
* the public key stored in the host's ```authorized_keys``` file access to that host

### Why public and private keys?
When two users communicate over the internet, it is assumed that bad actors may listen to the wire communications. By combining your private key with another user's public key using clever elements of polynomials, you can establish trust via a confirmation message only the two users can decipher.

### Public and Private Keys
By default, keys live in ```~/.ssh```. If there are no keys in your user ssh folder, create keys via the following command-line program and follow the wizard (note that a password is not required):
```
$> ssh keygen
```
The above script will place a public and private "key pair" in your user's home directory (```~/.ssh```).

***********************************************************
### The ```config``` File
Creating ssh shortcuts for common locations can be done in the ```~/.ssh/config``` file, searched for by default by the ssh program. The basic format is shown below:
```
Host <nickname>
    User <username>
    Hostname <hostname>
```
There are plenty of other arguments, but for the sake of this exercise, these are all we need. 

### Exercise

* Paste the contents of your public key in the ```#capp-dev-module``` Slack channel.
* Create a new entry in your ```~/.ssh/config``` file (or a new file altogether) with the following details:
  * nickname: "capp-dev"
  * username: "ec2-user"
  * hostname: #TODO
* ssh into this host with ```ssh <nickname>```, in this case ```ssh capp-dev```. (Note this will only work once I've added your public key to the list of ```authorized_keys```)
* Create a new empty file with ```touch``` in the ec2-user's ```~/capp_ssh``` directory.
* Lastly, add another entry to your ```~/.ssh/config``` file to access your cs.uchicago.edu machine:
    * nickname: you pick!
    * username: your CNET_ID
    * hostname: linux.cs.uchicago.edu

***********************************************************
## Sending Files (```scp```)
Most of the time, you will want to send pre-packaged files to a server for unwrapping and execution (in the case of an app you've built to run in the cloud). In this case, without a nice user interface, ```scp``` is the tool for the job. 

### Exercise
Let's repeat the previous exercise, this time touching a new fil locally, then using ```scp``` to send it to the ec2 server.

Format:
```
$> scp <file/folder> <username>@<hostname>:<path_to_folder>
```

In this case, your command will look like the following:
```
scp <your_file> ec2-user@#TODO:~/capp_scp/
```
Your command should complete without error

***********************************************************
## Working with APIs (```curl```)
Most of us will work with APIs via python, using the "requests" library to get and post information from/to APIs. However, there may be cases where you want to quickly test an API endpoint, in which case knowing the basics of ```curl``` come in handy.

Download an http page
### Post a message to a simple flask app
I've set up a simple API to accept your messages from a ```curl``` statement. Try running the following command to send a ```json``` message to the website:

```
curl -d "{\"cnetid\": \"YOUR_CNET_ID\"}" -H "Content-Type: application/json" -X POST http://#TODO/api
```

**Note** this can take any json object so long as the server expects the format you send.

***********************************************************
## Python Environments
For different classes/jobs/projects, you will inevitably need to manage different versions of your python modules, python versions (e.g. 2.7 vs. 3.7) and the number of dependencies in your project. Enter the world of python virtual environments!

Virtual environments allow you to configure a project specific Python environment, complete with its own dependency set and python version. There are two main contenders:
* conda - a toolkit built-in to Anaconda (popularly known for Jupyter notebooks)
* virtualenv - a standalone pip installation that does basically the same thing

While ```virtualenv``` is better known today, it will likely be overtaken by ```conda```, as conda leverages native matrix operators to greatly improve the speeds of ```numpy```, ```pandas```, ```scikit-learn```, ```tensorflow```, etc.

### Creating an environment
Source info: https://conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html

Start by creating an environment for a given python version (usually this goes in the top level of your repository)
* ```conda create -n yourenvname python=x.x [--file requirements.txt]```
* ex: ```conda create -n capp_dev_module python=3.7```

Next, activate the new environment:
* ```source activate yourenvname```

Next, install packages (just like you would with pip):
* ```conda install -n yourenvname [package_name]```
* ex: ```conda install -n capp_dev_module scipy=0.15.0```

Deactivating an environment can be done via the following (in the active shell): 
```source deactivate```

Saving all installed requirements to a standard format can be done as follows:
```conda list -e > requirements.txt```

The point of saving requirements is to ensure your development team maintains a standardized list of dependencies that can be easily kept up to date. Be sure to keep your ```requirements.txt``` file in your repository, share with your teammates, and update these requirements regularly!

***********************************************************
## Further Virtualization (Shallow Dive) #TODO
* Vagrant
* Docker
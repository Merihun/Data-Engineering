### Why do we need shell scripts

There are many reasons to write shell scripts –

- To avoid repetitive work and automation
- System admins use shell scripting for routine backups
- System monitoring
- Adding new functionality to the shell etc.

### Advantages of shell scripts

- The command and syntax are exactly the same as those directly entered in command line, so programmer do not need to switch to entirely different syntax
- Writing shell scripts are much quicker
- Quick start
- Interactive debugging etc.

### Below are some basic General purpose shell terminal commands

## whoami: 
Display the name of the current user

## id
This command displays the user id and group id information of the current user.

## date
The date command displays current date and time.
## date "+%D"
displays current date in mm/dd/yy format.
Below are some of the popular format specifiers that you can try out.

  %d	Display the day of the month (01 to 31)
  %h	Displays abbreviated month name (Jan to Dec)
  %m	Displays the month of year (01 to 12)
  %Y	Display four-digit year
  %T	Display the time in 24 hour format as HH:MM:SS
  %H	Display the hour

## ls
- List the files and directories in the current directory.<br>
  <b>ls /bin</b> : list all the files in the /bin directory.<br>
  <b>ls /bin/b* </b> : List all files starting with b in the /bin directory.<br>
  <b>ls /bin/*r </b> : List all files ending with r in the /bin directory.<br>
- <b> ls -l</b> Prints a long list of files that has additional information compared to the simple ls command.
Here are some popular options that you can try with the ls command.
  -a	list all the files including hidden files
  -d	list directories themselves, not their contents
  -h	with -l and -s, print sizes like 1K, 234M, 2G etc
  -l	long listing of files which include information about permission, owner, size etc
  -F	classify files by appending type indcator like *,/ etc. to file names
  -r	reverse order while sorting
  -S	sort by file size, largest first
  -t	sort by time, newest first
- Example 
- <b>ls -la /etc</b> to get a long listing of all files in /etc, including hidden files, if any
- <b>ls -lt /etc</b> to list the files based on modifcation time, use -t option. The most recently modified file will be on top.This is more frequently used with -l option.
- <b>ls -ld /etc</b> to view the current directory attributes instead of their contents, use the following command. If you want any other directory's attributes, provide the directory name as argument.
- <b>ls -lS /etc</b> to list the files sorted by file size in descending order /etc, use -S option.
- <b>ls -lrS /etc</b> to get the files sorted by file size in ascending order in /etc , add -r option.
### Get basic information about the operating system

## uname
By default the command prints the kernel name. You will see Linux printed in the output.<br>
  <b> uname - a</b> : Using the -a opton prints all the system information in the following order:<br> 
    Kernel name, network node hostname, kernel release date, kernel version, machine hardware name, hardware platform, operating system.
#### Get information about active processes

## ps
<b> ps</b> lists the processes that are currently running and their PIDs (process ids).
The output contains the processes that are owned by you.<br>
  <b> ps -e </b> : The -e option displays all the processes running on the system. This includes processes owned by other users also.<br>

### Get information on the running processes and system resources
## top
- <b>top</b> command provides a dynamic real-time view of the running system.
- It shows the summary information of the system and the list of processes or threads which are currently managed by the Kernel.
- It gives information related to cpu and memory usage per process.
<img width="983" alt="image" src="https://user-images.githubusercontent.com/26862785/135751978-3cdbf6e6-f1de-494a-9ef3-605bef7575b0.png">
When started for the first time, you'll be presented with the following elements on the main top screen.

Summary Area. - shows information like system uptime, number of users, load average, memory usage etc.
Fields/Columns Header.
Task Area.
- The output keeps refreshing until you press <b> 'q' or Ctrl+c</b>
- If you want to exit automatically after a specified number of repetitions, use the -n option as in the 
    <b> top -n 10</b>:
- using 'top' we can find out which process is consuming the most resources. You can press the following keys while 'top' is running to sort the list :
  M - sort by memory usage 
  P - sort by CPU usage 
  N - sort by process ID 
  T - sort by the running time

### Display Messages
## echo
- echo command displays the given text on the screen. <br>
    echo "Welcome to the linux lab"
- These special characters help you better format your output.
  \n	Represents a newline character
  \t	A tab character
- Use the -e option of the echo command when working with special characters.
- echo -e "This will be printed \nin two lines"
### Download a file from the internet.
## wget
- wget command helps you to donwload a file at a given url.
- The below line of command downloads the file usdoi.txt from the given url.
  wget https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBM-DB0250EN-SkillsNetwork/labs/Bash%20Scripting/usdoi.txt
### View the Reference Manual
## man
- man command displays the user manual of the command given as argument.
- For example, to see the manual page of 'ls' command, use: <b> man ls</b>

# Directory Management Commands
### Print the name of your current working directory.

## pwd
### Create a directory
## mkdir
- mkdir creates a new directory.
- To create a directory named 'scripts' in your current directory, run this command: <b> mkdir scripts</b>
### Change your current working directory.
## cd
- To get into the directory scripts directory, run the command: <b> cd scripts</b>
- If you use cd without any directory name, it will move you back to your home directory.
- Run the command <b> cd ..</b>  to move to the parent directory. 
- <b>.. </b>is a shortcut that refers to the parent directory of your current directory.
### Delete directory
## rmdir

#### Search and locate files
## find

- Find command is used to search for files in a directory. You can search for files based on different categories like file name, file type, owner, size, timestamps etc. 
- Find command conducts the search in the entire directory tree starting from the directory name given.
- find /etc -name '*.txt' - This command finds all txt files in the subfolders of the /etc directory.

### Display the amount of disk space available on file systems

## df
- The 'df' command displays the information of device name, total blocks, total disk space, used disk space, available disk space and mount points on a file system.
- Without any arguments, the commands shows the information for all currently mounted file systems.
- <b>df -h</b> Use the -h option to view the disk space usage in human readable format, i.e, in megabytes, gigabytes etc.

### File Management Commands
## cat
- cat command displays contents of files.
- <b>cat usdoi.txt</b> command prints the content of the file usdoi.txt which you have downloaded earlier.

### Display file contents page-wise

## more

- The more command displays the file contents page by page.
- Press spacebar to display the next page.

#### Package and compress archive files

## zip

- zip command allows you to compress files.
- The following command creates a zip named config.zip and of all the files with extension .conf in the /etc directory.
  <b> zip config.zip /etc/*.conf </b>
- The -r option can be used to zip the entire folder.
- The following command creates an archive of the '/bin' directory.
    <b>zip -r bin.zip /bin </b>
### Extract, list, or test compressed files in a ZIP archive

## unzip

- <b>unzip -l config.zip</b> This command lists the files of the archive called 'config.zip': 
- <b>unzip bin.zip</b> This command extracts all the files in the archive bin.zip.
### Access Control Commands

- Each file/directory has permissions set for the file owner, group owner and others.
- The following permissions are set for each file:
    <b>read	r</b>
    <b>write	w</b>
    <b>execute	x</b>
- To see the permissions currently set for a file, run ls -l command.
- For example, to see the permissions for a file named 'usdoi.txt' in your current directory, run: <b>ls -l usdoi.txt </b>

A sample output looks like:

-rw-r--r-- 1 theia theia 8121 May 31 16:45 usdoi.txt

The permissions set here are 'rw-r--r--'

Here, owner has read and write permissions, group owner has read permission and others also have read permission.

## chmod

- <b>chmod</b> command lets you change the permissions set for a file.
- The change of permissions is specified with the help of a combination of the following characters: 
- <b>r, w and x	</b> representing <b>read, write and execute</b> permissions respectively
- <b>u,g and o</b>	representing user categories <b>owner, group and others</b> respectively
- <b>+, -	</b> representing <b>grant and revoke</b> operations respectively
- <b>chmod -r usdoi.txt</b> removes read permission for all (user,group and other) on usdoi.txt.
- <b>chmod +r usdoi.txt</b> removes read permission for all (user,group and other) on usdoi.txt.
- <b> chmod o-r usdoi.txt </b> removes the read permission for 'others' category.

### Text Processing Commands

## wc

- <b>wc usdoi.txt</b> If you want to find the number of lines, words and characters in a file, for example 'usdoi.txt'.
    Output:  <b>152 1330 8121 usdoi.txt152 </b>
- The output contains the number of lines followed by number of words followed by number of characters in the file.
- <b> wc -l usdoi.txt</b> Print only the number of lines in 'usdoi.txt'.
- <b> wc -w usdoi.txt</b> Print only the number of words in 'usdoi.txt'.
- <b> wc -c usdoi.txt</b> Print only the number of characters in 'usdoi.txt'.

### Perform search operations within the text
## grep
- grep command allows you to specify patterns and search for lines matching the pattern, from the input text.
- <b>grep people usdoi.txt</b> prints all lines in the file usdoi.txt which contain the word people.

Some of the frequently used options of grep are:
    -n	Along with the matching lines, print the line numbers also
    -c	Get the count of matching lines
    -i	Ignore the case of the text while matching
    -v	Print all lines which do not contain the pattern
    -w	Match only if the pattern matches whole words
    
- <b>grep -v login /etc/passwd</b> Prints all lines from the /etc/passwd file, which do not contain the pattern login.
- <b>grep "string" FILE_PATTERN</b> Checking for the given string in multiple files.
- exmple if we have two files named demo_file and demo_file1, the command <b>grep "this" demo_* </b> will grab all lines with the word "this".
- <b>grep -i "string" FILE</b> Case insensitive search using grep -i. Example <b>grep -i "the" demo_file</b>
- Additional reference:15 Practical Grep Command Examples In Linux / UNIX https://www.thegeekstuff.com/2009/03/15-practical-unix-grep-command-examples/

## sed
- Syntax: <b>sed OPTIONS... [SCRIPT] [INPUTFILE...] </b>
- SED command in UNIX is stands for stream editor and it can perform lot’s of function on file like, searching, find and replace, insertion or deletion.
- Though most common use of SED command in UNIX is for substitution or for find and replace. 
- By using SED you can edit files even without opening it, which is much quicker way to find and replace something in file, than first opening that file in VI Editor and then changing it.
- SED command in unix supports regular expression which allows it perform complex pattern matching.
Example:(display the content of geekfile.txt file) <br>
<b>$cat > geekfile.txt</b><br><br>

<b>unix is great os. unix is opensource. unix is free os.
learn operating system.
unix linux which one you choose.
unix is easy to learn.unix is a multiuser os.Learn unix .unix is a powerful.</b>

<b>$sed 's/unix/linux/' geekfile.txt</b>
output: 
linux is great os. unix is opensource. unix is free os.
learn operating system.
linux linux which one you choose.
linux is easy to learn.unix is a multiuser os.Learn unix .unix is a powerful.

- <b>$sed 's/unix/linux/2' geekfile.txt</b> Replacing the nth occurrence of a pattern in a line
output:
unix is great os. linux is opensource. unix is free os.
learn operating system.
unix linux which one you choose.
unix is easy to learn.linux is a multiuser os.Learn unix .unix is a powerful.
Addition Reference on SED: https://www.geeksforgeeks.org/sed-command-in-linux-unix-with-examples/

### Networking commands
## hostname
- Show the system's host name
- To view the current host name, run the command below .
- <b>hostname -i</b> You can use the -i option to view the IP adrress of the host:


### Test if a host is reachable
## ping

- ping www.google.com The command keeps sending data packets to the www.google.com server and prints the response it gets back. 
- (Press Ctrl+C to stop pinging)
- If you want to ping only for a limited number of times, use -c option. ping -c 5 www.google.com

### Display network interface configuration

## ifconfig

- Configures or displays network interface parameters for a network.
- Display the configuration of all network interfaces of the system:
## ifconfig eth0

- Display the configuration of the ethernet adapter.
- eth0 is usually the primary network interface that connects your server to the network.
- You can see your server's ip address in the line number 2 after the word inet.

### Transfer data from or to a server

## curl
- Access the file at the given url and display the contents on to the screen.
- curl https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBM-DB0250EN-SkillsNetwork/labs/Bash%20Scripting/usdoi.txt
- Access the file at the given url and save it in the current directory.

curl -O https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBM-DB0250EN-SkillsNetwork/labs/Bash%20Scripting/usdoi.txt



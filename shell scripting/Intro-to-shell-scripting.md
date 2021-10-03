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
List the files and directories in the current directory.<br>
  <b>ls /bin</b> : list all the files in the /bin directory.<br>
  <b>ls /bin/b* </b> : List all files starting with b in the /bin directory.<br>
  <b>ls /bin/*r </b> : List all files ending with r in the /bin directory.<br>
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

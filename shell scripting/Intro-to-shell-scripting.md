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
By default the command prints the kernel name. You will see Linux printed in the output.
  <b> uname - a</b> : Using the -a opton prints all the system information in the following order:<br> 
    Kernel name, network node hostname, kernel release date, kernel version, machine hardware name, hardware platform, operating system.

# All about CronJob

The cron is a software utility, offered by a Linux-like operating system that automates the scheduled task at a predetermined time. It is a daemon process, which runs as a background process and performs the specified operations at the predefined time when a certain event or condition is triggered without the intervention of a user.

It is started automatically from /etc/init.d on entering multi-user run levels. 

Syntax: 

cron [-f] [-l] [-L loglevel]
Options: 
 

-f : Used to stay in foreground mode, and don’t daemonize.
-l : This will enable the LSB compliant names for /etc/cron.d files.
-n : Used to add the FQDN in the subject when sending mails.
-L loglevel : This option will tell the cron what to log about the jobs with the following values: 
1 : It will log the start of all cron jobs.
2 : It will log the end of all cron jobs.
4 : It will log all the failed jobs. Here the exit status will not equal to zero.
8 : It will log the process number of all the cron jobs.

The crontab (abbreviation for “cron table”) is list of commands to execute the scheduled tasks at specific time. It allows the user to add, remove or modify the scheduled tasks. The crontab command syntax has six fields separated by space where the first five represent the time to run the task and the last one is for the command. 

Minute (holds a value between 0-59)
Hour (holds value between 0-23)
Day of Month (holds value between 1-31)
Month of the year (holds a value between 1-12 or Jan-Dec, the first three letters of the month’s name shall be used)
Day of the week (holds a value between 0-6 or Sun-Sat, here also first three letters of the day shall be used)
Command

Remember that a crontab schedule has 5 stars relation to the time periods minute, hour, day-of-month, month-of-year, day-of-week. For this task, assume Sunday is the 0th day rather than the 7th day (as in some unix systems).

Note that where all time periods are not specified in the instructions below, you can assume those time periods are 'every' (*).

There are a few things to keep in mind before scheduling cron jobs:

All cron jobs are scheduled in the local time zone in which the system where the jobs are being scheduled operates. This could be troublesome if jobs are being scheduled on servers with multinational personnel using it. Especially if the users also belong to countries that follow Daylight Savings Time practice.
All cron jobs run in their own isolated, anonymous shell sessions and their output to STDOUT (if any) must be directed to a file if we wish to see them.
All cron jobs run in the context of the user for which they were scheduled. It is therefore always good practice to provide an absolute path to scripts and output files to avoid any confusion and cluttering.

# Copying files to SMB server with robocopy

Below is the .bat file I've been using for copying files from one server to another.
Replace ```__dunder__``` texts with your own.

To run this .bat automatically I just create new simple task in Task Scheduler with
"Run whether user is logged in or not" option enabled. Please see 
[this fix regarding Task Scheduler permissions](https://github.com/mpuromaki/today-i-learned/blob/master/Windows/Configure-batch-as-background-task.md).

```
:: backup.bat

@echo off
echo ------------------------------------------------
echo Automatic system file backup.
echo ------------------------------------------------

net use R: \\__SERVER_IP_ADDR__\backup /u:__USER__ __PASSWORD__

@echo on
robocopy C:\BACKUP\ R:\__SERVER_FOLDER__\ /E /Z /FFT /MT:4 /W:10

@echo off
echo ------------------------------------------------
echo Finished. This window will close automatically.
echo ------------------------------------------------

exit 0
```

As you probably noted this script uses drive letter R: for the backup server shared folder.  
C:/BACKUP/ is the local folder from where data is copied. The system I use this with does couple things:
 - It creates backups in to that location with current date in the folder name.
 - It removes too old backup folders from that location.

Those things allow this robocopy operation to be so simple.

## What are those switches on robocopy?

```
/E    copy subfolders, including empty ones
/Z    Restartable mode, survives network outage
/FFT  more secure timestamps
/MT:4 multithreaded, 4 threads
/W:10 wait 10 between retries
```

Add /MOVE to move all files instead of copying.  
[Click Here to see proper guide for robocopy.](https://adamtheautomator.com/robocopy/)

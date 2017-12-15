---
layout: post
title:  "sh notes part 2"
date:   2017-05-10 08:36:46 +0900
categories: sh
---

--------sh 

//TODO copy vim macros


## TERM
```sh
export TERM=xterm
export TERM=vt220  for what???


xterm(coloring) superset of vt220
vt220(function keys work) is superset of vt100
( i think it is basic)
set
export TERM=xterm

then open "topas" and see the coloring.
```


## check dir exists and create
```sh
mkdir -p foo

[ -d foo ] || mkdir foo
```

## check the inode no.

```sh 
istat

$ istat abcd.txt
Inode 9802400 on device 101/1   File
Protection: rw-rw-r--
Owner: 1102(txxx3)           Group: 14(txxx)
Link count:   1         Length 762 bytes

Last updated:   Mon Aug 11 14:50:53 JST 2014
Last modified:  Mon Aug 11 14:50:53 JST 2014
Last accessed:  Mon Aug 11 15:07:20 JST 2014

or 

ls -il

```

## use of inode
```sh
1)
inode no. can be used if the file is created newly. 
for example 
	when the log file is created  newly for each time 
		or 
	the old log file is updated?
	
2) copy of the same file  also will have different inode number.
but cksum will be same.

$ cksum abcd.txt
361954219 762 abcd.txt
$
$ cksum copy_of_abcd.txt
361954219 762 copy_of_abcd.txt

# cksum changes as content changes
# but inode num does not change
	
```

## find using inode
```sh
$ find . -inum 2571705
```

## remove file by inode number
```sh
# 1) find first and confirm the result is as expected 
$ find . -inum 2571705 -exec ls -l {} \;

# 2) then do rm.
-rw-rw-r--    1 t24test3 t24test          12 Aug 25 14:40 ./-abcd
$ find . -inum 2571705 -exec rm {} \;

```

## remove files with spl. char
```sh
# remove file starting with -char

$ rm ./-abcd

       or

$ rm -- -abcd

       or
 
by inum using find

```

## understanding du command
```sh
$ du abcd.txt
  8       abcd.txt
$ ls -l abcd.txt
  -rw-rw-r--    1 txxx txx           1 Aug 26 16:55 abcd.txt
$ ls -ls abcd.txt
   4 -rw-rw-r--    1 txxx txx           1 Aug 26 16:55 abcd.txt
$ pwd
  /t24install/1C/bnk/bnk.run
$ du -k abcd.txt
  4       abcd.txt
$

$ du -a dir1
8       dir1/abcd_log
8       dir1/abcd_log_20140430
8       dir1/abcd_log_20140501
8       dir1/abcd_log_20140502
8       dir1/abcd_log_20140505
8       dir1/abcd_log_20140506
8       dir1/abcd_log_20140507
8       dir1/abcd_log_20140508
8       dir1/abcd_log_20140509
8       dir1/abcd_log_20140512
8       dir1/abcd_log_20140513
8       dir1/abcd_log_20140514
8       dir1/abcd_log_20140515
8       dir1/abcd_log_20140516
8       dir1/abcd_log_20140519
8       dir1/abcd_log_20140520
8       dir1/abcd_log_20140521
8       dir1/abcd_log_20140522
8       dir1/abcd_log_20140523
8       dir1/abcd_log_20140526
8       dir1/abcd_log_20140527
8       dir1/abcd_log_20140528
8       dir1/abcd_log_20140529
8       dir1/abcd_log_20140530
8       dir1/abcd_log_20140602
8       dir1/abcd_log_20140613
8       dir1/abcd_log_20140616
224     dir1

totally 27 files + 1 dir ( 28*8 => 224 )

$ du -s dir1 ( -s is same as default )
224     dir1
$ du -ks dir1
112     dir1
$ du dir1
224     dir1
```


## list only dirs
```sh
$ ls  -l | grep ^d | grep .*abcd.*
drwxrwxr-x    2 txxxx txx        4096 Aug 19 09:30 abcd1
drwxrwxr-x    2 txxxx txx        4096 Aug 13 16:26 abcd2
drwxrwxr-x    2 txxxx txx         256 Aug 13 16:24 abcd3
drwxrwxr-x    7 txxxx txx         256 Aug 26 09:42 abcd4
drwxr-x---    8 txxxx txx        4096 Aug 08 16:18 abcd5
drwxrwxr-x    5 txxxx txx         256 Aug 14 13:53 abcd6
drwxr-x---    5 txxxx txx        4096 Jul 02 11:49 abcd7
drwxrwxr-x    2 txxxx txx         256 Jul 22 11:02 abcd8
drwxrwxr-x    2 txxxx txx         256 Jul 22 11:02 abcd9
drwxrwxr-x    2 txxxx txx      180224 Jul 24 16:45 abcd10
drwxrwxr-x    2 txxxx txx         256 Jul 23 09:03 abcd11
drwxrwxr-x    2 txxxx txx      270336 Jul 24 17:11 abcd12
drwxrwxr-x    2 txxxx txx         256 Jul 23 09:03 abcd13
drwxrwxr-x    3 txxxx txx         256 Aug 15 13:48 abcd14
drwxr-x---    6 txxxx txx        4096 Aug 21 16:48 abcd15
drwxrwxr-x    2 txxxx txx         256 Aug 06 17:00 abcd16
drwxrwxr-x    2 txxxx txx        4096 Aug 25 15:19 abcd17
drwxrwxr-x    2 txxxx txx        4096 Aug 26 15:18 abcd18
drwxrwxr-x    2 txxxx txx         256 Aug 11 13:25 abcd19
$ ls  -l | grep ^d | grep .*abcd.* | wc -l
      19
$ ls -dl abcd*/ | wc -l
      19
```
## grep NOT print grep itself)
```sh
ps -aef | grep abcd | grep -v grep 
```

## find  findings!!!

```sh
$ find . -mmin -60 -exec ls {} \;
./abcd


$ ls -l
total 40
-rw-rw-r--    1 txxxx txx          68 Aug 28 08:54 abcd
-rw-rw-r--    1 txxxx txx          15 Aug 27 11:29 abcd1
-rw-rw-r--    1 txxxx txx          15 Aug 27 11:29 abcd2
-rw-rw-r--    1 txxxx txx          15 Aug 27 11:29 abcd3
-rw-rw-r--    1 txxxx txx          15 Aug 27 11:30 abcd4


$ find . -mmin -60 -exec basename {} \;
abcd

$ find . -mmin -60 -exec basename {} \; | xargs ls -l
-rw-rw-r--    1 txxxx txx          68 Aug 28 08:54 abcd

$ find . -mmin -60 -exec ls -l {} \;
-rw-rw-r--    1 txxxx txx          68 Aug 28 08:54 ./abcd

$ find . -mmin  +60
.   --> please note the current dir
./abcd1
./abcd2
./abcd3
./abcd4
$
$
$ find . -mmin  +60 -type d
.
$ find . -mmin  +60 -type f
./abcd1
./abcd2
./abcd3
./abcd4
```

```sh
du  -> user level  only

df -> file system level ( metadata like 
    
    refer ibm doc  "ibm why du -s and df disagree"


$ du -s abcddir
680     abcddir
$
$  df abcddir
Filesystem    512-blocks      Free %Used    Iused %Iused Mounted on
/dxx/orkkk  150732800  17341112   89%  1711878    22% /txxxx
$
$
$ echo $((150732800-680))
150732120
$
```

## is it so ? (df total - du used > df free)
```sh
$ if [ 150732120 -gt 17341112 ]; then echo "du reports less, so 'df total - du used > df free'"; fi
du reports less, so 'df total - du used > df free'
$

# df vs du

※: if a file is deleted by application, 
even before the application exit, 
du will show it as a free space,
but df will not show it as a free space until the application exit.
```

## ps
```sh
$ echo $$
14352812
$
$
$ ps -T 14352812
      PID    TTY  TIME CMD
 14352812  pts/4  0:00 sh
  3801772  pts/4  0:00     \--ps

also use -f flag
$ ps -f -T 14352812
     UID      PID     PPID   C    STIME    TTY  TIME CMD
txxx 14352812 23659018   0 10:37:27  pts/4  0:00 sh
txxx 54001698 14352812   4 10:45:27  pts/4  0:00     \--ps -f -T 14352812

# man PSから抜粋 (抜粋: ばっすい EXCERPT)

 -T pid
            Displays the process hierarchy rooted at a given pid in a tree format using ASCII art. This flag can be used in
            combination with the -f, -F, -o, and -l flags.
```

## 0 or 1 
```sh
echo $((2512+228==2740))
1 => means true
※: 0 means false
```

## AIX ( Advanced Interactive eXecutive ) a i ex

## to create dir if does not exist ( 
```sh
# note down the double pipe 
[ -d abcddir1 ] || mkdir abcddir1

# another way  use of -p switch
mkdir -p abcddir1

# TODO need to check the exit status of above two commands1
```


## unix chat tools
```sh
talk
wall
```

## grep does not match its own command ( //TODO how ? )

```sh
$ ps -aef | grep qdaemon
    rxxt  6422744  4063276   0   Dec 03      -  0:00 /usr/sbin/qdaemon
txxx 56951074 56623488   0 11:20:49  pts/2  0:00 grep qdaemon
$
$ ps -aef | grep [q]daemon
    rxxt  6422744  4063276   0   Dec 03      -  0:00 /usr/sbin/qdaemon
```

## pass awk output to command using xargs

```sh
$ ls -lt | head -2 | tail -1 | awk '{print $9}'
log.ERROR.20140902-133811.28377530
$ ls -lt | head -2 | tail -1 | awk '{print $9}' | xargs cat

........
Log file created at: 2014/09/02 13:38:11
Running on machine: abcdmc
F_PROTOCOL by 20 in tablespace ddd
...........
```

## space at end of line.
```sh
# teraterm window vs teraterm log

#copy from teraterm window ignores trailing spaces

copy cat ouput from teraterm  window to notepad.
there will be no trailing space.

view teraterm log output file, to find trailing space.
```

## to remove the spl. char( files containing &lt;&lt; chars ) files
```sh  
  ls -l *\<\<*
  rm *\<\<*
```

## errpt
```sh
#Generates a report of logged errors.
/var/adm/ras/errlog

errpt -i /var/adm/ras/errlog

or

errpt

# to start errdemon 
errdemon

```

## remove duplicates
```sh
# remove duplicates with sort
sort -u  filename.txt

# remove duplicate without sort
awk '!x[$0]++' filename.txt

#also excel remove duplicates

# fgrep
--binary-files=text => o/p binary text
-r => recursive to subdir

$ fgrep --binary-files=text -n 'this is st'  hw.out
...(bin o/p)
...

$ fgrep  -n 'this is st'  hw.out
Binary file hw.out matches


# uniq

abcd.txt
---
thi
this
this
is
fun
---
# uniq -c (only take the adjacent lines to uniq... before this sort, if you want true uniq)
$ uniq -c abcd.txt 
   1 thi
   2 this
   1 is
   1 fun
   
$ uniq -c abcd.txt | awk '{print $2}'
thi
this
is
fun

# ( print only unique)
$ uniq -u abcd.txt 
thi
is
fun

# display only the duplicate item
$ uniq -d count.txt
10

$ uniq -D count.txt
uniq: Not a recognized flag: D

-D option to print duplicate items as it occurs ( but not found in unix)
in linux it is supposed to display as ( 8 times 10 )

10
10
10
10
10
10
10
10

uniq -s skip n chars
-f skip n fields
-w compares first n chars

```


## stty key settings

```sh
$ dsds^H^H^H
sh: dsds^H^H^H:  not found.
$
$
$ stty erase '^H'
$
$ dsds


stty erase '^H' => backspace to delete
stty erase '^?' => delete key to delete a char
note only either of one only will work

stty sane
stty reset
can be used to reset terminal

ctrl+H => delete the char

ctrl+J => Enter key

$ stty -a
speed 9600 baud; 43 rows; 129 columns;
eucw 1:1:0:0, scrw 1:1:0:0:
intr = ^C; quit = <undef>; erase = ^H; kill = ^U; eof = ^D; eol = ^@
eol2 = ^@; start = ^Q; stop = ^S; susp = <undef>; dsusp = <undef>
reprint = ^R; discard = <undef>; werase = ^W; lnext = <undef>
-parenb -parodd cs8 -cstopb hupcl cread -clocal -parext
-ignbrk brkint ignpar -parmrk -inpck -istrip -inlcr -igncr icrnl -iuclc
-ixon -ixany -ixoff imaxbel
isig icanon -xcase echo echoe echok -echonl -noflsh
-tostop echoctl -echoprt echoke -flusho -pending iexten
opost -olcuc onlcr -ocrnl -onocr -onlret -ofill -ofdel tab3

※ 43 rows , 129 columns the current screen size

```

```sh

umask 0 => set rwx for all
umask -S  => displays the corresponding rwx format

chmod +rwx file.txt => then umask is applied for each u , g , o
                       since NO u/g/o is mentioned in chmod switch

check for Non stop module

find inode ...using istat 
what is diff. between 
$ istat hw.c
Inode 9570218 on device 101/1   File
Protection: rw-rw-r--
Owner: 1102(txxxx3)           Group: 14(txxxx)
Link count:   1         Length 181 bytes

Last updated:   Fri Jan 30 10:34:01 JST 2015
Last modified:  Fri Jan 30 10:34:01 JST 2015
Last accessed:  Fri Jan 30 10:39:00 JST 2015


----------------


inode, inumber
 |
 +-------> refers to data structure.
remove file by inumber


$ ls -laila ( to remember easily)
total 40
9569968 drwxrwxr-x    2 txxxx3 txxxx         256 Jan 30 10:34 .
9570299 -rw-rw-r--    1 txxxx3 txxxx         181 Jan 30 10:34 .(hwinp.c
9569959 drwxrwxr-x    5 txxxx3 txxxx         256 Jan 14 15:04 ..
9569969 -rw-rw-r--    1 txxxx3 txxxx         117 Jan 14 13:44 MyGetPpid.c
9569971 -rwxrwxr-x    1 txxxx3 txxxx        4520 Jan 30 10:33 a.out
9570218 -rw-rw-r--    1 txxxx3 txxxx         181 Jan 30 10:34 hw.c
$


fsck ==> file system check and repair
        can be done based on inode number

fsck
    -y assumes yes response to all questions
    -i inode number
    -p prevent display of messages

df - %IUsed . when reaching max
    use smitty chfs

    smitty ( System Management Interface Tool)          
            --> in the option select "Allow small inode extents"
```


## awk 
```sh
word count using awk

syntax:
--------
awk '/pattern/ {x++} END {print x}' file.txt

---------------------


$ awk '/ksh/ {count["cnt"]++} END {print count["cnt"]}' sample.txt
41
$ awk '/ksh/ {x++} END {print x}' sample.txt
41
$ awk '/ksh/' sample.txt | wc
      41      41    1996



* print the even lines.
awk 'NR % 2 == 0 {print $0}' sample.txt

* print the no.of even lines
$ awk 'NR%2==0 {x++} END {print x}' sample.txt
※ be careful not to print $x, it is without dollar

* printing the env. variable using awk
$ awk -v x=$JAVA_HOME '{print x}'
1
/usr/java6_64
2
/usr/java6_64


* can you tell the diff. between the following two lines of awk
$ awk 'NR%2==0 x++ END {print x}' sample.txt
52
$ awk 'NR%2==0 {x++} END {print x}' sample.txt
26

with out curly brace {x++},
NR%2==0 x++ are separate commands
so x++ will be executed for all lines.
if you put NR%2==0 {x++}
then x++ will be executed for even lines only.

$ ls -l |  awk ' total+=$5 END {print total}'
-rw-rw-r--    1 txxxx3 txxxx        2364 Feb 18 13:32 sample.txt
-rw-rw-r--    1 txxxx3 txxxx        1996 Feb 18 13:33 sample2.txt
4360

it is better to place bracket
$ ls -l |  awk ' {total+=$5} END {print total}'
4360
now note ls -l output is not listed once we place the {}, since default prnit $0 is omitted.

```
## Sticky Bit 
```sh
load in to swap space and stick it there For faster loading in RAM

s +4 => run file as owner

s +2 => run file as group

t +1 => on dir, only the dirs owner / file owner can mv , delete file in that dir
/tmp dir
```

## cat -ven
```sh
$ cat -ven  myst1.c
v -> verbose
e -> ending with $
n -> line #
```

## grep multibyte char
```sh
grep --color='auto' -n -P "[\x80-\xFF]" <filename>


$ grep --color='auto' -n -P "[\x80-\xFF]" abcd.txt
10:                笆停亦笆稚his is div 2
```
---------

## lfile.sh
```sh
l_serverip=10.xxx.xxx.xxx
l_username=t24test3

l_content=dhshdjsdsjdakd342423
l_filename=abcd4.txt

# beware of {} curly braces
# if it is ${r_content}, then in the rfile it has to be ${r_content}
# if it is ${rcontent}, then in the rfile it has to be ${rcontent}

script=$(sed -e "s|\${r_content}|$l_content|" -e "s|\${r_filename}|$l_filename|" rfile.sh)

ssh ${l_username}@${l_serverip} $script
```

## 2016/11/16:
```
~~~~~~~~~~~
    when doing $script in the ssh, if there is ioctl system call error,
    surround the $script with " (double quotes.)
    probably with 3 double quotes.
    """ (the middle one representing the actual double quote)
```
---------------

## rfile.sh
```
# beware of {} curly braces
# if it is ${r_content}, then in the lfile it has to be ${r_content}
# if it is ${rcontent}, then in the lfile it has to be ${rcontent}

echo ${r_content} > ${r_filename}

call lfile from new6d to run rfile.sh in new1C.
```
----------------

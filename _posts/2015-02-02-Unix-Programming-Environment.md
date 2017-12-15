---
layout: post
title:  "Unix: Programming envinronment"
date:   2015-02-02 07:28:46 +0900
categories: Unix 
---

# unix: Programming envinronment:

## history
```txt
1970:
    unix written in assembly

1972/73:
    Ritchie designed C language.

    Ritchie and Thompson rewrote unix in C

    ( breaking the tradition that system software is normally
         written in assembly lang. )

    No. of unix installations : 10 ( more expected ! )

Unix : not acronym.


No single program make it popular.

But the philosophy "approach to programming" make it easiser.

Combining many programs ( doing trivial tasks in isolation ),
    => results useful tool.
```
## Chapter 1: Unix for beginners
----------
```txt
    * time sharing operating system
    * controls peripheral devices ( discs, terminals, printers, etc )
    * provides file system that manages programs, data/docs.

    Unix includes
        *       NOT only kernel
        * but, complier, editor, command lang, pg for copying, printing files.
```

## 1.1. Getting started:
---------------------
```txt
    * Unix system : full duplex. ( typed char is sent to system ,
        and sent back to terminal.)
    * echo process copies chars back to screen
    * when typing password, echo process is turned off.
    * control char: ENTER char ( CTRL + M)
        ctrl m does not work as enter key in sublime
        check the ctrl m inside the brackets below.
        (  dfsds dsds)

        check the ctrl+m in the notepad.
    * ctrl+d => no more input
    * ctrl+g => telephone mani pol siripaval ivala....hhehehe
    * ctrl+h => backspace
    * ctrl+i => tab/indent

    * DELETE => RUBOUT
    * BREAK  => INTERRUPT

    CTRL+C / DELETE key => program stops immediately, without waiting to finish.


stty:
    Sets, resets, and reports workstation operating parameters.

stty -a
    => full listing

write
    command for sending messages

mesg ???
```

## myriad meaning:
---------------
```
    : unit of thousand

    n no of. items

    ex: There are myriad programs that manipulate files.
```

## ed command
----------
```
ed
a => append
...
...
...
. => end
w <filename/>
nn ( no of chars will be shown)
q
```

******************************************************************************

## ls commands

```sh
ls -l

$ ls -l
total 96 -------------> no of blocks . NOT no. of files.

    1 block size is 512 or 1024 chars ( bytes)

-rwxrwxr-x    1 txxxx txx            4518 Jan 04 2016  a.out
-rw-rw-r--    1 txxxx txx               7 May 10 13:46 b.out
drwxrwxr-x    2 txxxx txx             256 Mar 11 2016  c2
-rw-rw-r--    1 txxxx txx              27 Sep 26 13:09 fun.txt
-rw-r-----    1 txxxx txx             435 May 10 15:55 i2a.c
-rw-rw-r--    1 txxxx txx               0 May 10 13:24 myalloc2.c
-rw-rw-r--    1 txxxx txx               0 May 10 13:24 mycall.c
-rw-rw-r--    1 txxxx txx               0 May 10 13:24 mysort.c
drwxrwxr-x    2 txxxx txx            4096 Sep 12 14:59 redis
drwxr-x---    2 txxxx txx            4096 Sep 21 13:48 sample
-rw-r-----    1 txxxx txx             156 Dec 28 2015  sample.c
drwxrwxr-x    2 txxxx txx            4096 Aug 17 09:07 sds
drwxr-x---  ---> 4 txxxx txx            4096 Sep 26 13:19 sis
-rw-rw-r--  |  1 txxxx txx              47 Aug 19 16:45 test.sh
-rwxrwxr-x  |  1 txxxx txx              84 Jan 28 2016  tst_src.sh
$           |
            |
            +--> 4 ( no of links)


$ ls -li
total 96
148774 -rwxrwxr-x    1 txxxx txx            4518 Jan 04 2016  a.out
3335597 -rw-rw-r--    1 txxxx txx               7 May 10 13:46 b.out
4705823 drwxrwxr-x    2 txxxx txx             256 Mar 11 2016  c2
148940 -rw-rw-r--    1 txxxx txx              27 Sep 26 13:09 fun.txt
148775 -rw-r-----    1 txxxx txx             435 May 10 15:55 i2a.c
3335407 -rw-rw-r--    1 txxxx txx               0 May 10 13:24 myalloc2.c
3335408 -rw-rw-r--    1 txxxx txx               0 May 10 13:24 mycall.c
3335409 -rw-rw-r--    1 txxxx txx               0 May 10 13:24 mysort.c
5128998 drwxrwxr-x    2 txxxx txx            4096 Sep 12 14:59 redis
5423424 drwxr-x---    2 txxxx txx            4096 Sep 21 13:48 sample
148773 -rw-r-----    1 txxxx txx             156 Dec 28 2015  sample.c
3031200 drwxrwxr-x    2 txxxx txx            4096 Aug 17 09:07 sds
        6091456 drwxr-x---    4 txxxx txx            4096 Sep 26 13:19 sis
148985 -rw-rw-r--    1 txxxx txx              47 Aug 19 16:45 test.sh
148984 -rwxrwxr-x    1 txxxx txx              84 Jan 28 2016  tst_src.sh


6091456 => inum of sis has 4 links

that means 4 - 2(const. i.e. dir by default has 2 links) => 2 subdirs for sis

$ ls -laid sis/*/
6091203 drwxrwxr-x    3 txxxx txx            4096 Sep 26 13:17 sis/classes/
1876493 drwxrwxr-x    2 txxxx txx             256 Sep 26 13:19 sis/firstleveldir/

$ ls -lai sis/classes/
total 136
6091203 drwxrwxr-x    3 txxxx txx            4096 Sep 26 13:17 .
    ===> refers to sis dir 6091456 drwxr-x---    4 txxxx txx            4096 Sep 26 13:19 ..
6091227 -rw-rw-r--    1 txxxx txx             473 Mar 30 2016  Cast.class
6091237 -rw-rw-r--    1 txxxx txx             620 Apr 06 09:12 ConvertStr2Integer.class
6091236 -rw-rw-r--    1 txxxx txx             150 Apr 06 09:12 ConvertStr2IntegerIntf.class
6091225 -rw-rw-r--    1 txxxx txx            1043 Mar 29 2016  FailFast.class
6091226 -rw-rw-r--    1 txxxx txx             457 Mar 30 2016  IntMax.class
6091228 -rw-rw-r--    1 txxxx txx             677 Mar 31 14:12 MyArrayList.class
6091231 -rw-rw-r--    1 txxxx txx             693 Mar 31 16:20 MyFor.class
6091234 -rw-rw-r--    1 txxxx txx             711 Apr 01 09:22 MyGen.class
6091204 -rw-rw-r--    1 txxxx txx             265 Mar 28 2016  MyIterator.class
6091205 -rw-rw-r--    1 txxxx txx             308 Mar 28 2016  MyListIterator.class
6091239 -rw-rw-r--    1 txxxx txx             577 Apr 07 13:17 MyQuickSort.class
6091230 -rw-rw-r--    1 txxxx txx            1399 Apr 01 14:25 MySort.class
6091235 -rw-rw-r--    1 txxxx txx            1132 Apr 06 09:12 MySort2.class
6091229 -rw-rw-r--    1 txxxx txx            1230 Mar 31 09:39 Performance.class
6091238 -rw-rw-r--    1 txxxx txx             963 Apr 05 16:08 RemDup.class
1876492 drwxrwxr-x    2 txxxx txx             256 Sep 26 13:17 onemoredir

$ ls -lai sis/firstleveldir/
total 8
1876493 drwxrwxr-x    2 txxxx txx             256 Sep 26 13:19 .
6091456 drwxr-x---    4 txxxx txx            4096 Sep 26 13:19 ..     ===> refers to sis dir
```

******************************************************************************

## no of chars in file.[ CR(od) LF(0a) ]
```sh
# ls -l --> shows no. of chars in file.
$ ls -l
total 96
-rwxrwxr-x    1 txxxx txx            4518 Jan 04 2016  a.out
-rw-rw-r--    1 txxxx txx               7 May 10 13:46 b.out
drwxrwxr-x    2 txxxx txx             256 Mar 11 2016  c2
-rw-rw-r--    1 txxxx txx              27 Sep 26 13:09 fun.txt ---------------> 27 is the no of chars in the file
-rw-r-----    1 txxxx txx             435 May 10 15:55 i2a.c
-rw-rw-r--    1 txxxx txx               0 May 10 13:24 myalloc2.c
-rw-rw-r--    1 txxxx txx               0 May 10 13:24 mycall.c
-rw-rw-r--    1 txxxx txx               0 May 10 13:24 mysort.c
drwxrwxr-x    2 txxxx txx            4096 Sep 12 14:59 redis
drwxr-x---    2 txxxx txx            4096 Sep 21 13:48 sample
-rw-r-----    1 txxxx txx             156 Dec 28 2015  sample.c
drwxrwxr-x    2 txxxx txx            4096 Aug 17 09:07 sds
drwxr-x---    4 txxxx txx            4096 Sep 26 13:19 sis
-rw-rw-r--    1 txxxx txx              47 Aug 19 16:45 test.sh
-rwxrwxr-x    1 txxxx txx              84 Jan 28 2016  tst_src.sh


$
$ wc fun.txt
       4       6      27 fun.txt


download the file in ascii mode to windows, and check the no of bytes.

c:\dhans\temp99>dir fun.txt
 ドライブ C のボリューム ラベルは Windows7_OS です
 ボリューム シリアル番号は A41D-0AB2 です

 c:\dhans\temp99 のディレクトリ

2016/09/26  13:09                31 fun.txt
               1 個のファイル                  31 バイト
               0 個のディレクトリ  405,779,013,632 バイトの空き領域

c:\dhans\temp99>

31 bytes - 27 bytes => 4 bytes diff means 4 lines in the file.

that is due to download from unix to windows , 0a converted to 0d0a
4 0ds increase results 4 lines !!!
```

******************************************************************************

## japanese chars
```sh
btw, single japanese char has 3 bytes, ??? ( expected 2 bytes , not sure
have to study again ?!?!?!?)

$ had fun.txt
        Hex/Ascii dump of 'fun.txt'

000000    e3 82 84 0a             -                            ....
$
$ cat fun.txt
や
$
```

******************************************************************************


## ed
```sh
1,$p => print all lines ( from line 1 to last line)

1p => print first line
2p => print second line
```

******************************************************************************


## paste
```sh
$ paste fun.txt jik.txt
this    THIS
is      IS
fun     FUN
$
$
$ cat fun.txt
this
is
fun
$
$ cat jik.txt
THIS
IS
FUN
$
```
******************************************************************************

## pr (print format)
```sh
$ pr -3 fun.txt --> three column format

Mon Sep 26 14:55:50 JST 2016 fun.txt Page 1

this                    is                      fun

# ******************************************************************************

pr is not for formatting.

formatting commands:

nroff / troff

grep :
:g/re/p => global regular expression and print.

in vim
:g/re/p => lists the lines.

```

******************************************************************************


## sort
```sh
-n => numeric
-r => reverse
-f => fold UPPERCASE and lowercase together
```
******************************************************************************

## head tail
```sh
$ cat fun.txt
this
is
fun
$
$ tail -1 fun.txt
fun
$
$ head -2 fun.txt
this
is
$
$ head +1 fun.txt
Usage: head [-Count | -n Number | -c Number] [File...]
$
$ tail +1 fun.txt
this
is
fun
$ head -1 fun.txt
this
$

```
**<<<<<<<<<<<<<<<<NOTE : head does not have +ve no. usage.>>>>>>>>>>>>>>>>>>>>>>>>**

## head +n CAN`T
```sh
$ head +2 fun.txt
Usage: head [-Count | -n Number | -c Number] [File...]
$
```
******************************************************************************

## cmp utility
```
$ cmp fun.txt jik.txt
fun.txt jik.txt differ: char 1, line 1
$
$ cat  fun.txt jik.txt
this
is
fun
THIS
IS
FUN
$ diff fun.txt jik.txt
1,3c1,3
< this
< is
< fun
---
> THIS
> IS
> FUN
$
$
$ sdiff fun.txt jik.txt
this                                                            |  THIS
is                                                              |  IS
fun                                                             |  FUN
$
```
******************************************************************************

## cmp vs diff :
```sh
cmp: works on any kind of file.
diff: works on text files only.

$ cp a1.out a2.out
$
$ cksum -h MD5 a1.out a2.out
cksum: -h: A file or directory in the path name does not exist.
cksum: MD5: A file or directory in the path name does not exist.
3030550714 4388 a1.out
3030550714 4388 a2.out
$
$
$ csum -h MD5 a1.out a2.out
d19f871a032b3998df86091fe26dc656  a1.out
d19f871a032b3998df86091fe26dc656  a2.out
$
$ cmp a1.out a2.out
$
```
******************************************************************************

## ls for last used.
```sh
ls -u -> last used ( accessed ,NOT u for updated)
```
******************************************************************************

## with out using "ls" command, list out the files in the dir.
```sh
    cat <dir name/>

$ cat .
ﾁ@.E$..ﾁAswapme.cﾂa.outﾁCstructref.cﾁDstructpass.cﾁEmystruct.cﾁFstructp1.cﾁGstore.cﾁHslc.cﾁIshowbits.cﾁJmysc.cﾁKa2i.cﾁLa2i2.cﾁMmyrand.cﾁNmysf.cﾁOarrofst1.cﾁParrp.cﾁQsampledebug.cﾁRsample2.cﾁSarrporg.cﾁTforkme.cﾁUchr0.cﾁVchr0_link.cﾁWissp.cﾁXmytd.cﾁYmymem.cﾁZmynewst.cﾂﾔmystrlen2.cﾂﾕmystrlen1.cﾂﾖmystrrev2.cﾃ$mycolon.cﾍqn1.cﾁﾌmysort.cﾁﾍred.cﾁﾎmychsort.cﾁﾏrlrubits.cﾁﾐmyalloc2.cﾁﾑmycall.cﾁﾒmycall.oﾁﾓtestchmod.cﾁﾔtestchmodﾁﾕdumpcore.cﾁﾖdumpcoreﾁﾗarrofst2.cﾁﾘcircle.cﾁﾙmkpt.cﾁﾚstlen.cﾁ穗ode.cﾁﾜsamplewrite.cﾁﾝsample.txtﾁﾞsds.cﾁﾟsds2.cﾁ灣ds2.hﾁ疣ode.hﾁﾛcore.oldﾁ緜oreﾁ舖tringlen.cﾁ蚶um35.cﾁ訛rralloc.cﾁ輛roblem11.cﾁ鑒etint.cﾁ駑ypr.cﾁ麈aq7.1.cﾁ・aq7.2.cﾁ・aq7.3.b.cﾁ凬aq7.4.cﾁ綠r4.cﾁ・r5.cﾁr6.cﾁa_arg_sample.ﾁystrlen.cﾁ1.cﾁamplesize.cﾁ1.cﾁ2.cﾁ3.cﾁylist.cﾁylist.hﾁ伹ymap.cﾁ璟ymap.hﾁ・y9fp1.cﾂ
                                                        see82.cﾂ
dupcmd.cﾂmygetchar.cﾂuserinput.cﾂfilesize.cﾂabcd.txtﾂmyeof.cﾂmyeof2.cﾂsdshdr.cﾂmysignal.cﾂmy2low.cﾂmylogo.cﾂmyfork1.cﾂfl2.cﾂpeulerqn50.ﾂeulerqn50.cﾂhw$ cﾂhw3.cﾁBa1.outﾂa2.outﾂ$lu.txtﾂ%lt.txt$
$
```

******************************************************************************

## unix dirs
```sh
    bin (link)
    dev
    etc
    usr
    tmp
    unix (link)
    boot-------------------=========================???? where is boot //TODO

$ cd /

$ ls -l
total 280 => no. of blocks (NOT files)
lrwxrwxrwx    1 root     system           10 Sep 04 2014  HORCM -> /opt/HORCM
drwx--x--x    2 root     system          256 Jun 20 13:44 Mail
drwxr-xr-x    4 root     system          256 Jul 31 2014  admin
drwxrwxrwx    8 root     system          256 Feb 16 2015  app
drwxr-x---    2 root     audit           256 Jul 31 2014  audit
drwxrwxrwx    5 root     system          256 Sep 05 09:02 backup
lrwxrwxrwx    1 bin      bin               8 Jul 31 2014  bin -> /usr/bin-------=========================
-rw-r--r--    1 root     system         6642 Nov 16 2014  bosinst.data
drwxr-xr-x    3 root     system          256 Aug 03 2014  core
-rw-r--r--    1 root     system          317 Jul 31 2015  dead.letter
drwxrwxr-x    8 root     system        16384 Sep 27 13:00 dev-------------------=========================
drwxr-xr-x   16 root     system         4096 Jul 31 2014  esa
drwxr-xr-x   42 root     system        16384 Sep 27 12:29 etc-------------------=========================
drwxr-xr-x   42 bin      bin            4096 Feb 05 2016  home
-rw-r--r--    1 root     system        22457 Nov 16 2014  image.data
lrwxrwxrwx    1 root     system           10 Jun 10 18:41 ivott -> /app/ivott
lrwxrwxrwx    1 bin      bin               8 Jul 31 2014  lib -> /usr/lib
drwxr-xr-x    3 root     system          256 Aug 03 2014  libmgr
drwx------    2 root     system          256 Jul 31 2014  lost+found
drwxr-xr-x  195 bin      bin           12288 May 24 19:03 lpp
-rwxr-xr-x    1 root     system          805 Aug 18 2014  mkuser.sh
drwxr-xr-x    4 bin      bin             256 Aug 19 2014  mnt
drwxr-xr-x    2 root     system          256 May 24 15:46 mnt2
drwxr-xr-x    2 root     system          256 May 24 19:00 mnt3
drwxr-xr-x    4 root     system          256 Sep 05 2014  ope
lrwxrwxrwx    1 root     system           14 Apr 16 2015  ope2 -> /userhome/ope2
drwxr-xr-x   28 root     oinstall       4096 Jun 03 2015  opt
-rwxr--r--    1 root     system         7094 Aug 11 2014  oracle_set_disk.sh
drwxr-xr-x    4 pconsole pconsole        256 May 24 19:10 pconsole
-rw-r-----    1 root     system         2581 Sep 09 2014  perf_nmon.ksh
dr-xr-xr-x    1 root     system            0 Sep 27 13:17 proc
drwxr-xr-x    3 bin      bin            4096 Sep 25 15:49 sbin
drwxr-xr-x    9 root     system         4096 Jun 30 2015  smbc
-rw-r--r--    1 root     system            0 Sep 26 23:59 smit.log
-rw-r--r--    1 root     system            0 Sep 26 23:59 smit.script
-rw-r--r--    1 root     system            0 Sep 26 23:59 smit.transaction
drwxrwxr-x    2 root     system          256 Jul 31 2014  tftpboot
drwxrwxrwt   37 bin      bin           16384 Sep 27 13:17 tmp-------------------=========================
drwxr-xr-x    2 root     system          256 Sep 25 2014  tsmlog
lrwxrwxrwx    1 bin      bin               5 Jul 31 2014  u -> /home
lrwxrwxrwx    1 root     system           21 Jul 31 2014  unix -> /usr/lib/boot/unix_64-----------=======
drwxr-xr-x    9 root     system          256 Sep 25 2014  userbackup000
drwxr-xr-x    4 root     system          256 Nov 12 2015  userhome
drwxr-xr-x    2 root     system          256 Sep 25 2014  userlog000
drwxr-xr-x    3 root     system          256 Sep 25 2014  usermenu
drwxr-xr-x    3 root     system          256 Sep 25 2014  usermon
drwxr-xr-x    7 root     system          256 Sep 25 2014  userperf
lrwxrwxrwx    1 root     system           15 Apr 23 2015  uservar000 -> /var/uservar000
drwxr-xr-x   60 bin      bin            4096 May 24 19:05 usr-------------------=========================
drwxr-xr-x   38 bin      bin            4096 May 24 19:05 var
$
```

******************************************************************************

## sh
```
rmdir does NOT remove empty dir.
can be used for safer side in shell script based on situations.


$ rmdir sds
rmdir: 0653-611 Directory sds is not empty.
$
$
$ mkdir sds2
$ rmdir sds2
```

******************************************************************************
##                            SHELL
```sh
shell is a program like ls, who.

shell provides the following facilities.
====================
1) file name shorthands.
====================
    i.e. instead of typing
        cat  file1.txt file2.txt file3.txt
    we can type as
        cat file*.txt

    the point to note is cat command does not see * char,
    it is the shell that interprets "file*.txt" to
        file1.txt file2.txt file3.txt

[] short hand.====================
$ touch temp.txt temx.txt
$ ls -l tem*
-rw-rw-r--    1 txxxx txx               0 Sep 27 14:12 temp.txt
-rw-rw-r--    1 txxxx txx               0 Sep 27 14:12 temx.txt
$ ls -l tem[px].txt
-rw-rw-r--    1 txxxx txx               0 Sep 27 14:12 temp.txt
-rw-rw-r--    1 txxxx txx               0 Sep 27 14:12 temx.txt
$
$ ls -l tem[py].txt     ==================== either p or y
-rw-rw-r--    1 txxxx txx               0 Sep 27 14:12 temp.txt
$

? shorthand --> for single char.====================
$ touch a.txt
$
$
$ ls ?
ls: 0653-341 The file ? does not exist.
$ mv a.txt a
$
$ ls ?
a

====================
2) i/o redirection < and pipes
====================

sort < abcd.txt =======>  <abcd.txt is intrepreted by shell and contents of
                          abcd.txt file is passed to sort.

sort abcd.txt   =======> sort takes the argument of the file and file content
                         is sorted.
sort ( if no file names are given, the standard input is sorted)

$ sort
this
is
fun <ctrl +d> here to terminate input.(i.e. saying no more input)
fun  --> here is the sorted contents.
is
this
$

ls | pr -3 => 3 column list of file names.


| (pipes)
----------
cat abcd.txt | sort

o/p of one pg is input of another pg.

/******************************************************************************/

$ cat abcd.sh
echo ">>> this is the first line"
date
sleep 8
echo ">>> this is the third line"
date

$


$ sleep 10 && echo "abcd" | ./abcd.sh &
[24]    64291282 -------> process id of sleep 10
$ >>> this is the first line
Tue Sep 27 16:45:30 JST 2016
>>> this is the third line
Tue Sep 27 16:45:38 JST 2016
$


note : run the following in separate shell(teraterm)
$ ps -lT 62456292 --> pid of current shell echo $$
       F S      UID      PID     PPID   C PRI NI ADDR    SZ    WCHAN    TTY  TIME CMD
  200401 A      207 62456292 29491570   0  60 20 b227a9480   416 f1000e00458f8878  pts/0  0:00 sh
     401 A      207 64291282 62456292   0  68 24 a5fc45480   456           pts/0  0:00     \--sh
  200001 A      207 63308070 64291282   0  68 24 bad81a480   168 f1000a00e09bcfb0  pts/0  0:00         \--sleep
$


. ========> current dir.

./a.out => first look in to current dir before looking into $PATH

sample execution:
-----------------
/******************************************************************************/

jsh txxxx ~ -->sh
$
$
$ cd SIS.LD.BP
$
$ cd sh
$ pwd
/app/txxinstall/6D/bnk/bnk.run/SIS.LD.BP/sh
$
$
$ echo $PATH
/usr/java7_64:/app/txxinstall/6D/R13/bin:/app/txxinstall/6D/R13/config:/usr/java7_64/jre/bin:/usr/java7_64/bin:/opt/app/oracle/11.2.0.4/bin:/usr/ccs/bin:/usr/vacpp/bin:/usr/bin:/etc:/usr/sbin:/usr/ucb:/usr/bin/X11:/sbin:/usr/java6_64/jre/bin:/usr/java6_64/bin:.:/opt/freeware/bin:/app/txxinstall/6D/bnk/bnk.run/globuspatchbin:/app/txxinstall/6D/bnk/bnk.run/mbbin:/app/txxinstall/6D/bnk/bnk.run/globusbin:/usr/ccs/bin:/usr/ucb:/app/txxinstall/6D/bnk/bnk.run/RGbin:/app/txxinstall/6D/R13/XMLORACLE/bin:/app/txxinstall/6D/bnk/bnk.run/tusbin:/app/txxinstall/6D/bnk/bnk.run/txxbin:/app/txxinstall/6D/bnk/bnk.run/updater/bin:/app/txxinstall/6D/bnk/bnk.run/RGbin:/app/txxinstall/6D/bnk/bnk.run/GR0800005bin
$
$
$ export PATH=$PATH:/app/txxinstall/6D/bnk/bnk.run/SIS.LD.BP/c/sample
$
$
$ set -o emacs
$
$ echo $PATH
/usr/java7_64:/app/txxinstall/6D/R13/bin:/app/txxinstall/6D/R13/config:/usr/java7_64/jre/bin:/usr/java7_64/bin:/opt/app/oracle/11.2.0.4/bin:/usr/ccs/bin:/usr/vacpp/bin:/usr/bin:/etc:/usr/sbin:/usr/ucb:/usr/bin/X11:/sbin:/usr/java6_64/jre/bin:/usr/java6_64/bin:.:/opt/freeware/bin:/app/txxinstall/6D/bnk/bnk.run/globuspatchbin:/app/txxinstall/6D/bnk/bnk.run/mbbin:/app/txxinstall/6D/bnk/bnk.run/globusbin:/usr/ccs/bin:/usr/ucb:/app/txxinstall/6D/bnk/bnk.run/RGbin:/app/txxinstall/6D/R13/XMLORACLE/bin:/app/txxinstall/6D/bnk/bnk.run/tusbin:/app/txxinstall/6D/bnk/bnk.run/txxbin:/app/txxinstall/6D/bnk/bnk.run/updater/bin:/app/txxinstall/6D/bnk/bnk.run/RGbin:/app/txxinstall/6D/bnk/bnk.run/GR0800005bin:/app/txxinstall/6D/bnk/bnk.run/SIS.LD.BP/c/sample
$
$ cc hw.c
cc: 1501-228 (W) input file hw.c not found
$ hw.c
sh: hw.c:  not found.
$ a.out
>>>this is fun
$ which a.out
/app/txxinstall/6D/bnk/bnk.run/SIS.LD.BP/c/sample/a.out
$ pwd
/app/txxinstall/6D/bnk/bnk.run/SIS.LD.BP/sh
$
$
$ touch a.out
$ a.out
>>>this is fun
$ ./a.out
sh: ./a.out: 0403-006 Execute permission denied.
```

******************************************************************************

## ls -1 roughly equal to
```sh
    ls | pr -1
    ( but pr prints header)
```
******************************************************************************

## wait
```
wait waits for the background jobs to finish and return to prompt.
refer page 34
```
## kill
```sh
kill 0 ( size 0 kills your health !)
kills all the processes, except the current login shell
```
******************************************************************************

## start vi and run shell commands with
```sh
!

then the running command is child of vi ., and grandchild of shell

:!sleep 10 && echo "abcd" | ./abcd.sh &


$ ps -lT 65929628
       F S      UID      PID     PPID   C PRI NI ADDR    SZ    WCHAN    TTY  TIME CMD
  200401 A      207 65929628 63177058   0  60 20 8ee8cd480   796          pts/10  0:00 sh
  200001 A      207 63504746 65929628   0  60 20 acde7e480   780          pts/10  0:00     \--vi
  200401 A      207 46530986 63504746   5  60 20 975455590  9564        * pts/10  0:00         \--jsh
  240001 A      207 28377384 46530986   1  60 20 90c140590 183672 f1000a00e2283b20      -  0:00             \--oracle
```
******************************************************************************

## check empty string in sh script
```sh
check only for empty string of a variable:
not checking for (defined) OR (NOT defined)


if [ -z "$TMPVAR" ]; then
        echo "NOT ok"
else
        echo "ok"
fi


$ ./a.sh
NOT ok
$ export TMPVAR=
$ ./a.sh
NOT ok
$ export TMPVAR=""
$ ./a.sh
NOT ok
$ export TMPVAR="des"
$ ./a.sh
ok
```

******************************************************************************

## radical thinking !!!
```sh
$ p ---> no, error , but how can you tell,
$
$
$ s
sh: s:  not found.
$ p ------> 1) no error, oh something is found as executable as "p" ( 100 % correct)
$ which p --> 2) where is it
/app/txxinstall/6D/R13/bin/p  --> 3) oh ! it is in txx product r13
                                 ( may not be 100% correct, since "p" may be
                                 copied from some other bin to R13/bin dir)

                                 morale :
                                ---------

<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<always check for missing parts that can be used
proving the statement is 100% NOT correct>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>
$
$
$
$ jshow -c p                  --> 4) check to make sure p belongs to r13/bin
                                    by using jshow.
Executable:          /app/txxinstall/6D/R13/bin/p.so
                     jBC main() version 13.0 Fri Jul 10 07:21:22 2015
                     jBC main() source file unknown
Executable (DUP!!):  /app/txxinstall/6D/R13/bin/p
                     jBC main() version 13.0 Fri Jul 10 07:21:22 2015
                     jBC main() source file unknown
$
$
$
```
******************************************************************************

## nohup
```sh
nohup nice cmnd & ->

    nice to lower the priority for the resource hungry process.


nohup:
-----

terminal 1:
-----------

nohup find / -print &

wait for the o/p to print.


terminal 2:
-----------
$ ps -lT 9437524
       F S      UID      PID     PPID   C PRI NI ADDR    SZ    WCHAN    TTY  TIME CMD
  200001 A      207  9437524 66191742  12  74 24 99e1ca480   424           pts/1  0:00 find
$ ps -lT 9437524
       F S      UID      PID     PPID   C PRI NI ADDR    SZ    WCHAN    TTY  TIME CMD
  200001 A      207  9437524 66191742  18  78 24 99e1ca480   424           pts/1  0:00 find
$ ps -lT 9437524
       F S      UID      PID     PPID   C PRI NI ADDR    SZ    WCHAN    TTY  TIME CMD            (a) ------
  240001 A      207  9437524        1  17  77 24 99e1ca480   424           pts/1  0:01 find
$ ps -lT 9437524
       F S      UID      PID     PPID   C PRI NI ADDR    SZ    WCHAN    TTY  TIME CMD            (b) ======
  240001 A      207  9437524        1  16  77 24 99e1ca480   424 f1000a01e8281b58      -  0:02 find
$ ps -lT 9437524
       F S      UID      PID     PPID   C PRI NI ADDR    SZ    WCHAN    TTY  TIME CMD
  240001 A      207  9437524        1  59 101 24 99e1ca480   424               -  0:03 find
                                    |
                                    +------> note the PPID changes
                                             from
                                             66191742 ( shell PID (echo $$))
                                             to
                                             1
                    This indicates that SIGHUP was received.


* Also note that TTY column is - for nohup process.
but it takes time to update to -.

Please refer, in line (a) PPID is updated to 1, but TTY is still pts/1
but in the next execution TTY is updated to - .

also in some uix, the sign may be ? instead of -
```

******************************************************************************

## find nohup process
```sh
ps xw --> can also be used to identify the nohup process.
```

******************************************************************************

## nice
```sh
nice affects the CPU time

niceness to other program
with higher value ( higher niceness to other programs)


CPU time will be proportional to 20-p

1) nice 0 will receive ( default, i think even if we are not specifying...)

20 -0 / 20 - 0 => 1 * 100 => 100 % cpu time

2) nice +15 will receive

20 - 15 / 20 - 0 => 5 / 20 => 1/4 => 0.25 * 100 => 25% of CPU time

3) nice -10 will receive

20 - (-10) / 20 - 0 => 30 / 20 => 3/ 2 * 100=> 150% of CPU time ( really ?!!!)

on BSD system ( i could not understand the calculation from wikipedia )
it is said as 10 to 1

similar programs:
-----------------

linux ionice -> affects scheduling of i/o

renice -> change priority already running
```

******************************************************************************

## at 
```sh
$at 1417 ( job run at 2:17 pm)
ls
ctrl+d
$
$
$ jobs -l
[22] + 59507172  Running                 at 1417
[21] - 64028940  Running                 date
[20]   55771612  Running                 nice ls
[19]   55771606  Running                 man nice
[18]   64160048  Running                 ps xw
[17]   42074536  Running                 vi abcd.txt
[16]   48234986  Running                 rm nohup.out
[15]   27132246  Running                 vi nohup.out
[14]   31785038  Running                 ls -lt
[13]   6357340   Running                 ls -lt
[12]   27132198  Running                 ps -lT 9437524
[11]   63635814  Running                 man nohup
[10]   50790746  Running                 ps -lT 9437524
[9]   63701472   Running                 ps -lT 9437524
[8]   66781468   Running                 ps -lT 9437524
[7]   64160158   Running                 ls -lt
[6]   56623538   Running                 ls -lt
[5]   11141464   Running                 ps -lT 9437524
[4]   11141462   Running                 ps -lT 9437524
[3]   64160100   Running                 ps -lT 9437524
[2]   8520182    Running                 ps -lT 9437524
[1]   13107610   Running                 ps -lT 9437524

```
******************************************************************************


## find out ctrl M chars in a file
```sh
od --> octal dump
od -cbx

c-> char
b->byte(octal)
x->hex

od -c  abcd2.txt | grep '\\r'


$ cat -ve abcd2.txt ------------------------------------------------------------
    @Override public int size() {^M$
      return IntMath.factorial(inputList.size()); $
    }$
$
    @Override public boolean isEmpty() {$
      return false;$
    }$
$
    @Override public Iterator<List<E>> iterator() {$
      return new PermutationIterator<E>(inputList);$
    }$
$
    @Override public boolean contains(@ullable Object obj) {$
      if (obj instanceof List) {$
        List<?> list = (List<?>) obj;$
        return isPermutation(inputList, list);$
      }$
      return false;$
    }$
$
    @Override public String toString() {$
      return "permutations(" + inputList + ")";$
    }$
  }$
$
  private static class PermutationIterator<E>$
$

$ od -c  abcd2.txt------------------------------------------------------------
0000000                    @   O   v   e   r   r   i   d   e       p   u
0000020    b   l   i   c       i   n   t       s   i   z   e   (   )
0000040    {  \r  \n                           r   e   t   u   r   n
0000060    I   n   t   M   a   t   h   .   f   a   c   t   o   r   i   a
0000100    l   (   i   n   p   u   t   L   i   s   t   .   s   i   z   e
0000120    (   )   )   ;      \n                   }  \n  \n
0000140        @   O   v   e   r   r   i   d   e       p   u   b   l   i
0000160    c       b   o   o   l   e   a   n       i   s   E   m   p   t
0000200    y   (   )       {  \n                           r   e   t   u
0000220    r   n       f   a   l   s   e   ;  \n                   }  \n
0000240   \n                   @   O   v   e   r   r   i   d   e       p
0000260    u   b   l   i   c       I   t   e   r   a   t   o   r   <   L
0000300    i   s   t   <   E   >   >       i   t   e   r   a   t   o   r
0000320    (   )       {  \n                           r   e   t   u   r
0000340    n       n   e   w       P   e   r   m   u   t   a   t   i   o
0000360    n   I   t   e   r   a   t   o   r   <   E   >   (   i   n   p
0000400    u   t   L   i   s   t   )   ;  \n                   }  \n  \n
0000420                    @   O   v   e   r   r   i   d   e       p   u
0000440    b   l   i   c       b   o   o   l   e   a   n       c   o   n
0000460    t   a   i   n   s   (   @   u   l   l   a   b   l   e       O
0000500    b   j   e   c   t       o   b   j   )       {  \n
0000520                i   f       (   o   b   j       i   n   s   t   a
0000540    n   c   e   o   f       L   i   s   t   )       {  \n
0000560                            L   i   s   t   <   ?   >       l   i
0000600    s   t       =       (   L   i   s   t   <   ?   >   )       o
0000620    b   j   ;  \n                                   r   e   t   u
0000640    r   n       i   s   P   e   r   m   u   t   a   t   i   o   n
0000660    (   i   n   p   u   t   L   i   s   t   ,       l   i   s   t
0000700    )   ;  \n                           }  \n
0000720        r   e   t   u   r   n       f   a   l   s   e   ;  \n
0000740                }  \n  \n                   @   O   v   e   r   r
0000760    i   d   e       p   u   b   l   i   c       S   t   r   i   n
0001000    g       t   o   S   t   r   i   n   g   (   )       {  \n
0001020                        r   e   t   u   r   n       "   p   e   r
0001040    m   u   t   a   t   i   o   n   s   (   "       +       i   n
0001060    p   u   t   L   i   s   t       +       "   )   "   ;  \n
0001100                }  \n           }  \n  \n           p   r   i   v
0001120    a   t   e       s   t   a   t   i   c       c   l   a   s   s
0001140        P   e   r   m   u   t   a   t   i   o   n   I   t   e   r
0001160    a   t   o   r   <   E   >  \n
0001170
$
$ od -c  abcd2.txt | grep '\r'-----------------------------NOT OK---------------
0000000                    @   O   v   e   r   r   i   d   e       p   u
0000040    {  \r  \n                           r   e   t   u   r   n
0000060    I   n   t   M   a   t   h   .   f   a   c   t   o   r   i   a
0000140        @   O   v   e   r   r   i   d   e       p   u   b   l   i
0000200    y   (   )       {  \n                           r   e   t   u
0000220    r   n       f   a   l   s   e   ;  \n                   }  \n
0000240   \n                   @   O   v   e   r   r   i   d   e       p
0000260    u   b   l   i   c       I   t   e   r   a   t   o   r   <   L
0000300    i   s   t   <   E   >   >       i   t   e   r   a   t   o   r
0000320    (   )       {  \n                           r   e   t   u   r
0000340    n       n   e   w       P   e   r   m   u   t   a   t   i   o
0000360    n   I   t   e   r   a   t   o   r   <   E   >   (   i   n   p
0000420                    @   O   v   e   r   r   i   d   e       p   u
0000620    b   j   ;  \n                                   r   e   t   u
0000640    r   n       i   s   P   e   r   m   u   t   a   t   i   o   n
0000720        r   e   t   u   r   n       f   a   l   s   e   ;  \n
0000740                }  \n  \n                   @   O   v   e   r   r
0000760    i   d   e       p   u   b   l   i   c       S   t   r   i   n
0001000    g       t   o   S   t   r   i   n   g   (   )       {  \n
0001020                        r   e   t   u   r   n       "   p   e   r
0001100                }  \n           }  \n  \n           p   r   i   v
0001140        P   e   r   m   u   t   a   t   i   o   n   I   t   e   r
0001160    a   t   o   r   <   E   >  \n
$ od -c  abcd2.txt | grep '\\r'--------------WATCH OUT \\r instead \r -----OK---
0000040    {  \r  \n                           r   e   t   u   r   n

$ cat -vet abcd2.txt  --> cat -t for tab -v for spl char like ^M, e for ending
                          with $
    @Override public int size() {^M$
      return IntMath.factorial(inputList.size()); $
    ^I}$




cat
1
1


cat -u
unbuffered o/p.

cat and cat -u behaves the same.

ctrl+d => sends signal saying no more input
with empty string on the command line, no more input says
i just want to quit.

for i/p programs like cat, it is a way of saying no more input to the program.

```

******************************************************************************

## file type
```sh
set -o emacs

ctrl+R
    => ^R
    then type the word for search
    ^RLD ( press end enter)
    cd SIS.LD.BP/sh ( appears)


identify file type using file,
it uses the fast method by looking few hundered bytes.

$ od /usr/bin/ed | head -3
0000000  000737 000004 051162 115720 000000 000000 000000 000000
(here 737 expected to 410 as per the book. may be aix differs.)
0000020  000110 010007 000413 000001 000000 107143 000000 047205
0000040  000000 113132 020000 056060 010000 000400 020000 007543
$ ls -l /usr/bin/ed
-r-xr-xr-x    2 bin      bin           61650 Aug 23 2014  /usr/bin/ed
$ file /usr/bin/ed
/usr/bin/ed: executable (RISC System/6000) or object module
```

******************************************************************************

## why the system does not track the file types more carefully....????
```sh
    many commands operate at any file at all.
    no reason to restrict their capabilities.
    formatless idea is much depper than that.

finding files in a dir. with du ( disk usage )

du ( default includes subdirs)
-k = in kilobytes
-m = in megabytes

-a = including files.
```

******************************************************************************
##                finding files
```sh
$ find . -name 1.txt
./test/1.txt
./test/extdir/1.txt
$
$
$
$ du -a | grep 1.txt
8       ./test/1.txt
8       ./test/extdir/1.txt
$

$ du
16      ./test/extdir
56      ./test
152     .
```
******************************************************************************
##            du samples
```sh
$ du
16      ./test/extdir
56      ./test
152     .

du . is same as du

$ du .
16      ./test/extdir
56      ./test
152     .
$

du -a ( including files)


$ du -a
0       ./XMLdriver.log
0       ./a.out
8       ./a.sh
0       ./abcd.out
8       ./abcd.sh
24      ./abcd.tar
8       ./abcd.txt
8       ./abcd2.txt
8       ./b.sh
8       ./sample2.txt
8       ./sample3.txt
8       ./sample4.txt
8       ./test/1.txt
8       ./test/2.txt
8       ./test/extdir/1.txt
8       ./test/extdir/2.txt
16      ./test/extdir
24      ./test/sample.tar
56      ./test
0       ./zxy.txt
152     .
```

******************************************************************************

## octal dump of dir
```
$ cat .
$ .c..b!abcd.shb"abcd.outdｮabcd.txtdｩabcd2.txtdｪXMLdriver.logdｫsample2.txtdｬsample3.txtdｭsample4.txtbaabcd.tarttestb9zxy.txtb:a.outb;a.shb<b.sh$
$
$
$ od -c .
0000000    b       .  \0  \0  \0  \0  \0  \0  \0  \0  \0  \0  \0  \0  \0
0000020  001   c   .   .  \0  \0  \0  \0  \0  \0  \0  \0  \0  \0  \0  \0
0000040    b   !   a   b   c   d   .   s   h  \0  \0  \0  \0  \0  \0  \0
0000060    b   "   a   b   c   d   .   o   u   t  \0  \0  \0  \0  \0  \0
0000100    d 256   a   b   c   d   .   t   x   t  \0  \0  \0  \0  \0  \0
0000120    d 251   a   b   c   d   2   .   t   x   t  \0  \0  \0  \0  \0
0000140    d 252   X   M   L   d   r   i   v   e   r   .   l   o   g  \0
0000160    d 253   s   a   m   p   l   e   2   .   t   x   t  \0  \0  \0
0000200    d 254   s   a   m   p   l   e   3   .   t   x   t  \0  \0  \0
0000220    d 255   s   a   m   p   l   e   4   .   t   x   t  \0  \0  \0
0000240    b   a   a   b   c   d   .   t   a   r  \0  \0  \0  \0  \0  \0
0000260  223   t   t   e   s   t  \0  \0  \0  \0  \0  \0  \0  \0  \0  \0
0000300    b   9   z   x   y   .   t   x   t  \0  \0  \0  \0  \0  \0  \0
0000320    b   :   a   .   o   u   t  \0  \0  \0  \0  \0  \0  \0  \0  \0
0000340    b   ;   a   .   s   h  \0  \0  \0  \0  \0  \0  \0  \0  \0  \0
0000360    b   <   b   .   s   h  \0  \0  \0  \0  \0  \0  \0  \0  \0  \0
0000400
$
$
with octal number ---------------------------------------
$ od -cb .
0000000    b       .  \0  \0  \0  \0  \0  \0  \0  \0  \0  \0  \0  \0  \0
         142 040 056 000 000 000 000 000 000 000 000 000 000 000 000 000
0000020  001   c   .   .  \0  \0  \0  \0  \0  \0  \0  \0  \0  \0  \0  \0
         001 143 056 056 000 000 000 000 000 000 000 000 000 000 000 000
0000040    b   !   a   b   c   d   .   s   h  \0  \0  \0  \0  \0  \0  \0
         142 041 141 142 143 144 056 163 150 000 000 000 000 000 000 000
0000060    b   "   a   b   c   d   .   o   u   t  \0  \0  \0  \0  \0  \0
         142 042 141 142 143 144 056 157 165 164 000 000 000 000 000 000
0000100    d 256   a   b   c   d   .   t   x   t  \0  \0  \0  \0  \0  \0
         144 256 141 142 143 144 056 164 170 164 000 000 000 000 000 000
0000120    d 251   a   b   c   d   2   .   t   x   t  \0  \0  \0  \0  \0
         144 251 141 142 143 144 062 056 164 170 164 000 000 000 000 000
0000140    d 252   X   M   L   d   r   i   v   e   r   .   l   o   g  \0
         144 252 130 115 114 144 162 151 166 145 162 056 154 157 147 000
0000160    d 253   s   a   m   p   l   e   2   .   t   x   t  \0  \0  \0
         144 253 163 141 155 160 154 145 062 056 164 170 164 000 000 000
0000200    d 254   s   a   m   p   l   e   3   .   t   x   t  \0  \0  \0
         144 254 163 141 155 160 154 145 063 056 164 170 164 000 000 000
0000220    d 255   s   a   m   p   l   e   4   .   t   x   t  \0  \0  \0
         144 255 163 141 155 160 154 145 064 056 164 170 164 000 000 000
0000240    b   a   a   b   c   d   .   t   a   r  \0  \0  \0  \0  \0  \0
         142 141 141 142 143 144 056 164 141 162 000 000 000 000 000 000
0000260  223   t   t   e   s   t  \0  \0  \0  \0  \0  \0  \0  \0  \0  \0
         223 164 164 145 163 164 000 000 000 000 000 000 000 000 000 000
0000300    b   9   z   x   y   .   t   x   t  \0  \0  \0  \0  \0  \0  \0
         142 071 172 170 171 056 164 170 164 000 000 000 000 000 000 000
0000320    b   :   a   .   o   u   t  \0  \0  \0  \0  \0  \0  \0  \0  \0
         142 072 141 056 157 165 164 000 000 000 000 000 000 000 000 000
0000340    b   ;   a   .   s   h  \0  \0  \0  \0  \0  \0  \0  \0  \0  \0
         142 073 141 056 163 150 000 000 000 000 000 000 000 000 000 000
0000360    b   <   b   .   s   h  \0  \0  \0  \0  \0  \0  \0  \0  \0  \0
         142 074 142 056 163 150 000 000 000 000 000 000 000 000 000 000
0000400
```
******************************************************************************

## dir content may be captured using
```
cat . > foo

ls -f
    lists the dir listing ( including -a)

now
    ls -f < foo
        is same as
    ls -f

$ cat . > foo
$ ls -f foo
ls: foo: A parameter must be a directory.

$ cat foo
$ .c..b!abcd.shb"abcd.outdｮabcd.txtdｩabcd2.txtdｪXMLdriver.logdｫsample2.txtdｬsample3.txtdｭsample4.txtbaabcd.tarttestb9zxy.txtb:a.outb;a.shb<b.shb=foo$
$
$

$ ls -f < foo
.              XMLdriver.log  a.sh           abcd.sh        abcd.txt       b.sh           sample2.txt    sample4.txt    zxy.txt
..             a.out          abcd.out       abcd.tar       abcd2.txt      foo            sample3.txt    test
$

$ ls -f
.              XMLdriver.log  a.sh           abcd.sh        abcd.txt       b.sh           sample2.txt    sample4.txt    zxy.txt
..             a.out          abcd.out       abcd.tar       abcd2.txt      foo            sample3.txt    test
```
******************************************************************************

## how does the pwd command operate?
```sh
i think like this

$ ls -lia
total 112
5399072 drwxrwxr-x    3 txxxx txx            4096 Sep 30 09:26 .
3408227 drwxrwxr-x   13 txxxx txx            4096 Sep 09 16:19 ..
5399722 -rw-rw-r--    1 txxxx txx               0 May 23 13:08 XMLdriver.log
5399098 -rw-rw-r--    1 txxxx txx               0 Sep 28 08:53 a.out
5399099 -rwxrwxr-x    1 txxxx txx              60 Sep 28 09:53 a.sh
5399074 -rw-rw-r--    1 txxxx txx               0 Feb 16 2016  abcd.out
5399073 -rwxrwxr-x    1 txxxx txx              87 Sep 27 16:26 abcd.sh
5399137 -rw-r-----    1 txxxx txx           10240 May 24 15:05 abcd.tar
5399726 -rw-r--r--    1 txxxx txx               2 Sep 16 09:30 abcd.txt
5399721 -rw-rw-r--    1 txxxx txx             633 Sep 29 15:09 abcd2.txt
5399100 -rwxrwxr-x    1 txxxx txx              60 Sep 28 09:58 b.sh
5399101 -rw-rw-r--    1 txxxx txx             272 Sep 30 09:26 foo
5399723 -rw-r-----    1 txxxx txx              14 Sep 14 13:07 sample2.txt
5399724 -rw-rw-r--    1 txxxx txx              11 May 23 13:22 sample3.txt
5399725 -rw-rw-r--    1 txxxx txx              16 May 23 13:50 sample4.txt
4035444 drwxrwxr-x    3 txxxx txx             256 Jul 07 09:10 test
5399097 -rw-rw-r--    1 txxxx txx               0 Sep 14 13:08 zxy.txt

$ find ../ -inum 5399072
../sh1 ( found the working dir name !!!)
```
******************************************************************************

## some info on pwd vs $PWD
```sh
$ pwd
/app/txxinstall/6D/bnk/bnk.run/SIS.LD.BP/sh
$
$ echo $PWD
/app/txxinstall/6D/bnk/bnk.run/SIS.LD.BP/sh

$ mv ../sh ../sh1
$ pwd
/app/txxinstall/6D/bnk/bnk.run/SIS.LD.BP/sh1
$ echo $PWD
/app/txxinstall/6D/bnk/bnk.run/SIS.LD.BP/sh -------------> ( NOT sh1 ? )
                                                        until you cd $PWD wont be updated
                                                        but, pwd ( is routine)

$ cd .
$ pwd
/app/txxinstall/6D/bnk/bnk.run/SIS.LD.BP/sh1
$ echo $PWD
/app/txxinstall/6D/bnk/bnk.run/SIS.LD.BP/sh1 -------------> now PWD var is updated.

```
******************************************************************************
##    cd .. on root dir
```sh
$ cd /
$
$
$ cd ..
$ pwd
/
$ cd .
$ ls -lid */
49190 dr-xr-xr-x   10 root     sys            4096 Sep 04 2014  HORCM/
147472 drwx--x--x    2 root     system          256 Jun 20 13:44 Mail/
    2 drwxr-xr-x    4 root     system          256 Jul 31 2014  admin/ --------> note down many dir with inode number as 2
    2 drwxrwxrwx    8 root     system          256 Feb 16 2015  app/   --------> note down many dir with inode number as 2
    2 drwxrwxrwx    5 root     system          256 Sep 05 09:02 backup/--------> note down many dir with inode number as 2
   18 drwxr-xr-x    4 bin      bin           53248 May 24 19:12 bin/
    2 drwxr-xr-x    3 root     system          256 Aug 03 2014  core/
 4160 drwxrwxr-x    8 root     system        16384 Sep 30 10:00 dev/
11037 drwxr-xr-x   16 root     system         4096 Jul 31 2014  esa/
   12 drwxr-xr-x   42 root     system        16384 Sep 30 09:29 etc/
    2 drwxr-xr-x   42 bin      bin            4096 Feb 05 2016  home/--------> note down many dir with inode number as 2
3394316 drwxrwx---    5 twifusr  gif             256 Jun 07 17:29 ivott/
    8 drwxr-xr-x   46 bin      bin           36864 May 24 19:12 lib/
    2 drwxr-xr-x    3 root     system          256 Aug 03 2014  libmgr/
  182 drwxr-xr-x  195 bin      bin           12288 May 24 19:03 lpp/
  183 drwxr-xr-x    4 bin      bin             256 Aug 19 2014  mnt/
148708 drwxr-xr-x    2 root     system          256 May 24 15:46 mnt2/
148713 drwxr-xr-x    2 root     system          256 May 24 19:00 mnt3/
    2 drwxr-xr-x    4 root     system          256 Sep 05 2014  ope/
47264 drwxr-xr-x   11 apladmin gapl           4096 Sep 28 17:19 ope2/
    2 drwxr-xr-x   28 root     oinstall       4096 Jun 03 2015  opt/
 5286 drwxr-xr-x    4 pconsole pconsole        256 May 24 19:10 pconsole/
    2 dr-xr-xr-x    1 root     system            0 Sep 30 10:03 proc/
  185 drwxr-xr-x    3 bin      bin            4096 Sep 25 15:49 sbin/
32772 drwxr-xr-x    9 root     system         4096 Jun 30 2015  smbc/
 5179 drwxrwxr-x    2 root     system          256 Jul 31 2014  tftpboot/
    2 drwxrwxrwt   36 bin      bin           16384 Sep 30 10:03 tmp/
40985 drwxr-xr-x    2 root     system          256 Sep 25 2014  tsmlog/
    2 drwxr-xr-x   42 bin      bin            4096 Feb 05 2016  u/
41079 drwxr-xr-x    9 root     system          256 Sep 25 2014  userbackup000/
41081 drwxr-xr-x    4 root     system          256 Nov 12 2015  userhome/
41087 drwxr-xr-x    2 root     system          256 Sep 25 2014  userlog000/
41088 drwxr-xr-x    3 root     system          256 Sep 25 2014  usermenu/
41152 drwxr-xr-x    3 root     system          256 Sep 25 2014  usermon/
41089 drwxr-xr-x    7 root     system          256 Sep 25 2014  userperf/
69632 drwxr-xr-x   10 root     system          256 May 27 2015  uservar000/
    2 drwxr-xr-x   60 bin      bin            4096 May 24 19:05 usr/
    2 drwxr-xr-x   38 bin      bin            4096 May 24 19:05 var/
```
******************************************************************************
## du and find
```sh
du is used to monitor disk usage,
but can be also used find files.

which is faster "find" or "du" for finding files.

du (default block size in 512 bytes)

-k ( block size in 1024 bytes)

du -s => summary

du -a => all files and dir and files in that subdirs
```
******************************************************************************
##            important usage of find
```sh
$ touch abcd.txt
$
$ find . -mmin -60 -------------> files modified in last 60 mins
.
./abcd.txt
./du.txt
./find.txt
$
$
$ touch du.txt
$
$
$
$ find . -newer abcd.txt  ----------> files that are newer than this file.
./du.txt
$
```
******************************************************************************
## ls vs sh script to find file.
```sh
$ ls -ld sh
drwxrwxr-x    3 txxxx txx            4096 Sep 30 09:26 sh

test condition for finding file / dir.

$ [ -f sh ] || echo not found
not found
$ [ -d sh ] || echo not found
```
******************************************************************************

## grep 4096 ( 4kb ) to find out the dirs.
```sh
$ ls -lai  | grep 4096
3408227 drwxrwxr-x   13 txxxx txx            4096 Sep 30 13:47 .
3060018 drwxr-x---    2 txxxx txx            4096 Apr 14 10:54 NOTES
3060017 drwxrwxr-x    5 txxxx txx            4096 Mar 11 2016  backup
148772 drwxrwxr-x    8 txxxx txx            4096 Sep 27 13:36 c
5399072 drwxrwxr-x    3 txxxx txx            4096 Sep 30 09:26 sh
```
******************************************************************************
## cd using find cmd
```sh
$ find . -name test | xargs cd --> NOT WORKING

$ find . -name test -exec cd {} \; --> NOT WORKING

$ cd `find . -name test` --> only works
```
******************************************************************************
## crypt cmd
```sh
man crypt found

but,
crypt command NOT found.
```
******************************************************************************

## /etc/passwd file contains user data
```sh
loginname:encryptedpwd:uid:gid:miscellany:logindir:loginshell

If
    loginshell may be empty -->
then
    use default shell /bin/sh


miscellany may contain address, ph no., or any other data.

$ ls -l  /etc/passwd
-rw-r--r--    1 root     security       2463 Feb 05 2016  /etc/passwd
$
$
$
$ ls -lg  /etc/passwd -----------------------------------------> g for group
-rw-r--r--    1 security       2463 Feb 05 2016  /etc/passwd
```
******************************************************************************

## set uid bits
```sh
$ ls -l  /etc/passwd
-rw-r--r--    1 root     security       2463 Feb 05 2016  /etc/passwd
$
$ ls -l  /bin/passwd
-r-sr-xr-x    1 root     security      42980 Aug 23 2014  /bin/passwd
    |
    |
    +----> Note down the "s" instead of "x"

i.e.  when the user runs the /bin/passwd command,
 1) ( the passwd command check the user is current user, so that he/she can change
    his/her password alone ) --> i think so,
 2) if the above match is ok, it edits the /etc/passwd file
    with the owner of the  /etc/passwd command,
    so that changing the pwd of current user is possible.
    because only root can edit the /etc/passwd file ultimately.

*    set-uid patent filed and given to Bell lab, ( Dennis Ritchie)

Potential danger:
-----------------
1) think of if /bin/passwd is with the
    -rwsrwxrwx permission, any body can change the content of /bin/passwd,
    so that malicious user can do whatever he/she wants,
    since he can gain the root access, by executing /bin/passwd, and
    from there he is on his/her own.

2) to avoid this, usually system set the set-uid to off when the file is modified.
    so that if the malicious user attempts to execute after modification he can not
    get "owner" permission.
```

--------------------------------------------------------------------------------
## overwrite contents of . ? NOT possible
```
$ ls -ld .
drwxrwxr-x    3 txxxx txx            4096 Sep 30 09:26 .
$
$ cat > .
Cannot write to a directory.,( even the root user)
```
--------------------------------------------------------------------------------

## setuid
```sh
setuid for shell script NOT working.
as per wikipedia most of the uix system ignore setuid flag for shell scripts,
since there is a increase failure of security.

mostly setuid is used by passwd kind of builtin programs.

I tried this, but does not work as expected ( expecting similiar to passwd change.)


txxtest1 logged in:;::::::::::::::::::::::::::::::::

$ ls -l permcheck*
-r-sr-xr-x    1 txxtest1 txx              61 Oct 04 14:45 permcheck.sh
-rw-r--r--    1 txxtest1 txx               1 Oct 04 16:28 permcheck.txt


$ cat permcheck.sh
echo "this is permission check"
echo "abcd" >> permcheck.txt
$
$ cat permcheck.txt

$
$
under txxxx logged in:^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

$ ./permcheck.sh
this is permission check
The file access permissions do not allow the specified action.
./permcheck.sh[2]: permcheck.txt: 0403-005 Cannot create the specified file.

Note: I am expecting permcheck.sh should run with the privilege of "txxtest1",
since s bit set for txxtest1 user. and can write into permcheck.txt.
but this is not happening.! ( may be shell script in aix not allowed to do
setuid bit.)

$ ls -ld .
dr-xr-xr-x    2 txxxx txx             256 Oct 05 08:55 .
$ pwd
/app/txxinstall/6D/bnk/bnk.run/SIS.LD.BP/sh/abcd
$

when the dir has no write permission, we can not create files in the dir,
as well can not remove the files in the dir.

$ ls -l
total 8
-rw-rw-r--    1 txxxx txx              14 Oct 05 08:55 file1.txt

but how come the file1.txt is present, if we can not create dir.
this file1.txt is being created first, then the dir "w" permission is
removed with chmod

$ ls -ld abcd
drwxrwxr-x    2 txxxx txx             256 Oct 05 08:55 abcd
$ chmod ug-w-w abcd
$ ls -ld abcd
dr-xr-xr-x    2 txxxx txx             256 Oct 05 08:55 abcd

$ touch efgh.txt
touch: 0652-046 Cannot create efgh.txt.  --> can not create file,
since dir has NO "w" permission.

$ rm -f file1.txt
rm: 0653-609 Cannot remove file1.txt.
The file access permissions do not allow the specified action.
$
```

-------------------
## protected file
```sh
now change the file1.txt as a protected file,
i.e.

change dir "abcd" with "w" permission.
make sure file permission is NOT set with "w" permission.

remove the file , it will ask for confirmation.

$
$ chmod +w abcd
$ ls -ld abcd
drwxrwxr-x    2 txxxx txx             256 Oct 05 08:55 abcd
$ ls -l abcd
total 8
-r--r--r--    1 txxxx txx              14 Oct 05 08:55 file1.txt
$
$ cd abcd
$
$
$ ls -l
total 8
-r--r--r--    1 txxxx txx              14 Oct 05 08:55 file1.txt

$ rm file1.txt
rm: Remove file1.txt? y
$
$ ls -l
total 0

------------------------------------- removing protected file with rm -f option
$ touch file2.txt
$
$ ls -l file2.txt
-rw-rw-r--    1 txxxx txx               0 Oct 05 09:11 file2.txt
$
$ chmod -w file2.txt
$
$ ls -l
total 0
-r--r--r--    1 txxxx txx               0 Oct 05 09:11 file2.txt
$
$
$ rm file2.txt
rm: Remove file2.txt? n
$
$
$ rm -f file2.txt


Million dollar question ?????????????????????????????????????

can the file can be removed if it has no write permission.

the answer is "yes". this file is called protected file.

this can be deleted with rm, then give confirmation y of y/n
    or
rm -f option.

but the important thing is dir has to have the write permission.
otherwise you can not delete the file.
Looks like dir is the parent ( i think so) covering the child.

```
******************************************************************************
## file permissions

```sh

last updated --> permission bit changes
last modified --> contents
last accessed --> accessed ( for dir --> listing out contents)

$ istat abcd
Inode 4132864 on device 37/2    Directory
Protection: rwxrwxr-x
Owner: 207(txxxx)            Group: 5800(txx)
Link count:   2         Length 256 bytes

Last updated:   Wed Oct  5 09:22:26 JST 2016
Last modified:  Wed Oct  5 09:12:02 JST 2016
Last accessed:  Wed Oct  5 09:20:31 JST 2016

$ chmod +w abcd
$
$ istat abcd
Inode 4132864 on device 37/2    Directory
Protection: rwxrwxr-x
Owner: 207(txxxx)            Group: 5800(txx)
Link count:   2         Length 256 bytes

Last updated:   Wed Oct  5 09:22:44 JST 2016
Last modified:  Wed Oct  5 09:12:02 JST 2016
Last accessed:  Wed Oct  5 09:20:31 JST 2016



i-node => index node
became inode...
```
******************************************************************************
## ls by dates
```sh

ls -l  --> sort by name (default)

ls -lc --> sort by last updated

ls -lt --> sort by last modified

ls -lu --> sort by last used ( accessed)


$ istat abcd
Inode 4132864 on device 37/2    Directory
Protection: rwxrwxr-x
Owner: 207(txxxx)            Group: 5800(txx)
Link count:   2         Length 256 bytes

Last updated:   Wed Oct  5 09:22:44 JST 2016
Last modified:  Wed Oct  5 09:12:02 JST 2016
Last accessed:  Wed Oct  5 09:50:48 JST 2016

$
$
$ ls -ld abcd
drwxrwxr-x    2 txxxx txx             256 Oct 05 09:12 abcd
$
$ ls -ldc abcd
drwxrwxr-x    2 txxxx txx             256 Oct 05 09:22 abcd
$
$ ls -ldt abcd
drwxrwxr-x    2 txxxx txx             256 Oct 05 09:12 abcd
$
$ ls -ldu abcd
drwxrwxr-x    2 txxxx txx             256 Oct 05 09:50 abcd

         same as
$ ls -ltu  --> ls -lt

         same as
$ ls -lut  --> ls -lu

// TODO
explore
od octal dump first two bytes for dir
Page 71

```
******************************************************************************
```sh
$ od -d .
0000000  25120 11776 00000 00000 00000 00000 00000 00000
0000020  00355 11822 00000 00000 00000 00000 00000 00000
0000040  25121 24930 25444 11891 26624 00000 00000 00000
0000060  25122 24930 25444 11887 30068 00000 00000 00000
0000100  25774 24930 25444 11892 30836 00000 00000 00000
0000120  25769 24930 25444 12846 29816 29696 00000 00000
0000140  25770 22605 19556 29289 30309 29230 27759 26368
0000160  25771 29537 28016 27749 12846 29816 29696 00000
0000200  25772 29537 28016 27749 13102 29816 29696 00000
0000220  25773 29537 28016 27749 13358 29816 29696 00000
0000240  25185 24930 25444 11892 24946 00000 00000 00000
0000260  37748 29797 29556 00000 00000 00000 00000 00000
0000300  25145 31352 31022 29816 29696 00000 00000 00000
0000320  25146 24878 28533 29696 00000 00000 00000 00000
0000340  25147 24878 29544 00000 00000 00000 00000 00000
0000360  25148 25134 29544 00000 00000 00000 00000 00000
0000400  25149 26223 28416 00000 00000 00000 00000 00000
0000420  25150 28773 29293 25448 25955 27438 29544 00000
0000440  25151 28773 29293 25448 25955 27438 29816 29696
0000460  04096 24930 25444 00000 00000 00000 00000 00000
0000500  25152 24930 25444 12908 28275 11892 30836 00000
0000520  25769 24930 25444 12908 28264 11892 30836 00000
0000540
```

## ls
```sh
$ ls -li abcd2*
5399721 -rw-rw-r--    2 txxxx txx             633 Sep 29 15:09 abcd2.txt
5399721 -rw-rw-r--    2 txxxx txx             633 Sep 29 15:09 abcd2lnh.txt
5399104 lrwxrwxrwx    1 txxxx txx               9 Oct 06 16:40 abcd2lns.txt -> abcd2.txt
$

Page 71 and 72 related to

od -d information with dir info ( i-node number) in the content as per book,
not available in the aix system. explore this....why not now???
```

******************************************************************************
## ls with inum

```sh
$ cat > 1.txt
111111111111
11111111
$ ls -l l
total 8
-rw-rw-r--    1 txxxx txx              22 Oct 07 14:35 1.txt


make a hard link ( i.e. same file in the file system but being referred by
different name)

$ ln 1.txt 2.txt

$ ls -li
total 16
4133478 -rw-rw-r--    2 txxxx txx              22 Oct 07 14:35 1.txt
4133478 -rw-rw-r--    2 txxxx txx              22 Oct 07 14:35 2.txt

ls -l reports 16 blocks, but no it is 8 blocks only in actual,
ls -l reports WRONG !
```

******************************************************************************

## cpying with
```sh
cp -R is better rather than -r ...
     it includes Special file types,
     1) FIFO files
     2) block and character device files.
    are re-created instead of copied.

    -h flag to follow symbolic links instead of copy.

but aix does not seem to find out


cp -R
cp -r
cp -Rh

all seems to work in the same manner.

$ ls -lR *
abcd1:
total 8
-rw-rw-r--    1 txxxx txx              18 Oct 12 15:15 file1.txt
lrwxrwxrwx    1 txxxx txx               9 Oct 12 15:18 file1_s.txt -> file1.txt

abcd2:
total 0
$
$
$
$ cp -Rh abcd1 abcd2/.
$
$

$ ls -liR *
abcd1:
total 8
4901190 -rw-rw-r--    1 txxxx txx              18 Oct 12 15:15 file1.txt
4901191 lrwxrwxrwx    1 txxxx txx               9 Oct 12 15:18 file1_s.txt -> file1.txt

abcd2:
total 0
2093126 drwxrwxr-x    2 txxxx txx             256 Oct 12 16:46 abcd1

abcd2/abcd1:
total 8
2093127 -rw-rw-r--    1 txxxx txx              18 Oct 12 16:46 file1.txt
2093128 lrwxrwxrwx    1 txxxx txx               9 Oct 12 16:46 file1_s.txt -> file1.txt
```

******************************************************************************

## ls 
```sh
$
$ pwd
/
$

$ ls -ld bin boot dev etc lib tmp unix usr
ls: 0653-341 The file boot does not exist.
lrwxrwxrwx    1 bin      bin               8 Jul 31 2014  bin -> /usr/bin
drwxrwxr-x    8 root     system        16384 Oct 13 13:00 dev
drwxr-xr-x   42 root     system        16384 Oct 13 12:51 etc
lrwxrwxrwx    1 bin      bin               8 Jul 31 2014  lib -> /usr/lib
drwxrwxrwt   36 bin      bin           16384 Oct 13 13:50 tmp
lrwxrwxrwx    1 root     system           21 Jul 31 2014  unix -> /usr/lib/boot/unix_64
drwxr-xr-x   60 bin      bin            4096 May 24 19:05 usr
$
```
******************************************************************************

## /etc/group
```sh
$ ls -l /etc/group   -------> lists members of each group.
-rw-r--r--    1 root     security       1663 Feb 05 2016  /etc/group

members of group "txx" can be found by reading "/etc/group" file

$ cat /etc/group
...
txx:!:5800:txxadmin,txxtest1,txxtest2,txxtest3,txxxx,txxtest5
...
```

******************************************************************************

## getty
```sh
$ which getty
/usr/sbin/getty

Purpose

       Sets the characteristics of ports.

//TODO: study more about getty


rc:
---
    system bootstrap
            |
            |
            |
            +----------> then
                        execute shell commands in /etc/rc file.

$ ls -l /etc/rc
-r-xr-xr--    1 bin      bin            3660 Jul 31 2014  /etc/rc
$
$
$ cat /etc/rc
.........hmm
you cant understand the contents as of now...
```

******************************************************************************

## /lib
```sh
ls -l /lib --> lists the ls format, but "ls -l /usr/lib" lists files.
So, if it is a link, "ls" will not expand the list to files.


$ ls -l /lib
lrwxrwxrwx    1 bin      bin               8 Jul 31 2014  /lib -> /usr/lib
$
$ ls -l /usr/lib
total 376712
drwxr-xr-x    3 bin      bin             256 May 24 19:06 IbBaseLib
lrwxrwxrwx    1 bin      bin              30 Jul 31 2014  IbBaseLib.a -> /usr/lib/IbBaseLib/IbBaseLib.a
drwxr-xr-x    2 root     system          256 Jul 31 2014  IbGxLib
drwxr-xr-x    2 bin      bin             256 May 24 19:05 IbMxLib
...
...
...
lrwxrwxrwx    1 root     system           36 May 24 19:12 libct_stackdump.a -> /usr/sbin/rsct/lib/libct_stackdump.a
lrwxrwxrwx    1 root     system           29 May 24 19:05 libct_tr.a -> /usr/sbin/rsct/lib/libct_tr.a
lrwxrwxrwx    1 root     system           34 May 24 19:05 libctmss_pkcs.a -> /usr/sbin/rsct/lib/libctmss_pkcs.a
-r--r--r--    1 bin      bin          418731 Jul 31 2014  libcur.a
lrwxrwxrwx    1 bin      bin              25 Jul 31 2014  libcurses.a -> /usr/ccs/lib/libxcurses.a
-r--r--r--    1 bin      bin          201428 May 24 19:05 libcxgb3-rdmav2.so
drwxr-xr-x    3 bin      bin             256 May 24 19:11 libdapl
...
...
...
lrwxrwxrwx    1 bin      bin              24 Jul 31 2014  vgrindefs -> /usr/share/lib/vgrindefs
lrwxrwxrwx    1 root     system           16 Jul 31 2014  wpars -> /usr/lib/corrals
drwxr-xr-x    3 bin      bin             256 Sep 26 2012  xorg
lrwxrwxrwx    1 bin      bin              18 Jul 31 2014  xpass -> /usr/ccs/lib/xpass
```

---
layout: post
title:  "Random C notes"
date:   2016-02-10 08:36:46 +0900
categories: c
---

## total memory by c prog.

```c
#include <unistd.h>
#include <sys/types.h>
#include <sys/resource.h>

main() {

	int a;
	
	a = 5 * 6;
	size_t total_memory = sysconf(_SC_AIX_REALMEM) * 1024;
	
	printf("%zu\n", total_memory);

}

/*also look in to vminfo.h*/
```

## sftp
```sh
sftp from bash command prompt
	sshpass
	
sftp sftp://${USER}@${HOST}

```


## simplified version of word count 
```c
#include <stdio.h>

main() {

	int i;
	i = 0;

	int str1 = getchar();

	while ( str1 != EOF ) {

		switch (str1) {

		case '\n':
		i++;
		break;

		case '\t':
		i++;
		break;

		case ' ':
		i++;
		break;

		}

		str1 = getchar();

	}

	printf("%d\n ", i);


}
```c


## TODO checkitout
```c

#include <stdio.h>

/* count digits, white space, others */
main()
{
   int c, i, nwhite, nother;
   int ndigit[10];
   nwhite = nother = 0;
   for (i = 0; i < 10; ++i)
     ndigit[i] = 0;
     while ((c = getchar()) != EOF)
       if (c >= '0' && c <= '9') {
          printf("%s\n", "found");
          printf("%d\n", c);
       } else
          printf("%s\t", "not found");
       /*
        ++ndigit[c-'0'];
       else if (c == ' ' || c == '\n' || c == '\t')
         ++nwhite;
       else
         ++nother;
    printf("digits =");
    for (i = 0; i < 10; ++i)
       printf(" %d", ndigit[i]);
    printf(", white space = %d, other = %d\n",
           nwhite, nother);
        */
}


c-'0' only will result correct int
the above pgm will result in following o/p.

$ a.out
ds12
not found       not found       found
49
found
50
not found

```

## process mgmt


## unix process mgmt
```c
lspv -- to list hdisk
70 hard disks

process mgmt


Structure
-------------------
|                 | also known as 
|   task_struct   | --------------> process descriptor   
|                 | 
-------------------

How to fine tune process execution and performance ? 
You have to know the task_struct ( process descriptor)


parent
    child
        fork()
        exec()  --> Copy On Write of "process descriptor"
        exit()---
                |
+--<<<---<<<<<--+
|
*
wait() 
    | then
    -->removes pd(process descriptor) detail.


------------------------------------------------------------------------------------------
-------------------
|                 | also known as 
| thread_info     | --------------> process descriptor   
|                 | 
------------------- 



^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

WHY thread is better
because they share the same resource ( process )
No need to copy resources on creation.
better peroformance.

^^^^^^^^^^^^^^^^^^^^^

Process priority 

dynamic priority
static priority --> can be adjusted by niche

Nice Level 
    -20 high
    -18

    ...
    0  default
    4 becoming low...


    19 low
           
change the nice level


-------------

Context switching is costly ? 
1. Data Info 
2. cache is copied 
---> Register

Context : a running process

 --------------Processor------------
|   --------Register-----------     |
|   |                         |     |
|   |   1) data info          |     | 
|   |   2) cache              |-----|------------------> data info
|   |                         |     |                       &
|   ---------------------------     |                    cache 
 -----------------------------------                that is copied called context
    
How many registers a processor has ?

Check the intel core i5 spec.

There are so many registers., when i look at the intel core i5 spec pdf.

32 bit processor }    refers to
                 }-------------->   Size of the register.
64 bit processor }

-------------------------------

Interrupt handling: 
    high priority

E.g. Keyboard/Mouse I/O device-->   Interrupt handler ---...
    ..-->   Process execution (already running) -->  Kernel switch to handle 


hard interrupt --> can NOT be found in /proc/interrupts
soft interrupt --> protocol operation TCP/IP, SCSI (Small Computer System Interface)
                                              derived from 
                                              SASI ( Shugart Associates System Interface) which is Seagate


-------------------

Process State:
-------------

TASK_RUNNING <-- SIGINT / SIGSTOP
TASK_STOPPED --> SIGCONT
TASK_INTERRUPTIBLE  
TASK_UNINTERRUPTIBLE

TASK_ZOMBIE (list out zombie process)


ps -T 20316976
list in TREE format


Process Memeory Segments:
--------------------------

Text : executables 
Data : 
    data : static variables 
    BSS : Block Started by Symbol
    Heap : malloc() 
Stack : local variables
        fn(param1, param2)
        return address of fn()

****** how to get parent id using program.
getpid() returns normally the pid of process
getppid() returns the shell running the pgm.
But I want to get the parent pid of shell.
how how how ???
```

## c code with math lib function
```c
When c file using math library function,
compile with -lm option
c99 -lm chap1m2.c
```


## some c code
```c
#include <stdio.h>


struct abcd {

        int a;
        short b;
        int c;
        char c1;
} myabcd1;


int main(int argc, char **argv) {
        printf("%d\n", sizeof(struct abcd));
        return 0;
}

ans:
int           4    4     4
short         2    2+2   4
int           4    4     4
char          1    1+3   4

----------------

struct abcd {

        int a;          4
        char c1;        1
        char b;         1
} myabcd1;


int main(int argc, char **argv) {
        printf("%d\n", sizeof(struct abcd));
        return 0;
}

size is 8

total size should be multiple of 4 ( otherwise this one will be of size 6)
```


## c math.sqrt
```c
sqrt  --> math.lib


mypoint1.c" 16 lines, 285 characters
-----------------------------------------
double sqrt(double);

struct point {
        int x;
        int y;
};

int main(int argc, char **argv) {
        struct point p1 = { 3, 4};
        printf("%d\t%d\t%lf\n", p1.x * p1.x , p1.y * p1.y, sqrt(p1.x * p1.x + p1.y * p1.y));   =======> lf
        return 0;
}

compile with -lm option

-l link to lib(m) library

libm.so --> mathlib 

```

 ## c notes
```c
1) no semicolon at the end of
  #define
2) no data type mentioned in the arguments of define


#include <stdio.h>

#define min(a, b) ( a <= b ? a : b )
#define max(a, b) ( a >= b ? a : b )

int main(int argc, char **argv) {
     printf("%d\n", min(4,3) <= max(4,3) );
     printf("%d\n", max(4,3) >= min(4,3) );
     return 0;
}

```


## structure returning structure
```c
#include <stdio.h>
#include <math.h>


//double sqrt(double);

struct point {
     int x;
     int y;
};


struct point makepoint(int x, int y) {
     struct point p1;  // ==>  no initialiation like in OOP lang = new P1() like ....
     p1.x = x;
     p1.y = y;

//        printf(">>>>>>>>this is fun\n");
     return p1;
}

int main(int argc, char **argv) {
//        struct point p1 = { 3, 4};
       struct point p1 = makepoint(3, 4);
     printf("%d\t%d\t%lf\n", p1.x * p1.x , p1.y * p1.y, sqrt(p1.x * p1.x + p1.y * p1.y));
     return 0;
}
```


// TODO
Bayesian filter(used in google) vs If filter (used in msft) read in Matt (redis) blog mentioned by Joel

  entire context of mail is taken for filtering spam.



## some more c notes
```c
/******************************************************************************/

for the following check :

isspace(getchar())


by enering

\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\

        |
abcd <--+



( abcd and then "Enter" ),

///////////////////////////////

isspace is "true"

/******************************************************************************/



when playing with char* ptr ,
like when you are doing *ptr++ , i.e. *(ptr++)

make sure you store the original location to have the value of "ptr".

otherwise after calculation, ptr pointing to the end of ptr value.

    as shown in the following example:
~~~~~~~~~~~~~~~~~~~~~~~
    #include <stdio.h>
    #include <ctype.h>

    int main(int argc, char const *argv[])
    {
        char *sh = (char*)malloc(sizeof(char) * 100);

        /* copy of variable NOT to affect original pointing location */
        char *shcpy = sh;

        while (  !isspace(*shcpy++ = getchar())  ) {

        }


        printf("#%s#\n", sh);
        return 0;
    }

~~~~~~~~~~~~~~~~~~~~~~

fseek,
ftell,
rewind

--------------------------------------------------------------------------------

can EOF be obtained by entering -1

        #include <stdio.h>

        int main(int argc, char const *argv[])
        {
            char c;

            while( (c = getchar()) != EOF ) {
                printf("%c ", c);
            }
            printf("\n");
            return 0;
        }


no, if you type -1, it will be two chars.

 d --------------> (Input)
d  --------------> (output)
 -1   -----------> i/p
- 1   -----------> o/p

```


1<<8 => 2pow8 => 256
2<<8 => 2 * 2pow8 => 512

but how to calculate

512>>8 =>

    1<<8
        then if the result is less than 512
        i.e. 512 greater than 1<<8 ( 2pow8)

        512/ (1<<8) => 2
following is the sample go program.
( ignore the type T struct part. it is not required for this sample.)


package main

import "fmt"


type T struct {
    name string
    val int
}

func main() {
    pr := fmt.Printf
    pr("%d\n", 512>>8)

    x := 1<<8

    y := 0
    if 512 > x {
        y = 512/x
    }
    pr("y=%d\n", y)

}

/******************************************************************************/

####



## c signal
```c
signal(SIGINT, sighandler);
                |
                +--> void sighandler(int signum) {
                }


signal fn. can be called by n no. of times.
the last one called will have an effect.

sample pgm.

/******************************************************************************/

#include <stdio.h>
#include <stdlib.h>
#include <signal.h>

void sighandler(int);
void sighandler2(int);

int main(int argc, char const *argv[])
{
    signal(SIGINT, sighandler);
    signal(SIGINT, sighandler2);
    while(1) {
        printf("%s\n", "this is mysignal...");
        sleep(1);
    }
    return 0;
}


void sighandler(int signum) {
    printf("%s%d\n", "signalnum=", signum);
}

void sighandler2(int signum) {
    printf("%s%d\n", "222signalnum=", signum);
}

/******************************************************************************/
following is the output

$ a.out
this is mysignal...
this is mysignal...
^C222signalnum=2   ----------> on ctrl + C ( first time )
this is mysignal...
this is mysignal...
this is mysignal...
this is mysignal...
this is mysignal...
^C$                -----------> on ctrl + C one more time.

/******************************************************************************/



## c notes
```sh
c realloc

realloc and assign tbe ptr to original ptr.

newsh = realloc(sh, sizeof(struct sdshdr)+newlen+1);


original ptr will be dellocated automatically.


as of now safe bet is

newsh = realloc(sh, sizeof(struct sdshdr)+newlen+1);
as in sdsMakeRoomFor method.

```


## c notes
```sh

When process is killed with SIGUSR1

the corresponding registered handler will be called.


sample....
# myfork1.c

#include <stdio.h>
#include <stdlib.h>
/*#include <signal.h>*/


void myhandler(int);

void myhandler(int signum){
    printf(">>>signum=%d %s\n", signum, "Phantom");
    /*exit(0);*/
}


int main(int argc, char const *argv[])
{
    printf("%s\n", "this is my fork");

    pid_t pid1;
    signal(SIGUSR1, myhandler);

    if (pid1 = fork() == 0) {
        printf("%s\n", ">>>Ghost");
        exit(0);
    }

    kill(pid1, SIGUSR1);
    printf("nINJA....");

    return 0;
}

/******************************************************************************/

```

## 1e3
```c

#include <stdio.h>
#include <stdlib.h>

int main(int argc, char const *argv[])
{
    printf("%s\n", "this is nanosec");

    printf("%d\n", 1e3);--> 1e3 some where in redis src found as a fix to 10e3
    return 0;
}
```

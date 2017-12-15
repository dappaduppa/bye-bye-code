---
layout: post
title:  "Random REDIS notes"
date:   2017-09-27 12:15:46 +0900
categories: C, REDIS
---


**sds** 


**mysample.c**

Creating Simple Dynamic String in c  


```
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

typedef char *sds;

struct sdsheader {
	int len;
	int free;	
	char buf[];

};


//sds sdsnewlen(const void *str, long len); 



sds sdsnewlen(const void *str, int len) {
	struct sdsheader *sh;
	sh = malloc(sizeof (struct sdsheader) + len + 1);
	sh->len = len;
	sh->free = 0;
	memcpy(sh->buf, str, len);
	sh->buf[len] = '\0';
	return sh->buf;
}

int main(int argc, char **argv) {
	sds str = sdsnewlen("funny", 5);
	struct sdsheader *sh = str - (sizeof(struct sdsheader));
	printf("%s\n", str);
	printf("%d\n", sh->len);
	printf("%d\n", sh->free);
	
	return 0;
	
}
```

## redis list
```txt
in redis, list is basically a linkedlist.
how to print boolean

%s for string
%b for boolean?
```
## redis notes
```c
redis

    main

        initServerConfig()
            malloc NOT needed for redisServer struct ? !
            since it is defined as (static)
            static struct redisServer server;
            // TODO
            I am not sure. have to check ...

            server
                dbnum
                port
                verbosity
                maxidletime
                savaparams, logfile = NULL
                ( logfile = NULL means std. o/p)


                ResetServerSaveParams()

                    /* reset only the saveparams..and
                    saveparams related stuff in
                    redisServer struct/object.
                    Not the redisServer .*/
                    free(server.saveparams);
                    server.saveparams = NULL;
                    server.saveparamslen = 0;

                    struct saveparams {
                        time_t seconds;
                        int changes;
                    };


                appendServerSaveParams()
                save after
                            1hr 1change
                            5 mins 100 change
                            1 min 10,000 change

                            is written as
                            appendServerSaveParams(1*60*60, 1);
                            appendServerSaveParams(5*60, 100);
                            appendServerSaveParams(60, 10,000);



        initServer()
            signal(SIGHUP, SIG_IGN);
            signal(SIGPIPE, SIG_IGN);


        ResetServerSaveParams()

        loadServerConfig()

        loadDb("dump.rdb")


        aeCreateFileEvent ----> aeEventLoop
                                fd   <----------------------<+
                                mask                         |
                                aeFileProc                   |
                                clientData                   |
                                aeEventFinalizerProc         |
                                                             |
            aeFileEvent fe...malloc it                       |
            /* File event structure */                       |          similar
            typedef struct aeFileEvent {                     |           to fd
                int fd; ----------------------> fd ---------->             |
                int mask; /* one of AE_(READABLE|WRITABLE|EXCEPTION) */ ---+
                aeFileProc *fileProc; // assigned from i/p param
                aeEventFinalizerProc *finalizerProc; // assigned from i/p param
                void *clientData;
                struct aeFileEvent *next; // aeEventLoop->fileEventHead
            } aeFileEvent;





        aeMain

        aeDeleteEventLoop
```


## more redis notes
```sh
sds: (S)imple (D)ynamic (S)trings

sds.h
=====
    include sys/types.h
            --> used for size_t, time_t variables...

    1) define sds alias for char*

    2) define sdshdr containing
        len,
        free,
        char[]

    sdsnewlen (char *str, size_t len)
            // copying string to another var. doesnt work as you expect.
            // you have to memcpy,

            struct sdshdr *sh = (void *)malloc(sizeof(struct sdshdr) + len + 1);
            sh->len = len;
            sh->free = 0;
            /*sh->buf = str;*/
            memcpy(sh->buf, str, len);

            sh->buf[len] = '\0';
            return sh->buf;
    sdsnew


        sds sdsnew(char *str) {
            return sdsnewlen(str, strlen(str));
        }

    sdsempty

    sdslen(sds s)
        s - sizeof(struct sdshdr) -> len
        -------------------------
            sdshdr struct ptr
    sdsdup
    sdsfree
        freeing the sdshdr ptr only.
        no need to free char buf[]
    sdsavail

    sdscatlen
    sdscat

    sdscpylen
    sdscpy

    sdscatprintf
    sdstrim
    sdsrange
    sdsupdatelen
            struct sdshdr *sh = (void*) (s-(sizeof(struct sdshdr)));
            int reallen = strlen(s);
            sh->free += (sh->len-reallen);
            ------------------------------

                sh->free + sh->len  => total length

                from the total length
                minus
                the actual len
                => set to the modified free length


            sh->len = reallen;
    sdscmp
    *sdssplitlen

    sdstolower
        include ctype.h for tolower fn.
                -------
                may be ctype => convert types?

```
---------------------------

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/

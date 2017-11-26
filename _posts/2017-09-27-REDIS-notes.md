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

---------------------------

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/

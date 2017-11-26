---
layout: post
title:  "Java Collections consolidated"
date:   2017-08-16 11:49:46 +0900
categories: Java, Collections
---

[SO: frequent : Java + Collections](https://stackoverflow.com/questions/tagged/collections+java?page=22&sort=frequent&pagesize=50)


1)   
[Convert String into ArrayList\<MyClass\>](https://stackoverflow.com/questions/24998055/convert-string-into-arraylistmyclass)  

```java 
package collect;

import java.util.regex.Matcher;
import java.util.regex.Pattern;

public class SamplePattern {

	public static void main(String[] args) {
		final String sampleStr = "[\"cd\",5,6,7], [\"rtt\",55,33,12], [\"by65\",87,87,12]";
		
		final Pattern pattern = Pattern.compile("\\[(.*?)\\]");
		final Matcher matcher = pattern.matcher(sampleStr);
		
		while(matcher.find()) {
			final String entireStr = matcher.group(1);
			System.out.println(entireStr);
			final String[] groups = entireStr.split(",");
			for (final String string : groups) {
				System.out.println(string);
			}
		}
		
		
	}

}
```

TODO: sample on  
  1) list.add / remove   
  2) parallely iterate iterator and see iterator is making copy of list and  
    not throwing CME ( ConcurrentModificationException )  
Note: For Each loop is using itertor beneath it.

---------------------------

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/

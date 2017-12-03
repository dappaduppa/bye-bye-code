---
layout: post
title:  "Welcome to Jekyll!"
date:   2017-05-23 06:07:46 +0900
categories: jekyll update
---
You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. You can rebuild the site in many different ways, but the most common way is to run `jekyll serve`, which launches a web server and auto-regenerates your site when a file is updated.

To add new posts, simply add a file in the `_posts` directory that follows the convention `YYYY-MM-DD-name-of-post.ext` and includes the necessary front matter. Take a look at the source for this post to get an idea about how it works.

Jekyll also offers powerful support for code snippets:

{% highlight ruby %}
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
{% endhighlight %}

Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk].

## my notes
------------------------------------------------------
jekyll

```sh
------------------------------------------------------
install and running  
gem install jekyll bundler
jekyll new my-awesome-site
cd my-awesome-site
~/my-awesome-site $ bundle exec jekyll serve
# => Now browse to 
-----------------------------------------------------
	delivery basic usage:
	---------------------
1)	
jekyll build
	# current dir into ./_site
2) 
jekyll build --destination <destination>
	# current dir into <destination>
	
3)
jekyll build --source <source> --destination <destination>
	# current <source> dir into <destination>
	
4) 
jekyll build --watch 
	# 1) plus
	# current dir watched for changes., and regenerated automatically.
	
	# _config.yml NOT included for automatic regeneration
	# "Data" files are reloaded
	
Site build removes <destination> dir.
if you want to keep files 
use <keep_files> configuration.
```
--------------------------------------------------------

```sh
	Serving the content
	
	
1) 
	jekyll serve
	* default option is to watch
	to disable watch 
	
	jekyll serve --no-watch
	
2)
	jekyll serve --detach 
	
In the root dir.
	/
		_config.yml
		( use spaces ONLY)
			source:    _source
			destination:    _deploy
	
	
then
	jekyll build
		(or)
	jekyll build --source _source --destination _deploy 
	
	are the same.
```

## For documentation:
-----------------
	gem install jekyll-docs
	
	then run
	
	jekyll docs ( from the terminal) --> have to see this at home.
	
Configuration:
-------------
	Jekyll manages Content-Type and Cache-Control
	
	Jekyll set Content-Type dynamically
	
	Jekyll fight chrome by specifying correct "Cache-Control" values   
		(statically, means not changes based on content-type)  
		to avoid caching during development time.  
		
	
Custom WEBrick Headers:
-----------------------
	# _config.yml  
	webrick:  
		headers:  
			My-Header: My-Value  
			My-Other-Header: My-Other-Value  
			
```sh
	
JEKYLL_ENV=production jekyll build  
 ( i think production / jekyll / build are separate values )  
then use it in the code like,  
{% if jekyll.environment == "production" %}  
   {% include disqus.html %}  
{% endif %}  
default value for JEKYLL_ENV is "development"
advantage: without changing config files, etc , ready to be deployed.
practically macys project comes to mind, have to use completely 
different configuration file.
With in single confiuration file, if we manage all the prod vs dev vs test environment settings,
chances are errors or missed out property will reflect the test or dev environment in production. Oops !
	Front Matter defaults:
	---------------------
	
	YAML Front Matter
	
triple dashed line
---
layout: post
title: blog like hacker
---
front matter variables are optional.
What is liquid tags and variables?
---------------------------------
Gobal variabes;
	layout : filename with out extension ( files placed in the _layouts dir )
	permalink: blog url other than site wide url setting
	published: false ( then, blog pages will not be included in the site generation )
	
	
	
	jekyll build --unpublished 
	jekyll serve --unpublished

```
	
-----------------


[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/

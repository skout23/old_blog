---
layout: post
title: Might help if I ...
category: 
tags: []
---
{% include JB/setup %}

take a few mins and learn markdown at some point. 
I am failing at it today.   
I setup a [toto][1] based blog, I followed these steps at [rubyinside][2]. 
I signed up with [heroku][3] and did the following:   

following:  

* sudo gem install toto
* sudo gem install heroku  
... 
* git clone git://github.com/cloudhead/dorothy.git my_new_blog
* cd my_new_blog
* heroku create
* modified config.ru to my needs
* heroku addons:add custom_domains
* heroku domains:add blog.skoutsec.org
* heroku domains:add blog.gambits.org
* added CNAME's to point to 'proxy.heroku.com.'
* rm articles/1900
* rake new
    * answered the questions
* added what you are reading now to the resulting file
* git add .
* git commit \-am 'Initial post'
* git push heroku master  


Now I need to get to work on the layout and old posts.

[1]: http://cloudhead.io/toto "toto"
[2]: http://http://www.rubyinside.com/deploy-blog-with-toto-and-heroku-2962.html/        "rubyinside"
[3]: http://api.heroku.com/signup  "Heroku Signup"

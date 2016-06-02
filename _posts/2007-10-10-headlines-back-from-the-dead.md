---
layout: post
title: "Headlines back from the dead"
category: 
tags: []
---
{% include JB/setup %}

So I have added back my headlines feature... somewhat... look to the right hand side of this page, and you will see the links.<a href="http://skoutsec.org//" onclick="return top.js.OpenExtLink(window,event,this)" target="_blank"></a>

I modified aggr (a wordpress plugin:<a href="http://wp-plugins.net/plugin/aggr/" onclick="return top.js.OpenExtLink(window,event,this)" target="_blank">http://wp-plugins.net/plugin<wbr></wbr>/aggr/</a> ) adding a limited link count, and limited title expansion, and documented my small changes on the authors website ( <a href="http://perassi.org/p/aggr/" onclick="return top.js.OpenExtLink(window,event,this)" target="_blank">http://perassi.org/p/aggr/</a> ).  Sounds more involved than it was... just added two variables and used substr to limit the titles to 40 chars (my pref).

Still need to flesh out a bunch of the feeds, and I need to work on the pre-caching / retention of the feeds.  I only have  world news / tech / rev eng currently pulling feeds at the moment.  Will work on the empty pages in the coming days.

I am not married to any of the feeds either, so if you know of a feed/section that should be represented and is not, let me know.  So
please make suggestions for the headlines.  in regards to the Programming and Sec pages, I have a definite idea of what I want to
accomplish... programming might be split into lang/arch specific pages.

---
layout: post
title: svn update
category: 
tags: []
---
{% include JB/setup %}

I can't tell you how much I like using svn to pull down the latest version of webapps rather than wget, and the whole untar and copy recipe.  So no longer do I have to run:
<code>
$ cd webdir/temp
$ wget http://wordpress.org/latest.tar.gz
$ tar xzvf latest.tar.gz
$ cd wordpress
$ cp -ar * ~/webdir
$ ...various cleanups here.
</code>

Now all I need do is:
<code>
$ cd webdir
$ svn update
$ cleanup stuff here if needed.
</code>

Much nicer!  Thanks for the SVN access, even if Linus hates svn/cvs.

For a complete guide on how to setup your own svn goodness, please read this <a href="http://codex.wordpress.org/Installing/Updating_WordPress_with_Subversion" title="WordPress with SVN" target="_blank">article</a>.

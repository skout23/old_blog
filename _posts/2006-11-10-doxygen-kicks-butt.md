---
layout: post
title: "Doxygen Kicks butt!"
category: 
tags: []
---
{% include JB/setup %}

So I finally broke down and spent an hour familiarizing myself with doxygen today... it is just plain cool!  So I will give a quick walk through of what I did to get more familiar with it.  FYI this is very UNIX specific at this point, however should translate to Win32 with little effort, or just send me the pre and I will generate docs for it and send you back the results.  I was very surprised at the terseness of the manual page for doxygen.  For some reason I had always thought doxygen was complicated.  To my surprise it was not, just needs to be configured properly and it is a generating fool.  So to begin..

~
[Doxygen][1]:

"Doxygen is a documentation system for C++, C, Java, Objective-C, Python, IDL (Corba and Microsoft flavors) and to some extent PHP, C#, and D."  

I created doxygen docs for the latest nmap project source, check it out.  The frontpage might look tiny, but this page is hiding 95Mb of html and png files.  Notice that every single UML graphic is click-able as well.
samples:  

THESE were removed since it was an older version of nmap.

It requires a graphviz program, "dot" to create the graphs, and I found I needed to have the FreeSans.ttf font available as well.  So for an archlinux distro (my personal favorite distro) it was a simple pacman (archlinux package management tool) command to get everything up and running.  

<pre>sudo pacman -S doxygen extra/graphviz fontconfig extra/ttf-cheapskate extra/ttf-ms-fonts</pre>  

Doxygen uses a configfile to determine how it will generate the requested documentation.  Now the configfile can be daunting, as a  
<pre>grep -v ^# DoxygenConfig.default | sort -u | wc -l </pre>  
results in 173 possible tags/options to parse.  But fret not, I went line by line and think I came up with a quite verbose(maybe too verbose) level of documentation generation, and I will provide the DefaultConfigFile I generated and modified to create the sample docs (latest nmap source).

My default doxygen config can be found at my public github repo [here][2].  

Only really need to change the following tags to get the same level of documentation as the sample.  

<pre>
PROJECT_NAME = "ProjectNameHere"
PROJECT_NUMBER = "0.0.1"
</pre>  

I did not set/mess with the  <pre>OPTIMIZE_OUTPUT_FOR_C</pre> or <pre>OPTIMIZE_OUTPUT_JAVA</pre> options since I was not certain how it would handle a C++ project, and have not run it against a java project yet.  A java project will be my next task.

I set my config to :
<pre>OUTPUT_DIRECTORY = Dox
GENERATE_HTML = YES
HTML_OUTPUT = html</pre>

which outputs all docs in html format into a dir structure of project/Dox/html.

There are a bunch of other output formats to choose from as well.  next think I will start creating CHM files of the generated docs as well.

Steps:
1. create the default Config  or just skip to 2 if you use my DoxygenConfig.default file
$ <pre>doxygen -g ConfigFileName</pre>

2. edit the configFile to suit your needs.
$ <pre>vi ConfigFileName</pre>

3. copy the DoxygenConfig file to the project root directory.
$ <pre>cp ConfigFileName some/path/to/project/src</pre>

4. cd into the top level directory of the project
$ <pre>cd some/path/to/project/src</pre>

5. start generating docs.
$ <pre>doxygen ConfigFileName</pre>

6. get some food/coffee, watch an episode of Lost.  nmap source took just about an hour to create.

7. review the docs in "some/path/to/project/src/Dox/html" in a browser and you are ready to start learning more than you probably will ever need to know about the code you are generating documentation for.

Many Thanks to the Doxygen team!

[1]: http://www.stack.nl/~dimitri/doxygen/index.html "Doxygen"
[2]: http://github.com/skout23/gambit_files "Config file download"

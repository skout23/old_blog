---
layout: post
title: "Jenkins and Selenium Grid == simple* headless ui testing"
category: 
tags: []
---
{% include JB/setup %}

So in and effort to write a bit more, here is a semi-quick lesson on getting selenium tests and testsuites to run via jenkins jobs.  On a related topic which somehow is completely different, I present a very quick howto on setting up the selenium grid (Selenium Plugin + manually updated jar) hub and first node in the grid on jenkins as well.  When I was setting these up to work together, I failed to grasp that these two pairings of Selenium and Jenkins were different beasts.  Which is my own fault for not reading more before jumping in.

## Common setup and requirements ##
	
  I am using Jenkins on a headless ubuntu ec2 instance.  I went the route of installing jenkins via [apt-get] [1].  I had issues trying to get jenkins to cleanly install and work under tomcat with a frontend of nginx, but I know it can work I was just getting it working fast. So with headless linux, to get browsers to work you need an xserver of some sort.  I went with [Xvfb] [2]
    .  I am sure there were other steps needed, but I am running out of steam on this post so will move on to the meat.

## Selenium IDE testSuites & Jenkins ##

  In Jenkins, install the SeleniumHQ [plugin] [3], this will download a standalone selenium server which you can then add to jenkins jobs as a "SeleniumHQ htmlSuite Run" build step.  A quick example would be to use the Selenium IDE firefox plugin to create a simple test, and testsuite. Export those test and testsuite as html files, and then publish the html files into your SCM of choice.  Create a new jenkins job, point the job's SCM at the location of where you stored the testsuites, and then add a build step which runs the testsuite, point the suiteFile and resultFile to the jenkins job workspace.  Run the jenkins job.  You should then have a resultfile which you can view via the jenkins job workspace.  (I know I should add more here, maybe in a later release)
	
## Selenium Grid ##
  If you want to develope more involved scripts and only run them via an exposed Seleniumn Grid, then in Jenkins install the Selenium [Plugin] [4], which is different then the previous HQ plugin.  This will start up an always on Selenium Grid hub, which will wait for nodes to attach to.  At first I ran into problems because of the dual install of the HQ plugin, to solve this I changed the port the grid starts up on (4040), to a different port.  I then failed to RTFM and had trouble when pointing scripts to the hub.  What I needed to do first was attach worker nodes to the hub, and then my remote scripts worked.  To get an always on worker node, I just started up an always on worker node running on the same jenkins box.  Take note NODEPORT and HUBPORT need to be suited for your jenkins env.

     java -jar selenium-server-standalone-2.30.0.jar -port NODEPORT -role node -hub http://localhost:HUBPORT/wd/hub

  After that I had a working grid, to which I could run my remote scripts against.

[1]: https://wiki.jenkins-ci.org/display/JENKINS/Installing+Jenkins+on+Ubuntu "apt-get"
[2]: http://alex.nederlof.com/blog/2012/11/19/installing-selenium-with-jenkins-on-ubuntu/ "much better post"
[3]: http://wiki.hudson-ci.org/display/HUDSON/Seleniumhq+Plugin "Seleniumhq plugin"
[4]: http://wiki.jenkins-ci.org/display/JENKINS/Selenium+Plugin "Selenium Plugin"
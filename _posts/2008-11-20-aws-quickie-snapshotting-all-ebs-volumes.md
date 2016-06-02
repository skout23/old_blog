---
layout: post
title: "AWS quickie snapshotting all EBS volumes"
category: 
tags: []
---
{% include JB/setup %}

So this might be of some use to some folks.  I use <a title="AWS" href="http://aws.amazon.com" target="_blank">AWS</a> a bunch for work, and am really happy that <a title="EBS" href="http://aws.amazon.com/ebs/" target="_blank">EBS</a> is now very stable.  Recently I have been moving away from using <a title="RightScale" href="http://www.rightscale.com/" target="_blank">RightScale</a> as my main interface to AWS, and getting more interested in available command-line and API level tools.  The one that has caught my fancy this last week was the rubygem <a title="amazon-ec2" href="http://github.com/grempe/amazon-ec2/tree/master" target="_blank">amazon-ec2</a>, <!--more-->it provides ruby methods for most if not all exposed EC2 API calls, and also comes with a nice tool ec2sh, which is a shell for using AWS.  Problem is that besides the basic <a title="tutorials" href="http://www.fiveclouds.com/2008/05/03/getting-off-the-ground-with-ec2/" target="_blank">tutorials</a>, and the source code, there was not many docs providing practical usage for this gem and shell.  So to help remedy that I am posting a quick command I use daily.  Here is a command you can throw at the ec2sh shell to get it to snapshot all your attached volumes.

<code>
@ec2.describe_volumes.volumeSet.item.each{ |volume| puts "Creating snapshot of #{volume.volumeId}"; @ec2.create_snapshot(:volume_id =&gt; volume.volumeId)} == true
</code>

I have tried putting this into a simple ruby script, however I apparently fail at gem updates.  if you have some practical tips in dealing with AWS, feel free to post comments, or send me a link.

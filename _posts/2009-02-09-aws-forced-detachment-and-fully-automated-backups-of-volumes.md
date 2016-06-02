---
layout: post
title: "AWS forced detachment and fully automated backups of volumes"
category: 
tags: []
---
{% include JB/setup %}

*EDIT 2012-06-26 * a better / updated version of this is now one of my [GIST's][1] 

Just a quick one today.  More uses of the ec2 gem.  Here is a command to force a volume to detach via ec2sh:

  @ec2.detach_volume(:volume_id=>;"INSERT_YOUR_VOLID", :force=>;'true')

And here is a script to handle all your backup needs.  It deletes snapshots older than 2 weeks and then takes a snapshot of all your current vol's .  (attached or not)  (you can play with this pretty easily). 

*EDIT* updated the script to delete first, and then snapshot, also added some comments to the code.

[1]: https://gist.github.com/3001264 "EBS Gist"


    #!/usr/bin/env ruby

    require 'rubygems'
    require 'EC2'

    ACCESS_KEY_ID='YOUR_KEY'
    SECRET_ACCESS_KEY='YOUR_SECRET_KEY'

    # Target date 2 weeks = (60 secs * 60 mins * 24 hours * 14 days)
    targetDate = Time.now - (60 * 60 * 24 * 14)

    # instanciate a class object connection to ec2 
    ec2 = EC2::Base.new(:access_key_id => ACCESS_KEY_ID, :secret_access_key => SECRET_ACCESS_KEY)

    # access all snapshots associated with your aws account
    # from the class object then delete the snapshots
    # older than targetDate

    ec2.describe_snapshots.snapshotSet.item.each do |snapshot|
      if Time.parse(snapshot.startTime) < targetDate
        puts "About to delete #{snapshot.snapshotId}"
        ec2.delete_snapshot(:snapshot_id => snapshot.snapshotId)
      end
    end


    # access all volumes associated with your aws account 
    # available from the class object then create a snapshot
    # of each volume (attached or not)

    ec2.describe_volumes.volumeSet.item.each do |volume|
      puts "Creating SNAPshot of #{volume.volumeId}"
      ec2.create_snapshot(:volume_id => volume.volumeId)
    end

Hope someone finds this useful, let me know if you are doing interesting stuff with AWS or various gems that interact with it.  Lets automate this stuff and get busy having fun.

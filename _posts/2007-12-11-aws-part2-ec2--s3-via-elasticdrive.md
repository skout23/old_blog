---
layout: post
title: "AWS part2 EC2 + S3 via ElasticDrive"
category: 
tags: []
---
{% include JB/setup %}

EDIT: Nevermind all this FUSE based solutions … they are pitiful when
compared to Amazon EBS… just stick with that, and make sure you build
your volumes in the same datacentre as the running instance.

–

I know it is bad form to post a part 2 before posting a part 1, but I
wanted to get this posted ASAP, so that I would not forget it. I have
been working with AWS recently, and here is a quick howto which cobbles
together some stuff that was not clear to me before.

Caveats: mounting, formatting, unmounting, and syncing data on the
raided devices is slow, like 2 hours to format a 160G partition slow,
unmounting taking upwards of 20mins, and still not sure how long it
takes to sync data. It could be that I am doing something really stupid,
so please review and let me know if I can speed this up. Thanks

Note: I just tried
`echo $((30*1024)) &gt; /proc/sys/dev/raid/speed_limit_min` will see how
that turns out.

1.  Backup any data that might be on the /mnt partition or the /data
    directory:
    `# mkdir /root/backup # cp -R /mnt/* /root/backup # cp -R /data/* /root/backup !!!! watch out for this next one if you are trying to recover # rm -rf /data/*`

2.  Unmount the default /mnt partition (aka /dev/sda2): `# umount /mnt`

3.  Install the needed FUSE, xfs, raid and lvm libraries and packages:
    `# apt-get install libfuse-dev libfuse2 fuse-utils python-dev mdadm lvm2 xfsprogs`

4.  Load the fuse kernel module: `# modprobe fuse`

5.  Download and install elasticdrive:
    `# wget http://www.elasticdrive.com/uploads/media/elasticdrive-0.4.0_dist.tar_01.gz # tar xzvf elasticdrive-0.4.0_dist.tar_01.tgz # cd elasticdrive-0.4.0_dist # ./install`

6.  Edit the /etc/elastic.ini file to meet your needs, the following is
    geared towards a raid1 setup of /dev/sda2 & /fuse/ed0 . The file is
    pretty much stock except the drives section, I increased the
    maxthreads to improve the write performance, might need to play with
    the ttl as well. The following blocksize and blocks creates a drive
    that is close to the default /dev/sda2 so that we can raid1 them.
    change S3Key, S3Secret, and S3Bucket to the target:
    `# vim /etc/elasticdrive.ini`

`[drives] fuseblock|/fuse="s3://S3key:S3Secret@aws.amazon.com/?bucket= S3BUCKET&amp;;stripesize=65536&amp;;blocksize=4096&amp;;blocks= 41092092&amp;;sizebyblocks=1&amp;a;maxthreads=25&amp;;ttl=40"`

1.  Start the elasticdrive service:
    `# elasticdrive /etc/elasticdrive.ini -d`

2.  Verify the /fuse/ed0 `# ls -al /fuse/ed0`

3.  Setup the loop device and RAID1:
    `# losetup /dev/loop0 /fuse/ed0 # /etc/init.d/mdadm-raid start #  mdadm --create /dev/md0 -n2 --level 1 /dev/sda2 --write-mostly /dev/loop0`

4.  Verify /dev/md0 `# cat /proc/mdstat`

!! Note might want to wait for the /dev/md0 to finish syncing before
going forward.

1.  LVM setup (optional)
    `# /etc/init.d/lvm start # pvcreate /dev/md0 # vgcreate S3Data /dev/md0 # lvcreate -L 150G -n lv0 S3Data`

2.  Format and mount the drives: a) if you decided to just used mdadm
    raid1 without the LVM `# mkfs.xfs /dev/md0 # mount /dev/md0 /data`

b) if you go the LVM route
`# mkfs.xfs /dev/S3Data/lv0 # mount /dev/S3Data/lv0 /data`

Clean bring down procedures, could take over an hour:

1.  Unmount the LVM or RAID1 `# umount /data # /etc/init.d/lvm stop`

2.  Stop the RAID1, and stop the loop device
    `# mdadm -S /dev/md0 # /etc/init.d/mdadm-raid stop # /etc/init.d/mdadm stop # losetup -d /dev/loop0`

3.  Kill the elasticdrive service `# pkill elasticdrive`

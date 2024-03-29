---
layout: post
title: "Step-by-step instructions for debugging IOS using gdb"
category: 
tags: []
---
{% include JB/setup %}

First off, this is not my work.  It is the work of Andy Davis.  I was just not able to find a place to link to with this info, so thought I would repost, if this is your work, and you would like me to remove it please just let me know.

Step-by-step instructions for debugging IOS using gdb - Andy Davis,
2008 (iosftpexploit "at" googlemail &lt;dot&gt; com):

I have been asked by many people for a simple step-by-step guide for
setting up an IOS exploit development environment, which includes
connecting to a Cisco router using gdb, so here goes:

(By the way the router I connect to is a Cisco 2621XM)

Installing and configuring minicom:

In Ubuntu type "apt-get install minicom"

Connect the console port of your router to your PC using a Cisco UTP DB9 cable

type "minicom -s"

Scroll down to "Serial port setup"

Set the "Serial device" to "/dev/ttyS0" (COM1 - or whatever your
router is connected to on your PC)

Set "Bps/Par/Bits" to "9600 8N1"

exit the submenu then scroll down to "Modem and dialling"

Set "Init string" and "Reset string" to be blank

exit the submenu then scroll down to "Save setup as dfl"

Exit

Type "minicom" - hit return a few times and you should have an IOS prompt

Exit minicom by typing "ctrl-a" then "x" return

Installing and configuring gdb:

Go to http://ftp.gnu.org/gnu/gdb/

Download "gdb-6.0.tar.gz" and "gdb-6.1.1.tar.gz"

tar xvfz gdb-6.0.tar.gz
tar xvfz gdb-6.1.1.tar.gz

copy "gdb-6.1.1/include/obstack.h" to "gdb-6.0/include/" (obstack.h in
gdb 6.0 is broken)

edit gdb-6.0/gdb/remote.c

on line 4261

change "static int remote_cisco_mode;" to "static int remote_cisco_mode = 1;"

edit gdb-6.0/sim/ppc/ppc-instructions

on line 1285, under "LABEL(Done):"

Add the line "(void)0;"

cd gdb-6.0/

./configure --target=powerpc-elf

make

make install

In your home directory create a file called ".gdbinit" containing the following:

target remote /dev/ttyS0 (or whatever port your router is connected to)

Connecting to the router using gdb:

type "minicom" to connect to the router

enter "enable mode" by typing "en" followed by the enable password

type "gdb kernel" - the router will display the following:

||||

Type "ctrl-a" then "x" to exit minicom

type "powerpc-elf-gdb"

gdb will connect to the router via the serial cable and display the following:

GNU gdb 6.0
Copyright 2003 Free Software Foundation, Inc.
GDB is free software, covered by the GNU General Public License, and you are
welcome to change it and/or distribute copies of it under certain conditions.
Type "show copying" to see the conditions.
There is absolutely no warranty for GDB.  Type "show warranty" for details.
This GDB was configured as "--host=i686-pc-linux-gnu --target=powerpc-elf".
warning: Relocation packet received with no symbol file.  Packet Dropped

0x00000000 in ?? ()

Congratulations you are now debugging IOS ;-)

One unusual feature, which I have yet to explain is that when the
registers are displayed they are all offset by 1 e.g:

(gdb) info reg
r0             0x50     80
r1             0x1      1
r2             0x81c97498       -2117503848
r3             0x816e0000       -2123497472
r4             0x8195e054       -2120884140
r5             0x81c974b0       -2117503824
r6             0x3      3

The register displayed as r0 is a bogus value and the value of r0 is
actually 0x00000001, r1 is 0x81c97498 etc.

Another "feature" of debugging IOS with gdb is that when you set a
breakpoint and then continue running IOS, when the breakpoint is
triggered, gdb has actually overwritten the instructions at the
address at which the breakpoint was set with the value 0x7d821008 and
therefore, you need to take a note of the bytes associated with the
instruction at that address and replace them after the breakpoint has
been triggered before continuing.

To continue normal execution of IOS from within gdb:

Type "c" return
Hit ctrl-c twice
Type "y" return
Type "quit" return

Hopefully this information will promote further IOS security research

Cheers,

Andy

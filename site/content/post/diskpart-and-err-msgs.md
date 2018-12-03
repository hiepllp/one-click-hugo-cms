---
title: Diskpart and err msgs
date: 2018-12-03T02:12:52.965Z
description: >-
  Credit to Wolfgang

  http://www.jwgoerlich.us/blogengine/post/2009/11/05/Use-Diskpart-to-Create-and-Format-Partitions.aspx
---
To use the command line to bring a disk online, create a partition, and format it, run the following commands:



 C:\> Diskpart



DISKPART> list disk

DISKPART> select disk (id)

DISKPART> online disk (if the disk is not online)

DISKPART> attributes disk clear readonly

DISKPART> clean

DISKPART> convert mbr (or gpt)

DISKPART> create partition primary

DISKPART> select part 1

DISKPART> active (if this is the boot partition)

DISKPART> format fs=ntfs label=(name) quick

DISKPART> assign letter (letter)

DISKPART> list volume





The following are common errors displayed if you miss a step:



DISKPART> clean

DiskPart has encountered an error: The media is write protected.

See the System Event Log for more information.



Resolution: run "attributes disk clear readonly" before trying to clean the volume and create the partition.



DISKPART> convert mbr



Virtual Disk Service error:

The specified disk is not convertible. CDROMs and DVDs

are examples of disks that are not convertable.



Resolution: clear all data off the disk before converting by running the clean command.



DISKPART> create partition primary

Virtual Disk Service error:

There is not enough usable space for this operation.



Resolution: run "clean" before trying to create the partition.



DISKPART> format fs=ntfs quick

Virtual Disk Service error:

The volume is not online.



Resolution: online the disk, create the partition, and convert to mbr before formatting.





The following are common errors displayed if there is a hardware problem:



DISKPART> clean

DiskPart has encountered an error: The device is not ready.

See the System Event Log for more information.



Resolution: If the event log entry states "The driver detected a controller error on \Device", the problem is likely your storage controller on your mainboard. Check your hard drive connections and reload your storage conroller driver. If the event log entry states "VDS fails to write boot code on a disk during clean operation. Error code: 80070015@02070008", the hard drive itself has failed.





The following are common errors you may see if there is a hardware problem:



DISKPART> clean

DiskPart has encountered an error: The media is write protected.

See the System Event Log for more information.



Resolution: Running "attributes disk clear readonly", as mentioned above, is the first step. Next, the disk is may be locked by an active process, in which case a reboot generally clears the error.



If the disk is a USB flash drive, check that it does not have a write protect switch. The Imation Clip and the Kanguru Flash Blu II, for example, both have this feature and will cause the above error if protected. USB devices may also need a low-level format to reset the drive. In these cases, please see the manufacturer for such tools. (Patroit has such a tool here.)



If the disk is SAN attached storage, check that the LUN is not presented in a read-only state. Windows Server 2008 also has a condition were SAN storage is erroneously reported as read-only. See the Microsoft article 971436 for details and a hotfix.

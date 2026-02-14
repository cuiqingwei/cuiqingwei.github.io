---
layout: post
title: 'Kali Linux Live USB on MAC'
date: 2016-10-10 02:27:40
comments: true
tags:
  - kali-linux
  - linux
---

1. download Kali Linux iso images from http://www.kali.org/downloads/
2. Format the usb stick in disk utility as msdos
3. Open Terminal Window and run the following

```bash
➜  ~ diskutil list
/dev/disk0 (internal, physical):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:      GUID_partition_scheme                        *128.0 GB   disk0
   1:                        EFI EFI                     209.7 MB   disk0s1
   2:                  Apple_HFS OS X                    127.2 GB   disk0s2
   3:                 Apple_Boot Recovery HD             650.0 MB   disk0s3
/dev/disk1 (internal, physical):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:      GUID_partition_scheme                        *500.1 GB   disk1
   1:                        EFI EFI                     209.7 MB   disk1s1
   2:                  Apple_HFS WORK                    400.0 GB   disk1s2
   3:                  Apple_HFS DATA                    99.6 GB    disk1s3
/dev/disk2 (external, physical):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:     FDisk_partition_scheme                        *8.0 GB     disk2
   1:                 DOS_FAT_32 KALI                    8.0 GB     disk2s1
```

<!--more-->

4. my USB device is /dev/disk2, Then unmount disk by diskutil command

```bash
➜  ~ diskutil umountDisk /dev/disk2
Unmount of all volumes on disk2 was successful
```

5. You may have to run this as sudo

```bash
sudo dd if=kali-linux-2016.2-amd64.iso of=/dev/disk2 bs=512 conv=noerror,sync
6009312+0 records in
6009312+0 records out
3076767744 bytes transferred in 2227.332489 secs (1381369 bytes/sec)
```

Waiting for complete
When It success

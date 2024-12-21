---
title: 'Notes about an ORICO enclosure (TX25C3/RTL9201)'
date: 2024-12-21T10:26:46+07:00
# weight: 1
# aliases: ["/first"]
tags: [" "]
author: "dinhcap"
# author: ["Me", "You"] # multiple authors
showToc: true
TocOpen: false
draft: false ############CHANGE
hidemeta: false
comments: true
description: "ORICO main pages and support rarely discloses their chipset so here's my two cents." #CHANGE
#canonicalURL: "https://canonical.url/to/page/"
disableShare: false
disableHLJS: true
hideSummary: false
searchHidden: false
ShowReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
ShowWordCount: true
ShowRssButtonInSectionTermList: false
UseHugoToc: false
editPost:
    URL: "https://github.com/dng-nguyn/dinhcap-dev/tree/main/content/"
    Text: " Suggest Changes" # edit text
    appendFilePath: true # to append file path to Edit link
---

This is a Type-C to Type-C enclosure, with the Realtek RTL9201 chipset:
```sh
Bus 002 Device 003: ID 0bda:9201 Realtek Semiconductor Corp. RTL9201
```
It has support for UASP, TRIM and SMART, but the latter was not mentioned.

For SMART, existing versions of smartmontools output the following:
```sh
root@proxmox:~# smartctl -a /dev/sdc

smartctl 7.3 2022-02-28 r5338 [x86_64-linux-6.11.0-1-pve] (local build)
Copyright (C) 2002-22, Bruce Allen, Christian Franke, www.smartmontools.org

/dev/sdc: Unknown USB bridge [0x0bda:0x9201 (0xf200)]
Please specify device type with the -d option.

Use smartctl -h to get a usage summary
```
Support for the device was only added in Oct 15, 2024 ([Changeset #5621](https://www.smartmontools.org/changeset/5621))

The two workarounds are:

Appending `-d sat`, e.g. `smartctl -d sat -a /dev/sdx` to existing versions also works.
 
Using [newer versions](https://builds.smartmontools.org/) that fixed this. These may be unstable.


In Windows, it works fine using CrystalDiskInfo.
---
title: 'GMKTec NucBox M5 Upgraded (M5 PRO) Review as a Home Server.'
date: 2024-05-30T16:21:01+07:00
# weight: 1
# aliases: ["/first"]
tags: ["review"]
author: "dinhcap"
# author: ["Me", "You"] # multiple authors
showToc: true
TocOpen: false
draft: false ############CHANGE
hidemeta: false
comments: true
description: "A Budget Mini PC option for $200" #CHANGE
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

Originally, I've wanted a new computer to replace my current one. It's an old Xeon E3-1230v2 with 4 cores and 8 threads, with more than enough power to 'currently' support the stuff that I use/host. However, the expandability was not so great: A H61 motherboard with 2 DDR3 ram slots, at a maximum of 16GB total. The machine was also used for regular web browsing and writing for this website (yay) as a secondary computer so there's no room to add more stuff. On top of that, it sips 60-70 watts on idle! So my idea was to get an Intel N100 Mini PC, but after weighing the pros and cons, I've decided to spend more and settle on this 5700U machine because of it's efficiency, more PCIe lanes and RAM support.

{{< figure src="5700u-fp6.webp" link="5700u-fp6.webp" caption="The Zen 2 Mobile FP6 Platform. Supports 2 full x4 lanes for NVMe" align=center >}}

# Overview of the unit.

{{< figure src="exploded.webp" link="exploded.webp" caption="The exploded view of the PC" align=center >}}

I got the barebone unit for the base price of $200 via a local reseller. This is the best price you could ask for a mini PC with this much features. Here are some of the specs:

## CPU

### Ryzen 7 5700U

The core of the computer features a Zen 2 (2.5 generations ago) processor.

It is a Zen 2 refresh on a 5th gen series. 8 cores and 16 threads. Thermals are quite terrible though. There are 3 options in the BIOS: Quiet (10W), Balanced (15W) and Performance (35W). From what I've observed, the Quiet preset locks the CPU at 1.8 GHz, Balanced is, well, balanced and Performance is both a noise and temps disaster, always throttle at 90 degrees under any type of workload, even single core. I supposed that the temperatures would be somewhat better compared to a traditional laptop, but this is literally what laptop temps are. I have also repasted the CPU, but it was to no avail. Under most tasks, balanced is adequate and suits my needs.

{{< figure src="generic-bench.webp" link="generic-bench.webp" caption="Some generic benchmarks." align=center >}}

For more general benchmarks for desktop use cases, check out [This review on notebookcheck.net](https://www.notebookcheck.net/GMK-NucBox-M5-mini-PC-review-AMD-Zen-2-is-feeling-long-in-the-tooth.822952.0.html), as it covers everything about the unit performance under different tasks, as well as noise levels.

## Wi-Fi & Bluetooth

### MediaTek MT7922 Wi-Fi 6/6E & BT 5.2

It's a cheap Wi-Fi card, but with AP Mode:

```sh
Supported interface modes:
        * managed
        * AP
        * AP/VLAN
        * monitor
        * P2P-client
        * P2P-GO
```

Which is insanely good. It also fully supports 3 bands: 2.4, 5 and 6, but I don't have any 6 GHz devices in my household, so I can't confirm that. With OpenWRT, 802.11ax mode at 5GHz, I got a max negotiated speed of 866 Mbit/s, though my end devices aren't fast enough.

{{< figure src="wifi-speed.webp" link="wifi-speed.webp" caption="The Wi-Fi speed with OpenWRT. It reaches the max speed under thre right conditions." align=center >}}

Additionally, there's only one interface available, so one band at a time.

For Bluetooth, I haven't found a use case for it as a home server.

## Ethernet

### Realtek RTL8125 2.5GbE Dual

This is the most significant change to the original M5, as the original one used Intel NICs. This means that you won't be able to use this for ESXi or vSphere, but I don't plan on using those on bare-metal anyways. Other than that, the NICs function normally, with no packet drops (as far as I'm concerned with my stuff) and disconnects, work well on Proxmox. 

## I/O Ports

The images provided by GMKTec are self-explanatory.

{{< figure src="io.webp" link="io.webp" caption="Adequate ports. The two other sides are air vents." align=center >}}

I/O Ports should not be a big issue for servers though, since one doesn't need many for a 24/7 machine. Additionally, you could always add more ports with USB hubs.

## My own parts

As the unit is barebone, so I had to supply it with my own RAM and Storage. Here's what I've chosen:

### RAM

I paired the unit with a single stick of G.Skill Ripjaws 32GB 3200MHz CL22 (Model F4-3200C22S-32GRS). The stick is recognized and runs at the full 3200MHz speed, without additional configurations (If there are even ways to configure it in the first place). I bought a single stick because I want additional room to upgrade it later.

### Storage

For storage, I got a single SK Hynix PC801 (OEM version of the P41 Platinum) 512GB. I bought it 'used' (2TB written, 40hrs power on) for $30 which was a no-brainer. It's a high-end SSD at an entry-level price.

{{< figure src="virtio-disk-bench.webp" link="virtio-disk-bench.webp" caption="SSD performance on a Windows 11 VM with VirtIO SCSI." align=center >}}

I also have a spare drive so I used a UGREEN 2.5" enclosure (SKU 30727 or Model CM471 with ASM235CM chipset) for a 500GB HDD (It's 10 years old at this point lol). Contrary to what other posts I've found from other people, I have not had any 'random' disconnections whatsoever. As unreliable as the USB connection looks, the drive functions as expected. The disk is pass-through'd to a Nextcloud instance as a local storage, and everything works well, even though the initial delay and speed is somewhat slower than a traditional SATA connection.



# The build.

*..It's all plastic really.*

The unit features two fans, one little fan on the cover and one for the heatsink. The little fan on top features 0 use as extremely small and acts as a placebo. There is no room for it to take air in, so it shouldn't exist in the first place.

{{< figure src="fan.webp" link="fan.webp" caption="The fan in question. It's a 40mm or 50mm fan, I don't have a ruler with me. Note that there's a cover on top so the fan is effectively blocked." align=center >}}

As mentioned, the fan on the heatsink can get pretty loud, but only in Performance mode. In Balanced mode, the CPU gets throttled enough so that it doesn't get hot enough, and as a result it is really silent.

There is enough headroom for the first SSD heatsink slot, but probably impossible for the second drive because of the small fan above it.


There are 4 Phillips #0 screws that need to be removed in order to access the SSD and RAM slots, and they also provide you with a cheap screwdriver. I can't really tell whether these screws get stripped really easily, or that might be my own skill issue. Now I have to use a flat-head screwdriver for some of the screws.

{{< figure src="open.webp" link="open.webp" caption="Pretty easy to access the internals. The case is also used as an Wi-Fi antenna, as the wires are soldered directly onto the plastic." align=center >}}

To access the CPU and the heatsink, you need to remove 11 (!) more screws. The first 4 are for the motherboard and the case, which you need to bend the plastic case to get the motherboard out. The remaining 3 and 4 are for the heatsink fan and the heatsink itself, respectively. Funnily enough, the fan actually have 4 screw holes but they only used 3, the other is left unused because of the CMOS battery in place.

{{< figure src="bottom.webp" link="bottom.webp" caption="The heatsink. It's inefficient and terrible. There aren't any fins on top of the CPU but instead they put them on the side where the air blows." align=center >}}

There's just the CPU in contact with the heatsink.

## Notes

There is a problem with the BIOS for most units which prevents you from installing Proxmox/Linux distros. The error usually goes like this:

```bash
EFI boot mode detected, mounting efivars filesystem mount: /sys/firmware/efi/efivars: mount(2) system call failed: Operation not supported.
dmesg(1) may have more information after failed mount system call.
Installation aborted - unable to continue (type exit or CTRL-D to reboot)
```

or something around the line of 

```bash
modprobe: ERROR: could not insert 'kvm_amd': Operation not supported
```

Apparently this issue only occurs on the new models (M5 Pro), not the old ones.

This is resolved by enabling "NX Mode" inside the BIOS option Advanced > CPU Configuration. Though some people reported that this does not work. Alternatively, you can install Proxmox/Linux distros on another computer and then transfer the drive to the machine. If you decide to use Ubuntu, their provided images work, though I don't recommend using those.

# Takeaway

Here are my opinions on the unit as a home server:

### Pros:

- 8 cores 16 threads, plenty for any type of workload.
- Small, quiet (under Balanced mode) and low power usage.
- Dual 2.5GbE NICs (Realtek).
- Best unit (in my opinion) in the $200 price range.
- Wi-Fi card can be used as an AP.

### Cons:

- Placebo fan.
- Miserable thermals - get hots really easily under load.
- Dual 2.5GbE NICs (Realtek) (if you use VMware).
- No 2.5" slots available.

{{< figure src="server.webp" link="server.webp" caption="The entire \"server\" itself" align=center >}}

I've been using the unit as the main server for a few weeks, and most problems I've encountered were of my skill issues, rather than the machine itself. It's a great server overall, though the temps can be better. It runs 24/7 in my room now, and is way better than the previous one.
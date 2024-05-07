
---
title: 'Cool Applications and Services'
date: 2024-05-07T20:40:36+07:00
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
description: "These are random applications/services that I recommend/use on a daily basis." #CHANGE
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
# Tools for Windows specifically


## [Microsoft Activation Scripts (MAS)](https://massgrave.dev)

Open source, one of the most widely used activation script. Used by MS support employees, so why not?
The script source code is even [hosted on GitHub](https://github.com/massgravel/Microsoft-Activation-Scripts), which itself is owned & operated by Microsoft. MAS provides activation for nearly all Windows and Office distributions, even ones that you didn't know existed. They have activation methods different from tools like KMSPico and other volume-based licensing, which is cool.
<blockquote class="twitter-tweet" data-dnt="true" data-theme="dark"><p lang="en" dir="ltr">I can&#39;t believe it.<br>My official Microsoft Store Windows 10 Pro key wouldn&#39;t activate. Support couldn&#39;t help me yesterday.<br><br>Today it was elevated. Official Microsoft support (not a scam) logged in with Quick Assist and ran a command to activate windows.<br><br>BRO IT&#39;S A CRACK<br>NO CAP <a href="https://t.co/0vcRGu9PDE">pic.twitter.com/0vcRGu9PDE</a></p>&mdash; TCNO/TroubleChute (@TCNOco) <a href="https://twitter.com/TCNOco/status/1634620446002774018?ref_src=twsrc%5Etfw">March 11, 2023</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

Running the script is as easy as

```powershell
irm https://massgrave.dev/get | iex
```
Then follow the on-screen instructions.

{{< figure src="massgr.webp" link="massgr.webp" caption="MAS Terminal User Interface." align=center >}}


## [WizTree](https://www.diskanalyzer.com/) (non-free)

A free (as in beer) replacement to the old WinDirStat for Windows. This has been my go-to application to solve low disk space since I can know exactly what to delete by looking at the map. Random 50 GB file you didn't know existed? Gone. Random 10-year-old dump/log files? Also gone.

{{< figure src="wiztree.webp" link="wiztree.webp" caption="WizTree User Interface, with a tree layout of files and a storage map." align=center >}}

For Linux, there is an open source application that can do the same task called [QDirStat](https://github.com/shundhammer/qdirstat).


## [Everything by voidtools](https://www.voidtools.com/)

Insanely fast application to find specific files on your computer. Yes that's it.

{{< figure src="everything.webp" link="everything.webp" caption="Everything searches your files recursively on the whole device." align=center >}}


## [Windows patches by AME](https://ameliorated.io/)

Most legit Windows patcher I could find. Open source, with three separate "playbooks" to choose from. AME for privacy, AtlasOS for gaming or ReviOS for more general purpose stuff. Be aware that you should know what you're doing and how to fix your stuff (especially for AME playbook), since the modification scripts are more geared towards tech-savvy people.

{{< figure src="ame-wiz.webp" link="ame-wiz.webp" caption="AME Wizard with ReviOS 10." align=center >}}


## [Snappy Driver Installer](https://sdi-tool.org/)

Despite its old user interface, the windows drivers provided are very up-to-date (updated weekly). The driver themselves are hosted by the community via torrents, so pretty decent download speeds. If you decide to choose the full (35 GB) version, you can install drivers on pretty much any computer that has ever existed. Fully offline.

{{< figure src="sdi-ui.webp" link="sdi-ui.webp" caption="SDI User Interface." align=center >}}


# General purpose tools, cross-platform


## [Proton VPN](https://protonvpn.com/)

People use VPNs for a bunch of different reasons (I know what you're using it for). While there are millions of different free (of cost) VPNs our there, [most of them are malicious](https://www.bleepingcomputer.com/news/security/free-vpn-apps-on-google-play-turned-android-phones-into-proxies/). They turn your devices [into proxies](https://www.malwarebytes.com/blog/news/2024/04/free-vpn-apps-turn-android-phones-into-criminal-proxies), sells your data, filled with adwares, slow speed, etc.. But Proton VPN is different and reputable, as one of the most trusted VPNs out there. They have an extremely generous free tier (unlimited data usage!) in 5 different countries around the globe (enough for me at least) and their speed is also very fast for a free tier. What's the catch then? ~~Apparently, there's just none honestly (as far as I know of.)~~

P/S: I have recently found that you now can't specify which country to connect to if you're using the mobile version, and you can only use the fastest, closest server. Which, if you're trying to access content from a specific country, this won't be that exactly good.

{{< figure src="proton-gui.webp" link="proton-gui.webp" caption="Proton VPN GUI for PC." align=center >}}


## [NextDNS](https://nextdns.io/) (non-free) and [RethinkDNS](https://rethinkdns.com/)

Basically Pi-hole, with additional protections. Always online, (mostly) customizable blocklists ('mostly' because you can't upload your own blocklists, but the provided ones are probably more than enough). NextDNS has many features, such as: Analytics (total blocked, most blocked domains etc.. similar to Pi-hole)
, as well as Denylists and Allowlists as well as additional protection features,....

{{< figure src="nextdns-addi.webp" link="nextdns-addi.webp" caption="NextDNS main selling point, I guess?" align=center >}}

{{< figure src="nextdns-alytc.webp" link="nextdns-alytc.webp" caption="Analytics, similar to Pi-hole." align=center >}}

{{< figure src="nextdns-ls.webp" link="nextdns-ls.webp" caption="Support for blocklists." align=center >}}

Meanwhile, RethinkDNS offers less features (blocklist only) but has unlimited requests (as opposed to NextDNS' 300k per month) and the ability to [self-host your own instance](https://github.com/serverless-dns/serverless-dns), though I haven't got much success in doing so. RethinkDNS also gives you over 180 blocklists to choose from, so it's more than enough for the average Joe to use.

{{< figure src="rethnk-bl.webp" link="rethnk-bl.webp" caption="Categories for RethinkDNS blocklists." align=center >}}


## [Brave Browser](https://brave.com/) and [Cromite](https://github.com/uazo/cromite)

Brave Browser is the more popular and widely-recognized one, with built-in ads and trackers blocking, being faster than chrome, good privacy and security practices. They got their own Brave Search engine (albeit slow and not accurate depending on regions). What I don't actually like is Brave Wallet, Brave Talk and Brave Rewards, along with other Web3 bullshits and ads, but they're fully toggleable via brave://flags so it's somewhat fine.

Cromite is the fork version of the discontinued Bromite project. It also offers security enhancements and anti-fingerprinting stuff, while also being way less bloated than Brave (without the rewards and web3). I mainly use this browser on Android.

Both of these browsers are based on Chromium, so they are pretty much Chrome, without the spyware.


# For Media/Applications


## [YouTube ReVanced](https://revanced.app/)

This needs no introduction. YouTube Vanced's successor for Android. No ads, fully customizable, SponsorBlock, you name it. Best YouTube patch out there. Another one is [NewPipe](https://newpipe.net/), but I don't like its different UI and no logins (for which I actually want otherwise YouTube just gives me random boomer videos.) I also have made a [script](https://github.com/dng-nguyn/auto-revanced-linux) which helps with automating patching of YouTube on Linux, so it's much more convenient. (the script is half broken lol). A more approachable method is using the official [ReVanced Manager](https://github.com/ReVanced/revanced-manager), which also helps you find suitable patches. ReVanced also has support for other applications as well, not only YouTube.

{{< figure src="revanced-mgr.webp" link="revanced-mgr.webp" caption="ReVanced Manager User interface." align=center >}}


## [SpotX](https://github.com/SpotX-Official/SpotX) and [xManager](https://www.xmanagerapp.com/)

{{< figure src="xmanager.webp" link="xmanager.webp" caption="Image I found on google search" align=center >}}

Both spotify patches that give you Premium. SpotX is for all desktop OSes, while xManager is for Android. I actually haven't used Spotify for quite a while now since there are lots of music which I like that isn't available on there. Additionally, Spotify has become more and more greedy, as they [now charge you for lyrics](https://www.androidauthority.com/spotify-premium-lyrics-3439096/), which is quite awful. I have stuck with YouTube Music, since, well, it's YouTube. There are no songs that aren't available there - no region locks, no copyrighted bs, etc..


## [Seal](https://github.com/JunkFood02/Seal) and [Cobalt](https://cobalt.tools/)

Best media downloader out there. Many settings, SponsorBlock, multi-platforms (yt-dlp, which is literally any media you can think of). Just press share, click Seal (Quick Download) and voilà! You're done! The interface is also extremely nice (subjective opinion) with Material Design 3.

{{< figure src="seal-custom.webp" link="seal-custom.webp" caption="Bunch of download options to choose from" height="650" align=center >}}

{{< figure src="seal-quickdl.webp" link="seal-quickdl.webp" caption="Realllly convenient quick download tool - two clicks away from saving your favorite memes and videos." height="650" align=center >}}

For desktop/web clients, Cobalt is really good as well. It is the best web interface for downloading videos on the web compared to other sites, has very high resolution (8K!) support, [self-hostable](https://github.com/wukko/cobalt) and they also provide an API. Not as convenient as Seal on Android devices though.

{{< figure src="cobalt-settings.webp" link="cobalt-settings.webp" caption="Cobalt also allows for choosing sizes and formats." align=center >}}


## [Vencord](https://vencord.dev/)

Custom modifications for the Discord client. Apparently this breaks the ToS, but Discord hasn't bothered to ban anyone for it (I assume so) (Disclaimer: I'm not responsible for your account getting banned.) Support for plugins, streaming in native resolution, fake emojis, custom themes.. and more

{{< figure src="venc-inst.webp" link="venc-inst.webp" caption="Vencord Installation GUI." align=center >}}

{{< figure src="venc-plugins.webp" link="venc-plugins.webp" caption="A bunch of QoL and 'not legal' plugins" align=center >}}

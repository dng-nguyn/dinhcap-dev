---
title: 'Exporting & Visualizing Xiaomi Air Purifier data with Prometheus & Grafana'
date: 2025-06-21T17:52:56+07:00
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
description: "i like charts" #CHANGE
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

I recently bought a Xiaomi Air Purifier (4 Lite) and it does not have historical data. So as any normal human would do I export its data and create the history myself.

I have made the exporter [here,](https://github.com/dng-nguyn/miio-airp-exporter) together with a Grafana dashboard.

# What can we monitor?

This is a list of things we can monitor from the device:

- Environment:
	- Air Quality Index (PM2.5 Concentration)
	- Temperature
	- Relative Humidity
- Device Status:
	- Power Status ON/OFF State (Uptime)
	- Current Mode (Auto, Sleep, Manual)
	- Fan level (Manual mode only)
	- Fan RPM (Not shown in Xiaomi Home app)
	- Device Fault (Normal, Sensor Lost, Motor Stop)
- Filter Life:
	- % Filter Life Used
	- Filter Life Used (in days)
	- Filter Life Remaining (in days)

{{< figure src="grafana.webp" link="grafana.webp" caption="Grafana Dashboard" align=center >}}


# Prerequisites

Install `python-miio` and `prometheus-client`.

## Device IP & token

Xiaomi devices are authenticated with a 32-character hexadecimal token. 
Normally you can get this by signing into Xiaomi Cloud via `miiocli cloud` from the [python-miio](https://pypi.org/project/python-miio/) library. Unfortunately, Xiaomi has changed some things internally so we use [mik-laj's script](https://gist.github.com/mik-laj/97994a82cad049cd4b843edd9acb990b) to request login via QR Code.

If you don't see your device after successfully authenticating, it is possible that it was registered in a different region. You can try [my modified script](https://gist.github.com/dng-nguyn/cd3d7a0fe3c10856057f80f9f663039a) to find it across regions.

Device IP can be found with `miio discover` command.

## Device Model

If you want to use the exporter with any Xiaomi air purifier other than the 4 Lite, you may want to verify and adjust the script in accordance with the [Xiaomi's MIoT Specs.](https://home.miot-spec.com/)

The device model can be retrieved with `miiocli`command line:
```bash
miiocli device --ip <DEVICE_IP> --token <DEVICE_TOKEN> info
```
If the command line tool does not work, you can try this python script:

```py
from miio import Device
IP = "192.168.1.xxx"
TOKEN = "2c82ea271c375619b845c3d1f3e775f3"
try:
    device = Device(IP, TOKEN)
    info = device.info()
    print("Model:", info.model)
except Exception as e:
    print("Error:", e)
```
Modify the IP and Token accordingly.

Once that’s done, you’ll see your device’s identifier - for example, `zhimi.airp.rmb1` for the 4 Lite Air Purifier. You can then lookup [Xiaomi's MIoT Specs](https://home.miot-spec.com/) and modify the exporter script according to your device. Each element usually comes with its own SIID and PIID.

# Usage

[Navigate to the exporter script here.](https://github.com/dng-nguyn/miio-airp-exporter)

## Running the exporter
Execute the script: 
```sh
export.py JSON_FILE
```

JSON Configuration is as follow: 
```json
{
  "prometheus_port": 8000,
  "polling_interval_seconds": 60,
  "purifiers": [
    {
      "ip": "192.168.1.255",
      "token": "2c82ea271c375619b845c3d1f3e775f3",
      "name": "Bedroom"
     }
  ]
}
```

## Importing the dashboard


The dashboard is provided as a JSON file inside the repository [or on Grafana Labs](https://grafana.com/grafana/dashboards/23587-xiaomi-air-purifier-4-lite-exporter/) for easy importing. As previously mentioned, you may need to adjust settings appopriately for different devices.

And that's it! You now have a functional dashboard with Air Quality Monitoring, Temperature, Humidity and Filter Life all in one spot!
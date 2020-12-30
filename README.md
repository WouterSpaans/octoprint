# Install Octoprint on Debian

Hardware:
 - Intel NUC 10 Performance NUC10i3FNK (https://tweakers.net/pricewatch/1508342/intel-nuc-10-performance-nuc10i3fnk.html)

Software:
 - Ubuntu 20.04.1 (https://releases.ubuntu.com/20.04.1/ubuntu-20.04.1-live-server-amd64.iso)
 
Config:
 - Hostname = octoprint

Enable WiFi
```bash
sudo apt install network-manager
sudo nmcli d wifi connect [SSID] password [PASSWORD]
sudo nmcli d wifi
```

Tweaks:
 - 

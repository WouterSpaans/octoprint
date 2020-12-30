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

https://community.octoprint.org/t/setting-up-octoprint-on-a-raspberry-pi-running-raspbian-or-raspberry-pi-os/2337
Install prereq
```
cd ~
sudo apt update
sudo apt install python3-pip python3-dev python3-setuptools python3-venv git libyaml-dev build-essential
mkdir OctoPrint && cd OctoPrint
python3 -m venv venv
source venv/bin/activate
```

Install octoprint
```
pip install pip --upgrade
pip install octoprint
```

Configure stuff
```
sudo usermod -a -G tty wouter
sudo usermod -a -G dialout wouter
```

Logout / Login

Start first time
```
~/OctoPrint/venv/bin/octoprint serve
```

Auto start
```
wget https://github.com/OctoPrint/OctoPrint/raw/master/scripts/octoprint.service && sudo mv octoprint.service /etc/systemd/system/octoprint.service

sudo chmod +x /home/wouter/OctoPrint/venv/bin/octoprint

sudo nano /etc/systemd/system/octoprint.service
  User=wouter
  ExecStart=/home/wouter/OctoPrint/venv/bin/octoprint


sudo systemctl enable octoprint.service

```

```
sudo apt install haproxy
sudo nano /etc/haproxy/haproxy.cfg
```

```
global
        maxconn 4096
        user haproxy
        group haproxy
        daemon
        log 127.0.0.1 local0 debug

defaults
        log     global
        mode    http
        option  httplog
        option  dontlognull
        retries 3
        option redispatch
        option http-server-close
        option forwardfor
        maxconn 2000
        timeout connect 5s
        timeout client  15min
        timeout server  15min

frontend public
        bind :::80 v4v6
        use_backend webcam if { path_beg /webcam/ }
        default_backend octoprint

backend octoprint
        reqrep ^([^\ :]*)\ /(.*)     \1\ /\2
        option forwardfor
        server octoprint1 127.0.0.1:5000

backend webcam
        reqrep ^([^\ :]*)\ /webcam/(.*)     \1\ /\2
        server webcam1  127.0.0.1:8080
```

```
sudo nano /etc/default/haproxy
 ENABLED 1
sudo service haproxy start
```

```
sudo nano ~/.octoprint/config.yaml
server:
    host: 127.0.0.1
```


```
sudo nano /etc/sudoers.d/octoprint-shutdown
  wouter ALL=NOPASSWD: /sbin/shutdown
```


```
sudo reboot now
```
Tweaks:
 - 

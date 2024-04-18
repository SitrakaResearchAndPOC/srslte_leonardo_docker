# srslte_leonardo_docker
# INSTALLING CPU POWER
```
apt update
```
Installing CPUPOWER
```
apt install linux-tools-common
```
```
apt install linux-tools-generic
```
(installing also linux-tools-<number-proposed>-tools-generic by taping command cpupower)
```
cpupower frequency-set -g performance 
```


# INSTALLING WITH USRP
## installing docker and os ubuntu 20.04
```
apt-get install docker.io
```
```
docker image pull ubuntu:20.04
```
```
docker images
```
or
```
docker image ps
```
## run docker
```
docker run -it --privileged -v /dev/bus/usb:/dev/bus/usb --net=host ubuntu:20.04 /bin/bash
```
```
apt-get install iptables
```
## installing srslte and sim programmer over ubuntu

```
wget https://raw.githubusercontent.com/SitrakaResearchAndPOC/srslte_leonardo/main/srsepc_if_masq.sh
```
```
chmod +x srsepc_if_masq.sh
```


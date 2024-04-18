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
# INSTALLING NETTOOLS
```
apt update
```
```
apt-get install net-tools
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
apt update
```
```
apt-get install build-essential cmake libfftw3-dev libmbedtls-dev libboost-program-options-dev libconfig++-dev libsctp-dev  
```
```
apt install python3-pip
```
```
apt install git
```
```
pip3 install mako
```
```
apt install git cmake g++ libboost-all-dev libgmp-dev swig python3-numpy python3-mako python3-sphinx python3-lxml doxygen libfftw3-dev libsdl1.2-dev libgsl-dev libqwt-qt5-dev libqt5opengl5-dev python3-pyqt5 liblog4cpp5-dev libzmq3-dev python3-yaml python3-click python3-click-plugins python3-zmq python3-scipy python3-gi python3-gi-cairo gobject-introspection gir1.2-gtk-3.0 build-essential libusb-1.0-0-dev python3-docutils python3-setuptools python3-ruamel.yaml python-is-python3
```
```
apt install libtinfo5 libncurses5
```
```
apt-get  install curl wget zip net-tools
```


## Concifugring srsepc_if_masq
```
wget https://raw.githubusercontent.com/SitrakaResearchAndPOC/srslte_leonardo/main/srsepc_if_masq.sh
```
```
chmod +x srsepc_if_masq.sh
```


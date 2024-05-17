# srslte_leonardo_docker
# INSTALLING CPU POWER
```
apt update
```
Installing CPUPOWER
```
apt-get install linux-tools-common
```
```
apt-get install linux-tools-generic
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
apt install net-tools
```

# INSTALLING WITH USRP
## installing docker and os ubuntu 20.04
```
apt install docker.io
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
docker rm leonardousrp
```
```
docker run -itd --privileged -v /dev/bus/usb:/dev/bus/usb -v /tmp/.X11-unix:/tmp/.X11-unix:ro -v $XAUTHORITY:/home/user/.Xauthority:ro --net=host --env="DISPLAY=$DISPLAY" --env="LC_ALL=C.UTF-8" --env="LANG=C.UTF-8"  --name leonardousrp ubuntu:20.04 
```
```
docker restart leonardousrp
```
## installing srslte and sim programmer over ubuntu
```
xhost +
```
```
docker exec -ti leonardousrp bash
```
```
apt update
```
```
apt install net-tools zsh wireshark iptables nano inetutils-ping
```
```
apt install build-essential cmake libfftw3-dev libmbedtls-dev libboost-program-options-dev libconfig++-dev libsctp-dev  
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
apt  install curl wget zip net-tools
```
## Installing UHD
```
git clone -b "UHD-4.0" https://github.com/EttusResearch/uhd
```
```
cd uhd/host/
```
```
mkdir build
```
```
cd build
```
```
cmake ..
```
```
make -j$(nproc --ignore=1)
```
```
make install
```
```
ldconfig
```
```
cd ../../..
```
```
uhd_images_downloader
```

## Installing opencells simcard
```
curl https://open-cells.com/d5138782a8739209ec5760865b1e53b0/uicc-v3.2.tgz > uicc-v3.2.tgz
```
or using : 
```
wget https://open-cells.com/d5138782a8739209ec5760865b1e53b0/uicc-v3.2.tgz
```
Compiling  : 
```
tar -xvf uicc-v3.2.tgz
```
```
cd uicc-v3.2
```
```
make clean
```
```
make 
```
```
./program_uicc --help
```
```
cd ..
```

## Installing srsRAN
```
curl -L https://github.com/srsran/srsRAN_4G/archive/refs/tags/release_23_04.tar.gz > srsRAN_23_04.tar.gz
```
```
tar -xvf srsRAN_23_04.tar.gz
```
```
cd srsRAN_4G-release_23_04/
```
```
mkdir build
```
```
cd build
```
```
cmake ..
```
```
make -j$(nproc --ignore=1)
```
```
make test
```
```
make install
```
```
ldconfig
```
```
cd ../..
```
## On linux : Programming SIMCARD
```
cd uicc-v3.2
```
Please replace xxxxxxxx by 0c010955 </br>
RQ : More information about parmeters of program_uicc please check this [github](https://github.com/SitrakaResearchAndPOC/fork_program_uicc) and [youtube](https://www.youtube.com/watch?v=nfTzATOZd_s)
```
sudo ./program_uicc --adm xxxxxxxx --imsi 208920100001101 --isdn 00000001 --acc 0001 --key 6874736969202073796d4b2079650a73 --opc 504f20634f6320504f50206363500a4f -spn "OpenCells01" --authenticate 
```
Configure the apn of phone as : OpenCells01
## Creating the configuration
```
mkdir -p /root/.config
```
```
mkdir -p /root/.config/srsran
```
```
rm -rf /root/.config/srsran/*
```
For USRP : config
```
wget https://raw.githubusercontent.com/SitrakaResearchAndPOC/srslte_leonardo/main/srsran_config.zip
```
```
unzip srsran_config.zip
```
```
cp srsran/* /root/.config/srsran
```
```
nano /root/.config/srsran/epc.conf
```
Change [pcap] enable by true
 
## Concifugring srsepc_if_masq
```
wget https://raw.githubusercontent.com/SitrakaResearchAndPOC/srslte_leonardo/main/srsepc_if_masq.sh
```
```
chmod +x srsepc_if_masq.sh
```
Test
```
./srsepc_if_masq.sh
```
or test
```
bash srsepc_if_masq.sh
```
```
exit
```

# launching SRSLTE
close all terminal and open new fresh one
* On terminal 1
running cpupower
```
cpupower frequency-set -g performance 
```
```
docker exec -ti leonardousrp srsepc
```
* On terminal 2
(tape ctrl+shift+T before)
```
cpupower frequency-set -g performance 
```
```
docker exec -ti leonardousrp uhd_usrp_probe
```
```
docker exec -ti leonardousrp srsenb
```
* On terminal 3
(tape ctrl+shift+T before)
```
cpupower frequency-set -g performance 
```
```
docker exec -ti leonardousrp ifconfig
```
Get the interface having internet as <if_internet>
```
docker exec -ti leonardousrp  bash srsepc_if_masq.sh <internet>
```
```
docker exec -ti leonardousrp ufw disable # don't wory if there is not ufw on your system
```
```
docker exec -ti leonardousrp sysctl -w net.ipv4.ip_forward=1
```
```
xhost + 
```
```
docker exec -ti leonardousrp wireshark
```


# loading .tar.gz for others machine
```
docker save > leonardousrp.tar.gz
```

## saving image (command on new terminal not on docker)
Tape ctrl+shit+T
```
sudo su
```
```
docker ps
```
find the id 
```
docker commit  <id> leonardousrp
```
```
docker save leonardousrp > leonardousrp.tar.gz
```


# INSTALLING WITH BLADERF
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
docker rm leonardobladerf
```
```
docker run -itd --privileged -v /dev/bus/usb:/dev/bus/usb -v /tmp/.X11-unix:/tmp/.X11-unix:ro -v $XAUTHORITY:/home/user/.Xauthority:ro --net=host --env="DISPLAY=$DISPLAY" --env="LC_ALL=C.UTF-8" --env="LANG=C.UTF-8"  --name leonardobladerf ubuntu:20.04 
```
```
docker restart leonardobladerf
```
```
apt install nano wireshark
```

```
apt install iptables
```
## installing srslte and sim programmer over ubuntu
```
apt update
```
```
apt install build-essential cmake libfftw3-dev libmbedtls-dev libboost-program-options-dev libconfig++-dev libsctp-dev  
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
apt  install curl wget zip net-tools
```
## Installing BladeRF Driver
```
git clone https://github.com/Nuand/bladeRF.git
```
```
cd bladeRF
```
```
mkdir build
```
```
cd build
```
```
cmake ../
```
```
make
```
```
sudo make install
```
```
sudo ldconfig
```
```
cd ../..
```
## Installing opencells simcard
```
curl https://open-cells.com/d5138782a8739209ec5760865b1e53b0/uicc-v3.2.tgz > uicc-v3.2.tgz
```
or using : 
```
wget https://open-cells.com/d5138782a8739209ec5760865b1e53b0/uicc-v3.2.tgz
```
Compiling  : 
```
tar -xvf uicc-v3.2.tgz
```
```
cd uicc-v3.2
```
```
make clean
```
```
make 
```
```
./program_uicc --help
```
```
cd ..
```

## Installing srsRAN
```
curl -L https://github.com/srsran/srsRAN_4G/archive/refs/tags/release_23_04.tar.gz > srsRAN_23_04.tar.gz
```
```
tar -xvf srsRAN_23_04.tar.gz
```
```
cd srsRAN_4G-release_23_04/
```
```
mkdir build
```
```
cd build
```
```
cmake ..
```
```
make -j$(nproc --ignore=1)
```
```
make test
```
```
make install
```
```
ldconfig
```
```
cd ../..
```
## On linux : Programming SIMCARD
```
cd uicc-v3.2
```
Please replace xxxxxxxx by 0c010955 </br>
RQ : More information about parmeters of program_uicc please check this [github](https://github.com/SitrakaResearchAndPOC/fork_program_uicc) and [youtube](https://www.youtube.com/watch?v=nfTzATOZd_s)
```
sudo ./program_uicc --adm xxxxxxxx --imsi 208920100001101 --isdn 00000001 --acc 0001 --key 6874736969202073796d4b2079650a73 --opc 504f20634f6320504f50206363500a4f -spn "OpenCells01" --authenticate 
```
Configure the apn of phone as : OpenCells01
## Creating the configuration
```
mkdir /root/.config
```
```
mkdir /root/.config/srsran
```
```
rm -rf /root/.config/srsran/*
```
For BLADERF : config
```
wget https://raw.githubusercontent.com/SitrakaResearchAndPOC/srslte_leonardo/main/srsran_config_bladerf.zip
```
```
unzip srsran_config_bladerf.zip
```
```
cp srsran/* /root/.config/srsran
```
Change [pcap] enable by true

## Concifugring srsepc_if_masq
```
wget https://raw.githubusercontent.com/SitrakaResearchAndPOC/srslte_leonardo/main/srsepc_if_masq.sh
```
```
chmod +x srsepc_if_masq.sh
```
Test
```
./srsepc_if_masq.sh
```
or test
```
bash srsepc_if_masq.sh
```
## Getting firmeware for flashing bladerf
```
wget https://www.nuand.com/fpga/hostedxA4-latest.rbf
```
```
bladeRF-cli -l hostedxA4-latest.rbf
```

## saving image (command on new terminal not on docker)
Tape ctrl+shit+T
```
sudo su
```
```
docker ps
```
find the id 
```
docker commit  <id> leonardobladerf
```
```
docker save leonardobladerf > leonardobladerf.tar.gz
```
# launching SRSLTE
close all terminal and open new fresh one 
* On terminal 1
Use cpupower mode 
```
cpupower frequency-set -g performance 
```
running
```
docker exec -ti leonardobladerf  bladeRF-cli -i
```
Tape info 
```
info
```
And Tape exit or ctrl-D
```
docker exec -ti  leonardobladerf srsepc
```
* On terminal 2
(tape ctrl+shift+T before)
```
cpupower frequency-set -g performance 
```
```
docker exec -ti leonardobladerf bladeRF-cli -p
```
```
docker exec -ti leonardobladerf srsenb
```
If not load, please reload firemware on bladerf : 
```
docker exec -ti leonardobladerf bladeRF-cli -l hostedxA4-latest.rbf
```
* On terminal 3
(tape ctrl+shift+T before) 
```
sudo cpupower frequency-set -g performance
```
```
docker exec -ti leonardobladerf ifconfig
```
Get the interface having internet as <if_internet>
```
docker exec -ti leonardobladerf  bash srsepc_if_masq.sh <if_internet>
```
```
docker exec -ti leonardobladerf ufw disable # don't wory if there is not ufw on your system
```
```
docker exec -ti leonardobladerf sysctl -w net.ipv4.ip_forward=1
```
```
xhost + 
```
docker exec -ti leonardobladerf wireshark
```


# loading .tar.gz for others machine
```
docker save >  leonardobladerf.tar.gz
```

RQ : Error time burst </br>
* The USB doesn't detect USB3.0 </br>
* The processing is no enough </br>
* The configuration is bad indeed about device argument and references clock of equipement </br>

# Link
* [uhd](https://www.linkedin.com/pulse/installation-uhd-40-gnu-radio-38-rfnoc-ubuntu-2004-murthy-s)
* [leonardo](https://blog.leonardotamiano.xyz/tech/build-your-own-4g-network/)
* [hackmd](https://hackmd.io/@4G7xxurNQEGA3Apb11YQJg/H1GJsHt9o)
* [cyberloginit](https://cyberloginit.com/2018/05/03/build-a-lte-network-with-srslte-and-program-your-own-usim-card.html)
* [ilabt](https://doc.ilabt.imec.be/ilabt/wilab/tutorials/usrp_b200.html)
* [tenettech](https://blogspot.tenettech.net/2019/10/17/validation-of-4g-lte-using-software-defined-radio/)
* [zhixun-wireless](http://zhixun-wireless.top/install-and-configure-srslte-enb-epc-on-ubuntu)
* [nickvsnetworking](https://nickvsnetworking.com/srslte-install-for-bladerf-limesdr-on-debian-ubuntu/)
* [programmersought](https://programmersought.com/article/84496400694/)
* [radioactive](https://band.radio/srsLTE)
* [adamtosher](https://adam-toscher.medium.com/how-to-create-an-evil-lte-twin-34b0a9ce193b)
* [matlab_hash_used](https://github.com/paulchen2713/SHA3)
* [matlab_hash_ref](https://www.mathworks.com/matlabcentral/fileexchange/31795-sha-algorithms-160-224-256-384-512)	


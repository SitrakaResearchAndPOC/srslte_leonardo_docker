# srslte_leonardo_docker
# Installing on host machine cpupower
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


# Installing with config and first test srslte over usrp  after over bladerf
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


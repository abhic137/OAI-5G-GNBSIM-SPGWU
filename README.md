# 5GC-GNBSIM-SPGWU
## Download the 5G core repo
```
git clone https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-fed.git

```
## Build the Gnbsim image OR Pull the image

Pulling
```
docker pull rohankharade/gnbsim
docker image tag rohankharade/gnbsim:latest gnbsim:latest

```
Building
```
cd
git clone https://gitlab.eurecom.fr/kharade/gnbsim.git
cd gnbsim
docker build --tag gnbsim:latest --target gnbsim --file docker/Dockerfile.ubuntu.22.04 .

```
## Running the core 
```
cd oai-cn5g-fed/docker-compose
sudo docker compose -f docker-compose-basic-nrf.yaml up -d
sudo docker ps -a
sudo docker logs --follow oai-amf
```
## Running the Gnbsim
```
cd oai-cn5g-fed/docker-compose
sudo docker-compose -f docker-compose-gnbsim.yaml up -d gnbsim
sudo docker ps -a

```
Now you can check in the AMF logs that a GNB and a UE are connected to the core.
## Ping test
you can check the ip of ue in the smf logs
```
sudo docker logs oai-smf | grep -o '.*UE IPv4 Address.*' | grep 'UE IPv4 Address'

```
```
docker exec oai-ext-dn ping -c 3 12.1.1.2 #Here we ping UE from external DN container.
docker exec gnbsim ping -c 3 -I 12.1.1.2 google.com #Here we ping external DN from UE (gnbsim) container.

```
## Iperf test
```
sudo docker exec -it oai-ext-dn iperf3 -s   #server
sudo docker exec -it gnbsim iperf3 -c 192.168.70.135 -B 12.1.1.2 #client
```


refrence: https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-fed/-/blob/master/docs/DEPLOY_SA5G_MINI_WITH_GNBSIM.md

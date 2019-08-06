# Ansible Lab
```
Demonstrating a very simple setup to learn ansible
A quick spinup of 4 docker containers on top of Ubuntu Trusty OS
Relied on Virtual BOX & vagrant to build Ubuntu Trusty image and docker on top of that 
```
## Prerequesties
### Install and Configure Virtual box and vagrant 
[Vagrant Install](https://www.vagrantup.com/downloads.html) .

[Virtual BOX](https://www.virtualbox.org/wiki/Downloads) .
#### Bring up Vagrant box 
```
mkdir ansible-lab
cd ansible-lab
vagrant init
Edit Vagrant file and add "ubuntu/trusty64" on config.vm.box as below 
config.vm.box = "ubuntu/trusty64"
#### uncomment below line to set the new private network ####
config.vm.network "private_network", ip: "192.168.33.10"
vagrant up 
```
### Install and Configure docker and docker-compose to spin up containers
#### Connect to vagrant box
```
vagrant ssh
apt-get update
vi /etc/resolv.conf and add nameserver 8.8.8.8 before existing name server entry 
```
#### Docker installation
```
apt-get install     apt-transport-https     ca-certificates     curl     software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
add-apt-repository    "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
     $(lsb_release -cs) \
     stable"
apt-get update
apt-get install docker-ce
docker run hello-world   - This will not work because of very updated version of docker when we install , downgrade to lower version and try 
apt-get install docker-ce=17.12.0~ce-0~ubuntu
``` 
#### docker-compose installation
```
sudo curl -L "https://github.com/docker/compose/releases/download/1.24.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
ln -s /usr/bin/docker-compose /usr/local/bin/docker-compose
chmod 755 /usr/local/bin/docker-compose
```
#### Bring the setup live and accessible
```
cd /root
git clone https://github.com/vv2812/ansible-study.git
cd ansible-study
tar -xzf ansible-study-vv.tar.gz
```
#### Build the containers based on the config details mentioned in docker-compose.yml
```
docker-compose up -d --build
```
##### Once complete verify containers are spawned_
```
docker ps -a
````
#### Connect to master01 to make sure passwordless authentication works from master01 to host01 host02 and host03
```
docker exec -it master01 bash
```
##### Verify basic ansible commands works
```
cat /etc/ansible/inventory - Three category of hosts are mentioned in A , B & C Geographical location
ansible -i /etc/ansible/inventory all --list-hosts
ansible -i /etc/ansible/inventory -m ping all
```
#### Cleanup the setup
#### Bring down and remove the container
```
docker rm -f $(docker ps -aq)
```
#### Delete volume
```
docker volume list
docker volume rm volume-name
````
#### Clean the images from local repository
docker rmi -f $(docker images -aq)

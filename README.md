Ansible Lab

#Introduction

Demonstrating a very simple setup to learn ansible
A quick spinup of 4 docker containers on top of Ubuntu Trusty OS
Relied on Virtual BOX & vagrant to build Ubuntu Trusty image and docker on top of that 

#Prerequesties

Install Virtual box and vagrant 

https://www.vagrantup.com/downloads.html

https://www.virtualbox.org/wiki/Downloads


#"ubuntu/trusty64" vagrant box bringup

mkdir ansible-lab
cd ansible-lab
vagrant init

Edit Vagrant file and add "ubuntu/trusty64" on config.vm.box as below 

config.vm.box = "ubuntu/trusty64"

vagrant up 

#Setting up docker and docker-compose to spin up containers

vagrant ssh
apt-get update


#Docker installation
sudo apt-get install     apt-transport-https     ca-certificates     curl     software-properties-common

Add Docker’s official GPG key:

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

Verify the gpg key with finger print

sudo apt-key fingerprint 0EBFCD88


pub   4096R/0EBFCD88 2017-02-22
      Key fingerprint = 9DC8 5822 9FC7 DD38 854A  E2D8 8D81 803C 0EBF CD88
uid                  Docker Release (CE deb) <docker@docker.com>
sub   4096R/F273FCD8 2017-02-22



add-apt-repository    "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
     $(lsb_release -cs) \
     stable"

apt-get update

apt-get install docker-ce

docker run hello-world   - This will not work because of very updated version of docker when we install , downgrade to lower version and try 

apt-get install docker-ce=17.12.0~ce-0~ubuntu


#docker-compose installation

sudo curl -L "https://github.com/docker/compose/releases/download/1.24.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

ln -s /usr/bin/docker-compose /usr/local/bin/docker-compose

chmod 755 /usr/local/bin/docker-compose

#Bring the setup live and accessible
#Clone the docker-compose.yml and other config files from git repo

cd /root

tar -xzf ansible-study-vv.tar.gz

cd ansible

#Build the containers based on the config details mentioned in docker-compose.yml

docker-compose up -d --build

#Once complete verify containers are spawned 

docker ps -a

#Connect to master01 to make sure passwordless authentication works from master01 to host01 host02 and host03

docker exec -it master01 bash

From master01 try ssh host01 , 02 & 03 to make sure passwordless authentication works fine

#Verify basic ansible commands works

cat /etc/ansible/inventory - Three category of hosts are mentioned in A , B & C Geographical location

ansible -i /etc/ansible/inventory all --list-hosts

ansible -i /etc/ansible/inventory -m ping all

#Cleanup the setup

Bring down and clean the container

docker rm -f $(docker ps -aq)

Delete volume

docker volume list
docker volume rm volume-name

Clean the images from local repository

docker rmi -f $(docker images -aq)

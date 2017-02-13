---
layout: post
title: "GPU-enabled Tensorflow in Docker on AWS"
date: 2017-02-13 02:22:00 +0000
short_description: "A very quick guide and setup script."
image_preview: https://secure.gravatar.com/avatar/26da7b36ff8bb5db4211400358dc7c4e.jpg?s=512&r=g&d=mm
tags: [python, tensorflow, docker, gpu]
---
I might elaborate on this later, but at the moment this is a barebones script for getting Nvidia-Docker up and running on an Ubuntu 14.04 AWS instance (I used a spot g2.xlarge in Oregon) to then run the Tensorflow-GPU docker image. It puts into one place the commands you have to follow which are given separately (with more commentary) at the following links:

* [Installing Docker engine on Ubuntu](https://docs.docker.com/engine/installation/linux/ubuntu/)

* [Using Docker without sudo](https://docs.docker.com/engine/installation/linux/linux-postinstall/)
  * Tested on both Ubuntu and the basic Amazon Linux AMI

* [Installing nvidia-docker](https://github.com/NVIDIA/nvidia-docker)
  * Need nvidia-docker to be able to use the GPU
  * Installation of the nvidia drivers requires supervision and interaction by the user, unlike most other steps in the script---I found default settings worked fine, though it was pointed out to me that if you want to use X to access the AWS instance graphically, you may want to let Nvidia overwrite X where it asks to
  * gcc is required, so install `build-essential`; apparently `nvidia-modprobe` is also necessary but I didn't try it without so don't know (a) if it's necessary, and (b) what error messages might arise without it

* [Pulling the Tensorflow-GPU docker image](https://github.com/tensorflow/tensorflow/tree/master/tensorflow/tools/docker)

* The final step launches a Jupyter notebook server that you can access by substituting in the Public IP address (see AWS instance details panel) into the notebook location that the terminal output shows

* An alternative is to link in to a bash terminal within the Docker container, where you can run python scripts without using Notebook, etc. but will leave writing about that to another day

***

## The script

{% highlight bash %}

# Ubuntu 14.04
# Note python is 2.7 by default

sudo apt-get -y update
sudo apt-get -y upgrade

# INSTALL DOCKER ENGINE
# https://docs.docker.com/engine/installation/linux/ubuntu/
sudo apt-get -y install curl linux-image-extra-$(uname -r) linux-image-extra-virtual
sudo apt-get -y install apt-transport-https ca-certificates
curl -fsSL https://yum.dockerproject.org/gpg | sudo apt-key add -
sudo apt-get -y install software-properties-common
sudo add-apt-repository \
       "deb https://apt.dockerproject.org/repo/ \
       ubuntu-$(lsb_release -cs) \
       main"
sudo apt-get -y update
sudo apt-get -y install docker-engine
# Optional: testing
# sudo docker run hello-world  

# ALLOW NON-SUDO DOCKER
# https://docs.docker.com/engine/installation/linux/linux-postinstall/
sudo groupadd docker
sudo usermod -aG docker $USER
# Log out and log back in (i.e. exit, then ssh back in)
# testing, again:
# docker run hello-world  

# INSTALL NVIDIA DOCKER and PREREQs
# https://github.com/NVIDIA/nvidia-docker
sudo apt-get -y install build-essential  # need gcc
sudo apt-get -y install nvidia-modprobe  # not sure this is essential
wget -P /tmp http://us.download.nvidia.com/XFree86/Linux-x86_64/367.57/NVIDIA-Linux-x86_64-367.57.run
# when running the nvidia install, default settings fine
sudo sh /tmp/NVIDIA-Linux-x86_64-367.57.run
rm /tmp/NVIDIA-Linux-x86_64-367.57.run
# Install nvidia-docker and nvidia-docker-plugin
wget -P /tmp https://github.com/NVIDIA/nvidia-docker/releases/download/v1.0.0/nvidia-docker_1.0.0-1_amd64.deb
sudo dpkg -i /tmp/nvidia-docker*.deb && rm /tmp/nvidia-docker*.deb
# Test nvidia-smi
nvidia-docker run --rm nvidia/cuda nvidia-smi

# PULL TENSORFLOW DOCKER IMG
# https://github.com/tensorflow/tensorflow/tree/master/tensorflow/tools/docker
docker pull gcr.io/tensorflow/tensorflow:latest-gpu

# Launch notebook:
nvidia-docker run -it -p 8888:8888 gcr.io/tensorflow/tensorflow:latest-gpu

{% endhighlight %}

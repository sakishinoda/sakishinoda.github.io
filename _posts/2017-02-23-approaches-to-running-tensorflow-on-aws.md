---
layout: post
title: Approaches to running Tensorflow on AWS
date: 2017-02-23 13:09:00 +0000
short_description: What are the options when it comes to setting up Tensorflow on AWS?
image_preview: https://secure.gravatar.com/avatar/26da7b36ff8bb5db4211400358dc7c4e.jpg?s=512&r=g&d=mm
tags: [python, tensorflow, docker, gpu]

---
_Work in progress_

## Contents
{:.no_toc}
* TOC
{:toc}

## Instance types, regions, and pricing

### [Instance types](https://aws.amazon.com/ec2/instance-types/)
GPU instances most appropriate: g2.2xlarge and p2.xlarge.
g2.2xlarge uses Nvidia GRID K520, "each with 1,536 CUDA cores and 4GB of video memory". p2.xlarge uses NVIDIA K80 GPUs, "each with 2,496 parallel processing cores and 12GiB of GPU memory".

### Regions and pricing
Pricing is a function of the instance type and what region you run your instance in. Not all instances are available in all regions. I use US East (N. Virginia) and US West (Oregon).

* [On-demand pricing](https://aws.amazon.com/ec2/pricing/on-demand/)
* [Spot pricing](https://aws.amazon.com/ec2/spot/pricing/)

### On-demand vs. spot instances
TLDR; If you have an AMI you are happy with and you don't mind mounting disks when launching new instances, and you don't mind not being able to temporarily STOP instances (as opposed to terminate), spot instances are not scary and are very cheap!

* Need to request limits for on-demand instances, which can take a few days
* Spot instances require a little more care to make sure your data doesn't get lost, BUT so much cheaper
* Spot instance cost varies not just by region but also availability zone by region and time of day

My personal workaround to use spot instances:

* Launch spot instances from an AMI
* Mount a persistent EBS drive
* Format and use git to pull all necessary data and code onto it the first time it's used, using:
	* [Formatting and mounting the first time](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-using-volumes.html)
	* [Attaching the volume in general](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-attaching-volume.html)
	* Git LFS
	* Git Personal Access Tokens
* Save models and data frequently to this drive

## Installation types
TLDR;

* [Install from scratch using Nvidia instructions](http://www.nvidia.com/object/gpu-accelerated-applications-tensorflow-installation.html)
* [Install with Nvidia-Docker and the Tensorflow-GPU image](https://github.com/NVIDIA/nvidia-docker/wiki/Deploy-on-Amazon-EC2) (or also see [my post]([2017/02/13/gpu-enabled-tensorflow-in-docker-on-aws.html]))
* Install with a prebuilt AMI like [AWS Deep Learning AMI](https://aws.amazon.com/blogs/ai/the-aws-deep-learning-ami-now-with-ubuntu/) (available in standard CentOS-like Amazon Linux and Ubuntu)

### Docker 
The main argument to use Docker, in my view, is to not have to:

* sign up for Nvidia accelerated developer in order to download cuDNN
* install Bazel so you can build Tensorflow from source
* build Tensorflow from source

Of course, Nvidia handily provide step-by-step instructions for both alternatives:

* [With Docker](https://github.com/NVIDIA/nvidia-docker/wiki/Deploy-on-Amazon-EC2)
* [Without Docker](http://www.nvidia.com/object/gpu-accelerated-applications-tensorflow-installation.html)


### Prebuilt AMI's
I have tried:

* [AWS Deep Learning AMI](https://aws.amazon.com/blogs/ai/the-aws-deep-learning-ami-now-with-ubuntu/) (very big, requests 50GB root drive, but FREE): 

I have not tried:

* [Bitfusion Ubuntu 14 Tensorflow](https://aws.amazon.com/marketplace/pp/B01EYKBEQ0) (costs money)
* [Open and free TF AMI by Ritchie Ng (NUS)](https://github.com/ritchieng/tensorflow-aws-ami) (available in lots of regions, free, works on P2 and G2 apparently)


### From scratch
I haven't done [this]((http://www.nvidia.com/object/gpu-accelerated-applications-tensorflow-installation.html)) yet but intend to do so soon.

## Miscellaneous

### Security groups on AWS

* To enable Jupyter Notebook access, need an Inbound Custom TCP rule for port 8888. [More on how to secure a remote server Jupyter notebook](http://jupyter-notebook.readthedocs.io/en/latest/public_server.html)
* To enable Tensorboard access, need an Inbound Custom TCP rule for port 6006

### SSH from iPhone

* Termius supports *.pem certificates
* Termius also supports port forwarding (e.g. for Tensorboard viewing)

### Links

* [Someone's tutorial on setting up Keras on AWS](http://machinelearningmastery.com/develop-evaluate-large-deep-learning-models-keras-amazon-web-services/)
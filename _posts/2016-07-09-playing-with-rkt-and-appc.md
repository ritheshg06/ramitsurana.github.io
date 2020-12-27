---
layout: post
title: "Playing with rkt and appc"
date: 2016-07-09
author: Ramit Surana
tags: [rkt aci docker]
category: [coreos, rkt, containers]
description: "Exploring the rkt container ecosystem"
comments: true
---

Evolution is an ongoing process. In the containers spectrum it has been a regular custom to experiment with new stuff and make it better with time. Saying so one of the coolest evolving projects is the [rkt][8] project by [Coreos][11] guys. It is a substitute of the famous [docker][6] container project and uses different methodology than the [docker][6] project. So, I began exploring and found out that its a really interesting approach on building containers.

In this post I have shared some of my findings below:


##  About rkt

[rkt][8] is an awesome project which started way back with a minor conflict of opinions on containers between the [Docker][6] guys and [CoreOS][11] guys. It is designed for security, simplicity, and composability within modern cluster architectures. [rkt][8] implements a modern, open, standard container format, the [App Container (appc)][9], but can also execute other container images, like those created with [docker][6]. The most fundamental atomic unit in [rkt][8] is the pod, which is a group of related containers that share resources. One of the amazing things about [rkt][8] is that it is a single binary that integrates with init systems, scripts, and complex devops pipelines.  Containers take their correct place in the PID heirachy and can be managed with standard utilities.

![rkt-1 0-banner](https://cloud.githubusercontent.com/assets/8342133/16660420/ce24e2e0-448b-11e6-97b5-b56079d0f631.png)


## About appc

[appc][9] defines several independent but composable aspects involved in running application containers, including an image format, runtime environment, and discovery mechanism for application containers. It creates images known as ACI. ACI's full form is Application Container Image. You can think of it as a similar to [docker][6] Images.


[acbuild][10] is a simple unix utility for constructing ACI manifests and container filesystems.[acbuild][10] presents options for mapping ports, mounting filesystems, and specifying the base containers from which higher-level images are built — a dependency, in [acbuild][10] parlance.

Some of the features of ACI include:

* Composable
* Security
* Decentralization
* Open Source

![app-container-rkt-14-638](https://cloud.githubusercontent.com/assets/8342133/16660112/b66d8dc4-448a-11e6-916b-ff109cf64fe7.jpg)

# Building ACI Images

Building aci images is easy to use with an awesome [acbuild][10] project. Using this project you can easily create your own aci images. Its under heavy development process and might change its workflow in the future. By building aci images you can directly use them with rkt. Much as like you can use [docker][6] images with [docker][6].


Before starting to use the acibuild you need to follow these steps:

<script src="https://gist.github.com/ramitsurana/6421a4bfb3425a6c018bff85ffcae0d3.js"></script>

Instead of running multiple commands on the terminal. I have decide to use one of the simple techniques to build an aci image.
In this technique we use a shell script to automate the process of building an ACI. Using the shell script we can execute multiple [acbuild][10] commands. You can consider it as a similar way like using Dockerfile to build your [docker][6] images.

<script src="https://gist.github.com/ramitsurana/06f08da66dc9ec1c3a6299773bdaf4f0.js"></script>

## Building rkt containers

Here are the steps to follow:

<script src="https://gist.github.com/ramitsurana/0a1c8e9f4af1b01e35c035c9b519564c.js"></script>

## How it works ?

[rkt][8] works in multiple ways. Some of the most common ways include by using:

* Url of aci image,
* Using [docker][6] registry
* Using [quay.io][12] registry


But all of these include one common thing that is to use the aci images while building up the [rkt][8] containers. So I am going to discuss one of the most commonly used methods to build up [rkt][8] containers. In this method I am going to fetch a simple docker container from the [docker][6] registry and use it to build up a [rkt][8] container.

````
rkt run --interactive docker://ubuntu --insecure-options=image
````
In the above command, we use `--interactive` flag for using the STDIN and STDOUT devices from your kernel. The `--insecure-options=image` is used because some of the docker images don't have image signature verification. This flag skips the trouble of verifying image. Behind the scenes here's a depiction of what happens:

![rkt-work](https://cloud.githubusercontent.com/assets/8342133/16678420/85a9368a-44fc-11e6-9271-770ce896056c.png)


Basically what [rkt][8] does is simply pull the [docker][6] image from the registry.In every case you have to specify the registry using which you are pulling down the image. As per usage there are two popular options namely, [docker][6] registry and [quay.io][12]. After it fetches the image. A utility called as [docker2aci](https://github.com/appc/docker2aci) is used to convert these images to aci images. Then using this image the rkt container automatically brings up the [rkt][8] container.

## Garbage Collection

This is one of the features that I love the most about [rkt][8] containers. In this feature,[rkt][8] can easily clear out containers that have exited and are not in use. Truly saying I feel that this feature is of very high importance. The reason is that when you deploy a huge lot of containers in real time scenario,it becomes really hard to manage containers which have exited and which are running. This creates a true messy condition for the user.Using the [rkt][8] gc command,it can automatically reap exited pods and container images from the local store after a configurable grace period.

<script src="https://gist.github.com/ramitsurana/e22d35562383600e5b68d645cd7a2c52.js"></script>

## Switching from Docker to rkt

As a regular [Docker][6] user myself I find it hard to understand and use all the best of commands in [rkt][8].So
I have tried to summarize some of the commands that can be used while switching from [docker][6] to [rkt][8] :

Docker | rkt
-------- | --------
docker pull | rkt fetch
docker ps | rkt list
docker images | rkt image list
docker run -i -t | rkt run --interactive

Hope you enjoyed this post,please tell us your opinions in the comments section below.

  [6]: http://docker.com
  [7]: https://cloud.githubusercontent.com/assets/8342133/12071970/ed85ee72-b0ed-11e5-9a99-d4b0d8d8a36a.png
  [8]: http://coreos.com/rkt
  [9]: http://github.com/appc/spec
  [10]: https://github.com/appc/acbuild
  [11]: http://coreos.com
  [12]: http://quay.io


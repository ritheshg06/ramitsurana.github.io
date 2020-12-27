---
layout: post
title: "Containers in Production"
date: 2017-08-24
author: Ramit Surana
tags: containers tips openshift
category: Introduction
excerpt: "Containers usage tips and tricks."
---

Hello World ! Today our industry is surrounded by Containers.Every other company wants to build something on top of the containers.The containers have taken a rapid replacement with virtualization.There are several initiatives that are being taken by new age startups and guys who are enthusiastic about the idea.

So,in this rather small yet interesting blog post I plan to uncover some tips and tricks that people in the container industry are using to make sure they stay ahead of the new age revolution.Also in the later part of this tutorial we are going to see a really cool way to deploy docker containers in production.So let’s get started :

## Tips & Tricks

### Infrastructure as a code

Imagine setting up a hundred microservices that we want to run and each having it’s own requirements.What would be the time taken for such a deployment ? Days,months.In order to prevent such time loss & complexity we have the concept of Infrastructure as a Code.The idea is to build and setup one template using which we can easily create large variety of services.For this purpose we can use some of the tools such as [Terraform](http://terraform.io/), [CloudFormation](https://aws.amazon.com/cloudformation/) etc.

### Data Leak Debugging

Yikes ! Containers leak ! But the real question is how can you check it out ? Simple let’s debug our docker container.But, How to do that ? Using the docker exec command.Actually the docker exec command is inspired by a project called as [nsenter](https://github.com/jpetazzo/nsenter).It is basically done via running a shell inside your docker container. 

### Smaller Image,Better Image

Minimizing the size of your docker images is a challenging task in production.Using some tools and techniques we can minimize the docker image from dockerfile.For this purpose we can use [Docker lint](https://github.com/projectatomic/dockerfile_lint) tool by Project atomic.

### Container inside Container

Have you ever thought,can a container be run inside another container ? One of these thoughts have been explored in the past.The results were optimistic and some limitations were observed.You can check out more info at [Dind(Docker-in-Docker)](https://github.com/jpetazzo/dind).

That’s it folks on the tips and tricks section.Let’s explore one amazing tool that many companies plan to or have started to use in production.Yes you are guessing it right ! I am talking about openshift.

## What is Openshift ?

In simple words openshift is a PaaS based platform which companies have been using in production.The project is maintained and started by the RedHat folks.In the new version of openshift we can see that the project has now become a part of the kubernetes project.Starting its version 3,openshift now deploys its services in containers.

## Openshift Architecture 

![arch-diagram](https://user-images.githubusercontent.com/8342133/29638409-d3ebd124-8874-11e7-9596-ed5411422371.png)

@ Copyright Openshift by RedHat

## Openshift Setup

Openshift can be easily installed and run using several different ways.For my own usage I use minishift.It is an all in one vm which you can use on your local machine.Another way to do it is run it on a docker container by using the below command:

````
// Can be run only on a RedHat Distribution based Operating System
$ sudo docker run -d --name "origin" \
        --privileged --pid=host --net=host \
        -v /:/rootfs:ro -v /var/run:/var/run:rw -v /sys:/sys -v /var/lib/docker:/var/lib/docker:rw \
        -v /var/lib/origin/openshift.local.volumes:/var/lib/origin/openshift.local.volumes:rslave \ 
        openshift/origin start
````

The last and the most recommended way to use openshift is to install it manually using the installation instructions as given on its project [site](https://github.com/openshift/origin).

## Openshift Playground

You can check out some of the cool features of openshift such as Console, CLI etc. The web UI is available at 192.168.99.100:8443.

![open1](https://user-images.githubusercontent.com/8342133/29638575-433f77ba-8875-11e7-9c33-8c9d40686f39.png)


The default user is admin and the password is admin for the same.Click on the New Project button in order to create a new project.

![open2](https://user-images.githubusercontent.com/8342133/29638619-6bcfeeb2-8875-11e7-8f82-3cab8ce7bca9.png)

Choose the name of the project that you wish to continue with.

![open3](https://user-images.githubusercontent.com/8342133/29638669-8a2bdf7e-8875-11e7-8342-4a5a5862391f.png)

As you can see that you have 3 options using which we can easily build an up and running docker container app.The first option includes using some common languages for building your application.The second option includes a docker image which can be pulled from dockerhub.The third option includes using an JSON/YAML file.For this quick demo I am going to choose the first option.

You can quickly search nodejs in the search bar and select it as your base.Please select the first option and click on the Select button.

![open4](https://user-images.githubusercontent.com/8342133/29638695-a979f23a-8875-11e7-9679-48834ab95179.png)

Here I am going to use the sample code present at [https://github.com/openshift/nodejs-ex](https://github.com/openshift/nodejs-ex) .It is a sample nodejs based app that is provided by the Openshift Guys.

![open5](https://user-images.githubusercontent.com/8342133/29638750-e1c0a12a-8875-11e7-8c36-a4baa330bfa7.png)

When you are done,you can proceed to the create button present at the bottom of the page.

![open6](https://user-images.githubusercontent.com/8342133/29638822-1d0dcfe6-8876-11e7-84df-a96dcff1093b.png)

Voila ! You have created your first app.It is now deployed in a container which is present in a newly created pod.Awesome ! Let’s check it out using the Continue to Overview link present at the top of the page.

![open7](https://user-images.githubusercontent.com/8342133/29638869-42ca11a4-8876-11e7-818d-e6147df0fd92.png)

As you can see above the pod has been created and a container inside it is now running our chosen application code.We can check the app UI via its URL present at the top right corner of the page.

![open8](https://user-images.githubusercontent.com/8342133/29638895-66d27ad2-8876-11e7-9885-5349c15ecc3e.png)

## Conclusion
The conclusion is simple.The world has been never been simpler & fun.With containers at our service we are now deploying faster scalable infrastructure applications for masses.In the end,I hope you enjoyed reading the post :)

If you are interested in books, do make sure to buy some of these books from links down below. I am sure you will find them useful.


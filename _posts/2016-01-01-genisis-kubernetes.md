---
layout: post
title: "The genisis of Kubernetes"
date: 2016-01-01
author: Ramit Surana
tags: docker orchestration automation devops kubernetes
category: orchestration docker google etcd coreos
excerpt: "A brief study on Kubernetes and its components"
---

Although Docker has made quite a reputation,
in introducing containers as a proper solution for big and small companies in today's competitive scenario, there are many companies
that mastered the art of running scalable, production workloads in containers.Google,which is one of those popular names,
deals with more than two billion containers per week have been dealing and using containers for over a decade and has introduced
one of its brilliant products [Kubernetes][6]

## What is Kubernetes ?

[Kubernetes][6] is an open source orchestration system for Docker containers.
It handles scheduling onto nodes in a compute cluster and actively manages workloads to ensure that
their state matches the users declared intentions. Using the concepts of "labels" and "pods",
it groups the containers which make up an application into logical units for easy management and discovery.


##  Basic Terminologies about Kubernetes :

- *Pods*:
  The smallest deployable units that can be created, scheduled, and managed. Its a logical collection of containers that belong to an application.

- *Master*:
  The central control point that provides a unified view of the cluster. There is a single master node that
  control multiple minions.

- *Minion*:
  The worker node that run tasks as delegated by the master. Minions can run one or more pods.
  It provides an application-specific “virtual host” in a containerized environment.

- *Selector*:
  It is a query against labels, producing a set of result.

- *Controller*:
  A reconciliation loop that drives current state towards desired state.​

- *Service*:
  It is a set of pods that work together.​


## Architecture:

![kubernetes-key-concepts][7]

## Kubelet:

It is the primary "node agent" that runs on each node.
The kubelet works in terms of a PodSpec.(PodSpec is a YAML or JSON object that describes a pod​).
It takes a set of PodSpecs that are provided through various mechanisms and ensures that the containers described
in those PodSpecs are running and healthy.Other than from an PodSpec from the apiserver, there are three ways that
a container manifest can be provided to the Kubelet :

- *File*:
  Path passed as a flag on the command line. This file is rechecked every 20 seconds (configurable with a flag).​

- *HTTP endpoint*:
  HTTP endpoint passed as a parameter on the command line. This endpoint is checked every 20 seconds​-

- *HTTP server*:
  The kubelet can also listen for HTTP and respond to a simple API (underspecd currently) to submit a new manifest.​

## Kubernetes Cluster:

![microservices-containers-docker-bangalore-meetup-12-13-638][8]


## Etcd:

Etcd is a distributed consistent key-value store for shared configuration and service discovery.
It is by default built in for kubernetes.​All cluster data is stored here in etcd.​

![etcd][9]

## Services:

It is abstraction which defines a logical set of Pods and a policy by which to access them -
sometimes called as micro-service.Each service is given its own IP address and port which remains
constant for the lifetime of the service. So to access the service from inside your application or
container you just nedd to bind to the IP address and port number for the service.A service, through its
label selector, can resolve two or more pods. Over the life of a service, the set of pods which
comprise that service can grow, shrink, or turn over completely.


![services-overview][10]

## NameSpaces:

It enable you to manage different environments within the same cluster.
It is the abstraction which defines a logical set of Pods and a policy by which to access them -
sometimes called a micro-service.You can also run different types of server, batch, or other
jobs in the same cluster without worrying about them affecting each other.

![lxc_architecture][11]

## Flannel:

It was formerly known as Rudder.​Flannel is etcd backed network fabric for containers.
It uses etcd to store the network configuration, allocated subnets, and auxiliary data (such as host's IP).
The simplest backend of flannel is udp which uses a TUN device to encapsulate every IP fragment in a UDP packet.

![flannel][12]

## Replication Controller:

It ensures that a specified number of pod "replicas" are running at any one time.
If there are too many, the replication controller kills some pods. If there are too few,
it starts more pods.The pod Template creates new pods from a template.The labels work is to remove
the pod from a replication controller's target set,to change the pod's label.Also,deleting a replication
controller does not affect the pods it created.

## Replication Controller Architecure:


![replication-controller][13]

## Volumes:

The Volume is just a directory, possibly with some data in it, which is accessible to the containers in a pod.
It cannot mount onto other volumes or have hard links to other volumes.Each container in the Pod
must independently specify where to mount each volume.Kubernetes also supports several types of Volumes,such as :
Empty Dir,host Path,gce Persistent Disk,aws Elastic Block Store,nfs,iscsi,glusterfs,rbd,gitRepo,
secret,persistent Volume Claim etc.


It is composed of multiple libraries, which are designed to work together, or
can be used independently with other testing tools like Cucumber or Minitest.
The parts of RSpec are:​

- *rspec-core:*
The spec runner, providing a rich command line program, flexible and customizable reporting,
and an API to organize your code examples.

- *rspec-expectations:*
Provides a readable API to express expected outcomes of a code example.  ​

- *rspec-mocks:*
Test double framework, providing multiple types of fake objects to allow you
to tightly control the environment in which your specs run.   

- *rspec-rails:*
Supports using RSpec to test Ruby on Rails applications in place of Rails' built-in test framework.

![kube-gluster][14]

## Monitoring using CAdvisor:

cAdvisor is short for Container Advisor.Heapster is a cluster-wide aggregator of monitoring and event data.
Kubelet itself fetches the data from cAdvisor.It is an open source container resource usage and performance analysis agent.
Kubelet manages the pods and containers running on a machine.​

![cadvisor_logo_padded][15]


## Monitoring Architecture:

The monitoring architecture of [kubernetes][6] look like:
![monitoring-architecture][16]

## Labels and Selectors:

Labels are the key/value pairs that are attached to objects, such as pods.
Labels are intended to be used to specify identifying attributes of objects that are meaningful and
relevant to users.Using label selector, the client/user can identify a set of objects. The label selector
is the core grouping primitive in Kubernetes.

![kubernetes-label][17]

For more reference on this topic,my slides can be found at [slideshare][18].

**Hope this helps! Keep forking.**

  [6]: https://www.kubernetes.io/
  [7]: https://cloud.githubusercontent.com/assets/8342133/10783468/159283b0-7d7e-11e5-8e90-f9907b423e04.png
  [8]: https://cloud.githubusercontent.com/assets/8342133/10783557/a835bc8c-7d7e-11e5-88d1-4b065e63ca7d.jpg
  [9]: https://cloud.githubusercontent.com/assets/8342133/10783604/fec41ad0-7d7e-11e5-98de-02974c755796.png
  [10]:https://cloud.githubusercontent.com/assets/8342133/10783651/59df8076-7d7f-11e5-9dd9-5bf19dcdf48f.png
  [11]:https://cloud.githubusercontent.com/assets/8342133/10783678/87a37530-7d7f-11e5-840f-d5497c484ca2.png
  [12]:https://cloud.githubusercontent.com/assets/8342133/10783726/c9a8a752-7d7f-11e5-8e8d-9c7760149168.png
  [13]:https://cloud.githubusercontent.com/assets/8342133/10783809/5f95e8b0-7d80-11e5-93b3-2aab631909f4.png  
  [14]:https://cloud.githubusercontent.com/assets/8342133/10783904/04b25f4a-7d81-11e5-9d66-8c6b08db1c23.png
  [15]:https://cloud.githubusercontent.com/assets/8342133/10841839/c23074b6-7f12-11e5-883c-40bc51598a8a.png
  [16]:https://cloud.githubusercontent.com/assets/8342133/10784135/58e3a424-7d82-11e5-8753-388253ab1bd7.png
  [17]:https://cloud.githubusercontent.com/assets/8342133/10784176/a3783e82-7d82-11e5-8ba2-d0a7ddea2159.jpg
  [18]:http://www.slideshare.net/ramitsurana/a-brief-study-on-kubernetes-and-its-components


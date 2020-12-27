---
layout: post
title: "Achieving CI/CD with Kubernetes"
date: 2016-08-01
author: Ramit Surana
tags: kubernetes jenkins docker fabric8 Continous Integration Delivery
category: [containers, jenkins, docker]
excerpt: "Building CI/CD pipeline with kubernetes"
comments: true
---

Hola amigos !!(*In English - Hello Friends !!*) Hope you are having a jolly good day ! In terms of [Martin Fowler](http://www.martinfowler.com),Continous Integration can be defined as
"Continuous Integration is a software development practice where members of a team integrate their work frequently, usually each person integrates at least daily - leading to multiple integrations per day. Each integration is verified by an automated build (including test) to detect integration errors as quickly as possible. Many teams find that this approach leads to significantly reduced integration problems and allows a team to develop cohesive software more rapidly."

In this rather interesting piece of article we are going to discuss and explore two amazing and rather interesting pieces technology.One i.e. Jenkins,a popular Continous Integration/Deployment tool and Second i.e. Kubernetes.As an added bonus we are also going to discover fabric8 an awesome tool for microservices platform.So let's get started, 

**WARNING** Your machine may hang several times while performing the below steps.Choose a pc with high configuration.

## Methodology

There are many methodology using which we can achieve CI/CD for the on our machine.Currently,in this article we are focussed on

* [Kubernetes-Jenkins Plugin](#kubernetes-jenkins-plugin)
* [Fabric8](#fabric8)


## Overview of Architecture

Before Starting our work,first let's take a moment and analyze the workflow required to start using [kubernetes][11] containers with [jenkins][7].[Kubernetes][11] is an amazing orchestration engine for containers developed an amazing open source community.The fact that [kubernetes][11] was started by Google,gives Kubernetes an amazing advantage to use multiple open source container projects.By default,[docker][6] is the one that is supported and used most with [kubernetes][11].So the workflow,with [docker][6] containers looks like,

![k8s-jenkins-docker](https://cloud.githubusercontent.com/assets/8342133/16848715/ba1473ea-4a14-11e6-99c2-cf8899c351a9.png)

Similarly while using [rkt][9] containers a.k.a rktnetes.Here's the architecture:

![k8s-jenkins-rkt](https://cloud.githubusercontent.com/assets/8342133/16848756/ef5afe52-4a14-11e6-8723-a251b0b5c675.png)

*Currently there is no plugin supported [rkt][9] containers.But I assume that the workflow will remain the same after its due implementation.* 

## Kubernetes-Jenkins Plugin

**Setting up Kubernetes on Host Machine**

Setting up [kubernetes][11] on your host machine is an easy task.In case you wish to try out on your local machine I would recommend you to try out [minikube][8].In case you wish to the best of [kubernetes][11],please refer to .Here is a quick follow up guide to get you started with setting up [minikube][8] on your local machine :

<script src="https://gist.github.com/ramitsurana/21a137d1007980cadb98253b43a35ead.js"></script>

An amazing work in the direction of using [jenkins](7) and [kubernetes](11) has been done by [carlossg](https://twitter.com/carlossg).He has built an awesome [kubernetes][11] plugin for [jenkins][7]. Using this plugin you can easily start using [jenkins][7] with [kubernetes][11] directly.Also to provide users with more easy options to configure.He has built a [jenkins][7] image which by default contains the [kubernetes][11] plugin.This image is available at [docker hub](https://hub.docker.com/r/csanchez/jenkins-kubernetes/).In the next steps we are going to fetch this image from docker hub and create a volume /var/jenkins_home for storing all your [jenkins][7] data.

### One Problem

Although we are doing everything as we planned to do.We will still run into a problem,you will notice that whenever you are about to restart your [jenkins][7] container after closing it down.All your data is lost.Whatever you did like creating jobs,installing plugins etc. will be lost.This is one of the common problems with containers.Let's discuss it in a bit depth:

### A word about Data Containers

Data is a tricky concept when it comes to containers.The containers are not very good example of keeping data secure and available all the time.There have been many incidents in the past where the containers have been seen to leak data.There are many ways to deal with such a problem.One is to use docker volumes.Due to some reasons,I did not found it that useful.One of the ways I found to deal with persistent storage is to crete another container,called as Data Container, and use it as a source of storing data instead of depending only one image.Here's a simple figure on how we plan to use the Data Container to ensure reliability of our data,

![containers](https://cloud.githubusercontent.com/assets/8342133/17329686/54dab2e2-58e1-11e6-9ea8-15717dba3a2e.png)

Here are the steps below to start using the jenkins kubernetes image,

````
//Pulling down the jenkins-kubernetes image
$ docker pull csanchez/jenkins-kubernetes
````

````
//Created a container for containing jenkins data with the image name csanchez/jenkins-kubernetes
$ docker create --name jenkins-k8s csanchez/jenkins-kubernetes
````
The above command will create and save data in a container called jenkins-k8s,which will be used whenever we wish to further use the jenkins containers with a persistent volume.

````
//Running jenkins using another container containing data
$ docker run --volumes-from jenkins-k8s -p 8080:8080 -p 50000:50000 -v /var/jenkins_home csanchez/jenkins-kubernetes
````

Open http://localhost:8080 in your browser,you should see the below screen:

![dash](https://cloud.githubusercontent.com/assets/8342133/17297438/5a53522e-5823-11e6-8501-af049b8c2b5f.png)


### Configuring settings for Kubernetes over Jenkins

Now the jenkins is pre-configured with [kubernetes][11] plugin.So let's jump to the next step.Using the jenkins GUI go to
**Manage Jenkins** -> **Configure System** -> **Cloud** -> **Add a new Cloud *** --> **Kubernetes**
The screen looks like below after you have followed the above steps:


![k8s-config](https://cloud.githubusercontent.com/assets/8342133/17297596/02cd4bc6-5824-11e6-8ecc-2c4193f91186.png)

Now fill up your configuration settings according to the the pic below:

![k8s-jenkins5](https://cloud.githubusercontent.com/assets/8342133/17330268/d12382b4-58e3-11e6-9139-49f6a58c6026.png)

In case you wish to use jenkins slave you can use the jnlp-slave image on docker hub.This is a simple image that is used to set up slave node template for you.You configure a slave pod by creating a template like in the figure below,

![k8s-jenkins6](https://cloud.githubusercontent.com/assets/8342133/17330331/16f93c02-58e4-11e6-8a29-e3c47cfd64c4.png)

In order to use jenkins slave on the run.While creating a new job on jenkins,do this under configure settings of your job

![k8s-jenkins7](https://cloud.githubusercontent.com/assets/8342133/17330427/6fed6ac2-58e4-11e6-948a-bc9805459862.png)

Now just put the name of the label you put in **kubernetes pod template** under the restrict section.Now save and apply the settings for your new job.When build this job you should see the slave node now running after you have build up the job.
That's all folks !! You are ready to go,you can now add more of your plugins as per your needs.

## Fabric8

It is an open source microservices platform based on [Docker][6], [Kubernetes][11] and [Jenkins][7].It is built by the Red Hat guys.The purpose of the project is to make it easy to create microservices, build, test and deploy them via Continuous Delivery pipelines then run and manage them with Continuous Improvement and ChatOps.
 
Fabric8 installs and configures the following things for you automatically

* [Jenkins][7]
* [Gogs](https://gogs.io/)
* Fabric8 registry​
* [Nexus](https://wiki.jenkins-ci.org/display/JENKINS/Nexus+Artifact+Uploader)
* [SonarQube](http://sonarqube.org)

Here's a brief pic of the architecture of [Fabric8][10]

![screen shot 2014-11-06 at 10 07 10](https://cloud.githubusercontent.com/assets/8342133/17319339/a37ae71e-58aa-11e6-9956-2562973a2bc4.png)

In order to get started,first you need to install the command line tool for [fabric8][10] i.e. gofabric8.You can do that by downloading gofabric8 from https://github.com/fabric8io/gofabric8/releases.Unzipping it and use

```` 
$ sudo cp /usr/local/bin/ gofabric8
````
You can check its installation by running `$ gofabric8' on your terminal.Now run the following commands below, 

````
$ gofabric8 deploy -y
````
Your terminal screen should look like this

![fabric8](https://cloud.githubusercontent.com/assets/8342133/17319676/e272ed34-58ac-11e6-9a9d-fdd80e4971af.png)

Generating Secrets

````
$ gofabric8 secrets -y
````
Your terminal screen should look like this

![fabric8-2](https://cloud.githubusercontent.com/assets/8342133/17319713/2eab1ab4-58ad-11e6-8fb2-aad173854063.png)

Check for the status of pods using kubectl

````
$ kubectl get pods
````
![jenkins-pods](https://cloud.githubusercontent.com/assets/8342133/17319224/d4440dd6-58a9-11e6-871e-f5db9d52ca14.png)

It will take a while to get all the container images to pull down and getting started.So you can go out and drink coffee :)
You can use kubectl describe pods to check if something fails.

![fabric8-4](https://cloud.githubusercontent.com/assets/8342133/17319386/def51db4-58aa-11e6-9358-659510c082d4.png)

You can checkout the status of your pods via a opening the [kubernetes][11] dashboard in a browser:

http://192.168.99.100:30000

![fabric8-5](https://cloud.githubusercontent.com/assets/8342133/17319479/882ba7f4-58ab-11e6-822c-d8c5da934ae2.png)
 
Similarly you can open the [fabric8][10] hawtio browser interface

![fabric8-console](https://cloud.githubusercontent.com/assets/8342133/17319603/62bcd1a4-58ac-11e6-90b4-4955f54d3274.png)

From my analysis here's what happened when you ran the following commands.Basically,in a short version [fabric8][10] created 6 pods containing 6 different containers which can be controlled by the [fabric8][10] console or the [kubernetes][11] dashboard.Now you can easily integrate various different apps by using the hawtio dashboard.Here's a similar depiction of the same as shown below,

![fabric8-layout](https://cloud.githubusercontent.com/assets/8342133/17319646/ac7e9f2a-58ac-11e6-9b0c-31bb993c950c.png)

## Achieving CI/CD 

Easier said than done,building [jenkins][7] from source and integrating [kubernetes][11] is one part of the story.But achieving Continous Delivery with your setup is another very different and complex part of the story.Here are some of my tips on using certain plugins that could help you in achieving Continous Delivery with [jenkins][7]:

* [Pipeline Plugin](https://jenkins.io/solutions/pipeline/)

This is a core plugin built by the [jenkins][7] community.This plugin ensures that you can easily integrate any orchestration engine with your enviorment with very less complexity.Currently this was started as different communities had started building different plugins for various orchestration engines.This created a mess of various plugins.Using this plugin users now can implement a project’s entire build/test/deploy pipeline in a Jenkinsfile and store that alongside their code, treating their pipeline as another piece of code to be checked into source control

* [Github Plugin](https://wiki.jenkins-ci.org/display/JENKINS/GitHub+Plugin)

These days most companies are using github as SCM tool.In this case I would recommend you to use this plugin.This plugin helps you to push the code from github and analyze,test it over [Jenkins][7].For authentication purposes I would recommend you to look
[Github Oauth Plugin](https://wiki.jenkins-ci.org/display/JENKINS/GitHub+OAuth+Plugin)

* [Docker Plugin](https://wiki.jenkins-ci.org/display/JENKINS/Docker+Plugin)

For [Docker][6] guys this is one of the most suitable plugin that helps you do almost everything with [docker][6].This plugin also helps you to use [docker][6] containers as slaves. There are several other [docker][6] plugins that according to time and your usage you can switch over with.

* [AWS Pipeline](https://wiki.jenkins-ci.org/display/JENKINS/AWS+CodePipeline+Plugin)

The AWS guys have introduced an awesome service called as pipeline.This particular service helps you to attain continous delivery with your aws setup.Currently this plugin is under heavy development and might not be suitable for production enviorments.Also checkout [AWS CodeCommit](https://wiki.jenkins-ci.org/display/JENKINS/CodeCommit+URL+Helper).

* [OpenStack](https://wiki.jenkins-ci.org/display/JENKINS/Openstack+Cloud+Plugin)

For the openstack users,this plugin is suitable to configure your openstack settings with your openstack enviorment. 

* [Google Cloud Platform](https://wiki.jenkins-ci.org/display/JENKINS/Google+Deployment+Manager+Plugin)

The deployment manager is service started by the Google Cloud platform.Using Deployment Manager,you can create flexible and declarative templates that can deploy a variety of Cloud Platform services, such as Google Cloud Storage, Google Compute Engine, Google Cloud SQL, and leave it to Deployment Manager to manage the Cloud Platform resources defined in your templates as deployments.
This is a very new plugin.But I think it is worth a try if you wish to automate and sort things out with Google Cloud Platform.

In the end,I hope you enjoyed reading this article.Please let me know your valuable thoughts in the comments section below.Regrarding the blog post above I have posted my slides below.Hope you had a fun time experimenting and have a lovely day :)

<iframe src="//www.slideshare.net/slideshow/embed_code/key/D66umPt8TAvYdD" width="595" height="485" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC; border-width:1px; margin-bottom:5px; max-width: 100%;" allowfullscreen> </iframe> <div style="margin-bottom:5px"> <strong> <a href="//www.slideshare.net/ramitsurana/achieving-cicd-with-kubernetes" title="Achieving CI/CD with Kubernetes" target="_blank">Achieving CI/CD with Kubernetes</a> </strong> from <strong><a href="//www.slideshare.net/ramitsurana" target="_blank">Ramit Surana</a></strong> </div>

  [1]: http://theremotelab.com
  [2]: https://www.linkedin.com/company/the-remote-lab
  [3]: https://www.facebook.com/TheRemoteLab
  [4]: https://github.com/TheRemoteLab
  [5]: https://twitter.com/TheRemoteLab
  [6]: http://docker.com
  [7]: https://jenkins.io
  [8]: http://github.com/kubernetes/minikube
  [9]: http://coreos.com/rkt
  [10]: http://fabric8.io
  [11]: http://kubernetes.io		

---
layout: post
title: "Automating your CI/CD with Chef Automate & Habitat - Part 1"
date: 2018-01-14
author: Ramit Surana
tags: Chef CI/CD Jenkins Habitat Automate
category: [Jenkins, Continous Delivery, Continous Integration, Syntax, Chef, Habitat, Automate]
excerpt: "Using Chef Tools to automate your CI/CD Journey"
---

Hallo Freunde (German)(Hello Friends),As [Martin Fowler](http://martinfowler.com) correctly explains continous delivery:

> Continuous Delivery is a software development discipline where you build software in such a way that the software can be released to production at any time.The primary goal of the process is to be production ready anytime and anywhere.

In this simple and amazing piece of article we are going to discuss and explore some new amazing and rather interesting pieces technology.One i.e. Habitat,an Automation tool that Automates your process to build and publish Docker Images and Second i.e. Automate, which is a new chef CI/CD tool with a cool new dashboard & better features.As an added bonus I am  also going to share some nice tips that I use to make my life easier while handling the CI/CD pipelines.So let’s get started,


## Habitat

### Introduction to Habitat

Habitat is a new amazing tool introduced by Chef.It basically tries to serve one motive i.e. to automate the process of making a container image as easily as possible.You can think of it as **Dockerfile** for the docker except that it has some new features for building images and process to publish it in CI/CD perspective. The tool has been introduced in 2016 & is still into development phase. It is written in rust and reactive by nature. Now let's do some installation:

First, visit https://github.com/habitat-sh/habitat#install :

````
$ curl https://raw.githubusercontent.com/habitat-sh/habitat/master/components/hab/install.sh | sudo bash
````

After the installation, try running it on the command line using the below command:

````
$ hab
hab 0.51.0/20171219021329

Authors: The Habitat Maintainers <humans@habitat.sh>
"A Habitat is the natural environment for your services" - Alan Turing

USAGE:
    hab [SUBCOMMAND]

FLAGS:
    -h, --help       Prints help information
    -V, --version    Prints version information

SUBCOMMANDS:
    bldr      Commands relating to Habitat Builder
    cli       Commands relating to Habitat runtime config
    config    Commands relating to Habitat runtime config
    file      Commands relating to Habitat files
    help      Prints this message or the help of the given subcommand(s)
    origin    Commands relating to Habitat origin keys
    pkg       Commands relating to Habitat packages
    plan      Commands relating to plans and other app-specific configuration.
    ring      Commands relating to Habitat rings
    studio    Commands relating to Habitat Studios
    sup       Commands relating to the Habitat Supervisor
    svc       Commands relating to Habitat services
    user      Commands relating to Habitat users


ALIASES:
    apply      Alias for: 'config apply'
    install    Alias for: 'pkg install'
    run        Alias for: 'sup run'
    setup      Alias for: 'cli setup'
    start      Alias for: 'svc start'
    stop       Alias for: 'svc stop'
    term       Alias for: 'sup term'

````

If you receive the above output, then you have successfully installed habitat.


### Habitat Architecture

![chef-habitat](https://user-images.githubusercontent.com/8342133/34907583-145ce07a-f8a7-11e7-9c73-8f020a1cb739.png)

Now upon closely looking at its architecture and how to write it. You can clearly observe the various files one has to write in order to bring up the container/image. The main file in this section is the **plan.sh** file which is responsible for the deployment strategy/dependencies/package name of the habitat image. It is mandatory to make this file and configure it properly in order to achieve the best results.

Next, is the **default.toml** file. This file contains the information about the ports and external configurations of your application that you have. It is similar to having **nginx.conf** for **nginx** or **apache2conf** for **apache**, which I believe is an interesting and good idea. 

For the hooks part, I observed its usage while exploring some of the samples provided by habitat team in there docs.In simple terms, it is basically breaking down your requirements as per your application into multiple stages each having its priority in different order while running your application. Like we have **Entrypoint** in **Dockerfile**. For example, Here the file **Init** in scripts contains your initialization commands.

Some sample examples:

<script src="https://gist.github.com/ramitsurana/96e38aab74ea5529f89d02bbd8822493.js"></script>


### Habitat Builder

The Habitat Builder is a place similar to Docker Hub/Quay.io. It is a place where you can automatically check in you code with habitat and build a variety of different container images. It also enables you to publish your docker images on docker hub by connecting your docker hub account.To get started sign up at [Habitat Builder](https://bldr.habitat.sh).

![habitat1](https://user-images.githubusercontent.com/8342133/34906971-4004eb8c-f89d-11e7-8241-4761a59d8563.png)

The term origin here can be defined as a namespace which is created by the user/organization to build one's own packages. It is similar to defining your name in the dockerhub account.

![habitat2](https://user-images.githubusercontent.com/8342133/34907004-c936141c-f89d-11e7-88fe-85277ac813a0.png)

As you can observe from above Habitat asks you to connect your GitHub account and specify the path to which your plan.sh file is placed. It has a by default path under the habitat folder in which it searches your plan.sh file. You can specify your path and use the dockerhub integration if you wish to publish your images to dockerhub.

Similar to DockerHub, you can also connect your ECR Registry on your AWS account by visiting the Integrations section.

![habitat3](https://user-images.githubusercontent.com/8342133/34907040-5400acba-f89e-11e7-9269-0ad62b71a6b5.png)

After creating a package/build you can observe the dependencies by scrolling down the page:

![habitat4](https://user-images.githubusercontent.com/8342133/34907085-34168d74-f89f-11e7-8c75-e5f7e7cc22be.png)

Here you can observe that it consists of 2 sections, labelled as Transitive dependencies and Dependencies.In simple terms, the transitive dependencies can be labelled as a basic set of packages that are required by every application that you wish to build using docker. These are provisioned and managed by the Habitat Team. You can also treat it similar to the **FROM** Section when writing a Dockerfile. 

On the other hand, Dependencies label is used to signify the extra packages you are using/mentioned in your **plan.sh** file being used by your application.

### Habitat Studio

Habitat Studio is a another important feature of Habitat that allows you to test and run you application in simulation to like a real enviornment before you publish it. If you are familiar with python, you can think it as similar as [virtualenv](https://virtualenv.pypa.io/en/stable/). So let's try out hab studio.

````
$ hab studio setup
````

![habitat5](https://user-images.githubusercontent.com/8342133/34915287-a6e4174a-f949-11e7-8f62-e804f2756859.png)

In the setup default origin, choose your name for origin. In my case I am taking it as **ramitsurana**. 

![habitat6](https://user-images.githubusercontent.com/8342133/34915299-d07925d2-f949-11e7-8ebb-104744fc4795.png)

![habitat7](https://user-images.githubusercontent.com/8342133/34915319-5787f03a-f94a-11e7-955e-bd8050449202.png)

In order to achieve our objective we are going to use this github feature 

![habitat8](https://user-images.githubusercontent.com/8342133/34915325-70bca69a-f94a-11e7-9298-c0dc927471e0.png)

In case you are wondering how to create a new access Github token, please open the following [url](https://github.com/settings/tokens)

![github-access-token](https://user-images.githubusercontent.com/8342133/34915423-6f297f68-f94c-11e7-9bd7-74c6b893bb6c.png)

Copy the generated token in the cli tool for hab & you are good to go. 

**Do make sure to save this token. We will use it in the next part of the article.**

### Docker Vs Habitat

<img src="https://user-images.githubusercontent.com/8342133/34904574-7af45216-f86e-11e7-87a0-1f2abf6aea3b.png" width="auto" height="auto" />

## Chef Automate

### Introduction to Chef Automate

Chef automate is a CI/CD Based solution provided by [Chef](http://chef.io) to complete your end to end delivery requirements. It provides you with necessary tools to make your life easier and simple. It has by default integration for features & tools like [Inspec](https://www.inspec.io/) for Compliance, LDAP/SAML Support, [Slack](http://www.slack.com) Integration for Notifications etc. 

![chef-automate-habitat](https://user-images.githubusercontent.com/8342133/34904580-9db6c5ae-f86e-11e7-974e-87ead19b8dac.png)

## Trying Out on Local System:

Chef Automate can be easily tried on your local system by downloading Chef Automate from [here](https://downloads.chef.io/automate)

For the cli, Download the package from [here](https://downloads.chef.io/automate) .

In order to check, try running:

````
ramit@ramit-Inspiron-3542:~$ automate-ctl
I don't know that command.
omnibus-ctl: command (subcommand)
create-enterprise
  Create a new enterprise
create-user
  Create a new user
create-users
  Create new users from a tsv file
delete-enterprise
  Deletes an existing enterprise
delete-project
  Deletes an existing project
delete-runner
.....
````

Check if everything is good or not:
````
ramit@ramit-Inspiron-3542:~$ sudo automate-ctl preflight-check
[sudo] password for ramit: 

Running Preflight Checks:
  Checking for required resources...
    ✔ [passed]  CPU at least 4 cores
    ✖ [failed]  memory at least 16GB
  Checking for required directories...
    ✔ [passed]  /var
    ✔ [passed]  /var has at least 80GB free
    ✔ [passed]  /etc
  Checking for required umask...
    ✔ [passed]  0022
....
````

For Authenticating License:

(Grab your free License from [here](https://learn.chef.io/modules/try-chef-automate#/))
````
// Setup License & organization
$ automate-ctl setup --license /$PATH/automate.license --server-url https://localhost/organizations/$org_name --enterprise default --configure --no-build-node

$ automate-ctl reconfigure

//Create a default user and password
$ automate-ctl create-user default $user --password admin --roles "admin"
````
Try opening **http://127.0.0.1:80** to interact with the Web UI.

### Chef Automate Setup on AWS EC2

#### Prerequisites

In order for the Chef Automate Setup to work, we will use a minimal setup in order to proceed. Here are the configuration details:

| Category      | Inbound Security Ports Access         | Operating System & Instance Size                |
| ------------- |:-------------------------------------:|:------------------------------------------------|
| Chef Server | 22 (SSH), 80 (HTTP), 443 (HTTPS), 10000-10003 (push jobs) | Ubuntu 16.04(ami-21766642) & t2.micro  |
| Chef Automate Server| 22 (SSH), 80 (HTTP), 443 (HTTPS), 8989 (Git)  | Ubuntu 16.04(ami-21766642) & t2.large|

Do make sure to install the **License file** required for running Chef Automate from [here](https://learn.chef.io/modules/try-chef-automate#/).Its a 30 day free trial. As per its current [pricing](https://www.chef.io/pricing/) page the fee for Chef Automate on AWS is $0.0155 node/hour.

Also, we will be using fully-qualified domain names (FQDNs) as recommended by Chef.

Please make sure to **note the FQDN for both your Chef server and Chef Automate server**.

````
$CHEF_SERVER_FQDN="Public DNS NAME of Chef Server EC2 Instance"
$CHEF_AUTOMATE_FQDN="Public DNS NAME of Chef Automate EC2 Instance"
````
#### Setup

Let's get started:

Using the aws console,we can start 2 EC2 instances with ( t2.large ) instance type. Make sure to configre your security groups like shown below:

![chef-server-rules](https://user-images.githubusercontent.com/8342133/34915271-430eb8f6-f949-11e7-8674-d6bf5323480f.png)

**Make sure to add Port 8989 for Git with Chef-Automate Server.**

After bringing up the **chef-server** machine, please log into the machine and use git to clone the following repo:

````
$ git clone https://github.com/ramitsurana/chef-automate-habitat
````
Run scripts/install-chef-server.sh :

````
// Make sure to set the variable with proper DNS Name
$ CHEF_AUTOMATE_FQDN="Public DNS NAME of Chef Automate EC2 Instance"

// Add permissions to execute
$ chmod +x $HOME/chef-automate-habitat/scripts/install-chef-server.sh

// Run the script to install chef
$ sudo $HOME/chef-automate-habitat/scripts/install-chef-server.sh $CHEF_AUTOMATE_FQDN ramit
````

Successfully Copy Files to **Chef Server** from **local machine** using scp:

````
// License File
$ scp -i ~/.ssh/private_key -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null ~/Downloads/automate.license ubuntu@ec2-52-91-162-254.compute-1.amazonaws.com:/tmp
````


Successfully Copy Files to **Chef Automate** from **Chef Server** using scp:
````
//Copy New PEM File from Chef Server to your local machine
$ scp -i <YOUR-EC2>.pem -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null ubuntu@<YOUR-CHEF-SERVER-DNS>:/drop/delivery.pem /tmp

//Upload the delivery.pem file to Chef Automate instance 
$ scp -i <YOUR-CHEF-AUTOMATE>.pem -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null /tmp/delivery.pem ubuntu@<YOUR-CHEF-AUTOMATE-DNS>:/home/ubuntu

//Upload the your license file to Chef Automate instance 
$ scp -i <YOUR-CHEF-AUTOMATE>.pem -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null /<PATH-TO-AUTOMATE-LICENSE>.license ubuntu@<YOUR-CHEF-AUTOMATE-DNS>:/home/ubuntu
````
Now your Chef Server is fully up and ready. We now move onto Chef-Automate Sever, after getting into it using ssh. Follow these below steps:

Use git to clone the following repo:

````
$ git clone https://github.com/ramitsurana/chef-automate-habitat
````

````
// Make sure to set the variable with proper DNS Name
$CHEF_SERVER_FQDN="Public DNS NAME of Chef Server EC2 Instance"

// Add permissions to execute
$ chmod +x $HOME/chef-automate-habitat/scripts/install-chef-automate.sh

// Run the script to install chef
$ sudo $HOME/chef-automate-habitat/scripts/install-chef-automate.sh $CHEF_SERVER_FQDN ramit
````

After completing the above steps, you can proceed to open the DNS/IP for Chef Automate Server

![chef-login](https://user-images.githubusercontent.com/8342133/34936721-7687a976-fa08-11e7-9337-ec84521efdad.png)

Hoorah ! You have successfully configures chef automate and now you are ready to login.

Let's start exploring some new features of the Chef Automate Dashboard:

With your **user name** and password **admin**, try to login. You will observe the following screen:

![chef-login1](https://user-images.githubusercontent.com/8342133/34937222-1341306a-fa0a-11e7-9801-3c2cf4206785.png)

For shutting down chef automate:

````
$ sudo automate-ctl stop
````

![chef-automate-internals](https://user-images.githubusercontent.com/8342133/34936511-b2b2e574-fa07-11e7-9497-502e7687af7b.png)


### Chef Automate Internals

Some of the chef automate internals that I observed while exploring this tool are as follows:

![chef-automate-arch](https://user-images.githubusercontent.com/8342133/34941142-801a87be-fa18-11e7-9bd6-a399548c082c.png)

* [Elasticsearch](https://www.elastic.co/products/elasticsearch)
* [Logstash](https://www.elastic.co/products/logstash)
* [Nginx](https://www.nginx.com/)
* [Postgresql](https://www.postgresql.org/)
* [RabbitMq](https://www.rabbitmq.com/)

We will be discussing more on this in the next article :)

## Tips & Tricks on CI/CD

As a bonus, sharing some tips on building & managing CI/CD in a better way:

 * Automate Liveness Agent
 
 You can also use chef automate [liveness agent](https://github.com/chef/automate-liveness-agent) for sending keepalive messages to Chef Automate, which prevents nodes that are up but not frequently running Chef Client from appearing as "missing" in the Automate UI. At the time of writing, it is currently in development.

 * Using Syntax Checker

One of the most primary starting point in CI/CD is to write a file using which we describe how our jobs are handled in the pipeline. For various tools, there are many syntax validators. I found them really useful. Some of there tools are:
  
  1. [Jenkins](https://job-dsl.herokuapp.com/)
  2. [Gitlab](https://gitlab.com/ci/lint)
  3. [Travis](https://lint.travis-ci.org/)

* Use CI Web Pages for better output in Web Development Related Projects

You can use this script in Gitlab (.gitlab-ci.yml) to obtain the output at 
*http://<-USERNAME-OF-GITLAB->.gitlab.io/<-PROJECT-NAME->/*

````
pages:
  stage: deploy
  script:
  - mkdir .public
  - cp -r * .public
  - mv .public public
  artifacts:
    paths:
    - public
  only:
  - master
````

For GitHub use the below script in (_config.yml) to obtain the output at 
*http://<-USERNAME-OF-GITHUB->.github.io/<-PROJECT-NAME->/*

````
theme: jekyll-theme-cayman
````

* Avoid using Polling using GitHub Hooks

As correctly said by [Koshuke](http://kohsuke.org/2011/12/01/polling-must-die-triggering-jenkins-builds-from-a-git-hook/), it is important that we adopt new methods to trigger the pipelines.

* Use proper checkout strategy

One of the mistakes that one can do while checking out multiple repositories in a pipeline is the fact that unintended commit on other repo might be triggering the pipeline. The best command to checkout the repo is this :

````
checkout(
poll: false,
scm: [
  $class: 'GitSCM', branches: [[name: '*/master']],
  userRemoteConfigs: [[
    url: MY_URL.git,
    credentialsId: CREDENTIALS_ID]],
  extensions: [
    [$class: 'DisableRemotePoll'],
    [$class: 'PathRestriction', excludedRegions: '', includedRegions: '*']]
])

````

* Try Using Python for writing automation scripts

Python is a super amazing and fun language to work with. One of the cool reasons why I recommend it is because of the awesome libraries it has support to like [dictionary](https://pypi.python.org/pypi/PyDictionary/1.3.4), [json](https://docs.python.org/2/library/json.html), [csv](https://docs.python.org/2/library/csv.html) etc. 


## Conclusion

Exploring Chef Automate & Habitat has been a heck of a fun task for me. It enabled me to learn more about the upcoming new technologies and its usage in the DevOps world. In the end, I Hope you enjoyed this post,do share this post & tell me your fun experiences with chef in the below comments section.

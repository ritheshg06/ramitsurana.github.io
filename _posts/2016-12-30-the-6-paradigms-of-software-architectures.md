---
layout: post
title: "The 6 paradigms of software architectures"
date: 2016-12-30
author: Ramit Surana
tags: software architecture serverless microservices
category: software architectures
excerpt: "Exploring different types of enterprise class software architectures"
comments: true
---

Bonjour les amis (French) (Hello Friends).In this small,introductory and interesting post on software architectures,I am going to explore some very interesting software architectures used by industries.The software world is revolutionizing like anything.The need of people from delivering solutions have changed.Priorities of software companies have changed.The need of the Hour relies on scalability,resillency and delivery.Building software architectures to better correlate and solve today's problems helps us in defining the future roles of software in the tech world.In this post I have explored some of these software architecture's.Hope you find it a worthy read and Happy New Year 2017 :)

![different-arch](https://cloud.githubusercontent.com/assets/8342133/19617754/61667bb8-9856-11e6-82f0-0d548a452e30.png)


## Java EE Architecture

The beauty of any software architecture is dependent on the ubiquity of the language used for designing architectures.I think that the very first mature step in the direction of building enterprise class architectures was done by [Java](http://java.com).The concept of such type of architectures is based on building architecture based on business logic.The layer upon layer logic is much necessary for building such type of framework.Some of these type of frameworks are 

* [Spring Framework](http://spring.io)
* [Hibernate](http://hibernate.org)

and many more ...

A sample figure of Java EE based Architecture based on [Spring Framework](http://spring.io):

![overview-full](https://cloud.githubusercontent.com/assets/8342133/19943059/e599a97e-a15b-11e6-97a0-aabe1cf3b2a6.png)

## Monolith Architecture

When we look at startups or newly based enterprises who have started using different languages or API's for there applications.It becomes a routine to build applications based on service by service.Each layer consisting of different usage and depends upon the previous one.I think the beauty of monolith applications lie in the fact that one can easily rely on it with ease without much chances of failure for limited amount of users.This strategy makes it very wasy to adopt.Such type of applications require very less modification in code.But in monolith it is important to make sure that lots of discussion is done with other members as changes across variety of different backend services can be required to modify any changes as it may affect the overall application.

A sample figure of Monolith based Architecture

![richardson-microservices-part1-1_monolithic-architecture](https://cloud.githubusercontent.com/assets/8342133/19943135/28e6e372-a15c-11e6-83c5-0ca2bc6b2fdc.png)

## Microservices Architecture

Microservices came into action when bigger enterprises started to face certain problems with monolith architecture based services.In bigger enterprises it is important to modify changes and make it quicker to adapt in code.This creates a challenge as time is of crucial importance here.This challenge helps in the making of distributed services across enterprise.The story of adoption of microservices is a tough one.Certain challenges lie in this transformation.But the benefits seems to be outnumbered when compared.I would recommend anyone to look at the case studies of [Netflix](https://www.programmableweb.com/news/why-netflix-moved-to-microservices-architecture/elsewhere-web/2016/04/02), [Google](http://highscalability.com/blog/2015/12/1/deep-lessons-from-google-and-ebay-on-building-ecosystems-of.html), Twitter and Uber while studying microservices and its adoption.

Checkout [microservices](https://github.com/arun-gupta/microservices) by Arun Gupta for some nice code

A sample figure of Microservices Architecture

![richardson-microservices-part3-monolith-vs-microservices](https://cloud.githubusercontent.com/assets/8342133/19943184/4f9c0f92-a15c-11e6-9b7b-0bee57b0cd62.png)

### Serverless Architecture

This type of architecture has been in the market for very less no. of years.It was first introduced by Amazon Web Services in 2014.The serverless architecture is ideally more suited for startups and medium level enterprises.It helps them to automate the process of scaling and costs by building Function as a Service.Currently a lot of the development has been observed in this type of architecture.There has been a lot of different frameworks and organizations being developed in the recent couple of years.One such service is the [Lambda Service](https://aws.amazon.com/lambda/) by AWS.There are different types of services by different types of cloud service providers:

* Lambda Service - [AWS](http://aws.amazon.com)
* [Google Functions](https://cloud.google.com/functions/docs/) - Google Cloud Platform
* [Azure Functions](https://azure.microsoft.com/en-in/services/functions/) - Azure
* [OpenWhisk](https://developer.ibm.com/openwhisk/) - IBM Bluemix

Although the serverless architecture looks more like a dream come true for some people.Believe it or not,it is not.It also suffers from a serious disadvantage of monitoring.These functions can easily build a solution from one company to other but hard to engineer and monitor altogether at the same time.The serverless architecture is usually seen as implemented in [nodejs](http://nodejs.org).But there are several different languages such as Python,Go etc which can be implemented by using various different frameworks.Some of these frameworks are 

* [Serveless Framework](http://serverless.com)
* [Apex](http://apex.com)
* [Chalice](https://github.com/awslabs/chalice)

and many more..

<img width="688" alt="serverless-app" src="https://cloud.githubusercontent.com/assets/8342133/19942695/a5c9156a-a15a-11e6-8885-23d918c71b57.png">


### Reactive Architecture

Micoservices are highly revolutionised on a concept of [Domain Driven Design](http://domainlanguage.com) by [Eric Evans](https://twitter.com/ericevans0?lang=en).One of the core concepts of Domain Driven Design is Bounded Contexts.When we look at it from mathematics point of view,it reveals us that some form of it can be related to Sets and Boundries.It means that certain mathematical logics of sets and boundries can be applied here.A lot of great work has been done in this direction by [Debainsh Ghosh](https://twitter.com/debasishg).In his series of [blog posts](http://debasishg.blogspot.in/), he clearly explains how we can simulate these conditions and make it better.One of the nice things that I really like about this architecture is that it eases up the process of transforming huge enterprises to a more combustile and functional type.

One of the languages that this architecture has been implemented is [Scala](http://www.scala-lang.org/).The scala language is implemented over [Java](http://java.com).The beauty of this language is the clarity and much less code than java to implement some task in hand.It has been popular in various big data based solutions.

![sapr_0201](https://cloud.githubusercontent.com/assets/8342133/19943645/51acc964-a15e-11e6-9a47-435f00f1ce69.jpg)


### Lambda Architecture

This awesome [architecture](http://lambda-architecture.net/) has been built by [Nathan Marz](https://twitter.com/nathanmarz) and [Michael Hausenblas](http://mhausenblas.info/).In the famous article on [How to beat CAP Theorem ?](http://nathanmarz.com/blog/how-to-beat-the-cap-theorem.html) by [Nathan Marz](https://twitter.com/nathanmarz).He explains and redefines the methodology using which huge databases architectures can be made with slight changes onto our original concepts of redefining data models.The main idea of the Lambda Architecture is to build Big Data systems as a series of layers.Each layer satisfies a subset of the properties and builds upon the functionality provided by the layers beneath it.

![enterprise-lambda-architecture](https://cloud.githubusercontent.com/assets/8342133/19943306/d32a3032-a15c-11e6-8cec-61118add9d2d.jpg)

## Conclusion

As Marc Chernoff has always said,"In every end, there is also a beginning".
At the end,I feel that every type of software architecture has its own benefits and losses.There is no stop to evolution when it comes to the software architecture world.The study in the post above innvolves limited study into the evolution of software architecture.I hope that new software architecture continue to evolve & progress. Happy New Year:)

A Similar Presentation on monoliths and microservices can be found at [Slideshare](http://www.slideshare.net/ramitsurana/building-big-architectures-xp-conference-2016).

Thanks for reading !

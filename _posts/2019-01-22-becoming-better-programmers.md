---
layout: post
title: "The Pro Programmer Philosophy and some random thoughts"
date: 2019-01-22
author: Ramit Surana
tags: OOPS Python Senior Programmer 
excerpt: "How can we be become better programmers?"
---

**NOTE**: The only objective of this post is only to discuss some of the best practices that I have observed and learnt from various people around me. As this is an ongoing journey, I thank all of my friends/colleague/mentors for always beleiving and helping me to be the best version of myself. For this particular blog, I have choosen Python as the programming language.



So let's start:

1. Lesser Code vs More Code:

During our college times, we all love some or the other coding competition or hackathons, where cool projects are won based on different ideas. Most movies, if none the less, showcase the scenarios where the person who writes the fastest and working code seems to have won the challenge. In a competitive environment, like today, this all seems to have gone at the next level.

One common mistake, I have done in my early stages is writing multiple levels of logic into a single file or method. 
** Disclamer: This seems to have increased since the introduction of AWS Lambda **

The task for the below code is as follows -

* Fetch the list of all the buckets
* Get the first bucket from the list of all the buckets in that AWS accounts
* Copy some random files into that bucket
* Delete the bucket

```
import boto3
import requests

s3 = boto3.resource('s3')
s3_client = boto3.client('s3')

def lambda_handler(event, context):
    for bucket in s3.buckets.all():
       print(bucket.name)
    )
    print(copyObject)
```


2. Protect your code by using Pylint, Pycodestyle and Pytest
3. Lock Versions
3. Encapsulate Everything
4. Avoid Hardcoding at all costs


5. Being a doctor is better than being an Engineer at work

One of the best advices mentioned in the book **Clean Code** is about treating your code as your patient.
A good doctor would be the one whose patient would never have to return complaining about issues after any kind of operation or surgery.
Similarly, long lasting code would be the one having the most modularity and simplicity over some other code.

In our typical IT environment, this is one of the most neglected things that anyone bothers to look into.
Most of us are often taught how to work faster instead of how to work better. But one should always remember, 

![425007-Stephen-R-Covey-Quote-You-always-reap-what-you-sow-there-is-no](https://user-images.githubusercontent.com/8342133/54302956-e5ff8180-45e7-11e9-818e-0655cbcd5286.jpg)

Here is a bonus talk on the subject -

<iframe width="560" height="315" src="https://www.youtube.com/embed/rI8tNMsozo0" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>


<img src="https://user-images.githubusercontent.com/8342133/29498006-04930f60-8611-11e7-892c-1ad2e641e218.jpg" width="700">

References -

* https://www.developer.com/java/other/article.php/988641/Its-Not-About-Lines-of-Code.htm

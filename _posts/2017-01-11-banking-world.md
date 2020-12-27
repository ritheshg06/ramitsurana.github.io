---
layout: post
title: "Using P2P networking for digital banking transactions"
date: 2017-01-11
author: Ramit Surana
category: [P2P, Networking, BlockChain, Bitcoin, Dapps, OpenBazar]
excerpt: "Building modern day digital transaction systems"
---

Today's the internet is almost everything.But making things fast,secure and for banks, the perfect term would be micro-payments.Doing this type of payments requires a completely new transformation for the banks.This gives the open space for a new term called as FinTech or Financial Technology.We will be discussing more and more such terms in the below post.So hope you guys enjoy it !

## Introduction

Today everthing is on the internet.We all are going through a completely new era of transformation.With major countries such as India pushing efforts towards a cashless economy,it has become important for the banks to develop new methods and techniques for helping large quantity of customers go cashless easily.

## What is Digital Currency ?

Paper as important as the ones used in making the currency for several countries is still a piece of paper.Just because the government makes it and authorises it does not make it invaluable.A digtal currency is a form of cash that exists only in the digital sense.Even if you have a quarter millon dollars in your account it does not mean that the government has that much amount of cash with them in your name or account.Every bank has a certain cash amount limit that they could store.This amount is usually in the range of amount of couple of hundered of millions of dollars.Beyond that the central body controlling and authorizing the banks in every country stops them to store any exceeding amount for the same.The federal Reserve banks imposes heavy penalty charges on banks failing to abide by its rules and regulations.

## How does a transaction actually occour ?

Here is a diagram depicting what actually happens when anyone transfers or receives a certain amount to/from another bank account

![bankops-1](https://cloud.githubusercontent.com/assets/8342133/21854458/d0e20348-d840-11e6-9b9f-0f445a2a9ed1.png)

It is simple as you can see above that a third party is duly innvolved whenever a transaction is happening.This third party can be VISA,Paypal,Rupay etc. The third party acts a intermediary between transactions happening and also helping in making it secure.THe Https certificate is used everywhere and everything is duly encrypted.Also the whole transaction system occours over a different port such as 401.

## The problem for banks

I think,the real problem that lies for the banks is that with a lot of customers around the globe it is impossible for the banks to just deal with one third party client such as Paypal or Visa in every possible case.It needs to make sure that wherever the bank provides it services it has a proper third party client for the same.Recently there have been a lot of newcomers in this domain such as Rupay and .The banks pay a huge amount of money for the services that these third party clients provide.

## What is Bitcoin ?

Bitcoin is a small yet powerful concept for the same.It was first introduced by Mashimoto in 2009.(Atleast that's what appears on the research paper that has been published) In reality the founders of Bitcoin are still unknown.There have been several claims ans stuff but it's hard to say anything for sure.It uses the P2P protocol for transferring and receving money.It is SHA-256 encrypted so it's hard enough for people to crack it.One of the main features of the Bitcoin is that there is no reqirement for any third party for transferring the bitcoins from one place to another.This makes it tough for people to maintain a trust over it.In simple words,bitcoin is simple P2P networking with no governments and banks in between.

## How does Bitcoin actually work ?

If you already thrilled by the idea of Bitcoin and are thinking of buying some.Trust me and do not buy it.In the past the usage of bitcoins have been done by terroists for the same.So the big question still remains unanswered,how does it work ? Well for starters it's simple.The image below shares the exact picture of how bitcoin system works:

![how-bitcoin-works-1](https://cloud.githubusercontent.com/assets/8342133/21854763/028cc63e-d842-11e6-84eb-edf6abc4742f.png)


Actually Bitcoin is one of the implementations of Blockchain.It works by adopting the blockchain methodology 

## What is a Blockchain ? 

Blockchain is a series of networks built using a variety of different computers and packets.The transfer of bitcoins is done through the same.Blockchain depends on a variety of different stuff.It is important for banks to ensure that these stuff can be easily be easily focussed and maintained with time due to legal duties they are bound to:

* Smart Assessts
* Public Ledgers
* Private Ledgers

and more....

## Using Epehream

The Epheream is a cool new open source platform for using blockchain in real time scenarios.It helps us unlock new transactions that can take place using the Bloackchain concept.Its written in [Go](https://golang.org).Awesome ! It is easy to use and various apps could be built for the same on top of it.Below here I am showing you a small example of one of the apps that I have been building on top of it.

Download the latest package for your os from [Ethereum Releases](https://github.com/ethereum/mist/releases)

````
$ sudo apt-get install software-properties-common
````

````
$ sudo add-apt-repository -y ethereum/ethereum
````

````
$ sudo apt-get update
````

````
$ sudo apt-get install etherum
````

For using geth console:

````
$ sudo geth console
````

## Using Mist Browser

Download the Debian file of mist browser from [Download page](https://github.com/ethereum/mist/releases).

![mist](https://cloud.githubusercontent.com/assets/8342133/21855922/1dd62c74-d846-11e6-9dcb-856b1ffd5199.png)


## Building Dapps

Mist browser is the tool of choice to browse and use Ðapps.Dapps are the applications built by developers using which you can easily do amazing things.They are easy to build and after some early releases can be deployed over enterprise.

![the-ethereum-experience-42-638](https://cloud.githubusercontent.com/assets/8342133/21855128/4badb3fe-d843-11e6-814c-aac0cc01548e.jpg)


## What is IPFS ?

Do you just trust and heavily rely on Https certificates for your security ? Think again.IPFS is one those disruptive pieces of technology that can make you adopt new approaches to the security lines.For banks security is one of the prime concerns as black hat hackers are trying to keep on attacking.IPFS is a peer-to-peer hypermedia protocol to make the web faster, safer, and more open.

![1456818734ipfs-logo](https://cloud.githubusercontent.com/assets/8342133/21854895/80d83726-d842-11e6-8906-2174a74a0f32.png)


## Using IPFS

Download the tar package available at https://ipfs.io/docs/install/.

Run the installation script,found in the package.

![ipfs-install](https://cloud.githubusercontent.com/assets/8342133/21089726/74afb22a-c060-11e6-84d6-f3ce22331622.png)

Initialize the ipfs using:

````
$ ipfs init
````
![ipfs-init](https://cloud.githubusercontent.com/assets/8342133/21089799/0aa57d82-c061-11e6-8e06-23da0c5d1b8c.png)

Note your ipfs hash,and try showing the output using cat:

````
$ ipfs cat ($Hash)
````

![ipfs-cat](https://cloud.githubusercontent.com/assets/8342133/21089935/4159eba0-c062-11e6-9961-f29e04797469.png)


## Some cool stuff to check out

* [OpenBaazar](https:openbaazar.org) - Combination of eBay and Torrent
* [CoinBase](https://coinbase.com) - A Wallet service for storing and managing Bitcoins
* [Filecoin](http://filecoin.io/) - Storage solution for storing coins
* [Beaker](https://beakerbrowser.com) - P2P based browser

## Conclusion

Banks are fun.No seriously :).It has been a real fun time for me to explore such unique stuff that has been changing the way we live and how the future ahead might be.The advancements in the payment techniques are awesome and I have high hopes that with time these might get better.Hope you guys enjoyed the post and please share your side of the views in the comments too.Have a happy day !

Here are the slides:

<iframe src="//www.slideshare.net/slideshow/embed_code/key/2v9mKJCHXotoj4" width="595" height="485" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC; border-width:1px; margin-bottom:5px; max-width: 100%;" allowfullscreen> </iframe> <div style="margin-bottom:5px"> <strong> <a href="//www.slideshare.net/ramitsurana/building-digital-transaction-systems-in-the-new-banking-world" title="Building Digital Transaction Systems in the new Banking World" target="_blank">Building Digital Transaction Systems in the new Banking World</a> </strong> from <strong><a target="_blank" href="//www.slideshare.net/ramitsurana">Ramit Surana</a></strong> </div>

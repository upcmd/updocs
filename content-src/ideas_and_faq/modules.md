---
title: "more funcs support"
date: 2020-06-25T22:32:46+11:00
draft: false
weight: 2204
---

#### why not to add more features supporting via func

So far, UP cmd simply supports 3 funcs: shell, call and cmd, it is simply enough to tackle almost all different type of common programming issues

As explained in why not another func for http GET/POST or Rest API, the philosophy of the design of UP cmd is to encourage to reuse all existing CLI tools available.

A few reasons this is the best:

1. Implementing new features into UP cmd will definitely change the codebase and result in some kind of unstable behavior or bugs. Any such change will probably break user's build pipeline which could best to be avoided. 

2. By invoking the right version of external tool, you instantly gain what you need, if you are not happy, you can use a upgraded version or downgraded version of that tool. This does not necessary bundle together with UP cmd, it perfectly avoids the dependency hell problem or being forced to upgrade for the sake of upgrading to use one tiny feature of new version but break the rest of your entire stack. Refer to: https://nixos.org/nixos/nix-pills/why-you-should-give-it-a-try.html#idm140737320838032 to taste a little of dependency hell.     

3. It will benefit from all open source community and gain most success of your own project and product

#### best practice using UP cmd

It encourages collaboration hence it supports a feature of module. A project team should be brave enough to share and open source the common used module, for example, the modules like below:

* a module to deal AWS EBS backup and restore
* a module to create full web application stack
* a module to do helm deployment of certain version of software in k8s
* a module to bootstrap a linux box and install all required softwares
* a module in Mac to setup Mac with defined softwareds
........

The ideas could be limitless   

In this way, your own code could be reused and verified and tested by others, hence you could avoid some problems encountered by others and save time and possibly avoid disaster in your own production

Simply saying, try to use an existing module first and keep improving on it and contribute to it, or publish your own quality module for others, it benefits the mutual way.   


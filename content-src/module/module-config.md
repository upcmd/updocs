---
title: "module config"
date: 2020-06-25T22:32:46+11:00
draft: false
weight: 2130
---

#### Configure module

* alias: alias is a client side name for the module to be address and named

For example, if your code repo name is sayhi, if you do not use the alias, by default the module name will be sayhi, when you invoke the task: SayHI, then you invoke: sayhi.SayHI, with the alias like below example, named himodule, you invoke it as: himodule.SayHI instead  

For the below field with a prefix _ in the example, it means that is the implicit value it will hold when above logic applied

For the below field with a prefix :> in the example, it means that it demands it to be configured as mandatory to continue

```
- repo: https://github.com/up/sayhi.git
  alias: himodule
```

#### with only repo defined

rules:

* clone the repo, repo name will be the module name
* if version==empty, then it will use master
* if dir==empty, it will use cloned git repo root dir
* dir==empty, then mod.dir will be auto mapped to repo name


```
  - repo: https://github.com/up/sayhi.git
    _dir: sayhi
    _alias: sayhi
    _version: master
    _iid: noname
```

#### repo with pullpolicy

The pullpolicy is the policy used to clone and pull git repo, there are three types of policies:

* manual
* always
* skip

rules:

* skip: if repo exist already, then skip it and do not pull, this is the default if it is not configured
* manual: if repo exist already, then stop processing and fails straight away, it will let user to manually resolve this
* always: always delete the repo and repull from remote again

```
  - repo: https://github.com/up/sayhi.git
    pullpolicy: manual | always | skip
```    

#### both repo and dir exist and configured

rules:

* if both repo and dir exist, then the repo will be cloned and named to dir name

```
  - repo: https://github.com/up/sayhi.git
    dir: himodule
    _alias: himodule
    _version: master
    _iid: noname
```

#### both repo and subdir exist and configured

rules:

* this is to use sub directory ./sayhi/a/b as the module location instead of root repo dir
* alias is mandatory

```
  - repo: https://github.com/up/sayhi.git
    subdir: a/b
    _dir: sayhi
    :>alias: require setup to make it clear what the module name is
    _version: master
    _iid: noname
```

#### version is a tag
    
rules:

* use version value as tag name to check it out

```
  - repo: https://github.com/up/sayhi.git
    version: 1.0.0
    _dir: sayhi
    _alias: sayhi
    _iid: noname
```

#### use alias

rules:

* the alias will be a translated name to be uesd in caller's code base, eg. call: hi.SayHiTask, without alias, you will have to use sayhi.SayHiTask. This is to just to avoid duplicated repo name, eg: https://github.com/somethingeles/sayhi.git

```
  - repo: https://github.com/up/sayhi.git
    alias: hi
    _version: master
    _dir: sayhi
    _iid: noname
```

#### use SHA as version

rules:

* sha value is a specific value to checkout if it presents as version
* the full SHA is preferred, but 7 digits is fine

```
  - repo: https://github.com/up/sayhello.git
    version: abc123e
    _alias: sayhello
    _dir: sayhello
    _iid: noname
```

### use local module from a directory

rules:

* if there is no repo, then it will use the dir as module and incorporate as module
* alias is mandatory

```  
  - dir: c/d
    :>alias: require setup to make it clear what the module name is
    _iid: noname
```

### use absolute path

rules:

* you can use absolute path for dir to be the location of a module
* alias is mandatory
* absolute path is not preferred, reletive path to current working directory or refdir is always preferred

```
  - dir: /etc/c/d
    :>alias: require setup to make it clear what the module name is
    _iid: noname
```

#### use instanceid

rules:

* iid is the instance id, which could be used in module, if it is empty then use default nonamed

```
  - dir: /etc/c/d
    :>alias: require setup to make it clear what the module name is
    iid: dev
```

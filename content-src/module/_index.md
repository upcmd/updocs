---
title: "modules"
date: 2020-02-07T14:32:46+11:00
draft: false
weight: 2100
---

#### Use module

Module is the way for code to be organised and wrapped in a reusable way. It encourages community members to share developed code shipped following a standard. It also encourage collaboration and code reuse.

#### Use cases

##### Project based code reuse

```

mywebapp_project/
    src/                                 <--    project src code
    static/                              <--    project static resources
    up.yml
    upconfig.yml
    modlock.yml
    aws_stack_module/                    <--     project based local module deal with aws cloudformation stack creation/deletion etc
    .upmodules/
        efs_module/                      <--     a external module from remote git repo
        elastic_cache_module/            <--     a external module from remote git repo
        postgres_module/                 <--     a external module from remote git repo
    
```    

This is a typical structure you manage your project, you can reuse all external modules from remote git repo, simply define these modules in upconfig.yml

An simple example would be:

```
Modules:
  - repo: https://github.com/peterlee/efs_module.git
    iid: trial

  - repo: https://github.com/jamesrules/elastic_cache.git
    alias: elastic_cache_module

  - repo: https://github.com/joedoe/postgres_module.git
    version: release_2.0.0

  - dir: aws_stack_module

```

##### Use as system wide tool box to provide some utils

```

~/mycliutils/
    up.yml
    upconfig.yml
    modlock.yml
    personal_module/                      <--     local module
    .upmodules/
        dep1_module/                      <--     a external module from remote git repo
        dep2_module/                      <--     a external module from remote git repo
```    

upconfig.yml
```    
workdir: refdir
```    


bash.rc
```
alias bootstrap_my_mac=up ngo bootstrap_mac -d ~/mycliutils/ --configdir=~/mycliutils/    
alias setup_network=up ngo setup_network -d ~/mycliutils/ --configdir=~/mycliutils/    
```

This example shows that you can setup a directory as jailed work environment of certain functionalities, eg bootstrap the whole mac, installing all required softwares, use brew install all pkgs, setup python virtual env, node_js environment, ruby, rvm etc. In this way, all the operation, cached intermediate files will be generated in that setup utils directory, eg ~/mycliutils/, you can make it as one centralised directory, or a decentralised multiple directories, it's flexible and up to you to decide. 
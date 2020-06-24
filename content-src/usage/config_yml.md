---
title: "config yml"
date: 2020-02-07T14:32:46+11:00
draft: false
weight: 103
---

#### Configuration file: upconfig.yml

A upconfig.yml is essential for some default configuration used by up command program

A default example config:

```

version: 1.0.0
Verbose: v
MaxCallLayers: 8

```

#### Configuration items

Common configruations:

* version: This is the version of your configuration

* Verbose: This is the default verbose level

The supported verbose level: 

    1. v
    2. vv
    3. vvv
    4. vvvv
    5. vvvvv
    6. vvvvvv

Level vvvvvv is used for debugging and reporting bug only 

* MaxCallLayers: This is the allowed max call layers you want your program to handle

In case your program accidentally enters a recursive endless loop, this will ensure your program to exit 

If this is not configured or it is configured to be 0, then your task execution might have the risk of endless loop if not coded and tested properly
 
* RefDir: this is the main directory that your task files, flow file, dvar file, template files and all different type of externalised files located

If this is configured, you do not need to use RefDir tag in your task/func configuration

Otherwise, this RefDir can still be obtainable in two way:

Firstly, from cli command line args, ref to:  [CLI command line args](../../usage/cli_args)
Secondly, use RefDir element in task configuration in various different occasions

* TaskFile: This is a default preferred task name you would like UP cmd to load it by default

Without it, you will need to specify a task name in your CLI command

Please ref to:  [CLI command line args](../../usage/cli_args) for details

* ConfigDir: This is the default directory preferred to save your upconfig.yml

You could simply make default to current directory, so that you could have a project based configuration, or you could use a global config, eg: ~/.up/upconfig.yml for all projects

* ConfigFile: This is the default configfile name for your configuration, by default it is upconfig, that means your config file will be loaded frm upconfig.yml

* ShellType: This is the shell to execute the shell func command

You could configure it in following ways:

1. default: /bin/shell

If you do not setup anything, this would be the default

2. customized location of your own shell, eg: /usr/local/local/shell

3. maybe more generic, if you do not care about the path of the shell executable: env shell

4. you can try different type of shell, eg: /usr/local/bin/bash

Please note, it is best practice to use /bin/shell for consistency of the behavior and note that the tasks are mainly for CI/CD build and it is better to not to have any flavour but just simply and run anywhere, also in this way, your code could be wrapped and publish to be shared as module to be easily used by others

5. use: GOSH, GOSH is a golang implementation of shell parser, if you choose to use GOSH, it means you don't have to rely a MacBook or Linux machine to run UP cmd, you can use windows to execute UP cmd 

Please note, here GOSH is just a string name, not a path

Please ref to:  [gosh cross platform shell parser](https://github.com/mvdan/sh) for details

6. use: GOSH externally

You can install GOSH into your local as binary, following above details, you should be able to install it using go install: 

```bash
go install mvdan.cc/sh/v3/cmd/gosh
```

then setup ShellType: ~/go/bin/GOSH
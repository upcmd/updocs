---
title: "command line basics"
date: 2020-06-25T22:32:46+11:00
draft: false
weight: 101
---

#### Uage

```

Ξ ▶ up           
usage: up [<flags>] <command> [<args> ...]

UP: The Ultimate Provisioner

Flags:
      --help                     Show context-sensitive help (also try --help-long and --help-man).
  -v, --verbose=VERBOSE          verbose level: v-vvvvv
  -d, --refdir=REFDIR            ref yml files directory
  -t, --taskfile=TASKFILE        task file to load (without yml extension)
  -i, --instance=INSTANCE        instance name for execution
  -p, --execprofile=EXECPROFILE  profile name for execution to setup a group environment variables
      --configdir=CONFIGDIR      config file directory to load from|default .
      --configfile=CONFIGFILE    config file name to load without yml extension|default config

Commands:
  help [<command>...]
    Show help.

  ngo [<taskname>]
    run a entry task

  init
    create a default skeleton for a quick start

  list [<taskname>]
    list tasks and plays

  assist [<assistname>]
    assist: templatefunc|version

  validate <validatetaskname>
    validate tasks and plays

```

#### How to run a task

* use ngo command to execute a task
* by default it will take task name Main as the default entry if not specified

```

Ξ ▶ up ngo taskname           

```


#### Auto create a required config.yml file and skeleton task file to start

```

Ξ ▶ up init
-init default skeleton and configuration

Ξ ▶ ls             
up.yml  upconfig.yml

Ξ ▶ cat upconfig.yml

version: 1.0.0
Verbose: v
MaxCallLayers: 8
RefDir: .
TaskFile: up
ConfigDir: .
ConfigFile: upconfig

Ξ ▶ cat up.yml

tasks:
  -
    name: Main
    desc: main entry
    task:
      -
        func: shell
        desc: main job
        do:
          - echo "hello world"
```

#### Check version

```
▶ up assist version
loading [Config]:  ./upconfig.yml
Main config:
             Version -> 1.0.0
              RefDir -> .
             WorkDir -> cwd
          AbsWorkDir -> /up-project/up
            TaskFile -> up.yml
             Verbose -> v
          ModuleName -> self
           ShellType -> /bin/sh
       MaxCallLayers -> 8
 MaxModuelCallLayers -> 256
work dir: /up-project/up
-assist: version

version_info:
  Type: tagged-version
  Tag: 0.9.1
  Vesion: 0.9.1
  SHA: f8e54f33074a1e1e57e6da93697533aa404695fd
  Source: https://github.com/upcmd/up/tree/f8e54f33074a1e1e57e6da93697533aa404695fd
  Changes: |
    1. Version query support via: up assist version  

```
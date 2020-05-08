---
title: "command line basics"
date: 2020-02-07T14:32:46+11:00
draft: false
weight: 101
---

#### Uage

```

Ξ ▶ up           
usage: up [<flags>] <command> [<args> ...]

UP: The Ultimate Provisioner

Flags:
      --help                   Show context-sensitive help (also try --help-long and --help-man).
  -v, --verbose=VERBOSE        verbose level: v-vvvvv
  -d, --refdir=REFDIR          ref yml files directory
  -t, --taskfile=TASKFILE      task file to load (without yml extension)
  -i, --instance=INSTANCE      instance name for execution
      --configdir=CONFIGDIR    config file directory to load from|default .
      --configfile=CONFIGFILE  config file name to load without yml extension|default config

Commands:
  help [<command>...]
    Show help.

  ngo [<taskname>]
    run a entry task

  init
    create a default skeleton for a quick start

  list [<taskname>]
    list tasks and plays

  validate <validatetaskname>
    validate tasks and plays

```

#### How to run a task

* use ngo command to execute a task
* by default it will take task name Main as the default entry if not specified

```

Ξ ▶ up ngo taskname           

```

#### Minimal requirement to run a task

You will need a upconfig.yml file to be there in your current working directory, otherwise you will get an error like below:

```

Ξ ▶ up ngo taskname
loading [Config]:  ./upconfig
yml file: ./upconfig errored: Config File "upconfig" Not Found 

```


#### Auto create a required config.yml file and skeleton task file to start

```

Ξ ▶ up init
-init default skeleton and configuration

Ξ ▶ ls             
up.yml  upconfig.yml

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

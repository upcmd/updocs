---
title: "command args"
date: 2020-02-07T14:32:46+11:00
draft: false
weight: 104
---

### Command args

#### Verbose

```
  -v, --verbose=VERBOSE        verbose level: v-vvvvv
```

verbose is one of the flags used to take the input for verbose level

The supported verbose level: 

    1. v
    2. vv
    3. vvv
    4. vvvv
    5. vvvvv
    6. vvvvvv

For example:

If you run the default Main task

```
Ξ ▶ up ngo         
loading [Config]:  ./upconfig
-exec task: Main
loading [Task]:  ./up
instance id: nonamed
Task1: [Main ==> Main: main entry ]
-Step1: [: main job ]
cmd( 1):
hello world
 .. ok
. ok

```

If you run the default Main task, with verbose level vvv

```
Ξ localtest/01 git:(wip) ▶ up ngo -v vvv   
loading [Config]:  ./upconfig
-exec task: Main
loading [Task]:  ./up
instance id: nonamed
Task1: [Main ==> Main: main entry ]
-Step1: [: main job ]
overall final exec vars:

(*core.Cache)({
  "up_runtime_task_layer_number": 0
})

cmd( 1):
echo "hello world"

hello world
 .. ok
. ok
```

Of level vvvv

```

Ξ ▶ up ngo -v vvvv
loading [Config]:  ./upconfig
 :verbose level:  vvvv
 :instance name:  
-exec task: Main
loading [Task]:  ./up
---------group vars----------

global: {
}


groups members:[]
instance id: nonamed
merged[ nonamed ] runtime vars:
{
}

-------runtime global final merged with dvars-------

{
}

  located task-> 1 [Main]: 
Task1: [Main ==> Main: main entry ]
Executing task stack layer: 1

-Step1: [: main job ]
{
  Name: "",
  Do: {
    "echo \"hello world\""
  },
  Dox: <nil>,
  Func: "shell",
  Vars: <nil>,
  Dvars: <nil>,
  Desc: "main job",
  Reg: "",
  Flags: <nil>,
  If: "",
  Else: <nil>,
  Loop: <nil>,
  Until: "",
  RefDir: ""
}

current exec runtime vars:
(*core.Cache)({
  "up_runtime_task_layer_number": 0
})

overall final exec vars:

(*core.Cache)({
  "up_runtime_task_layer_number": 0
})

cmd( 1):
echo "hello world"

 \_ echo "hello world"
hello world
 .. ok
. ok

```

#### refdir

```
  -d, --refdir=REFDIR          ref yml files directory
```

refdir is used to take refdir input value

Please ref to:  [config](../../usage/config_yml) for details

Please note that the command line arg input value will override the default configuration value

#### taskfile

```
  -t, --taskfile=TASKFILE      task file to load (without yml extension)
```

taskfile is used to take which task file to load the tasks from

Please note that the command line arg input value will override the default configuration value

Please also ref to:  [config](../../usage/config_yml) for details about taskfile

#### instance id

```
  -i, --instance=INSTANCE      instance name for execution
```


instance is a arg to take input for an instance id for the task execution

An instance id is a context/profile name for your task execution, for example, you could call your instance id "dev", which is a representation of dev environment

The instance somehow is linked to your var scope and var group, in such a case, your eventual specific context vars will be merged and rendered automatically

If there is no instance id given, by default UP cmd will just use nonamed to call it and it will not be categorized into any scope group 

Please also ref to:  [scope](../../scope) for details 
 
#### configdir and configfile

```
  --configdir=CONFIGDIR    config file directory to load from|default .
  --configfile=CONFIGFILE  config file name to load without yml extension|default config
```

configdir and confifile are the flags to take input of the configuration yml file from specified directory and filename

These input will override the default configuration

Please also ref to:  [config](../../usage/config_yml) for details about taskfile


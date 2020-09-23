---
title: "environment vars"
date: 2020-06-25T22:32:46+11:00
draft: false
weight: 1100
---

## Use pure env

There are two ways you can use pure env, which unset all preset/inherited environment variales

* use --pure in command arg
* use pure in config

Note that only the Pure config entry in entry module of upconfig.yml will be used, but not every module's, as one pure env setting up will unset all current session context env vars to empty and this is to ensure that the pure operation is always from caller session

### Run the example

up ngo task -d ./tests/functests -t e200.yml --configdir=./tests/functests --pure

### Demo to call the default task

```
Ξ /up-project/up git:(master) ▶  up ngo task -d ./tests/functests -t wip.yml -i dev --configdir=./tests/functests --pure
loading [Config]:  ./tests/functests/upconfig.yml
true
Main config:
           Version -> 1.0.0
            RefDir -> ./tests/functests
           WorkDir -> cwd
        AbsWorkDir -> /up-project/up
          TaskFile -> wip.yml
           Verbose -> v
        ModuleName -> self
         ShellType -> /bin/sh
     MaxCallLayers -> 8
           Timeout -> 3600000
MaxModuelCallLayers -> 256
         EntryTask -> task
work dir: /up-project/up
-exec task: task
loading [Task]:  ./tests/functests/wip.yml
-set pure env context done.
module: [self], instance id: [dev], exec profile: []
Task1: [task ==> task:  ]
-Step1:
cmd( 1):
-
my env vars:
PWD=/up-project/up
SHLVL=1
_=/usr/bin/env

-
.. ok
. ok
```

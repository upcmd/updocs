---
title: "exec profile with pure env flag"
date: 2020-06-25T22:32:46+11:00
draft: false
weight: 1790
---

### Run the example

up ngo -p pure-env-vars -d ./tests/functests -t e0198.yml --configdir=./tests/functests

### Use pure flag in eprofile

Sometimes when you start a cli program, you would like all the env vars are statically declared from within your shell script or rc file, but not inherited from current shell session context

In order to achieve this, we can use the pure flag in the eprofile

When you state pure to be ture, it will clean up and unset all existing env vars from the inherited shell context

### What about I want to do the same to set pure if start the task without using eprofile?

In this case, it is a matter to just use cmd: virtualEnv, then use pure arg

Refer to c0193 for example

### Exec log

The expected result is that it will unset all env vars and leave only the declared PERSON_NAME there in the context

Please note that there are still a few contextual env vars showing. This is because they are the one required by the shell execution and can not be unset

```

/up-project/up git:(master) ▶ up ngo -p pure-env-vars -d ./tests/functests -t e0198.yml --configdir=./tests/functests
loading [Config]:  ./tests/functests/upconfig.yml
Main config:
           Version -> 1.0.0
            RefDir -> ./tests/functests
           WorkDir -> cwd
        AbsWorkDir -> /up-project/up
          TaskFile -> e0198.yml
           Verbose -> v
        ModuleName -> self
         ShellType -> /bin/sh
     MaxCallLayers -> 8
           Timeout -> 3600000
MaxModuelCallLayers -> 256
         EntryTask -> Main
work dir: /up-project/up
-exec task: Main
loading [Task]:  ./tests/functests/e0198.yml
module: [self], instance id: [nonamed], exec profile: [pure-env-vars]
-set pure env context done.
Task1: [task ==> task:  ]
-Step1:
cmd( 1):
-
my pre-set env var: Peter-Jackson
=================================
PERSON_NAME=Peter-Jackson
PWD=/up-project/up
SHLVL=1
_=/usr/bin/env

-
.. ok
. ok

```

### Code

```


eprofiles:
  - name: pure-env-vars
    taskname: task
    pure: true
    desc: |
      a test profile starting with a pure env vars environment
      meaning the execution shell session will unset all inherited env vars from caller's context
    evars:
      - name: PERSON_NAME
        value: Peter-Jackson

tasks:

  -
    name: task
    task:
      -
        func: shell
        do: |
          echo """my pre-set env var: $PERSON_NAME"""
          echo "================================="
          env|grep =


```

#### Relevant

#### Environment variables

* [set env var](../../env-vars/c0048/)
* [get env var](../../env-vars/c0046/)
* [virtual env](../../env-vars/c0193/)

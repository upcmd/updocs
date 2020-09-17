---
title: "exec profile with entry task"
date: 2020-06-25T22:32:46+11:00
draft: false
weight: 1789
---

### CICD Pipeline integration

To integrate with pipeline, one challenge is to manage the input args. By using execution profile, we aim to reduce the number of input parameters and fill in or configure the manually in many places in cicd platforms or pipelines

In order to achieve this, we can use two optional fields:

```
taskname: a_entry_task_name
verbose: vvv
```


It simply instruct up cli to use the indicated task name and verbose level instead of the task name and verbose as command line argument

so this turns the command line from:

```
up ngo task_to_call_dictated_by_eprofile -v v
```

to:

```
up ngo -p test_eprofile_exec
```

### config

```

eprofiles:
  - name: test_eprofile_exec
    instance: dev

    taskname: task_to_call_dictated_by_eprofile
    verbose: vvv

    evars:
      - name: ENV_VAR_AAA
        value: aaa

tasks:
  -
    name: task
    task:
      -
        func: cmd
        do:
          - name: print
            cmd: hahaha

  -
    name: task_to_call_dictated_by_eprofile
    task:
      -
        func: cmd
        desc: |
          if you use eprofile, then evar UP_EXEC_ENTRY_TASK_NAME's value will be used as entry task, it will override whatever has been input from command line
        do:
          - name: print
            cmd: this is a entry task according to the evar - UP_EXEC_ENTRY_TASK_NAME

```

### log

```
Ξ /up-project/up git:(master) ▶ goup ngo -p test_eprofile_exec -d ./tests/functests -t e0194.yml --configdir=./tests/functests -v v
loading [Config]:  ./tests/functests/upconfig.yml
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
           EntryTask -> Main
work dir: /up-project/up
-exec task: Main
loading [Task]:  ./tests/functests/wip.yml
module: [self], instance id: [dev], exec profile: [test_eprofile_exec]
profile - test_eprofile_exec envVars:

(*core.Cache)({
})

Task2: [task_to_call_dictated_by_eprofile ==> task_to_call_dictated_by_eprofile:  ]
-Step1: [: if you use eprofile, then evar UP_EXEC_ENTRY_TASK_NAME's value will be used as entry task, it will override whatever has been input from command line
 ]
self: final context exec vars:

(*core.Cache)({
  "up_runtime_task_layer_number": 0
})

~SubStep1: [print:  ]
this is a entry task according to the evar - UP_EXEC_ENTRY_TASK_NAME

```

#### Relevant

#### Environment variables

* [set env var](../../env-vars/c0048/)
* [get env var](../../env-vars/c0046/)
* [virtual env](../../env-vars/c0193/)
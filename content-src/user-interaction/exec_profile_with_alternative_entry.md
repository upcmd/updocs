---
title: "exec profile with alternative entry"
date: 2020-06-25T22:32:46+11:00
draft: false
weight: 1791
---

### What do we want to achieve?

Most of time, the execution profile is good enough for a pipeline entry, which linked to a default taskname to start. However there are times that the execution context, such as those env vars are used for the same pipeline but in different stages. In this case, we eventually just use the same eprofile but different env vars

To solve this issue, we introduce to use an env var named: UP_ENTRY_TASK. If you set this env var in your pipeline and execute up cmd with the exec profile, then up cmd will use the entry task dicated from UP_ENTRY_TASK instead of the default one using taskname

### Demo to call the default task

```
Ξ /up-project/up git:(master) ▶ goup ngo -p entry_task -d ./tests/functests -t e0199.yml --configdir=./tests/functests -v vvv
loading [Config]:  ./tests/functests/upconfig.yml
Main config:
Version -> 1.0.0
RefDir -> ./tests/functests
WorkDir -> cwd
AbsWorkDir -> /up-project/up
TaskFile -> e0199.yml
Verbose -> vvv
ModuleName -> self
ShellType -> /bin/sh
MaxCallLayers -> 8
Timeout -> 3600000
MaxModuelCallLayers -> 256
EntryTask -> Main
work dir: /up-project/up
-exec task: Main
loading [Task]:  ./tests/functests/e0199.yml
module: [self], instance id: [nonamed], exec profile: [pure-env-vars]
entry task: alterative_entry_task
-set pure env context done.
profile - pure-env-vars envVars:
Task1: [default_entry_task ==> default_entry_task:  ]
cmd( 1):
echo "default task"
-
default task

-
.. ok
. ok

```

### Demo to call the alternative task using env var UP_ENTRY_TASK

```
Ξ /up-project/up git:(master) ▶ export UP_ENTRY_TASK=alterative_entry_task

Ξ /up-project/up git:(master) ▶ goup ngo -p entry_task -d ./tests/functests -t e0199.yml --configdir=./tests/functests
loading [Config]:  ./tests/functests/upconfig.yml
Main config:
Version -> 1.0.0
RefDir -> ./tests/functests
WorkDir -> cwd
AbsWorkDir -> /up-project/up
TaskFile -> e0199.yml
Verbose -> v
ModuleName -> self
ShellType -> /bin/sh
MaxCallLayers -> 8
Timeout -> 3600000
MaxModuelCallLayers -> 256
EntryTask -> Main
work dir: /up-project/up
-exec task: Main
loading [Task]:  ./tests/functests/e0199.yml
module: [self], instance id: [nonamed], exec profile: [entry_task]
entry task: alterative_entry_task
Task2: [alterative_entry_task ==> alterative_entry_task:  ]
-Step1:
cmd( 1):
-
alternative task

-
.. ok
. ok
```

#### Relevant

#### Environment variables

* [set env var](../../env-vars/c0048/)
* [get env var](../../env-vars/c0046/)
* [virtual env](../../env-vars/c0193/)
* [pure env](../../user-interaction/exec_profile_with_pure_envvrs/)

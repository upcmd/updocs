---
title: "interactive session"
date: 2020-06-25T22:32:46+11:00
draft: false
weight: 901
---

### interactive session

If there is login shell in interactive session, it will enter the interactive session until the user exits the session

### Demo

```

Ξ /up-project/up git:(master) ▶ wiptest
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
           EntryTask -> task
  ModRepoUsernameRef -> 
  ModRepoPasswordRef -> 
work dir: /up-project/up
-exec task: task
loading [Task]:  ./tests/functests/p0201.yml
module: [self], instance id: [dev], exec profile: []
Task1: [task ==> task:  ]
-Step1: [
it will print hello
then traped in a shell session in docker run
once user exits the session
then it will continue the proc to print the world
]
cmd( 1):
-
hello

-
 .. ok
cmd( 2):
-
81b0134b8a7d:/# echo "I am in docker"
I am in docker
81b0134b8a7d:/# exit

-
 .. ok
cmd( 3):
-
world

-
 .. ok
. ok

```
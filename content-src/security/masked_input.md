---
title: "mask sensitive input"
date: 2020-06-25T22:32:46+11:00
draft: false
weight: 1205
---

#### Feature

When you use prompt to accept user input, the terminal will log the senstive information, such as password. There is chance this will go to the logs and be exposed to cause security issue.

Requirement from issue: https://github.com/upcmd/up/issues/20

### Example

```
tasks:
  -
    name: task
    task:
      - func: shell
        desc: input raw value
        dvars:
          - name: username
            flags: [prompt,]
        do:
          - echo "hello {{.username}}"

      - func: shell
        desc: |
          input secret, eg a password
          this will be masked
          however password is still leaked during the debugging or in higher verbose level
        dvars:
          - name: password
            flags: [prompt, masked]
        do:
          - echo "password is - {{.password}}"

      - func: shell
        desc: |
          password will be saved and kept into vault intead
        dvars:
          - name: protectedPassword
            flags:
              - prompt
              - masked
              - secret
        do:
          - echo "this print out nothing as protectedPassword is stored in the vault as secret"
          - echo "protectedPassword is - {{.protectedPassword}}"
          - echo "this print out the retrieved secret from vault"
          - echo "protectedPassword is - {{ "protectedPassword" | fromVault}}"

```

#### Log

```
Ξ /up-project/up git:(master) ▶ up ngo task -d ./tests/functests -t p0210.yml -i dev --configdir=./tests/functests
loading [Config]:  ./tests/functests/upconfig.yml
Main config:
             Version -> 1.0.0
              RefDir -> ./tests/functests
             WorkDir -> cwd
          AbsWorkDir -> /up-project/up
            TaskFile -> wip.yml
             Verbose -> vvv
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
loading [Task]:  ./tests/functests/wip.yml
module: [self], instance id: [dev], exec profile: []
profile -  envVars:

(*core.Cache)({
})

Task1: [task ==> task:  ]
-Step1: [: input raw value ]
Enter Value For [username]: 
This will be saved as username's value
Tom
self: final context exec vars:

(*core.Cache)({
  "up_runtime_task_layer_number": 0,
  "username": "Tom"
})

cmd( 1):
echo "hello {{.username}}"

-
hello Tom

-
 .. ok
. ok
-Step2: [
input secret, eg a password
this will be masked
however password is still leaked during the debugging or in higher verbose level
]
Enter Value For [password]: 
This will be saved as password's value
**********
self: final context exec vars:

(*core.Cache)({
  "up_runtime_task_layer_number": 0,
  "password": "mypassword",
  "last_result": (*utils.ExecResult)({
    Cmd: "echo \"hello Tom\"",
    Code: 0,
    Output: "hello Tom",
    ErrMsg: ""
  })
})

cmd( 1):
echo "password is - {{.password}}"

-
password is - mypassword

-
 .. ok
. ok
-Step3: [
password will be saved and kept into vault intead
]
Enter Value For [protectedPassword]: 
This will be saved as protectedPassword's value
*****************
self: final context exec vars:

(*core.Cache)({
  "last_result": (*utils.ExecResult)({
    Cmd: "echo \"password is - mypassword\"",
    Code: 0,
    Output: "password is - mypassword",
    ErrMsg: ""
  }),
  "up_runtime_task_layer_number": 0
})

cmd( 1):
echo "this print out nothing as protectedPassword is stored in the vault as secret"

-
this print out nothing as protectedPassword is stored in the vault as secret

-
 .. ok
cmd( 2):
echo "protectedPassword is - {{.protectedPassword}}"

-
protectedPassword is - <no value>

-
 .. ok
cmd( 3):
echo "this print out the retrieved secret from vault"

-
this print out the retrieved secret from vault

-
 .. ok
cmd( 4):
echo "protectedPassword is - {{ "protectedPassword" | fromVault}}"

-
protectedPassword is - my_super_password

-
 .. ok
. ok

```

#### Relevant

* [ref to code](https://github.com/upcmd/up/tree/master/tests/modtests/0014)
* [use vault](https://upcmd.netlify.app/security/c0201/)
* [retrieve value from vault](https://upcmd.netlify.app/security/c0204/)
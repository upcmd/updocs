---
title: "module repo auth"
date: 2020-06-25T22:32:46+11:00
draft: false
weight: 2131
---

#### Use auth for private repo

You may use a private repo for UPcmd module, then you will have to config the auth: username and password

##### Global credentials

In most of the cases, you will probably just use a global username and password for all your modules, as they are likely to be hosted in your organisation

In this case, set below values in upconfig.yml

`
ModRepoUsernameRef: GIT_USERNAME
ModRepoPasswordRef: GIT_PASSWORD
`

These two values refer to two env vars: GIT_USERNAME and GIT_PASSWORD

They expect in your CI build environment, there are preset values like below: 

`
export GIT_USERNAME='something'
export GIT_PASSWORD='a_password'
`

##### Individual credentials for each repo of the module

`
  - repo: https://github.com/upcmd/auth_test_module.git
    alias: hello
    #ref to an env var name
    #this example shows it obtains a pre set credential from current shell context
    UsernameRef: AUTH_TEST_MODULE_GIT_USERNAME
    PasswordRef: AUTH_TEST_MODULE_GIT_PASSWORD
    PullPolicy: always
`

Same as the above, the two values of UsernameRef and PasswordRef are pointing to two env vars:

`
    AUTH_TEST_MODULE_GIT_USERNAME
    AUTH_TEST_MODULE_GIT_PASSWORD
`

Just set them in your env context

##### Set the secure env context for the credentials

Please ref to the modtest example 14

`
dvars:
  - name: enc_key
    value: my_enc_key
    flags:
      - secret

  - name: github_password
    value: '{{ "something_secret" | encryptAES .enc_key }}'
    flags:
      - v

  - name: GITHUB_USERNAME
    value: my_github_username
    flags:
      - envVar
      - v

  - name: GITHUB_PASSWORD_ENCRYPTED
    value: '{{.github_password}}'
    flags:
      - secure
      - envVar
      - v
`

This shows you could have an already encrypted env var: GITHUB_PASSWORD_ENCRYPTED, which was used in repo:

`
  - repo: https://github.com/upcmd/auth_test_module.git
    alias: hi
    #ref to an env var name
    #this example shows it obtain the configured credential from up.yml in secure context
    UsernameRef: GITHUB_USERNAME
    PasswordRef: GITHUB_PASSWORD_ENCRYPTED
    PullPolicy: always

`

##### Log

`
Ξ /up-project/up git:(master) ▶ mpullx 0014
loading [Config]:  ./tests/modtests/0014/upconfig.yml
Main config:
             Version -> 1.0.0
              RefDir -> ./tests/modtests/0014
             WorkDir -> refdir
          AbsWorkDir -> /up-project/up/tests/modtests/0014
            TaskFile -> up.yml
             Verbose -> v
          ModuleName -> self
           ShellType -> /bin/sh
       MaxCallLayers -> 8
             Timeout -> 3600000
 MaxModuelCallLayers -> 256
           EntryTask -> 
  ModRepoUsernameRef -> GIT_USERNAME
  ModRepoPasswordRef -> GIT_PASSWORD
work dir: /up-project/up/tests/modtests/0014
loading [Task]:  ./up.yml
module: [self], instance id: [dev], exec profile: []
dvar> github_password:
"something"
dvar> GITHUB_USERNAME:
"something"

-
stephencheng
dvar> GITHUB_PASSWORD_ENCRYPTED:
"something"

-validate all modules:
-pull repos:
loading [Config]:  /up-project/up/tests/modtests/0014/upconfig.yml

-main direct modules:
- repo: https://github.com/upcmd/auth_test_module.git
  tag: ""
  version: ""
  alias: hello
  dir: ""
  subdir: ""
  iid: ""
  pullpolicy: always
  usernameref: AUTH_TEST_MODULE_GIT_USERNAME
  passwordref: AUTH_TEST_MODULE_GIT_PASSWORD
- repo: https://github.com/upcmd/auth_test_module.git
  tag: ""
  version: ""
  alias: hi
  dir: ""
  subdir: ""
  iid: ""
  pullpolicy: always
  usernameref: AUTH_TEST_MODULE_GIT_USERNAME
  passwordref: GITHUB_PASSWORD_ENCRYPTED

+------------+-----------------------------------------------+
|  PROPERTY  |                     VALUE                     |
+------------+-----------------------------------------------+
| alias      | hello                                         |
| dir        | .upmodules/hello@master                       |
| repo       | https://github.com/upcmd/auth_test_module.git |
| version    | master                                        |
| pullpolicy | always                                        |
| instanceid | nonamed                                       |
| subdir     |                                               |
+------------+-----------------------------------------------+
removing .upmodules/hello@master ...Enumerating objects: 3, done.
Counting objects: 100% (3/3), done.
Compressing objects: 100% (2/2), done.
Total 3 (delta 0), reused 3 (delta 0), pack-reused 0
checkout version
checkout version: master ...
git checkout master
Already on 'master'
Your branch is up to date with 'origin/master'.
+------------+-----------------------------------------------+
|  PROPERTY  |                     VALUE                     |
+------------+-----------------------------------------------+
| alias      | hi                                            |
| dir        | .upmodules/hi@master                          |
| repo       | https://github.com/upcmd/auth_test_module.git |
| version    | master                                        |
| pullpolicy | always                                        |
| instanceid | nonamed                                       |
| subdir     |                                               |
+------------+-----------------------------------------------+
removing .upmodules/hi@master ...Enumerating objects: 3, done.
Counting objects: 100% (3/3), done.
Compressing objects: 100% (2/2), done.
Total 3 (delta 0), reused 3 (delta 0), pack-reused 0
checkout version
checkout version: master ...
git checkout master
Already on 'master'
Your branch is up to date with 'origin/master'.
 WARN: [config file does not exist] - [use builtin defaults]
+------------+-----------------------------------------------+
|  PROPERTY  |                     VALUE                     |
+------------+-----------------------------------------------+
| alias      | hello                                         |
| dir        | .upmodules/hello@master                       |
| repo       | https://github.com/upcmd/auth_test_module.git |
| version    | master                                        |
| pullpolicy | always                                        |
| instanceid | nonamed                                       |
| subdir     |                                               |
+------------+-----------------------------------------------+
removing .upmodules/hello@master ...Enumerating objects: 3, done.
Counting objects: 100% (3/3), done.
Compressing objects: 100% (2/2), done.
Total 3 (delta 0), reused 3 (delta 0), pack-reused 0
checkout version
checkout version: master ...
git checkout master
Already on 'master'
Your branch is up to date with 'origin/master'.
 WARN: [config file does not exist] - [use builtin defaults]

`

#### Relevant

#### Environment variables

* [ref to example](https://github.com/upcmd/up/tree/master/tests/modtests/0014)

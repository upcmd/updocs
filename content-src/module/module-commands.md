---
title: "module commands"
date: 2020-06-25T22:32:46+11:00
draft: false
weight: 2120
---

#### Manage modules using mod command

#### List modules

```
Ξ modtests/0008 ▶ up mod list
loading [Config]:  ./upconfig.yml
Main config:
Version -> 1.0.0
RefDir -> .
WorkDir -> cwd
TaskFile -> up.yml
Verbose -> v
ModuleName -> focused_engelbart0
MaxCallLayers -> 8
work dir: ./tests/modtests/0008
loading [Task]:  ./up.yml
module: [focused_engelbart0] instance id: [nonamed]
-list all modules:
+-----+--------------+------------------------+-------------------------------------------+------------------------------------------+------------+--------+
| IDX |    ALIAS     |          DIR           |                   REPO                    |                 VERSION                  | INSTANCEID | SUBDIR |
+-----+--------------+------------------------+-------------------------------------------+------------------------------------------+------------+--------+
|   1 | hello        | .upmodule/hello        | https://github.com/upcmd/hello-module.git | new_feature_br                           | nonamed    |        |
|   2 | hello-dummy1 | .upmodule/hello-dummy1 | https://github.com/upcmd/app-server.git   | release_v2.0                             | nonamed    |        |
|   3 | hello-dummy2 | .upmodule/hello-dummy2 | https://github.com/upcmd/database.git     | 25456bbcd17db524d1148e42bdcc3bb36ce90042 | nonamed    |        |
+-----+--------------+------------------------+-------------------------------------------+------------------------------------------+------------+--------+

```

#### update modules

update module is to pull the module repo from the defined url. If the version is defined, eg a branch name, a tag or a commit SHA, then it will checkout that specific version 

```

modtests/0008 git:▶ up mod update
loading [Config]:  ./upconfig.yml
Main config:
             Version -> 1.0.0
              RefDir -> .
             WorkDir -> cwd
            TaskFile -> up.yml
             Verbose -> v
          ModuleName -> goofy_almeida8
       MaxCallLayers -> 8
work dir: ./tests/modtests/0008
loading [Task]:  ./up.yml
module: [goofy_almeida8] instance id: [nonamed]
-pull repos:
pull repo
+------------+-------------------------------------------+
|  PROPERTY  |                   VALUE                   |
+------------+-------------------------------------------+
| alias      | hello                                     |
| dir        | .upmodule/hello                           |
| repo       | https://github.com/upcmd/hello-module.git |
| version    | v2                                        |
| pullpolicy | skip                                      |
| instanceid | nonamed                                   |
| subdir     |                                           |
+------------+-------------------------------------------+
 WARN: [module repo exist: skipped] - [repo: [.upmodule/hello]]
checkout version
 WARN: [Locked version differs, use locked version] - [locked: 25456bbcd17db524d1148e42bdcc3bb36ce90042, configured: v2]
checkout version: 25456bbcd17db524d1148e42bdcc3bb36ce90042 ...
git checkout 25456bbcd17db524d1148e42bdcc3bb36ce90042
HEAD is now at 25456bb v2 feature
pull repo
+------------+-------------------------------------------+
|  PROPERTY  |                   VALUE                   |
+------------+-------------------------------------------+
| alias      | hello-dummy1                              |
| dir        | .upmodule/hello-dummy1                    |
| repo       | https://github.com/upcmd/hello-module.git |
| version    | master                                    |
| pullpolicy | always                                    |
| instanceid | nonamed                                   |
| subdir     |                                           |
+------------+-------------------------------------------+

removing .upmodule/hello-dummy1 ...Enumerating objects: 9, done.
Counting objects: 100% (9/9), done.
Compressing objects: 100% (4/4), done.
Total 9 (delta 2), reused 9 (delta 2), pack-reused 0
checkout version
 WARN: [Locked version differs, use locked version] - [locked: e8e9dc5dc913e211fdaa3bf1bd8a6618e602e8ff, configured: master]
checkout version: e8e9dc5dc913e211fdaa3bf1bd8a6618e602e8ff ...
git checkout e8e9dc5dc913e211fdaa3bf1bd8a6618e602e8ff
Note: switching to 'e8e9dc5dc913e211fdaa3bf1bd8a6618e602e8ff'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by switching back to a branch.

HEAD is now at e8e9dc5 updated

pull repo
+------------+-------------------------------------------+
|  PROPERTY  |                   VALUE                   |
+------------+-------------------------------------------+
| alias      | hello-dummy2                              |
| dir        | .upmodule/hello-dummy2                    |
| repo       | https://github.com/upcmd/hello-module.git |
| version    | 25456bbcd17db524d1148e42bdcc3bb36ce90042  |
| pullpolicy | always                                    |
| instanceid | nonamed                                   |
| subdir     |                                           |
+------------+-------------------------------------------+


removing .upmodule/hello-dummy2 ...Enumerating objects: 9, done.
Counting objects: 100% (9/9), done.
Compressing objects: 100% (4/4), done.
Total 9 (delta 2), reused 9 (delta 2), pack-reused 0
checkout version
 WARN: [Locked version differs, use locked version] - [locked: e8e9dc5dc913e211fdaa3bf1bd8a6618e602e8ff, configured: 25456bbcd17db524d1148e42bdcc3bb36ce90042]
checkout version: e8e9dc5dc913e211fdaa3bf1bd8a6618e602e8ff ...
git checkout e8e9dc5dc913e211fdaa3bf1bd8a6618e602e8ff
Note: switching to 'e8e9dc5dc913e211fdaa3bf1bd8a6618e602e8ff'.

You are in 'detached HEAD' state. 

HEAD is now at e8e9dc5 updated

```

#### lock modules

Once you complete your development and tests, if you are happy with the task implementation from the module, you can lock the modules. By doing so, there will be a modlock.yml file created, for your future update operation, if there is a configuration entry in your configuration:

```
ModuleLock: true
```   

then the module update will respect this lock flag, the module update and repo pull operation will check out and use that specific locked version for you

```

Ξ modtests/0008 git ▶ up mod lock  
loading [Config]:  ./upconfig.yml
Main config:
             Version -> 1.0.0
              RefDir -> .
             WorkDir -> cwd
            TaskFile -> up.yml
             Verbose -> v
          ModuleName -> suspicious_hodgkin1
       MaxCallLayers -> 8
work dir: ./tests/modtests/0008
loading [Task]:  ./up.yml
module: [suspicious_hodgkin1] instance id: [nonamed]
-lock repos:
+------------+-------------------------------------------+
|  PROPERTY  |                   VALUE                   |
+------------+-------------------------------------------+
| alias      | hello                                     |
| dir        | .upmodule/hello                           |
| repo       | https://github.com/upcmd/hello-module.git |
| version    | v2                                        |
| pullpolicy | skip                                      |
| instanceid | nonamed                                   |
| subdir     |                                           |
+------------+-------------------------------------------+
+------------+-------------------------------------------+
|  PROPERTY  |                   VALUE                   |
+------------+-------------------------------------------+
| alias      | hello-dummy1                              |
| dir        | .upmodule/hello-dummy1                    |
| repo       | https://github.com/upcmd/hello-module.git |
| version    | master                                    |
| pullpolicy | always                                    |
| instanceid | nonamed                                   |
| subdir     |                                           |
+------------+-------------------------------------------+
+------------+-------------------------------------------+
|  PROPERTY  |                   VALUE                   |
+------------+-------------------------------------------+
| alias      | hello-dummy2                              |
| dir        | .upmodule/hello-dummy2                    |
| repo       | https://github.com/upcmd/hello-module.git |
| version    | 25456bbcd17db524d1148e42bdcc3bb36ce90042  |
| pullpolicy | always                                    |
| instanceid | nonamed                                   |
| subdir     |                                           |
+------------+-------------------------------------------+
versions:
{
  "hello-dummy1": "e8e9dc5dc913e211fdaa3bf1bd8a6618e602e8ff",
  "hello-dummy2": "e8e9dc5dc913e211fdaa3bf1bd8a6618e602e8ff",
  "hello": "25456bbcd17db524d1148e42bdcc3bb36ce90042"
}

Please check in: [modlock.yml] into code repo     

```


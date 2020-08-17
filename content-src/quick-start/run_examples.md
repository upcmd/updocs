---
title: "how to run the examples"
date: 2020-06-25T22:32:46+11:00
draft: false
weight: 320
---

#### How to run all the examples

The examples are located: ./tests/functests or ./tests/modtests

##### Run example of the regular example

In you bash/zsh, load the funcs.rc, which contains a function called run_example, shown as below:
  
* funcs.rc:
  
```

run_example(){
echo """syntax: run_example case_file_name verbose_level
example:
  run_example c0033 v
  run_example c0033 vvv
"""

  if [ "$1" == "" ];then
    echo "please input the casename, eg c0033, which could be located at ./tests/functests"
  else

    if [ "$2" == "" ];then
      verbose=v
    else
      verbose=$2
    fi

  up ngo task -d ./tests/functests -t $1 --configdir=./tests/functests -v $verbose
  fi
}

```

* load the func

```
. funcs.rc
```

* run the example

```

Ξ /up-project/up git:(master) ▶ . ./funcs.rc
Ξ /up-project/up git:(master) ▶ run_example   
syntax: run_example case_file_name verbose_level
example:
  run_example c0033 v
  run_example c0033 vvv

please input the casename, eg c0033, which could be located at ./tests/functests

Ξ /up-project/up git:(master) ▶ run_example c0033
syntax: run_example case_file_name verbose_level
example:
  run_example c0033 v
  run_example c0033 vvv

loading [Config]:  ./tests/functests/upconfig.yml
Main config:
             Version -> 1.0.0
              RefDir -> ./tests/functests
             WorkDir -> cwd
          AbsWorkDir -> /up-project/up
            TaskFile -> c0033
             Verbose -> v
          ModuleName -> self
           ShellType -> /bin/sh
       MaxCallLayers -> 8
             Timeout -> 3600000
 MaxModuelCallLayers -> 256
work dir: /up-project/up
-exec task: task
loading [Task]:  ./tests/functests/c0033
module: [self], instance id: [nonamed], exec profile: []
Task1: [task ==> task:  ]
-Step1:
cmd( 1):
1.school -> address:
  state: NSW
  city: sydney
  suburb:
    name: sydney
    postcode: 2000
    CBD: LOCAL
  school: Sydney Grammar
principal: Mr Right

 .. ok
cmd( 2):
2.school object CBD -> LOCAL
 .. ok
cmd( 3):
3.school object-> Sydney Grammar
 .. ok
cmd( 4):
4.school object-> Mr Right
 .. ok
. ok
Ξ /up-project/up git:(master) ▶ run_example c0033 vvv
syntax: run_example case_file_name verbose_level
example:
  run_example c0033 v
  run_example c0033 vvv

loading [Config]:  ./tests/functests/upconfig.yml
Main config:
             Version -> 1.0.0
              RefDir -> ./tests/functests
             WorkDir -> cwd
          AbsWorkDir -> /up-project/up
            TaskFile -> c0033
             Verbose -> vvv
          ModuleName -> self
           ShellType -> /bin/sh
       MaxCallLayers -> 8
             Timeout -> 3600000
 MaxModuelCallLayers -> 256
work dir: /up-project/up
-exec task: task
loading [Task]:  ./tests/functests/c0033
module: [self], instance id: [nonamed], exec profile: []
profile -  envVars:

(*core.Cache)({
})

Task1: [task ==> task:  ]
-Step1:
self: final context exec vars:

(*core.Cache)({
  "school_object": {
    "principal": "Mr Right",
    "address": {
      "state": "NSW",
      "city": "sydney",
      "suburb": {
        "name": "sydney",
        "postcode": 2000,
        "CBD": "LOCAL"
      },
      "school": "Sydney Grammar"
    }
  },
  "student": {
    "address": {
      "school": "Sydney Grammar",
      "suburb": {
        "CBD": true,
        "name": "sydney",
        "postcode": 2000
      }
    },
    "name": "Tom",
    "gender": "Male"
  },
  "a": "local-a",
  "b": "local-b",
  "school": "address:\n  state: NSW\n  city: sydney\n  suburb:\n    name: sydney\n    postcode: 2000\n    CBD: LOCAL\n  school: Sydney Grammar\nprincipal: Mr Right\n"
})

cmd( 1):
echo """1.school -> {{.school}}"""

1.school -> address:
  state: NSW
  city: sydney
  suburb:
    name: sydney
    postcode: 2000
    CBD: LOCAL
  school: Sydney Grammar
principal: Mr Right

 .. ok
cmd( 2):
echo """2.school object CBD -> {{.school_object.address.suburb.CBD}}"""

2.school object CBD -> LOCAL
 .. ok
cmd( 3):
echo """3.school object-> {{.school_object.address.school}}"""

3.school object-> Sydney Grammar
 .. ok
cmd( 4):
echo """4.school object-> {{.school_object.principal}}"""

4.school object-> Mr Right
 .. ok
. ok

```


##### Run example of the module example

* funcs.rc:

```
run_module_example(){

echo """syntax: run_module_example case_name verbose_level
example:
  run_module_example 0003 v
  run_module_example 0003 vvv
"""

  if [ "$1" == "" ];then
    echo "please input the module casename(dir name) located at ./tests/modtests"
  else

    if [ "$2" == "" ];then
      verbose=v
    else
      verbose=$2
    fi

    dir=./tests/modtests/$1
    up ngo -d $dir --configdir=$dir -w refdir -v $verbose
  fi
}


```

* load the func

```
. funcs.rc
```

* run the module example

```

run_module_example 0001

run_module_example 0008 vvv

```
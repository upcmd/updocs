---
title: "prompt in taskScope"
date: 2020-06-25T22:32:46+11:00
draft: false
weight: 1785
---

#### Receive user input

If your task need to receive some value from user input, you can use the dvar with prompt flag

###### Task

```

tasks:

  -
    name: task
    task:
      -
        func: shell
        desc: ask about the faketime for this commit, eg [167 days ago 10:30 pm]
        dvars:
          - name: fist_name
            flags:
              - prompt
              - taskScope

          - name: last_name
            value: hanks
            flags:
              - taskScope
        do:
          - |
            echo """my name is: {{.fist_name}} {{.last_name}}"""

      -
        func: cmd
        do:
          - name: print
            cmd: 'my name is: {{.fist_name}} {{.last_name}}'


```

###### Log

```
▶  up ngo task -d ./tests/functests -t p0150 -i dev --configdir=./tests/functests 
syntax: uptestx c0033
loading [Config]:  ./tests/functests/upconfig.yml
Main config:
             Version -> 1.0.0
              RefDir -> ./tests/functests
             WorkDir -> cwd
            TaskFile -> p0150
             Verbose -> v
       MaxCallLayers -> 8
 MaxModuelCallLayers -> 256
-exec task: task
loading [Task]:  ./tests/functests/p0150
module: [berserk_rosalind3] instance id: [dev]
Task1: [task ==> task:  ]
-Step1: [: ask about the faketime for this commit, eg [167 days ago 10:30 pm] ]
Enter Value For fist_name: 
This will be saved as fist_name's value
tom
cmd( 1):
my name is: tom hanks
 .. ok
. ok
-Step2:
~SubStep1: [print:  ]
my name is: tom hanks
```

This shows that you are ask to provide a value of dvar first_name, the input value is "tom", so the first_name is now assigned value of tom, since the dvar is flaged with taskScope, it means the value of first_name is accessible accross different funcs in this task
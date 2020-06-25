---
title: "user prompt"
date: 2020-06-25T22:32:46+11:00
draft: false
weight: 1785
---

#### Receive user input

If your task need to receive some value from user input, you can use the dvar with prompt flag

###### Task

```

dvars:
  - name: student
    flags: [prompt,]

tasks:
  -
    name: task
    task:

      - func: shell
        do:
          - echo "hello {{.student}}"

```

###### Log

```
▶  up ngo task -d ./tests/functests -t p0057 -i dev --configdir=./tests/functests 
loading [Config]:  ./tests/functests/upconfig.yml
Main config:
             Version -> 1.0.0
              RefDir -> ./tests/functests
             WorkDir -> cwd
            TaskFile -> p0057
             Verbose -> v
       MaxCallLayers -> 8
 MaxModuelCallLayers -> 256
-exec task: task
loading [Task]:  ./tests/functests/p0057
module: [determined_colden1] instance id: [dev]
Enter Value For student:
This will be saved as student's value
tom hanks
Task1: [task ==> task:  ]
-Step1:
cmd( 1):
hello tom hanks
 .. ok
. ok

```

This shows that you are ask to provide a value of dvar student, the input value is "tom hanks", so the student is now assigned value of hanks
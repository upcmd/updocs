---
title: "chained pipein from stdin"
date: 2020-06-25T22:32:46+11:00
draft: false
weight: 1786
---

#### Pipe in data

You can use UPcmd to chain the command output as input and choose a task to handle the data

This case shows that the output of a restapi call to http://httpbin.org/get will be the a pipe in input to UPcmd, then the result is automatically saved to register name: up_runtime_task_pipe_in_content

Initially the input result is always a string, then we use toObj cmd to auto convert it to a object named my_http_get_response, then we will be able to access the whole object

Please note that the command will use the assigned task name for execution, however it is up to the user to design the workflow to decide how and what to use the register pipein value

#### extended use case
You could use UPcmd in following way:

* register the input and parse it to an object, process it and pass it on for your task
* chain it through to another UPcmd, eg.
```
  cmd1 | up ngo task1 | up ngo task2 | cmd2 | up ngo task3 | ......
```

It is not recommend to chain directly to another UPcmd as you could route it internally.

###### Task

```yaml

tasks:
  -
    name: task
    desc: process the stdin pipe-in data
    task:

      -
        func: cmd
        do:
          - name: inspect
            cmd:
              - exec_vars

          - name: print
            desc: this is the pipe in string content
            cmd: '{{.up_runtime_task_pipe_in_content}}'

          - name: toObj
            cmd:
              reg: my_http_get_response
              fromkey: up_runtime_task_pipe_in_content

          - name: printObj
            desc: this is the pipe in object
            cmd: my_http_get_response

          - name: print
            desc: access the object
            cmd: |
              "headers": {
                User-Agent: {{index .my_http_get_response.headers "User-Agent"}},
                "X-Amzn-Trace-Id": "{{index .my_http_get_response.headers "X-Amzn-Trace-Id"}}",
                "Accept": "{{.my_http_get_response.headers.Accept}}",
                "Host": "{{.my_http_get_response.headers.Host}}",
              }

```

###### Log

```
▶ curl -X GET "http://httpbin.org/get" -H "accept: application/json" |uptestx c0152
syntax: uptestx c0033
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   268  100   268    0     0    384      0 --:--:-- --:--:-- --:--:--   383
loading [Config]:  ./tests/functests/upconfig.yml
Main config:
             Version -> 1.0.0
              RefDir -> ./tests/functests
             WorkDir -> cwd
          AbsWorkDir -> /up-project/up
            TaskFile -> c0152
             Verbose -> v
          ModuleName -> self
           ShellType -> /bin/sh
       MaxCallLayers -> 8
 MaxModuelCallLayers -> 256
work dir: /up-project/up
-exec task: task
loading [Task]:  ./tests/functests/c0152
module: [self] instance id: [dev]
Task1: [task ==> task: process the stdin pipe-in data ]
-Step1:
~SubStep1: [inspect:  ]
 1: inspect[exec_vars](*core.Cache)({
  "up_runtime_task_pipe_in_content": "{\n  \"args\": {}, \n  \"headers\": {\n    \"Accept\": \"application/json\", \n    \"Host\": \"httpbin.org\", \n    \"User-Agent\": \"curl/7.54.0\", \n    \"X-Amzn-Trace-Id\": \"Root=1-5f0c77f1-ebf4ac10f454017069691ef0\"\n  }, \n  \"origin\": \"118.208.99.251\", \n  \"url\": \"http://httpbin.org/get\"\n}\n"
})

~SubStep2: [print: this is the pipe in string content ]
{
  "args": {}, 
  "headers": {
    "Accept": "application/json", 
    "Host": "httpbin.org", 
    "User-Agent": "curl/7.54.0", 
    "X-Amzn-Trace-Id": "Root=1-5f0c77f1-ebf4ac10f454017069691ef0"
  }, 
  "origin": "118.208.99.251", 
  "url": "http://httpbin.org/get"
}

~SubStep3: [toObj:  ]
~SubStep4: [printObj: this is the pipe in object ]
object:
 my_http_get_response: {
  "origin": "118.208.99.251",
  "url": "http://httpbin.org/get",
  "args": {
  },
  "headers": {
    "X-Amzn-Trace-Id": "Root=1-5f0c77f1-ebf4ac10f454017069691ef0",
    "Accept": "application/json",
    "Host": "httpbin.org",
    "User-Agent": "curl/7.54.0"
  }
}

~SubStep5: [print: access the object ]
"headers": {
  User-Agent: curl/7.54.0,
  "X-Amzn-Trace-Id": "Root=1-5f0c77f1-ebf4ac10f454017069691ef0",
  "Accept": "application/json",
  "Host": "httpbin.org",
}
```


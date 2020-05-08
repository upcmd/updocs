---
title: "list tasks"
date: 2020-02-07T14:32:46+11:00
draft: false
weight: 102
---

#### list all available tasks

```

Ξ ▶ up list           

```

example with detailed args:

```
# up list -d ./tests/functests -t c0116.yml -i dev --configdir=./tests/functests

loading [Config]:  ./tests/functests/upconfig
loading [Task]:  ./tests/functests/c0116.yml
instance id: dev
-task list
     1|     task: main entry
     2| subtask1: subtask to test reg and return
     3| subtask2: subtask to test reg and return
```

#### list a specific task

```

Ξ ▶ up list taskname

```

example with detailed args:

```
# up list task -d ./tests/functests -t c0116.yml -i dev --configdir=./tests/functests

loading [Config]:  ./tests/functests/upconfig
loading [Task]:  ./tests/functests/c0116.yml
instance id: dev
task: main entry.
├── step1: check value of tom after it is registered in current task stack
├── [step2 /loop..]  call subtask and exam the return value in following steps
│   └── [subtask1]  subtask to test reg and return
│       ├── : check value of tom after it is registered in current task stack
│       ├── : check value of tom and it should be available in current stack
│       ├── []  call subtask and exam the return value in following steps
│       │   └── [subtask2]  subtask to test reg and return
│       │       ├── : check value of tom after it is registered in current task stack
│       │       └── : check value of tom and it should be available in current stack
│       └── : check value of tom and it should be available in current stack
└── : check value of tom

```


#### list all tasks in details

```

Ξ ▶ up list =           

```

example with detailed args:

```
up list = -d ./tests/functests -t c0116.yml -i dev --configdir=./tests/functests
loading [Config]:  ./tests/functests/upconfig
loading [Task]:  ./tests/functests/c0116.yml
instance id: dev
-inspect all tasks:
task: main entry.
├── step1: check value of tom after it is registered in current task stack
├── [step2 /loop..]  call subtask and exam the return value in following steps
│   └── [subtask1]  subtask to test reg and return
│       ├── : check value of tom after it is registered in current task stack
│       ├── : check value of tom and it should be available in current stack
│       ├── []  call subtask and exam the return value in following steps
│       │   └── [subtask2]  subtask to test reg and return
│       │       ├── : check value of tom after it is registered in current task stack
│       │       └── : check value of tom and it should be available in current stack
│       └── : check value of tom and it should be available in current stack
└── : check value of tom

subtask1: subtask to test reg and return.
├── : check value of tom after it is registered in current task stack
├── : check value of tom and it should be available in current stack
├── [ /loop..]  call subtask and exam the return value in following steps
│   └── [subtask2]  subtask to test reg and return
│       ├── : check value of tom after it is registered in current task stack
│       └── : check value of tom and it should be available in current stack
└── : check value of tom and it should be available in current stack

subtask2: subtask to test reg and return.
├── : check value of tom after it is registered in current task stack
└── : check value of tom and it should be available in current stack

```


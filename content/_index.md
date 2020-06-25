---
title: "up cmd"
date: 2020-06-25T22:32:46+11:00
draft: false
---

# Up Command

## UP  -       The Ultimate Provisioner

UP is designed and implemented to address some of the common problems of:

  * configuration management
  * build, continuously delivery, integration with ci/cd

It is a build tool with some features learnt from using Ansible, Make, Rake, Ant, Gradle, Puppet, Taskfile etc, but it is a little smarter to try to make things a lot easier.

```
tasks:
  -
    name: task
    task:
      -
        func: shell
        do:
          - echo "hello, world"
```


###  Why yet another build tool

* Make was initially designed and used for building C program, even though it could be adopted for other purpose, some of the hard to learn trivials offten cause problems than the benefits added to the team, and it is burning your brain. It is hard to make automation task a little more complex, readbility degrades rapidly and it is risky to implement critical logic using Make. Make is just a little old for modern business requirements.

* Rake is smart and powerful. If you don't mind learning a little bit of Ruby, it is a good choice of building tool. Similarly to Ant, Gradle. They are all bind to a language specific, it is just not right when it comes to the case that you want to automate things in cloud environment. In most of cases when it requires automation in a cloud environment, in a given spun up AWS EC2 instance, a shell session, a kubernete pod, you would want some thing just works without any dependencies. You simply do not want to mantain the consistency of chain of upgrding path for all language pkgs in multiple environments. In these cases, Rake, Gradle, Ant are not best options.

* Ansible, Puppet are configuration tools. They are powerful, there are many builtin well tested modules you could use. However they are problem just too huge for little job and most of the time it tends to over kill.

A common usage of Ansible for many companies is to use the local ssh execution with group/host vars for automation, which is simply not right. Also the way the vars being managed is not fine grained. The ansible role as a reusable module is not flexible to implement more complicated tasks.

* It comes to my mind the tool like taskfile: https://taskfile.dev/ is kind of tiny tool make build and automation a lot cleaner and elegant, however it lacks some of the features I wanted in a practical cloud environment for CI/CD, devops automation, hence this project is born for that purpose


### Goal

The goal of UP is to provide a quick (I'd say the quickest) solution to enable continuously integration and continuously deployment (CI/CD). It is simple to use and yet powerful to achieve many common challenges nowadays devops guys are facing in a Cloud environment

It is designed with very well consideration of collaboration with automation in Kubernetes, helm charts, api call

It is also put best practice of integration with common CI/CD tools, such as GOCD, Jenkins, Drone, Gitlab CI and so on

It is bringing a DSL programming into CLI and enable OO design and fast test driven development and delivery cycle     

### Features

1. Drop in replacement for Makefile, but way more powerful. It uses a composition model rather than dependency model for flexibility/composibility
2. Implemented in golang, so no dependency hell, no maintenance of runtime and ensure the version consistency across multiple/many execution contexts
3. Use scopes to manage group of environments, the variables associated with the scope. Fine grained scoping model to support variable auto overriding/merging. Similar to Ansible global/group vars, host vars, but more powerful to support leaf level object auto merging
4. Use dvar - dynamic var, a special design to achieve many incredible tasks, for example:
  * manage security: encrypt/decrypt
  * dynamics on dynamics: it allows you to specify how many layers of expansion you'd like for a pointing variable
  * builtin templating capability
  * use golang template, supporting all(220+) (builtin|sprig|gtf) funcs/pipeline so that your configuration could be well controlled in template using objects
  * auto conversion of yaml result to object
  * conform the hierarchical scoping model for var merging to leaf level
  * manage setup/read env vars in the same scoping model so that you could have seamless integration with minimal exposed demanding ENV vars from CD/CI tool
  * auto validation for mandatory vars
5. Color print and adjustable verbose level
6. Flexible programming model to allow you to separate implementation with interface so that common code could be reused via task_ref
Allow empty skeleton to be laid for testing driving process or guide as seudo code, but fill in the details and implementation gradually
7. Flow control:
  * ignore_error
  * dry run
  * if condition support
  * loop support to iterate through a list of items
8. Flexible configuration to load dvar, scope, flow from external yaml so that the programming code will be a little cleaner and organised
9. Support the unlimited yml object, so yml config in var is text and it is also object.It could be merged in scopes automatically, it could be processed using go template
10. Battery included for common builtin commands: print, reg, dereg, template, readfile, writefile
11. Builtin yml liter and object query, modification
12. Call func is really shining powerful design to be used:
    * Compose the sequential execution of block of code
    * Use the stack design, so it segregates all its local vars so that the vars used in its implementation will not pollute the caller's vars
    * It serves like a interface to separates the goal and implementation and makes the code is reusable  

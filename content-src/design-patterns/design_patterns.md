---
title: "design patterns"
date: 2020-02-07T14:32:46+11:00
draft: false
weight: 1002
---

#### Classic design patterns

#### How do I construct a dependency chain and manage the vars scopes 

Simply use call and put the tasks into the array list to chain them through

The example below shows that the private vars scope will be shared and common for all callee tasks

In this case, the combination of call and detailed callee task implementation would create a scope manageable via firstly overriding global runtime vars, then pass them through to inject needed vars as args to callee tasks 

```
tasks:
  -
    name: Main
    task:
      -
        func: call
        vars:
          a: local-var-a
          b: local-var-b
        do: 
          - task1        
          - task2        
```

So the task1 could obtain three types of inputs:

1. global merged runtime vars(including vars from scopes) passing down from call func
2. overridden vars: a and b
3. declared local private vars

So the do action could make use of implicit global vars and local private vars, the behavior could also be controlled by injected caller vars

#### Best practice on organising vars

1. To make your code auto aware the context, always put them into scope
    * if it is common shared one, put into global scope,
    * use context group for group scope, eg, nonprod group with members of: dev, staging etc
    * the group scope will automatically merge with global scope with more detailed settings 
    * during the runtime, specify a instanceid, if it falls into a scope group, it will automatically load that vars from that scope group
    
2. Putting the vars to different layers is flexible, the basic guideline is to start it with vars declaration in global runtime, eg 

```
vars:
  a: aa

tasks:
  -
    name: Main
    task:
      dox:
```

The benefit doing this is that you can lift them up to scope profile group, then global group, or if it is more specific to a task, then define them directly in func, in this way

Or, you could start with all local vars in func, then if you think they are common to all tasks and can be shared across, then lift them up to global runtime scope. This practice normally used for adhoc test, quick mockup etc.

3. There is no task vars declaration tag, like vars in func, or in scope group, but you can use dvar flag to mark a var to be a taskvar scope, then it will be shared in the entire task. The reason not to use task var tag is to reduce complexity and increase the composibilty instead.

4. call func is really really powerful tool. It uses IOC (inverse of controll) to inject needed vars to callee, while overriding and merging global runtime vars.

```

global vars <-- [override and merge]  call func [override] -> callee func [vars injected from call]

```    
 
5. Combine call func with logic, loop and another call func, it provides infinite solutions for solving problems, read detailed call func use cases and demos for comprehensive understandings 

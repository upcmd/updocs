---
title: "call and vars"
date: 2020-06-25T22:32:46+11:00
draft: false
weight: 1001
---

### Different ways of use call

#### call as entry point and local vars are used   

```

tasks:
  -
    name: main
    task:
      -
        func: call
        vars:
          a: local-var-a
          b: local-var-b
        do: something        

```

The above code is essentially equivalent to below pseudo code: 

```

func main(){
  a :=local-var-a
  b :=local-var-b
  dosomething(a,b)
}

```

The execution runtime for main func will be:

```
  a: local-var-a
  b: local-var-b
```


#### call as entry point and local vars override global runtime vars  

```

scopes:
  ..............

vars:
  a: global-var-a
  b: global-var-b

tasks:
  -
    name: Say_world
    task:
      -
        func: call
        vars:
          a: local-var-a
          b: local-var-b
        do: something        

```

The above code is essentially equivalent to below pseudo code: 

```
a:=global-var-a
b:=global-var-b
c:=global-var-c

func main(){
  a :=local-var-a
  b :=local-var-b
  dosomething(a,b,c)
}

```

The execution runtime for main func will be:

```
  a: local-var-a
  b: local-var-b
  c: global-var-c
```

#### call as entry point and local vars override global runtime vars

In this case, the global vars will override the vars merged from scopes definition, we do not go to details about this further  

```

scopes:
  ..............

vars:
  a: global-var-a
  b: global-var-b
  c: global-var-c
tasks:
  -
    name: main
    task:
      -
        func: call
        vars:
          a: local-var-a
          b: local-var-b
        do: something        

```

The above code is essentially equivalent to below pseudo code: 

```
a:=global-var-a
b:=global-var-b
c:=global-var-c

func main(){
  a :=local-var-a
  b :=local-var-b
  dosomething(a,b,c)
}

```

The execution runtime for main func will be:

```
  a: local-var-a
  b: local-var-b
  c: global-var-c
```

#### call as method or reusable unit for detailed implementation

In this case, we talk about the callee task of "action"

```

scopes:
  ..............

vars:
  a: global-var-a
  b: global-var-b
  d: global-var-d

tasks:
  -
    name: Main
    task:
      -
        func: call
        vars:
          a: caller-var-a
          b: caller-var-b
          e: caller-var-e
        do: action        

  -
    name: action
    task:
      -
        func: cmd
        vars:
          a: local-var-a
          b: local-var-b
          c: local-var-c
        do: something        

```

The above code is essentially equivalent to below pseudo code: 

```

func(){
  a=global-var-a
  b=global-var-b
  d=global-var-d
  
  func Main(){
      a=caller-var-a
      b=caller-var-b
      e=caller-var-e

       action:=func(a=local-var-a, b=local-var-a, c=local-var-c){
            dosomething()
       }

      action(a,b,c)  
  }
}()

```

The execution runtime for action func will be:

```
  a: caller-var-a 
  b: caller-var-b 
  c: local-var-c 
  d: global-var-d
  e: caller-var-e
```

How does it work:

* var a's value caller-var-a in Main extends global runtime value global-var-a. then it is used a func arg to call func action
* note that loca1l-var-* in action func served as default values, if there is same name var, eg var a in global runtime or caller's execution runtime, then it will be overridden by caller  


#### call as method or reusable unit in module invoked by external func 

In this case, we focus on the execution runtime vars in the callee task of "action" in module

external caller task:

```

scopes:
  ..............

vars:
  a: external-global-var-a
  b: external-global-var-b
  d: external-global-var-d

tasks:
  -
    name: Main
    task:
      -
        func: call
        vars:
          a: external-caller-var-a
          b: external-caller-var-b
          e: external-caller-var-e
        do: module.action        

```

At this point, the exec runtime for call func:

```

(external_caller_exec_vars):
  a=external-caller-var-a
  b=external-caller-var-b
  e=external-caller-var-e
  d=external-global-var-d
```

callee task in the module

```

scopes:
  ..............

vars: (module_global_merged_vars)
  a: callee-global-var-a
  d: callee-global-var-d
  f: callee-global-var-f
  g: callee-global-var-g

tasks:
  -
    name: action
    task:
      -
        func: cmd
        vars:
          a: local-var-a
          b: local-var-b
          c: local-var-c
        do: something        

```

At this point, the caller func will serve exactly as what it is doing as internal func, it 
    
    1. overrides and merge with the callee's global then
    
```
external_caller_exec_vars override module_global_merged_vars, step1 result:

  a=external-caller-var-a
  b=external-caller-var-b
  e=external-caller-var-e
  d=external-global-var-d
  f=callee-global-var-f
  g=callee-global-var-g

```
    
    2. pass in all vars to override callee's local vars, step2 result and final execution runtime vars:
    
```

  a=external-caller-var-a
  b=external-caller-var-b
  e=external-caller-var-e
  d=external-global-var-d
  f=callee-global-var-f
  g=callee-global-var-g
  c=local-var-c

```


The above code is essentially equivalent to below pseudo code: 

```

func(){
  
  a=callee-global-var-a
  d=callee-global-var-d
  f=callee-global-var-f
  g=callee-global-var-g

  func Main(){
      a=external-caller-var-a
      b=external-caller-var-b
      e=external-caller-var-e
      d=external-global-var-d

      action(a,b,d,e,f)  
  }

  func action(a=local-var-a, b=local-var-a, c=local-var-c){
     dosomething()
  }


}()


```
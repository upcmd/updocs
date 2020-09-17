---
title: "access and register sub object 1"
date: 2020-06-25T22:32:46+11:00
draft: false
weight: 1805
---

## Access sub element and register the object

There are two ways you can do so:

* see dvar instances_1 
* see dvar instances_2 

## Adapt the object for loop

It is little tricky to use correct data type in for loop

* see dvar instances_3 

### UP.yml

Self explained in the desc

```

tasks:
  -
    name: Main
    task:
      -
        func: shell
        desc: |
          this will get a response of:
          Output: {
            "LoadBalancerDescriptions": [
              {
                "LoadBalancerName": "xx-elb",
                "DNSName": "x-y-z.elb.amazonaws.com",
                ...........
                "Instances": [
                  {
                    "InstanceId": "i-1234567890"
                  },
                  {
                    "InstanceId": "i-9876543210"
                  }
                ],
                ...........
              }
            ]
          }
        do: aws elb describe-load-balancers --load-balancer-names my-web-app-elb

      -
        func: cmd
        dvars:
          - name: elb
            value: '{{.last_result.Output}}'
            desc: |
              this will map the reponse to name of elb
              then convert it to an object and register to cache
              the object name would be kept the same called elb, so now elb is a object instead of string content
              it is marked as in taskScope, means that it will be available to be acessible in the next func call in Main task
            flags:
              - keepName
              - toObj
              - reg
              - taskScope

          - name: void
            desc: |
              index .elb.LoadBalancerDescriptions 0 will locate the sub object of Instances
              objToYml template func is chained and called to to convert the objToYml
              then the yml is again converted to an object
              then registered as name of instances_1 which is a object
              the dvar name "void" means it does not register the string value to cached

              * note: instances_1 is a object of type of *interface{}

              "instances_1": (*[]interface {})({
                {
                  "InstanceId": "i-1234567890"
                },
                {
                  "InstanceId": "i-9876543210"
                }
              })

              So you can use access and reference instances_1 in template value, however this internal type will not match what is required for loop tag, even though it looks like it is iteratable

              You can access, reference to sub element and iterate it using the go template though, for example:
              '{{(index .instances_1. 0).Instances}}' => "i-1234567890"
            value: '{{(index .elb.LoadBalancerDescriptions 0).Instances |objToYml|ymlToObj|reg "instances_1"}}'

          - name: void
            desc: |
              regObj is a template func as short hand to register the chain through object into the cache
              the name of the object is instances_2
            value: '{{(index .elb.LoadBalancerDescriptions 0).Instances |regObj "instances_2"}}'

          - name: instances_3
            desc: |
              index .elb.LoadBalancerDescriptions 0 will locate the sub object of Instances
              objToYml template func is chained and called to to convert the objToYml, now the result is yml string
              toObj flag decorate the behavior to convert the yml string to an object and the object name is kept the same as instances_3

              * note the different of instances_3 and ( instances_1 or instances_2) is the internal data type

              instances_3 data type is a plain slice/array and we can use this for the loop tag
              {
                {
                  "InstanceId": "i-1234567890"
                },
                {
                  "InstanceId": "i-9876543210"
                }
              }


            value: '{{(index .elb.LoadBalancerDescriptions 0).Instances | objToYml}}'
            flags:
              - v
              - keepName
              - toObj

        loop: 'instances_3'
        do:
          - name: print
            cmd: '{{.loopitem.InstanceId}}'

```

## Log

```
Task1: [Main ==> Main:  ]
-Step1:
cmd( 1):
-
            "LoadBalancerDescriptions": [
              {
                "LoadBalancerName": "xx-elb",
                "DNSName": "x-y-z.elb.amazonaws.com",
                ...........
                "Instances": [
                  {
                    "InstanceId": "i-1234567890"
                  },
                  {
                    "InstanceId": "i-9876543210"
                  }
                ],
                ...........
              }
            ]
          }
-
 .. ok
. ok
-Step2:
dvar> instances:
"- InstanceId: i-1234567890\n- InstanceId: i-9876543210\n"

-
- InstanceId: i-1234567890
- InstanceId: i-9876543210

dvar[object]> instances:
{
  {
    "InstanceId": "i-1234567890"
  },
  {
    "InstanceId": "i-9876543210"
  }
}

~SubStep1: [print:  ]
i-1234567890
~SubStep1: [print:  ]
i-9876543210
```
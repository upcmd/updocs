---
title: "exec profile example"
date: 2020-06-25T22:32:46+11:00
draft: false
weight: 1788
---

#### Example use case

This example shows that:

1. Create a jenkins stack - the server, the slave in AWS/K8s
2. The execution could be from local testing machine, or GoCD for prod stack creation and Jenkins slave for dev testing
3. We can set up 3 execution profiles correlating to the 3 types of executions 
4. The example shows that you are able to use program control to stop execution to impact prod deployment
5. You can also use env var obtained from either GoCD or jenkins slave build machine to determine what special prior step to run
6. The env vars are scoped into scopes, so they can be shared access for all instances running in same scope group 

###### Task

```yaml

scopes:
  - name: global
    vars:
      my_docker_ep: gitlab/mydocker_repo/jenkins
    dvars:
      - name: ENV
        value: dev1
        flags:
          - envVar

      - name: JENKINS_NAME
        value: '{{ env "JENKINS_NAME" |validateMandatoryFailIfNone "JENKINS_NAME" }}'

      - name: JENKINS_MASTER_DOCKER_IMAGE_URL
        value: '{{.my_docker_ep}}/master'
        flags:
          - envVar

      - name: JENKINS_SLAVE_DOCKER_IMAGE_URL
        value: '{{.my_docker_ep}}/slave'
        flags:
          - envVar

      - name: JENKINS_EXPECTED_VERSION
        value: 2.222.4
        flags:
          - envVar

  - name: nonprod
    members:
      - dev
      - staging
    dvars:
      - name: ASSUME_ROLE_ARN
        value: arn:aws:iam::123456789012:role/my-nonn-prod-iam-role
        flags:
          - envVar
      - name: JENKINS_PVT_USERNAME
        value: svc_go
        flags:
          - envVar

  - name: prod
    members:
      - dr
      - prod
    vars:
      host_alias: prod
    dvars:
      - name: ENV
        value: prod
        flags:
          - envVar
      - name: ASSUME_ROLE_ARN
        value: arn:aws:iam::998654321012:role/my-nonn-prod-iam-role
        flags:
          - envVar

      - name: JENKINS_USERNAME
        value: my_jenkins_prod_user
        flags:
          - envVar

  - name: staging
    dvars:
      - name: ENV
        value: staging
        flags:
          - envVar

dvars:
  - name: JENKINS_URL
    desc: |
      auto render to a specific valid url
    value: '{{ printf "%s-%s.myjenkins.xyz" .JENKINS_NAME .ENV}}'
    flags:
      - envVar

eprofiles:
  - name: my-jenkins-local-test
    desc: |
      a test profile for test in local machine
    instance: staging
    evars:
      - name: JENKINS_NAME
        value: my-jenkins-test

      - name: SKIP_EBS_BACKUP
        value: "true"

  - name: jenkins-prod-gocd
    instance: prod
    desc: |
      a profile for jenkins prod stack creation in GoCD
    evars:
      - name: JENKINS_NAME
        value: jenkins-prod

      - name: SKIP_EBS_BACKUP
        value: "false"

  - name: jenkins-dev
    instance: dev
    desc: |
      all other tests are running in jenkins slave
    evars:
      - name: JENKINS_NAME
        value: dev-jenkins

      - name: SKIP_EBS_BACKUP
        value: "true"

tasks:
  -
    name: task
    desc: main entry
    task:
      - func: cmd
        do:
          - name: inspect
            cmd:
              - exec_vars
              - exec_base_env_vars_configured

          - name: assert
            cmd:
              - '{{eq .JENKINS_NAME "my-jenkins-test" }}'
              - '{{eq .JENKINS_URL "my-jenkins-test-staging.myjenkins.xyz" }}'
              - '{{eq .JENKINS_SLAVE_DOCKER_IMAGE_URL "gitlab/mydocker_repo/jenkins/slave" }}'

      -
        func: shell
        dvars:
          - name: env_export_test
            value: '{{envExport "exec_env_vars_configured" ""}}'
            flags:
              - v
        desc: main job
        do:
          - 'echo """{{.env_export_test}}"""'

      -
        func: shell
        do:
          - echo "demo conditionally quit"
        if: '{{ne .ENV "prod"}}'
        else:
          - func: cmd
            do:
              - name: print
                cmd: not allow to touch prod
              - name: exit

      - func: block
        desc: |
          conditionally exec in gocd / jenkins / local
          obtain the env var from the build environment, it could be either GoCD agent, or a Jenkins slave
          it does not matter if the agent is in a EC2 instance, or a docker, or inside of k8s pod, as long as the env var used as indication is right for your case, below is just for example
        dvars:
          - name: gocd_marker
            value: '{{ env "GOCD_AGENT_VERSION" }}'
            flags:
              - taskScope
              - v
          - name: jenkins_marker
            value: '{{ env "JENKINS_20_AGENT_SERVICE_PORT" }}'
            flags:
              - taskScope
              - v
        if: '{{or (ne .gocd_marker "None") (ne .jenkins_marker "None")}}'
        do:
          -
            func: shell
            desc: |
              run in gocd if it detects there is a GoCD marker
              also it runs the piped in comand as prior script before real provision step
            do: |
              #{{.up_runtime_task_pipe_in_content}}
              echo "make my_jenkins_stack_creation"
            if: '{{(ne .gocd_marker "None") }}'
          -
            func: shell
            desc: |
              run in jenkins
              it use different prior step to assume role
            do: |
              echo "source ./assume_role.sh"
              echo "make my_jenkins_stack_creation"
            if: '{{(ne .jenkins_marker "None")}}'
        else:
          -
            func: shell
            do:
              - echo "starting my real stack creation work"
              - echo "this job is running in my local machine"
              - echo "make my_jenkins_stack_creation"

```

###### Log

```
loading [Config]:  ./tests/functests/upconfig.yml
Main config:
             Version -> 1.0.0
              RefDir -> ./tests/functests
             WorkDir -> cwd
          AbsWorkDir -> /up-project/up
            TaskFile -> wip.yml
             Verbose -> v
          ModuleName -> self
           ShellType -> /bin/sh
       MaxCallLayers -> 8
 MaxModuelCallLayers -> 256
work dir: /up-project/up
-exec task: task
loading [Task]:  ./tests/functests/wip.yml
module: [self], instance id: [staging], exec profile: [my-jenkins-local-test]
Task1: [task ==> task: main entry ]
-Step1:
~SubStep1: [inspect:  ]
 1: inspect[exec_vars]
(*core.Cache)({
  "ASSUME_ROLE_ARN": "arn:aws:iam::123456789012:role/my-nonn-prod-iam-role",
  "envVar_JENKINS_URL": "my-jenkins-test-staging.myjenkins.xyz",
  "JENKINS_URL": "my-jenkins-test-staging.myjenkins.xyz",
  "envVar_JENKINS_SLAVE_DOCKER_IMAGE_URL": "gitlab/mydocker_repo/jenkins/slave",
  "JENKINS_MASTER_DOCKER_IMAGE_URL": "gitlab/mydocker_repo/jenkins/master",
  "envVar_JENKINS_EXPECTED_VERSION": "2.222.4",
  "JENKINS_NAME": "my-jenkins-test",
  "envVar_JENKINS_MASTER_DOCKER_IMAGE_URL": "gitlab/mydocker_repo/jenkins/master",
  "envVar_ENV": "staging",
  "JENKINS_SLAVE_DOCKER_IMAGE_URL": "gitlab/mydocker_repo/jenkins/slave",
  "ENV": "staging",
  "JENKINS_EXPECTED_VERSION": "2.222.4",
  "JENKINS_PVT_USERNAME": "svc_go",
  "my_docker_ep": "gitlab/mydocker_repo/jenkins",
  "envVar_ASSUME_ROLE_ARN": "arn:aws:iam::123456789012:role/my-nonn-prod-iam-role",
  "envVar_JENKINS_PVT_USERNAME": "svc_go"
})

 2: inspect[exec_base_env_vars_configured]
  1:                    JENKINS_NAME = my-jenkins-test
  2:                 SKIP_EBS_BACKUP = true
  3:                 ASSUME_ROLE_ARN = arn:aws:iam::123456789012:role/my-nonn-prod-iam-role
  4:            JENKINS_PVT_USERNAME = svc_go
  5:        JENKINS_EXPECTED_VERSION = 2.222.4
  6:                     JENKINS_URL = my-jenkins-test-staging.myjenkins.xyz
  7: JENKINS_MASTER_DOCKER_IMAGE_URL = gitlab/mydocker_repo/jenkins/master
  8:                             ENV = staging
  9:  JENKINS_SLAVE_DOCKER_IMAGE_URL = gitlab/mydocker_repo/jenkins/slave
~SubStep2: [assert:  ]
 1 ASSERT OK:     [{{eq .JENKINS_NAME "my-jenkins-test" }}]
 2 ASSERT OK:     [{{eq .JENKINS_URL "my-jenkins-test-staging.myjenkins.xyz" }}]
 3 ASSERT OK:     [{{eq .JENKINS_SLAVE_DOCKER_IMAGE_URL "gitlab/mydocker_repo/jenkins/slave" }}]
-Step2: [: main job ]
  1:                    JENKINS_NAME = my-jenkins-test
  2:                 SKIP_EBS_BACKUP = true
  3:        JENKINS_EXPECTED_VERSION = 2.222.4
  4:  JENKINS_SLAVE_DOCKER_IMAGE_URL = gitlab/mydocker_repo/jenkins/slave
  5: JENKINS_MASTER_DOCKER_IMAGE_URL = gitlab/mydocker_repo/jenkins/master
  6:                     JENKINS_URL = my-jenkins-test-staging.myjenkins.xyz
  7:                             ENV = staging
  8:            JENKINS_PVT_USERNAME = svc_go
  9:                 ASSUME_ROLE_ARN = arn:aws:iam::123456789012:role/my-nonn-prod-iam-role
dvar> env_export_test:
"export JENKINS_NAME=\"my-jenkins-test\"\nexport SKIP_EBS_BACKUP=\"true\"\nexport JENKINS_EXPECTED_VERSION=\"2.222.4\"\nexport JENKINS_SLAVE_DOCKER_IMAGE_URL=\"gitlab/mydocker_repo/jenkins/slave\"\nexport JENKINS_MASTER_DOCKER_IMAGE_URL=\"gitlab/mydocker_repo/jenkins/master\"\nexport JENKINS_URL=\"my-jenkins-test-staging.myjenkins.xyz\"\nexport ENV=\"staging\"\nexport JENKINS_PVT_USERNAME=\"svc_go\"\nexport ASSUME_ROLE_ARN=\"arn:aws:iam::123456789012:role/my-nonn-prod-iam-role\"\n"

-
export JENKINS_NAME="my-jenkins-test"
export SKIP_EBS_BACKUP="true"
export JENKINS_EXPECTED_VERSION="2.222.4"
export JENKINS_SLAVE_DOCKER_IMAGE_URL="gitlab/mydocker_repo/jenkins/slave"
export JENKINS_MASTER_DOCKER_IMAGE_URL="gitlab/mydocker_repo/jenkins/master"
export JENKINS_URL="my-jenkins-test-staging.myjenkins.xyz"
export ENV="staging"
export JENKINS_PVT_USERNAME="svc_go"
export ASSUME_ROLE_ARN="arn:aws:iam::123456789012:role/my-nonn-prod-iam-role"

cmd( 1):
export JENKINS_NAME=my-jenkins-test
export SKIP_EBS_BACKUP=true
export JENKINS_EXPECTED_VERSION=2.222.4
export JENKINS_SLAVE_DOCKER_IMAGE_URL=gitlab/mydocker_repo/jenkins/slave
export JENKINS_MASTER_DOCKER_IMAGE_URL=gitlab/mydocker_repo/jenkins/master
export JENKINS_URL=my-jenkins-test-staging.myjenkins.xyz
export ENV=staging
export JENKINS_PVT_USERNAME=svc_go
export ASSUME_ROLE_ARN=arn:aws:iam::123456789012:role/my-nonn-prod-iam-role

 .. ok
. ok
-Step3:
cmd( 1):
demo conditionally quit
 .. ok
. ok
-Step4: [: conditionally exec in gocd / jenkins / local
obtain the env var from the build environment, it could be either GoCD agent, or a Jenkins slave
it does not matter if the agent is in a EC2 instance, or a docker, or inside of k8s pod, as long as the env var used as indication is right for your case, below is just for example
 ]
dvar> gocd_marker:
"1.2.3"

-
1.2.3
dvar> jenkins_marker:
"None"

-
None
-Step1: [: run in gocd if it detects there is a GoCD marker
also it runs the piped in comand as prior script before real provision step
 ]
cmd( 1):
make my_jenkins_stack_creation
 .. ok
. ok
-Step2: [: run in jenkins
it use different prior step to assume role
 ]

```

#### Relevant

#### Environment variables

* [set env var](../../env-vars/c0048/)
* [get env var](../../env-vars/c0046/)

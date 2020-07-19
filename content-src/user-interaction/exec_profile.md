---
title: "profile with env vars logs"
date: 2020-06-25T22:32:46+11:00
draft: false
weight: 1787
---

#### Concept

So far, we can use an instanceid as context id to select what set of configuration we want to use, nomally they are categorized by global configuration, environment group configuration and individual context instance, for example as below:

```
global\_
        |_nonprod
            |_dev
            |_staging
        |_prod
            |_prod
```

You could use [ up ngo mytask -i dev ] to do deployment work aginst dev environment

However these configuration are more like server (backend) configuration. Most of time, we will have some other configurations, they are related to pipeline user input, for example:

* username
* password(secured)
* flags for build/deployment/workflow to behave differently, eg,
    * create db everytime or skip
    * a resource id (ec2-instanceid/iam-profile-id/database name etc) as data injection for the workflow
* temporary config entry for manual test

All these settings are more like a user execution profile rather static and stable server environment configuration, they are the environment variables input from users

In this case we could classify them all together into an execution profile, for example

```
dev(instanceid):
|_ ENV var1
|_ ENV var2
|_ ENV var3

global\_
        |_nonprod
            |_dev
                 \_dev1_test
                 \_dev2_test:
                 \_dev1_use_memcache:
                 \_dev2_no_db_recreat
            |_staging
        |_prod
            |_prod
```

By using such a execution profile, the CI/CD tools does not need to handle the multiple environment variable entries and secure variable entries, all these could be handled by UPcmd. To trigger the pipeline, a code push is all needed.

This turns the audit to a precise git commit history so it is tracable to understand whose code has actually cause the problem

Please note the execution profile extend dev instance, see the differences of the final var merged

```
up ngo task -d ./tests/functests -t c0153 -p dev1_use_memcache --configdir=./tests/functests
```
        
#### Best practice

1. Turn every pipeline settings, eg multiple environment variables to so called evar (evironment var) pair under eprofiles/evars/[key,value]
2. All the other global/group vars, instance vars, runtime global vars are still kept the same ways as they are
3. Normally, for your existing CLI CI/CD build, you would have already got the logic to map the environment vars to your internal programming vars, these will be kept the same as they are
4. The env vars in eprofiles are already takeing effect, they are just like the env vars you setup in your pipeline GUI to take baisc parameters to instruct how the build/deploy will behave


* Conventional CI/CD pipeline

```
env vars -> pipeline tool via GUI settings -> CLI workflow
```

* UPcmd pipeline
```
UPcmd ==match profile name==>> eprofile/evars => UPcmd managed workflow
```


###### Task

```yaml

eprofiles:
  - name: dev1_use_memcache
    instance: dev
    evars:
      - name: DEPLOY_USER
        value: tomhanks
      - name: SKIP_CREATE_RDS
        value: "yes"
      - name: DB_HOST
        value: devtest_database.mycompany.local

scopes:
  - name: global
    vars:
      db_driver: postgres
      port: 5432
    dvars:
      - name: A_GLOBAL_ENV_VAR
        value: a_global_env_var
        flags:
          - envVar

  - name: nonprod
    members:
      - dev
      - staging
    vars:
      db_host: nonpord_database.test.host
      db_port: 8354
      db_user: test_db_user
      db_password: could_be_encrypted_using_upcmd_too
    dvars:
      - name: db_password
        value: '6HmsmiJIW1PfIXcF4WwOKOMDiL7PstgfKs2aRFajrwY='

  - name: prod
    members: [prod]
    vars:
      host_alias: prod

  - name: dev
    vars:
      host_alias: dev
    dvars:
      - name: db_host
        value: '{{ env "DB_HOST" |default "nonpord_database.test.host" }}'
      - name: A_DEV_ENV_VAR
        value: '{{ env "A_GLOBAL_ENV_VAR" |default "A_GLOBAL_ENV_VAR_NOT_LOCATED" }}'

  - name: staging
    vars:
      host_alias: staging

  - name: prod
    vars:
      host_alias: prod
      db_host: pord_database.proddb.host
      db_user: prod_db_user
    dvars:
      - name: db_password
        value: 'prod_encrypte_aes'

dvars:
  - name: db_hostname
    value: '{{.host_alias}}.myapp.com'
  - name: db_url
    value: 'jdbc:{{.db_driver}}://{{.db_hostname}}:{{.db_port}}/test?user={{.db_user}}&password={{.db_password}}&ssl=true'

tasks:
  -
    name: task
    task:

      -
        func: cmd
        do:
          - name: inspect
            cmd:
              - exec_vars
          -
            name: assert
            cmd:
              - '{{eq .A_GLOBAL_ENV_VAR "a_global_env_var"}}'
              - '{{eq .A_DEV_ENV_VAR "a_global_env_var"}}'
              - '{{eq .db_host "devtest_database.mycompany.local"}}'
              - '{{eq .db_url "jdbc:postgres://dev.myapp.com:8354/test?user=test_db_user&password=6HmsmiJIW1PfIXcF4WwOKOMDiL7PstgfKs2aRFajrwY=&ssl=true"}}'

```

###### Log

```
-exec task: task
loading [Task]:  ./tests/functests/c0153
  "profile - dev1_use_memcache envVars:"
  (*core.Cache)({
"envVar_DEPLOY_USER": "tomhanks",
"envVar_SKIP_CREATE_RDS": "yes",
"envVar_DB_HOST": "devtest_database.mycompany.local"
})

module: [self] instance id: [dev]
Task1: [task ==> task:  ]
-Step1:
~SubStep1: [inspect:  ]
  1: inspect[exec_vars](*core.Cache)({
    "A_GLOBAL_ENV_VAR": "a_global_env_var",
    "db_password": "6HmsmiJIW1PfIXcF4WwOKOMDiL7PstgfKs2aRFajrwY=",
    "db_user": "test_db_user",
    "db_host": "devtest_database.mycompany.local",
    "host_alias": "dev",
    "db_url": "jdbc:postgres://dev.myapp.com:8354/test?user=test_db_user&password=6HmsmiJIW1PfIXcF4WwOKOMDiL7PstgfKs2aRFajrwY=&ssl=true",
    "port": 5432,
    "A_DEV_ENV_VAR": "a_global_env_var",
    "envVar_A_GLOBAL_ENV_VAR": "a_global_env_var",
    "db_driver": "postgres",
    "db_port": 8354,
    "db_hostname": "dev.myapp.com"
})

~SubStep2: [assert:  ]
    1 ASSERT OK:     [{{eq .A_GLOBAL_ENV_VAR "a_global_env_var"}}]
    2 ASSERT OK:     [{{eq .A_DEV_ENV_VAR "a_global_env_var"}}]
    3 ASSERT OK:     [{{eq .db_host "devtest_database.mycompany.local"}}]
    4 ASSERT OK:     [{{eq .db_url "jdbc:postgres://dev.myapp.com:8354/test?user=test_db_user&password=6HmsmiJIW1PfIXcF4WwOKOMDiL7PstgfKs2aRFajrwY=&ssl=true"}}]
```

```
up ngo task -d ./tests/functests -t -id dev c0153 --configdir=./tests/functests
```

```
1: inspect[exec_vars](*core.Cache)({
  "db_driver": "postgres",
  "db_url": "jdbc:postgres://dev.myapp.com:8354/test?user=test_db_user&password=6HmsmiJIW1PfIXcF4WwOKOMDiL7PstgfKs2aRFajrwY=&ssl=true",
  "db_user": "test_db_user",
  "db_hostname": "dev.myapp.com",
  "db_password": "6HmsmiJIW1PfIXcF4WwOKOMDiL7PstgfKs2aRFajrwY=",
  "db_port": 8354,
  "host_alias": "dev",
  "port": 5432,
  "db_host": "nonpord_database.test.host"
})

```

#### Relevant

#### Environment variables

* [set env var](../../env-vars/c0048/)
* [get env var](../../env-vars/c0046/)

---
title: "use env var to encrypt and decrypt"
date: 2020-06-25T22:32:46+11:00
draft: false
weight: 1203
---

## Demo to use env var to decrypt and encrypt

### config

```
scopes:
  - name: nonprod
    members: [dev, staging]
    dvars:
      - name: ENC_KEY_NP
        value: this_str_is_my_nonprod_enc_key
        flags:
          - envVar
      - name: enc_key
        value: '{{ env "ENC_KEY_NP" |validateMandatoryFailIfNone "enc_key"}}'

  - name: prod
    members: [prod]
    dvars:
      - name: ENC_KEY_PROD
        value: this_str_is_my_prod_enc_key
        flags:
          - envVar
      - name: enc_key
        value: '{{ env "ENC_KEY_PROD" |validateMandatoryFailIfNone "enc_key"}}'

tasks:
  -
    name: task
    task:
      - func: call
        dvars:
          - name: choice
            desc: choose 1 to encrypt or anyting else to decrypt
            flags: [prompt]
        do: encrypt
        if: '{{eq .choice "1"}}'
        else:
          - func: call
            do: decrypt

  -
    name: encrypt
    task:
      -
        func: cmd
        dvars:
          - name: raw
            flags:
              - prompt
          - name: encrypted
            value: '{{ .raw | encryptAES .enc_key}}'
          - name: decrypted
            value: '{{ .encrypted | decryptAES .enc_key}}'
        do:
          - name: print
            cmd: '{{ .raw }}'
          - name: print
            cmd: '{{ .encrypted }}'
          - name: print
            cmd: '{{ .decrypted }}'

  -
    name: decrypt
    task:
      -
        func: cmd
        dvars:
          - name: encrypted
            desc: please input the encrypted value
            flags:
              - prompt
          - name: decrypted
            value: '{{ .encrypted | decryptAES .enc_key}}'
        do:
          - name: print
            cmd: '{{ .encrypted }}'
          - name: print
            cmd: '{{ .decrypted }}'

```

### log

```

Ξ /up-project/up git:(master) ▶ up ngo task -d ./tests/functests -t p0193.yml -i dev --configdir=./tests/functests
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
             Timeout -> 3600000
 MaxModuelCallLayers -> 256
work dir: /up-project/up
-exec task: task
module: [self], instance id: [dev], exec profile: []
Task1: [task ==> task:  ]
-Step1:
Enter Value For [choice]: 
choose 1 to encrypt or anyting else to decrypt
1
=Task2: [task ==> encrypt:  ]
--Step1:
Enter Value For [raw]: 
This will be saved as raw's value
hello
~~SubStep1: [print:  ]
hello
~~SubStep2: [print:  ]
9csmIg35CAlsy7hddAtKbIx7R7IRAzRweY5xGbKAgOo=
~~SubStep3: [print:  ]
hello
Ξ /up-project/up git:(master) ▶ up ngo task -d ./tests/functests -t p0193.yml -i dev --configdir=./tests/functests
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
             Timeout -> 3600000
 MaxModuelCallLayers -> 256
work dir: /up-project/up
-exec task: task
loading [Task]:  ./tests/functests/wip.yml
module: [self], instance id: [dev], exec profile: []
Task1: [task ==> task:  ]
-Step1:
Enter Value For [choice]: 
choose 1 to encrypt or anyting else to decrypt
2
-Step1:
=Task3: [task ==> decrypt:  ]
--Step1:
Enter Value For [encrypted]: 
please input the encrypted value
9csmIg35CAlsy7hddAtKbIx7R7IRAzRweY5xGbKAgOo=
~~SubStep1: [print:  ]
9csmIg35CAlsy7hddAtKbIx7R7IRAzRweY5xGbKAgOo=
~~SubStep2: [print:  ]
hello

```
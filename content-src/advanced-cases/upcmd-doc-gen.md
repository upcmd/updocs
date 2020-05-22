---
title: "upcmd doco auto generation"
date: 2020-02-07T14:32:46+11:00
draft: false
weight: 2020
---

#### All this whole site and document are generated from one UP cmd task

#### The tasks

```
Ξ up-project/updocs git:(wip) ▶ up list                               
loading [Config]:  ./upconfig
loading [Task]:  ./up
instance id: nonamed
-task list
     1|              build: build the showcases automatically 
     2| process_main_entry:  
     3|       process_case:  
     4|        data_enrich:  
     5|       generate_doc:  
     6| generate_log_pages: generage verbose level log 
-

Ξ  ▶ up list =

loading [Config]:  ./upconfig
loading [Task]:  ./up
instance id: nonamed
-inspect all tasks:
build: build the showcases automatically.
├── : prepare all the self documented cases
├── : prepare the directory for the already documented cases
├── [ /loop..]  build different types of cases
│   └── [process_main_entry]  
│       ├── : prepare all the self documented cases
│       ├── : debug
│       ├── : get the case list object
│       └── [ /loop..]  filter out all cases have already got documents
│           └── [process_case]  
│               ├── : 
│               ├── []  add casename and log_dir into data model
│               │   └── [data_enrich]  
│               │       ├── : 
│               │       ├── : 
│               │       └── : 
│               ├── : 
│               ├── : 
│               └── []  create sub folders and template the docment
│                   └── [generate_doc]  
│                       ├── : 
│                       ├── : 
│                       ├── : debug and exit
│                       ├── : generate the document
│                       └── [loop_verbose_level /loop..]  loop all different verbose level caller
│                           └── [generate_log_pages]  generage verbose level log
│                               └── : 
└── : copy generated docs to publish dir

process_main_entry: .
├── : prepare all the self documented cases
├── : debug
├── : get the case list object
└── [ /loop..]  filter out all cases have already got documents
    └── [process_case]  
        ├── : 
        ├── []  add casename and log_dir into data model
        │   └── [data_enrich]  
        │       ├── : 
        │       ├── : 
        │       └── : 
        ├── : 
        ├── : 
        └── []  create sub folders and template the docment
            └── [generate_doc]  
                ├── : 
                ├── : 
                ├── : debug and exit
                ├── : generate the document
                └── [loop_verbose_level /loop..]  loop all different verbose level caller
                    └── [generate_log_pages]  generage verbose level log
                        └── : 

process_case: .
├── : 
├── [ /loop..]  add casename and log_dir into data model
│   └── [data_enrich]  
│       ├── : 
│       ├── : 
│       └── : 
├── : 
├── : 
└── [ /loop..]  create sub folders and template the docment
    └── [generate_doc]  
        ├── : 
        ├── : 
        ├── : debug and exit
        ├── : generate the document
        └── [loop_verbose_level /loop..]  loop all different verbose level caller
            └── [generate_log_pages]  generage verbose level log
                └── : 

data_enrich: .
├── : 
├── : 
└── : 

generate_doc: .
├── : 
├── : 
├── : debug and exit
├── : generate the document
└── [loop_verbose_level /loop..]  loop all different verbose level caller
    └── [generate_log_pages]  generage verbose level log
        └── : 

generate_log_pages: generage verbose level log.
└── : 


```

```
version: 1.0.0

vars:
  cached_functests_dir: ./build/functests
  cases_dir: ./build/cases
  log_dir: ./reflogs
  content_dir: ./content
  content_src_dir: ./content-src

dvars:
  - name: upproj_dir
    value: '{{ env "UP_PROJ_DIR" |validateMandatoryFailIfNone "upproj_dir" }}'
    flags: [vvv]

  - name: src_functests_dir
    value: '{{.upproj_dir}}/tests/functests'

  - name: ref_dir
    value: '{{.cached_functests_dir}}'

  - name: rendered_log_dir
    value: '{{.cases_dir}}/logs'


tasks:
  -
    name: build
    desc: build the showcases automatically
    task:

      -
        func: shell
        desc: prepare all the self documented cases
        do:
          - 'rm -rf {{.content_dir}}'
          - 'mkdir -p {{.content_dir}}'
          - rm -rf ./build
          - mkdir -p ./build
          - 'cp -rf {{.src_functests_dir}} {{.cached_functests_dir}}'

      -
        func: shell
        desc: prepare the directory for the already documented cases
        do:
          - rm -rf {{.cases_dir}}
          - mkdir -p {{.cases_dir}}
          - mkdir -p {{.rendered_log_dir}}

      -
        func: call
        desc: build different types of cases
        do:
          - process_main_entry
        loop:
          - c????
          - f????

      -
        func: shell
        desc: copy generated docs to publish dir
        do:
          - 'cp -rf {{.cases_dir}}/* {{.content_dir}}/'
          - 'cp -rf {{.content_src_dir}}/* {{.content_dir}}/'
          - 'mkdir -p {{.content_dir}}/reflogs'
          - 'cp -rf {{.log_dir}}/* {{.content_dir}}/reflogs/'
          - kill `ps -ef|grep hugo|tail -1|awk '{print $2}'`

  -
    name: process_main_entry
    task:
      -
        func: shell
        desc: prepare all the self documented cases
        do:
          - 'cd {{.cached_functests_dir}}; ls {{.loopitem}}.yml'
        reg: cases

      - func: cmd
        desc: debug
        dox:
          -
            name: print
            cmd: '{{.cases}}'

      - func: cmd
        desc: get the case list object
        dvars:
          - name: void
            value: '{{ .cases | splitLines |reg "caselist" }}'

      -
        func: call
        desc: filter out all cases have already got documents
        do:
          - process_case
        loop: "caselist"

  -
    name: process_case
    task:

      -
        func: cmd
        do:
          - name: reg
            cmd:
              name: casefilename
              value: '{{.loopitem}}'

          - name: reg
            cmd:
              name: casename
              value: '{{.casefilename | replace ".yml" ""}}'

          - name: print
            cmd: 'processing {{.casefilename}}'

          - name: readfile
            desc: read yml file
            cmd:
              filename: '{{.casefilename}}'
              dir: '{{.cached_functests_dir}}'
              reg: caseyml

          - name: print
            cmd: |
              yml file content:
              {{.caseyml}}

          - name: query
            cmd:
              ymlkey: caseyml
              path: doc_meta.
              reg: docyml
            flags:
              - ymlonly

      -
        func: call
        desc: add casename and log_dir into data model
        do: data_enrich
        if: '{{ gt (.docyml|len) 0 }}'

      -
        func: cmd
        do:

          - name: readfile
            desc: review if the file content is correct
            cmdx:
              filename: '{{.casefilename}}'
              dir: '{{.ref_dir}}'
              reg: debug_yml

          - name: print
            cmdx: |
              debug filename: {{.casefilename}}
              debug dir: {{.ref_dir}}
              content before change:
              ---
              {{.debug_yml}}
              ---

          - name: yml_delete
            cmd:
              ymlfile: '{{.casefilename}}'
              refdir: '{{.ref_dir}}'
              path: doc_meta
            flags:
              - inplace

          - name: yml_delete
            cmd:
              ymlfile: '{{.casefilename}}'
              refdir: '{{.ref_dir}}'
              path: notes
            flags:
              - inplace

          - name: readfile
            desc: review if the file content is correct
            cmd:
              filename: '{{.casefilename}}'
              dir: '{{.ref_dir}}'
              reg: final_yml

          - name: print
            desc: show the modified yml content
            cmd: '{{.final_yml}}'

          - namex: exit

      -
        func: cmd
        dvars:
          - name: casedoc
            value: '{{.docyml}}'
            ex pand: 2
            flags: [to_object, reg]

      -
        func: call
        desc: create sub folders and template the docment
        do: generate_doc
        if: '{{ gt (.casedoc|len) 0 }}'

  -
    name: data_enrich
    task:
      - func: cmd
        do:
          - name: yml_write
            cmd:
              ymlstr: '{{.docyml}}'
              path: casename
              value: '{{.casename}}'
              reg: docyml
          - name: yml_write
            cmd:
              ymlstr: '{{.docyml}}'
              path: ref_dir
              value: '{{.ref_dir}}'
              reg: docyml
          - name: yml_write
            cmd:
              ymlstr: '{{.docyml}}'
              path: main_yml_ref_file
              value: '{{.ref_dir}}/{{.casefilename}}'
              reg: docyml
          - name: yml_write
            cmd:
              ymlstr: '{{.docyml}}'
              path: main_log_ref_file
              value: '{{.log_dir}}/{{.casename}}_vvv.log'
              reg: docyml

          - name: query
            cmd:
              ymlkey: docyml
              reg: weightstr
              path: weight.
            flags:
              - ymlonly

          - name: print
            cmd: |
                existing weight: {{.weightstr}}
                weight string length: {{.weightstr|len}}

      - func: cmd
        do:
          - name: print
            cmd: 'setup weight'

          - name: yml_write
            cmd:
              ymlstr: '{{.docyml}}'
              path: weight
              value: '{{ substr 1 5 .casename | atoi |mul 10| add 100 }}'
              reg: docyml
        if: '{{ eq (print .weightstr|len) 0 }}'

      - func: cmd
        do:

#          - name: yml_write
#            cmd:
#              ymlstr: '{{.docyml}}'
#              path: page_weight
#              value: '{{ .casename |replace "c" "" |atoi}}'
#              reg: docyml
#          - name: yml_write
#            cmd:
#              ymlstr: '{{.docyml}}'
#              path: log_dir
#              nodevalue: '{{.log_dir}}'
#              reg: docyml

          - name: print
            cmd: |
              ---------- docyml content---------
              {{.docyml}}

          - name: return
            cmd:
              - docyml

#          - name: exit
  -
    name: generate_doc
    task:

#      - func: shell
#        do:
#          - 'echo  "casename: {{.casename}} - folder: {{.casedoc_object.folder}}" >> debug'

      - func: cmd
        do:
          - name: reg
            cmd:
              name: target_dir
              value: '{{.cases_dir}}/{{.casedoc_object.folder}}'
#          - name: trace
#            cmd: debug ==>


#      - func: shell
#        do:
#          - 'echo  "casename: {{.casename}} - target: {{.target_dir}}" >> debug'

#      - func: cmd
#        do:
#          - name: trace
#            cmd: <== debug

      - func: shell
        do:
          - 'mkdir -p {{.target_dir}}'

      -
        func: cmd
        desc: debug and exit
        dox:
          - name: printobj
            cmd: casedoc_object
#          - name: exit
        if: '{{eq .casename "c0009"}}'

      -
        func: cmd
        desc: generate the document
        do:
          - name: printobj
            cmd: casedoc_object

          - name: print
            desc: casedoc length
            cmd: '{{ .casedoc|len }}'

          - name: template
            desc: template the document
            cmd:
              src: ./templates/doc.template
              dest: '{{.target_dir}}/{{.casename}}.md'
              datakey: casedoc_object

      -
        func: call
        name: loop_verbose_level
        desc: loop all different verbose level caller
        do: generate_log_pages
        loop:
          - v
          - vv
          - vvv
          - vvvv
          - vvvvv
          - vvvvvv
          - vvvvvvv

  -
    name: generate_log_pages
    desc: generage verbose level log
    task:
      -
        func: cmd
        dvars:
          - name: logidinumber
            value: '{{ substr 1 5 .casename | atoi |mul 10| add 10000 |add .loopindex }}'
            flags: [vvv,]

          - name: logtemplate
            value: |
              title: {{.casename}}_{{.loopitem}}
              weight: {{.logidinumber}}
              loglevle: {{.loopitem}}
              casename: {{.casename}}
              relatedlink: ../../{{.casedoc_object.folder}}/{{.casename}}
              log_ref_file: {{.log_dir}}/{{.casename}}_{{.loopitem}}.log
            flags:
              - to_object
              - vvvvv
        do:
          - name: print
            cmd: '{{.logtemplate}}'
          - name: print
            cmd: '{{.loopitem}}'

          - name: template
            desc: generate the log
            cmd:
              src: ./templates/log.template
              dest: '{{.rendered_log_dir}}/{{.casename}}_{{.loopitem}}.md'
              datakey: logtemplate_object

```


#### log

```
loading [Config]:  ./upconfig
-exec task: build
loading [Task]:  ./up
instance id: nonamed
Task1: [build ==> build: build the showcases automatically ]
-Step1: [: prepare all the self documented cases ]
cmd( 1):

 .. ok
cmd( 2):

 .. ok
cmd( 3):

 .. ok
cmd( 4):

 .. ok
cmd( 5):

 .. ok
. ok
-Step2: [: prepare the directory for the already documented cases ]
cmd( 1):

 .. ok
cmd( 2):

 .. ok
cmd( 3):

 .. ok
. ok
-Step3: [: build different types of cases ]
=Task2: [build ==> process_main_entry:  ]
--Step1: [: prepare all the self documented cases ]
cmd( 1):
c0001.yml
c0002.yml
c0003.yml
c0004.yml
c0005.yml
c0006.yml
c0007.yml
c0008.yml
c0009.yml
c0010.yml
c0011.yml
c0012.yml
c0013.yml
c0014.yml
c0017.yml
c0019.yml
c0020.yml
c0021.yml
c0022.yml
c0023.yml
c0024.yml
c0025.yml
c0026.yml
c0027.yml
c0028.yml
c0029.yml
c0030.yml
c0031.yml
c0032.yml
c0033.yml
c0034.yml
c0035.yml
c0036.yml
c0037.yml
c0038.yml
c0039.yml
c0040.yml
c0041.yml
c0042.yml
c0043.yml
c0044.yml
c0046.yml
c0047.yml
c0048.yml
c0049.yml
c0050.yml
c0051.yml
c0052.yml
c0054.yml
c0055.yml
c0056.yml
c0059.yml
c0060.yml
c0061.yml
c0062.yml
c0063.yml
c0064.yml
c0065.yml
c0066.yml
c0068.yml
c0069.yml
c0070.yml
c0071.yml
c0072.yml
c0073.yml
c0074.yml
c0075.yml
c0076.yml
c0078.yml
c0079.yml
c0080.yml
c0081.yml
c0082.yml
c0083.yml
c0084.yml
c0085.yml
c0086.yml
c0087.yml
c0088.yml
c0089.yml
c0090.yml
c0091.yml
c0092.yml
c0093.yml
c0094.yml
c0095.yml
c0096.yml
c0098.yml
c0099.yml
c0100.yml
c0101.yml
c0102.yml
c0103.yml
c0104.yml
c0105.yml
c0106.yml
c0107.yml
c0108.yml
c0109.yml
c0110.yml
c0111.yml
c0112.yml
c0113.yml
c0114.yml
c0115.yml
c0116.yml
c0117.yml
c0118.yml
c0119.yml
c0120.yml
c0121.yml
c0122.yml
c0123.yml
c0124.yml
c0125.yml
c0126.yml
c0127.yml
c0128.yml
c0129.yml
c0130.yml
c0131.yml
c0132.yml
c0133.yml
c0134.yml
c0135.yml
c0136.yml
c0137.yml
c0138.yml
c0139.yml
c0140.yml
 .. ok
. ok
--Step2: [: debug ]
 WARN: [*] - [Step is deactivated!]
--Step3: [: get the case list object ]
 WARN: [cmd] - [Not implemented or void for no action!]
--Step4: [: filter out all cases have already got documents ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0001.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
516
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0001_v
weight: 10010
loglevle: v
casename: c0001
relatedlink: ../../quick-start/c0001
log_ref_file: ./reflogs/c0001_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0001_vv
weight: 10011
loglevle: vv
casename: c0001
relatedlink: ../../quick-start/c0001
log_ref_file: ./reflogs/c0001_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0001_vvv
weight: 10012
loglevle: vvv
casename: c0001
relatedlink: ../../quick-start/c0001
log_ref_file: ./reflogs/c0001_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0001_vvvv
weight: 10013
loglevle: vvvv
casename: c0001
relatedlink: ../../quick-start/c0001
log_ref_file: ./reflogs/c0001_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0001_vvvvv
weight: 10014
loglevle: vvvvv
casename: c0001
relatedlink: ../../quick-start/c0001
log_ref_file: ./reflogs/c0001_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0001_vvvvvv
weight: 10015
loglevle: vvvvvv
casename: c0001
relatedlink: ../../quick-start/c0001
log_ref_file: ./reflogs/c0001_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0001_vvvvvvv
weight: 10016
loglevle: vvvvvvv
casename: c0001
relatedlink: ../../quick-start/c0001
log_ref_file: ./reflogs/c0001_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0002.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
1254
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0002_v
weight: 10020
loglevle: v
casename: c0002
relatedlink: ../../quick-start/c0002
log_ref_file: ./reflogs/c0002_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0002_vv
weight: 10021
loglevle: vv
casename: c0002
relatedlink: ../../quick-start/c0002
log_ref_file: ./reflogs/c0002_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0002_vvv
weight: 10022
loglevle: vvv
casename: c0002
relatedlink: ../../quick-start/c0002
log_ref_file: ./reflogs/c0002_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0002_vvvv
weight: 10023
loglevle: vvvv
casename: c0002
relatedlink: ../../quick-start/c0002
log_ref_file: ./reflogs/c0002_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0002_vvvvv
weight: 10024
loglevle: vvvvv
casename: c0002
relatedlink: ../../quick-start/c0002
log_ref_file: ./reflogs/c0002_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0002_vvvvvv
weight: 10025
loglevle: vvvvvv
casename: c0002
relatedlink: ../../quick-start/c0002
log_ref_file: ./reflogs/c0002_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0002_vvvvvvv
weight: 10026
loglevle: vvvvvvv
casename: c0002
relatedlink: ../../quick-start/c0002
log_ref_file: ./reflogs/c0002_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0003.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
313
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0003_v
weight: 10030
loglevle: v
casename: c0003
relatedlink: ../../quick-start/c0003
log_ref_file: ./reflogs/c0003_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0003_vv
weight: 10031
loglevle: vv
casename: c0003
relatedlink: ../../quick-start/c0003
log_ref_file: ./reflogs/c0003_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0003_vvv
weight: 10032
loglevle: vvv
casename: c0003
relatedlink: ../../quick-start/c0003
log_ref_file: ./reflogs/c0003_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0003_vvvv
weight: 10033
loglevle: vvvv
casename: c0003
relatedlink: ../../quick-start/c0003
log_ref_file: ./reflogs/c0003_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0003_vvvvv
weight: 10034
loglevle: vvvvv
casename: c0003
relatedlink: ../../quick-start/c0003
log_ref_file: ./reflogs/c0003_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0003_vvvvvv
weight: 10035
loglevle: vvvvvv
casename: c0003
relatedlink: ../../quick-start/c0003
log_ref_file: ./reflogs/c0003_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0003_vvvvvvv
weight: 10036
loglevle: vvvvvvv
casename: c0003
relatedlink: ../../quick-start/c0003
log_ref_file: ./reflogs/c0003_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0004.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
279
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0004_v
weight: 10040
loglevle: v
casename: c0004
relatedlink: ../../quick-start/c0004
log_ref_file: ./reflogs/c0004_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0004_vv
weight: 10041
loglevle: vv
casename: c0004
relatedlink: ../../quick-start/c0004
log_ref_file: ./reflogs/c0004_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0004_vvv
weight: 10042
loglevle: vvv
casename: c0004
relatedlink: ../../quick-start/c0004
log_ref_file: ./reflogs/c0004_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0004_vvvv
weight: 10043
loglevle: vvvv
casename: c0004
relatedlink: ../../quick-start/c0004
log_ref_file: ./reflogs/c0004_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0004_vvvvv
weight: 10044
loglevle: vvvvv
casename: c0004
relatedlink: ../../quick-start/c0004
log_ref_file: ./reflogs/c0004_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0004_vvvvvv
weight: 10045
loglevle: vvvvvv
casename: c0004
relatedlink: ../../quick-start/c0004
log_ref_file: ./reflogs/c0004_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0004_vvvvvvv
weight: 10046
loglevle: vvvvvvv
casename: c0004
relatedlink: ../../quick-start/c0004
log_ref_file: ./reflogs/c0004_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0005.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
576
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0005_v
weight: 10050
loglevle: v
casename: c0005
relatedlink: ../../quick-start/c0005
log_ref_file: ./reflogs/c0005_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0005_vv
weight: 10051
loglevle: vv
casename: c0005
relatedlink: ../../quick-start/c0005
log_ref_file: ./reflogs/c0005_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0005_vvv
weight: 10052
loglevle: vvv
casename: c0005
relatedlink: ../../quick-start/c0005
log_ref_file: ./reflogs/c0005_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0005_vvvv
weight: 10053
loglevle: vvvv
casename: c0005
relatedlink: ../../quick-start/c0005
log_ref_file: ./reflogs/c0005_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0005_vvvvv
weight: 10054
loglevle: vvvvv
casename: c0005
relatedlink: ../../quick-start/c0005
log_ref_file: ./reflogs/c0005_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0005_vvvvvv
weight: 10055
loglevle: vvvvvv
casename: c0005
relatedlink: ../../quick-start/c0005
log_ref_file: ./reflogs/c0005_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0005_vvvvvvv
weight: 10056
loglevle: vvvvvvv
casename: c0005
relatedlink: ../../quick-start/c0005
log_ref_file: ./reflogs/c0005_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0006.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
416
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0006_v
weight: 10060
loglevle: v
casename: c0006
relatedlink: ../../quick-start/c0006
log_ref_file: ./reflogs/c0006_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0006_vv
weight: 10061
loglevle: vv
casename: c0006
relatedlink: ../../quick-start/c0006
log_ref_file: ./reflogs/c0006_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0006_vvv
weight: 10062
loglevle: vvv
casename: c0006
relatedlink: ../../quick-start/c0006
log_ref_file: ./reflogs/c0006_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0006_vvvv
weight: 10063
loglevle: vvvv
casename: c0006
relatedlink: ../../quick-start/c0006
log_ref_file: ./reflogs/c0006_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0006_vvvvv
weight: 10064
loglevle: vvvvv
casename: c0006
relatedlink: ../../quick-start/c0006
log_ref_file: ./reflogs/c0006_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0006_vvvvvv
weight: 10065
loglevle: vvvvvv
casename: c0006
relatedlink: ../../quick-start/c0006
log_ref_file: ./reflogs/c0006_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0006_vvvvvvv
weight: 10066
loglevle: vvvvvvv
casename: c0006
relatedlink: ../../quick-start/c0006
log_ref_file: ./reflogs/c0006_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0007.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
646
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0007_v
weight: 10070
loglevle: v
casename: c0007
relatedlink: ../../quick-start/c0007
log_ref_file: ./reflogs/c0007_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0007_vv
weight: 10071
loglevle: vv
casename: c0007
relatedlink: ../../quick-start/c0007
log_ref_file: ./reflogs/c0007_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0007_vvv
weight: 10072
loglevle: vvv
casename: c0007
relatedlink: ../../quick-start/c0007
log_ref_file: ./reflogs/c0007_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0007_vvvv
weight: 10073
loglevle: vvvv
casename: c0007
relatedlink: ../../quick-start/c0007
log_ref_file: ./reflogs/c0007_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0007_vvvvv
weight: 10074
loglevle: vvvvv
casename: c0007
relatedlink: ../../quick-start/c0007
log_ref_file: ./reflogs/c0007_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0007_vvvvvv
weight: 10075
loglevle: vvvvvv
casename: c0007
relatedlink: ../../quick-start/c0007
log_ref_file: ./reflogs/c0007_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0007_vvvvvvv
weight: 10076
loglevle: vvvvvvv
casename: c0007
relatedlink: ../../quick-start/c0007
log_ref_file: ./reflogs/c0007_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0008.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
1673
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0008_v
weight: 10080
loglevle: v
casename: c0008
relatedlink: ../../scope/c0008
log_ref_file: ./reflogs/c0008_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0008_vv
weight: 10081
loglevle: vv
casename: c0008
relatedlink: ../../scope/c0008
log_ref_file: ./reflogs/c0008_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0008_vvv
weight: 10082
loglevle: vvv
casename: c0008
relatedlink: ../../scope/c0008
log_ref_file: ./reflogs/c0008_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0008_vvvv
weight: 10083
loglevle: vvvv
casename: c0008
relatedlink: ../../scope/c0008
log_ref_file: ./reflogs/c0008_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0008_vvvvv
weight: 10084
loglevle: vvvvv
casename: c0008
relatedlink: ../../scope/c0008
log_ref_file: ./reflogs/c0008_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0008_vvvvvv
weight: 10085
loglevle: vvvvvv
casename: c0008
relatedlink: ../../scope/c0008
log_ref_file: ./reflogs/c0008_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0008_vvvvvvv
weight: 10086
loglevle: vvvvvvv
casename: c0008
relatedlink: ../../scope/c0008
log_ref_file: ./reflogs/c0008_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0009.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
1693
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0009_v
weight: 10090
loglevle: v
casename: c0009
relatedlink: ../../scope/c0009
log_ref_file: ./reflogs/c0009_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0009_vv
weight: 10091
loglevle: vv
casename: c0009
relatedlink: ../../scope/c0009
log_ref_file: ./reflogs/c0009_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0009_vvv
weight: 10092
loglevle: vvv
casename: c0009
relatedlink: ../../scope/c0009
log_ref_file: ./reflogs/c0009_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0009_vvvv
weight: 10093
loglevle: vvvv
casename: c0009
relatedlink: ../../scope/c0009
log_ref_file: ./reflogs/c0009_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0009_vvvvv
weight: 10094
loglevle: vvvvv
casename: c0009
relatedlink: ../../scope/c0009
log_ref_file: ./reflogs/c0009_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0009_vvvvvv
weight: 10095
loglevle: vvvvvv
casename: c0009
relatedlink: ../../scope/c0009
log_ref_file: ./reflogs/c0009_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0009_vvvvvvv
weight: 10096
loglevle: vvvvvvv
casename: c0009
relatedlink: ../../scope/c0009
log_ref_file: ./reflogs/c0009_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0010.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
1266
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0010_v
weight: 10100
loglevle: v
casename: c0010
relatedlink: ../../scope/c0010
log_ref_file: ./reflogs/c0010_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0010_vv
weight: 10101
loglevle: vv
casename: c0010
relatedlink: ../../scope/c0010
log_ref_file: ./reflogs/c0010_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0010_vvv
weight: 10102
loglevle: vvv
casename: c0010
relatedlink: ../../scope/c0010
log_ref_file: ./reflogs/c0010_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0010_vvvv
weight: 10103
loglevle: vvvv
casename: c0010
relatedlink: ../../scope/c0010
log_ref_file: ./reflogs/c0010_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0010_vvvvv
weight: 10104
loglevle: vvvvv
casename: c0010
relatedlink: ../../scope/c0010
log_ref_file: ./reflogs/c0010_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0010_vvvvvv
weight: 10105
loglevle: vvvvvv
casename: c0010
relatedlink: ../../scope/c0010
log_ref_file: ./reflogs/c0010_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0010_vvvvvvv
weight: 10106
loglevle: vvvvvvv
casename: c0010
relatedlink: ../../scope/c0010
log_ref_file: ./reflogs/c0010_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0011.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
295
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0011_v
weight: 10110
loglevle: v
casename: c0011
relatedlink: ../../scope/c0011
log_ref_file: ./reflogs/c0011_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0011_vv
weight: 10111
loglevle: vv
casename: c0011
relatedlink: ../../scope/c0011
log_ref_file: ./reflogs/c0011_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0011_vvv
weight: 10112
loglevle: vvv
casename: c0011
relatedlink: ../../scope/c0011
log_ref_file: ./reflogs/c0011_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0011_vvvv
weight: 10113
loglevle: vvvv
casename: c0011
relatedlink: ../../scope/c0011
log_ref_file: ./reflogs/c0011_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0011_vvvvv
weight: 10114
loglevle: vvvvv
casename: c0011
relatedlink: ../../scope/c0011
log_ref_file: ./reflogs/c0011_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0011_vvvvvv
weight: 10115
loglevle: vvvvvv
casename: c0011
relatedlink: ../../scope/c0011
log_ref_file: ./reflogs/c0011_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0011_vvvvvvv
weight: 10116
loglevle: vvvvvvv
casename: c0011
relatedlink: ../../scope/c0011
log_ref_file: ./reflogs/c0011_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0012.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
629
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0012_v
weight: 10120
loglevle: v
casename: c0012
relatedlink: ../../vars/c0012
log_ref_file: ./reflogs/c0012_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0012_vv
weight: 10121
loglevle: vv
casename: c0012
relatedlink: ../../vars/c0012
log_ref_file: ./reflogs/c0012_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0012_vvv
weight: 10122
loglevle: vvv
casename: c0012
relatedlink: ../../vars/c0012
log_ref_file: ./reflogs/c0012_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0012_vvvv
weight: 10123
loglevle: vvvv
casename: c0012
relatedlink: ../../vars/c0012
log_ref_file: ./reflogs/c0012_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0012_vvvvv
weight: 10124
loglevle: vvvvv
casename: c0012
relatedlink: ../../vars/c0012
log_ref_file: ./reflogs/c0012_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0012_vvvvvv
weight: 10125
loglevle: vvvvvv
casename: c0012
relatedlink: ../../vars/c0012
log_ref_file: ./reflogs/c0012_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0012_vvvvvvv
weight: 10126
loglevle: vvvvvvv
casename: c0012
relatedlink: ../../vars/c0012
log_ref_file: ./reflogs/c0012_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0013.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
1777
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0013_v
weight: 10130
loglevle: v
casename: c0013
relatedlink: ../../vars/c0013
log_ref_file: ./reflogs/c0013_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0013_vv
weight: 10131
loglevle: vv
casename: c0013
relatedlink: ../../vars/c0013
log_ref_file: ./reflogs/c0013_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0013_vvv
weight: 10132
loglevle: vvv
casename: c0013
relatedlink: ../../vars/c0013
log_ref_file: ./reflogs/c0013_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0013_vvvv
weight: 10133
loglevle: vvvv
casename: c0013
relatedlink: ../../vars/c0013
log_ref_file: ./reflogs/c0013_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0013_vvvvv
weight: 10134
loglevle: vvvvv
casename: c0013
relatedlink: ../../vars/c0013
log_ref_file: ./reflogs/c0013_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0013_vvvvvv
weight: 10135
loglevle: vvvvvv
casename: c0013
relatedlink: ../../vars/c0013
log_ref_file: ./reflogs/c0013_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0013_vvvvvvv
weight: 10136
loglevle: vvvvvvv
casename: c0013
relatedlink: ../../vars/c0013
log_ref_file: ./reflogs/c0013_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0014.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
1430
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0014_v
weight: 10140
loglevle: v
casename: c0014
relatedlink: ../../vars/c0014
log_ref_file: ./reflogs/c0014_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0014_vv
weight: 10141
loglevle: vv
casename: c0014
relatedlink: ../../vars/c0014
log_ref_file: ./reflogs/c0014_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0014_vvv
weight: 10142
loglevle: vvv
casename: c0014
relatedlink: ../../vars/c0014
log_ref_file: ./reflogs/c0014_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0014_vvvv
weight: 10143
loglevle: vvvv
casename: c0014
relatedlink: ../../vars/c0014
log_ref_file: ./reflogs/c0014_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0014_vvvvv
weight: 10144
loglevle: vvvvv
casename: c0014
relatedlink: ../../vars/c0014
log_ref_file: ./reflogs/c0014_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0014_vvvvvv
weight: 10145
loglevle: vvvvvv
casename: c0014
relatedlink: ../../vars/c0014
log_ref_file: ./reflogs/c0014_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0014_vvvvvvv
weight: 10146
loglevle: vvvvvvv
casename: c0014
relatedlink: ../../vars/c0014
log_ref_file: ./reflogs/c0014_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0017.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
592
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0017_v
weight: 10170
loglevle: v
casename: c0017
relatedlink: ../../call-func/c0017
log_ref_file: ./reflogs/c0017_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0017_vv
weight: 10171
loglevle: vv
casename: c0017
relatedlink: ../../call-func/c0017
log_ref_file: ./reflogs/c0017_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0017_vvv
weight: 10172
loglevle: vvv
casename: c0017
relatedlink: ../../call-func/c0017
log_ref_file: ./reflogs/c0017_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0017_vvvv
weight: 10173
loglevle: vvvv
casename: c0017
relatedlink: ../../call-func/c0017
log_ref_file: ./reflogs/c0017_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0017_vvvvv
weight: 10174
loglevle: vvvvv
casename: c0017
relatedlink: ../../call-func/c0017
log_ref_file: ./reflogs/c0017_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0017_vvvvvv
weight: 10175
loglevle: vvvvvv
casename: c0017
relatedlink: ../../call-func/c0017
log_ref_file: ./reflogs/c0017_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0017_vvvvvvv
weight: 10176
loglevle: vvvvvvv
casename: c0017
relatedlink: ../../call-func/c0017
log_ref_file: ./reflogs/c0017_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0019.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
473
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0019_v
weight: 10190
loglevle: v
casename: c0019
relatedlink: ../../vars/c0019
log_ref_file: ./reflogs/c0019_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0019_vv
weight: 10191
loglevle: vv
casename: c0019
relatedlink: ../../vars/c0019
log_ref_file: ./reflogs/c0019_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0019_vvv
weight: 10192
loglevle: vvv
casename: c0019
relatedlink: ../../vars/c0019
log_ref_file: ./reflogs/c0019_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0019_vvvv
weight: 10193
loglevle: vvvv
casename: c0019
relatedlink: ../../vars/c0019
log_ref_file: ./reflogs/c0019_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0019_vvvvv
weight: 10194
loglevle: vvvvv
casename: c0019
relatedlink: ../../vars/c0019
log_ref_file: ./reflogs/c0019_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0019_vvvvvv
weight: 10195
loglevle: vvvvvv
casename: c0019
relatedlink: ../../vars/c0019
log_ref_file: ./reflogs/c0019_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0019_vvvvvvv
weight: 10196
loglevle: vvvvvvv
casename: c0019
relatedlink: ../../vars/c0019
log_ref_file: ./reflogs/c0019_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0020.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
823
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0020_v
weight: 10200
loglevle: v
casename: c0020
relatedlink: ../../call-func/c0020
log_ref_file: ./reflogs/c0020_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0020_vv
weight: 10201
loglevle: vv
casename: c0020
relatedlink: ../../call-func/c0020
log_ref_file: ./reflogs/c0020_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0020_vvv
weight: 10202
loglevle: vvv
casename: c0020
relatedlink: ../../call-func/c0020
log_ref_file: ./reflogs/c0020_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0020_vvvv
weight: 10203
loglevle: vvvv
casename: c0020
relatedlink: ../../call-func/c0020
log_ref_file: ./reflogs/c0020_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0020_vvvvv
weight: 10204
loglevle: vvvvv
casename: c0020
relatedlink: ../../call-func/c0020
log_ref_file: ./reflogs/c0020_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0020_vvvvvv
weight: 10205
loglevle: vvvvvv
casename: c0020
relatedlink: ../../call-func/c0020
log_ref_file: ./reflogs/c0020_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0020_vvvvvvv
weight: 10206
loglevle: vvvvvvv
casename: c0020
relatedlink: ../../call-func/c0020
log_ref_file: ./reflogs/c0020_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0021.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
377
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0021_v
weight: 10210
loglevle: v
casename: c0021
relatedlink: ../../vars/c0021
log_ref_file: ./reflogs/c0021_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0021_vv
weight: 10211
loglevle: vv
casename: c0021
relatedlink: ../../vars/c0021
log_ref_file: ./reflogs/c0021_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0021_vvv
weight: 10212
loglevle: vvv
casename: c0021
relatedlink: ../../vars/c0021
log_ref_file: ./reflogs/c0021_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0021_vvvv
weight: 10213
loglevle: vvvv
casename: c0021
relatedlink: ../../vars/c0021
log_ref_file: ./reflogs/c0021_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0021_vvvvv
weight: 10214
loglevle: vvvvv
casename: c0021
relatedlink: ../../vars/c0021
log_ref_file: ./reflogs/c0021_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0021_vvvvvv
weight: 10215
loglevle: vvvvvv
casename: c0021
relatedlink: ../../vars/c0021
log_ref_file: ./reflogs/c0021_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0021_vvvvvvv
weight: 10216
loglevle: vvvvvvv
casename: c0021
relatedlink: ../../vars/c0021
log_ref_file: ./reflogs/c0021_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0022.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
564
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0022_v
weight: 10220
loglevle: v
casename: c0022
relatedlink: ../../vars/c0022
log_ref_file: ./reflogs/c0022_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0022_vv
weight: 10221
loglevle: vv
casename: c0022
relatedlink: ../../vars/c0022
log_ref_file: ./reflogs/c0022_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0022_vvv
weight: 10222
loglevle: vvv
casename: c0022
relatedlink: ../../vars/c0022
log_ref_file: ./reflogs/c0022_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0022_vvvv
weight: 10223
loglevle: vvvv
casename: c0022
relatedlink: ../../vars/c0022
log_ref_file: ./reflogs/c0022_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0022_vvvvv
weight: 10224
loglevle: vvvvv
casename: c0022
relatedlink: ../../vars/c0022
log_ref_file: ./reflogs/c0022_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0022_vvvvvv
weight: 10225
loglevle: vvvvvv
casename: c0022
relatedlink: ../../vars/c0022
log_ref_file: ./reflogs/c0022_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0022_vvvvvvv
weight: 10226
loglevle: vvvvvvv
casename: c0022
relatedlink: ../../vars/c0022
log_ref_file: ./reflogs/c0022_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0023.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
1194
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0023_v
weight: 10230
loglevle: v
casename: c0023
relatedlink: ../../dvars/c0023
log_ref_file: ./reflogs/c0023_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0023_vv
weight: 10231
loglevle: vv
casename: c0023
relatedlink: ../../dvars/c0023
log_ref_file: ./reflogs/c0023_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0023_vvv
weight: 10232
loglevle: vvv
casename: c0023
relatedlink: ../../dvars/c0023
log_ref_file: ./reflogs/c0023_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0023_vvvv
weight: 10233
loglevle: vvvv
casename: c0023
relatedlink: ../../dvars/c0023
log_ref_file: ./reflogs/c0023_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0023_vvvvv
weight: 10234
loglevle: vvvvv
casename: c0023
relatedlink: ../../dvars/c0023
log_ref_file: ./reflogs/c0023_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0023_vvvvvv
weight: 10235
loglevle: vvvvvv
casename: c0023
relatedlink: ../../dvars/c0023
log_ref_file: ./reflogs/c0023_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0023_vvvvvvv
weight: 10236
loglevle: vvvvvvv
casename: c0023
relatedlink: ../../dvars/c0023
log_ref_file: ./reflogs/c0023_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0024.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
293
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0024_v
weight: 10240
loglevle: v
casename: c0024
relatedlink: ../../dvars/c0024
log_ref_file: ./reflogs/c0024_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0024_vv
weight: 10241
loglevle: vv
casename: c0024
relatedlink: ../../dvars/c0024
log_ref_file: ./reflogs/c0024_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0024_vvv
weight: 10242
loglevle: vvv
casename: c0024
relatedlink: ../../dvars/c0024
log_ref_file: ./reflogs/c0024_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0024_vvvv
weight: 10243
loglevle: vvvv
casename: c0024
relatedlink: ../../dvars/c0024
log_ref_file: ./reflogs/c0024_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0024_vvvvv
weight: 10244
loglevle: vvvvv
casename: c0024
relatedlink: ../../dvars/c0024
log_ref_file: ./reflogs/c0024_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0024_vvvvvv
weight: 10245
loglevle: vvvvvv
casename: c0024
relatedlink: ../../dvars/c0024
log_ref_file: ./reflogs/c0024_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0024_vvvvvvv
weight: 10246
loglevle: vvvvvvv
casename: c0024
relatedlink: ../../dvars/c0024
log_ref_file: ./reflogs/c0024_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0025.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
544
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0025_v
weight: 10250
loglevle: v
casename: c0025
relatedlink: ../../dvars/c0025
log_ref_file: ./reflogs/c0025_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0025_vv
weight: 10251
loglevle: vv
casename: c0025
relatedlink: ../../dvars/c0025
log_ref_file: ./reflogs/c0025_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0025_vvv
weight: 10252
loglevle: vvv
casename: c0025
relatedlink: ../../dvars/c0025
log_ref_file: ./reflogs/c0025_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0025_vvvv
weight: 10253
loglevle: vvvv
casename: c0025
relatedlink: ../../dvars/c0025
log_ref_file: ./reflogs/c0025_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0025_vvvvv
weight: 10254
loglevle: vvvvv
casename: c0025
relatedlink: ../../dvars/c0025
log_ref_file: ./reflogs/c0025_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0025_vvvvvv
weight: 10255
loglevle: vvvvvv
casename: c0025
relatedlink: ../../dvars/c0025
log_ref_file: ./reflogs/c0025_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0025_vvvvvvv
weight: 10256
loglevle: vvvvvvv
casename: c0025
relatedlink: ../../dvars/c0025
log_ref_file: ./reflogs/c0025_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0026.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
463
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0026_v
weight: 10260
loglevle: v
casename: c0026
relatedlink: ../../shell-func/c0026
log_ref_file: ./reflogs/c0026_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0026_vv
weight: 10261
loglevle: vv
casename: c0026
relatedlink: ../../shell-func/c0026
log_ref_file: ./reflogs/c0026_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0026_vvv
weight: 10262
loglevle: vvv
casename: c0026
relatedlink: ../../shell-func/c0026
log_ref_file: ./reflogs/c0026_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0026_vvvv
weight: 10263
loglevle: vvvv
casename: c0026
relatedlink: ../../shell-func/c0026
log_ref_file: ./reflogs/c0026_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0026_vvvvv
weight: 10264
loglevle: vvvvv
casename: c0026
relatedlink: ../../shell-func/c0026
log_ref_file: ./reflogs/c0026_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0026_vvvvvv
weight: 10265
loglevle: vvvvvv
casename: c0026
relatedlink: ../../shell-func/c0026
log_ref_file: ./reflogs/c0026_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0026_vvvvvvv
weight: 10266
loglevle: vvvvvvv
casename: c0026
relatedlink: ../../shell-func/c0026
log_ref_file: ./reflogs/c0026_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0027.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
702
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0027_v
weight: 10270
loglevle: v
casename: c0027
relatedlink: ../../dvars/c0027
log_ref_file: ./reflogs/c0027_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0027_vv
weight: 10271
loglevle: vv
casename: c0027
relatedlink: ../../dvars/c0027
log_ref_file: ./reflogs/c0027_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0027_vvv
weight: 10272
loglevle: vvv
casename: c0027
relatedlink: ../../dvars/c0027
log_ref_file: ./reflogs/c0027_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0027_vvvv
weight: 10273
loglevle: vvvv
casename: c0027
relatedlink: ../../dvars/c0027
log_ref_file: ./reflogs/c0027_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0027_vvvvv
weight: 10274
loglevle: vvvvv
casename: c0027
relatedlink: ../../dvars/c0027
log_ref_file: ./reflogs/c0027_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0027_vvvvvv
weight: 10275
loglevle: vvvvvv
casename: c0027
relatedlink: ../../dvars/c0027
log_ref_file: ./reflogs/c0027_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0027_vvvvvvv
weight: 10276
loglevle: vvvvvvv
casename: c0027
relatedlink: ../../dvars/c0027
log_ref_file: ./reflogs/c0027_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0028.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
488
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0028_v
weight: 10280
loglevle: v
casename: c0028
relatedlink: ../../dvars/c0028
log_ref_file: ./reflogs/c0028_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0028_vv
weight: 10281
loglevle: vv
casename: c0028
relatedlink: ../../dvars/c0028
log_ref_file: ./reflogs/c0028_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0028_vvv
weight: 10282
loglevle: vvv
casename: c0028
relatedlink: ../../dvars/c0028
log_ref_file: ./reflogs/c0028_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0028_vvvv
weight: 10283
loglevle: vvvv
casename: c0028
relatedlink: ../../dvars/c0028
log_ref_file: ./reflogs/c0028_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0028_vvvvv
weight: 10284
loglevle: vvvvv
casename: c0028
relatedlink: ../../dvars/c0028
log_ref_file: ./reflogs/c0028_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0028_vvvvvv
weight: 10285
loglevle: vvvvvv
casename: c0028
relatedlink: ../../dvars/c0028
log_ref_file: ./reflogs/c0028_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0028_vvvvvvv
weight: 10286
loglevle: vvvvvvv
casename: c0028
relatedlink: ../../dvars/c0028
log_ref_file: ./reflogs/c0028_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0029.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
429
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0029_v
weight: 10290
loglevle: v
casename: c0029
relatedlink: ../../dvars/c0029
log_ref_file: ./reflogs/c0029_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0029_vv
weight: 10291
loglevle: vv
casename: c0029
relatedlink: ../../dvars/c0029
log_ref_file: ./reflogs/c0029_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0029_vvv
weight: 10292
loglevle: vvv
casename: c0029
relatedlink: ../../dvars/c0029
log_ref_file: ./reflogs/c0029_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0029_vvvv
weight: 10293
loglevle: vvvv
casename: c0029
relatedlink: ../../dvars/c0029
log_ref_file: ./reflogs/c0029_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0029_vvvvv
weight: 10294
loglevle: vvvvv
casename: c0029
relatedlink: ../../dvars/c0029
log_ref_file: ./reflogs/c0029_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0029_vvvvvv
weight: 10295
loglevle: vvvvvv
casename: c0029
relatedlink: ../../dvars/c0029
log_ref_file: ./reflogs/c0029_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0029_vvvvvvv
weight: 10296
loglevle: vvvvvvv
casename: c0029
relatedlink: ../../dvars/c0029
log_ref_file: ./reflogs/c0029_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0030.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
540
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0030_v
weight: 10300
loglevle: v
casename: c0030
relatedlink: ../../dvars/c0030
log_ref_file: ./reflogs/c0030_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0030_vv
weight: 10301
loglevle: vv
casename: c0030
relatedlink: ../../dvars/c0030
log_ref_file: ./reflogs/c0030_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0030_vvv
weight: 10302
loglevle: vvv
casename: c0030
relatedlink: ../../dvars/c0030
log_ref_file: ./reflogs/c0030_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0030_vvvv
weight: 10303
loglevle: vvvv
casename: c0030
relatedlink: ../../dvars/c0030
log_ref_file: ./reflogs/c0030_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0030_vvvvv
weight: 10304
loglevle: vvvvv
casename: c0030
relatedlink: ../../dvars/c0030
log_ref_file: ./reflogs/c0030_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0030_vvvvvv
weight: 10305
loglevle: vvvvvv
casename: c0030
relatedlink: ../../dvars/c0030
log_ref_file: ./reflogs/c0030_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0030_vvvvvvv
weight: 10306
loglevle: vvvvvvv
casename: c0030
relatedlink: ../../dvars/c0030
log_ref_file: ./reflogs/c0030_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0031.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
334
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0031_v
weight: 10310
loglevle: v
casename: c0031
relatedlink: ../../dvars/c0031
log_ref_file: ./reflogs/c0031_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0031_vv
weight: 10311
loglevle: vv
casename: c0031
relatedlink: ../../dvars/c0031
log_ref_file: ./reflogs/c0031_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0031_vvv
weight: 10312
loglevle: vvv
casename: c0031
relatedlink: ../../dvars/c0031
log_ref_file: ./reflogs/c0031_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0031_vvvv
weight: 10313
loglevle: vvvv
casename: c0031
relatedlink: ../../dvars/c0031
log_ref_file: ./reflogs/c0031_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0031_vvvvv
weight: 10314
loglevle: vvvvv
casename: c0031
relatedlink: ../../dvars/c0031
log_ref_file: ./reflogs/c0031_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0031_vvvvvv
weight: 10315
loglevle: vvvvvv
casename: c0031
relatedlink: ../../dvars/c0031
log_ref_file: ./reflogs/c0031_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0031_vvvvvvv
weight: 10316
loglevle: vvvvvvv
casename: c0031
relatedlink: ../../dvars/c0031
log_ref_file: ./reflogs/c0031_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0032.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
552
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0032_v
weight: 10320
loglevle: v
casename: c0032
relatedlink: ../../dvars/c0032
log_ref_file: ./reflogs/c0032_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0032_vv
weight: 10321
loglevle: vv
casename: c0032
relatedlink: ../../dvars/c0032
log_ref_file: ./reflogs/c0032_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0032_vvv
weight: 10322
loglevle: vvv
casename: c0032
relatedlink: ../../dvars/c0032
log_ref_file: ./reflogs/c0032_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0032_vvvv
weight: 10323
loglevle: vvvv
casename: c0032
relatedlink: ../../dvars/c0032
log_ref_file: ./reflogs/c0032_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0032_vvvvv
weight: 10324
loglevle: vvvvv
casename: c0032
relatedlink: ../../dvars/c0032
log_ref_file: ./reflogs/c0032_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0032_vvvvvv
weight: 10325
loglevle: vvvvvv
casename: c0032
relatedlink: ../../dvars/c0032
log_ref_file: ./reflogs/c0032_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0032_vvvvvvv
weight: 10326
loglevle: vvvvvvv
casename: c0032
relatedlink: ../../dvars/c0032
log_ref_file: ./reflogs/c0032_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0033.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
279
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0033_v
weight: 10330
loglevle: v
casename: c0033
relatedlink: ../../dvars/c0033
log_ref_file: ./reflogs/c0033_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0033_vv
weight: 10331
loglevle: vv
casename: c0033
relatedlink: ../../dvars/c0033
log_ref_file: ./reflogs/c0033_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0033_vvv
weight: 10332
loglevle: vvv
casename: c0033
relatedlink: ../../dvars/c0033
log_ref_file: ./reflogs/c0033_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0033_vvvv
weight: 10333
loglevle: vvvv
casename: c0033
relatedlink: ../../dvars/c0033
log_ref_file: ./reflogs/c0033_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0033_vvvvv
weight: 10334
loglevle: vvvvv
casename: c0033
relatedlink: ../../dvars/c0033
log_ref_file: ./reflogs/c0033_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0033_vvvvvv
weight: 10335
loglevle: vvvvvv
casename: c0033
relatedlink: ../../dvars/c0033
log_ref_file: ./reflogs/c0033_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0033_vvvvvvv
weight: 10336
loglevle: vvvvvvv
casename: c0033
relatedlink: ../../dvars/c0033
log_ref_file: ./reflogs/c0033_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0034.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
332
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0034_v
weight: 10340
loglevle: v
casename: c0034
relatedlink: ../../dvars/c0034
log_ref_file: ./reflogs/c0034_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0034_vv
weight: 10341
loglevle: vv
casename: c0034
relatedlink: ../../dvars/c0034
log_ref_file: ./reflogs/c0034_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0034_vvv
weight: 10342
loglevle: vvv
casename: c0034
relatedlink: ../../dvars/c0034
log_ref_file: ./reflogs/c0034_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0034_vvvv
weight: 10343
loglevle: vvvv
casename: c0034
relatedlink: ../../dvars/c0034
log_ref_file: ./reflogs/c0034_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0034_vvvvv
weight: 10344
loglevle: vvvvv
casename: c0034
relatedlink: ../../dvars/c0034
log_ref_file: ./reflogs/c0034_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0034_vvvvvv
weight: 10345
loglevle: vvvvvv
casename: c0034
relatedlink: ../../dvars/c0034
log_ref_file: ./reflogs/c0034_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0034_vvvvvvv
weight: 10346
loglevle: vvvvvvv
casename: c0034
relatedlink: ../../dvars/c0034
log_ref_file: ./reflogs/c0034_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0035.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
357
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0035_v
weight: 10350
loglevle: v
casename: c0035
relatedlink: ../../vars/c0035
log_ref_file: ./reflogs/c0035_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0035_vv
weight: 10351
loglevle: vv
casename: c0035
relatedlink: ../../vars/c0035
log_ref_file: ./reflogs/c0035_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0035_vvv
weight: 10352
loglevle: vvv
casename: c0035
relatedlink: ../../vars/c0035
log_ref_file: ./reflogs/c0035_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0035_vvvv
weight: 10353
loglevle: vvvv
casename: c0035
relatedlink: ../../vars/c0035
log_ref_file: ./reflogs/c0035_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0035_vvvvv
weight: 10354
loglevle: vvvvv
casename: c0035
relatedlink: ../../vars/c0035
log_ref_file: ./reflogs/c0035_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0035_vvvvvv
weight: 10355
loglevle: vvvvvv
casename: c0035
relatedlink: ../../vars/c0035
log_ref_file: ./reflogs/c0035_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0035_vvvvvvv
weight: 10356
loglevle: vvvvvvv
casename: c0035
relatedlink: ../../vars/c0035
log_ref_file: ./reflogs/c0035_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0036.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
597
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0036_v
weight: 10360
loglevle: v
casename: c0036
relatedlink: ../../template/c0036
log_ref_file: ./reflogs/c0036_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0036_vv
weight: 10361
loglevle: vv
casename: c0036
relatedlink: ../../template/c0036
log_ref_file: ./reflogs/c0036_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0036_vvv
weight: 10362
loglevle: vvv
casename: c0036
relatedlink: ../../template/c0036
log_ref_file: ./reflogs/c0036_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0036_vvvv
weight: 10363
loglevle: vvvv
casename: c0036
relatedlink: ../../template/c0036
log_ref_file: ./reflogs/c0036_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0036_vvvvv
weight: 10364
loglevle: vvvvv
casename: c0036
relatedlink: ../../template/c0036
log_ref_file: ./reflogs/c0036_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0036_vvvvvv
weight: 10365
loglevle: vvvvvv
casename: c0036
relatedlink: ../../template/c0036
log_ref_file: ./reflogs/c0036_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0036_vvvvvvv
weight: 10366
loglevle: vvvvvvv
casename: c0036
relatedlink: ../../template/c0036
log_ref_file: ./reflogs/c0036_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0037.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
1568
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0037_v
weight: 10370
loglevle: v
casename: c0037
relatedlink: ../../template/c0037
log_ref_file: ./reflogs/c0037_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0037_vv
weight: 10371
loglevle: vv
casename: c0037
relatedlink: ../../template/c0037
log_ref_file: ./reflogs/c0037_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0037_vvv
weight: 10372
loglevle: vvv
casename: c0037
relatedlink: ../../template/c0037
log_ref_file: ./reflogs/c0037_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0037_vvvv
weight: 10373
loglevle: vvvv
casename: c0037
relatedlink: ../../template/c0037
log_ref_file: ./reflogs/c0037_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0037_vvvvv
weight: 10374
loglevle: vvvvv
casename: c0037
relatedlink: ../../template/c0037
log_ref_file: ./reflogs/c0037_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0037_vvvvvv
weight: 10375
loglevle: vvvvvv
casename: c0037
relatedlink: ../../template/c0037
log_ref_file: ./reflogs/c0037_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0037_vvvvvvv
weight: 10376
loglevle: vvvvvvv
casename: c0037
relatedlink: ../../template/c0037
log_ref_file: ./reflogs/c0037_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0038.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
498
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0038_v
weight: 10380
loglevle: v
casename: c0038
relatedlink: ../../template/c0038
log_ref_file: ./reflogs/c0038_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0038_vv
weight: 10381
loglevle: vv
casename: c0038
relatedlink: ../../template/c0038
log_ref_file: ./reflogs/c0038_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0038_vvv
weight: 10382
loglevle: vvv
casename: c0038
relatedlink: ../../template/c0038
log_ref_file: ./reflogs/c0038_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0038_vvvv
weight: 10383
loglevle: vvvv
casename: c0038
relatedlink: ../../template/c0038
log_ref_file: ./reflogs/c0038_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0038_vvvvv
weight: 10384
loglevle: vvvvv
casename: c0038
relatedlink: ../../template/c0038
log_ref_file: ./reflogs/c0038_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0038_vvvvvv
weight: 10385
loglevle: vvvvvv
casename: c0038
relatedlink: ../../template/c0038
log_ref_file: ./reflogs/c0038_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0038_vvvvvvv
weight: 10386
loglevle: vvvvvvv
casename: c0038
relatedlink: ../../template/c0038
log_ref_file: ./reflogs/c0038_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0039.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
370
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0039_v
weight: 10390
loglevle: v
casename: c0039
relatedlink: ../../template/c0039
log_ref_file: ./reflogs/c0039_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0039_vv
weight: 10391
loglevle: vv
casename: c0039
relatedlink: ../../template/c0039
log_ref_file: ./reflogs/c0039_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0039_vvv
weight: 10392
loglevle: vvv
casename: c0039
relatedlink: ../../template/c0039
log_ref_file: ./reflogs/c0039_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0039_vvvv
weight: 10393
loglevle: vvvv
casename: c0039
relatedlink: ../../template/c0039
log_ref_file: ./reflogs/c0039_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0039_vvvvv
weight: 10394
loglevle: vvvvv
casename: c0039
relatedlink: ../../template/c0039
log_ref_file: ./reflogs/c0039_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0039_vvvvvv
weight: 10395
loglevle: vvvvvv
casename: c0039
relatedlink: ../../template/c0039
log_ref_file: ./reflogs/c0039_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0039_vvvvvvv
weight: 10396
loglevle: vvvvvvv
casename: c0039
relatedlink: ../../template/c0039
log_ref_file: ./reflogs/c0039_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0040.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
256
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0040_v
weight: 10400
loglevle: v
casename: c0040
relatedlink: ../../template/c0040
log_ref_file: ./reflogs/c0040_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0040_vv
weight: 10401
loglevle: vv
casename: c0040
relatedlink: ../../template/c0040
log_ref_file: ./reflogs/c0040_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0040_vvv
weight: 10402
loglevle: vvv
casename: c0040
relatedlink: ../../template/c0040
log_ref_file: ./reflogs/c0040_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0040_vvvv
weight: 10403
loglevle: vvvv
casename: c0040
relatedlink: ../../template/c0040
log_ref_file: ./reflogs/c0040_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0040_vvvvv
weight: 10404
loglevle: vvvvv
casename: c0040
relatedlink: ../../template/c0040
log_ref_file: ./reflogs/c0040_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0040_vvvvvv
weight: 10405
loglevle: vvvvvv
casename: c0040
relatedlink: ../../template/c0040
log_ref_file: ./reflogs/c0040_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0040_vvvvvvv
weight: 10406
loglevle: vvvvvvv
casename: c0040
relatedlink: ../../template/c0040
log_ref_file: ./reflogs/c0040_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0041.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
349
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0041_v
weight: 10410
loglevle: v
casename: c0041
relatedlink: ../../shell-func/c0041
log_ref_file: ./reflogs/c0041_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0041_vv
weight: 10411
loglevle: vv
casename: c0041
relatedlink: ../../shell-func/c0041
log_ref_file: ./reflogs/c0041_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0041_vvv
weight: 10412
loglevle: vvv
casename: c0041
relatedlink: ../../shell-func/c0041
log_ref_file: ./reflogs/c0041_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0041_vvvv
weight: 10413
loglevle: vvvv
casename: c0041
relatedlink: ../../shell-func/c0041
log_ref_file: ./reflogs/c0041_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0041_vvvvv
weight: 10414
loglevle: vvvvv
casename: c0041
relatedlink: ../../shell-func/c0041
log_ref_file: ./reflogs/c0041_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0041_vvvvvv
weight: 10415
loglevle: vvvvvv
casename: c0041
relatedlink: ../../shell-func/c0041
log_ref_file: ./reflogs/c0041_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0041_vvvvvvv
weight: 10416
loglevle: vvvvvvv
casename: c0041
relatedlink: ../../shell-func/c0041
log_ref_file: ./reflogs/c0041_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0042.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
905
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0042_v
weight: 10420
loglevle: v
casename: c0042
relatedlink: ../../template/c0042
log_ref_file: ./reflogs/c0042_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0042_vv
weight: 10421
loglevle: vv
casename: c0042
relatedlink: ../../template/c0042
log_ref_file: ./reflogs/c0042_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0042_vvv
weight: 10422
loglevle: vvv
casename: c0042
relatedlink: ../../template/c0042
log_ref_file: ./reflogs/c0042_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0042_vvvv
weight: 10423
loglevle: vvvv
casename: c0042
relatedlink: ../../template/c0042
log_ref_file: ./reflogs/c0042_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0042_vvvvv
weight: 10424
loglevle: vvvvv
casename: c0042
relatedlink: ../../template/c0042
log_ref_file: ./reflogs/c0042_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0042_vvvvvv
weight: 10425
loglevle: vvvvvv
casename: c0042
relatedlink: ../../template/c0042
log_ref_file: ./reflogs/c0042_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0042_vvvvvvv
weight: 10426
loglevle: vvvvvvv
casename: c0042
relatedlink: ../../template/c0042
log_ref_file: ./reflogs/c0042_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0043.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
723
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0043_v
weight: 10430
loglevle: v
casename: c0043
relatedlink: ../../design-patterns/c0043
log_ref_file: ./reflogs/c0043_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0043_vv
weight: 10431
loglevle: vv
casename: c0043
relatedlink: ../../design-patterns/c0043
log_ref_file: ./reflogs/c0043_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0043_vvv
weight: 10432
loglevle: vvv
casename: c0043
relatedlink: ../../design-patterns/c0043
log_ref_file: ./reflogs/c0043_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0043_vvvv
weight: 10433
loglevle: vvvv
casename: c0043
relatedlink: ../../design-patterns/c0043
log_ref_file: ./reflogs/c0043_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0043_vvvvv
weight: 10434
loglevle: vvvvv
casename: c0043
relatedlink: ../../design-patterns/c0043
log_ref_file: ./reflogs/c0043_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0043_vvvvvv
weight: 10435
loglevle: vvvvvv
casename: c0043
relatedlink: ../../design-patterns/c0043
log_ref_file: ./reflogs/c0043_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0043_vvvvvvv
weight: 10436
loglevle: vvvvvvv
casename: c0043
relatedlink: ../../design-patterns/c0043
log_ref_file: ./reflogs/c0043_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0044.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
286
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0044_v
weight: 10440
loglevle: v
casename: c0044
relatedlink: ../../env-vars/c0044
log_ref_file: ./reflogs/c0044_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0044_vv
weight: 10441
loglevle: vv
casename: c0044
relatedlink: ../../env-vars/c0044
log_ref_file: ./reflogs/c0044_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0044_vvv
weight: 10442
loglevle: vvv
casename: c0044
relatedlink: ../../env-vars/c0044
log_ref_file: ./reflogs/c0044_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0044_vvvv
weight: 10443
loglevle: vvvv
casename: c0044
relatedlink: ../../env-vars/c0044
log_ref_file: ./reflogs/c0044_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0044_vvvvv
weight: 10444
loglevle: vvvvv
casename: c0044
relatedlink: ../../env-vars/c0044
log_ref_file: ./reflogs/c0044_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0044_vvvvvv
weight: 10445
loglevle: vvvvvv
casename: c0044
relatedlink: ../../env-vars/c0044
log_ref_file: ./reflogs/c0044_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0044_vvvvvvv
weight: 10446
loglevle: vvvvvvv
casename: c0044
relatedlink: ../../env-vars/c0044
log_ref_file: ./reflogs/c0044_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0046.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
370
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0046_v
weight: 10460
loglevle: v
casename: c0046
relatedlink: ../../env-vars/c0046
log_ref_file: ./reflogs/c0046_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0046_vv
weight: 10461
loglevle: vv
casename: c0046
relatedlink: ../../env-vars/c0046
log_ref_file: ./reflogs/c0046_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0046_vvv
weight: 10462
loglevle: vvv
casename: c0046
relatedlink: ../../env-vars/c0046
log_ref_file: ./reflogs/c0046_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0046_vvvv
weight: 10463
loglevle: vvvv
casename: c0046
relatedlink: ../../env-vars/c0046
log_ref_file: ./reflogs/c0046_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0046_vvvvv
weight: 10464
loglevle: vvvvv
casename: c0046
relatedlink: ../../env-vars/c0046
log_ref_file: ./reflogs/c0046_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0046_vvvvvv
weight: 10465
loglevle: vvvvvv
casename: c0046
relatedlink: ../../env-vars/c0046
log_ref_file: ./reflogs/c0046_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0046_vvvvvvv
weight: 10466
loglevle: vvvvvvv
casename: c0046
relatedlink: ../../env-vars/c0046
log_ref_file: ./reflogs/c0046_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0047.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
493
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0047_v
weight: 10470
loglevle: v
casename: c0047
relatedlink: ../../design-patterns/c0047
log_ref_file: ./reflogs/c0047_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0047_vv
weight: 10471
loglevle: vv
casename: c0047
relatedlink: ../../design-patterns/c0047
log_ref_file: ./reflogs/c0047_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0047_vvv
weight: 10472
loglevle: vvv
casename: c0047
relatedlink: ../../design-patterns/c0047
log_ref_file: ./reflogs/c0047_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0047_vvvv
weight: 10473
loglevle: vvvv
casename: c0047
relatedlink: ../../design-patterns/c0047
log_ref_file: ./reflogs/c0047_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0047_vvvvv
weight: 10474
loglevle: vvvvv
casename: c0047
relatedlink: ../../design-patterns/c0047
log_ref_file: ./reflogs/c0047_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0047_vvvvvv
weight: 10475
loglevle: vvvvvv
casename: c0047
relatedlink: ../../design-patterns/c0047
log_ref_file: ./reflogs/c0047_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0047_vvvvvvv
weight: 10476
loglevle: vvvvvvv
casename: c0047
relatedlink: ../../design-patterns/c0047
log_ref_file: ./reflogs/c0047_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0048.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
380
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0048_v
weight: 10480
loglevle: v
casename: c0048
relatedlink: ../../env-vars/c0048
log_ref_file: ./reflogs/c0048_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0048_vv
weight: 10481
loglevle: vv
casename: c0048
relatedlink: ../../env-vars/c0048
log_ref_file: ./reflogs/c0048_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0048_vvv
weight: 10482
loglevle: vvv
casename: c0048
relatedlink: ../../env-vars/c0048
log_ref_file: ./reflogs/c0048_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0048_vvvv
weight: 10483
loglevle: vvvv
casename: c0048
relatedlink: ../../env-vars/c0048
log_ref_file: ./reflogs/c0048_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0048_vvvvv
weight: 10484
loglevle: vvvvv
casename: c0048
relatedlink: ../../env-vars/c0048
log_ref_file: ./reflogs/c0048_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0048_vvvvvv
weight: 10485
loglevle: vvvvvv
casename: c0048
relatedlink: ../../env-vars/c0048
log_ref_file: ./reflogs/c0048_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0048_vvvvvvv
weight: 10486
loglevle: vvvvvvv
casename: c0048
relatedlink: ../../env-vars/c0048
log_ref_file: ./reflogs/c0048_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0049.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
787
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0049_v
weight: 10490
loglevle: v
casename: c0049
relatedlink: ../../env-vars/c0049
log_ref_file: ./reflogs/c0049_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0049_vv
weight: 10491
loglevle: vv
casename: c0049
relatedlink: ../../env-vars/c0049
log_ref_file: ./reflogs/c0049_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0049_vvv
weight: 10492
loglevle: vvv
casename: c0049
relatedlink: ../../env-vars/c0049
log_ref_file: ./reflogs/c0049_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0049_vvvv
weight: 10493
loglevle: vvvv
casename: c0049
relatedlink: ../../env-vars/c0049
log_ref_file: ./reflogs/c0049_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0049_vvvvv
weight: 10494
loglevle: vvvvv
casename: c0049
relatedlink: ../../env-vars/c0049
log_ref_file: ./reflogs/c0049_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0049_vvvvvv
weight: 10495
loglevle: vvvvvv
casename: c0049
relatedlink: ../../env-vars/c0049
log_ref_file: ./reflogs/c0049_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0049_vvvvvvv
weight: 10496
loglevle: vvvvvvv
casename: c0049
relatedlink: ../../env-vars/c0049
log_ref_file: ./reflogs/c0049_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0050.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
451
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0050_v
weight: 10500
loglevle: v
casename: c0050
relatedlink: ../../dvars/c0050
log_ref_file: ./reflogs/c0050_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0050_vv
weight: 10501
loglevle: vv
casename: c0050
relatedlink: ../../dvars/c0050
log_ref_file: ./reflogs/c0050_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0050_vvv
weight: 10502
loglevle: vvv
casename: c0050
relatedlink: ../../dvars/c0050
log_ref_file: ./reflogs/c0050_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0050_vvvv
weight: 10503
loglevle: vvvv
casename: c0050
relatedlink: ../../dvars/c0050
log_ref_file: ./reflogs/c0050_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0050_vvvvv
weight: 10504
loglevle: vvvvv
casename: c0050
relatedlink: ../../dvars/c0050
log_ref_file: ./reflogs/c0050_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0050_vvvvvv
weight: 10505
loglevle: vvvvvv
casename: c0050
relatedlink: ../../dvars/c0050
log_ref_file: ./reflogs/c0050_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0050_vvvvvvv
weight: 10506
loglevle: vvvvvvv
casename: c0050
relatedlink: ../../dvars/c0050
log_ref_file: ./reflogs/c0050_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0051.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
1502
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0051_v
weight: 10510
loglevle: v
casename: c0051
relatedlink: ../../security/c0051
log_ref_file: ./reflogs/c0051_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0051_vv
weight: 10511
loglevle: vv
casename: c0051
relatedlink: ../../security/c0051
log_ref_file: ./reflogs/c0051_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0051_vvv
weight: 10512
loglevle: vvv
casename: c0051
relatedlink: ../../security/c0051
log_ref_file: ./reflogs/c0051_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0051_vvvv
weight: 10513
loglevle: vvvv
casename: c0051
relatedlink: ../../security/c0051
log_ref_file: ./reflogs/c0051_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0051_vvvvv
weight: 10514
loglevle: vvvvv
casename: c0051
relatedlink: ../../security/c0051
log_ref_file: ./reflogs/c0051_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0051_vvvvvv
weight: 10515
loglevle: vvvvvv
casename: c0051
relatedlink: ../../security/c0051
log_ref_file: ./reflogs/c0051_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0051_vvvvvvv
weight: 10516
loglevle: vvvvvvv
casename: c0051
relatedlink: ../../security/c0051
log_ref_file: ./reflogs/c0051_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0052.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
450
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0052_v
weight: 10520
loglevle: v
casename: c0052
relatedlink: ../../shell-func/c0052
log_ref_file: ./reflogs/c0052_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0052_vv
weight: 10521
loglevle: vv
casename: c0052
relatedlink: ../../shell-func/c0052
log_ref_file: ./reflogs/c0052_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0052_vvv
weight: 10522
loglevle: vvv
casename: c0052
relatedlink: ../../shell-func/c0052
log_ref_file: ./reflogs/c0052_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0052_vvvv
weight: 10523
loglevle: vvvv
casename: c0052
relatedlink: ../../shell-func/c0052
log_ref_file: ./reflogs/c0052_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0052_vvvvv
weight: 10524
loglevle: vvvvv
casename: c0052
relatedlink: ../../shell-func/c0052
log_ref_file: ./reflogs/c0052_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0052_vvvvvv
weight: 10525
loglevle: vvvvvv
casename: c0052
relatedlink: ../../shell-func/c0052
log_ref_file: ./reflogs/c0052_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0052_vvvvvvv
weight: 10526
loglevle: vvvvvvv
casename: c0052
relatedlink: ../../shell-func/c0052
log_ref_file: ./reflogs/c0052_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0054.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
418
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0054_v
weight: 10540
loglevle: v
casename: c0054
relatedlink: ../../flow-controll/c0054
log_ref_file: ./reflogs/c0054_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0054_vv
weight: 10541
loglevle: vv
casename: c0054
relatedlink: ../../flow-controll/c0054
log_ref_file: ./reflogs/c0054_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0054_vvv
weight: 10542
loglevle: vvv
casename: c0054
relatedlink: ../../flow-controll/c0054
log_ref_file: ./reflogs/c0054_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0054_vvvv
weight: 10543
loglevle: vvvv
casename: c0054
relatedlink: ../../flow-controll/c0054
log_ref_file: ./reflogs/c0054_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0054_vvvvv
weight: 10544
loglevle: vvvvv
casename: c0054
relatedlink: ../../flow-controll/c0054
log_ref_file: ./reflogs/c0054_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0054_vvvvvv
weight: 10545
loglevle: vvvvvv
casename: c0054
relatedlink: ../../flow-controll/c0054
log_ref_file: ./reflogs/c0054_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0054_vvvvvvv
weight: 10546
loglevle: vvvvvvv
casename: c0054
relatedlink: ../../flow-controll/c0054
log_ref_file: ./reflogs/c0054_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0055.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
837
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0055_v
weight: 10550
loglevle: v
casename: c0055
relatedlink: ../../flow-controll/c0055
log_ref_file: ./reflogs/c0055_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0055_vv
weight: 10551
loglevle: vv
casename: c0055
relatedlink: ../../flow-controll/c0055
log_ref_file: ./reflogs/c0055_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0055_vvv
weight: 10552
loglevle: vvv
casename: c0055
relatedlink: ../../flow-controll/c0055
log_ref_file: ./reflogs/c0055_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0055_vvvv
weight: 10553
loglevle: vvvv
casename: c0055
relatedlink: ../../flow-controll/c0055
log_ref_file: ./reflogs/c0055_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0055_vvvvv
weight: 10554
loglevle: vvvvv
casename: c0055
relatedlink: ../../flow-controll/c0055
log_ref_file: ./reflogs/c0055_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0055_vvvvvv
weight: 10555
loglevle: vvvvvv
casename: c0055
relatedlink: ../../flow-controll/c0055
log_ref_file: ./reflogs/c0055_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0055_vvvvvvv
weight: 10556
loglevle: vvvvvvv
casename: c0055
relatedlink: ../../flow-controll/c0055
log_ref_file: ./reflogs/c0055_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0056.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 1400

weight string length: 5

----Step2:
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
1169
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0056_v
weight: 10560
loglevle: v
casename: c0056
relatedlink: ../../loop/c0056
log_ref_file: ./reflogs/c0056_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0056_vv
weight: 10561
loglevle: vv
casename: c0056
relatedlink: ../../loop/c0056
log_ref_file: ./reflogs/c0056_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0056_vvv
weight: 10562
loglevle: vvv
casename: c0056
relatedlink: ../../loop/c0056
log_ref_file: ./reflogs/c0056_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0056_vvvv
weight: 10563
loglevle: vvvv
casename: c0056
relatedlink: ../../loop/c0056
log_ref_file: ./reflogs/c0056_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0056_vvvvv
weight: 10564
loglevle: vvvvv
casename: c0056
relatedlink: ../../loop/c0056
log_ref_file: ./reflogs/c0056_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0056_vvvvvv
weight: 10565
loglevle: vvvvvv
casename: c0056
relatedlink: ../../loop/c0056
log_ref_file: ./reflogs/c0056_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0056_vvvvvvv
weight: 10566
loglevle: vvvvvvv
casename: c0056
relatedlink: ../../loop/c0056
log_ref_file: ./reflogs/c0056_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0059.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
622
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0059_v
weight: 10590
loglevle: v
casename: c0059
relatedlink: ../../organization/c0059
log_ref_file: ./reflogs/c0059_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0059_vv
weight: 10591
loglevle: vv
casename: c0059
relatedlink: ../../organization/c0059
log_ref_file: ./reflogs/c0059_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0059_vvv
weight: 10592
loglevle: vvv
casename: c0059
relatedlink: ../../organization/c0059
log_ref_file: ./reflogs/c0059_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0059_vvvv
weight: 10593
loglevle: vvvv
casename: c0059
relatedlink: ../../organization/c0059
log_ref_file: ./reflogs/c0059_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0059_vvvvv
weight: 10594
loglevle: vvvvv
casename: c0059
relatedlink: ../../organization/c0059
log_ref_file: ./reflogs/c0059_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0059_vvvvvv
weight: 10595
loglevle: vvvvvv
casename: c0059
relatedlink: ../../organization/c0059
log_ref_file: ./reflogs/c0059_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0059_vvvvvvv
weight: 10596
loglevle: vvvvvvv
casename: c0059
relatedlink: ../../organization/c0059
log_ref_file: ./reflogs/c0059_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0060.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
711
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0060_v
weight: 10600
loglevle: v
casename: c0060
relatedlink: ../../organization/c0060
log_ref_file: ./reflogs/c0060_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0060_vv
weight: 10601
loglevle: vv
casename: c0060
relatedlink: ../../organization/c0060
log_ref_file: ./reflogs/c0060_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0060_vvv
weight: 10602
loglevle: vvv
casename: c0060
relatedlink: ../../organization/c0060
log_ref_file: ./reflogs/c0060_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0060_vvvv
weight: 10603
loglevle: vvvv
casename: c0060
relatedlink: ../../organization/c0060
log_ref_file: ./reflogs/c0060_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0060_vvvvv
weight: 10604
loglevle: vvvvv
casename: c0060
relatedlink: ../../organization/c0060
log_ref_file: ./reflogs/c0060_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0060_vvvvvv
weight: 10605
loglevle: vvvvvv
casename: c0060
relatedlink: ../../organization/c0060
log_ref_file: ./reflogs/c0060_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0060_vvvvvvv
weight: 10606
loglevle: vvvvvvv
casename: c0060
relatedlink: ../../organization/c0060
log_ref_file: ./reflogs/c0060_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0061.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
823
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0061_v
weight: 10610
loglevle: v
casename: c0061
relatedlink: ../../organization/c0061
log_ref_file: ./reflogs/c0061_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0061_vv
weight: 10611
loglevle: vv
casename: c0061
relatedlink: ../../organization/c0061
log_ref_file: ./reflogs/c0061_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0061_vvv
weight: 10612
loglevle: vvv
casename: c0061
relatedlink: ../../organization/c0061
log_ref_file: ./reflogs/c0061_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0061_vvvv
weight: 10613
loglevle: vvvv
casename: c0061
relatedlink: ../../organization/c0061
log_ref_file: ./reflogs/c0061_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0061_vvvvv
weight: 10614
loglevle: vvvvv
casename: c0061
relatedlink: ../../organization/c0061
log_ref_file: ./reflogs/c0061_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0061_vvvvvv
weight: 10615
loglevle: vvvvvv
casename: c0061
relatedlink: ../../organization/c0061
log_ref_file: ./reflogs/c0061_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0061_vvvvvvv
weight: 10616
loglevle: vvvvvvv
casename: c0061
relatedlink: ../../organization/c0061
log_ref_file: ./reflogs/c0061_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0062.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
955
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0062_v
weight: 10620
loglevle: v
casename: c0062
relatedlink: ../../organization/c0062
log_ref_file: ./reflogs/c0062_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0062_vv
weight: 10621
loglevle: vv
casename: c0062
relatedlink: ../../organization/c0062
log_ref_file: ./reflogs/c0062_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0062_vvv
weight: 10622
loglevle: vvv
casename: c0062
relatedlink: ../../organization/c0062
log_ref_file: ./reflogs/c0062_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0062_vvvv
weight: 10623
loglevle: vvvv
casename: c0062
relatedlink: ../../organization/c0062
log_ref_file: ./reflogs/c0062_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0062_vvvvv
weight: 10624
loglevle: vvvvv
casename: c0062
relatedlink: ../../organization/c0062
log_ref_file: ./reflogs/c0062_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0062_vvvvvv
weight: 10625
loglevle: vvvvvv
casename: c0062
relatedlink: ../../organization/c0062
log_ref_file: ./reflogs/c0062_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0062_vvvvvvv
weight: 10626
loglevle: vvvvvvv
casename: c0062
relatedlink: ../../organization/c0062
log_ref_file: ./reflogs/c0062_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0063.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
790
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0063_v
weight: 10630
loglevle: v
casename: c0063
relatedlink: ../../flow-controll/c0063
log_ref_file: ./reflogs/c0063_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0063_vv
weight: 10631
loglevle: vv
casename: c0063
relatedlink: ../../flow-controll/c0063
log_ref_file: ./reflogs/c0063_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0063_vvv
weight: 10632
loglevle: vvv
casename: c0063
relatedlink: ../../flow-controll/c0063
log_ref_file: ./reflogs/c0063_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0063_vvvv
weight: 10633
loglevle: vvvv
casename: c0063
relatedlink: ../../flow-controll/c0063
log_ref_file: ./reflogs/c0063_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0063_vvvvv
weight: 10634
loglevle: vvvvv
casename: c0063
relatedlink: ../../flow-controll/c0063
log_ref_file: ./reflogs/c0063_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0063_vvvvvv
weight: 10635
loglevle: vvvvvv
casename: c0063
relatedlink: ../../flow-controll/c0063
log_ref_file: ./reflogs/c0063_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0063_vvvvvvv
weight: 10636
loglevle: vvvvvvv
casename: c0063
relatedlink: ../../flow-controll/c0063
log_ref_file: ./reflogs/c0063_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0064.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
652
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0064_v
weight: 10640
loglevle: v
casename: c0064
relatedlink: ../../cmd-func/c0064
log_ref_file: ./reflogs/c0064_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0064_vv
weight: 10641
loglevle: vv
casename: c0064
relatedlink: ../../cmd-func/c0064
log_ref_file: ./reflogs/c0064_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0064_vvv
weight: 10642
loglevle: vvv
casename: c0064
relatedlink: ../../cmd-func/c0064
log_ref_file: ./reflogs/c0064_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0064_vvvv
weight: 10643
loglevle: vvvv
casename: c0064
relatedlink: ../../cmd-func/c0064
log_ref_file: ./reflogs/c0064_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0064_vvvvv
weight: 10644
loglevle: vvvvv
casename: c0064
relatedlink: ../../cmd-func/c0064
log_ref_file: ./reflogs/c0064_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0064_vvvvvv
weight: 10645
loglevle: vvvvvv
casename: c0064
relatedlink: ../../cmd-func/c0064
log_ref_file: ./reflogs/c0064_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0064_vvvvvvv
weight: 10646
loglevle: vvvvvvv
casename: c0064
relatedlink: ../../cmd-func/c0064
log_ref_file: ./reflogs/c0064_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0065.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
660
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0065_v
weight: 10650
loglevle: v
casename: c0065
relatedlink: ../../test-debug/c0065
log_ref_file: ./reflogs/c0065_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0065_vv
weight: 10651
loglevle: vv
casename: c0065
relatedlink: ../../test-debug/c0065
log_ref_file: ./reflogs/c0065_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0065_vvv
weight: 10652
loglevle: vvv
casename: c0065
relatedlink: ../../test-debug/c0065
log_ref_file: ./reflogs/c0065_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0065_vvvv
weight: 10653
loglevle: vvvv
casename: c0065
relatedlink: ../../test-debug/c0065
log_ref_file: ./reflogs/c0065_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0065_vvvvv
weight: 10654
loglevle: vvvvv
casename: c0065
relatedlink: ../../test-debug/c0065
log_ref_file: ./reflogs/c0065_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0065_vvvvvv
weight: 10655
loglevle: vvvvvv
casename: c0065
relatedlink: ../../test-debug/c0065
log_ref_file: ./reflogs/c0065_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0065_vvvvvvv
weight: 10656
loglevle: vvvvvvv
casename: c0065
relatedlink: ../../test-debug/c0065
log_ref_file: ./reflogs/c0065_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0066.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
1281
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0066_v
weight: 10660
loglevle: v
casename: c0066
relatedlink: ../../cmd-func/c0066
log_ref_file: ./reflogs/c0066_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0066_vv
weight: 10661
loglevle: vv
casename: c0066
relatedlink: ../../cmd-func/c0066
log_ref_file: ./reflogs/c0066_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0066_vvv
weight: 10662
loglevle: vvv
casename: c0066
relatedlink: ../../cmd-func/c0066
log_ref_file: ./reflogs/c0066_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0066_vvvv
weight: 10663
loglevle: vvvv
casename: c0066
relatedlink: ../../cmd-func/c0066
log_ref_file: ./reflogs/c0066_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0066_vvvvv
weight: 10664
loglevle: vvvvv
casename: c0066
relatedlink: ../../cmd-func/c0066
log_ref_file: ./reflogs/c0066_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0066_vvvvvv
weight: 10665
loglevle: vvvvvv
casename: c0066
relatedlink: ../../cmd-func/c0066
log_ref_file: ./reflogs/c0066_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0066_vvvvvvv
weight: 10666
loglevle: vvvvvvv
casename: c0066
relatedlink: ../../cmd-func/c0066
log_ref_file: ./reflogs/c0066_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0068.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
582
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0068_v
weight: 10680
loglevle: v
casename: c0068
relatedlink: ../../flow-controll/c0068
log_ref_file: ./reflogs/c0068_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0068_vv
weight: 10681
loglevle: vv
casename: c0068
relatedlink: ../../flow-controll/c0068
log_ref_file: ./reflogs/c0068_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0068_vvv
weight: 10682
loglevle: vvv
casename: c0068
relatedlink: ../../flow-controll/c0068
log_ref_file: ./reflogs/c0068_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0068_vvvv
weight: 10683
loglevle: vvvv
casename: c0068
relatedlink: ../../flow-controll/c0068
log_ref_file: ./reflogs/c0068_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0068_vvvvv
weight: 10684
loglevle: vvvvv
casename: c0068
relatedlink: ../../flow-controll/c0068
log_ref_file: ./reflogs/c0068_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0068_vvvvvv
weight: 10685
loglevle: vvvvvv
casename: c0068
relatedlink: ../../flow-controll/c0068
log_ref_file: ./reflogs/c0068_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0068_vvvvvvv
weight: 10686
loglevle: vvvvvvv
casename: c0068
relatedlink: ../../flow-controll/c0068
log_ref_file: ./reflogs/c0068_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0069.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
685
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0069_v
weight: 10690
loglevle: v
casename: c0069
relatedlink: ../../templating/c0069
log_ref_file: ./reflogs/c0069_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0069_vv
weight: 10691
loglevle: vv
casename: c0069
relatedlink: ../../templating/c0069
log_ref_file: ./reflogs/c0069_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0069_vvv
weight: 10692
loglevle: vvv
casename: c0069
relatedlink: ../../templating/c0069
log_ref_file: ./reflogs/c0069_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0069_vvvv
weight: 10693
loglevle: vvvv
casename: c0069
relatedlink: ../../templating/c0069
log_ref_file: ./reflogs/c0069_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0069_vvvvv
weight: 10694
loglevle: vvvvv
casename: c0069
relatedlink: ../../templating/c0069
log_ref_file: ./reflogs/c0069_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0069_vvvvvv
weight: 10695
loglevle: vvvvvv
casename: c0069
relatedlink: ../../templating/c0069
log_ref_file: ./reflogs/c0069_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0069_vvvvvvv
weight: 10696
loglevle: vvvvvvv
casename: c0069
relatedlink: ../../templating/c0069
log_ref_file: ./reflogs/c0069_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0070.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
530
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0070_v
weight: 10700
loglevle: v
casename: c0070
relatedlink: ../../dvars/c0070
log_ref_file: ./reflogs/c0070_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0070_vv
weight: 10701
loglevle: vv
casename: c0070
relatedlink: ../../dvars/c0070
log_ref_file: ./reflogs/c0070_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0070_vvv
weight: 10702
loglevle: vvv
casename: c0070
relatedlink: ../../dvars/c0070
log_ref_file: ./reflogs/c0070_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0070_vvvv
weight: 10703
loglevle: vvvv
casename: c0070
relatedlink: ../../dvars/c0070
log_ref_file: ./reflogs/c0070_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0070_vvvvv
weight: 10704
loglevle: vvvvv
casename: c0070
relatedlink: ../../dvars/c0070
log_ref_file: ./reflogs/c0070_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0070_vvvvvv
weight: 10705
loglevle: vvvvvv
casename: c0070
relatedlink: ../../dvars/c0070
log_ref_file: ./reflogs/c0070_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0070_vvvvvvv
weight: 10706
loglevle: vvvvvvv
casename: c0070
relatedlink: ../../dvars/c0070
log_ref_file: ./reflogs/c0070_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0071.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
692
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0071_v
weight: 10710
loglevle: v
casename: c0071
relatedlink: ../../cmd-func/c0071
log_ref_file: ./reflogs/c0071_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0071_vv
weight: 10711
loglevle: vv
casename: c0071
relatedlink: ../../cmd-func/c0071
log_ref_file: ./reflogs/c0071_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0071_vvv
weight: 10712
loglevle: vvv
casename: c0071
relatedlink: ../../cmd-func/c0071
log_ref_file: ./reflogs/c0071_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0071_vvvv
weight: 10713
loglevle: vvvv
casename: c0071
relatedlink: ../../cmd-func/c0071
log_ref_file: ./reflogs/c0071_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0071_vvvvv
weight: 10714
loglevle: vvvvv
casename: c0071
relatedlink: ../../cmd-func/c0071
log_ref_file: ./reflogs/c0071_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0071_vvvvvv
weight: 10715
loglevle: vvvvvv
casename: c0071
relatedlink: ../../cmd-func/c0071
log_ref_file: ./reflogs/c0071_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0071_vvvvvvv
weight: 10716
loglevle: vvvvvvv
casename: c0071
relatedlink: ../../cmd-func/c0071
log_ref_file: ./reflogs/c0071_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0072.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
741
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0072_v
weight: 10720
loglevle: v
casename: c0072
relatedlink: ../../templating/c0072
log_ref_file: ./reflogs/c0072_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0072_vv
weight: 10721
loglevle: vv
casename: c0072
relatedlink: ../../templating/c0072
log_ref_file: ./reflogs/c0072_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0072_vvv
weight: 10722
loglevle: vvv
casename: c0072
relatedlink: ../../templating/c0072
log_ref_file: ./reflogs/c0072_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0072_vvvv
weight: 10723
loglevle: vvvv
casename: c0072
relatedlink: ../../templating/c0072
log_ref_file: ./reflogs/c0072_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0072_vvvvv
weight: 10724
loglevle: vvvvv
casename: c0072
relatedlink: ../../templating/c0072
log_ref_file: ./reflogs/c0072_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0072_vvvvvv
weight: 10725
loglevle: vvvvvv
casename: c0072
relatedlink: ../../templating/c0072
log_ref_file: ./reflogs/c0072_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0072_vvvvvvv
weight: 10726
loglevle: vvvvvvv
casename: c0072
relatedlink: ../../templating/c0072
log_ref_file: ./reflogs/c0072_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0073.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
1686
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0073_v
weight: 10730
loglevle: v
casename: c0073
relatedlink: ../../syntax/c0073
log_ref_file: ./reflogs/c0073_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0073_vv
weight: 10731
loglevle: vv
casename: c0073
relatedlink: ../../syntax/c0073
log_ref_file: ./reflogs/c0073_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0073_vvv
weight: 10732
loglevle: vvv
casename: c0073
relatedlink: ../../syntax/c0073
log_ref_file: ./reflogs/c0073_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0073_vvvv
weight: 10733
loglevle: vvvv
casename: c0073
relatedlink: ../../syntax/c0073
log_ref_file: ./reflogs/c0073_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0073_vvvvv
weight: 10734
loglevle: vvvvv
casename: c0073
relatedlink: ../../syntax/c0073
log_ref_file: ./reflogs/c0073_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0073_vvvvvv
weight: 10735
loglevle: vvvvvv
casename: c0073
relatedlink: ../../syntax/c0073
log_ref_file: ./reflogs/c0073_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0073_vvvvvvv
weight: 10736
loglevle: vvvvvvv
casename: c0073
relatedlink: ../../syntax/c0073
log_ref_file: ./reflogs/c0073_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0074.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
642
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0074_v
weight: 10740
loglevle: v
casename: c0074
relatedlink: ../../query-object/c0074
log_ref_file: ./reflogs/c0074_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0074_vv
weight: 10741
loglevle: vv
casename: c0074
relatedlink: ../../query-object/c0074
log_ref_file: ./reflogs/c0074_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0074_vvv
weight: 10742
loglevle: vvv
casename: c0074
relatedlink: ../../query-object/c0074
log_ref_file: ./reflogs/c0074_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0074_vvvv
weight: 10743
loglevle: vvvv
casename: c0074
relatedlink: ../../query-object/c0074
log_ref_file: ./reflogs/c0074_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0074_vvvvv
weight: 10744
loglevle: vvvvv
casename: c0074
relatedlink: ../../query-object/c0074
log_ref_file: ./reflogs/c0074_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0074_vvvvvv
weight: 10745
loglevle: vvvvvv
casename: c0074
relatedlink: ../../query-object/c0074
log_ref_file: ./reflogs/c0074_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0074_vvvvvvv
weight: 10746
loglevle: vvvvvvv
casename: c0074
relatedlink: ../../query-object/c0074
log_ref_file: ./reflogs/c0074_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0075.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
608
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0075_v
weight: 10750
loglevle: v
casename: c0075
relatedlink: ../../template/c0075
log_ref_file: ./reflogs/c0075_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0075_vv
weight: 10751
loglevle: vv
casename: c0075
relatedlink: ../../template/c0075
log_ref_file: ./reflogs/c0075_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0075_vvv
weight: 10752
loglevle: vvv
casename: c0075
relatedlink: ../../template/c0075
log_ref_file: ./reflogs/c0075_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0075_vvvv
weight: 10753
loglevle: vvvv
casename: c0075
relatedlink: ../../template/c0075
log_ref_file: ./reflogs/c0075_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0075_vvvvv
weight: 10754
loglevle: vvvvv
casename: c0075
relatedlink: ../../template/c0075
log_ref_file: ./reflogs/c0075_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0075_vvvvvv
weight: 10755
loglevle: vvvvvv
casename: c0075
relatedlink: ../../template/c0075
log_ref_file: ./reflogs/c0075_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0075_vvvvvvv
weight: 10756
loglevle: vvvvvvv
casename: c0075
relatedlink: ../../template/c0075
log_ref_file: ./reflogs/c0075_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0076.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
728
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0076_v
weight: 10760
loglevle: v
casename: c0076
relatedlink: ../../vars/c0076
log_ref_file: ./reflogs/c0076_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0076_vv
weight: 10761
loglevle: vv
casename: c0076
relatedlink: ../../vars/c0076
log_ref_file: ./reflogs/c0076_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0076_vvv
weight: 10762
loglevle: vvv
casename: c0076
relatedlink: ../../vars/c0076
log_ref_file: ./reflogs/c0076_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0076_vvvv
weight: 10763
loglevle: vvvv
casename: c0076
relatedlink: ../../vars/c0076
log_ref_file: ./reflogs/c0076_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0076_vvvvv
weight: 10764
loglevle: vvvvv
casename: c0076
relatedlink: ../../vars/c0076
log_ref_file: ./reflogs/c0076_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0076_vvvvvv
weight: 10765
loglevle: vvvvvv
casename: c0076
relatedlink: ../../vars/c0076
log_ref_file: ./reflogs/c0076_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0076_vvvvvvv
weight: 10766
loglevle: vvvvvvv
casename: c0076
relatedlink: ../../vars/c0076
log_ref_file: ./reflogs/c0076_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0078.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
379
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0078_v
weight: 10780
loglevle: v
casename: c0078
relatedlink: ../../dvars/c0078
log_ref_file: ./reflogs/c0078_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0078_vv
weight: 10781
loglevle: vv
casename: c0078
relatedlink: ../../dvars/c0078
log_ref_file: ./reflogs/c0078_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0078_vvv
weight: 10782
loglevle: vvv
casename: c0078
relatedlink: ../../dvars/c0078
log_ref_file: ./reflogs/c0078_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0078_vvvv
weight: 10783
loglevle: vvvv
casename: c0078
relatedlink: ../../dvars/c0078
log_ref_file: ./reflogs/c0078_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0078_vvvvv
weight: 10784
loglevle: vvvvv
casename: c0078
relatedlink: ../../dvars/c0078
log_ref_file: ./reflogs/c0078_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0078_vvvvvv
weight: 10785
loglevle: vvvvvv
casename: c0078
relatedlink: ../../dvars/c0078
log_ref_file: ./reflogs/c0078_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0078_vvvvvvv
weight: 10786
loglevle: vvvvvvv
casename: c0078
relatedlink: ../../dvars/c0078
log_ref_file: ./reflogs/c0078_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0079.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
830
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0079_v
weight: 10790
loglevle: v
casename: c0079
relatedlink: ../../design-patterns/c0079
log_ref_file: ./reflogs/c0079_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0079_vv
weight: 10791
loglevle: vv
casename: c0079
relatedlink: ../../design-patterns/c0079
log_ref_file: ./reflogs/c0079_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0079_vvv
weight: 10792
loglevle: vvv
casename: c0079
relatedlink: ../../design-patterns/c0079
log_ref_file: ./reflogs/c0079_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0079_vvvv
weight: 10793
loglevle: vvvv
casename: c0079
relatedlink: ../../design-patterns/c0079
log_ref_file: ./reflogs/c0079_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0079_vvvvv
weight: 10794
loglevle: vvvvv
casename: c0079
relatedlink: ../../design-patterns/c0079
log_ref_file: ./reflogs/c0079_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0079_vvvvvv
weight: 10795
loglevle: vvvvvv
casename: c0079
relatedlink: ../../design-patterns/c0079
log_ref_file: ./reflogs/c0079_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0079_vvvvvvv
weight: 10796
loglevle: vvvvvvv
casename: c0079
relatedlink: ../../design-patterns/c0079
log_ref_file: ./reflogs/c0079_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0080.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
362
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0080_v
weight: 10800
loglevle: v
casename: c0080
relatedlink: ../../design-patterns/c0080
log_ref_file: ./reflogs/c0080_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0080_vv
weight: 10801
loglevle: vv
casename: c0080
relatedlink: ../../design-patterns/c0080
log_ref_file: ./reflogs/c0080_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0080_vvv
weight: 10802
loglevle: vvv
casename: c0080
relatedlink: ../../design-patterns/c0080
log_ref_file: ./reflogs/c0080_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0080_vvvv
weight: 10803
loglevle: vvvv
casename: c0080
relatedlink: ../../design-patterns/c0080
log_ref_file: ./reflogs/c0080_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0080_vvvvv
weight: 10804
loglevle: vvvvv
casename: c0080
relatedlink: ../../design-patterns/c0080
log_ref_file: ./reflogs/c0080_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0080_vvvvvv
weight: 10805
loglevle: vvvvvv
casename: c0080
relatedlink: ../../design-patterns/c0080
log_ref_file: ./reflogs/c0080_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0080_vvvvvvv
weight: 10806
loglevle: vvvvvvv
casename: c0080
relatedlink: ../../design-patterns/c0080
log_ref_file: ./reflogs/c0080_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0081.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
1046
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0081_v
weight: 10810
loglevle: v
casename: c0081
relatedlink: ../../design-patterns/c0081
log_ref_file: ./reflogs/c0081_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0081_vv
weight: 10811
loglevle: vv
casename: c0081
relatedlink: ../../design-patterns/c0081
log_ref_file: ./reflogs/c0081_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0081_vvv
weight: 10812
loglevle: vvv
casename: c0081
relatedlink: ../../design-patterns/c0081
log_ref_file: ./reflogs/c0081_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0081_vvvv
weight: 10813
loglevle: vvvv
casename: c0081
relatedlink: ../../design-patterns/c0081
log_ref_file: ./reflogs/c0081_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0081_vvvvv
weight: 10814
loglevle: vvvvv
casename: c0081
relatedlink: ../../design-patterns/c0081
log_ref_file: ./reflogs/c0081_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0081_vvvvvv
weight: 10815
loglevle: vvvvvv
casename: c0081
relatedlink: ../../design-patterns/c0081
log_ref_file: ./reflogs/c0081_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0081_vvvvvvv
weight: 10816
loglevle: vvvvvvv
casename: c0081
relatedlink: ../../design-patterns/c0081
log_ref_file: ./reflogs/c0081_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0082.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
1039
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0082_v
weight: 10820
loglevle: v
casename: c0082
relatedlink: ../../dvars/c0082
log_ref_file: ./reflogs/c0082_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0082_vv
weight: 10821
loglevle: vv
casename: c0082
relatedlink: ../../dvars/c0082
log_ref_file: ./reflogs/c0082_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0082_vvv
weight: 10822
loglevle: vvv
casename: c0082
relatedlink: ../../dvars/c0082
log_ref_file: ./reflogs/c0082_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0082_vvvv
weight: 10823
loglevle: vvvv
casename: c0082
relatedlink: ../../dvars/c0082
log_ref_file: ./reflogs/c0082_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0082_vvvvv
weight: 10824
loglevle: vvvvv
casename: c0082
relatedlink: ../../dvars/c0082
log_ref_file: ./reflogs/c0082_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0082_vvvvvv
weight: 10825
loglevle: vvvvvv
casename: c0082
relatedlink: ../../dvars/c0082
log_ref_file: ./reflogs/c0082_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0082_vvvvvvv
weight: 10826
loglevle: vvvvvvv
casename: c0082
relatedlink: ../../dvars/c0082
log_ref_file: ./reflogs/c0082_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0083.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0084.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0085.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
268
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0085_v
weight: 10850
loglevle: v
casename: c0085
relatedlink: ../../template/c0085
log_ref_file: ./reflogs/c0085_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0085_vv
weight: 10851
loglevle: vv
casename: c0085
relatedlink: ../../template/c0085
log_ref_file: ./reflogs/c0085_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0085_vvv
weight: 10852
loglevle: vvv
casename: c0085
relatedlink: ../../template/c0085
log_ref_file: ./reflogs/c0085_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0085_vvvv
weight: 10853
loglevle: vvvv
casename: c0085
relatedlink: ../../template/c0085
log_ref_file: ./reflogs/c0085_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0085_vvvvv
weight: 10854
loglevle: vvvvv
casename: c0085
relatedlink: ../../template/c0085
log_ref_file: ./reflogs/c0085_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0085_vvvvvv
weight: 10855
loglevle: vvvvvv
casename: c0085
relatedlink: ../../template/c0085
log_ref_file: ./reflogs/c0085_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0085_vvvvvvv
weight: 10856
loglevle: vvvvvvv
casename: c0085
relatedlink: ../../template/c0085
log_ref_file: ./reflogs/c0085_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0086.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
443
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0086_v
weight: 10860
loglevle: v
casename: c0086
relatedlink: ../../dvars/c0086
log_ref_file: ./reflogs/c0086_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0086_vv
weight: 10861
loglevle: vv
casename: c0086
relatedlink: ../../dvars/c0086
log_ref_file: ./reflogs/c0086_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0086_vvv
weight: 10862
loglevle: vvv
casename: c0086
relatedlink: ../../dvars/c0086
log_ref_file: ./reflogs/c0086_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0086_vvvv
weight: 10863
loglevle: vvvv
casename: c0086
relatedlink: ../../dvars/c0086
log_ref_file: ./reflogs/c0086_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0086_vvvvv
weight: 10864
loglevle: vvvvv
casename: c0086
relatedlink: ../../dvars/c0086
log_ref_file: ./reflogs/c0086_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0086_vvvvvv
weight: 10865
loglevle: vvvvvv
casename: c0086
relatedlink: ../../dvars/c0086
log_ref_file: ./reflogs/c0086_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0086_vvvvvvv
weight: 10866
loglevle: vvvvvvv
casename: c0086
relatedlink: ../../dvars/c0086
log_ref_file: ./reflogs/c0086_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0087.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
239
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0087_v
weight: 10870
loglevle: v
casename: c0087
relatedlink: ../../cmd-func/c0087
log_ref_file: ./reflogs/c0087_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0087_vv
weight: 10871
loglevle: vv
casename: c0087
relatedlink: ../../cmd-func/c0087
log_ref_file: ./reflogs/c0087_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0087_vvv
weight: 10872
loglevle: vvv
casename: c0087
relatedlink: ../../cmd-func/c0087
log_ref_file: ./reflogs/c0087_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0087_vvvv
weight: 10873
loglevle: vvvv
casename: c0087
relatedlink: ../../cmd-func/c0087
log_ref_file: ./reflogs/c0087_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0087_vvvvv
weight: 10874
loglevle: vvvvv
casename: c0087
relatedlink: ../../cmd-func/c0087
log_ref_file: ./reflogs/c0087_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0087_vvvvvv
weight: 10875
loglevle: vvvvvv
casename: c0087
relatedlink: ../../cmd-func/c0087
log_ref_file: ./reflogs/c0087_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0087_vvvvvvv
weight: 10876
loglevle: vvvvvvv
casename: c0087
relatedlink: ../../cmd-func/c0087
log_ref_file: ./reflogs/c0087_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0088.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
877
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0088_v
weight: 10880
loglevle: v
casename: c0088
relatedlink: ../../dvars/c0088
log_ref_file: ./reflogs/c0088_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0088_vv
weight: 10881
loglevle: vv
casename: c0088
relatedlink: ../../dvars/c0088
log_ref_file: ./reflogs/c0088_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0088_vvv
weight: 10882
loglevle: vvv
casename: c0088
relatedlink: ../../dvars/c0088
log_ref_file: ./reflogs/c0088_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0088_vvvv
weight: 10883
loglevle: vvvv
casename: c0088
relatedlink: ../../dvars/c0088
log_ref_file: ./reflogs/c0088_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0088_vvvvv
weight: 10884
loglevle: vvvvv
casename: c0088
relatedlink: ../../dvars/c0088
log_ref_file: ./reflogs/c0088_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0088_vvvvvv
weight: 10885
loglevle: vvvvvv
casename: c0088
relatedlink: ../../dvars/c0088
log_ref_file: ./reflogs/c0088_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0088_vvvvvvv
weight: 10886
loglevle: vvvvvvv
casename: c0088
relatedlink: ../../dvars/c0088
log_ref_file: ./reflogs/c0088_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0089.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
970
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0089_v
weight: 10890
loglevle: v
casename: c0089
relatedlink: ../../organization/c0089
log_ref_file: ./reflogs/c0089_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0089_vv
weight: 10891
loglevle: vv
casename: c0089
relatedlink: ../../organization/c0089
log_ref_file: ./reflogs/c0089_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0089_vvv
weight: 10892
loglevle: vvv
casename: c0089
relatedlink: ../../organization/c0089
log_ref_file: ./reflogs/c0089_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0089_vvvv
weight: 10893
loglevle: vvvv
casename: c0089
relatedlink: ../../organization/c0089
log_ref_file: ./reflogs/c0089_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0089_vvvvv
weight: 10894
loglevle: vvvvv
casename: c0089
relatedlink: ../../organization/c0089
log_ref_file: ./reflogs/c0089_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0089_vvvvvv
weight: 10895
loglevle: vvvvvv
casename: c0089
relatedlink: ../../organization/c0089
log_ref_file: ./reflogs/c0089_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0089_vvvvvvv
weight: 10896
loglevle: vvvvvvv
casename: c0089
relatedlink: ../../organization/c0089
log_ref_file: ./reflogs/c0089_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0090.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
395
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0090_v
weight: 10900
loglevle: v
casename: c0090
relatedlink: ../../loop/c0090
log_ref_file: ./reflogs/c0090_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0090_vv
weight: 10901
loglevle: vv
casename: c0090
relatedlink: ../../loop/c0090
log_ref_file: ./reflogs/c0090_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0090_vvv
weight: 10902
loglevle: vvv
casename: c0090
relatedlink: ../../loop/c0090
log_ref_file: ./reflogs/c0090_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0090_vvvv
weight: 10903
loglevle: vvvv
casename: c0090
relatedlink: ../../loop/c0090
log_ref_file: ./reflogs/c0090_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0090_vvvvv
weight: 10904
loglevle: vvvvv
casename: c0090
relatedlink: ../../loop/c0090
log_ref_file: ./reflogs/c0090_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0090_vvvvvv
weight: 10905
loglevle: vvvvvv
casename: c0090
relatedlink: ../../loop/c0090
log_ref_file: ./reflogs/c0090_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0090_vvvvvvv
weight: 10906
loglevle: vvvvvvv
casename: c0090
relatedlink: ../../loop/c0090
log_ref_file: ./reflogs/c0090_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0091.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
402
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0091_v
weight: 10910
loglevle: v
casename: c0091
relatedlink: ../../loop/c0091
log_ref_file: ./reflogs/c0091_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0091_vv
weight: 10911
loglevle: vv
casename: c0091
relatedlink: ../../loop/c0091
log_ref_file: ./reflogs/c0091_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0091_vvv
weight: 10912
loglevle: vvv
casename: c0091
relatedlink: ../../loop/c0091
log_ref_file: ./reflogs/c0091_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0091_vvvv
weight: 10913
loglevle: vvvv
casename: c0091
relatedlink: ../../loop/c0091
log_ref_file: ./reflogs/c0091_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0091_vvvvv
weight: 10914
loglevle: vvvvv
casename: c0091
relatedlink: ../../loop/c0091
log_ref_file: ./reflogs/c0091_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0091_vvvvvv
weight: 10915
loglevle: vvvvvv
casename: c0091
relatedlink: ../../loop/c0091
log_ref_file: ./reflogs/c0091_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0091_vvvvvvv
weight: 10916
loglevle: vvvvvvv
casename: c0091
relatedlink: ../../loop/c0091
log_ref_file: ./reflogs/c0091_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0092.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
399
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0092_v
weight: 10920
loglevle: v
casename: c0092
relatedlink: ../../flow-controll/c0092
log_ref_file: ./reflogs/c0092_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0092_vv
weight: 10921
loglevle: vv
casename: c0092
relatedlink: ../../flow-controll/c0092
log_ref_file: ./reflogs/c0092_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0092_vvv
weight: 10922
loglevle: vvv
casename: c0092
relatedlink: ../../flow-controll/c0092
log_ref_file: ./reflogs/c0092_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0092_vvvv
weight: 10923
loglevle: vvvv
casename: c0092
relatedlink: ../../flow-controll/c0092
log_ref_file: ./reflogs/c0092_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0092_vvvvv
weight: 10924
loglevle: vvvvv
casename: c0092
relatedlink: ../../flow-controll/c0092
log_ref_file: ./reflogs/c0092_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0092_vvvvvv
weight: 10925
loglevle: vvvvvv
casename: c0092
relatedlink: ../../flow-controll/c0092
log_ref_file: ./reflogs/c0092_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0092_vvvvvvv
weight: 10926
loglevle: vvvvvvv
casename: c0092
relatedlink: ../../flow-controll/c0092
log_ref_file: ./reflogs/c0092_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0093.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
348
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0093_v
weight: 10930
loglevle: v
casename: c0093
relatedlink: ../../flow-controll/c0093
log_ref_file: ./reflogs/c0093_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0093_vv
weight: 10931
loglevle: vv
casename: c0093
relatedlink: ../../flow-controll/c0093
log_ref_file: ./reflogs/c0093_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0093_vvv
weight: 10932
loglevle: vvv
casename: c0093
relatedlink: ../../flow-controll/c0093
log_ref_file: ./reflogs/c0093_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0093_vvvv
weight: 10933
loglevle: vvvv
casename: c0093
relatedlink: ../../flow-controll/c0093
log_ref_file: ./reflogs/c0093_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0093_vvvvv
weight: 10934
loglevle: vvvvv
casename: c0093
relatedlink: ../../flow-controll/c0093
log_ref_file: ./reflogs/c0093_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0093_vvvvvv
weight: 10935
loglevle: vvvvvv
casename: c0093
relatedlink: ../../flow-controll/c0093
log_ref_file: ./reflogs/c0093_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0093_vvvvvvv
weight: 10936
loglevle: vvvvvvv
casename: c0093
relatedlink: ../../flow-controll/c0093
log_ref_file: ./reflogs/c0093_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0094.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
733
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0094_v
weight: 10940
loglevle: v
casename: c0094
relatedlink: ../../design-patterns/c0094
log_ref_file: ./reflogs/c0094_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0094_vv
weight: 10941
loglevle: vv
casename: c0094
relatedlink: ../../design-patterns/c0094
log_ref_file: ./reflogs/c0094_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0094_vvv
weight: 10942
loglevle: vvv
casename: c0094
relatedlink: ../../design-patterns/c0094
log_ref_file: ./reflogs/c0094_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0094_vvvv
weight: 10943
loglevle: vvvv
casename: c0094
relatedlink: ../../design-patterns/c0094
log_ref_file: ./reflogs/c0094_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0094_vvvvv
weight: 10944
loglevle: vvvvv
casename: c0094
relatedlink: ../../design-patterns/c0094
log_ref_file: ./reflogs/c0094_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0094_vvvvvv
weight: 10945
loglevle: vvvvvv
casename: c0094
relatedlink: ../../design-patterns/c0094
log_ref_file: ./reflogs/c0094_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0094_vvvvvvv
weight: 10946
loglevle: vvvvvvv
casename: c0094
relatedlink: ../../design-patterns/c0094
log_ref_file: ./reflogs/c0094_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0095.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
736
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0095_v
weight: 10950
loglevle: v
casename: c0095
relatedlink: ../../cmd-func/c0095
log_ref_file: ./reflogs/c0095_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0095_vv
weight: 10951
loglevle: vv
casename: c0095
relatedlink: ../../cmd-func/c0095
log_ref_file: ./reflogs/c0095_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0095_vvv
weight: 10952
loglevle: vvv
casename: c0095
relatedlink: ../../cmd-func/c0095
log_ref_file: ./reflogs/c0095_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0095_vvvv
weight: 10953
loglevle: vvvv
casename: c0095
relatedlink: ../../cmd-func/c0095
log_ref_file: ./reflogs/c0095_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0095_vvvvv
weight: 10954
loglevle: vvvvv
casename: c0095
relatedlink: ../../cmd-func/c0095
log_ref_file: ./reflogs/c0095_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0095_vvvvvv
weight: 10955
loglevle: vvvvvv
casename: c0095
relatedlink: ../../cmd-func/c0095
log_ref_file: ./reflogs/c0095_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0095_vvvvvvv
weight: 10956
loglevle: vvvvvvv
casename: c0095
relatedlink: ../../cmd-func/c0095
log_ref_file: ./reflogs/c0095_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0096.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
1198
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0096_v
weight: 10960
loglevle: v
casename: c0096
relatedlink: ../../templating/c0096
log_ref_file: ./reflogs/c0096_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0096_vv
weight: 10961
loglevle: vv
casename: c0096
relatedlink: ../../templating/c0096
log_ref_file: ./reflogs/c0096_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0096_vvv
weight: 10962
loglevle: vvv
casename: c0096
relatedlink: ../../templating/c0096
log_ref_file: ./reflogs/c0096_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0096_vvvv
weight: 10963
loglevle: vvvv
casename: c0096
relatedlink: ../../templating/c0096
log_ref_file: ./reflogs/c0096_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0096_vvvvv
weight: 10964
loglevle: vvvvv
casename: c0096
relatedlink: ../../templating/c0096
log_ref_file: ./reflogs/c0096_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0096_vvvvvv
weight: 10965
loglevle: vvvvvv
casename: c0096
relatedlink: ../../templating/c0096
log_ref_file: ./reflogs/c0096_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0096_vvvvvvv
weight: 10966
loglevle: vvvvvvv
casename: c0096
relatedlink: ../../templating/c0096
log_ref_file: ./reflogs/c0096_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0098.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
1013
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0098_v
weight: 10980
loglevle: v
casename: c0098
relatedlink: ../../dvars/c0098
log_ref_file: ./reflogs/c0098_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0098_vv
weight: 10981
loglevle: vv
casename: c0098
relatedlink: ../../dvars/c0098
log_ref_file: ./reflogs/c0098_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0098_vvv
weight: 10982
loglevle: vvv
casename: c0098
relatedlink: ../../dvars/c0098
log_ref_file: ./reflogs/c0098_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0098_vvvv
weight: 10983
loglevle: vvvv
casename: c0098
relatedlink: ../../dvars/c0098
log_ref_file: ./reflogs/c0098_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0098_vvvvv
weight: 10984
loglevle: vvvvv
casename: c0098
relatedlink: ../../dvars/c0098
log_ref_file: ./reflogs/c0098_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0098_vvvvvv
weight: 10985
loglevle: vvvvvv
casename: c0098
relatedlink: ../../dvars/c0098
log_ref_file: ./reflogs/c0098_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0098_vvvvvvv
weight: 10986
loglevle: vvvvvvv
casename: c0098
relatedlink: ../../dvars/c0098
log_ref_file: ./reflogs/c0098_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0099.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
983
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0099_v
weight: 10990
loglevle: v
casename: c0099
relatedlink: ../../dvars/c0099
log_ref_file: ./reflogs/c0099_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0099_vv
weight: 10991
loglevle: vv
casename: c0099
relatedlink: ../../dvars/c0099
log_ref_file: ./reflogs/c0099_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0099_vvv
weight: 10992
loglevle: vvv
casename: c0099
relatedlink: ../../dvars/c0099
log_ref_file: ./reflogs/c0099_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0099_vvvv
weight: 10993
loglevle: vvvv
casename: c0099
relatedlink: ../../dvars/c0099
log_ref_file: ./reflogs/c0099_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0099_vvvvv
weight: 10994
loglevle: vvvvv
casename: c0099
relatedlink: ../../dvars/c0099
log_ref_file: ./reflogs/c0099_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0099_vvvvvv
weight: 10995
loglevle: vvvvvv
casename: c0099
relatedlink: ../../dvars/c0099
log_ref_file: ./reflogs/c0099_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0099_vvvvvvv
weight: 10996
loglevle: vvvvvvv
casename: c0099
relatedlink: ../../dvars/c0099
log_ref_file: ./reflogs/c0099_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0100.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
1470
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0100_v
weight: 11000
loglevle: v
casename: c0100
relatedlink: ../../query-object/c0100
log_ref_file: ./reflogs/c0100_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0100_vv
weight: 11001
loglevle: vv
casename: c0100
relatedlink: ../../query-object/c0100
log_ref_file: ./reflogs/c0100_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0100_vvv
weight: 11002
loglevle: vvv
casename: c0100
relatedlink: ../../query-object/c0100
log_ref_file: ./reflogs/c0100_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0100_vvvv
weight: 11003
loglevle: vvvv
casename: c0100
relatedlink: ../../query-object/c0100
log_ref_file: ./reflogs/c0100_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0100_vvvvv
weight: 11004
loglevle: vvvvv
casename: c0100
relatedlink: ../../query-object/c0100
log_ref_file: ./reflogs/c0100_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0100_vvvvvv
weight: 11005
loglevle: vvvvvv
casename: c0100
relatedlink: ../../query-object/c0100
log_ref_file: ./reflogs/c0100_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0100_vvvvvvv
weight: 11006
loglevle: vvvvvvv
casename: c0100
relatedlink: ../../query-object/c0100
log_ref_file: ./reflogs/c0100_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0101.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
659
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0101_v
weight: 11010
loglevle: v
casename: c0101
relatedlink: ../../test-debug/c0101
log_ref_file: ./reflogs/c0101_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0101_vv
weight: 11011
loglevle: vv
casename: c0101
relatedlink: ../../test-debug/c0101
log_ref_file: ./reflogs/c0101_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0101_vvv
weight: 11012
loglevle: vvv
casename: c0101
relatedlink: ../../test-debug/c0101
log_ref_file: ./reflogs/c0101_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0101_vvvv
weight: 11013
loglevle: vvvv
casename: c0101
relatedlink: ../../test-debug/c0101
log_ref_file: ./reflogs/c0101_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0101_vvvvv
weight: 11014
loglevle: vvvvv
casename: c0101
relatedlink: ../../test-debug/c0101
log_ref_file: ./reflogs/c0101_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0101_vvvvvv
weight: 11015
loglevle: vvvvvv
casename: c0101
relatedlink: ../../test-debug/c0101
log_ref_file: ./reflogs/c0101_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0101_vvvvvvv
weight: 11016
loglevle: vvvvvvv
casename: c0101
relatedlink: ../../test-debug/c0101
log_ref_file: ./reflogs/c0101_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0102.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
927
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0102_v
weight: 11020
loglevle: v
casename: c0102
relatedlink: ../../cmd-func/c0102
log_ref_file: ./reflogs/c0102_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0102_vv
weight: 11021
loglevle: vv
casename: c0102
relatedlink: ../../cmd-func/c0102
log_ref_file: ./reflogs/c0102_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0102_vvv
weight: 11022
loglevle: vvv
casename: c0102
relatedlink: ../../cmd-func/c0102
log_ref_file: ./reflogs/c0102_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0102_vvvv
weight: 11023
loglevle: vvvv
casename: c0102
relatedlink: ../../cmd-func/c0102
log_ref_file: ./reflogs/c0102_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0102_vvvvv
weight: 11024
loglevle: vvvvv
casename: c0102
relatedlink: ../../cmd-func/c0102
log_ref_file: ./reflogs/c0102_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0102_vvvvvv
weight: 11025
loglevle: vvvvvv
casename: c0102
relatedlink: ../../cmd-func/c0102
log_ref_file: ./reflogs/c0102_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0102_vvvvvvv
weight: 11026
loglevle: vvvvvvv
casename: c0102
relatedlink: ../../cmd-func/c0102
log_ref_file: ./reflogs/c0102_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0103.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
906
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0103_v
weight: 11030
loglevle: v
casename: c0103
relatedlink: ../../cmd-func/c0103
log_ref_file: ./reflogs/c0103_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0103_vv
weight: 11031
loglevle: vv
casename: c0103
relatedlink: ../../cmd-func/c0103
log_ref_file: ./reflogs/c0103_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0103_vvv
weight: 11032
loglevle: vvv
casename: c0103
relatedlink: ../../cmd-func/c0103
log_ref_file: ./reflogs/c0103_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0103_vvvv
weight: 11033
loglevle: vvvv
casename: c0103
relatedlink: ../../cmd-func/c0103
log_ref_file: ./reflogs/c0103_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0103_vvvvv
weight: 11034
loglevle: vvvvv
casename: c0103
relatedlink: ../../cmd-func/c0103
log_ref_file: ./reflogs/c0103_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0103_vvvvvv
weight: 11035
loglevle: vvvvvv
casename: c0103
relatedlink: ../../cmd-func/c0103
log_ref_file: ./reflogs/c0103_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0103_vvvvvvv
weight: 11036
loglevle: vvvvvvv
casename: c0103
relatedlink: ../../cmd-func/c0103
log_ref_file: ./reflogs/c0103_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0104.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 152

weight string length: 4

----Step2:
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
561
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0104_v
weight: 11040
loglevle: v
casename: c0104
relatedlink: ../../quick-start/c0104
log_ref_file: ./reflogs/c0104_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0104_vv
weight: 11041
loglevle: vv
casename: c0104
relatedlink: ../../quick-start/c0104
log_ref_file: ./reflogs/c0104_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0104_vvv
weight: 11042
loglevle: vvv
casename: c0104
relatedlink: ../../quick-start/c0104
log_ref_file: ./reflogs/c0104_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0104_vvvv
weight: 11043
loglevle: vvvv
casename: c0104
relatedlink: ../../quick-start/c0104
log_ref_file: ./reflogs/c0104_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0104_vvvvv
weight: 11044
loglevle: vvvvv
casename: c0104
relatedlink: ../../quick-start/c0104
log_ref_file: ./reflogs/c0104_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0104_vvvvvv
weight: 11045
loglevle: vvvvvv
casename: c0104
relatedlink: ../../quick-start/c0104
log_ref_file: ./reflogs/c0104_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0104_vvvvvvv
weight: 11046
loglevle: vvvvvvv
casename: c0104
relatedlink: ../../quick-start/c0104
log_ref_file: ./reflogs/c0104_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0105.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
299
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0105_v
weight: 11050
loglevle: v
casename: c0105
relatedlink: ../../vars/c0105
log_ref_file: ./reflogs/c0105_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0105_vv
weight: 11051
loglevle: vv
casename: c0105
relatedlink: ../../vars/c0105
log_ref_file: ./reflogs/c0105_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0105_vvv
weight: 11052
loglevle: vvv
casename: c0105
relatedlink: ../../vars/c0105
log_ref_file: ./reflogs/c0105_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0105_vvvv
weight: 11053
loglevle: vvvv
casename: c0105
relatedlink: ../../vars/c0105
log_ref_file: ./reflogs/c0105_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0105_vvvvv
weight: 11054
loglevle: vvvvv
casename: c0105
relatedlink: ../../vars/c0105
log_ref_file: ./reflogs/c0105_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0105_vvvvvv
weight: 11055
loglevle: vvvvvv
casename: c0105
relatedlink: ../../vars/c0105
log_ref_file: ./reflogs/c0105_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0105_vvvvvvv
weight: 11056
loglevle: vvvvvvv
casename: c0105
relatedlink: ../../vars/c0105
log_ref_file: ./reflogs/c0105_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0106.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
831
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0106_v
weight: 11060
loglevle: v
casename: c0106
relatedlink: ../../vars/c0106
log_ref_file: ./reflogs/c0106_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0106_vv
weight: 11061
loglevle: vv
casename: c0106
relatedlink: ../../vars/c0106
log_ref_file: ./reflogs/c0106_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0106_vvv
weight: 11062
loglevle: vvv
casename: c0106
relatedlink: ../../vars/c0106
log_ref_file: ./reflogs/c0106_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0106_vvvv
weight: 11063
loglevle: vvvv
casename: c0106
relatedlink: ../../vars/c0106
log_ref_file: ./reflogs/c0106_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0106_vvvvv
weight: 11064
loglevle: vvvvv
casename: c0106
relatedlink: ../../vars/c0106
log_ref_file: ./reflogs/c0106_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0106_vvvvvv
weight: 11065
loglevle: vvvvvv
casename: c0106
relatedlink: ../../vars/c0106
log_ref_file: ./reflogs/c0106_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0106_vvvvvvv
weight: 11066
loglevle: vvvvvvv
casename: c0106
relatedlink: ../../vars/c0106
log_ref_file: ./reflogs/c0106_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0107.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
485
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0107_v
weight: 11070
loglevle: v
casename: c0107
relatedlink: ../../test-debug/c0107
log_ref_file: ./reflogs/c0107_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0107_vv
weight: 11071
loglevle: vv
casename: c0107
relatedlink: ../../test-debug/c0107
log_ref_file: ./reflogs/c0107_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0107_vvv
weight: 11072
loglevle: vvv
casename: c0107
relatedlink: ../../test-debug/c0107
log_ref_file: ./reflogs/c0107_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0107_vvvv
weight: 11073
loglevle: vvvv
casename: c0107
relatedlink: ../../test-debug/c0107
log_ref_file: ./reflogs/c0107_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0107_vvvvv
weight: 11074
loglevle: vvvvv
casename: c0107
relatedlink: ../../test-debug/c0107
log_ref_file: ./reflogs/c0107_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0107_vvvvvv
weight: 11075
loglevle: vvvvvv
casename: c0107
relatedlink: ../../test-debug/c0107
log_ref_file: ./reflogs/c0107_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0107_vvvvvvv
weight: 11076
loglevle: vvvvvvv
casename: c0107
relatedlink: ../../test-debug/c0107
log_ref_file: ./reflogs/c0107_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0108.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
655
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0108_v
weight: 11080
loglevle: v
casename: c0108
relatedlink: ../../vars/c0108
log_ref_file: ./reflogs/c0108_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0108_vv
weight: 11081
loglevle: vv
casename: c0108
relatedlink: ../../vars/c0108
log_ref_file: ./reflogs/c0108_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0108_vvv
weight: 11082
loglevle: vvv
casename: c0108
relatedlink: ../../vars/c0108
log_ref_file: ./reflogs/c0108_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0108_vvvv
weight: 11083
loglevle: vvvv
casename: c0108
relatedlink: ../../vars/c0108
log_ref_file: ./reflogs/c0108_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0108_vvvvv
weight: 11084
loglevle: vvvvv
casename: c0108
relatedlink: ../../vars/c0108
log_ref_file: ./reflogs/c0108_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0108_vvvvvv
weight: 11085
loglevle: vvvvvv
casename: c0108
relatedlink: ../../vars/c0108
log_ref_file: ./reflogs/c0108_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0108_vvvvvvv
weight: 11086
loglevle: vvvvvvv
casename: c0108
relatedlink: ../../vars/c0108
log_ref_file: ./reflogs/c0108_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0109.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
395
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0109_v
weight: 11090
loglevle: v
casename: c0109
relatedlink: ../../vars/c0109
log_ref_file: ./reflogs/c0109_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0109_vv
weight: 11091
loglevle: vv
casename: c0109
relatedlink: ../../vars/c0109
log_ref_file: ./reflogs/c0109_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0109_vvv
weight: 11092
loglevle: vvv
casename: c0109
relatedlink: ../../vars/c0109
log_ref_file: ./reflogs/c0109_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0109_vvvv
weight: 11093
loglevle: vvvv
casename: c0109
relatedlink: ../../vars/c0109
log_ref_file: ./reflogs/c0109_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0109_vvvvv
weight: 11094
loglevle: vvvvv
casename: c0109
relatedlink: ../../vars/c0109
log_ref_file: ./reflogs/c0109_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0109_vvvvvv
weight: 11095
loglevle: vvvvvv
casename: c0109
relatedlink: ../../vars/c0109
log_ref_file: ./reflogs/c0109_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0109_vvvvvvv
weight: 11096
loglevle: vvvvvvv
casename: c0109
relatedlink: ../../vars/c0109
log_ref_file: ./reflogs/c0109_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0110.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
335
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0110_v
weight: 11100
loglevle: v
casename: c0110
relatedlink: ../../flow-controll/c0110
log_ref_file: ./reflogs/c0110_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0110_vv
weight: 11101
loglevle: vv
casename: c0110
relatedlink: ../../flow-controll/c0110
log_ref_file: ./reflogs/c0110_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0110_vvv
weight: 11102
loglevle: vvv
casename: c0110
relatedlink: ../../flow-controll/c0110
log_ref_file: ./reflogs/c0110_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0110_vvvv
weight: 11103
loglevle: vvvv
casename: c0110
relatedlink: ../../flow-controll/c0110
log_ref_file: ./reflogs/c0110_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0110_vvvvv
weight: 11104
loglevle: vvvvv
casename: c0110
relatedlink: ../../flow-controll/c0110
log_ref_file: ./reflogs/c0110_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0110_vvvvvv
weight: 11105
loglevle: vvvvvv
casename: c0110
relatedlink: ../../flow-controll/c0110
log_ref_file: ./reflogs/c0110_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0110_vvvvvvv
weight: 11106
loglevle: vvvvvvv
casename: c0110
relatedlink: ../../flow-controll/c0110
log_ref_file: ./reflogs/c0110_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0111.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
367
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0111_v
weight: 11110
loglevle: v
casename: c0111
relatedlink: ../../call-func/c0111
log_ref_file: ./reflogs/c0111_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0111_vv
weight: 11111
loglevle: vv
casename: c0111
relatedlink: ../../call-func/c0111
log_ref_file: ./reflogs/c0111_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0111_vvv
weight: 11112
loglevle: vvv
casename: c0111
relatedlink: ../../call-func/c0111
log_ref_file: ./reflogs/c0111_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0111_vvvv
weight: 11113
loglevle: vvvv
casename: c0111
relatedlink: ../../call-func/c0111
log_ref_file: ./reflogs/c0111_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0111_vvvvv
weight: 11114
loglevle: vvvvv
casename: c0111
relatedlink: ../../call-func/c0111
log_ref_file: ./reflogs/c0111_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0111_vvvvvv
weight: 11115
loglevle: vvvvvv
casename: c0111
relatedlink: ../../call-func/c0111
log_ref_file: ./reflogs/c0111_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0111_vvvvvvv
weight: 11116
loglevle: vvvvvvv
casename: c0111
relatedlink: ../../call-func/c0111
log_ref_file: ./reflogs/c0111_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0112.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
374
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0112_v
weight: 11120
loglevle: v
casename: c0112
relatedlink: ../../call-func/c0112
log_ref_file: ./reflogs/c0112_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0112_vv
weight: 11121
loglevle: vv
casename: c0112
relatedlink: ../../call-func/c0112
log_ref_file: ./reflogs/c0112_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0112_vvv
weight: 11122
loglevle: vvv
casename: c0112
relatedlink: ../../call-func/c0112
log_ref_file: ./reflogs/c0112_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0112_vvvv
weight: 11123
loglevle: vvvv
casename: c0112
relatedlink: ../../call-func/c0112
log_ref_file: ./reflogs/c0112_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0112_vvvvv
weight: 11124
loglevle: vvvvv
casename: c0112
relatedlink: ../../call-func/c0112
log_ref_file: ./reflogs/c0112_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0112_vvvvvv
weight: 11125
loglevle: vvvvvv
casename: c0112
relatedlink: ../../call-func/c0112
log_ref_file: ./reflogs/c0112_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0112_vvvvvvv
weight: 11126
loglevle: vvvvvvv
casename: c0112
relatedlink: ../../call-func/c0112
log_ref_file: ./reflogs/c0112_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0113.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
730
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0113_v
weight: 11130
loglevle: v
casename: c0113
relatedlink: ../../call-func/c0113
log_ref_file: ./reflogs/c0113_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0113_vv
weight: 11131
loglevle: vv
casename: c0113
relatedlink: ../../call-func/c0113
log_ref_file: ./reflogs/c0113_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0113_vvv
weight: 11132
loglevle: vvv
casename: c0113
relatedlink: ../../call-func/c0113
log_ref_file: ./reflogs/c0113_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0113_vvvv
weight: 11133
loglevle: vvvv
casename: c0113
relatedlink: ../../call-func/c0113
log_ref_file: ./reflogs/c0113_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0113_vvvvv
weight: 11134
loglevle: vvvvv
casename: c0113
relatedlink: ../../call-func/c0113
log_ref_file: ./reflogs/c0113_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0113_vvvvvv
weight: 11135
loglevle: vvvvvv
casename: c0113
relatedlink: ../../call-func/c0113
log_ref_file: ./reflogs/c0113_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0113_vvvvvvv
weight: 11136
loglevle: vvvvvvv
casename: c0113
relatedlink: ../../call-func/c0113
log_ref_file: ./reflogs/c0113_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0114.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
517
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0114_v
weight: 11140
loglevle: v
casename: c0114
relatedlink: ../../call-func/c0114
log_ref_file: ./reflogs/c0114_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0114_vv
weight: 11141
loglevle: vv
casename: c0114
relatedlink: ../../call-func/c0114
log_ref_file: ./reflogs/c0114_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0114_vvv
weight: 11142
loglevle: vvv
casename: c0114
relatedlink: ../../call-func/c0114
log_ref_file: ./reflogs/c0114_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0114_vvvv
weight: 11143
loglevle: vvvv
casename: c0114
relatedlink: ../../call-func/c0114
log_ref_file: ./reflogs/c0114_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0114_vvvvv
weight: 11144
loglevle: vvvvv
casename: c0114
relatedlink: ../../call-func/c0114
log_ref_file: ./reflogs/c0114_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0114_vvvvvv
weight: 11145
loglevle: vvvvvv
casename: c0114
relatedlink: ../../call-func/c0114
log_ref_file: ./reflogs/c0114_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0114_vvvvvvv
weight: 11146
loglevle: vvvvvvv
casename: c0114
relatedlink: ../../call-func/c0114
log_ref_file: ./reflogs/c0114_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0115.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
438
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0115_v
weight: 11150
loglevle: v
casename: c0115
relatedlink: ../../call-func/c0115
log_ref_file: ./reflogs/c0115_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0115_vv
weight: 11151
loglevle: vv
casename: c0115
relatedlink: ../../call-func/c0115
log_ref_file: ./reflogs/c0115_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0115_vvv
weight: 11152
loglevle: vvv
casename: c0115
relatedlink: ../../call-func/c0115
log_ref_file: ./reflogs/c0115_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0115_vvvv
weight: 11153
loglevle: vvvv
casename: c0115
relatedlink: ../../call-func/c0115
log_ref_file: ./reflogs/c0115_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0115_vvvvv
weight: 11154
loglevle: vvvvv
casename: c0115
relatedlink: ../../call-func/c0115
log_ref_file: ./reflogs/c0115_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0115_vvvvvv
weight: 11155
loglevle: vvvvvv
casename: c0115
relatedlink: ../../call-func/c0115
log_ref_file: ./reflogs/c0115_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0115_vvvvvvv
weight: 11156
loglevle: vvvvvvv
casename: c0115
relatedlink: ../../call-func/c0115
log_ref_file: ./reflogs/c0115_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0116.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0117.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
308
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0117_v
weight: 11170
loglevle: v
casename: c0117
relatedlink: ../../loop/c0117
log_ref_file: ./reflogs/c0117_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0117_vv
weight: 11171
loglevle: vv
casename: c0117
relatedlink: ../../loop/c0117
log_ref_file: ./reflogs/c0117_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0117_vvv
weight: 11172
loglevle: vvv
casename: c0117
relatedlink: ../../loop/c0117
log_ref_file: ./reflogs/c0117_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0117_vvvv
weight: 11173
loglevle: vvvv
casename: c0117
relatedlink: ../../loop/c0117
log_ref_file: ./reflogs/c0117_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0117_vvvvv
weight: 11174
loglevle: vvvvv
casename: c0117
relatedlink: ../../loop/c0117
log_ref_file: ./reflogs/c0117_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0117_vvvvvv
weight: 11175
loglevle: vvvvvv
casename: c0117
relatedlink: ../../loop/c0117
log_ref_file: ./reflogs/c0117_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0117_vvvvvvv
weight: 11176
loglevle: vvvvvvv
casename: c0117
relatedlink: ../../loop/c0117
log_ref_file: ./reflogs/c0117_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0118.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
267
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0118_v
weight: 11180
loglevle: v
casename: c0118
relatedlink: ../../loop/c0118
log_ref_file: ./reflogs/c0118_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0118_vv
weight: 11181
loglevle: vv
casename: c0118
relatedlink: ../../loop/c0118
log_ref_file: ./reflogs/c0118_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0118_vvv
weight: 11182
loglevle: vvv
casename: c0118
relatedlink: ../../loop/c0118
log_ref_file: ./reflogs/c0118_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0118_vvvv
weight: 11183
loglevle: vvvv
casename: c0118
relatedlink: ../../loop/c0118
log_ref_file: ./reflogs/c0118_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0118_vvvvv
weight: 11184
loglevle: vvvvv
casename: c0118
relatedlink: ../../loop/c0118
log_ref_file: ./reflogs/c0118_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0118_vvvvvv
weight: 11185
loglevle: vvvvvv
casename: c0118
relatedlink: ../../loop/c0118
log_ref_file: ./reflogs/c0118_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0118_vvvvvvv
weight: 11186
loglevle: vvvvvvv
casename: c0118
relatedlink: ../../loop/c0118
log_ref_file: ./reflogs/c0118_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0119.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
281
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0119_v
weight: 11190
loglevle: v
casename: c0119
relatedlink: ../../loop/c0119
log_ref_file: ./reflogs/c0119_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0119_vv
weight: 11191
loglevle: vv
casename: c0119
relatedlink: ../../loop/c0119
log_ref_file: ./reflogs/c0119_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0119_vvv
weight: 11192
loglevle: vvv
casename: c0119
relatedlink: ../../loop/c0119
log_ref_file: ./reflogs/c0119_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0119_vvvv
weight: 11193
loglevle: vvvv
casename: c0119
relatedlink: ../../loop/c0119
log_ref_file: ./reflogs/c0119_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0119_vvvvv
weight: 11194
loglevle: vvvvv
casename: c0119
relatedlink: ../../loop/c0119
log_ref_file: ./reflogs/c0119_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0119_vvvvvv
weight: 11195
loglevle: vvvvvv
casename: c0119
relatedlink: ../../loop/c0119
log_ref_file: ./reflogs/c0119_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0119_vvvvvvv
weight: 11196
loglevle: vvvvvvv
casename: c0119
relatedlink: ../../loop/c0119
log_ref_file: ./reflogs/c0119_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0120.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
416
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0120_v
weight: 11200
loglevle: v
casename: c0120
relatedlink: ../../cmd-func/c0120
log_ref_file: ./reflogs/c0120_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0120_vv
weight: 11201
loglevle: vv
casename: c0120
relatedlink: ../../cmd-func/c0120
log_ref_file: ./reflogs/c0120_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0120_vvv
weight: 11202
loglevle: vvv
casename: c0120
relatedlink: ../../cmd-func/c0120
log_ref_file: ./reflogs/c0120_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0120_vvvv
weight: 11203
loglevle: vvvv
casename: c0120
relatedlink: ../../cmd-func/c0120
log_ref_file: ./reflogs/c0120_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0120_vvvvv
weight: 11204
loglevle: vvvvv
casename: c0120
relatedlink: ../../cmd-func/c0120
log_ref_file: ./reflogs/c0120_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0120_vvvvvv
weight: 11205
loglevle: vvvvvv
casename: c0120
relatedlink: ../../cmd-func/c0120
log_ref_file: ./reflogs/c0120_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0120_vvvvvvv
weight: 11206
loglevle: vvvvvvv
casename: c0120
relatedlink: ../../cmd-func/c0120
log_ref_file: ./reflogs/c0120_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0121.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
435
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0121_v
weight: 11210
loglevle: v
casename: c0121
relatedlink: ../../flow-controll/c0121
log_ref_file: ./reflogs/c0121_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0121_vv
weight: 11211
loglevle: vv
casename: c0121
relatedlink: ../../flow-controll/c0121
log_ref_file: ./reflogs/c0121_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0121_vvv
weight: 11212
loglevle: vvv
casename: c0121
relatedlink: ../../flow-controll/c0121
log_ref_file: ./reflogs/c0121_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0121_vvvv
weight: 11213
loglevle: vvvv
casename: c0121
relatedlink: ../../flow-controll/c0121
log_ref_file: ./reflogs/c0121_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0121_vvvvv
weight: 11214
loglevle: vvvvv
casename: c0121
relatedlink: ../../flow-controll/c0121
log_ref_file: ./reflogs/c0121_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0121_vvvvvv
weight: 11215
loglevle: vvvvvv
casename: c0121
relatedlink: ../../flow-controll/c0121
log_ref_file: ./reflogs/c0121_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0121_vvvvvvv
weight: 11216
loglevle: vvvvvvv
casename: c0121
relatedlink: ../../flow-controll/c0121
log_ref_file: ./reflogs/c0121_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0122.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
606
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0122_v
weight: 11220
loglevle: v
casename: c0122
relatedlink: ../../flow-controll/c0122
log_ref_file: ./reflogs/c0122_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0122_vv
weight: 11221
loglevle: vv
casename: c0122
relatedlink: ../../flow-controll/c0122
log_ref_file: ./reflogs/c0122_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0122_vvv
weight: 11222
loglevle: vvv
casename: c0122
relatedlink: ../../flow-controll/c0122
log_ref_file: ./reflogs/c0122_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0122_vvvv
weight: 11223
loglevle: vvvv
casename: c0122
relatedlink: ../../flow-controll/c0122
log_ref_file: ./reflogs/c0122_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0122_vvvvv
weight: 11224
loglevle: vvvvv
casename: c0122
relatedlink: ../../flow-controll/c0122
log_ref_file: ./reflogs/c0122_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0122_vvvvvv
weight: 11225
loglevle: vvvvvv
casename: c0122
relatedlink: ../../flow-controll/c0122
log_ref_file: ./reflogs/c0122_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0122_vvvvvvv
weight: 11226
loglevle: vvvvvvv
casename: c0122
relatedlink: ../../flow-controll/c0122
log_ref_file: ./reflogs/c0122_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0123.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
635
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0123_v
weight: 11230
loglevle: v
casename: c0123
relatedlink: ../../flow-controll/c0123
log_ref_file: ./reflogs/c0123_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0123_vv
weight: 11231
loglevle: vv
casename: c0123
relatedlink: ../../flow-controll/c0123
log_ref_file: ./reflogs/c0123_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0123_vvv
weight: 11232
loglevle: vvv
casename: c0123
relatedlink: ../../flow-controll/c0123
log_ref_file: ./reflogs/c0123_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0123_vvvv
weight: 11233
loglevle: vvvv
casename: c0123
relatedlink: ../../flow-controll/c0123
log_ref_file: ./reflogs/c0123_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0123_vvvvv
weight: 11234
loglevle: vvvvv
casename: c0123
relatedlink: ../../flow-controll/c0123
log_ref_file: ./reflogs/c0123_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0123_vvvvvv
weight: 11235
loglevle: vvvvvv
casename: c0123
relatedlink: ../../flow-controll/c0123
log_ref_file: ./reflogs/c0123_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0123_vvvvvvv
weight: 11236
loglevle: vvvvvvv
casename: c0123
relatedlink: ../../flow-controll/c0123
log_ref_file: ./reflogs/c0123_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0124.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0125.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
704
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0125_v
weight: 11250
loglevle: v
casename: c0125
relatedlink: ../../loop/c0125
log_ref_file: ./reflogs/c0125_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0125_vv
weight: 11251
loglevle: vv
casename: c0125
relatedlink: ../../loop/c0125
log_ref_file: ./reflogs/c0125_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0125_vvv
weight: 11252
loglevle: vvv
casename: c0125
relatedlink: ../../loop/c0125
log_ref_file: ./reflogs/c0125_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0125_vvvv
weight: 11253
loglevle: vvvv
casename: c0125
relatedlink: ../../loop/c0125
log_ref_file: ./reflogs/c0125_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0125_vvvvv
weight: 11254
loglevle: vvvvv
casename: c0125
relatedlink: ../../loop/c0125
log_ref_file: ./reflogs/c0125_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0125_vvvvvv
weight: 11255
loglevle: vvvvvv
casename: c0125
relatedlink: ../../loop/c0125
log_ref_file: ./reflogs/c0125_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0125_vvvvvvv
weight: 11256
loglevle: vvvvvvv
casename: c0125
relatedlink: ../../loop/c0125
log_ref_file: ./reflogs/c0125_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0126.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
318
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0126_v
weight: 11260
loglevle: v
casename: c0126
relatedlink: ../../flow-controll/c0126
log_ref_file: ./reflogs/c0126_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0126_vv
weight: 11261
loglevle: vv
casename: c0126
relatedlink: ../../flow-controll/c0126
log_ref_file: ./reflogs/c0126_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0126_vvv
weight: 11262
loglevle: vvv
casename: c0126
relatedlink: ../../flow-controll/c0126
log_ref_file: ./reflogs/c0126_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0126_vvvv
weight: 11263
loglevle: vvvv
casename: c0126
relatedlink: ../../flow-controll/c0126
log_ref_file: ./reflogs/c0126_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0126_vvvvv
weight: 11264
loglevle: vvvvv
casename: c0126
relatedlink: ../../flow-controll/c0126
log_ref_file: ./reflogs/c0126_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0126_vvvvvv
weight: 11265
loglevle: vvvvvv
casename: c0126
relatedlink: ../../flow-controll/c0126
log_ref_file: ./reflogs/c0126_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0126_vvvvvvv
weight: 11266
loglevle: vvvvvvv
casename: c0126
relatedlink: ../../flow-controll/c0126
log_ref_file: ./reflogs/c0126_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0127.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
312
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0127_v
weight: 11270
loglevle: v
casename: c0127
relatedlink: ../../flow-controll/c0127
log_ref_file: ./reflogs/c0127_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0127_vv
weight: 11271
loglevle: vv
casename: c0127
relatedlink: ../../flow-controll/c0127
log_ref_file: ./reflogs/c0127_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0127_vvv
weight: 11272
loglevle: vvv
casename: c0127
relatedlink: ../../flow-controll/c0127
log_ref_file: ./reflogs/c0127_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0127_vvvv
weight: 11273
loglevle: vvvv
casename: c0127
relatedlink: ../../flow-controll/c0127
log_ref_file: ./reflogs/c0127_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0127_vvvvv
weight: 11274
loglevle: vvvvv
casename: c0127
relatedlink: ../../flow-controll/c0127
log_ref_file: ./reflogs/c0127_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0127_vvvvvv
weight: 11275
loglevle: vvvvvv
casename: c0127
relatedlink: ../../flow-controll/c0127
log_ref_file: ./reflogs/c0127_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0127_vvvvvvv
weight: 11276
loglevle: vvvvvvv
casename: c0127
relatedlink: ../../flow-controll/c0127
log_ref_file: ./reflogs/c0127_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0128.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
824
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0128_v
weight: 11280
loglevle: v
casename: c0128
relatedlink: ../../block-func/c0128
log_ref_file: ./reflogs/c0128_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0128_vv
weight: 11281
loglevle: vv
casename: c0128
relatedlink: ../../block-func/c0128
log_ref_file: ./reflogs/c0128_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0128_vvv
weight: 11282
loglevle: vvv
casename: c0128
relatedlink: ../../block-func/c0128
log_ref_file: ./reflogs/c0128_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0128_vvvv
weight: 11283
loglevle: vvvv
casename: c0128
relatedlink: ../../block-func/c0128
log_ref_file: ./reflogs/c0128_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0128_vvvvv
weight: 11284
loglevle: vvvvv
casename: c0128
relatedlink: ../../block-func/c0128
log_ref_file: ./reflogs/c0128_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0128_vvvvvv
weight: 11285
loglevle: vvvvvv
casename: c0128
relatedlink: ../../block-func/c0128
log_ref_file: ./reflogs/c0128_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0128_vvvvvvv
weight: 11286
loglevle: vvvvvvv
casename: c0128
relatedlink: ../../block-func/c0128
log_ref_file: ./reflogs/c0128_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0129.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
520
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0129_v
weight: 11290
loglevle: v
casename: c0129
relatedlink: ../../block-func/c0129
log_ref_file: ./reflogs/c0129_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0129_vv
weight: 11291
loglevle: vv
casename: c0129
relatedlink: ../../block-func/c0129
log_ref_file: ./reflogs/c0129_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0129_vvv
weight: 11292
loglevle: vvv
casename: c0129
relatedlink: ../../block-func/c0129
log_ref_file: ./reflogs/c0129_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0129_vvvv
weight: 11293
loglevle: vvvv
casename: c0129
relatedlink: ../../block-func/c0129
log_ref_file: ./reflogs/c0129_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0129_vvvvv
weight: 11294
loglevle: vvvvv
casename: c0129
relatedlink: ../../block-func/c0129
log_ref_file: ./reflogs/c0129_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0129_vvvvvv
weight: 11295
loglevle: vvvvvv
casename: c0129
relatedlink: ../../block-func/c0129
log_ref_file: ./reflogs/c0129_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0129_vvvvvvv
weight: 11296
loglevle: vvvvvvv
casename: c0129
relatedlink: ../../block-func/c0129
log_ref_file: ./reflogs/c0129_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0130.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
427
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0130_v
weight: 11300
loglevle: v
casename: c0130
relatedlink: ../../block-func/c0130
log_ref_file: ./reflogs/c0130_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0130_vv
weight: 11301
loglevle: vv
casename: c0130
relatedlink: ../../block-func/c0130
log_ref_file: ./reflogs/c0130_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0130_vvv
weight: 11302
loglevle: vvv
casename: c0130
relatedlink: ../../block-func/c0130
log_ref_file: ./reflogs/c0130_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0130_vvvv
weight: 11303
loglevle: vvvv
casename: c0130
relatedlink: ../../block-func/c0130
log_ref_file: ./reflogs/c0130_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0130_vvvvv
weight: 11304
loglevle: vvvvv
casename: c0130
relatedlink: ../../block-func/c0130
log_ref_file: ./reflogs/c0130_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0130_vvvvvv
weight: 11305
loglevle: vvvvvv
casename: c0130
relatedlink: ../../block-func/c0130
log_ref_file: ./reflogs/c0130_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0130_vvvvvvv
weight: 11306
loglevle: vvvvvvv
casename: c0130
relatedlink: ../../block-func/c0130
log_ref_file: ./reflogs/c0130_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0131.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
380
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0131_v
weight: 11310
loglevle: v
casename: c0131
relatedlink: ../../flow-controll/c0131
log_ref_file: ./reflogs/c0131_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0131_vv
weight: 11311
loglevle: vv
casename: c0131
relatedlink: ../../flow-controll/c0131
log_ref_file: ./reflogs/c0131_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0131_vvv
weight: 11312
loglevle: vvv
casename: c0131
relatedlink: ../../flow-controll/c0131
log_ref_file: ./reflogs/c0131_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0131_vvvv
weight: 11313
loglevle: vvvv
casename: c0131
relatedlink: ../../flow-controll/c0131
log_ref_file: ./reflogs/c0131_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0131_vvvvv
weight: 11314
loglevle: vvvvv
casename: c0131
relatedlink: ../../flow-controll/c0131
log_ref_file: ./reflogs/c0131_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0131_vvvvvv
weight: 11315
loglevle: vvvvvv
casename: c0131
relatedlink: ../../flow-controll/c0131
log_ref_file: ./reflogs/c0131_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0131_vvvvvvv
weight: 11316
loglevle: vvvvvvv
casename: c0131
relatedlink: ../../flow-controll/c0131
log_ref_file: ./reflogs/c0131_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0132.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
582
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0132_v
weight: 11320
loglevle: v
casename: c0132
relatedlink: ../../test-debug/c0132
log_ref_file: ./reflogs/c0132_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0132_vv
weight: 11321
loglevle: vv
casename: c0132
relatedlink: ../../test-debug/c0132
log_ref_file: ./reflogs/c0132_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0132_vvv
weight: 11322
loglevle: vvv
casename: c0132
relatedlink: ../../test-debug/c0132
log_ref_file: ./reflogs/c0132_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0132_vvvv
weight: 11323
loglevle: vvvv
casename: c0132
relatedlink: ../../test-debug/c0132
log_ref_file: ./reflogs/c0132_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0132_vvvvv
weight: 11324
loglevle: vvvvv
casename: c0132
relatedlink: ../../test-debug/c0132
log_ref_file: ./reflogs/c0132_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0132_vvvvvv
weight: 11325
loglevle: vvvvvv
casename: c0132
relatedlink: ../../test-debug/c0132
log_ref_file: ./reflogs/c0132_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0132_vvvvvvv
weight: 11326
loglevle: vvvvvvv
casename: c0132
relatedlink: ../../test-debug/c0132
log_ref_file: ./reflogs/c0132_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0133.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
328
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0133_v
weight: 11330
loglevle: v
casename: c0133
relatedlink: ../../test-debug/c0133
log_ref_file: ./reflogs/c0133_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0133_vv
weight: 11331
loglevle: vv
casename: c0133
relatedlink: ../../test-debug/c0133
log_ref_file: ./reflogs/c0133_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0133_vvv
weight: 11332
loglevle: vvv
casename: c0133
relatedlink: ../../test-debug/c0133
log_ref_file: ./reflogs/c0133_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0133_vvvv
weight: 11333
loglevle: vvvv
casename: c0133
relatedlink: ../../test-debug/c0133
log_ref_file: ./reflogs/c0133_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0133_vvvvv
weight: 11334
loglevle: vvvvv
casename: c0133
relatedlink: ../../test-debug/c0133
log_ref_file: ./reflogs/c0133_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0133_vvvvvv
weight: 11335
loglevle: vvvvvv
casename: c0133
relatedlink: ../../test-debug/c0133
log_ref_file: ./reflogs/c0133_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0133_vvvvvvv
weight: 11336
loglevle: vvvvvvv
casename: c0133
relatedlink: ../../test-debug/c0133
log_ref_file: ./reflogs/c0133_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0134.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
377
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0134_v
weight: 11340
loglevle: v
casename: c0134
relatedlink: ../../block-func/c0134
log_ref_file: ./reflogs/c0134_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0134_vv
weight: 11341
loglevle: vv
casename: c0134
relatedlink: ../../block-func/c0134
log_ref_file: ./reflogs/c0134_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0134_vvv
weight: 11342
loglevle: vvv
casename: c0134
relatedlink: ../../block-func/c0134
log_ref_file: ./reflogs/c0134_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0134_vvvv
weight: 11343
loglevle: vvvv
casename: c0134
relatedlink: ../../block-func/c0134
log_ref_file: ./reflogs/c0134_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0134_vvvvv
weight: 11344
loglevle: vvvvv
casename: c0134
relatedlink: ../../block-func/c0134
log_ref_file: ./reflogs/c0134_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0134_vvvvvv
weight: 11345
loglevle: vvvvvv
casename: c0134
relatedlink: ../../block-func/c0134
log_ref_file: ./reflogs/c0134_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0134_vvvvvvv
weight: 11346
loglevle: vvvvvvv
casename: c0134
relatedlink: ../../block-func/c0134
log_ref_file: ./reflogs/c0134_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0135.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
389
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0135_v
weight: 11350
loglevle: v
casename: c0135
relatedlink: ../../block-func/c0135
log_ref_file: ./reflogs/c0135_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0135_vv
weight: 11351
loglevle: vv
casename: c0135
relatedlink: ../../block-func/c0135
log_ref_file: ./reflogs/c0135_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0135_vvv
weight: 11352
loglevle: vvv
casename: c0135
relatedlink: ../../block-func/c0135
log_ref_file: ./reflogs/c0135_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0135_vvvv
weight: 11353
loglevle: vvvv
casename: c0135
relatedlink: ../../block-func/c0135
log_ref_file: ./reflogs/c0135_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0135_vvvvv
weight: 11354
loglevle: vvvvv
casename: c0135
relatedlink: ../../block-func/c0135
log_ref_file: ./reflogs/c0135_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0135_vvvvvv
weight: 11355
loglevle: vvvvvv
casename: c0135
relatedlink: ../../block-func/c0135
log_ref_file: ./reflogs/c0135_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0135_vvvvvvv
weight: 11356
loglevle: vvvvvvv
casename: c0135
relatedlink: ../../block-func/c0135
log_ref_file: ./reflogs/c0135_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0136.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
303
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0136_v
weight: 11360
loglevle: v
casename: c0136
relatedlink: ../../block-func/c0136
log_ref_file: ./reflogs/c0136_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0136_vv
weight: 11361
loglevle: vv
casename: c0136
relatedlink: ../../block-func/c0136
log_ref_file: ./reflogs/c0136_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0136_vvv
weight: 11362
loglevle: vvv
casename: c0136
relatedlink: ../../block-func/c0136
log_ref_file: ./reflogs/c0136_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0136_vvvv
weight: 11363
loglevle: vvvv
casename: c0136
relatedlink: ../../block-func/c0136
log_ref_file: ./reflogs/c0136_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0136_vvvvv
weight: 11364
loglevle: vvvvv
casename: c0136
relatedlink: ../../block-func/c0136
log_ref_file: ./reflogs/c0136_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0136_vvvvvv
weight: 11365
loglevle: vvvvvv
casename: c0136
relatedlink: ../../block-func/c0136
log_ref_file: ./reflogs/c0136_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0136_vvvvvvv
weight: 11366
loglevle: vvvvvvv
casename: c0136
relatedlink: ../../block-func/c0136
log_ref_file: ./reflogs/c0136_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0137.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
340
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0137_v
weight: 11370
loglevle: v
casename: c0137
relatedlink: ../../test-debug/c0137
log_ref_file: ./reflogs/c0137_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0137_vv
weight: 11371
loglevle: vv
casename: c0137
relatedlink: ../../test-debug/c0137
log_ref_file: ./reflogs/c0137_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0137_vvv
weight: 11372
loglevle: vvv
casename: c0137
relatedlink: ../../test-debug/c0137
log_ref_file: ./reflogs/c0137_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0137_vvvv
weight: 11373
loglevle: vvvv
casename: c0137
relatedlink: ../../test-debug/c0137
log_ref_file: ./reflogs/c0137_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0137_vvvvv
weight: 11374
loglevle: vvvvv
casename: c0137
relatedlink: ../../test-debug/c0137
log_ref_file: ./reflogs/c0137_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0137_vvvvvv
weight: 11375
loglevle: vvvvvv
casename: c0137
relatedlink: ../../test-debug/c0137
log_ref_file: ./reflogs/c0137_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0137_vvvvvvv
weight: 11376
loglevle: vvvvvvv
casename: c0137
relatedlink: ../../test-debug/c0137
log_ref_file: ./reflogs/c0137_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0138.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
588
~~~~SubStep3: [template: template the document ]
 WARN: [filecontent readfile] - [please fix file path: ./reflogs/c0138_vvv.log]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0138_v
weight: 11380
loglevle: v
casename: c0138
relatedlink: ../../test-debug/c0138
log_ref_file: ./reflogs/c0138_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
 WARN: [filecontent readfile] - [please fix file path: ./reflogs/c0138_v.log]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0138_vv
weight: 11381
loglevle: vv
casename: c0138
relatedlink: ../../test-debug/c0138
log_ref_file: ./reflogs/c0138_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
 WARN: [filecontent readfile] - [please fix file path: ./reflogs/c0138_vv.log]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0138_vvv
weight: 11382
loglevle: vvv
casename: c0138
relatedlink: ../../test-debug/c0138
log_ref_file: ./reflogs/c0138_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
 WARN: [filecontent readfile] - [please fix file path: ./reflogs/c0138_vvv.log]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0138_vvvv
weight: 11383
loglevle: vvvv
casename: c0138
relatedlink: ../../test-debug/c0138
log_ref_file: ./reflogs/c0138_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
 WARN: [filecontent readfile] - [please fix file path: ./reflogs/c0138_vvvv.log]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0138_vvvvv
weight: 11384
loglevle: vvvvv
casename: c0138
relatedlink: ../../test-debug/c0138
log_ref_file: ./reflogs/c0138_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
 WARN: [filecontent readfile] - [please fix file path: ./reflogs/c0138_vvvvv.log]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0138_vvvvvv
weight: 11385
loglevle: vvvvvv
casename: c0138
relatedlink: ../../test-debug/c0138
log_ref_file: ./reflogs/c0138_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
 WARN: [filecontent readfile] - [please fix file path: ./reflogs/c0138_vvvvvv.log]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0138_vvvvvvv
weight: 11386
loglevle: vvvvvvv
casename: c0138
relatedlink: ../../test-debug/c0138
log_ref_file: ./reflogs/c0138_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
 WARN: [filecontent readfile] - [please fix file path: ./reflogs/c0138_vvvvvvv.log]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0139.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
265
~~~~SubStep3: [template: template the document ]
 WARN: [filecontent readfile] - [please fix file path: ./reflogs/c0139_vvv.log]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0139_v
weight: 11390
loglevle: v
casename: c0139
relatedlink: ../../test-debug/c0139
log_ref_file: ./reflogs/c0139_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
 WARN: [filecontent readfile] - [please fix file path: ./reflogs/c0139_v.log]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0139_vv
weight: 11391
loglevle: vv
casename: c0139
relatedlink: ../../test-debug/c0139
log_ref_file: ./reflogs/c0139_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
 WARN: [filecontent readfile] - [please fix file path: ./reflogs/c0139_vv.log]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0139_vvv
weight: 11392
loglevle: vvv
casename: c0139
relatedlink: ../../test-debug/c0139
log_ref_file: ./reflogs/c0139_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
 WARN: [filecontent readfile] - [please fix file path: ./reflogs/c0139_vvv.log]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0139_vvvv
weight: 11393
loglevle: vvvv
casename: c0139
relatedlink: ../../test-debug/c0139
log_ref_file: ./reflogs/c0139_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
 WARN: [filecontent readfile] - [please fix file path: ./reflogs/c0139_vvvv.log]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0139_vvvvv
weight: 11394
loglevle: vvvvv
casename: c0139
relatedlink: ../../test-debug/c0139
log_ref_file: ./reflogs/c0139_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
 WARN: [filecontent readfile] - [please fix file path: ./reflogs/c0139_vvvvv.log]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0139_vvvvvv
weight: 11395
loglevle: vvvvvv
casename: c0139
relatedlink: ../../test-debug/c0139
log_ref_file: ./reflogs/c0139_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
 WARN: [filecontent readfile] - [please fix file path: ./reflogs/c0139_vvvvvv.log]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0139_vvvvvvv
weight: 11396
loglevle: vvvvvvv
casename: c0139
relatedlink: ../../test-debug/c0139
log_ref_file: ./reflogs/c0139_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
 WARN: [filecontent readfile] - [please fix file path: ./reflogs/c0139_vvvvvvv.log]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing c0140.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
1288
~~~~SubStep3: [template: template the document ]
 WARN: [filecontent readfile] - [please fix file path: ./reflogs/c0140_vvv.log]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0140_v
weight: 11400
loglevle: v
casename: c0140
relatedlink: ../../usage/c0140
log_ref_file: ./reflogs/c0140_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
 WARN: [filecontent readfile] - [please fix file path: ./reflogs/c0140_v.log]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0140_vv
weight: 11401
loglevle: vv
casename: c0140
relatedlink: ../../usage/c0140
log_ref_file: ./reflogs/c0140_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
 WARN: [filecontent readfile] - [please fix file path: ./reflogs/c0140_vv.log]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0140_vvv
weight: 11402
loglevle: vvv
casename: c0140
relatedlink: ../../usage/c0140
log_ref_file: ./reflogs/c0140_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
 WARN: [filecontent readfile] - [please fix file path: ./reflogs/c0140_vvv.log]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0140_vvvv
weight: 11403
loglevle: vvvv
casename: c0140
relatedlink: ../../usage/c0140
log_ref_file: ./reflogs/c0140_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
 WARN: [filecontent readfile] - [please fix file path: ./reflogs/c0140_vvvv.log]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0140_vvvvv
weight: 11404
loglevle: vvvvv
casename: c0140
relatedlink: ../../usage/c0140
log_ref_file: ./reflogs/c0140_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
 WARN: [filecontent readfile] - [please fix file path: ./reflogs/c0140_vvvvv.log]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0140_vvvvvv
weight: 11405
loglevle: vvvvvv
casename: c0140
relatedlink: ../../usage/c0140
log_ref_file: ./reflogs/c0140_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
 WARN: [filecontent readfile] - [please fix file path: ./reflogs/c0140_vvvvvv.log]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: c0140_vvvvvvv
weight: 11406
loglevle: vvvvvvv
casename: c0140
relatedlink: ../../usage/c0140
log_ref_file: ./reflogs/c0140_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
 WARN: [filecontent readfile] - [please fix file path: ./reflogs/c0140_vvvvvvv.log]
=Task2: [build ==> process_main_entry:  ]
--Step1: [: prepare all the self documented cases ]
cmd( 1):
f0001.yml
f0002.yml
f0009.yml
f0016.yml
f0018.yml
f0031.yml
f0037.yml
f0045.yml
f0053.yml
f0060.yml
f0061.yml
f0067.yml
f0077.yml
f0088.yml
f0097.yml
f0116.yml
f0117.yml
 .. ok
. ok
--Step2: [: debug ]
 WARN: [*] - [Step is deactivated!]
--Step3: [: get the case list object ]
 WARN: [cmd] - [Not implemented or void for no action!]
--Step4: [: filter out all cases have already got documents ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing f0001.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing f0002.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing f0009.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing f0016.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing f0018.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing f0031.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing f0037.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing f0045.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
===Task4: [build/process_main_entry/process_case ==> data_enrich:  ]
----Step1:
~~~~SubStep1: [yml_write:  ]
~~~~SubStep2: [yml_write:  ]
~~~~SubStep3: [yml_write:  ]
~~~~SubStep4: [yml_write:  ]
~~~~SubStep5: [query:  ]
~~~~SubStep6: [print:  ]
existing weight: 
weight string length: 0

----Step2:
~~~~SubStep1: [print:  ]
setup weight
~~~~SubStep2: [yml_write:  ]
----Step3:
~~~~SubStep1: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [return:  ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
===Task5: [build/process_main_entry/process_case ==> generate_doc:  ]
----Step1:
~~~~SubStep1: [reg:  ]
----Step2:
cmd( 1):

 .. ok
. ok
----Step3: [: debug and exit ]
 WARN: [*] - [Step is deactivated!]
----Step4: [: generate the document ]
~~~~SubStep1: [printobj:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~~SubStep2: [print: casedoc length ]
446
~~~~SubStep3: [template: template the document ]
----Step5: [loop_verbose_level: loop all different verbose level caller ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: f0045_v
weight: 10450
loglevle: v
casename: f0045
relatedlink: ../../env-vars/f0045
log_ref_file: ./reflogs/f0045_v.log

~~~~~SubStep2: [print:  ]
v
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: f0045_vv
weight: 10451
loglevle: vv
casename: f0045
relatedlink: ../../env-vars/f0045
log_ref_file: ./reflogs/f0045_vv.log

~~~~~SubStep2: [print:  ]
vv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: f0045_vvv
weight: 10452
loglevle: vvv
casename: f0045
relatedlink: ../../env-vars/f0045
log_ref_file: ./reflogs/f0045_vvv.log

~~~~~SubStep2: [print:  ]
vvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: f0045_vvvv
weight: 10453
loglevle: vvvv
casename: f0045
relatedlink: ../../env-vars/f0045
log_ref_file: ./reflogs/f0045_vvvv.log

~~~~~SubStep2: [print:  ]
vvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: f0045_vvvvv
weight: 10454
loglevle: vvvvv
casename: f0045
relatedlink: ../../env-vars/f0045
log_ref_file: ./reflogs/f0045_vvvvv.log

~~~~~SubStep2: [print:  ]
vvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: f0045_vvvvvv
weight: 10455
loglevle: vvvvvv
casename: f0045
relatedlink: ../../env-vars/f0045
log_ref_file: ./reflogs/f0045_vvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvv
~~~~~SubStep3: [template: generate the log ]
====Task6: [build/process_main_entry/process_case/generate_doc ==> generate_log_pages: generage verbose level log ]
-----Step1:
~~~~~SubStep1: [print:  ]
title: f0045_vvvvvvv
weight: 10456
loglevle: vvvvvvv
casename: f0045
relatedlink: ../../env-vars/f0045
log_ref_file: ./reflogs/f0045_vvvvvvv.log

~~~~~SubStep2: [print:  ]
vvvvvvv
~~~~~SubStep3: [template: generate the log ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing f0053.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing f0060.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing f0061.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing f0067.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing f0077.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing f0088.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing f0097.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing f0116.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
==Task3: [build/process_main_entry ==> process_case:  ]
---Step1:
~~~SubStep1: [reg:  ]
~~~SubStep2: [reg:  ]
~~~SubStep3: [print:  ]
processing f0117.yml
~~~SubStep4: [readfile: read yml file ]
~~~SubStep5: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep6: [query:  ]
---Step2: [: add casename and log_dir into data model ]
---Step3:
~~~SubStep1: [readfile: review if the file content is correct ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep2: [print:  ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep3: [yml_delete:  ]
~~~SubStep4: [yml_delete:  ]
~~~SubStep5: [readfile: review if the file content is correct ]
~~~SubStep6: [print: show the modified yml content ]
 WARN: [cmd] - [temporarily deactivated]
~~~SubStep7: [:  ]
 WARN: [cmd] - [temporarily deactivated]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5: [: create sub folders and template the docment ]
-Step4: [: copy generated docs to publish dir ]
cmd( 1):

 .. ok
cmd( 2):

 .. ok
cmd( 3):

 .. ok
cmd( 4):

 .. ok
cmd( 5):

 .. ok
. ok

```
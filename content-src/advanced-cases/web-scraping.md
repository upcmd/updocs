---
title: "web scraping example"
date: 2020-02-07T14:32:46+11:00
draft: false
weight: 2010
---

#### A scraping example

* disclaimer

Please note that this is only an example, not to meant to be a hack and below exercise does not use any credential and it is only used for technical demostration purpose only 

* list tasks

```
Ξ ▶ up list
loading [Config]:  ./upconfig
loading [Task]:  ./up
instance id: nonamed
-task list
     1|                task:  
     2|          probe_page:  
     3| locate_downloadpage:  
     4|            download:  
-

Ξ ▶ up list =      
loading [Config]:  ./upconfig
loading [Task]:  ./up
instance id: nonamed
-inspect all tasks:
task:  WARN: [evaluate max task stack layer] - [please setup max MaxCallLayers correctly, or fix recursive cycle calls]
 WARN: [evaluate max task stack layer] - [please setup max MaxCallLayers correctly, or fix recursive cycle calls]
.
├── [ /loop..]  
│   └── [probe_page]  
│       ├── : 
│       ├── : 
│       ├── : 
│       └── [ /loop..]  
│           └── [locate_downloadpage]  
│               ├── : 
│               ├── : 
│               ├── : 
│               ├── : 
│               ├── : 
│               ├── : 
│               └── [ /loop..]  
│                   └── [probe_page]  
│                       ├── : 
│                       ├── : 
│                       ├── : 
│                       └── [ /loop..]  
│                           └── [locate_downloadpage]  
│                               ├── : 
│                               ├── : 
│                               ├── : 
│                               ├── : 
│                               ├── : 
│                               ├── : 
│                               └── [ /loop..]  
│                                   └── [probe_page]  
│                                       ├── : 
│                                       ├── : 
│                                       ├── : 
│                                       └── [ /loop..]  
│                                           └── [locate_downloadpage]  
│                                               ├── : 
│                                               ├── : 
│                                               ├── : 
│                                               ├── : 
│                                               ├── : 
│                                               ├── : 
│                                               └── [ /loop..]  
│                                                   └── [probe_page]  
│                                                       ├── : 
│                                                       ├── : 
│                                                       ├── : 
│                                                       └── [ /loop..]  
│                                                           └── [locate_downloadpage]  
│                                                               ├── : 
│                                                               ├── : 
│                                                               ├── : 
│                                                               ├── : 
│                                                               ├── : 
│                                                               ├── : 
│                                                               └── [ /loop..]  
├── : 
└── [ /loop..]  

probe_page:  WARN: [evaluate max task stack layer] - [please setup max MaxCallLayers correctly, or fix recursive cycle calls]
.
├── : 
├── : 
├── : 
└── [ /loop..]  
    └── [locate_downloadpage]  
        ├── : 
        ├── : 
        ├── : 
        ├── : 
        ├── : 
        ├── : 
        └── [ /loop..]  
            └── [probe_page]  
                ├── : 
                ├── : 
                ├── : 
                └── [ /loop..]  
                    └── [locate_downloadpage]  
                        ├── : 
                        ├── : 
                        ├── : 
                        ├── : 
                        ├── : 
                        ├── : 
                        └── [ /loop..]  
                            └── [probe_page]  
                                ├── : 
                                ├── : 
                                ├── : 
                                └── [ /loop..]  
                                    └── [locate_downloadpage]  
                                        ├── : 
                                        ├── : 
                                        ├── : 
                                        ├── : 
                                        ├── : 
                                        ├── : 
                                        └── [ /loop..]  
                                            └── [probe_page]  
                                                ├── : 
                                                ├── : 
                                                ├── : 
                                                └── [ /loop..]  
                                                    └── [locate_downloadpage]  
                                                        ├── : 
                                                        ├── : 
                                                        ├── : 
                                                        ├── : 
                                                        ├── : 
                                                        ├── : 
                                                        └── [ /loop..]  
                                                            └── [probe_page]  
                                                                ├── : 
                                                                ├── : 
                                                                ├── : 
                                                                └── [ /loop..]  

locate_downloadpage:  WARN: [evaluate max task stack layer] - [please setup max MaxCallLayers correctly, or fix recursive cycle calls]
.
├── : 
├── : 
├── : 
├── : 
├── : 
├── : 
└── [ /loop..]  
    └── [probe_page]  
        ├── : 
        ├── : 
        ├── : 
        └── [ /loop..]  
            └── [locate_downloadpage]  
                ├── : 
                ├── : 
                ├── : 
                ├── : 
                ├── : 
                ├── : 
                └── [ /loop..]  
                    └── [probe_page]  
                        ├── : 
                        ├── : 
                        ├── : 
                        └── [ /loop..]  
                            └── [locate_downloadpage]  
                                ├── : 
                                ├── : 
                                ├── : 
                                ├── : 
                                ├── : 
                                ├── : 
                                └── [ /loop..]  
                                    └── [probe_page]  
                                        ├── : 
                                        ├── : 
                                        ├── : 
                                        └── [ /loop..]  
                                            └── [locate_downloadpage]  
                                                ├── : 
                                                ├── : 
                                                ├── : 
                                                ├── : 
                                                ├── : 
                                                ├── : 
                                                └── [ /loop..]  
                                                    └── [probe_page]  
                                                        ├── : 
                                                        ├── : 
                                                        ├── : 
                                                        └── [ /loop..]  
                                                            └── [locate_downloadpage]  
                                                                ├── : 
                                                                ├── : 
                                                                ├── : 
                                                                ├── : 
                                                                ├── : 
                                                                ├── : 
                                                                └── [ /loop..]  

download: .
├── : 
├── : 
└── : 

```

* up config

```

vars:
  baseurl: https://www.downloadtest.com
  saveto: ./pdflinks

tasks:
  -
    name: task
    task:
      - func: call
        vars:
          trylink: https://www.downloadtest.com
        do:
          - probe_page

      -
        func: shell
        do:
          - 'cat ./pdflinks |sort|uniq > {{.saveto}}'

      - func: call
        do: download

  -
    name: probe_page
    task:
      -
        func: cmd
        do:
          -
            name: print
            cmd: 'trylink: {{.trylink}}'

      -
        func: shell
        do:
          - |
            curl -s {{.trylink}} | grep ".html" |grep list_lnk |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"title="); print a[1]}' |tr -d \"
        reg: pagelist

      -
        func: cmd
        dvars:
          - name: void
            value: '{{ .pagelist | splitLines |reg "pagelist_object" }}'

      - func: call
        do:
          - locate_downloadpage
        loop: pagelist_object

  -
    name: locate_downloadpage
    task:
      -
        func: cmd
        dvars:
          - name: pageurl
            value: '{{.baseurl}}{{.loopitem}}'
            flags:
              - taskscope
        do:
          -
            name: print
            cmd: '{{.loopitem}}'

          -
            name: print
            cmd: 'locate pdf from: {{.pageurl}}'

          -
            name: print
            cmd: |
              curl -s {{.pageurl}} |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

      -
        func: shell
        do:
          - |
            curl -s {{.pageurl}} |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "|awk '{$1=$1};1'
        reg: pdflinks
        flags:
          - ignore_error

      -
        func: cmd
        do:
          -
            name: print
            cmd: 'return code: {{.last_result.Code}}'

          -
            name: reg
            cmd:
              name: errorcode
              value: '{{.last_result.Code}}'

      -
        func: cmd
        dvars:
          - name: void
            value: '{{ .pdflinks | splitLines |reg "pdflinks_object" }}'
        if: '{{gt (.pdflinks|len) 0}}'

      -
        func: shell
        do:
          - echo "processing {{.baseurl}}{{.loopitem}}"
          - 'echo """{{.baseurl}}{{.loopitem}}""" >> {{.saveto}}'
        loop: pdflinks_object

      -
        func: cmd
        do:
          -
            name: break
        if: '{{eq  .up_runtime_layer_number 4}}'

      -
        func: call
        dvars:
          - name: trylink
            value: '{{.pageurl}}'
        do:
          - probe_page
        if: '{{ne .errorcode "0"}}'


  -
    name: download
    task:
      -
        func: cmd
        do:
          -
            name: readfile
            cmd:
              filename: pdflinks
              dir: ./
              reg: urls

      -
        func: cmd
        dvars:
          - name: void
            value: '{{ .urls | splitLines |reg "urllist" }}'
            flags:
              - vvv
      -
        func: shell
        do:
          - echo {{.loopitem}}
          - |
              filename=`echo {{.loopitem}} |awk '{split($0,a,"pdf"); print a[1]}'|awk '{split($0,a,"/"); print a[5]}'`
              savename="$filename"pdf
              echo "save to: $savename"
              curl -o ./pdf/$savename {{.loopitem}}
        loop: urllist

```

#### Logging

```

loading [Config]:  ./upconfig
-exec task: task
loading [Task]:  ./up
instance id: nonamed
Task1: [task ==> task:  ]
-Step1:
=Task2: [task ==> probe_page:  ]
--Step1:
~~SubStep1: [print:  ]
trylink: https://www.downloadtest.com
--Step2:
cmd( 1):
/addition.html 
/full-algebra.html 
/full-angles.html 
/area.html 
/greater-less-than.html 
/counting.html 
/full-decimals.html 
/division.html 
/long-division.html 
/fractions.html 
/geometry.html 
/graphing.html 
/hundreds-chart.html 
/measurement.html 
/money.html 
/multiplication-basic-index.html 
/multiplication2.html 
/ordered-pairs.html 
/percents.html 
/perimeter.html 
/place-value.html 
/probability.html 
/rounding.html 
/full-skip-counting.html 
/subtraction.html 
/time.html 
/volume.html 
/word-problems-multi-step.html 
/full-math.html 
/full-math.html 
/1st-comprehension.html 
/2nd-comprehension.html 
/3rd-comprehension.html 
/4th-comprehension.html 
/5th-comprehension.html 
/6th-comprehension.html 
/comprehension.html 
/reading.html 
/causeeffect.html 
/factopinion.html 
/graphic-organizers.html 
/proofreading.html 
/synonyms-antonyms.html 
/writingideas.html 
/writing-storypics.html 
/writing.html 
/full-ela.html 
/letters-phonics.html 
/letters-phonics-vowels.html 
/phonics-blends.html 
/phonics-digraphs.html 
/word-families.html 
/full-phonics.html 
/full-letters-alphabet.html 
/build-a-sentence.html 
/full-sight-words.html 
/sight-words-individual.html 
/full-early-literacy.html 
/nouns.html 
/actionverbs.html 
/adjectives.html 
/adverbs.html 
/pronoun.html 
/punctuation.html 
/syllables.html 
/subjectpredicate.html 
/grammar.html 
/spelling-level-a.html 
/spelling-level-b.html 
/spelling-level-c.html 
/spelling-level-d.html 
/spelling-level-e.html 
/full-spelling.html 
/book-bunnicula.html 
/charlottes-web.html 
/book-magictreehouse-dinosaursbeforedark.html 
/book-the-boxcar-children.html 
/full-books.html 
/animals.html 
/animal-articles.html 
/butterfly-life-cycle.html 
/electricity.html 
/human-body.html 
/matter.html 
/simple-machines.html 
/solar-system-planets.html 
/weather.html 
/full-science.html 
/states-individual.html 
/explorers.html  
/landforms.html 
/maps.html  
/map-skills.html  
/full-social-studies.html 
/spring.html 
/mothers-day.html 
/fathers-day.html 
/calendars.html 
/full-holiday.html 
/brain-teasers.html 
/addition-squares.html 
/mystery-graph-picture.html 
/number-detective.html 
/states-lost.html 
/full-puzzles.html 
/teachingtools.html 
/awards.html 
/full-teacher.html 
/full-letters-alphabet.html 
/counting.html 
/shapes-basic.html 
/full-kindergarten.html 
/generator-word-search.html 
/generator-multiple-choice.html 
/generator-fill-in-the-blank.html 
/full-generators-index.html 
/full-index.html 
/help.html 
/terms-of-use.html 
/contact-us.html
 .. ok
. ok
--Step3:
 WARN: [cmd] - [Not implemented or void for no action!]
--Step4:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/addition.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/addition.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/addition.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):

 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
---Step5:
cmd( 1):
processing https://www.downloadtest.com
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/full-algebra.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/full-algebra.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/full-algebra.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):

 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
---Step5:
cmd( 1):
processing https://www.downloadtest.com
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/full-angles.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/full-angles.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/full-angles.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):

 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
---Step5:
cmd( 1):
processing https://www.downloadtest.com
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/area.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/area.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/area.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):

 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
---Step5:
cmd( 1):
processing https://www.downloadtest.com
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/greater-less-than.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/greater-less-than.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/greater-less-than.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):

 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
---Step5:
cmd( 1):
processing https://www.downloadtest.com
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/counting.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/counting.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/counting.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):

 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
---Step5:
cmd( 1):
processing https://www.downloadtest.com
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/full-decimals.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/full-decimals.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/full-decimals.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):

 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
---Step5:
cmd( 1):
processing https://www.downloadtest.com
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/division.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/division.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/division.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):
/division/division-ihavewhohas_TZNRZ.pdf?up=1467361866
/division/division-ihavewhohas_TZNRZ.pdf?up=1467361866
/division/division_matching_game_TZNTB.pdf?up=1466611200
/division/division_matching_game_TZNTB.pdf?up=1466611200
/division/cut-out-stars-division_DVSTR.pdf?up=1516109311
/division/division-fluency-game-1_BBAGT.pdf?up=1466611200
/division/division-puzzles-dividends-81-divisors-9_QURTE.pdf?up=1525333807
/division/division-numberhunt_AZZTY.pdf?up=1542276161
/division/caterpillar-division-within-100_PHCFG.pdf?up=1536579132
/division/division-flashcards_TZNTD.pdf?up=1466611200
/division/small-division-flashcards_SMDIV.pdf?up=1466611200
/multiplication/fact-families-cards-multiplication_TZNTF.pdf?up=1466611200
/division/division-cut-glue-basic_KDSFJ.pdf?up=1534413496
/division/equal-groups_TZNTM.pdf?up=1466611200
/division/equal-groups2_TZNTN.pdf?up=1466611200
/division/Division-1_JKLHJ.pdf?up=1556615247
/division/math-riddle-four-wheels-and-flies_ABGTH.pdf?up=1466611200
/division/division-1_TZNTR.pdf?up=1466611200
/division/multiplication-and-division-fact-families_TZNTT.pdf?up=1466611200
/division/counting-objects-division_TZNTW.pdf?up=1466611200
/division/basic-div-9_TZNTZ.pdf?up=1498640490
/division/division-thinking-questions_THINK.pdf?up=1466611200
/division/division-2_TZNWB.pdf?up=1466611200
/division/math-riddle-salt-water-sharks_HHYAU.pdf?up=1466611200
/division/division-properties_QUOTI.pdf?up=1473179981
/division/division-numberhunt_TZNWD.pdf?up=1466611200
/division/tic-tac-toe-division_TZNWF.pdf?up=1466611200
/division/basic-division-wordproblems_TZNWQ.pdf?up=1466611200
/division/scm-division-basic-facts_WRYIN.pdf?up=1488790178
/multiplication/multiplication-division-arrows_TZNWM.pdf?up=1466611200
/division/arrays-division_TTAES.pdf?up=1527070042
/division/using-arrays-to-divide_GGTAS.pdf?up=1527072363
/multiplication/fact-family-arrays_LPKOJ.pdf?up=1527061539
/division/division-quiz-0-2_TZNWR.pdf?up=1466611200
/division/division-quiz-0-3_TZNWT.pdf?up=1466611200
/division/division-quiz-0-4_TZNWW.pdf?up=1466611200
/division/division-quiz-0-5_TZNWZ.pdf?up=1466611200
/division/division-quiz-0-6_TZNZB.pdf?up=1466611200
/division/division-quiz-0-7_TZNZD.pdf?up=1466611200
/division/division-quiz-0-8_TZNZF.pdf?up=1466611200
/division/division-quiz-0-9_TZNZQ.pdf?up=1466611200
/division/division-quiz-0-10_TZNZM.pdf?up=1466611200
/division/division-quiz-0-11_TZNZN.pdf?up=1466611200
/division/division-quiz-0-12_DIVIS.pdf?up=1466611200
/division/qr-division-basic-word-problems_WORDP.pdf?up=1466611200
/division/qr-division-basic-qr_BASIC.pdf?up=1466611200
/generator-basic-division.html title=Basic Division Worksheet Generator>Basic Division Worksheet Generator</a><p>Create your own custom division printables with basic facts. You choose the range for the subtrahends and the differences. This generator tool lets you to create worksheets with 25 or 50 problems. Choose whether or not you want remainders.</p><div class=cat style=margin-top:20px><a name=I20139 id=I20139></a><div><h2 class=catTitle>Mystery Pictures</h2></div></div><div class=pdfBox><div class=pdfTtitle><a data-type=LoggedInPDF
/division/mystery-picture-division-back-to-school_CUTST.pdf?up=1581581196
/division/mystery-picture-division-beach_TZRBT.pdf?up=1499246843
/division/mystery-picture-division-clownfish_WQWZW.pdf?up=1499246847
/division/mystery-picture-division-fox_JWIOX.pdf?up=1518003582
/division/mystery-picture-division-balloon_WQWZR.pdf?up=1499246841
/division/mystery-picture-division-ladybug_ABZGT.pdf?up=1499246854
/division/mystery-picture-division-owl_ABZGF.pdf?up=1499246856
/division/mystery-picture-division-parrot_WQWZN.pdf?up=1499246857
/division/mystery-picture-division-robot_PEJSO.pdf?up=1518003583
/division/mystery-picture-division-rocket_TZRBF.pdf?up=1499246858
/division/mystery-picture-division-rooster_WQWZT.pdf?up=1499246859
/division/mystery-picture-division-beach-2_DIVID.pdf?up=1581581195
/division/mystery-picture-division-sea-monster_POLAU.pdf?up=1499246863
/division/mystery-picture-division-dog_WQWZZ.pdf?up=1499246849
/division/mystery-picture-division-sailboat_WQZBB.pdf?up=1499246860
/division/mystery-picture-division-whale_DIVIS.pdf?up=1499246866
/division/mystery-picture-division-fruit_WQZBD.pdf?up=1499246852
/division/mystery-picture-division-castle_WQZBF.pdf?up=1499246844
/division/mystery-picture-division-airplane_DIVID.pdf?up=1499246838
/division/cootie-catcher-math-divide-hardest_GAJXK.pdf?up=1466611200
/division/cootie-catcher-math-divide-2_ASHXS.pdf?up=1466611200
/division/cootie-catcher-math-divide-3_KAJJU.pdf?up=1466611200
/division/cootie-catcher-math-divide-4_AUDHX.pdf?up=1466611200
/division/cootie-catcher-math-divide-5_UHDNX.pdf?up=1466611200
/division/cootie-catcher-math-divide-6_XYSHD.pdf?up=1466611200
/division/cootie-catcher-math-divide-7_QUIEJ.pdf?up=1466611200
/division/cootie-catcher-math-divide-8_WJXUI.pdf?up=1466611200
/division/cootie-catcher-math-divide-9_GFSJK.pdf?up=1466611200
/division/cootie-catcher-math-divide-10_ETRDL.pdf?up=1466611200
/division/cootie-catcher-math-divide-11_DHSTW.pdf?up=1466611200
/division/cootie-catcher-math-divide-12_CJHNS.pdf?up=1466611200
/division/division-w-remainders_TZRBZ.pdf?up=1583228660
/division/math-riddle-the-happy-chess-player_KAIJU.pdf?up=1466611200
/division/task-cards-division-remainders_ZSFAR.pdf?up=1466611200
/division/division-3_TZRDB.pdf?up=1580893978
/division/one-digit-quotient-remainders_TZRDD.pdf?up=1580893977
/division/scm-division-1-digit-quotients-with-remainders_VYGHF.pdf?up=1488790188
 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5:
cmd( 1):
processing https://www.downloadtest.com/division/division-ihavewhohas_TZNRZ.pdf?up=1467361866
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/division/division-ihavewhohas_TZNRZ.pdf?up=1467361866
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/division/division_matching_game_TZNTB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/division/division_matching_game_TZNTB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/division/cut-out-stars-division_DVSTR.pdf?up=1516109311
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/division/division-fluency-game-1_BBAGT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/division/division-puzzles-dividends-81-divisors-9_QURTE.pdf?up=1525333807
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/division/division-numberhunt_AZZTY.pdf?up=1542276161
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/division/caterpillar-division-within-100_PHCFG.pdf?up=1536579132
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/division/division-flashcards_TZNTD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/division/small-division-flashcards_SMDIV.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/multiplication/fact-families-cards-multiplication_TZNTF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/division/division-cut-glue-basic_KDSFJ.pdf?up=1534413496
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/division/equal-groups_TZNTM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/division/equal-groups2_TZNTN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/division/Division-1_JKLHJ.pdf?up=1556615247
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/division/math-riddle-four-wheels-and-flies_ABGTH.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/division/division-1_TZNTR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/division/multiplication-and-division-fact-families_TZNTT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/division/counting-objects-division_TZNTW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/division/basic-div-9_TZNTZ.pdf?up=1498640490
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/division/division-thinking-questions_THINK.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/division/division-2_TZNWB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/division/math-riddle-salt-water-sharks_HHYAU.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/division/division-properties_QUOTI.pdf?up=1473179981
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/division/division-numberhunt_TZNWD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/division/tic-tac-toe-division_TZNWF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/division/basic-division-wordproblems_TZNWQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/division/scm-division-basic-facts_WRYIN.pdf?up=1488790178
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/multiplication/multiplication-division-arrows_TZNWM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/division/arrays-division_TTAES.pdf?up=1527070042
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/division/using-arrays-to-divide_GGTAS.pdf?up=1527072363
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/multiplication/fact-family-arrays_LPKOJ.pdf?up=1527061539
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/division/division-quiz-0-2_TZNWR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/division/division-quiz-0-3_TZNWT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/division/division-quiz-0-4_TZNWW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/division/division-quiz-0-5_TZNWZ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/division/division-quiz-0-6_TZNZB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/division/division-quiz-0-7_TZNZD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/division/division-quiz-0-8_TZNZF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/division/division-quiz-0-9_TZNZQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/division/division-quiz-0-10_TZNZM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/division/division-quiz-0-11_TZNZN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/division/division-quiz-0-12_DIVIS.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/division/qr-division-basic-word-problems_WORDP.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/division/qr-division-basic-qr_BASIC.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/generator-basic-division.html title=Basic Division Worksheet Generator>Basic Division Worksheet Generator</a><p>Create your own custom division printables with basic facts. You choose the range for the subtrahends and the differences. This generator tool lets you to create worksheets with 25 or 50 problems. Choose whether or not you want remainders.</p><div class=cat style=margin-top:20px><a name=I20139 id=I20139></a><div><h2 class=catTitle>Mystery Pictures</h2></div></div><div class=pdfBox><div class=pdfTtitle><a data-type=LoggedInPDF
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/division/mystery-picture-division-back-to-school_CUTST.pdf?up=1581581196
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/division/mystery-picture-division-beach_TZRBT.pdf?up=1499246843
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/division/mystery-picture-division-clownfish_WQWZW.pdf?up=1499246847
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/division/mystery-picture-division-fox_JWIOX.pdf?up=1518003582
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/division/mystery-picture-division-balloon_WQWZR.pdf?up=1499246841
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/division/mystery-picture-division-ladybug_ABZGT.pdf?up=1499246854
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/division/mystery-picture-division-owl_ABZGF.pdf?up=1499246856
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/division/mystery-picture-division-parrot_WQWZN.pdf?up=1499246857
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/division/mystery-picture-division-robot_PEJSO.pdf?up=1518003583
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/division/mystery-picture-division-rocket_TZRBF.pdf?up=1499246858
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/division/mystery-picture-division-rooster_WQWZT.pdf?up=1499246859
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/division/mystery-picture-division-beach-2_DIVID.pdf?up=1581581195
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/division/mystery-picture-division-sea-monster_POLAU.pdf?up=1499246863
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/division/mystery-picture-division-dog_WQWZZ.pdf?up=1499246849
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/division/mystery-picture-division-sailboat_WQZBB.pdf?up=1499246860
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/division/mystery-picture-division-whale_DIVIS.pdf?up=1499246866
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/division/mystery-picture-division-fruit_WQZBD.pdf?up=1499246852
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/division/mystery-picture-division-castle_WQZBF.pdf?up=1499246844
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/division/mystery-picture-division-airplane_DIVID.pdf?up=1499246838
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/division/cootie-catcher-math-divide-hardest_GAJXK.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/division/cootie-catcher-math-divide-2_ASHXS.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/division/cootie-catcher-math-divide-3_KAJJU.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/division/cootie-catcher-math-divide-4_AUDHX.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/division/cootie-catcher-math-divide-5_UHDNX.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/division/cootie-catcher-math-divide-6_XYSHD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/division/cootie-catcher-math-divide-7_QUIEJ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/division/cootie-catcher-math-divide-8_WJXUI.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/division/cootie-catcher-math-divide-9_GFSJK.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/division/cootie-catcher-math-divide-10_ETRDL.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/division/cootie-catcher-math-divide-11_DHSTW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/division/cootie-catcher-math-divide-12_CJHNS.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/division/division-w-remainders_TZRBZ.pdf?up=1583228660
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/division/math-riddle-the-happy-chess-player_KAIJU.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/division/task-cards-division-remainders_ZSFAR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/division/division-3_TZRDB.pdf?up=1580893978
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/division/one-digit-quotient-remainders_TZRDD.pdf?up=1580893977
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/division/scm-division-1-digit-quotients-with-remainders_VYGHF.pdf?up=1488790188
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/long-division.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/long-division.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/long-division.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):

 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
---Step5:
cmd( 1):
processing https://www.downloadtest.com
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/fractions.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/fractions.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/fractions.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):

 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
---Step5:
cmd( 1):
processing https://www.downloadtest.com
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/geometry.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/geometry.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/geometry.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):

 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
---Step5:
cmd( 1):
processing https://www.downloadtest.com
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/graphing.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/graphing.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/graphing.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):

 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
---Step5:
cmd( 1):
processing https://www.downloadtest.com
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/hundreds-chart.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/hundreds-chart.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/hundreds-chart.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):
/hundredschart/hundreds-chart-filled_WNRTB.pdf?up=1466611200
/hundredschart/hundreds-chart-filled_WNRTB.pdf?up=1466611200
/hundredschart/hundreds-chart-colored_WNRTD.pdf?up=1466611200
/hundredschart/hundreds-chart-colored_WNRTD.pdf?up=1466611200
/hundredschart/hundreds-chart-partial_WNRTF.pdf?up=1466611200
/hundredschart/hundreds-chart-mostly-filled_NCXZR.pdf?up=1518606864
/hundredschart/hundreds-chart-desktop_WNRTQ.pdf?up=1466611200
/hundredschart/hundreds-chart-blank_HUNDR.pdf?up=1466611200
/hundredschart/rounding-chart-arrows_WNRTM.pdf?up=1466611200
/hundredschart/hundreds-rounding-chart-colors_WNRTN.pdf?up=1466611200
/hundredschart/hundreds-chart-puzzles1_WNRTT.pdf?up=1466611200
/hundredschart/hundreds-chart-puzzles2_WNRTW.pdf?up=1466611200
/hundredschart/hundreds-chart-puzzles3_WNRTZ.pdf?up=1466611200
/hundredschart/100-chart-addition_TWKIZ.pdf?up=1466611200
/hundredschart/100-chart-addition2_GWJXI.pdf?up=1466611200
/hundredschart/99-chart-filled_WNRWQ.pdf?up=1466611200
/hundredschart/99-chart-partial_WNRWN.pdf?up=1466611200
/hundredschart/99-chart-colored_WNRWM.pdf?up=1466611200
/hundredschart/99-chart-desktop_WNRWR.pdf?up=1466611200
/hundredschart/99-rounding-chart-arrows_WNRWT.pdf?up=1466611200
/hundredschart/99-rounding-chart-colors_WNRWW.pdf?up=1466611200
/hundredschart/120-chart_GFWKX.pdf?up=1466611200
/hundredschart/120-chart_GFWKX.pdf?up=1466611200
/hundredschart/120-chart-colored_GWLSW.pdf?up=1466611200
/hundredschart/120-chart-partial_JSLWX.pdf?up=1466611200
/hundredschart/120-chart-mostly-filled_FTVCX.pdf?up=1518606865
/hundredschart/120-chart-desktop_WXPPD.pdf?up=1466611200
/hundredschart/120-chart-blank_UUOWX.pdf?up=1466611200
/hundredschart/120-rounding-chart-arrows_WKSLD.pdf?up=1466611200
/hundredschart/120-rounding-chart-colors_GWKZO.pdf?up=1466611200
/hundredschart/120-chart-puzzles_TWIXF.pdf?up=1466611200
/hundredschart/120-chart-puzzles1_PEJXP.pdf?up=1466611200
/hundredschart/120-chart-puzzles2_GWJXS.pdf?up=1466611200
/hundredschart/120-chart-puzzles3_HSIJS.pdf?up=1466611200
/hundredschart/120-chart-addition_GSKWS.pdf?up=1466611200
/hundredschart/120-chart-addition2_GHWXO.pdf?up=1466611200
/rounding/rounding-chart-arrows-1000_WPMFT.pdf?up=1466611200
/rounding/rounding-chart-1000_MANTS.pdf?up=1466611200
 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5:
cmd( 1):
processing https://www.downloadtest.com/hundredschart/hundreds-chart-filled_WNRTB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/hundredschart/hundreds-chart-filled_WNRTB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/hundredschart/hundreds-chart-colored_WNRTD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/hundredschart/hundreds-chart-colored_WNRTD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/hundredschart/hundreds-chart-partial_WNRTF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/hundredschart/hundreds-chart-mostly-filled_NCXZR.pdf?up=1518606864
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/hundredschart/hundreds-chart-desktop_WNRTQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/hundredschart/hundreds-chart-blank_HUNDR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/hundredschart/rounding-chart-arrows_WNRTM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/hundredschart/hundreds-rounding-chart-colors_WNRTN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/hundredschart/hundreds-chart-puzzles1_WNRTT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/hundredschart/hundreds-chart-puzzles2_WNRTW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/hundredschart/hundreds-chart-puzzles3_WNRTZ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/hundredschart/100-chart-addition_TWKIZ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/hundredschart/100-chart-addition2_GWJXI.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/hundredschart/99-chart-filled_WNRWQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/hundredschart/99-chart-partial_WNRWN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/hundredschart/99-chart-colored_WNRWM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/hundredschart/99-chart-desktop_WNRWR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/hundredschart/99-rounding-chart-arrows_WNRWT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/hundredschart/99-rounding-chart-colors_WNRWW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/hundredschart/120-chart_GFWKX.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/hundredschart/120-chart_GFWKX.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/hundredschart/120-chart-colored_GWLSW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/hundredschart/120-chart-partial_JSLWX.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/hundredschart/120-chart-mostly-filled_FTVCX.pdf?up=1518606865
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/hundredschart/120-chart-desktop_WXPPD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/hundredschart/120-chart-blank_UUOWX.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/hundredschart/120-rounding-chart-arrows_WKSLD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/hundredschart/120-rounding-chart-colors_GWKZO.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/hundredschart/120-chart-puzzles_TWIXF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/hundredschart/120-chart-puzzles1_PEJXP.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/hundredschart/120-chart-puzzles2_GWJXS.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/hundredschart/120-chart-puzzles3_HSIJS.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/hundredschart/120-chart-addition_GSKWS.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/hundredschart/120-chart-addition2_GHWXO.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/rounding/rounding-chart-arrows-1000_WPMFT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/rounding/rounding-chart-1000_MANTS.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/measurement.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/measurement.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/measurement.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):

 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
---Step5:
cmd( 1):
processing https://www.downloadtest.com
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/money.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/money.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/money.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):

 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
---Step5:
cmd( 1):
processing https://www.downloadtest.com
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/multiplication-basic-index.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/multiplication-basic-index.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/multiplication-basic-index.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):

 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
---Step5:
cmd( 1):
processing https://www.downloadtest.com
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/multiplication2.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/multiplication2.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/multiplication2.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):

 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
---Step5:
cmd( 1):
processing https://www.downloadtest.com
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/ordered-pairs.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/ordered-pairs.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/ordered-pairs.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):
/geometry/coordinate-grid2_TZDWB.pdf?up=1466611200
/geometry/coordinate-grid2_TZDWB.pdf?up=1466611200
/geometry/coordinate-grid_TZDWD.pdf?up=1466611200
/geometry/coordinate-grid_TZDWD.pdf?up=1466611200
/geometry/ordered-pairs-treasure-map-basic_RDYUI.pdf?up=1510314099
/geometry/coordinate-grid-polygon_GSJAK.pdf?up=1514544347
/geometry/ordered-pairs-chess_FSJIS.pdf?up=1509119217
/geometry/coordinate-grid-pictures_TZDWF.pdf?up=1529593888
/mystery-graph-picture/graph-owl_TZDWQ.pdf?up=1480675370
/geometry/ordered-pairs-treasure-map-int_HWEJU.pdf?up=1510314413
/geometry/coordinate-grid-polygon-int_HJSIK.pdf?up=1514995768
/geometry/coordinate-grid-2_PWHKX.pdf?up=1510641146
/geometry/coordinate-grid-2_PWHKX.pdf?up=1510641146
/geometry/coordinate-grid3_TZDWM.pdf?up=1510556903
/geometry/ordered-pairs-treasure-map-adv_GFDJX.pdf?up=1510313713
/geometry/coordinate-grid-polygon-adv_RSUAK.pdf?up=1515391851
 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5:
cmd( 1):
processing https://www.downloadtest.com/geometry/coordinate-grid2_TZDWB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/geometry/coordinate-grid2_TZDWB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/geometry/coordinate-grid_TZDWD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/geometry/coordinate-grid_TZDWD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/geometry/ordered-pairs-treasure-map-basic_RDYUI.pdf?up=1510314099
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/geometry/coordinate-grid-polygon_GSJAK.pdf?up=1514544347
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/geometry/ordered-pairs-chess_FSJIS.pdf?up=1509119217
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/geometry/coordinate-grid-pictures_TZDWF.pdf?up=1529593888
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/mystery-graph-picture/graph-owl_TZDWQ.pdf?up=1480675370
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/geometry/ordered-pairs-treasure-map-int_HWEJU.pdf?up=1510314413
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/geometry/coordinate-grid-polygon-int_HJSIK.pdf?up=1514995768
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/geometry/coordinate-grid-2_PWHKX.pdf?up=1510641146
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/geometry/coordinate-grid-2_PWHKX.pdf?up=1510641146
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/geometry/coordinate-grid3_TZDWM.pdf?up=1510556903
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/geometry/ordered-pairs-treasure-map-adv_GFDJX.pdf?up=1510313713
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/geometry/coordinate-grid-polygon-adv_RSUAK.pdf?up=1515391851
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/percents.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/percents.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/percents.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):
/percents/converting-fractions-decimals-percents_EASIE.pdf?up=1466611200
/percents/converting-fractions-decimals-percents_EASIE.pdf?up=1466611200
/percents/basic-percentages1_PERCE.pdf?up=1556868867
/percents/basic-percentages1_PERCE.pdf?up=1556868867
/percents/fraction-decimal-percent-shapes-easy_FDPSE.pdf?up=1466611200
/percents/percents-cut-outs_YWIZK.pdf?up=1466611200
/percents/converting-fractions-decimals-percents-harder_ADVAN.pdf?up=1548229098
/percents/percentages1_PERCE.pdf?up=1466611200
/percents/fraction-decimal-percent-pie-graphs_FDPPG.pdf?up=1501564973
/percents/fraction-decimal-percent-shapes-hard_FDPSH.pdf?up=1466611200
/percents/percent-word-problems-percents_WORDP.pdf?up=1466611200
/percents/fractions-decimals-percents-matching_FDPMG.pdf?up=1500017536
/percents/double-number-lines-1_JKGRW.pdf?up=1500019930
/percents/double-number-lines-2_PLSDR.pdf?up=1500022326
/space/weight-on-planets_WMTZF.pdf?up=1466611200
/graphing/pie-graph-hard-3_TWNZB.pdf?up=1466611200
/graphing/pie-graph-hard-3_TWNZB.pdf?up=1466611200
/graphing/pie-graph-hard-1_TWNZD.pdf?up=1466611200
/graphing/pie-graph-hard-2_TWNZF.pdf?up=1466611200
 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5:
cmd( 1):
processing https://www.downloadtest.com/percents/converting-fractions-decimals-percents_EASIE.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/percents/converting-fractions-decimals-percents_EASIE.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/percents/basic-percentages1_PERCE.pdf?up=1556868867
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/percents/basic-percentages1_PERCE.pdf?up=1556868867
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/percents/fraction-decimal-percent-shapes-easy_FDPSE.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/percents/percents-cut-outs_YWIZK.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/percents/converting-fractions-decimals-percents-harder_ADVAN.pdf?up=1548229098
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/percents/percentages1_PERCE.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/percents/fraction-decimal-percent-pie-graphs_FDPPG.pdf?up=1501564973
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/percents/fraction-decimal-percent-shapes-hard_FDPSH.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/percents/percent-word-problems-percents_WORDP.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/percents/fractions-decimals-percents-matching_FDPMG.pdf?up=1500017536
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/percents/double-number-lines-1_JKGRW.pdf?up=1500019930
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/percents/double-number-lines-2_PLSDR.pdf?up=1500022326
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/space/weight-on-planets_WMTZF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/graphing/pie-graph-hard-3_TWNZB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/graphing/pie-graph-hard-3_TWNZB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/graphing/pie-graph-hard-1_TWNZD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/graphing/pie-graph-hard-2_TWNZF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/perimeter.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/perimeter.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/perimeter.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):
/geometry/perimeter-1_TZFFB.pdf?up=1466611200
/geometry/perimeter-1_TZFFB.pdf?up=1466611200
/geometry/perimeter-3_TZFFD.pdf?up=1466611200
/geometry/perimeter-3_TZFFD.pdf?up=1466611200
/geometry/farm-perimeter_TZFFF.pdf?up=1466611200
/perimeter/perimeter-i-have-who-has_GANHU.pdf?up=1466611200
/perimeter/scoot-perimeter_ZNHAU.pdf?up=1494224369
/geometry/perimeter_TZFFQ.pdf?up=1466611200
/geometry/perimeter-2_TZFFN.pdf?up=1466611200
/geometry/perimeter-shapes_TZFFR.pdf?up=1466611200
/geometry/perimeter-house_HOUSE.pdf?up=1518609697
/geometry/perimeter2_TZFFT.pdf?up=1466611200
/geometry/perimeter-squares_TZFFW.pdf?up=1466611200
/geometry/perimeter-4_TZFQF.pdf?up=1466611200
/geometry/perimeter-4_TZFQF.pdf?up=1466611200
/geometry/perimeter-5_PERIM.pdf?up=1585734483
/geometry/perimeter-6_SYMME.pdf?up=1466611200
/geometry/area-perimeter-1_TZFQN.pdf?up=1466611200
/geometry/area-perimeter-rectangles_TZFQR.pdf?up=1562927629
/geometry/area-perimeter_TWBQQ.pdf?up=1466611200
 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5:
cmd( 1):
processing https://www.downloadtest.com/geometry/perimeter-1_TZFFB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/geometry/perimeter-1_TZFFB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/geometry/perimeter-3_TZFFD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/geometry/perimeter-3_TZFFD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/geometry/farm-perimeter_TZFFF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/perimeter/perimeter-i-have-who-has_GANHU.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/perimeter/scoot-perimeter_ZNHAU.pdf?up=1494224369
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/geometry/perimeter_TZFFQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/geometry/perimeter-2_TZFFN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/geometry/perimeter-shapes_TZFFR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/geometry/perimeter-house_HOUSE.pdf?up=1518609697
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/geometry/perimeter2_TZFFT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/geometry/perimeter-squares_TZFFW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/geometry/perimeter-4_TZFQF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/geometry/perimeter-4_TZFQF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/geometry/perimeter-5_PERIM.pdf?up=1585734483
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/geometry/perimeter-6_SYMME.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/geometry/area-perimeter-1_TZFQN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/geometry/area-perimeter-rectangles_TZFQR.pdf?up=1562927629
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/geometry/area-perimeter_TWBQQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/place-value.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/place-value.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/place-value.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):

 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
---Step5:
cmd( 1):
processing https://www.downloadtest.com
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/probability.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/probability.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/probability.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):
/probability/probability3_TZQMW.pdf?up=1466611200
/probability/probability3_TZQMW.pdf?up=1466611200
/probability/probability10_TZQMZ.pdf?up=1466611200
/probability/probability10_TZQMZ.pdf?up=1466611200
/probability/probability12_TWELV.pdf?up=1466611200
/probability/probability4_TZQNB.pdf?up=1466611200
/probability/probability5_TZQND.pdf?up=1466611200
/probability/probability6-letter-tiles_TZQNF.pdf?up=1466611200
/probability/probability9-prizewheel_TZQNQ.pdf?up=1466611200
/probability/probability11_TZQNN.pdf?up=1466611200
/probability/probability_TZQNR.pdf?up=1466611200
/probability/probability8-experiment-suits_TZQRF.pdf?up=1466611200
/probability/probability8-experiment-suits_TZQRF.pdf?up=1466611200
/probability/probability2_TZQRQ.pdf?up=1466611200
/probability/probability7-experiment-colors_TZQRM.pdf?up=1466611200
/probability/probability-spinners_TZQRN.pdf?up=1466611200
 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5:
cmd( 1):
processing https://www.downloadtest.com/probability/probability3_TZQMW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/probability/probability3_TZQMW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/probability/probability10_TZQMZ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/probability/probability10_TZQMZ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/probability/probability12_TWELV.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/probability/probability4_TZQNB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/probability/probability5_TZQND.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/probability/probability6-letter-tiles_TZQNF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/probability/probability9-prizewheel_TZQNQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/probability/probability11_TZQNN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/probability/probability_TZQNR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/probability/probability8-experiment-suits_TZQRF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/probability/probability8-experiment-suits_TZQRF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/probability/probability2_TZQRQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/probability/probability7-experiment-colors_TZQRM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/probability/probability-spinners_TZQRN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/rounding.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/rounding.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/rounding.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):

 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
---Step5:
cmd( 1):
processing https://www.downloadtest.com
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/full-skip-counting.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/full-skip-counting.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/full-skip-counting.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):

 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
---Step5:
cmd( 1):
processing https://www.downloadtest.com
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/subtraction.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/subtraction.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/subtraction.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):

 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
---Step5:
cmd( 1):
processing https://www.downloadtest.com
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/time.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/time.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/time.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):

 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
---Step5:
cmd( 1):
processing https://www.downloadtest.com
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/volume.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/volume.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/volume.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):

 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
---Step5:
cmd( 1):
processing https://www.downloadtest.com
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/word-problems-multi-step.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/word-problems-multi-step.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/word-problems-multi-step.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):
/multiple-step-problems/multiple-step-basic1_BASIC.pdf?up=1466611200
/multiple-step-problems/multiple-step-basic1_BASIC.pdf?up=1466611200
/multiple-step-problems/multiple-step-basic2_MULTI.pdf?up=1466611200
/multiple-step-problems/multiple-step-basic2_MULTI.pdf?up=1466611200
/multiple-step-problems/multiple-step-basic3_BOOKS.pdf?up=1484215445
/multiple-step-problems/multiple-step-basic4_TOMAT.pdf?up=1466611200
/multiple-step-problems/multiple-step1_MULTI.pdf?up=1466611200
/multiple-step-problems/multiple-step4-adv_FARMS.pdf?up=1466611200
/multiple-step-problems/multiple-step5_BANKS.pdf?up=1466611200
/multiple-step-problems/multiple-step6_ANIMA.pdf?up=1466611200
/multiple-step-problems/redwood-amusement-park_WORDP.pdf?up=1501563856
/multiple-step-problems/multi-step-algebraic_MOVIE.pdf?up=1472202736
/subtraction/change-subtraction-toyshop-harder_TZRWN.pdf?up=1557400162
/multiple-step-problems/multiple-step2_STEPS.pdf?up=1466611200
/multiple-step-problems/multiple-step2_STEPS.pdf?up=1466611200
/multiple-step-problems/multiple-step3-adv_LAWNS.pdf?up=1466611200
/multiple-step-problems/multiple-step-4th-5th-grade_COOKI.pdf?up=1466611200
/multiple-step-problems/multiple-step-4th-5th-grade-2_KAYAK.pdf?up=1466611200
 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5:
cmd( 1):
processing https://www.downloadtest.com/multiple-step-problems/multiple-step-basic1_BASIC.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/multiple-step-problems/multiple-step-basic1_BASIC.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/multiple-step-problems/multiple-step-basic2_MULTI.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/multiple-step-problems/multiple-step-basic2_MULTI.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/multiple-step-problems/multiple-step-basic3_BOOKS.pdf?up=1484215445
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/multiple-step-problems/multiple-step-basic4_TOMAT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/multiple-step-problems/multiple-step1_MULTI.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/multiple-step-problems/multiple-step4-adv_FARMS.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/multiple-step-problems/multiple-step5_BANKS.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/multiple-step-problems/multiple-step6_ANIMA.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/multiple-step-problems/redwood-amusement-park_WORDP.pdf?up=1501563856
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/multiple-step-problems/multi-step-algebraic_MOVIE.pdf?up=1472202736
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/subtraction/change-subtraction-toyshop-harder_TZRWN.pdf?up=1557400162
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/multiple-step-problems/multiple-step2_STEPS.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/multiple-step-problems/multiple-step2_STEPS.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/multiple-step-problems/multiple-step3-adv_LAWNS.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/multiple-step-problems/multiple-step-4th-5th-grade_COOKI.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/multiple-step-problems/multiple-step-4th-5th-grade-2_KAYAK.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/full-math.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/full-math.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/full-math.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):

 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
---Step5:
cmd( 1):
processing https://www.downloadtest.com
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/full-math.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/full-math.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/full-math.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):

 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
---Step5:
cmd( 1):
processing https://www.downloadtest.com
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/1st-comprehension.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/1st-comprehension.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/1st-comprehension.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):
/reading-comp/1st-rhyming-game_RGAME.pdf?up=1466611200
/reading-comp/1st-rhyming-game_RGAME.pdf?up=1466611200
/reading-comp/1st-rainy-day-surprise_RAINS.pdf?up=1466611200
/reading-comp/1st-rainy-day-surprise_RAINS.pdf?up=1466611200
/reading-comp/1st-ball-for-my-dog_TZZMD.pdf?up=1581322403
/reading-comp/1st-apple-poem_TZZMF.pdf?up=1488798275
/reading-comp/1st-a-perfect-picnic_SUNNY.pdf?up=1563523431
/reading-comp/1st-good-ideas_IDEAS.pdf?up=1568361810
/reading-comp/1st-mias-ghost_GHOST.pdf?up=1572277579
/reading-comp/1st-musical-picnic_MUSIC.pdf?up=1466611200
/reading-comp/1st-i-can-do-it-myself_CANDO.pdf?up=1466611200
/reading-comp/1st-not-now-pep_NNPGB.pdf?up=1505923297
/reading-comp/1st-lilys-happy-bedtime_STORY.pdf?up=1507653847
/reading-comp/1st-lily-and-the-missing-keys_KEYYS.pdf?up=1525081100
/reading-comp/1st-smelling-game-surprise_SMELL.pdf?up=1512372625
/reading-comp/1st-playing-catch_GAMES.pdf?up=1527663368
/reading-comp/1st-hiccup-help_775F8.pdf?up=1513930472
/reading-comp/1st-the-other-half_GAMES.pdf?up=1555067076
/reading-comp/1st-snow-fun_SNOWY.pdf?up=1542366460
/reading-comp/1st-where-is-tiger_KITTY.pdf?up=1546946189
/reading-comp/1st-sophie-shares_APPLE.pdf?up=1538731402
/reading-comp/1st-feeling-fun_PENNY.pdf?up=1515570404
/reading-comp/1st-max-the-dog_TZZMQ.pdf?up=1486465981
/reading-comp/1st-aunt-lees-pets_TZZMM.pdf?up=1581322404
/reading-comp/1st-bubble-baby_TZZMN.pdf?up=1490351375
/reading-comp/1st-baby-brother_TZZMR.pdf?up=1488799060
/reading-comp/1st-hippo-and-frog_TZZMT.pdf?up=1585897268
/reading-comp/1st-friends-for-tom_TZZMW.pdf?up=1466611200
/reading-comp/1st-sam-the-cat_TZZMZ.pdf?up=1488800833
/reading-comp/1st-earrings_TZZNB.pdf?up=1528798783
/reading-comp/1st-messy-room_TZZND.pdf?up=1581601833
/reading-comp/1st-the-missing-ice-cream-mystery_TREAT.pdf?up=1530603135
/reading-comp/1st-frog-on-a-log_TZZNF.pdf?up=1528798840
/reading-comp/1st-bumblebee_TZZNQ.pdf?up=1542795366
/reading-comp/1st-cat-and-mouse_TZZNM.pdf?up=1581601832
/reading-comp/1st-big-mouth-baby_TZZNN.pdf?up=1586850874
/reading-comp/1st-rosies-day-park_PARKS.pdf?up=1466611200
/reading-comp/1st-rock-band_TZZNR.pdf?up=1550834804
/reading-comp/1st-sir-wags-a-lot_WAGSA.pdf?up=1466611200
/reading-comp/1st-ian-builds-a-snowman_SNOWS.pdf?up=1466611200
/reading-comp/1st-ians-halloween-costume_COSTU.pdf?up=1466611200
/reading-comp/1st-sir-wags-a-lot-night-fright_SCARE.pdf?up=1539684991
/reading-comp/1st-ians-pumpkin_OKJAN.pdf?up=1478066107
/reading-comp/1st-a-job-for-ians-dog_PAINT.pdf?up=1475905496
/reading-comp/1st-sir-wags-a-lot-fishing_FISHI.pdf?up=1466611200
/reading-comp/1st-sir-wags-a-lot-camping_CAMPW.pdf?up=1466611200
/reading-comp/1st-ian-makes-his-dad-a-birthday-cake_MIXED.pdf?up=1488954601
/reading-comp/1st-ian-tries-catching-a-baseball_BASEB.pdf?up=1496904340
/reading-comp/1st-ians-lost-orange_FRUIT.pdf?up=1467046024
/reading-comp/1st-ian-reads-to-his-family_BOOKS.pdf?up=1501579212
/reading-comp/ian-finds-a-tree-for-his-tree-house_GHYGF.pdf?up=1466611200
/reading-comp/1st-ian-fixes-his-tree-house_ROSES.pdf?up=1515491474
/reading-comp/1st-ian-goes-to-a-new-school_SCHOO.pdf?up=1466611200
/christmas/cookies-for-santa_COOKL.pdf?up=1466611200
/reading-comp/1st-will-and-bud_TZZNT.pdf?up=1581322409
/reading-comp/1st-will-and-bud_TZZNT.pdf?up=1581322409
/reading-comp/1st-fireflies_TZZNW.pdf?up=1486457047
/reading-comp/1st-zac-and-sam_TZZRD.pdf?up=1581322408
/reading-comp/1st-fishing_TZZNZ.pdf?up=1585897012
/reading-comp/1st-play_TZZRB.pdf?up=1586850874
/reading-comp/1st-mystery-sounds_POPCO.pdf?up=1522065065
/reading-comp/1st-treats-and-a-trick_TREAT.pdf?up=1569319167
/reading-comp/1st-car-trip_TZZRF.pdf?up=1585897013
/reading-comp/1st-dan-does-tricks_TZZRQ.pdf?up=1577956306
/reading-comp/1st-the-missing-book-mystery_BOOKS.pdf?up=1530603094
/reading-comp/1st-missing-pencil_TZZRM.pdf?up=1528882421
/reading-comp/1st-farmers-birthday_TZZRN.pdf?up=1487231304
/phonics/short-a-catpoem_WQDNN.pdf?up=1585634299
/reading-comp/1st-codys-rock_TZZRZ.pdf?up=1486453176
/reading-comp/1st-hunters-card_TZZRT.pdf?up=1528882403
/reading-comp/1st-colors_TZZRW.pdf?up=1477558814
 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5:
cmd( 1):
processing https://www.downloadtest.com/reading-comp/1st-rhyming-game_RGAME.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/1st-rhyming-game_RGAME.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/1st-rainy-day-surprise_RAINS.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/1st-rainy-day-surprise_RAINS.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/1st-ball-for-my-dog_TZZMD.pdf?up=1581322403
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/1st-apple-poem_TZZMF.pdf?up=1488798275
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/1st-a-perfect-picnic_SUNNY.pdf?up=1563523431
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/1st-good-ideas_IDEAS.pdf?up=1568361810
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/1st-mias-ghost_GHOST.pdf?up=1572277579
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/1st-musical-picnic_MUSIC.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/1st-i-can-do-it-myself_CANDO.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/1st-not-now-pep_NNPGB.pdf?up=1505923297
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/1st-lilys-happy-bedtime_STORY.pdf?up=1507653847
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/1st-lily-and-the-missing-keys_KEYYS.pdf?up=1525081100
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/1st-smelling-game-surprise_SMELL.pdf?up=1512372625
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/1st-playing-catch_GAMES.pdf?up=1527663368
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/1st-hiccup-help_775F8.pdf?up=1513930472
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/1st-the-other-half_GAMES.pdf?up=1555067076
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/1st-snow-fun_SNOWY.pdf?up=1542366460
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/1st-where-is-tiger_KITTY.pdf?up=1546946189
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/1st-sophie-shares_APPLE.pdf?up=1538731402
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/1st-feeling-fun_PENNY.pdf?up=1515570404
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/1st-max-the-dog_TZZMQ.pdf?up=1486465981
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/1st-aunt-lees-pets_TZZMM.pdf?up=1581322404
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/1st-bubble-baby_TZZMN.pdf?up=1490351375
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/1st-baby-brother_TZZMR.pdf?up=1488799060
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/1st-hippo-and-frog_TZZMT.pdf?up=1585897268
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/1st-friends-for-tom_TZZMW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/1st-sam-the-cat_TZZMZ.pdf?up=1488800833
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/1st-earrings_TZZNB.pdf?up=1528798783
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/1st-messy-room_TZZND.pdf?up=1581601833
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/1st-the-missing-ice-cream-mystery_TREAT.pdf?up=1530603135
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/1st-frog-on-a-log_TZZNF.pdf?up=1528798840
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/1st-bumblebee_TZZNQ.pdf?up=1542795366
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/1st-cat-and-mouse_TZZNM.pdf?up=1581601832
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/1st-big-mouth-baby_TZZNN.pdf?up=1586850874
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/1st-rosies-day-park_PARKS.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/1st-rock-band_TZZNR.pdf?up=1550834804
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/1st-sir-wags-a-lot_WAGSA.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/1st-ian-builds-a-snowman_SNOWS.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/1st-ians-halloween-costume_COSTU.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/1st-sir-wags-a-lot-night-fright_SCARE.pdf?up=1539684991
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/1st-ians-pumpkin_OKJAN.pdf?up=1478066107
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/1st-a-job-for-ians-dog_PAINT.pdf?up=1475905496
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/1st-sir-wags-a-lot-fishing_FISHI.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/1st-sir-wags-a-lot-camping_CAMPW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/1st-ian-makes-his-dad-a-birthday-cake_MIXED.pdf?up=1488954601
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/1st-ian-tries-catching-a-baseball_BASEB.pdf?up=1496904340
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/1st-ians-lost-orange_FRUIT.pdf?up=1467046024
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/1st-ian-reads-to-his-family_BOOKS.pdf?up=1501579212
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/ian-finds-a-tree-for-his-tree-house_GHYGF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/1st-ian-fixes-his-tree-house_ROSES.pdf?up=1515491474
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/1st-ian-goes-to-a-new-school_SCHOO.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/christmas/cookies-for-santa_COOKL.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/1st-will-and-bud_TZZNT.pdf?up=1581322409
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/1st-will-and-bud_TZZNT.pdf?up=1581322409
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/1st-fireflies_TZZNW.pdf?up=1486457047
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/1st-zac-and-sam_TZZRD.pdf?up=1581322408
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/1st-fishing_TZZNZ.pdf?up=1585897012
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/1st-play_TZZRB.pdf?up=1586850874
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/1st-mystery-sounds_POPCO.pdf?up=1522065065
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/1st-treats-and-a-trick_TREAT.pdf?up=1569319167
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/1st-car-trip_TZZRF.pdf?up=1585897013
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/1st-dan-does-tricks_TZZRQ.pdf?up=1577956306
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/1st-the-missing-book-mystery_BOOKS.pdf?up=1530603094
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/1st-missing-pencil_TZZRM.pdf?up=1528882421
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/1st-farmers-birthday_TZZRN.pdf?up=1487231304
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/phonics/short-a-catpoem_WQDNN.pdf?up=1585634299
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/1st-codys-rock_TZZRZ.pdf?up=1486453176
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/1st-hunters-card_TZZRT.pdf?up=1528882403
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/1st-colors_TZZRW.pdf?up=1477558814
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/2nd-comprehension.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/2nd-comprehension.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/2nd-comprehension.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):
/reading-comp/2nd-planetrip_TZZTZ.pdf?up=1466611200
/reading-comp/2nd-planetrip_TZZTZ.pdf?up=1466611200
/reading-comp/2nd-another-view_CHLOE.pdf?up=1481892034
/reading-comp/2nd-another-view_CHLOE.pdf?up=1481892034
/reading-comp/2nd-noisy-silence_NOISY.pdf?up=1466611200
/reading-comp/2nd-superjoey_TZZTW.pdf?up=1466611200
/reading-comp/2nd-emmas-new-lunch-box_LUNCH.pdf?up=1466611200
/reading-comp/2nd-chicken-and-the-mighty-storm_STORM.pdf?up=1466611200
/reading-comp/2nd-rory-wait_BOOKS.pdf?up=1568363244
/reading-comp/2nd-three-snowmen_THREE.pdf?up=1576049214
/reading-comp/2nd-cock-a-doodle-snooze_ALARM.pdf?up=1486106165
/reading-comp/2nd-grandpa-missing-sock_SOCKS.pdf?up=1466611200
/reading-comp/2nd-pictures-in-the-clouds_CLOUD.pdf?up=1504689409
/reading-comp/2nd-bedtime-story_BEDTI.pdf?up=1466611200
/reading-comp/2nd-reading-adventure_BOOKS.pdf?up=1559719240
/reading-comp/2nd-hayleys-new-coat_HAYLE.pdf?up=1510037187
/reading-comp/2nd-snow-flowers_SNOWF.pdf?up=1549609297
/reading-comp/2nd-sammys-day-in-the-cayman-islands_ISLAN.pdf?up=1528700389
/reading-comp/1st-sorry-sheep_BAAAA.pdf?up=1548664369
/reading-comp/2nd-spider-web_TZZWB.pdf?up=1466611200
/reading-comp/2nd-blessie-bakes_TZZWD.pdf?up=1466611200
/reading-comp/2nd-thebat_TZZWF.pdf?up=1466611200
/reading-comp/2nd-hunt-for-gold_GHUNT.pdf?up=1466611200
/reading-comp/2nd-sock-puppet_TZZWQ.pdf?up=1466611200
/reading-comp/2nd-taylors-prize_CORND.pdf?up=1466611200
/reading-comp/2nd-daves-beans_TZZWM.pdf?up=1466611200
/reading-comp/2nd-aunt-lees-eggs_EGGGG.pdf?up=1466611200
/reading-comp/2nd-aunt-lees-pets_TZZWN.pdf?up=1466611200
/reading-comp/2nd-101pets-aunt-lees_TZZWR.pdf?up=1466611200
/reading-comp/2nd-treasure_TZZWT.pdf?up=1466611200
/reading-comp/2nd-mia-max-and-the-missing-earring_BOOTS.pdf?up=1494953130
/reading-comp/2nd-backyard-picnic_BACKY.pdf?up=1489313008
/reading-comp/2nd-emilys-harvest_TZZWW.pdf?up=1466611200
/reading-comp/2nd-dinner_TZZWZ.pdf?up=1466611200
/reading-comp/2nd-earningmoney-2_TZZZB.pdf?up=1466611200
/reading-comp/2nd-first-place-katy_TZZZD.pdf?up=1466611200
/reading-comp/2nd-thetwins_TZZZF.pdf?up=1466611200
/reading-comp/2nd-wheres-alex_TZZZQ.pdf?up=1466611200
/reading-comp/2nd-robotfun_TZZZN.pdf?up=1466611200
/reading-comp/2nd-i-am-who-i-am_TZZZR.pdf?up=1466611200
/reading-comp/2nd-rock-collecting_TZZZT.pdf?up=1489313009
/reading-comp/2nd-lunchbox_TZZZW.pdf?up=1466611200
/reading-comp/2nd-makingcookies_WBBBB.pdf?up=1549273849
/reading-comp/2nd-fifteen-minutes-pie_TIMES.pdf?up=1506941827
/reading-comp/petstore-2_WBBBD.pdf?up=1466611200
/reading-comp/2nd-homework_WBBBF.pdf?up=1466611200
/reading-comp/2nd-case-of-the-missing-fish_MFISH.pdf?up=1466611200
/reading-comp/jens-birthday-2_WBBBQ.pdf?up=1466611200
/reading-comp/2nd-adds-up_WBBBM.pdf?up=1466611200
/reading-comp/2nd-the-holiday-surprise_HOLID.pdf?up=1466611200
/reading-comp/2nd-playing-paper_PLAYI.pdf?up=1522835082
/reading-comp/2nd-kitten-play_KITTE.pdf?up=1466611200
/reading-comp/2nd-where-is-kitty_TZZZZ.pdf?up=1506941842
/reading-comp/2nd-annas-kite_KITES.pdf?up=1466611200
/reading-comp/2nd-annas-beach-bag_ANNBB.pdf?up=1466611200
/reading-comp/2nd-kitty-and-the-4th-of-july-parade_FOURT.pdf?up=1472194194
/reading-comp/1st-annas-olympic-yo-yo_OLYMP.pdf?up=1471843927
/reading-comp/2nd-anna-and-the-mummy_LPOKJ.pdf?up=1466611200
/reading-comp/2nd-anna-and-the-attic_AATAT.pdf?up=1490783390
/reading-comp/2nd-anna-and-the-missing-puzzle-piece_ANNAA.pdf?up=1500374155
/reading-comp/2nd-doll-house_DHOUS.pdf?up=1466611200
/reading-comp/2nd-annas-class-pet_ANNAS.pdf?up=1466611200
/reading-comp/2nd-anna-the-santa-trap_TRAPS.pdf?up=1466611200
/readerstheater/2nd-maisyspiggybank_WBBBR.pdf?up=1549284623
/readerstheater/2nd-maisyspiggybank_WBBBR.pdf?up=1549284623
/reading-comp/2nd-junk-art_WBBBW.pdf?up=1466611200
/reading-comp/2nd-snow-school_WBBBZ.pdf?up=1466611200
/reading-comp/2nd-summer_WBBDB.pdf?up=1477464207
/poems/2nd-nature-treasure-hunt_CREEK.pdf?up=1568714713
/poems/2nd-whale-watch-wonder_WHALE.pdf?up=1559924790
/reading-comp/2nd-first-day_WMBQZ.pdf?up=1466611200
/reading-comp/2nd-problem-solved_WBBDF.pdf?up=1466611200
/reading-comp/2nd-snakes_WBBDQ.pdf?up=1466611200
/reading-comp/2nd-animal-voices-poem_CROAK.pdf?up=1466611200
/reading-comp/2nd-reading_WBBDM.pdf?up=1466611200
/dinosaurs/2nd-t-rex_TYRAN.pdf?up=1466611200
/reading-comp/2nd-sleepybear_HYBER.pdf?up=1466611200
/reading-comp/2nd-champs-wet-nose_NOSEY.pdf?up=1466611200
/reading-comp/2nd-elephants_WBBDR.pdf?up=1466611200
/reading-comp/2nd-tulips_WBBDT.pdf?up=1466611200
/reading-comp/2nd-skunk-alert_SMELL.pdf?up=1466611200
/reading-comp/2nd-eggs_WBBDZ.pdf?up=1466611200
/reading-comp/comprehension5-orca_WBBFB.pdf?up=1466611200
/reading-comp/comprehension2-cheetah_WBBFD.pdf?up=1466611200
/reading-comp/2nd-bananas-pineapples_WBBFQ.pdf?up=1466611200
/reading-comp/2nd-maryharry-compare_WBBFN.pdf?up=1466611200
/reading-comp/2nd-beach-mtns_WBBFM.pdf?up=1466611200
/reading/main-idea_WBBFT.pdf?up=1466611200
 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5:
cmd( 1):
processing https://www.downloadtest.com/reading-comp/2nd-planetrip_TZZTZ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/2nd-planetrip_TZZTZ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/2nd-another-view_CHLOE.pdf?up=1481892034
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/2nd-another-view_CHLOE.pdf?up=1481892034
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/2nd-noisy-silence_NOISY.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/2nd-superjoey_TZZTW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/2nd-emmas-new-lunch-box_LUNCH.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/2nd-chicken-and-the-mighty-storm_STORM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/2nd-rory-wait_BOOKS.pdf?up=1568363244
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/2nd-three-snowmen_THREE.pdf?up=1576049214
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/2nd-cock-a-doodle-snooze_ALARM.pdf?up=1486106165
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/2nd-grandpa-missing-sock_SOCKS.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/2nd-pictures-in-the-clouds_CLOUD.pdf?up=1504689409
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/2nd-bedtime-story_BEDTI.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/2nd-reading-adventure_BOOKS.pdf?up=1559719240
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/2nd-hayleys-new-coat_HAYLE.pdf?up=1510037187
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/2nd-snow-flowers_SNOWF.pdf?up=1549609297
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/2nd-sammys-day-in-the-cayman-islands_ISLAN.pdf?up=1528700389
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/1st-sorry-sheep_BAAAA.pdf?up=1548664369
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/2nd-spider-web_TZZWB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/2nd-blessie-bakes_TZZWD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/2nd-thebat_TZZWF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/2nd-hunt-for-gold_GHUNT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/2nd-sock-puppet_TZZWQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/2nd-taylors-prize_CORND.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/2nd-daves-beans_TZZWM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/2nd-aunt-lees-eggs_EGGGG.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/2nd-aunt-lees-pets_TZZWN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/2nd-101pets-aunt-lees_TZZWR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/2nd-treasure_TZZWT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/2nd-mia-max-and-the-missing-earring_BOOTS.pdf?up=1494953130
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/2nd-backyard-picnic_BACKY.pdf?up=1489313008
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/2nd-emilys-harvest_TZZWW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/2nd-dinner_TZZWZ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/2nd-earningmoney-2_TZZZB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/2nd-first-place-katy_TZZZD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/2nd-thetwins_TZZZF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/2nd-wheres-alex_TZZZQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/2nd-robotfun_TZZZN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/2nd-i-am-who-i-am_TZZZR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/2nd-rock-collecting_TZZZT.pdf?up=1489313009
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/2nd-lunchbox_TZZZW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/2nd-makingcookies_WBBBB.pdf?up=1549273849
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/2nd-fifteen-minutes-pie_TIMES.pdf?up=1506941827
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/petstore-2_WBBBD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/2nd-homework_WBBBF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/2nd-case-of-the-missing-fish_MFISH.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/jens-birthday-2_WBBBQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/2nd-adds-up_WBBBM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/2nd-the-holiday-surprise_HOLID.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/2nd-playing-paper_PLAYI.pdf?up=1522835082
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/2nd-kitten-play_KITTE.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/2nd-where-is-kitty_TZZZZ.pdf?up=1506941842
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/2nd-annas-kite_KITES.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/2nd-annas-beach-bag_ANNBB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/2nd-kitty-and-the-4th-of-july-parade_FOURT.pdf?up=1472194194
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/1st-annas-olympic-yo-yo_OLYMP.pdf?up=1471843927
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/2nd-anna-and-the-mummy_LPOKJ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/2nd-anna-and-the-attic_AATAT.pdf?up=1490783390
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/2nd-anna-and-the-missing-puzzle-piece_ANNAA.pdf?up=1500374155
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/2nd-doll-house_DHOUS.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/2nd-annas-class-pet_ANNAS.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/2nd-anna-the-santa-trap_TRAPS.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/readerstheater/2nd-maisyspiggybank_WBBBR.pdf?up=1549284623
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/readerstheater/2nd-maisyspiggybank_WBBBR.pdf?up=1549284623
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/2nd-junk-art_WBBBW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/2nd-snow-school_WBBBZ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/2nd-summer_WBBDB.pdf?up=1477464207
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/poems/2nd-nature-treasure-hunt_CREEK.pdf?up=1568714713
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/poems/2nd-whale-watch-wonder_WHALE.pdf?up=1559924790
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/2nd-first-day_WMBQZ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/2nd-problem-solved_WBBDF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/2nd-snakes_WBBDQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/2nd-animal-voices-poem_CROAK.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/2nd-reading_WBBDM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/dinosaurs/2nd-t-rex_TYRAN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/2nd-sleepybear_HYBER.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/2nd-champs-wet-nose_NOSEY.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/2nd-elephants_WBBDR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/2nd-tulips_WBBDT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/2nd-skunk-alert_SMELL.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/2nd-eggs_WBBDZ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/comprehension5-orca_WBBFB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/comprehension2-cheetah_WBBFD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/2nd-bananas-pineapples_WBBFQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/2nd-maryharry-compare_WBBFN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/2nd-beach-mtns_WBBFM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading/main-idea_WBBFT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/3rd-comprehension.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/3rd-comprehension.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/3rd-comprehension.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):
/reading-comp/3rd-closet-creature_WBBQW.pdf?up=1466611200
/reading-comp/3rd-closet-creature_WBBQW.pdf?up=1466611200
/reading-comp/3rd-time-capsule_WBBQZ.pdf?up=1466611200
/reading-comp/3rd-time-capsule_WBBQZ.pdf?up=1466611200
/reading-comp/3rd-bens-loose-tooth_WBBMD.pdf?up=1466611200
/reading-comp/3rd-strangestar_WBBMF.pdf?up=1466611200
/reading-comp/3rd-haunted-tree_WBBMQ.pdf?up=1466611200
/reading-comp/3rd-brotherly-love_BRLOV.pdf?up=1466611200
/reading-comp/3rd-custodian-surprise_WBBMM.pdf?up=1466611200
/reading-comp/3rd-friends-helping-friends_HELPI.pdf?up=1466611200
/reading-comp/3rd-jezzabelles-old-school_HANNA.pdf?up=1486106150
/reading-comp/3rd-glasses_WBBMN.pdf?up=1466611200
/reading-comp/3rd-birthday-treat_WBBMR.pdf?up=1466666491
/reading-comp/3rd-snow-dance_WBBMW.pdf?up=1466611200
/reading-comp/3rd-shark-teeth_WBBMZ.pdf?up=1466611200
/reading-comp/3rd-how-to-pet-a-dolphin_FLIPP.pdf?up=1466611200
/reading-comp/3rd-a-walk-through-the-rainforest_FORES.pdf?up=1503896564
/reading-comp/3rd-the-perfect-camping-trip_TTRDE.pdf?up=1489662192
/reading-comp/3rd-leaf-races_RACES.pdf?up=1466611200
/reading-comp/3rd-math-problems_WBBNB.pdf?up=1466611200
/reading-comp/3rd-shh-it-is-a-secret_SHHHS.pdf?up=1468405472
/reading-comp/3rd-backyard-visitor_C.pdf?up=1466611200
/reading-comp/3rd-friday-13th_BKCAT.pdf?up=1466611200
/reading-comp/3rd-carnival-fever_WBBNQ.pdf?up=1466611200
/reading-comp/3rd-foul-ball_WBBNM.pdf?up=1466611200
/reading-comp/3rd-lunch-surprise_WBBNN.pdf?up=1466611200
/reading-comp/3rd-to-the-rescue_WBBNR.pdf?up=1466611200
/reading-comp/3rd-grandma-to-all_WBBNT.pdf?up=1466611200
/reading-comp/3rd-weathering-the-storm_WBBZW.pdf?up=1466611200
/reading-comp/3rd-someplace-special_WBBNW.pdf?up=1466611200
/reading-comp/3rd-playground_WBBNZ.pdf?up=1466611200
/reading-comp/3rd-super-robotoman_WBBRB.pdf?up=1466611200
/reading-comp/3rd-dragons_WBBRD.pdf?up=1466611200
/reading-comp/3rd-garden-for-one_GFONE.pdf?up=1466611200
/reading-comp/3rd-natures-rainbow_RAINB.pdf?up=1466611200
/reading-comp/3rd-mini-vacation_MINIV.pdf?up=1466611200
/reading-comp/3rd-making-fun_BOUNC.pdf?up=1466611200
/reading-comp/3rd-teacher-trouble_WBBRF.pdf?up=1466611200
/reading-comp/3rd-kids-best-friend_WBBRQ.pdf?up=1466611200
/reading-comp/3rd-pool-party_WBBRM.pdf?up=1466611200
/reading-comp/3rd-friday-night-games_GAMES.pdf?up=1466611200
/reading-comp/3rd-the-luck-of-the-owl_LOWLS.pdf?up=1466611200
/reading-comp/3rd-annas-diary_WBBRN.pdf?up=1466611200
/reading-comp/3rd-video-game-mania_WBBRR.pdf?up=1466611200
/reading-comp/3rd-canada_WBBRT.pdf?up=1466611200
/reading-comp/3rd-interview-grandma_GRAMS.pdf?up=1507047765
/reading-comp/3rd-swing-set-time-machine_WBBRW.pdf?up=1466611200
/reading-comp/3rd-sleepover_WBBRZ.pdf?up=1466611200
/reading-comp/3rd-scarecrow_WMFBM.pdf?up=1569216468
/reading-comp/3rd-friendly-sports_SPORT.pdf?up=1466611200
/reading-comp/3rd-tomorrow_WBBTD.pdf?up=1466611200
/reading-comp/3rd-whos-on-first_BASEB.pdf?up=1466611200
/reading-comp/3rd-spare_CCCCC.pdf?up=1466611200
/reading-comp/3rd-rulers-and-droolers_RULER.pdf?up=1466611200
/reading-comp/3rd-rainy-day-fun_RAINY.pdf?up=1466611200
/reading-comp/3rd-second-best_ACTRE.pdf?up=1466611200
/reading-comp/3rd-new-neighbors_NEIGH.pdf?up=1466611200
/reading-comp/3rd-erins-clover_WBBTF.pdf?up=1466611200
/reading-comp/3rd-friends_across_miles_LONBV.pdf?up=1466611200
/reading-comp/3rd-adventures-of-crazy-cat_CRAZE.pdf?up=1466611200
/reading-comp/3rd-unlucky_LUCKY.pdf?up=1466611200
/reading-comp/3rd-just-be-patient_PATIE.pdf?up=1466611200
/reading-comp/3rd-fossil_WBBTQ.pdf?up=1466611200
/readerstheater/3rd-bear-pet_WBBTN.pdf?up=1466611200
/readerstheater/3rd-bear-pet_WBBTN.pdf?up=1466611200
/reading-comp/3rd-princess-mimi-saves-the-day_ATCHO.pdf?up=1466611200
/reading-comp/3rd-alien-invasion_CHIPS.pdf?up=1532329757
/readerstheater/3rd-pop-poppity-pop_WBBTR.pdf?up=1466611200
/readerstheater/3rd-case-of-the-missing-socks_DRYER.pdf?up=1466611200
/poems/my-phone_PHONE.pdf?up=1466611200
/reading-comp/3rd-raccoon-rex_WBBTW.pdf?up=1466611200
/reading-comp/3rd-dragon_WBBTZ.pdf?up=1466611200
/reading-comp/3rd-creative-cooking_WBBWB.pdf?up=1466611200
/reading-comp/3rd-say-cheese_WBBWD.pdf?up=1466611200
/reading-comp/3rd-sock-balls_WBBWF.pdf?up=1466611200
/reading-comp/3rd-walk-with-my-dog_WBBWQ.pdf?up=1466611200
/reading-comp/3rd-race-poem_WBBWM.pdf?up=1466611200
/reading-comp/3rd-great-catch_WBBWN.pdf?up=1466611200
/reading-comp/3rd-tree-house-tools_WBBWR.pdf?up=1466611200
/reading-comp/3rd-grandmas-garden_WBBWT.pdf?up=1466611200
/poems/3rd-soccer-ball_SOCCE.pdf?up=1466611200
/reading-comp/3rd-fall-fun_WBBWW.pdf?up=1466611200
/poems/3rd-winter-surprise_POEMS.pdf?up=1516799903
/reading-comp/3rd-animals-living-together_WBBWZ.pdf?up=1506942549
/reading-comp/3rd-grandmas-attic_ATTIC.pdf?up=1466611200
/reading-comp/3rd-noflying_WBBZB.pdf?up=1466611200
/reading-comp/3rd-help-the-world-poem_HELPS.pdf?up=1466611200
/reading-comp/3rd-piggy-bank_WBBZF.pdf?up=1466611200
/reading-comp/4th-coral-reefs_CORAL.pdf?up=1466611200
/reading-comp/3rd-all-about-squishy-squid_SQUID.pdf?up=1490247729
/reading-comp/3rd-octopus_WBBZQ.pdf?up=1466611200
/reading-comp/3rd-the-pink-fairy-armadillo_PFARM.pdf?up=1466611200
/reading-comp/3rd-monarch_WDFWT.pdf?up=1493635262
/reading-comp/3rd-animal-madness-with-professor-aligator_ANIMA.pdf?up=1466611200
/reading-comp/3rd-germ-o-rama_ORAMA.pdf?up=1466611200
/reading-comp/3rd-bears_WBBZN.pdf?up=1466611200
/reading-comp/ReadingCompHolidays_WBBZR.pdf?up=1466611200
/reading-comp/3rd-giraffe_WBBZT.pdf?up=1549275619
/reading-comp/3rd-camels_WBBZZ.pdf?up=1466611200
/reading-comp/3rd-service-dogs-rock_HELPS.pdf?up=1550656643
/reading-comp/3rd-solar-energy_WBDBB.pdf?up=1477413517
/reading-comp/3rd-migration_WBDBD.pdf?up=1466611200
/reading-comp/3rd-lego_WBDBF.pdf?up=1466611200
/reading-comp/3rd-hibernation_WBDBQ.pdf?up=1466611200
/reading-comp/3rd-camouflage_WBDBM.pdf?up=1466611200
/reading-comp/3rd-bubbly-adventure_WBDBN.pdf?up=1466611200
/reading-comp/3rd-groundhog_WBDBR.pdf?up=1466611200
/reading-comp/3rd-skunks_WBBMB.pdf?up=1466611200
/reading-comp/3rd-habitats_WBDBT.pdf?up=1466611200
/reading-comp/3rd-roller-coaster-thrills_MOMEN.pdf?up=1466611200
/reading-comp/3rd-navajo-code-talkers_NATIV.pdf?up=1532587572
/reading-comp/3rd-martin-luther-king_WBDDB.pdf?up=1466611200
/reading-comp/3rd-chipmunks_WBDDF.pdf?up=1466611200
/reading-comp/3rd-pencils_WBBNF.pdf?up=1466611200
/reading-comp/comprehension10-whitehouse_WBDDQ.pdf?up=1466611200
/reading-comp/comprehension9-painted_WBDDM.pdf?up=1466611200
/reading-comp/comprehension8-rushmore_WBDDN.pdf?up=1466611200
/reading-comp/comprehension4-pyramids_WBDDR.pdf?up=1466611200
/read-aloud/read-aloud-bens-loose-tooth_READA.pdf?up=1474994253
/read-aloud/read-aloud-my-phone-poem_ALOUD.pdf?up=1474968451
 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5:
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-closet-creature_WBBQW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-closet-creature_WBBQW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-time-capsule_WBBQZ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-time-capsule_WBBQZ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-bens-loose-tooth_WBBMD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-strangestar_WBBMF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-haunted-tree_WBBMQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-brotherly-love_BRLOV.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-custodian-surprise_WBBMM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-friends-helping-friends_HELPI.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-jezzabelles-old-school_HANNA.pdf?up=1486106150
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-glasses_WBBMN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-birthday-treat_WBBMR.pdf?up=1466666491
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-snow-dance_WBBMW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-shark-teeth_WBBMZ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-how-to-pet-a-dolphin_FLIPP.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-a-walk-through-the-rainforest_FORES.pdf?up=1503896564
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-the-perfect-camping-trip_TTRDE.pdf?up=1489662192
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-leaf-races_RACES.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-math-problems_WBBNB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-shh-it-is-a-secret_SHHHS.pdf?up=1468405472
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-backyard-visitor_C.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-friday-13th_BKCAT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-carnival-fever_WBBNQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-foul-ball_WBBNM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-lunch-surprise_WBBNN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-to-the-rescue_WBBNR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-grandma-to-all_WBBNT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-weathering-the-storm_WBBZW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-someplace-special_WBBNW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-playground_WBBNZ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-super-robotoman_WBBRB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-dragons_WBBRD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-garden-for-one_GFONE.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-natures-rainbow_RAINB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-mini-vacation_MINIV.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-making-fun_BOUNC.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-teacher-trouble_WBBRF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-kids-best-friend_WBBRQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-pool-party_WBBRM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-friday-night-games_GAMES.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-the-luck-of-the-owl_LOWLS.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-annas-diary_WBBRN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-video-game-mania_WBBRR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-canada_WBBRT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-interview-grandma_GRAMS.pdf?up=1507047765
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-swing-set-time-machine_WBBRW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-sleepover_WBBRZ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-scarecrow_WMFBM.pdf?up=1569216468
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-friendly-sports_SPORT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-tomorrow_WBBTD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-whos-on-first_BASEB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-spare_CCCCC.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-rulers-and-droolers_RULER.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-rainy-day-fun_RAINY.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-second-best_ACTRE.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-new-neighbors_NEIGH.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-erins-clover_WBBTF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-friends_across_miles_LONBV.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-adventures-of-crazy-cat_CRAZE.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-unlucky_LUCKY.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-just-be-patient_PATIE.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-fossil_WBBTQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/readerstheater/3rd-bear-pet_WBBTN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/readerstheater/3rd-bear-pet_WBBTN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-princess-mimi-saves-the-day_ATCHO.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-alien-invasion_CHIPS.pdf?up=1532329757
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/readerstheater/3rd-pop-poppity-pop_WBBTR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/readerstheater/3rd-case-of-the-missing-socks_DRYER.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/poems/my-phone_PHONE.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-raccoon-rex_WBBTW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-dragon_WBBTZ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-creative-cooking_WBBWB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-say-cheese_WBBWD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-sock-balls_WBBWF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-walk-with-my-dog_WBBWQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-race-poem_WBBWM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-great-catch_WBBWN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-tree-house-tools_WBBWR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-grandmas-garden_WBBWT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/poems/3rd-soccer-ball_SOCCE.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-fall-fun_WBBWW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/poems/3rd-winter-surprise_POEMS.pdf?up=1516799903
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-animals-living-together_WBBWZ.pdf?up=1506942549
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-grandmas-attic_ATTIC.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-noflying_WBBZB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-help-the-world-poem_HELPS.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-piggy-bank_WBBZF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-coral-reefs_CORAL.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-all-about-squishy-squid_SQUID.pdf?up=1490247729
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-octopus_WBBZQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-the-pink-fairy-armadillo_PFARM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-monarch_WDFWT.pdf?up=1493635262
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-animal-madness-with-professor-aligator_ANIMA.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-germ-o-rama_ORAMA.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-bears_WBBZN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/ReadingCompHolidays_WBBZR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-giraffe_WBBZT.pdf?up=1549275619
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-camels_WBBZZ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-service-dogs-rock_HELPS.pdf?up=1550656643
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-solar-energy_WBDBB.pdf?up=1477413517
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-migration_WBDBD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-lego_WBDBF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-hibernation_WBDBQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-camouflage_WBDBM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-bubbly-adventure_WBDBN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-groundhog_WBDBR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-skunks_WBBMB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-habitats_WBDBT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-roller-coaster-thrills_MOMEN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-navajo-code-talkers_NATIV.pdf?up=1532587572
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-martin-luther-king_WBDDB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-chipmunks_WBDDF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-pencils_WBBNF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/comprehension10-whitehouse_WBDDQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/comprehension9-painted_WBDDM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/comprehension8-rushmore_WBDDN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/comprehension4-pyramids_WBDDR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/read-aloud/read-aloud-bens-loose-tooth_READA.pdf?up=1474994253
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/read-aloud/read-aloud-my-phone-poem_ALOUD.pdf?up=1474968451
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/4th-comprehension.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/4th-comprehension.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/4th-comprehension.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):
/reading-comp/4th-class-pictures_WBDQB.pdf?up=1506493967
/reading-comp/4th-class-pictures_WBDQB.pdf?up=1506493967
/reading-comp/4th-skeleton-key_WBDFZ.pdf?up=1466611200
/reading-comp/4th-skeleton-key_WBDFZ.pdf?up=1466611200
/reading-comp/4th-a-monster-out-of-a-molehill_MOLES.pdf?up=1540277858
/reading-comp/4th-snow-elephants_WMMQD.pdf?up=1466611200
/reading-comp/4th-bear-and-hare-warm-up_AFRAI.pdf?up=1486106143
/reading-comp/4th-the-dog-and-his-reflection_RASCA.pdf?up=1540277833
/reading-comp/4th-substitute-meany_MEANY.pdf?up=1466611200
/reading-comp/4th-frankensteinface_WBDQF.pdf?up=1466611200
/reading-comp/4th-road-trip_RTRIP.pdf?up=1467784605
/reading-comp/4th-fortune-teller_FORTU.pdf?up=1468909260
/reading-comp/4th-dinosaur-graveyard_WBDQM.pdf?up=1498817395
/reading-comp/4th-high-flyer_WBDQN.pdf?up=1466611200
/reading-comp/4th-diary-dancer_WBDQR.pdf?up=1466611200
/reading-comp/4th-underwater-adventure_UNDE.pdf?up=1466611200
/reading-comp/4th-readingcomprehension-CarolineCastle_WBDQT.pdf?up=1466611200
/reading-comp/4th-foggy-figure_FOGGY.pdf?up=1466611200
/reading-comp/4th-labyrinth_LABYR.pdf?up=1494574932
/reading-comp/4th-singing-cousin_SINGS.pdf?up=1466611200
/reading-comp/4th-big-city-fun_BIGCI.pdf?up=1466611200
/reading-comp/4th-misplaced_SOCKS.pdf?up=1466611200
/reading-comp/4th-rumors-running-wild_RUMOR.pdf?up=1468910600
/reading-comp/4th-different-cultures_DIFFT.pdf?up=1468567519
/reading-comp/4th-class-leader_LEADE.pdf?up=1471842653
/reading-comp/4th-catch_FISHY.pdf?up=1468566653
/reading-comp/4th-flag-many-faces_WBDMD.pdf?up=1466611200
/reading-comp/4th-a-promise-is-a-promise_SKATE.pdf?up=1466611200
/reading-comp/4th-flower-girl_WBDMF.pdf?up=1466611200
/reading-comp/4th-design-flaws_OIJNM.pdf?up=1466611200
/reading-comp/4th-shadow_SHADO.pdf?up=1466611200
/reading-comp/4th-shooting-for-perfection_SHOOT.pdf?up=1471450810
/reading-comp/4th-acting-the-part_ACTIN.pdf?up=1466611200
/reading-comp/4th-the-champ_SPORT.pdf?up=1466611200
/reading-comp/4th-spider-webs_WEBS.pdf?up=1466611200
/reading-comp/4th-sibling-swap_WBDMM.pdf?up=1466611200
/reading-comp/4th-rhythmless-blues_WBDMN.pdf?up=1489401011
/reading-comp/4th-new-girl_NEWSC.pdf?up=1468405478
/reading-comp/4th-a-snowy-spring_SPRIN.pdf?up=1466611200
/reading-comp/4th-erupting-with-fun_ERUPT.pdf?up=1466611200
/reading-comp/4th-broken-perspective_RAINB.pdf?up=1471843184
/reading-comp/4th-personal-introduction_INTRO.pdf?up=1468222097
/reading-comp/4th-new-puppy_PDAZE.pdf?up=1466611200
/reading-comp/4th-missing-information_INFOR.pdf?up=1466611200
/reading-comp/4th-a-home-for-kitty_KITTY.pdf?up=1466611200
/reading-comp/4th-out-of-time_GAMES.pdf?up=1471451748
/reading-comp/4th-we-are-all-winners_AOKAY.pdf?up=1466611200
/reading-comp/4th-nothing-to-fear_NOTHI.pdf?up=1471883799
/reading-comp/4th-olympic-games_OLYMP.pdf?up=1470933928
/reading-comp/4th-best-birthday-present-ever_HORSE.pdf?up=1466611200
/reading-comp/4th-moon-assignment_MOONS.pdf?up=1466611200
/reading-comp/4th-my-summer-vacation_VACAT.pdf?up=1466611200
/reading-comp/4th-big-cousin-big-responsibility_COUSI.pdf?up=1466611200
/readerstheater/4th-pirates_WBDNB.pdf?up=1466611200
/reading-comp/4th-rabbit-habit-poem_WBDNF.pdf?up=1466611200
/reading-comp/4th-snow-troops_WBDNQ.pdf?up=1466611200
/reading-comp/4th-playing-catch_WBDNM.pdf?up=1466611200
/reading-comp/4th-family-features_WBDNN.pdf?up=1466611200
/reading-comp/4th-butter-churn_WBDNR.pdf?up=1466611200
/reading-comp/4th-spring-surprises-poem_SPRING.pdf?up=1490166690
/reading-comp/4th-night-story_WBDNT.pdf?up=1466611200
/reading-comp/4th-similes_WBDNW.pdf?up=1466611200
/reading-comp/4th-metaphors_WBDNZ.pdf?up=1466611200
/reading-comp/4th-onomatopoeia_WBDRB.pdf?up=1466611200
/reading-comp/4th-sunset_SPOEM.pdf?up=1466611200
/reading-comp/4th-a-dragon-for-hailey_POEMS.pdf?up=1466611200
/reading-comp/4th-desert-flowers_WBDRD.pdf?up=1466611200
/poems/4th-urban-suburban-rural_WHERE.pdf?up=1572339166
/poems/4th-supply-and-demand_MONEY.pdf?up=1569474515
/reading-comp/4th-gingerbreadman_WBDRF.pdf?up=1466611200
/reading-comp/4th-these-trees_WBDRQ.pdf?up=1466611200
/reading-comp/4th-venus-flytraps_WBDRM.pdf?up=1466611200
/reading-comp/4th-frontier-life_WBDRN.pdf?up=1466611200
/reading-comp/4th-pillbugs_WBDRW.pdf?up=1466611200
/reading-comp/4th-pillbugs_WBDRW.pdf?up=1466611200
/reading-comp/4th-whats-the-deal-with-mold_MOLDY.pdf?up=1509708663
/reading-comp/4th-achoo_WBDRT.pdf?up=1473766647
/reading-comp/4th-running_WBDRZ.pdf?up=1466611200
/reading-comp/4th-recycling_IMPOR.pdf?up=1466611200
/reading-comp/4th-whats-the-deal-with-sleep_SLEEP.pdf?up=1550749441
/reading-comp/4th-everest_WBDTB.pdf?up=1466611200
/reading-comp/4th-popcorn-history_WBDTD.pdf?up=1466611200
/reading-comp/4th-dolphins_PORPO.pdf?up=1519642509
/reading-comp/4th-worlds-largest-fish_WHARK.pdf?up=1570170987
/reading-comp/4th-electric-eels_SHOCK.pdf?up=1476436969
/reading-comp/4th-fish-with-a-stinging-tail_STING.pdf?up=1528882354
/reading-comp/4th-constellations_WBDTF.pdf?up=1478782767
/reading-comp/4th-why-leaves-change-color_LEAFY.pdf?up=1499942833
/reading-comp/4th-chew-on-these-bubble-gum-facts_BUBBA.pdf?up=1490592757
/reading-comp/4th-stink-bugs_WBDTQ.pdf?up=1466611200
/reading-comp/4th-desert-cottontails_WBDTM.pdf?up=1466611200
/reading-comp/5th-rabbits-and-hares_BUNNY.pdf?up=1476871490
/reading-comp/4th-flightlessbirds_WBDTN.pdf?up=1466611200
/reading-comp/4th-polar-bears_WBDTR.pdf?up=1466611200
/reading-comp/5th-tiger-talk_TIGER.pdf?up=1470933889
/reading-comp/4th-nocturnal_WBDTT.pdf?up=1466611200
/reading-comp/4th-fennec-fox_WDFMZ.pdf?up=1466611200
/reading-comp/4th-porcupine_WBDTZ.pdf?up=1466611200
/reading-comp/4th-boa-python_WBDWB.pdf?up=1523353801
/reading-comp/4th-alligators-crocodiles_WBDWF.pdf?up=1563430046
/reading-comp/4th-dandelions_WBDWQ.pdf?up=1466611200
/reading-comp/4th-ants_WBDWM.pdf?up=1466611200
/reading-comp/4th-tarantula_WBDWN.pdf?up=1466611200
/reading-comp/4th-mermaids_WBDWR.pdf?up=1466611200
/reading-comp/4th-wetlands_WBDWT.pdf?up=1466611200
/reading-comp/4th-blimp_WBDWW.pdf?up=1466611200
/reading-comp/4th-the-big-ostrich_HATCH.pdf?up=1466611200
/reading-comp/4th_life-american-colonies_AMCOL.pdf?up=1580814208
/reading-comp/4th-grasshoppers_WBDWZ.pdf?up=1466611200
/reading-comp/4th-teddy-bear_WBDZB.pdf?up=1562583631
/reading-comp/4th-frogs-toads_WBDZD.pdf?up=1466611200
/reading-comp/4th-walnuts_WBDZF.pdf?up=1466611200
/reading-comp/4th-chameleon_WDFRN.pdf?up=1496216870
/reading-comp/4th-carnivores-herbivores_WBDZM.pdf?up=1466611200
/reading-comp/4th-a-pride-of-lions_TREES.pdf?up=1466611200
/reading-comp/4th-sharks_AREAW.pdf?up=1466611200
/reading-comp/readComp-Winter_WBDZN.pdf?up=1466611200
/reading-comp/4th-hedgehogs_WBDZR.pdf?up=1466611200
/reading-comp/4th-alligator-snapping-turtles_SNAPT.pdf?up=1550657117
/reading-comp/4th-iguanas_WBDZT.pdf?up=1466611200
/reading-comp/4th-disney_WBDZZ.pdf?up=1582716341
/reading-comp/4th-lincoln_WBFBB.pdf?up=1466611200
/reading-comp/4th-washington_WBFBD.pdf?up=1466611200
/reading-comp/4th-drseuss_WBFBF.pdf?up=1582707434
/reading-comp/rosaparks-4_WBFBQ.pdf?up=1581601831
/reading-comp/4th-toothache_WBFBN.pdf?up=1466611200
/reading-comp/4th-heels_WBFBR.pdf?up=1466611200
/reading-comp/comprehension7-mammothcave_WBFBT.pdf?up=1466611200
/reading-comp/comprehenson1-amazon_WBFBW.pdf?up=1466611200
/reading-comp/4th-haiku-poems_WBFBZ.pdf?up=1466611200
/reading-comp/4th-gertrude-ederle_WBFDB.pdf?up=1466611200
 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5:
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-class-pictures_WBDQB.pdf?up=1506493967
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-class-pictures_WBDQB.pdf?up=1506493967
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-skeleton-key_WBDFZ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-skeleton-key_WBDFZ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-a-monster-out-of-a-molehill_MOLES.pdf?up=1540277858
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-snow-elephants_WMMQD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-bear-and-hare-warm-up_AFRAI.pdf?up=1486106143
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-the-dog-and-his-reflection_RASCA.pdf?up=1540277833
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-substitute-meany_MEANY.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-frankensteinface_WBDQF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-road-trip_RTRIP.pdf?up=1467784605
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-fortune-teller_FORTU.pdf?up=1468909260
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-dinosaur-graveyard_WBDQM.pdf?up=1498817395
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-high-flyer_WBDQN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-diary-dancer_WBDQR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-underwater-adventure_UNDE.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-readingcomprehension-CarolineCastle_WBDQT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-foggy-figure_FOGGY.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-labyrinth_LABYR.pdf?up=1494574932
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-singing-cousin_SINGS.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-big-city-fun_BIGCI.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-misplaced_SOCKS.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-rumors-running-wild_RUMOR.pdf?up=1468910600
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-different-cultures_DIFFT.pdf?up=1468567519
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-class-leader_LEADE.pdf?up=1471842653
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-catch_FISHY.pdf?up=1468566653
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-flag-many-faces_WBDMD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-a-promise-is-a-promise_SKATE.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-flower-girl_WBDMF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-design-flaws_OIJNM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-shadow_SHADO.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-shooting-for-perfection_SHOOT.pdf?up=1471450810
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-acting-the-part_ACTIN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-the-champ_SPORT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-spider-webs_WEBS.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-sibling-swap_WBDMM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-rhythmless-blues_WBDMN.pdf?up=1489401011
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-new-girl_NEWSC.pdf?up=1468405478
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-a-snowy-spring_SPRIN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-erupting-with-fun_ERUPT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-broken-perspective_RAINB.pdf?up=1471843184
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-personal-introduction_INTRO.pdf?up=1468222097
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-new-puppy_PDAZE.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-missing-information_INFOR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-a-home-for-kitty_KITTY.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-out-of-time_GAMES.pdf?up=1471451748
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-we-are-all-winners_AOKAY.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-nothing-to-fear_NOTHI.pdf?up=1471883799
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-olympic-games_OLYMP.pdf?up=1470933928
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-best-birthday-present-ever_HORSE.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-moon-assignment_MOONS.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-my-summer-vacation_VACAT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-big-cousin-big-responsibility_COUSI.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/readerstheater/4th-pirates_WBDNB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-rabbit-habit-poem_WBDNF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-snow-troops_WBDNQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-playing-catch_WBDNM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-family-features_WBDNN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-butter-churn_WBDNR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-spring-surprises-poem_SPRING.pdf?up=1490166690
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-night-story_WBDNT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-similes_WBDNW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-metaphors_WBDNZ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-onomatopoeia_WBDRB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-sunset_SPOEM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-a-dragon-for-hailey_POEMS.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-desert-flowers_WBDRD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/poems/4th-urban-suburban-rural_WHERE.pdf?up=1572339166
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/poems/4th-supply-and-demand_MONEY.pdf?up=1569474515
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-gingerbreadman_WBDRF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-these-trees_WBDRQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-venus-flytraps_WBDRM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-frontier-life_WBDRN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-pillbugs_WBDRW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-pillbugs_WBDRW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-whats-the-deal-with-mold_MOLDY.pdf?up=1509708663
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-achoo_WBDRT.pdf?up=1473766647
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-running_WBDRZ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-recycling_IMPOR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-whats-the-deal-with-sleep_SLEEP.pdf?up=1550749441
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-everest_WBDTB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-popcorn-history_WBDTD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-dolphins_PORPO.pdf?up=1519642509
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-worlds-largest-fish_WHARK.pdf?up=1570170987
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-electric-eels_SHOCK.pdf?up=1476436969
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-fish-with-a-stinging-tail_STING.pdf?up=1528882354
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-constellations_WBDTF.pdf?up=1478782767
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-why-leaves-change-color_LEAFY.pdf?up=1499942833
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-chew-on-these-bubble-gum-facts_BUBBA.pdf?up=1490592757
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-stink-bugs_WBDTQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-desert-cottontails_WBDTM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-rabbits-and-hares_BUNNY.pdf?up=1476871490
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-flightlessbirds_WBDTN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-polar-bears_WBDTR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-tiger-talk_TIGER.pdf?up=1470933889
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-nocturnal_WBDTT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-fennec-fox_WDFMZ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-porcupine_WBDTZ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-boa-python_WBDWB.pdf?up=1523353801
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-alligators-crocodiles_WBDWF.pdf?up=1563430046
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-dandelions_WBDWQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-ants_WBDWM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-tarantula_WBDWN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-mermaids_WBDWR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-wetlands_WBDWT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-blimp_WBDWW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-the-big-ostrich_HATCH.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th_life-american-colonies_AMCOL.pdf?up=1580814208
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-grasshoppers_WBDWZ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-teddy-bear_WBDZB.pdf?up=1562583631
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-frogs-toads_WBDZD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-walnuts_WBDZF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-chameleon_WDFRN.pdf?up=1496216870
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-carnivores-herbivores_WBDZM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-a-pride-of-lions_TREES.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-sharks_AREAW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/readComp-Winter_WBDZN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-hedgehogs_WBDZR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-alligator-snapping-turtles_SNAPT.pdf?up=1550657117
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-iguanas_WBDZT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-disney_WBDZZ.pdf?up=1582716341
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-lincoln_WBFBB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-washington_WBFBD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-drseuss_WBFBF.pdf?up=1582707434
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/rosaparks-4_WBFBQ.pdf?up=1581601831
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-toothache_WBFBN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-heels_WBFBR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/comprehension7-mammothcave_WBFBT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/comprehenson1-amazon_WBFBW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-haiku-poems_WBFBZ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-gertrude-ederle_WBFDB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/5th-comprehension.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/5th-comprehension.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/5th-comprehension.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):
/reading-comp/5th-scorpions-bathr_WBFFB.pdf?up=1466611200
/reading-comp/5th-scorpions-bathr_WBFFB.pdf?up=1466611200
/reading-comp/5th-spitting-animals_WBFFF.pdf?up=1466611200
/reading-comp/5th-spitting-animals_WBFFF.pdf?up=1466611200
/reading-comp/5th-a-mob-of-meerkats_RKATS.pdf?up=1466611200
/reading-comp/5th-what-is-the-constitution_LAWSS.pdf?up=1565368225
/reading-comp/5th-lets-understand-the-bill-of-rights_RIGHT.pdf?up=1566924787
/reading-comp/5th-knit-bit_KNITS.pdf?up=1583395306
/reading-comp/5th-april-fools_WBFFD.pdf?up=1466611200
/reading-comp/5th-giant-pandas_CHINA.pdf?up=1473746345
/reading-comp/5th-red-pandas_NANAS.pdf?up=1520917326
/reading-comp/5th-lizard-with-a-blue-tongue_YHWQA.pdf?up=1466611200
/reading-comp/5th-american-bison_ISONB.pdf?up=1489422227
/reading-comp/5th-wolves-true-wilderness-animals_HOWLS.pdf?up=1523030962
/reading-comp/5th-the-amazingly-adapted-arctic-fox_FOXES.pdf?up=1516800061
/reading-comp/5th-brown-bears_KODIA.pdf?up=1503333640
/reading-comp/5th-the-worlds-largest-deer_MOOSE.pdf?up=1479459211
/reading-comp/5th-lets-get-to-know-china_CHINA.pdf?up=1563523438
/reading-comp/5th-the-ancient-civilization-of-china_ANCIE.pdf?up=1563523427
/reading-comp/5th-going-to-school-in-china_SCHOO.pdf?up=1563523429
/reading-comp/5th-skydiving_WBFFQ.pdf?up=1466611200
/reading-comp/5th-greatwhitesharks_WBFFN.pdf?up=1466611200
/reading-comp/5th-eight-legged-creatures_OCTOP.pdf?up=1559208112
/reading-comp/5th-types-of-rocks_METAM.pdf?up=1466611200
/reading-comp/5th-renewable-vs-nonrenewable-resources_SOLAR.pdf?up=1549970054
/reading-comp/5th-the-magic-of-rainbows_LIGHT.pdf?up=1490592805
/reading-comp/5th-hurricanes_WBFFR.pdf?up=1504242395
/reading-comp/5th-the-worlds-scaliest-mammal_PANGO.pdf?up=1548766065
/reading-comp/5th-seahorse_WBFFT.pdf?up=1466611200
/reading-comp/5th-scorpions_STING.pdf?up=1500024374
/reading/braille-alphabet_LOUIS.pdf?up=1466611200
/reading-comp/5th-what-is-a-wombat_AUSSI.pdf?up=1487054977
/reading-comp/5th-killer-algae_WBFFW.pdf?up=1466611200
/reading-comp/5th-sensitive-plant_WBFFZ.pdf?up=1466611200
/reading-comp/5th-railway-jim_WBFQB.pdf?up=1466611200
/reading-comp/5th-bridge-mechanics_BRIDG.pdf?up=1509513403
/reading-comp/comprehension6-libertybell_WBFQD.pdf?up=1466611200
/reading-comp/5th-ladybugs_WBFQF.pdf?up=1466611200
/reading-comp/5th-dwarf-planets_WBFQQ.pdf?up=1466611200
/reading-comp/5th-llama_WBFNT.pdf?up=1466611200
/reading-comp/5th-master-grasses_WBFNW.pdf?up=1466611200
/reading-comp/5th-jaguars_WBFQM.pdf?up=1466611200
/reading-comp/5th-howler-monkeys_HOWLR.pdf?up=1466611200
/reading-comp/5th-rainforest1_RAINF.pdf?up=1466611200
/reading-comp/5th-rainforest2-hoatzin_HOATZ.pdf?up=1466611200
/reading-comp/5th-greek-gods_WBFQN.pdf?up=1466611200
/reading-comp/5th-visayan-warty-pigs_WISAY.pdf?up=1466611200
/reading-comp/5th-elephant-seals_WBFQR.pdf?up=1466611200
/reading-comp/5th-invention-gum_WBDWD.pdf?up=1490596612
/reading-comp/5th-clockatoo_WBFQW.pdf?up=1466611200
/reading-comp/5th-hyperbole_WBFQZ.pdf?up=1466611200
/reading-comp/5th-spelling-bee_WBFMB.pdf?up=1466611200
/reading-comp/5th-flibbertigibbit_WBFMD.pdf?up=1466611200
/reading-comp/5th-tick-tock-watch_WBFMF.pdf?up=1466611200
/reading-comp/5th-ferry-poem_WBFMQ.pdf?up=1466611200
/reading-comp/5th-kitten-christmas-poem_WBFMM.pdf?up=1466611200
/reading-comp/5th-fairy-dance_WBFMN.pdf?up=1466611200
/reading-comp/5th-opossums_WBFMR.pdf?up=1466611200
/reading-comp/5th-rafflesia_WBFMT.pdf?up=1484645408
/reading-comp/5th-the-garden_TOMAT.pdf?up=1466611200
/reading-comp/5th-the-garden_TOMAT.pdf?up=1466611200
/reading-comp/5th-fancy-bread_PARAS.pdf?up=1466611200
/reading-comp/5th-the-most-wonderful-answer-of-all_FAIRY.pdf?up=1540277743
/reading-comp/5th-heroes-in-the-sky_VHERO.pdf?up=1466611200
/reading-comp/5th-field-day-champion_CHAMP.pdf?up=1469426599
/reading-comp/5th-hiddentreasure_WBFNB.pdf?up=1466611200
/reading-comp/5th-graduation-party_WBFND.pdf?up=1466611200
/reading-comp/5th-wailing-well_WBFNF.pdf?up=1466611200
/reading-comp/5th-pony-mark_WBFNQ.pdf?up=1466611200
/reading-comp/5th-locked-out_ALONE.pdf?up=1497872680
/reading-comp/5th-team-for-trish_WBFNM.pdf?up=1550581863
/reading-comp/5th-twister_WBFNN.pdf?up=1499923431
/reading-comp/5th-holiday-musical-blues_WBFNR.pdf?up=1496395569
/reading-comp/5th-medicine-woman_MEDDS.pdf?up=1580814209
/reading-comp/5th-alice-wonderland_WBFRD.pdf?up=1466611200
/reading-comp/5th-clara-barton_WBFRF.pdf?up=1466611200
/reading-comp/5th-louisa-may_WBFRQ.pdf?up=1582800408
/reading-comp/5th-jane-addams_WBFRM.pdf?up=1582716334
/reading-comp/5th-founding-fathers_WBFRN.pdf?up=1466611200
/reading-comp/5th-audubon_WBFRR.pdf?up=1515656576
/read-aloud/read-aloud-flibbertigibbit-poem-5_FLIB5.pdf?up=1475043878
/read-aloud/read-aloud-founding-fathers-5_ADAMS.pdf?up=1475043884
/reading-comp/5th-muir_WBFFM.pdf?up=1466611200
 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5:
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-scorpions-bathr_WBFFB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-scorpions-bathr_WBFFB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-spitting-animals_WBFFF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-spitting-animals_WBFFF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-a-mob-of-meerkats_RKATS.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-what-is-the-constitution_LAWSS.pdf?up=1565368225
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-lets-understand-the-bill-of-rights_RIGHT.pdf?up=1566924787
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-knit-bit_KNITS.pdf?up=1583395306
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-april-fools_WBFFD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-giant-pandas_CHINA.pdf?up=1473746345
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-red-pandas_NANAS.pdf?up=1520917326
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-lizard-with-a-blue-tongue_YHWQA.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-american-bison_ISONB.pdf?up=1489422227
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-wolves-true-wilderness-animals_HOWLS.pdf?up=1523030962
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-the-amazingly-adapted-arctic-fox_FOXES.pdf?up=1516800061
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-brown-bears_KODIA.pdf?up=1503333640
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-the-worlds-largest-deer_MOOSE.pdf?up=1479459211
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-lets-get-to-know-china_CHINA.pdf?up=1563523438
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-the-ancient-civilization-of-china_ANCIE.pdf?up=1563523427
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-going-to-school-in-china_SCHOO.pdf?up=1563523429
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-skydiving_WBFFQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-greatwhitesharks_WBFFN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-eight-legged-creatures_OCTOP.pdf?up=1559208112
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-types-of-rocks_METAM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-renewable-vs-nonrenewable-resources_SOLAR.pdf?up=1549970054
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-the-magic-of-rainbows_LIGHT.pdf?up=1490592805
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-hurricanes_WBFFR.pdf?up=1504242395
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-the-worlds-scaliest-mammal_PANGO.pdf?up=1548766065
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-seahorse_WBFFT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-scorpions_STING.pdf?up=1500024374
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading/braille-alphabet_LOUIS.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-what-is-a-wombat_AUSSI.pdf?up=1487054977
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-killer-algae_WBFFW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-sensitive-plant_WBFFZ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-railway-jim_WBFQB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-bridge-mechanics_BRIDG.pdf?up=1509513403
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/comprehension6-libertybell_WBFQD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-ladybugs_WBFQF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-dwarf-planets_WBFQQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-llama_WBFNT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-master-grasses_WBFNW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-jaguars_WBFQM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-howler-monkeys_HOWLR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-rainforest1_RAINF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-rainforest2-hoatzin_HOATZ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-greek-gods_WBFQN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-visayan-warty-pigs_WISAY.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-elephant-seals_WBFQR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-invention-gum_WBDWD.pdf?up=1490596612
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-clockatoo_WBFQW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-hyperbole_WBFQZ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-spelling-bee_WBFMB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-flibbertigibbit_WBFMD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-tick-tock-watch_WBFMF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-ferry-poem_WBFMQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-kitten-christmas-poem_WBFMM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-fairy-dance_WBFMN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-opossums_WBFMR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-rafflesia_WBFMT.pdf?up=1484645408
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-the-garden_TOMAT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-the-garden_TOMAT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-fancy-bread_PARAS.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-the-most-wonderful-answer-of-all_FAIRY.pdf?up=1540277743
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-heroes-in-the-sky_VHERO.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-field-day-champion_CHAMP.pdf?up=1469426599
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-hiddentreasure_WBFNB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-graduation-party_WBFND.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-wailing-well_WBFNF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-pony-mark_WBFNQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-locked-out_ALONE.pdf?up=1497872680
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-team-for-trish_WBFNM.pdf?up=1550581863
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-twister_WBFNN.pdf?up=1499923431
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-holiday-musical-blues_WBFNR.pdf?up=1496395569
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-medicine-woman_MEDDS.pdf?up=1580814209
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-alice-wonderland_WBFRD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-clara-barton_WBFRF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-louisa-may_WBFRQ.pdf?up=1582800408
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-jane-addams_WBFRM.pdf?up=1582716334
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-founding-fathers_WBFRN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-audubon_WBFRR.pdf?up=1515656576
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/read-aloud/read-aloud-flibbertigibbit-poem-5_FLIB5.pdf?up=1475043878
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/read-aloud/read-aloud-founding-fathers-5_ADAMS.pdf?up=1475043884
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-muir_WBFFM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/6th-comprehension.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/6th-comprehension.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/6th-comprehension.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):
/reading-comp/6th-elephant-seals_SNOUT.pdf?up=1501671913
/reading-comp/6th-elephant-seals_SNOUT.pdf?up=1501671913
/reading-comp/6th-terrifying-tsunamis_WATER.pdf?up=1466611200
/reading-comp/6th-terrifying-tsunamis_WATER.pdf?up=1466611200
/reading-comp/7th-fossils_GEOLO.pdf?up=1472620291
/reading-comp/6th-now-thats-gross_FLIES.pdf?up=1559208107
/reading-comp/6th-red-kangaroo_KANGA.pdf?up=1466611200
/reading-comp/6th-beavers_BUSYB.pdf?up=1466611200
/reading-comp/6th-leopards_SPOTS.pdf?up=1481272161
/reading-comp/6th-pigeons-working_PIGEONS.pdf?up=1466611200
/reading-comp/6th-valuable-vultures_WORLD.pdf?up=1488532351
/reading-comp/6th-poison-dart-frogs_DARTF.pdf?up=1508756019
/reading-comp/6th-sea-turtles_GREEN.pdf?up=1466611200
/reading-comp/6th-cells_BLOCK.pdf?up=1471585211
/reading-comp/6th-sea-snakes_SEASN.pdf?up=1466611200
/reading-comp/6th-mysterious-moray-eels_AMORE.pdf?up=1466611200
/reading-comp/science-innerplanets_WBRRN.pdf?up=1466611200
/reading-comp/science-outerplanets_WBRRR.pdf?up=1466611200
/reading-comp/6th-dollar-bill_BUCKS.pdf?up=1466611200
/reading-comp/6th-rhinoceros-beetle_BEETL.pdf?up=1466611200
/reading-comp/6th-leaf-cutter-ants_LEAFC.pdf?up=1466611200
/reading-comp/6th-stick-insects_STICK.pdf?up=1516349878
/reading-comp/6th-great-minds-albert-einstein_SCIEN.pdf?up=1490771615
/reading-comp/6th-great-minds-albert-einstein_SCIEN.pdf?up=1490771615
/reading-comp/6th-great-minds-j-k-rowling_MAGIC.pdf?up=1532329840
/reading-comp/6th-great-minds-helen-keller_GMSHK.pdf?up=1499670413
/reading-comp/6th-great-minds-martin-luther-king_MLKJR.pdf?up=1499842834
/reading-comp/6th-great-minds-vincent-van-gogh_VGOGH.pdf?up=1512995031
/reading-comp/6th-queen-of-the-night_BLOOM.pdf?up=1570171544
/reading-comp/6th-taxi-crab_TCRAB.pdf?up=1585897014
/reading-comp/6th-falling-asleep_SLEEP.pdf?up=1466611200
/reading-comp/6th-tumble_TUMBL.pdf?up=1466611200
 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5:
cmd( 1):
processing https://www.downloadtest.com/reading-comp/6th-elephant-seals_SNOUT.pdf?up=1501671913
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/6th-elephant-seals_SNOUT.pdf?up=1501671913
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/6th-terrifying-tsunamis_WATER.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/6th-terrifying-tsunamis_WATER.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/7th-fossils_GEOLO.pdf?up=1472620291
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/6th-now-thats-gross_FLIES.pdf?up=1559208107
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/6th-red-kangaroo_KANGA.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/6th-beavers_BUSYB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/6th-leopards_SPOTS.pdf?up=1481272161
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/6th-pigeons-working_PIGEONS.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/6th-valuable-vultures_WORLD.pdf?up=1488532351
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/6th-poison-dart-frogs_DARTF.pdf?up=1508756019
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/6th-sea-turtles_GREEN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/6th-cells_BLOCK.pdf?up=1471585211
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/6th-sea-snakes_SEASN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/6th-mysterious-moray-eels_AMORE.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/science-innerplanets_WBRRN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/science-outerplanets_WBRRR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/6th-dollar-bill_BUCKS.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/6th-rhinoceros-beetle_BEETL.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/6th-leaf-cutter-ants_LEAFC.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/6th-stick-insects_STICK.pdf?up=1516349878
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/6th-great-minds-albert-einstein_SCIEN.pdf?up=1490771615
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/6th-great-minds-albert-einstein_SCIEN.pdf?up=1490771615
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/6th-great-minds-j-k-rowling_MAGIC.pdf?up=1532329840
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/6th-great-minds-helen-keller_GMSHK.pdf?up=1499670413
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/6th-great-minds-martin-luther-king_MLKJR.pdf?up=1499842834
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/6th-great-minds-vincent-van-gogh_VGOGH.pdf?up=1512995031
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/6th-queen-of-the-night_BLOOM.pdf?up=1570171544
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/6th-taxi-crab_TCRAB.pdf?up=1585897014
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/6th-falling-asleep_SLEEP.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/6th-tumble_TUMBL.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/comprehension.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/comprehension.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/comprehension.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):

 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
---Step5:
cmd( 1):
processing https://www.downloadtest.com
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/reading.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/reading.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/reading.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):
/reading/book-bingo_WDFBF.pdf?up=1466611200
/reading/book-bingo_WDFBF.pdf?up=1466611200
/reading/book-bingo-2nd_WDFBQ.pdf?up=1466611200
/reading/book-bingo-2nd_WDFBQ.pdf?up=1466611200
/reading/book-bingo-3rd_WDFBM.pdf?up=1466611200
/reading/book-bingo-4th_WDFBN.pdf?up=1466611200
/reading/reading-report_WDFBW.pdf?up=1466611200
/reading/reading-report-non-fiction_WDFBZ.pdf?up=1466611200
/reading/reading-report-biography_WDFDB.pdf?up=1466611200
/reading/reading-report-mystery_WDFDD.pdf?up=1466611200
/reading/reading-report-fable_WDFDF.pdf?up=1466611200
/reading/fiction1-small_MDYXI.pdf?up=1466611200
/reading/fiction1-large_FSAJS.pdf?up=1466611200
/reading/non-fiction1-small_JDHAA.pdf?up=1466611200
/reading/non-fiction1-large_GSJZX.pdf?up=1466611200
/reading/biography1-small_GDJSY.pdf?up=1466611200
/reading/biography1-large_GDISU.pdf?up=1466611200
/reading/summarizer_WDFDM.pdf?up=1466611200
/reading/discussion-leader_WDFDN.pdf?up=1466611200
/reading/word-wizard_WDFDR.pdf?up=1466611200
/reading/illustrator_WDFDT.pdf?up=1466611200
/reading/real-life_WDFDW.pdf?up=1466611200
/reading/story-connector_WDFDZ.pdf?up=1466611200
/reading/lit-circle-cover2_WDFFB.pdf?up=1466611200
/reading/reading-chart_WDFFQ.pdf?up=1466611200
/reading/reading-log_WDFFR.pdf?up=1466611200
/sticker-charts/sticker-chart-reading-rocks-boy1_WDFFM.pdf?up=1466611200
/sticker-charts/sticker-chart-reading-rocks-girl1_WDFFN.pdf?up=1466611200
/reading/task-cards-book_discussion_QWERT.pdf?up=1484226920
/reading/task-cards-book_discussion_QWERT.pdf?up=1484226920
/reading/book-pennant_GGTAS.pdf?up=1562577029
/writing/author-letter_WDZQR.pdf?up=1471854505
/reading/reading-survey_WDDWF.pdf?up=1466611200
/reading/story-cube_WDDWD.pdf?up=1466611200
/reading/book-review_WDDWM.pdf?up=1466611200
/graphic-organizers/story_WDDWR.pdf?up=1466611200
/graphic-organizers/story-harder_WDDWT.pdf?up=1466611200
/reading/main-idea_WDDWW.pdf?up=1474966479
/reading/sequencing1_WDDWZ.pdf?up=1466611200
/reading/beginning-ending_WDDZB.pdf?up=1466611200
/reading/reading-real-or-fantasy_WDDZD.pdf?up=1466611200
/reading/a-what-does-it-mean_WDDZF.pdf?up=1466611200
/reading/question-words-cut-and-glue_JXTTY.pdf?up=1474537918
/reading/bookmarks1_FGPWM.pdf?up=1466611200
/reading/bookmarks2_WRGMS.pdf?up=1466611200
/reading/bookmarks_WDDWQ.pdf?up=1466611200
 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5:
cmd( 1):
processing https://www.downloadtest.com/reading/book-bingo_WDFBF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading/book-bingo_WDFBF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading/book-bingo-2nd_WDFBQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading/book-bingo-2nd_WDFBQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading/book-bingo-3rd_WDFBM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading/book-bingo-4th_WDFBN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading/reading-report_WDFBW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading/reading-report-non-fiction_WDFBZ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading/reading-report-biography_WDFDB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading/reading-report-mystery_WDFDD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading/reading-report-fable_WDFDF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading/fiction1-small_MDYXI.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading/fiction1-large_FSAJS.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading/non-fiction1-small_JDHAA.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading/non-fiction1-large_GSJZX.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading/biography1-small_GDJSY.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading/biography1-large_GDISU.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading/summarizer_WDFDM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading/discussion-leader_WDFDN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading/word-wizard_WDFDR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading/illustrator_WDFDT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading/real-life_WDFDW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading/story-connector_WDFDZ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading/lit-circle-cover2_WDFFB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading/reading-chart_WDFFQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading/reading-log_WDFFR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sticker-charts/sticker-chart-reading-rocks-boy1_WDFFM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sticker-charts/sticker-chart-reading-rocks-girl1_WDFFN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading/task-cards-book_discussion_QWERT.pdf?up=1484226920
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading/task-cards-book_discussion_QWERT.pdf?up=1484226920
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading/book-pennant_GGTAS.pdf?up=1562577029
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing/author-letter_WDZQR.pdf?up=1471854505
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading/reading-survey_WDDWF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading/story-cube_WDDWD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading/book-review_WDDWM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/graphic-organizers/story_WDDWR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/graphic-organizers/story-harder_WDDWT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading/main-idea_WDDWW.pdf?up=1474966479
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading/sequencing1_WDDWZ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading/beginning-ending_WDDZB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading/reading-real-or-fantasy_WDDZD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading/a-what-does-it-mean_WDDZF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading/question-words-cut-and-glue_JXTTY.pdf?up=1474537918
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading/bookmarks1_FGPWM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading/bookmarks2_WRGMS.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading/bookmarks_WDDWQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/causeeffect.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/causeeffect.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/causeeffect.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):
/causeeffectfactopinion/causeeffect1_WBMBB.pdf?up=1542264742
/causeeffectfactopinion/causeeffect1_WBMBB.pdf?up=1542264742
/causeeffectfactopinion/causeeffect2_WBMBD.pdf?up=1542265163
/causeeffectfactopinion/causeeffect2_WBMBD.pdf?up=1542265163
/causeeffectfactopinion/causeeffect4_WBMBF.pdf?up=1542265428
/causeeffectfactopinion/causeeffect3_WBMBQ.pdf?up=1558596332
/causeeffectfactopinion/causeeffect6_WBMBM.pdf?up=1559036208
/causeeffectfactopinion/CauseandEffectFill2_WBMBN.pdf?up=1541574473
/causeeffectfactopinion/causeeffect5_WBMBW.pdf?up=1541510405
/causeeffectfactopinion/causeeffect5_WBMBW.pdf?up=1541510405
/causeeffectfactopinion/HolidayCause-Effect_WBMBZ.pdf?up=1541510441
/causeeffectfactopinion/HolidayCause-Effect2_WBMDB.pdf?up=1541510443
 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5:
cmd( 1):
processing https://www.downloadtest.com/causeeffectfactopinion/causeeffect1_WBMBB.pdf?up=1542264742
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/causeeffectfactopinion/causeeffect1_WBMBB.pdf?up=1542264742
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/causeeffectfactopinion/causeeffect2_WBMBD.pdf?up=1542265163
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/causeeffectfactopinion/causeeffect2_WBMBD.pdf?up=1542265163
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/causeeffectfactopinion/causeeffect4_WBMBF.pdf?up=1542265428
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/causeeffectfactopinion/causeeffect3_WBMBQ.pdf?up=1558596332
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/causeeffectfactopinion/causeeffect6_WBMBM.pdf?up=1559036208
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/causeeffectfactopinion/CauseandEffectFill2_WBMBN.pdf?up=1541574473
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/causeeffectfactopinion/causeeffect5_WBMBW.pdf?up=1541510405
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/causeeffectfactopinion/causeeffect5_WBMBW.pdf?up=1541510405
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/causeeffectfactopinion/HolidayCause-Effect_WBMBZ.pdf?up=1541510441
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/causeeffectfactopinion/HolidayCause-Effect2_WBMDB.pdf?up=1541510443
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/factopinion.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/factopinion.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/factopinion.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):
/causeeffectfactopinion/animalfactopinion_WBNNF.pdf?up=1466611200
/causeeffectfactopinion/animalfactopinion_WBNNF.pdf?up=1466611200
/causeeffectfactopinion/factopinion1_WBNNQ.pdf?up=1466611200
/causeeffectfactopinion/factopinion1_WBNNQ.pdf?up=1466611200
/causeeffectfactopinion/factopinion2_WBNNM.pdf?up=1466611200
/causeeffectfactopinion/factopinion5_WBNNN.pdf?up=1466611200
/causeeffectfactopinion/factopinion3_WBNNR.pdf?up=1466611200
/causeeffectfactopinion/factopinion4_WBNNW.pdf?up=1466611200
/causeeffectfactopinion/factopinion4_WBNNW.pdf?up=1466611200
/causeeffectfactopinion/cootie-catcher-fact-opinion_AODJC.pdf?up=1466611200
 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5:
cmd( 1):
processing https://www.downloadtest.com/causeeffectfactopinion/animalfactopinion_WBNNF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/causeeffectfactopinion/animalfactopinion_WBNNF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/causeeffectfactopinion/factopinion1_WBNNQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/causeeffectfactopinion/factopinion1_WBNNQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/causeeffectfactopinion/factopinion2_WBNNM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/causeeffectfactopinion/factopinion5_WBNNN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/causeeffectfactopinion/factopinion3_WBNNR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/causeeffectfactopinion/factopinion4_WBNNW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/causeeffectfactopinion/factopinion4_WBNNW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/causeeffectfactopinion/cootie-catcher-fact-opinion_AODJC.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/graphic-organizers.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/graphic-organizers.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/graphic-organizers.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):
/graphic-organizers/hamburger-writing_WBRDM.pdf?up=1500476205
/graphic-organizers/hamburger-writing_WBRDM.pdf?up=1500476205
/graphic-organizers/hamburger-writing-1_HYSDA.pdf?up=1572872620
/graphic-organizers/hamburger-writing-1_HYSDA.pdf?up=1572872620
/graphic-organizers/hamburger-writing-2_SFRAT.pdf?up=1572872619
/graphic-organizers/flower-writing_WBRDR.pdf?up=1466611200
/graphic-organizers/flower-writing-lines_WBRDT.pdf?up=1466611200
/graphic-organizers/robot-writing_WBRDW.pdf?up=1466611200
/graphic-organizers/ice-cream-writing_WBRDZ.pdf?up=1466611200
/writing-persuasive-opinion/persuasive-writing-graphic-organizer_HTRGW.pdf?up=1466611200
/writing-persuasive-opinion/persuasive-writing-graphic-organizer2_HTERW.pdf?up=1466611200
/graphic-organizers/web-easier_WBRFB.pdf?up=1466611200
/graphic-organizers/web-higher_WBRFD.pdf?up=1466611200
/graphic-organizers/writing-web-4_WRWEB.pdf?up=1466611200
/graphic-organizers/writing-web-6_WBSIX.pdf?up=1466611200
/graphic-organizers/writing-web-8_WWEB8.pdf?up=1466611200
/five-senses/five-senses-generic_JYUHA.pdf?up=1466611200
/graphic-organizers/five-senses-web-2_GATHY.pdf?up=1466611200
/graphic-organizers/venn-no-lines_WBRFF.pdf?up=1466611200
/graphic-organizers/venn-lines_WBRFQ.pdf?up=1466611200
/five-senses/five-senses-graphic-organizer_THAYO.pdf?up=1466611200
/graphic-organizers/t-chart-lines_WBRFM.pdf?up=1466611200
/graphic-organizers/t-chart-plain_WBRFN.pdf?up=1466611200
/graphic-organizers/t-chart-numbered_WBRFR.pdf?up=1466611200
/graphic-organizers/concept-wheel-6_WBRFT.pdf?up=1466611200
/graphic-organizers/concept-wheel-8_WBRFW.pdf?up=1466611200
/graphic-organizers/story_WBRQD.pdf?up=1466611200
/graphic-organizers/story_WBRQD.pdf?up=1466611200
/graphic-organizers/story-harder_WBRQF.pdf?up=1466611200
/graphic-organizers/character-comparisons_WBRQQ.pdf?up=1466611200
/graphic-organizers/character-comparisons2_WBRQM.pdf?up=1466611200
/graphic-organizers/sequence-film_FILMS.pdf?up=1466611200
/graphic-organizers/sequence-chain_CHAIN.pdf?up=1466611200
/graphic-organizers/sequence-chain-lines_LINES.pdf?up=1466611200
/graphic-organizers/kwl-lines_WBRQR.pdf?up=1466611200
/graphic-organizers/kwl_WBRQT.pdf?up=1466611200
/graphic-organizers/kwhl-lines_WBRQW.pdf?up=1466611200
/graphic-organizers/kwhl_WBRQZ.pdf?up=1466611200
/graphic-organizers/relationship-tree1_RELTR.pdf?up=1466611200
/graphic-organizers/relationship-tree2_RTREE.pdf?up=1466611200
/graphic-organizers/relationship-tree3_RETRE.pdf?up=1466611200
/graphic-organizers/question-mark-organizer_QUORG.pdf?up=1475060154
/graphic-organizers/question-word-organizer_UUAYT.pdf?up=1475231201
 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5:
cmd( 1):
processing https://www.downloadtest.com/graphic-organizers/hamburger-writing_WBRDM.pdf?up=1500476205
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/graphic-organizers/hamburger-writing_WBRDM.pdf?up=1500476205
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/graphic-organizers/hamburger-writing-1_HYSDA.pdf?up=1572872620
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/graphic-organizers/hamburger-writing-1_HYSDA.pdf?up=1572872620
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/graphic-organizers/hamburger-writing-2_SFRAT.pdf?up=1572872619
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/graphic-organizers/flower-writing_WBRDR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/graphic-organizers/flower-writing-lines_WBRDT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/graphic-organizers/robot-writing_WBRDW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/graphic-organizers/ice-cream-writing_WBRDZ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing-persuasive-opinion/persuasive-writing-graphic-organizer_HTRGW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing-persuasive-opinion/persuasive-writing-graphic-organizer2_HTERW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/graphic-organizers/web-easier_WBRFB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/graphic-organizers/web-higher_WBRFD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/graphic-organizers/writing-web-4_WRWEB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/graphic-organizers/writing-web-6_WBSIX.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/graphic-organizers/writing-web-8_WWEB8.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/five-senses/five-senses-generic_JYUHA.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/graphic-organizers/five-senses-web-2_GATHY.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/graphic-organizers/venn-no-lines_WBRFF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/graphic-organizers/venn-lines_WBRFQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/five-senses/five-senses-graphic-organizer_THAYO.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/graphic-organizers/t-chart-lines_WBRFM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/graphic-organizers/t-chart-plain_WBRFN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/graphic-organizers/t-chart-numbered_WBRFR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/graphic-organizers/concept-wheel-6_WBRFT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/graphic-organizers/concept-wheel-8_WBRFW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/graphic-organizers/story_WBRQD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/graphic-organizers/story_WBRQD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/graphic-organizers/story-harder_WBRQF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/graphic-organizers/character-comparisons_WBRQQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/graphic-organizers/character-comparisons2_WBRQM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/graphic-organizers/sequence-film_FILMS.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/graphic-organizers/sequence-chain_CHAIN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/graphic-organizers/sequence-chain-lines_LINES.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/graphic-organizers/kwl-lines_WBRQR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/graphic-organizers/kwl_WBRQT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/graphic-organizers/kwhl-lines_WBRQW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/graphic-organizers/kwhl_WBRQZ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/graphic-organizers/relationship-tree1_RELTR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/graphic-organizers/relationship-tree2_RTREE.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/graphic-organizers/relationship-tree3_RETRE.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/graphic-organizers/question-mark-organizer_QUORG.pdf?up=1475060154
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/graphic-organizers/question-word-organizer_UUAYT.pdf?up=1475231201
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/proofreading.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/proofreading.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/proofreading.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):
/editing/editing-basketball_WBNRN.pdf?up=1466611200
/editing/editing-basketball_WBNRN.pdf?up=1466611200
/editing/editing-beach_WBNRR.pdf?up=1466611200
/editing/editing-beach_WBNRR.pdf?up=1466611200
/editing/editing-camping_WBNRT.pdf?up=1466611200
/editing/editing-candy-shop_WBNRW.pdf?up=1466611200
/editing/editing-pumpkin-patch_WBNRZ.pdf?up=1466611200
/editing/editing-spaghetti_WBNTB.pdf?up=1466611200
/editing/editing-street-hockey_WBNTD.pdf?up=1466611200
/editing/editing-swimming-pool_WBNTF.pdf?up=1466611200
/editing/editing-cleaning-up_WBNTQ.pdf?up=1466611200
/editing/editing-fish-tank_WBNTM.pdf?up=1466611200
/editing/editing-halloween-scare_WBNTN.pdf?up=1466611200
/editing/editing-mini-golf_WBNTR.pdf?up=1466611200
/editing/editing-pet-shop_WBNTT.pdf?up=1466611200
/writing/editing-wheel_WBNWQ.pdf?up=1466611200
/writing/editing-wheel_WBNWQ.pdf?up=1466611200
/writing/editing-wheel-color_WBNWM.pdf?up=1466611200
/editing/proofreading-bookmarks-basic_WBNWR.pdf?up=1466611200
/editing/proofreading-bookmarks-intermediate_WBNWT.pdf?up=1466611200
/editing/proofreading-bookmarks-advanced_WBNWW.pdf?up=1466611200
 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5:
cmd( 1):
processing https://www.downloadtest.com/editing/editing-basketball_WBNRN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/editing/editing-basketball_WBNRN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/editing/editing-beach_WBNRR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/editing/editing-beach_WBNRR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/editing/editing-camping_WBNRT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/editing/editing-candy-shop_WBNRW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/editing/editing-pumpkin-patch_WBNRZ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/editing/editing-spaghetti_WBNTB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/editing/editing-street-hockey_WBNTD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/editing/editing-swimming-pool_WBNTF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/editing/editing-cleaning-up_WBNTQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/editing/editing-fish-tank_WBNTM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/editing/editing-halloween-scare_WBNTN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/editing/editing-mini-golf_WBNTR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/editing/editing-pet-shop_WBNTT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing/editing-wheel_WBNWQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing/editing-wheel_WBNWQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing/editing-wheel-color_WBNWM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/editing/proofreading-bookmarks-basic_WBNWR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/editing/proofreading-bookmarks-intermediate_WBNWT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/editing/proofreading-bookmarks-advanced_WBNWW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/synonyms-antonyms.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/synonyms-antonyms.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/synonyms-antonyms.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):
/synonyms-antonyms/synonyms-cut-outs_WDWRZ.pdf?up=1466611200
/synonyms-antonyms/synonyms-cut-outs_WDWRZ.pdf?up=1466611200
/synonyms-antonyms/synonyms-cut-outs2_WDWTB.pdf?up=1466611200
/synonyms-antonyms/synonyms-cut-outs2_WDWTB.pdf?up=1466611200
/synonyms-antonyms/synonyms3_WDWTD.pdf?up=1466611200
/synonyms-antonyms/synonyms2_WDWTF.pdf?up=1466611200
/synonyms-antonyms/synonyms_WDWTQ.pdf?up=1466611200
/synonyms-antonyms/antonyms-cut-outs_WDWTN.pdf?up=1466611200
/synonyms-antonyms/antonyms-cut-outs2_WDWTR.pdf?up=1466611200
/synonyms-antonyms/antonyms_WDWTT.pdf?up=1466611200
/synonyms-antonyms/antonyms2_WDWTW.pdf?up=1466611200
/synonyms-antonyms/antonyms3_WDWTZ.pdf?up=1466611200
/synonyms-antonyms/antonyms4_WDWWB.pdf?up=1466611200
/synonyms-antonyms/syn-ant-mixed_WDWWQ.pdf?up=1466611200
/synonyms-antonyms/syn-ant-mixed_WDWWQ.pdf?up=1466611200
/synonyms-antonyms/scoot-synonyms-antonyms_OLAKU.pdf?up=1466611200
/synonyms-antonyms/synonyms-antonyms_WDWWM.pdf?up=1466611200
/synonyms-antonyms/syn-ant-homreview_WDWWN.pdf?up=1466611200
/synonyms-antonyms/syn-ant-analogies_WDWWR.pdf?up=1466611200
/synonyms-antonyms/synonym-antonym-qr-codes_LEVEL.pdf?up=1466611200
/synonyms-antonyms/synonym-antonym-qr-codes-intermediate_INTER.pdf?up=1466611200
/attributes/giraffe-hen_HENNS.pdf?up=1471939829
/attributes/big-small-heavy-light_BUGGS.pdf?up=1471938646
 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5:
cmd( 1):
processing https://www.downloadtest.com/synonyms-antonyms/synonyms-cut-outs_WDWRZ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/synonyms-antonyms/synonyms-cut-outs_WDWRZ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/synonyms-antonyms/synonyms-cut-outs2_WDWTB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/synonyms-antonyms/synonyms-cut-outs2_WDWTB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/synonyms-antonyms/synonyms3_WDWTD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/synonyms-antonyms/synonyms2_WDWTF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/synonyms-antonyms/synonyms_WDWTQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/synonyms-antonyms/antonyms-cut-outs_WDWTN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/synonyms-antonyms/antonyms-cut-outs2_WDWTR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/synonyms-antonyms/antonyms_WDWTT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/synonyms-antonyms/antonyms2_WDWTW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/synonyms-antonyms/antonyms3_WDWTZ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/synonyms-antonyms/antonyms4_WDWWB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/synonyms-antonyms/syn-ant-mixed_WDWWQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/synonyms-antonyms/syn-ant-mixed_WDWWQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/synonyms-antonyms/scoot-synonyms-antonyms_OLAKU.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/synonyms-antonyms/synonyms-antonyms_WDWWM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/synonyms-antonyms/syn-ant-homreview_WDWWN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/synonyms-antonyms/syn-ant-analogies_WDWWR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/synonyms-antonyms/synonym-antonym-qr-codes_LEVEL.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/synonyms-antonyms/synonym-antonym-qr-codes-intermediate_INTER.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/attributes/giraffe-hen_HENNS.pdf?up=1471939829
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/attributes/big-small-heavy-light_BUGGS.pdf?up=1471938646
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/writingideas.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/writingideas.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/writingideas.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):
/writing/1-bigmoney_WFBMF.pdf?up=1466611200
/writing/1-bigmoney_WFBMF.pdf?up=1466611200
/writing/2-brave_WFBMQ.pdf?up=1466611200
/writing/2-brave_WFBMQ.pdf?up=1466611200
/writing/3-meal_WFBMM.pdf?up=1466611200
/writing/4-dinosaur_WFBMN.pdf?up=1466611200
/writing/5-restaurant_WFBMR.pdf?up=1466611200
/writing/something-people-dont-know-about-me-wp_FACTS.pdf?up=1540982883
/writing/6-pirate_WFBMT.pdf?up=1466611200
/writing/7-pet_WFBMW.pdf?up=1466611200
/writing/big-waste-of-money-wp_MONEY.pdf?up=1540982884
/writing/8-siblings_WFBMZ.pdf?up=1466611200
/writing/9-celebritylunch_WFBNB.pdf?up=1466611200
/writing/10-bat_WFBND.pdf?up=1466611200
/writing/11-artist_WFBNF.pdf?up=1488363876
/writing/12-oldest_WFBNQ.pdf?up=1466611200
/writing/13-youngest_WFBNM.pdf?up=1466611200
/writing/14-petmonkey_WFBNN.pdf?up=1466611200
/writing/25-iwon_WFBNR.pdf?up=1486624952
/writing/26-sammich_WFBNT.pdf?up=1466611200
/writing/20-babydragon_WFBNW.pdf?up=1466611200
/writing/21-makingmoney_WFBNZ.pdf?up=1466611200
/writing/spider-web_WRITE.pdf?up=1466611200
/writing/smile_WRIT2.pdf?up=1466611200
/writing/build-a-story_TAGSG.pdf?up=1558596275
/writing/17-fouryearsago_WFBFN.pdf?up=1466611200
/writing/17-fouryearsago_WFBFN.pdf?up=1466611200
/writing/15-football_WFBFR.pdf?up=1466611200
/writing/16-home_WFBFT.pdf?up=1466611200
/writing/19-season_WFBFZ.pdf?up=1466611200
/writing/22-trip-to-outer-space_WFBQB.pdf?up=1486971416
/writing/23-familymember_WFBQD.pdf?up=1487159631
/writing/24-superme_WFBQF.pdf?up=1466611200
/writing/27-goodat_WFBQQ.pdf?up=1488364287
/writing/30-castle_WFBQM.pdf?up=1466611200
/writing/29-doctor_WFBQN.pdf?up=1487160449
/writing/18-fairy-godmother_WFGMO.pdf?up=1488537643
/writing/30-unusual-catch_WFGMO.pdf?up=1487160211
/writing/editing-wheel_WFBQT.pdf?up=1466611200
/writing/editing-wheel-color_WFBQW.pdf?up=1466611200
 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5:
cmd( 1):
processing https://www.downloadtest.com/writing/1-bigmoney_WFBMF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing/1-bigmoney_WFBMF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing/2-brave_WFBMQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing/2-brave_WFBMQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing/3-meal_WFBMM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing/4-dinosaur_WFBMN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing/5-restaurant_WFBMR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing/something-people-dont-know-about-me-wp_FACTS.pdf?up=1540982883
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing/6-pirate_WFBMT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing/7-pet_WFBMW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing/big-waste-of-money-wp_MONEY.pdf?up=1540982884
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing/8-siblings_WFBMZ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing/9-celebritylunch_WFBNB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing/10-bat_WFBND.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing/11-artist_WFBNF.pdf?up=1488363876
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing/12-oldest_WFBNQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing/13-youngest_WFBNM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing/14-petmonkey_WFBNN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing/25-iwon_WFBNR.pdf?up=1486624952
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing/26-sammich_WFBNT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing/20-babydragon_WFBNW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing/21-makingmoney_WFBNZ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing/spider-web_WRITE.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing/smile_WRIT2.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing/build-a-story_TAGSG.pdf?up=1558596275
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing/17-fouryearsago_WFBFN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing/17-fouryearsago_WFBFN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing/15-football_WFBFR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing/16-home_WFBFT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing/19-season_WFBFZ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing/22-trip-to-outer-space_WFBQB.pdf?up=1486971416
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing/23-familymember_WFBQD.pdf?up=1487159631
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing/24-superme_WFBQF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing/27-goodat_WFBQQ.pdf?up=1488364287
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing/30-castle_WFBQM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing/29-doctor_WFBQN.pdf?up=1487160449
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing/18-fairy-godmother_WFGMO.pdf?up=1488537643
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing/30-unusual-catch_WFGMO.pdf?up=1487160211
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing/editing-wheel_WFBQT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing/editing-wheel-color_WFBQW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/writing-storypics.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/writing-storypics.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/writing-storypics.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):
/writing-storypics/storypic-fishing_FISHI.pdf?up=1466611200
/writing-storypics/storypic-fishing_FISHI.pdf?up=1466611200
/writing-storypics/storypic-cat-pig-pool_CPOOL.pdf?up=1466611200
/writing-storypics/storypic-cat-pig-pool_CPOOL.pdf?up=1466611200
/writing-storypics/storypic-dragon_DRAGO.pdf?up=1466611200
/writing-storypics/storypic-kitten-tree_KTREE.pdf?up=1466611200
/writing-storypics/a-storypic-candy-corn_CORNS.pdf?up=1492767047
/writing-storypics/free-puppy_PUPPY.pdf?up=1466611200
/writing-storypics/a-storypic-baby-sister_WAAAA.pdf?up=1466611200
/writing-storypics/storypic-swimming-pool_SPOOL.pdf?up=1466611200
/writing-storypics/storypic-mean-monster_MONST.pdf?up=1466611200
/writing-storypics/storypic-tablet-kid_TABLT.pdf?up=1466611200
/writing-storypics/storypic-magician_MAGIC.pdf?up=1466611200
/writing-storypics/storypic-farm-horses_HORSE.pdf?up=1466611200
/writing-storypics/spring-kite-tree-storypic_TREES.pdf?up=1466611200
/writing-storypics/storypic-spooky-house-kids_HKIDS.pdf?up=1466611200
/olympics/story-pic-winter-olympics-daydream_SKIII.pdf?up=1466611200
/writing-storypics/storypic-car-wash_CWASH.pdf?up=1466611200
/writing-storypics/storypic-camp-fire_SMORE.pdf?up=1466611200
/backtoschool/story-pic-back-to-school_SCHOO.pdf?up=1466611200
/writing-storypics/storypic-icecream_ICECR.pdf?up=1466611200
/writing-storypics/storypic-detective_DETEC.pdf?up=1466611200
/writing-storypics/storypic-butterfly-girl_BTFLY.pdf?up=1466611200
/writing-storypics/storypic-boat-boy_BOATS.pdf?up=1466611200
/writing-storypics/storypic-car-guy_CAGUY.pdf?up=1466611200
/writing-storypics/storypic-birthday-three_THREE.pdf?up=1466611200
/presidents-day/storypic-lincoln-school-play-story_STORY.pdf?up=1466611200
/writing-storypics/storypic-firefighters_FIREF.pdf?up=1466611200
/writing-storypics/storypic-pets_PETSS.pdf?up=1466611200
/backtoschool/story-pic-apple-kid_APPLE.pdf?up=1466611200
/writing-storypics/storypic-laptop-girl_LAPTP.pdf?up=1466611200
/writing-storypics/a-storypic-tennis_WDZNT.pdf?up=1466611200
/writing-storypics/storypic-mailman_MAILM.pdf?up=1466611200
/drseuss/story-pic-seuss_UNICY.pdf?up=1466611200
/writing-storypics/storypic-bee-window_BWIND.pdf?up=1466611200
/writing-storypics/a-storypic-nervous-note-reader_NNOTE.pdf?up=1466611200
/writing-storypics/a-storypic-chef_WDZNW.pdf?up=1466611200
/writing-storypics/storypic-sing-dance-girl_SINGD.pdf?up=1466611200
/writing-storypics/storypic-pizza-veggies_PIZZA.pdf?up=1466611200
/writing-storypics/a-storypic-mouse-with-mouse_WDZRB.pdf?up=1466611200
/writing-storypics/a-storypic-travel-passport_WDZRD.pdf?up=1466611200
/writing-storypics/a-storypic-guitar-rockers_WDZRF.pdf?up=1466611200
/writing-storypics/a-storypic-scuba-dolphin_WDZRQ.pdf?up=1466611200
/writing-storypics/a-storypic-ladderman_WDZRN.pdf?up=1466611200
/writing-storypics/a-storypic-car-hands_WDZRR.pdf?up=1466611200
/writing-storypics/a-storypic-chase_WDZRT.pdf?up=1466611200
/writing-storypics/pic-spider-storypic_SPIDE.pdf?up=1466611200
/writing-storypics/a-storypic-elephant_WDZRW.pdf?up=1466611200
/writing-storypics/a-storypic-horserun_WDZRZ.pdf?up=1466611200
/writing-storypics/a-storypic-squirrel-bread_WDZTB.pdf?up=1466611200
/writing-storypics/a-stroypic-map_WDZTD.pdf?up=1466611200
/writing-storypics/pic-little-dog-big-cat-storypic_DOGCA.pdf?up=1466611200
/writing-storypics/a-storypic-three-bears_3BEAR.pdf?up=1492789165
/writing-storypics/a-storypic-three-bears_3BEAR.pdf?up=1492789165
/writing-storypics/a-storypic-three-pigs_3PIGS.pdf?up=1492789710
/writing-storypics/birthday-superhero-storypic_WDZTN.pdf?up=1466611200
/writing-storypics/christmas-elfstrike-storypic_WDZTR.pdf?up=1466611200
/writing-storypics/christmas-burpsack-storypic_WDZTT.pdf?up=1466611200
/writing-storypics/christmas-foxsanta-storypic_WDZTW.pdf?up=1466611200
/writing-storypics/christmas-reindeersack-storypic_WDZTZ.pdf?up=1466611200
/writing-storypics/christmas-videosanta-storypic_WDZWB.pdf?up=1466611200
/writing-storypics/dad-mug-storypic_DDMUG.pdf?up=1466611200
/writing-storypics/groundhogday-drive-storypic_WDZWD.pdf?up=1466611200
/writing-storypics/halloween-pumpkindog-storypic_WDZWF.pdf?up=1466611200
/writing-storypics/halloween-scaredkid-storypic_WDZWQ.pdf?up=1466611200
/writing-storypics/halloween-werewolf-storypic_WDZWM.pdf?up=1466611200
/writing-storypics/halloween-eyeballburger-storypic_WDZWN.pdf?up=1466611200
/writing-storypics/halloween-pumpkincarving-storypic_WDZWR.pdf?up=1466611200
/writing-storypics/hanukkah-menorah-storypic_WDZZD.pdf?up=1466611200
/writing-storypics/hanukkah-mousedreidel-storypic_WDZZF.pdf?up=1466611200
/writing-storypics/mom-hug-storypic_MOMMA.pdf?up=1466611200
/writing-storypics/stpatrick-leprechaunman-storypic_WDZZQ.pdf?up=1466611200
/writing-storypics/turkeysign-storypic_WDZZM.pdf?up=1466611200
/writing-storypics/turkey-story-fork_FORKS.pdf?up=1466611200
/writing-storypics/vegetarian-pic_WDZZR.pdf?up=1466611200
/writing-storypics/valentines-octo-storypic_WDZZT.pdf?up=1466611200
/writing-storypics/valentines-puppy-storypic_WDZZW.pdf?up=1466611200
/writing-storypics/winter-shovel-storypic_WDZZZ.pdf?up=1466611200
 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5:
cmd( 1):
processing https://www.downloadtest.com/writing-storypics/storypic-fishing_FISHI.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing-storypics/storypic-fishing_FISHI.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing-storypics/storypic-cat-pig-pool_CPOOL.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing-storypics/storypic-cat-pig-pool_CPOOL.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing-storypics/storypic-dragon_DRAGO.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing-storypics/storypic-kitten-tree_KTREE.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing-storypics/a-storypic-candy-corn_CORNS.pdf?up=1492767047
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing-storypics/free-puppy_PUPPY.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing-storypics/a-storypic-baby-sister_WAAAA.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing-storypics/storypic-swimming-pool_SPOOL.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing-storypics/storypic-mean-monster_MONST.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing-storypics/storypic-tablet-kid_TABLT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing-storypics/storypic-magician_MAGIC.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing-storypics/storypic-farm-horses_HORSE.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing-storypics/spring-kite-tree-storypic_TREES.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing-storypics/storypic-spooky-house-kids_HKIDS.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/olympics/story-pic-winter-olympics-daydream_SKIII.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing-storypics/storypic-car-wash_CWASH.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing-storypics/storypic-camp-fire_SMORE.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/backtoschool/story-pic-back-to-school_SCHOO.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing-storypics/storypic-icecream_ICECR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing-storypics/storypic-detective_DETEC.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing-storypics/storypic-butterfly-girl_BTFLY.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing-storypics/storypic-boat-boy_BOATS.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing-storypics/storypic-car-guy_CAGUY.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing-storypics/storypic-birthday-three_THREE.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/presidents-day/storypic-lincoln-school-play-story_STORY.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing-storypics/storypic-firefighters_FIREF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing-storypics/storypic-pets_PETSS.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/backtoschool/story-pic-apple-kid_APPLE.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing-storypics/storypic-laptop-girl_LAPTP.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing-storypics/a-storypic-tennis_WDZNT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing-storypics/storypic-mailman_MAILM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/drseuss/story-pic-seuss_UNICY.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing-storypics/storypic-bee-window_BWIND.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing-storypics/a-storypic-nervous-note-reader_NNOTE.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing-storypics/a-storypic-chef_WDZNW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing-storypics/storypic-sing-dance-girl_SINGD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing-storypics/storypic-pizza-veggies_PIZZA.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing-storypics/a-storypic-mouse-with-mouse_WDZRB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing-storypics/a-storypic-travel-passport_WDZRD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing-storypics/a-storypic-guitar-rockers_WDZRF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing-storypics/a-storypic-scuba-dolphin_WDZRQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing-storypics/a-storypic-ladderman_WDZRN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing-storypics/a-storypic-car-hands_WDZRR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing-storypics/a-storypic-chase_WDZRT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing-storypics/pic-spider-storypic_SPIDE.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing-storypics/a-storypic-elephant_WDZRW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing-storypics/a-storypic-horserun_WDZRZ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing-storypics/a-storypic-squirrel-bread_WDZTB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing-storypics/a-stroypic-map_WDZTD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing-storypics/pic-little-dog-big-cat-storypic_DOGCA.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing-storypics/a-storypic-three-bears_3BEAR.pdf?up=1492789165
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing-storypics/a-storypic-three-bears_3BEAR.pdf?up=1492789165
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing-storypics/a-storypic-three-pigs_3PIGS.pdf?up=1492789710
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing-storypics/birthday-superhero-storypic_WDZTN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing-storypics/christmas-elfstrike-storypic_WDZTR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing-storypics/christmas-burpsack-storypic_WDZTT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing-storypics/christmas-foxsanta-storypic_WDZTW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing-storypics/christmas-reindeersack-storypic_WDZTZ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing-storypics/christmas-videosanta-storypic_WDZWB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing-storypics/dad-mug-storypic_DDMUG.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing-storypics/groundhogday-drive-storypic_WDZWD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing-storypics/halloween-pumpkindog-storypic_WDZWF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing-storypics/halloween-scaredkid-storypic_WDZWQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing-storypics/halloween-werewolf-storypic_WDZWM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing-storypics/halloween-eyeballburger-storypic_WDZWN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing-storypics/halloween-pumpkincarving-storypic_WDZWR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing-storypics/hanukkah-menorah-storypic_WDZZD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing-storypics/hanukkah-mousedreidel-storypic_WDZZF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing-storypics/mom-hug-storypic_MOMMA.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing-storypics/stpatrick-leprechaunman-storypic_WDZZQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing-storypics/turkeysign-storypic_WDZZM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing-storypics/turkey-story-fork_FORKS.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing-storypics/vegetarian-pic_WDZZR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing-storypics/valentines-octo-storypic_WDZZT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing-storypics/valentines-puppy-storypic_WDZZW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing-storypics/winter-shovel-storypic_WDZZZ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/writing.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/writing.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/writing.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):
/writing/author-letter_WDZQR.pdf?up=1471854505
/writing/author-letter_WDZQR.pdf?up=1471854505
/writing/build-a-story_TAGSG.pdf?up=1558596275
/writing/build-a-story_TAGSG.pdf?up=1558596275
/writing/pirate-writing_WDZQT.pdf?up=1466611200
/writing/class-story_WDZQW.pdf?up=1466611200
/reading/braille-alphabet_LOUIS.pdf?up=1466611200
/reading-comp/4th-similes_WDZMB.pdf?up=1466611200
/reading-comp/4th-metaphors_WDZMD.pdf?up=1466611200
/reading-comp/5th-hyperbole_WDZMF.pdf?up=1466611200
/reading-comp/4th-onomatopoeia_WDZMQ.pdf?up=1466611200
/reading-comp/4th-personification_WDZMM.pdf?up=1466611200
/idioms/4th-idioms-poem_IPOEM.pdf?up=1466611200
/writing/writing-checklist_WDZMR.pdf?up=1468575735
/writing/writing-friend-checklist_WDZMT.pdf?up=1468576045
/writing/peer-editing_PEERE.pdf?up=1466611200
/writing/editing-wheel_WDZMW.pdf?up=1466611200
/writing/editing-wheel-color_WDZMZ.pdf?up=1466611200
/writing/address_WBQZM.pdf?up=1559113863
/writing/place-names_WDZNF.pdf?up=1466611200
/writing/writing-dates_WDZNQ.pdf?up=1561531104
/writing/thank-you-notes_WDZFR.pdf?up=1561531030
/writing/thank-you-notes_WDZFR.pdf?up=1561531030
/writing/thank-you-notes2_WDZFT.pdf?up=1561531030
/writing/letter-template_WDZFW.pdf?up=1466611200
/writing/letter-template2_WDZFZ.pdf?up=1466611200
/writing-dialog/writing-shoe-salesman_SHOES.pdf?up=1475571207
/writing-dialog/writing-cave-explorers_TORCH.pdf?up=1475571211
 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5:
cmd( 1):
processing https://www.downloadtest.com/writing/author-letter_WDZQR.pdf?up=1471854505
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing/author-letter_WDZQR.pdf?up=1471854505
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing/build-a-story_TAGSG.pdf?up=1558596275
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing/build-a-story_TAGSG.pdf?up=1558596275
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing/pirate-writing_WDZQT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing/class-story_WDZQW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading/braille-alphabet_LOUIS.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-similes_WDZMB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-metaphors_WDZMD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-hyperbole_WDZMF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-onomatopoeia_WDZMQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-personification_WDZMM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/idioms/4th-idioms-poem_IPOEM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing/writing-checklist_WDZMR.pdf?up=1468575735
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing/writing-friend-checklist_WDZMT.pdf?up=1468576045
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing/peer-editing_PEERE.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing/editing-wheel_WDZMW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing/editing-wheel-color_WDZMZ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing/address_WBQZM.pdf?up=1559113863
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing/place-names_WDZNF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing/writing-dates_WDZNQ.pdf?up=1561531104
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing/thank-you-notes_WDZFR.pdf?up=1561531030
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing/thank-you-notes_WDZFR.pdf?up=1561531030
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing/thank-you-notes2_WDZFT.pdf?up=1561531030
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing/letter-template_WDZFW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing/letter-template2_WDZFZ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing-dialog/writing-shoe-salesman_SHOES.pdf?up=1475571207
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing-dialog/writing-cave-explorers_TORCH.pdf?up=1475571211
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/full-ela.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/full-ela.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/full-ela.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):

 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
---Step5:
cmd( 1):
processing https://www.downloadtest.com
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/letters-phonics.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/letters-phonics.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/letters-phonics.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):

 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
---Step5:
cmd( 1):
processing https://www.downloadtest.com
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/letters-phonics-vowels.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/letters-phonics-vowels.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/letters-phonics-vowels.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):

 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
---Step5:
cmd( 1):
processing https://www.downloadtest.com
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/phonics-blends.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/phonics-blends.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/phonics-blends.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):

 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
---Step5:
cmd( 1):
processing https://www.downloadtest.com
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/phonics-digraphs.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/phonics-digraphs.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/phonics-digraphs.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):

 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
---Step5:
cmd( 1):
processing https://www.downloadtest.com
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/word-families.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/word-families.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/word-families.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):

 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
---Step5:
cmd( 1):
processing https://www.downloadtest.com
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/full-phonics.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/full-phonics.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/full-phonics.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):

 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
---Step5:
cmd( 1):
processing https://www.downloadtest.com
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/full-letters-alphabet.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/full-letters-alphabet.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/full-letters-alphabet.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):

 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
---Step5:
cmd( 1):
processing https://www.downloadtest.com
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/build-a-sentence.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/build-a-sentence.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/build-a-sentence.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):
/sentences-basic/make-sentences-sight-words-one_ONEON.pdf?up=1499935021
/sentences-basic/make-sentences-sight-words-one_ONEON.pdf?up=1499935021
/sentences-basic/make-sentences-sight-words-two_TWOTW.pdf?up=1499935022
/sentences-basic/make-sentences-sight-words-two_TWOTW.pdf?up=1499935022
/sentences-basic/make-sentences-sight-words-three_THREE.pdf?up=1499935022
/sentences-basic/build-a-sentence-3-we-run-fast_RFAST.pdf?up=1499934932
/sentences-basic/build-a-sentence-3-dad-is-tall_DADAD.pdf?up=1499934921
/sentences-basic/build-a-sentence-3-the-dog-barks_WOOFW.pdf?up=1499934931
/sentences-basic/build-a-sentence-3-sing-with-me_SONGS.pdf?up=1499934927
/sentences-basic/build-a-sentence-3-lets-go-home_GOHOM.pdf?up=1499934923
/sentences-basic/build-a-sentence-3-we-want-cake_CAKES.pdf?up=1499934933
/sentences-basic/build-a-sentence-3-the-cat-hides_HIDES.pdf?up=1499934930
/sentences-basic/build-a-sentence-3-mom-is-happy_HAPPY.pdf?up=1499934924
/sentences-basic/build-a-sentence-3-school-is-fun_SCFUN.pdf?up=1499934925
/sentences-basic/build-a-sentence-3-we-see-ducks_DUCKS.pdf?up=1499934933
/sentences-basic/build-a-sentence-3-close-the-door_CDOOR.pdf?up=1499934920
/sentences-basic/build-a-sentence-3-she-can-jump_JUMPU.pdf?up=1499934926
/sentences-basic/build-a-sentence-3-can-we-play_WPLAY.pdf?up=1499934919
/sentences-basic/build-a-sentence-3-how-are-you_WASUP.pdf?up=1499934922
/sentences-basic/build-a-sentence-3-the-bird-sings_SINGS.pdf?up=1499934929
/sentences-basic/build-a-sentence-3-a-fish-swims_SWIMS.pdf?up=1499934918
/sentences-basic/build-a-sentence-template-3_THREE.pdf?up=1499934993
/sentences-basic/build-a-sentence-4-duck_QUACK.pdf?up=1499934937
/sentences-basic/build-a-sentence-4-car_ZOOMC.pdf?up=1499934936
/sentences-basic/build-a-sentence-4-ball_BBALL.pdf?up=1499934935
/sentences-basic/build-a-sentence-4-moon_TMOON.pdf?up=1499934941
/sentences-basic/build-a-sentence-4-sing_LALAL.pdf?up=1499934944
/sentences-basic/build-a-sentence-4-he-saw-an-egg_EGGEG.pdf?up=1499934938
/sentences-basic/build-a-sentence-4-she-was-very-kind_VKIND.pdf?up=1499934943
/sentences-basic/build-a-sentence-4-my-dog-is-wet_WETDG.pdf?up=1499934942
/sentences-basic/build-a-sentence-4-what-time-is-it_WTIME.pdf?up=1499934950
/sentences-basic/build-a-sentence-4-that-cat-is-gray_GRAYC.pdf?up=1499934945
/sentences-basic/build-a-sentence-4-i-see-five-hats_5HATS.pdf?up=1499934938
/sentences-basic/build-a-sentence-4-the-fish-can-swim_SWIMF.pdf?up=1499934945
/sentences-basic/build-a-sentence-4-we-can-read-books_BOOKS.pdf?up=1499934948
/sentences-basic/build-a-sentence-4-the-pig-is-pink_PINKY.pdf?up=1499934946
/sentences-basic/build-a-sentence-4-this-is-my-toy_MYTOY.pdf?up=1499934947
/sentences-basic/build-a-sentence-4-words-template_TEMPL.pdf?up=1499934951
/sentences-basic/build-a-sentence-i-see-three-big-fish_FISHY.pdf?up=1499934982
/sentences-basic/build-a-sentence-i-see-three-big-fish_FISHY.pdf?up=1499934982
/sentences-basic/build-a-sentence-how-old-is-your-friend_SINGS.pdf?up=1499934978
/sentences-basic/build-a-sentence-look-at-the-blue-bird_BBIRD.pdf?up=1499934987
/sentences-basic/build-a-sentence-an-ant-has-six-legs_HELLO.pdf?up=1499934973
/sentences-basic/build-a-sentence-it-is-a-sunny-day_SUNND.pdf?up=1499934983
/sentences-basic/build-a-sentence-can-you-fly-the-kite_KITEK.pdf?up=1499934974
/sentences-basic/build-a-sentence-lets-walk-to-the-park_WPARK.pdf?up=1499934986
/sentences-basic/build-a-sentence-a-bird-can-fly-high_BIRDC.pdf?up=1499934972
/sentences-basic/build-a-sentence-can-you-see-the-pig_OINKS.pdf?up=1499934975
/sentences-basic/build-a-sentence-we-swam-in-the-pool_SPOOL.pdf?up=1499935000
/sentences-basic/build-a-sentence-i-can-tie-my-shoes_TIESH.pdf?up=1499934980
/sentences-basic/build-a-sentence-my-mom-sang-a-song_MOMSO.pdf?up=1499934992
/sentences-basic/build-a-sentence-my-dad-is-very-happy_HAPPY.pdf?up=1499934989
/sentences-basic/build-a-sentence-look-at-the-yellow-bird_BBIRD.pdf?up=1499934988
/sentences-basic/build-a-sentence-he-has-a-small-dog_SMALL.pdf?up=1499934976
/sentences-basic/build-a-sentence-i-broke-my-red-crayon_BROKE.pdf?up=1499934979
/sentences-basic/build-a-sentence-i-like-to-eat-fish_EFISH.pdf?up=1499934981
/sentences-basic/build-a-sentence-we-like-to-jump-rope_JUMPR.pdf?up=1499934999
/sentences-basic/build-a-sentence-where-did-my-friend-go_WHERE.pdf?up=1499935001
/sentences-basic/build-a-sentence-the-little-baby-drinks-milk_BABYM.pdf?up=1499934996
/sentences-basic/build-a-sentence-the-pig-can-roll-over_ROLLO.pdf?up=1499934997
/sentences-basic/build-a-sentence-it-is-dark-at-night_DARKN.pdf?up=1499934984
/sentences-basic/build-a-sentence-the-black-cat-saw-a-bird_BLACK.pdf?up=1499934994
/sentences-basic/build-a-sentence-we-saw-the-frog-jump_FROGJ.pdf?up=1499935000
/sentences-basic/build-a-sentence-he-like-to-eat-pizza_YUMMY.pdf?up=1499934977
/sentences-basic/build-a-sentence-the-brown-cow-eats-grass_COWGR.pdf?up=1499934995
/sentences-basic/build-a-sentence-lets-play-with-the-ball_PLAYB.pdf?up=1499934985
/sentences-basic/build-a-sentence-my-friend-likes-to-sing_FRIEN.pdf?up=1499934992
/sentences-basic/build-a-sentence-this-dog-can-play-ball_THISD.pdf?up=1499934998
/sentences-basic/build-a-sentence-5-word-template_5WORD.pdf?up=1499934952
/sentences-basic/build-a-sentence-6-i-will-ride-my-new-bike_BIKER.pdf?up=1499934953
/sentences-basic/build-a-sentence-6-it-is-going-to-rain-today_TODAY.pdf?up=1499934953
/sentences-basic/build-a-sentence-6-mr-brown-has-a-black-cow_BLCOW.pdf?up=1499934954
/sentences-basic/build-a-sentence-6-my-friend-and-i-ran-home_HOMEE.pdf?up=1499934955
/sentences-basic/build-a-sentence-6-my-little-dog-likes-to-play_DOGGG.pdf?up=1499934956
/sentences-basic/build-a-sentence-6-that-old-clock-does-not-work_CLOCK.pdf?up=1499934958
/sentences-basic/build-a-sentence-6-that-pig-rolls-in-the-mud_MUDPI.pdf?up=1499934959
/sentences-basic/build-a-sentence-6-the-dog-barks-at-the-cat_BARKS.pdf?up=1499934960
/sentences-basic/build-a-sentence-6-the-little-duck-ate-a-fish_FISHY.pdf?up=1499934961
/sentences-basic/build-a-sentence-6-the-tall-ship-will-sail-away_SHIPS.pdf?up=1499934962
/sentences-basic/build-a-sentence-6-the-yellow-bus-will-stop-here_YELLO.pdf?up=1499934963
/sentences-basic/build-a-sentence-6-they-sang-a-very-silly-song_SILLY.pdf?up=1499934964
/sentences-basic/build-a-sentence-6-they-went-to-the-pet-shop_PETSH.pdf?up=1499934965
/sentences-basic/build-a-sentence-6-wash-your-hands-in-the-sink_SINKH.pdf?up=1499934966
/sentences-basic/build-a-sentence-6-we-have-a-gift-for-you_GIFTS.pdf?up=1499934967
/sentences-basic/build-a-sentence-6-we-will-play-tag_PLAYT.pdf?up=1499934968
/sentences-basic/build-a-sentence-6-where-did-you-put-my-coat_COATW.pdf?up=1499934969
/sentences-basic/build-a-sentence-6-would-you-like-a-green-apple_GREEN.pdf?up=1499934970
/sentences-basic/build-a-sentence-6-you-can-have-this-toy-truck_TRUCC.pdf?up=1499934971
/sentences-basic/build-a-sentence-6-template_66666.pdf?up=1499934957
 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5:
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/make-sentences-sight-words-one_ONEON.pdf?up=1499935021
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/make-sentences-sight-words-one_ONEON.pdf?up=1499935021
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/make-sentences-sight-words-two_TWOTW.pdf?up=1499935022
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/make-sentences-sight-words-two_TWOTW.pdf?up=1499935022
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/make-sentences-sight-words-three_THREE.pdf?up=1499935022
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/build-a-sentence-3-we-run-fast_RFAST.pdf?up=1499934932
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/build-a-sentence-3-dad-is-tall_DADAD.pdf?up=1499934921
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/build-a-sentence-3-the-dog-barks_WOOFW.pdf?up=1499934931
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/build-a-sentence-3-sing-with-me_SONGS.pdf?up=1499934927
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/build-a-sentence-3-lets-go-home_GOHOM.pdf?up=1499934923
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/build-a-sentence-3-we-want-cake_CAKES.pdf?up=1499934933
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/build-a-sentence-3-the-cat-hides_HIDES.pdf?up=1499934930
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/build-a-sentence-3-mom-is-happy_HAPPY.pdf?up=1499934924
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/build-a-sentence-3-school-is-fun_SCFUN.pdf?up=1499934925
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/build-a-sentence-3-we-see-ducks_DUCKS.pdf?up=1499934933
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/build-a-sentence-3-close-the-door_CDOOR.pdf?up=1499934920
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/build-a-sentence-3-she-can-jump_JUMPU.pdf?up=1499934926
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/build-a-sentence-3-can-we-play_WPLAY.pdf?up=1499934919
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/build-a-sentence-3-how-are-you_WASUP.pdf?up=1499934922
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/build-a-sentence-3-the-bird-sings_SINGS.pdf?up=1499934929
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/build-a-sentence-3-a-fish-swims_SWIMS.pdf?up=1499934918
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/build-a-sentence-template-3_THREE.pdf?up=1499934993
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/build-a-sentence-4-duck_QUACK.pdf?up=1499934937
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/build-a-sentence-4-car_ZOOMC.pdf?up=1499934936
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/build-a-sentence-4-ball_BBALL.pdf?up=1499934935
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/build-a-sentence-4-moon_TMOON.pdf?up=1499934941
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/build-a-sentence-4-sing_LALAL.pdf?up=1499934944
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/build-a-sentence-4-he-saw-an-egg_EGGEG.pdf?up=1499934938
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/build-a-sentence-4-she-was-very-kind_VKIND.pdf?up=1499934943
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/build-a-sentence-4-my-dog-is-wet_WETDG.pdf?up=1499934942
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/build-a-sentence-4-what-time-is-it_WTIME.pdf?up=1499934950
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/build-a-sentence-4-that-cat-is-gray_GRAYC.pdf?up=1499934945
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/build-a-sentence-4-i-see-five-hats_5HATS.pdf?up=1499934938
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/build-a-sentence-4-the-fish-can-swim_SWIMF.pdf?up=1499934945
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/build-a-sentence-4-we-can-read-books_BOOKS.pdf?up=1499934948
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/build-a-sentence-4-the-pig-is-pink_PINKY.pdf?up=1499934946
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/build-a-sentence-4-this-is-my-toy_MYTOY.pdf?up=1499934947
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/build-a-sentence-4-words-template_TEMPL.pdf?up=1499934951
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/build-a-sentence-i-see-three-big-fish_FISHY.pdf?up=1499934982
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/build-a-sentence-i-see-three-big-fish_FISHY.pdf?up=1499934982
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/build-a-sentence-how-old-is-your-friend_SINGS.pdf?up=1499934978
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/build-a-sentence-look-at-the-blue-bird_BBIRD.pdf?up=1499934987
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/build-a-sentence-an-ant-has-six-legs_HELLO.pdf?up=1499934973
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/build-a-sentence-it-is-a-sunny-day_SUNND.pdf?up=1499934983
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/build-a-sentence-can-you-fly-the-kite_KITEK.pdf?up=1499934974
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/build-a-sentence-lets-walk-to-the-park_WPARK.pdf?up=1499934986
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/build-a-sentence-a-bird-can-fly-high_BIRDC.pdf?up=1499934972
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/build-a-sentence-can-you-see-the-pig_OINKS.pdf?up=1499934975
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/build-a-sentence-we-swam-in-the-pool_SPOOL.pdf?up=1499935000
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/build-a-sentence-i-can-tie-my-shoes_TIESH.pdf?up=1499934980
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/build-a-sentence-my-mom-sang-a-song_MOMSO.pdf?up=1499934992
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/build-a-sentence-my-dad-is-very-happy_HAPPY.pdf?up=1499934989
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/build-a-sentence-look-at-the-yellow-bird_BBIRD.pdf?up=1499934988
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/build-a-sentence-he-has-a-small-dog_SMALL.pdf?up=1499934976
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/build-a-sentence-i-broke-my-red-crayon_BROKE.pdf?up=1499934979
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/build-a-sentence-i-like-to-eat-fish_EFISH.pdf?up=1499934981
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/build-a-sentence-we-like-to-jump-rope_JUMPR.pdf?up=1499934999
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/build-a-sentence-where-did-my-friend-go_WHERE.pdf?up=1499935001
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/build-a-sentence-the-little-baby-drinks-milk_BABYM.pdf?up=1499934996
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/build-a-sentence-the-pig-can-roll-over_ROLLO.pdf?up=1499934997
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/build-a-sentence-it-is-dark-at-night_DARKN.pdf?up=1499934984
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/build-a-sentence-the-black-cat-saw-a-bird_BLACK.pdf?up=1499934994
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/build-a-sentence-we-saw-the-frog-jump_FROGJ.pdf?up=1499935000
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/build-a-sentence-he-like-to-eat-pizza_YUMMY.pdf?up=1499934977
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/build-a-sentence-the-brown-cow-eats-grass_COWGR.pdf?up=1499934995
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/build-a-sentence-lets-play-with-the-ball_PLAYB.pdf?up=1499934985
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/build-a-sentence-my-friend-likes-to-sing_FRIEN.pdf?up=1499934992
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/build-a-sentence-this-dog-can-play-ball_THISD.pdf?up=1499934998
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/build-a-sentence-5-word-template_5WORD.pdf?up=1499934952
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/build-a-sentence-6-i-will-ride-my-new-bike_BIKER.pdf?up=1499934953
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/build-a-sentence-6-it-is-going-to-rain-today_TODAY.pdf?up=1499934953
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/build-a-sentence-6-mr-brown-has-a-black-cow_BLCOW.pdf?up=1499934954
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/build-a-sentence-6-my-friend-and-i-ran-home_HOMEE.pdf?up=1499934955
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/build-a-sentence-6-my-little-dog-likes-to-play_DOGGG.pdf?up=1499934956
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/build-a-sentence-6-that-old-clock-does-not-work_CLOCK.pdf?up=1499934958
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/build-a-sentence-6-that-pig-rolls-in-the-mud_MUDPI.pdf?up=1499934959
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/build-a-sentence-6-the-dog-barks-at-the-cat_BARKS.pdf?up=1499934960
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/build-a-sentence-6-the-little-duck-ate-a-fish_FISHY.pdf?up=1499934961
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/build-a-sentence-6-the-tall-ship-will-sail-away_SHIPS.pdf?up=1499934962
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/build-a-sentence-6-the-yellow-bus-will-stop-here_YELLO.pdf?up=1499934963
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/build-a-sentence-6-they-sang-a-very-silly-song_SILLY.pdf?up=1499934964
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/build-a-sentence-6-they-went-to-the-pet-shop_PETSH.pdf?up=1499934965
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/build-a-sentence-6-wash-your-hands-in-the-sink_SINKH.pdf?up=1499934966
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/build-a-sentence-6-we-have-a-gift-for-you_GIFTS.pdf?up=1499934967
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/build-a-sentence-6-we-will-play-tag_PLAYT.pdf?up=1499934968
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/build-a-sentence-6-where-did-you-put-my-coat_COATW.pdf?up=1499934969
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/build-a-sentence-6-would-you-like-a-green-apple_GREEN.pdf?up=1499934970
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/build-a-sentence-6-you-can-have-this-toy-truck_TRUCC.pdf?up=1499934971
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/build-a-sentence-6-template_66666.pdf?up=1499934957
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/full-sight-words.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/full-sight-words.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/full-sight-words.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):

 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
---Step5:
cmd( 1):
processing https://www.downloadtest.com
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/sight-words-individual.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/sight-words-individual.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/sight-words-individual.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):

 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
---Step5:
cmd( 1):
processing https://www.downloadtest.com
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/full-early-literacy.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/full-early-literacy.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/full-early-literacy.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):

 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
---Step5:
cmd( 1):
processing https://www.downloadtest.com
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/nouns.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/nouns.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/nouns.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):
/nouns/nouns-cut-outs_WBTMB.pdf?up=1503308997
/nouns/nouns-cut-outs_WBTMB.pdf?up=1503308997
/nouns/nouns-cut-outs-adv_WBTMD.pdf?up=1503309000
/nouns/nouns-cut-outs-adv_WBTMD.pdf?up=1503309000
/nouns/noun-pennant-sentences_FFRAS.pdf?up=1530185510
/nouns/nouns-commonproper_WBTMQ.pdf?up=1466611200
/nouns/clown-is-a-noun-poem_CLOWN.pdf?up=1504803573
/nouns/nouns-capital_WBTMM.pdf?up=1559043017
/grammar/common-and-proper-nouns_WBTMN.pdf?up=1466611200
/nouns/nouns-cut-outs_GHWKS.pdf?up=1466611200
/nouns/sort-concrete-abstract_CCCCC.pdf?up=1466611200
/nouns/concrete-abstract-noun-sentences_VFRDF.pdf?up=1466611200
/nouns/concrete-abstract-noun-search_NOUNS.pdf?up=1466611200
/nouns/nouns-collective_COLLEC.pdf?up=1466611200
/nouns/nouns-collective-match_MATCH.pdf?up=1466611200
/nouns/plural-s-es_SDISH.pdf?up=1466611200
/nouns/plural-s-es_SDISH.pdf?up=1466611200
/nouns/nouns-singularandplural_WBTFW.pdf?up=1466611200
/nouns/scoot-plural-nouns_NAHJU.pdf?up=1466611200
/nouns/plural-noun-rules-worksheet_RULES.pdf?up=1466611200
/nouns/nouns-singularandplural2_WBTFZ.pdf?up=1558436893
/nouns/nouns-singularandplural3_WBTQB.pdf?up=1558436892
/nouns/thinking-about-nouns_TNOUN.pdf?up=1475233553
/grammar/noun-review_WBTQF.pdf?up=1466611200
 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5:
cmd( 1):
processing https://www.downloadtest.com/nouns/nouns-cut-outs_WBTMB.pdf?up=1503308997
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/nouns/nouns-cut-outs_WBTMB.pdf?up=1503308997
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/nouns/nouns-cut-outs-adv_WBTMD.pdf?up=1503309000
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/nouns/nouns-cut-outs-adv_WBTMD.pdf?up=1503309000
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/nouns/noun-pennant-sentences_FFRAS.pdf?up=1530185510
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/nouns/nouns-commonproper_WBTMQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/nouns/clown-is-a-noun-poem_CLOWN.pdf?up=1504803573
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/nouns/nouns-capital_WBTMM.pdf?up=1559043017
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/grammar/common-and-proper-nouns_WBTMN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/nouns/nouns-cut-outs_GHWKS.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/nouns/sort-concrete-abstract_CCCCC.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/nouns/concrete-abstract-noun-sentences_VFRDF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/nouns/concrete-abstract-noun-search_NOUNS.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/nouns/nouns-collective_COLLEC.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/nouns/nouns-collective-match_MATCH.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/nouns/plural-s-es_SDISH.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/nouns/plural-s-es_SDISH.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/nouns/nouns-singularandplural_WBTFW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/nouns/scoot-plural-nouns_NAHJU.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/nouns/plural-noun-rules-worksheet_RULES.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/nouns/nouns-singularandplural2_WBTFZ.pdf?up=1558436893
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/nouns/nouns-singularandplural3_WBTQB.pdf?up=1558436892
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/nouns/thinking-about-nouns_TNOUN.pdf?up=1475233553
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/grammar/noun-review_WBTQF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/actionverbs.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/actionverbs.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/actionverbs.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):
/actionverbs/action-verbs_WDWZM.pdf?up=1466611200
/actionverbs/action-verbs_WDWZM.pdf?up=1466611200
/actionverbs/action-verbs2_WDWZN.pdf?up=1466611200
/actionverbs/action-verbs2_WDWZN.pdf?up=1466611200
/actionverbs/play-poem-actionverbs_WDWZR.pdf?up=1466611200
/actionverbs/my-dog-named-verb-poem-actionverbs_FETCH.pdf?up=1504803288
/actionverbs/task-cards-action-verbs_FNJKF.pdf?up=1581509715
/actionverbs/actionverbs1_WDWZW.pdf?up=1466611200
/actionverbs/actionverbs2_WDWZZ.pdf?up=1466611200
/actionverbs/verbs-action_ACTIO.pdf?up=1466611200
/actionverbs/verbs-choose-the-action_SNRLM.pdf?up=1466611200
/actionverbs/thinking-about-action-verbs_AVERB.pdf?up=1475236729
/actionverbs/verbtenses_WDZBF.pdf?up=1466611200
/actionverbs/verbtenses-chart_WDZBR.pdf?up=1466611200
/verbs/scoot-verb-tenses_VFATG.pdf?up=1466611200
/actionverbs/action-verb-tense-color_WDZBQ.pdf?up=1572692291
/actionverbs/regularverbs2_WDZBM.pdf?up=1558520606
/actionverbs/regularverbs_WDZBN.pdf?up=1558520605
/actionverbs/subject-verb-agreement_WDZBT.pdf?up=1558520608
/actionverbs/irregularpasttenseverbmatch_WDZBW.pdf?up=1466611200
/actionverbs/irregularverbspasttense_WDZBZ.pdf?up=1466611200
/actionverbs/irregularverbspastparticiple_WDZDB.pdf?up=1466611200
/prefixes-suffixes/adding-ed_ADDED.pdf?up=1466611200
/prefixes-suffixes/adding-ed-ing_EDING.pdf?up=1466611200
/actionverbs/helpingverbs-basic_HVERB.pdf?up=1466611200
/actionverbs/helpingverbs-basic_HVERB.pdf?up=1466611200
/actionverbs/helpingverbs_WDZDF.pdf?up=1466611200
/actionverbs/helpingverbs-advanced_HVERB.pdf?up=1466611200
/actionverbs/linking-action-two-kinds-of-verbs_2VERB.pdf?up=1466611200
/actionverbs/linkingverbs_WDZDM.pdf?up=1466611200
/actionverbs/linkingvsactionverbs2_WDZDN.pdf?up=1466611200
/actionverbs/linkingvsactionverbs_WDZDR.pdf?up=1466611200
/actionverbs/verbsx_WDZDT.pdf?up=1466611200
/actionverbs/action-verbs-and-linking-verbs_AVLV1.pdf?up=1466611200
/actionverbs/find-the-linking-verbs_FINDL.pdf?up=1466611200
/actionverbs/action-or-linking_ALINK.pdf?up=1466611200
/actionverbs/linking-verb-complements_COMPL.pdf?up=1466611200
/commonly-confused-words/seen-saw_SAWSE.pdf?up=1484135824
/actionverbs/lie-lay_WDZDZ.pdf?up=1466611200
/actionverbs/animal-sounds_TRWSD.pdf?up=1466611200
 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5:
cmd( 1):
processing https://www.downloadtest.com/actionverbs/action-verbs_WDWZM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/actionverbs/action-verbs_WDWZM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/actionverbs/action-verbs2_WDWZN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/actionverbs/action-verbs2_WDWZN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/actionverbs/play-poem-actionverbs_WDWZR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/actionverbs/my-dog-named-verb-poem-actionverbs_FETCH.pdf?up=1504803288
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/actionverbs/task-cards-action-verbs_FNJKF.pdf?up=1581509715
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/actionverbs/actionverbs1_WDWZW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/actionverbs/actionverbs2_WDWZZ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/actionverbs/verbs-action_ACTIO.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/actionverbs/verbs-choose-the-action_SNRLM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/actionverbs/thinking-about-action-verbs_AVERB.pdf?up=1475236729
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/actionverbs/verbtenses_WDZBF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/actionverbs/verbtenses-chart_WDZBR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/verbs/scoot-verb-tenses_VFATG.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/actionverbs/action-verb-tense-color_WDZBQ.pdf?up=1572692291
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/actionverbs/regularverbs2_WDZBM.pdf?up=1558520606
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/actionverbs/regularverbs_WDZBN.pdf?up=1558520605
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/actionverbs/subject-verb-agreement_WDZBT.pdf?up=1558520608
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/actionverbs/irregularpasttenseverbmatch_WDZBW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/actionverbs/irregularverbspasttense_WDZBZ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/actionverbs/irregularverbspastparticiple_WDZDB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/prefixes-suffixes/adding-ed_ADDED.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/prefixes-suffixes/adding-ed-ing_EDING.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/actionverbs/helpingverbs-basic_HVERB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/actionverbs/helpingverbs-basic_HVERB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/actionverbs/helpingverbs_WDZDF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/actionverbs/helpingverbs-advanced_HVERB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/actionverbs/linking-action-two-kinds-of-verbs_2VERB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/actionverbs/linkingverbs_WDZDM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/actionverbs/linkingvsactionverbs2_WDZDN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/actionverbs/linkingvsactionverbs_WDZDR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/actionverbs/verbsx_WDZDT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/actionverbs/action-verbs-and-linking-verbs_AVLV1.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/actionverbs/find-the-linking-verbs_FINDL.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/actionverbs/action-or-linking_ALINK.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/actionverbs/linking-verb-complements_COMPL.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/commonly-confused-words/seen-saw_SAWSE.pdf?up=1484135824
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/actionverbs/lie-lay_WDZDZ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/actionverbs/animal-sounds_TRWSD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/adjectives.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/adjectives.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/adjectives.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):
/adjectives/alienadjectives_WBFWB.pdf?up=1558420390
/adjectives/alienadjectives_WBFWB.pdf?up=1558420390
/adjectives/adjectives1_WBFWD.pdf?up=1466611200
/adjectives/adjectives1_WBFWD.pdf?up=1466611200
/adjectives/adjectives-circleunderline_WBFWF.pdf?up=1558678423
/adjectives/adjectives_WBFWQ.pdf?up=1497370698
/adjectives/adjectives2_WBFWM.pdf?up=1558596034
/adjectives/adj1_WBFWN.pdf?up=1466611200
/adjectives/adj2_WBFWR.pdf?up=1558596029
/adjectives/kikis-kitty-adjectives_WBFWT.pdf?up=1558596029
/adjectives/addingadjectives_WBFWW.pdf?up=1558596033
/adjectives/thinking-about-adjectives_THADJ.pdf?up=1475224949
/adjectives/the-adjective-store-poem_ADJEC.pdf?up=1510902364
/adjectives/articles-a-an_AAANN.pdf?up=1466611200
/adjectives/sentences-fix-articles-a-an_NNAAA.pdf?up=1494926970
/adjectives/articles-a-an-the_THEAN.pdf?up=1466611200
/adjectives/aarticles-a-an_WBFZB.pdf?up=1558678423
/adjectives/articles-a-an-advanced_ADVAN.pdf?up=1466611200
/adjectives/adjective-adverb_WBFZF.pdf?up=1466611200
/adjectives/adjectives-erest_WBFZM.pdf?up=1466611200
/adjectives/adjectives-erest_WBFZM.pdf?up=1466611200
/adjectives/adjectives-moremost_WBFZN.pdf?up=1558420395
/adjectives/changingadjectives_WBFZR.pdf?up=1558678422
/adjectives/comparitiveandsuperlative_WBFZT.pdf?up=1466611200
/adjectives/fewer-less_WBFZZ.pdf?up=1558596033
/prepositions/prepositional-phrases-adjectives_PPADJ.pdf?up=1466611200
/prepositions/prepositional-phrases-adjectives-and-adverbs_ADJAD.pdf?up=1466611200
 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5:
cmd( 1):
processing https://www.downloadtest.com/adjectives/alienadjectives_WBFWB.pdf?up=1558420390
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/adjectives/alienadjectives_WBFWB.pdf?up=1558420390
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/adjectives/adjectives1_WBFWD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/adjectives/adjectives1_WBFWD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/adjectives/adjectives-circleunderline_WBFWF.pdf?up=1558678423
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/adjectives/adjectives_WBFWQ.pdf?up=1497370698
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/adjectives/adjectives2_WBFWM.pdf?up=1558596034
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/adjectives/adj1_WBFWN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/adjectives/adj2_WBFWR.pdf?up=1558596029
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/adjectives/kikis-kitty-adjectives_WBFWT.pdf?up=1558596029
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/adjectives/addingadjectives_WBFWW.pdf?up=1558596033
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/adjectives/thinking-about-adjectives_THADJ.pdf?up=1475224949
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/adjectives/the-adjective-store-poem_ADJEC.pdf?up=1510902364
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/adjectives/articles-a-an_AAANN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/adjectives/sentences-fix-articles-a-an_NNAAA.pdf?up=1494926970
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/adjectives/articles-a-an-the_THEAN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/adjectives/aarticles-a-an_WBFZB.pdf?up=1558678423
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/adjectives/articles-a-an-advanced_ADVAN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/adjectives/adjective-adverb_WBFZF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/adjectives/adjectives-erest_WBFZM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/adjectives/adjectives-erest_WBFZM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/adjectives/adjectives-moremost_WBFZN.pdf?up=1558420395
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/adjectives/changingadjectives_WBFZR.pdf?up=1558678422
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/adjectives/comparitiveandsuperlative_WBFZT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/adjectives/fewer-less_WBFZZ.pdf?up=1558596033
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/prepositions/prepositional-phrases-adjectives_PPADJ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/prepositions/prepositional-phrases-adjectives-and-adverbs_ADJAD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/adverbs.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/adverbs.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/adverbs.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):
/grammar/adverbs_WBQBQ.pdf?up=1466611200
/grammar/adverbs_WBQBQ.pdf?up=1466611200
/adverbs/adverb-howwhenwhere_WBQBM.pdf?up=1466611200
/adverbs/adverb-howwhenwhere_WBQBM.pdf?up=1466611200
/adverbs/adverbs2_WBQBN.pdf?up=1569215571
/poetry/adverb-poem_WRITE.pdf?up=1466611200
/adverbs/stanley-vs-the-storm-adverbs_FRIGH.pdf?up=1510904128
/adverbs/thinking-about-adverbs_TADVE.pdf?up=1475236610
/adverbs/adverbs1_WBQBR.pdf?up=1466611200
/adverbs/adverbworksheetX_WBQBT.pdf?up=1466611200
/adverbs/adverb-how_WBQDD.pdf?up=1466611200
/adjectives/adjective-adverb_WBQBW.pdf?up=1466611200
/adjectives/adjective-adverb_WBQBW.pdf?up=1466611200
/prepositions/prepositional-phrases-adverbs_ADADV.pdf?up=1466611200
/prepositions/prepositional-phrases-adjectives-and-adverbs_ADJAD.pdf?up=1466611200
 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5:
cmd( 1):
processing https://www.downloadtest.com/grammar/adverbs_WBQBQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/grammar/adverbs_WBQBQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/adverbs/adverb-howwhenwhere_WBQBM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/adverbs/adverb-howwhenwhere_WBQBM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/adverbs/adverbs2_WBQBN.pdf?up=1569215571
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/poetry/adverb-poem_WRITE.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/adverbs/stanley-vs-the-storm-adverbs_FRIGH.pdf?up=1510904128
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/adverbs/thinking-about-adverbs_TADVE.pdf?up=1475236610
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/adverbs/adverbs1_WBQBR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/adverbs/adverbworksheetX_WBQBT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/adverbs/adverb-how_WBQDD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/adjectives/adjective-adverb_WBQBW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/adjectives/adjective-adverb_WBQBW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/prepositions/prepositional-phrases-adverbs_ADADV.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/prepositions/prepositional-phrases-adjectives-and-adverbs_ADJAD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/pronoun.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/pronoun.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/pronoun.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):
/grammar/pronouns_WDDRM.pdf?up=1466611200
/grammar/pronouns_WDDRM.pdf?up=1466611200
/pronouns/thinking-about-pronouns_PNOUN.pdf?up=1475217210
/pronouns/thinking-about-pronouns_PNOUN.pdf?up=1475217210
/pronouns/pronouns3_WDDRN.pdf?up=1466611200
/pronouns/pronouns4_WDDRR.pdf?up=1558507178
/pronouns/pronouns1_WDDRT.pdf?up=1466611200
/pronouns/relative-pronouns_RELAT.pdf?up=1551094414
/pronouns/pronouns2_WDDTB.pdf?up=1558420345
/pronouns/pronouns8_WDDRW.pdf?up=1558420343
/pronouns/pronouns5_WDDRZ.pdf?up=1558420344
/pronouns/pronouns7_WDDTD.pdf?up=1558420346
/pronouns/pronouns6_WDDTM.pdf?up=1466611200
/pronouns/pronouns6_WDDTM.pdf?up=1466611200
/pronouns/more-i-me-pronouns_MEMEI.pdf?up=1466611200
/pronouns/i-me-pronouns_MEIII.pdf?up=1466611200
/pronouns/this-that-these-those_WDDTN.pdf?up=1558420342
 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5:
cmd( 1):
processing https://www.downloadtest.com/grammar/pronouns_WDDRM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/grammar/pronouns_WDDRM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/pronouns/thinking-about-pronouns_PNOUN.pdf?up=1475217210
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/pronouns/thinking-about-pronouns_PNOUN.pdf?up=1475217210
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/pronouns/pronouns3_WDDRN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/pronouns/pronouns4_WDDRR.pdf?up=1558507178
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/pronouns/pronouns1_WDDRT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/pronouns/relative-pronouns_RELAT.pdf?up=1551094414
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/pronouns/pronouns2_WDDTB.pdf?up=1558420345
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/pronouns/pronouns8_WDDRW.pdf?up=1558420343
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/pronouns/pronouns5_WDDRZ.pdf?up=1558420344
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/pronouns/pronouns7_WDDTD.pdf?up=1558420346
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/pronouns/pronouns6_WDDTM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/pronouns/pronouns6_WDDTM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/pronouns/more-i-me-pronouns_MEMEI.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/pronouns/i-me-pronouns_MEIII.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/pronouns/this-that-these-those_WDDTN.pdf?up=1558420342
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/punctuation.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/punctuation.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/punctuation.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):
/punctuation/punctuation_WBZZM.pdf?up=1466611200
/punctuation/punctuation_WBZZM.pdf?up=1466611200
/punctuation/punctuation-periodquestionmark_WBZWZ.pdf?up=1558520732
/punctuation/punctuation-periodquestionmark_WBZWZ.pdf?up=1558520732
/punctuation/punctuation-end_WBZZB.pdf?up=1466611200
/punctuation/punctuation101708_WBZZD.pdf?up=1466611200
/punctuation/punctuation-cut-and-paste1_WBZZF.pdf?up=1558520730
/punctuation/punctuation-cut-and-paste2_WBZZQ.pdf?up=1558420215
/punctuation/punctuation-mark-identification_WBZZN.pdf?up=1466611200
/punctuation/punctuation-markidentification2_WBZZR.pdf?up=1558520733
/punctuation/punctuation-unscramble_WBZZT.pdf?up=1466611200
/punctuation/cap-andpunc-envelopes_WDBBN.pdf?up=1466611200
/punctuation/cap-andpunc-envelopes_WDBBN.pdf?up=1466611200
/punctuation/writing-dates_WDBBR.pdf?up=1481810641
/punctuation/yes-no-comma_WDBBT.pdf?up=1558520732
/punctuation/punctuation-commas1_WDBBW.pdf?up=1558520732
/punctuation/punctuation-commas2_WDBBZ.pdf?up=1523268814
/punctuation/ap_WDBDF.pdf?up=1466611200
 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5:
cmd( 1):
processing https://www.downloadtest.com/punctuation/punctuation_WBZZM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/punctuation/punctuation_WBZZM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/punctuation/punctuation-periodquestionmark_WBZWZ.pdf?up=1558520732
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/punctuation/punctuation-periodquestionmark_WBZWZ.pdf?up=1558520732
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/punctuation/punctuation-end_WBZZB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/punctuation/punctuation101708_WBZZD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/punctuation/punctuation-cut-and-paste1_WBZZF.pdf?up=1558520730
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/punctuation/punctuation-cut-and-paste2_WBZZQ.pdf?up=1558420215
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/punctuation/punctuation-mark-identification_WBZZN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/punctuation/punctuation-markidentification2_WBZZR.pdf?up=1558520733
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/punctuation/punctuation-unscramble_WBZZT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/punctuation/cap-andpunc-envelopes_WDBBN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/punctuation/cap-andpunc-envelopes_WDBBN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/punctuation/writing-dates_WDBBR.pdf?up=1481810641
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/punctuation/yes-no-comma_WDBBT.pdf?up=1558520732
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/punctuation/punctuation-commas1_WDBBW.pdf?up=1558520732
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/punctuation/punctuation-commas2_WDBBZ.pdf?up=1523268814
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/punctuation/ap_WDBDF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/syllables.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/syllables.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/syllables.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):
/syllables/syllables6_WDWNN.pdf?up=1466611200
/syllables/syllables6_WDWNN.pdf?up=1466611200
/syllables/syllable-sea_SEASE.pdf?up=1466611200
/syllables/syllable-sea_SEASE.pdf?up=1466611200
/syllables/syllable-insects_INSEC.pdf?up=1466611200
/syllables/syllable-dinosaurs_DINOS.pdf?up=1466611200
/syllables/syllables2_WDWNR.pdf?up=1466611200
/syllables/syllables3_WDWNT.pdf?up=1466611200
/syllables/syllables5_WDWNW.pdf?up=1466611200
/syllables/syllables9_WDWNZ.pdf?up=1466611200
/syllables/cootie-catcher-syllables_YWENK.pdf?up=1466611200
/syllables/syllables10_WDWRB.pdf?up=1466611200
/syllables/syllables8_WDWRF.pdf?up=1472489983
/syllables/syllables8_WDWRF.pdf?up=1472489983
 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5:
cmd( 1):
processing https://www.downloadtest.com/syllables/syllables6_WDWNN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/syllables/syllables6_WDWNN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/syllables/syllable-sea_SEASE.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/syllables/syllable-sea_SEASE.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/syllables/syllable-insects_INSEC.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/syllables/syllable-dinosaurs_DINOS.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/syllables/syllables2_WDWNR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/syllables/syllables3_WDWNT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/syllables/syllables5_WDWNW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/syllables/syllables9_WDWNZ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/syllables/cootie-catcher-syllables_YWENK.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/syllables/syllables10_WDWRB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/syllables/syllables8_WDWRF.pdf?up=1472489983
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/syllables/syllables8_WDWRF.pdf?up=1472489983
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/subjectpredicate.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/subjectpredicate.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/subjectpredicate.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):
/subjectpredicate/subject-predicate-word-box_WDWMN.pdf?up=1466611200
/subjectpredicate/subject-predicate-word-box_WDWMN.pdf?up=1466611200
/subjectpredicate/subject-predicate-cut-and-glue_WDWMR.pdf?up=1558595981
/subjectpredicate/subject-predicate-cut-and-glue_WDWMR.pdf?up=1558595981
/subjectpredicate/subject-predicate-a_WDWMT.pdf?up=1466611200
/subjectpredicate/subject-predicate-b_WDWMW.pdf?up=1466611200
/subjectpredicate/subject-predicate-c_WDWNB.pdf?up=1466611200
/subjectpredicate/predicates-subjects-d_WDWND.pdf?up=1466611200
/subjectpredicate/predicates-subjects-e_SUBJE.pdf?up=1466611200
/subjectpredicate/simple-subject-predicate_SIMPL.pdf?up=1466611200
/subjectpredicate/simple-subject-predicate_SIMPL.pdf?up=1466611200
/subjectpredicate/simple-subjects_WDWNF.pdf?up=1466611200
/subjectpredicate/simple-subjects-unscramble_UNSCR.pdf?up=1466611200
/subjectpredicate/simple-predicate_WDWNQ.pdf?up=1466611200
/subjectpredicate/simple-s-and-p_SIMPL.pdf?up=1466611200
 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5:
cmd( 1):
processing https://www.downloadtest.com/subjectpredicate/subject-predicate-word-box_WDWMN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/subjectpredicate/subject-predicate-word-box_WDWMN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/subjectpredicate/subject-predicate-cut-and-glue_WDWMR.pdf?up=1558595981
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/subjectpredicate/subject-predicate-cut-and-glue_WDWMR.pdf?up=1558595981
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/subjectpredicate/subject-predicate-a_WDWMT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/subjectpredicate/subject-predicate-b_WDWMW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/subjectpredicate/subject-predicate-c_WDWNB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/subjectpredicate/predicates-subjects-d_WDWND.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/subjectpredicate/predicates-subjects-e_SUBJE.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/subjectpredicate/simple-subject-predicate_SIMPL.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/subjectpredicate/simple-subject-predicate_SIMPL.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/subjectpredicate/simple-subjects_WDWNF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/subjectpredicate/simple-subjects-unscramble_UNSCR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/subjectpredicate/simple-predicate_WDWNQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/subjectpredicate/simple-s-and-p_SIMPL.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/grammar.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/grammar.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/grammar.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):

 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
---Step5:
cmd( 1):
processing https://www.downloadtest.com
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/spelling-level-a.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/spelling-level-a.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/spelling-level-a.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):

 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
---Step5:
cmd( 1):
processing https://www.downloadtest.com
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/spelling-level-b.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/spelling-level-b.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/spelling-level-b.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):

 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
---Step5:
cmd( 1):
processing https://www.downloadtest.com
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/spelling-level-c.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/spelling-level-c.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/spelling-level-c.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):

 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
---Step5:
cmd( 1):
processing https://www.downloadtest.com
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/spelling-level-d.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/spelling-level-d.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/spelling-level-d.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):

 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
---Step5:
cmd( 1):
processing https://www.downloadtest.com
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/spelling-level-e.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/spelling-level-e.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/spelling-level-e.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):

 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
---Step5:
cmd( 1):
processing https://www.downloadtest.com
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/full-spelling.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/full-spelling.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/full-spelling.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):

 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
---Step5:
cmd( 1):
processing https://www.downloadtest.com
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/book-bunnicula.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/book-bunnicula.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/book-bunnicula.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):
/books/bunnicula-questions-1-2_ONE22.pdf?up=1466611200
/books/bunnicula-questions-1-2_ONE22.pdf?up=1466611200
/books/bunnicula-summarize-picture-1_BUNNI.pdf?up=1466611200
/books/bunnicula-summarize-picture-1_BUNNI.pdf?up=1466611200
/books/bunnicula-thinking-question-1-2_UNUSU.pdf?up=1466611200
/books/bunnicula-questions-3-4_33344.pdf?up=1466611200
/books/bunnicula-summarize-picture-3_PICTU.pdf?up=1466611200
/books/bunnicula-thinking-question-3-4_FUNNY.pdf?up=1466611200
/books/bunnicula-questions-5-6_FIVES.pdf?up=1466611200
/books/bunnicula-summarize-picture-6_STEAK.pdf?up=1466611200
/books/bunnicula-thinking-question-5-6_SMART.pdf?up=1466611200
/books/bunnicula-questions-7-9_SNINE.pdf?up=1466611200
/books/bunnicula-summarize-picture-8_SALAD.pdf?up=1466611200
/books/bunnicula-thinking-question-7-9_SEVEN.pdf?up=1466611200
/books/bunnicula-who-said-that_WHOSA.pdf?up=1486376756
/books/bunnicula-who-said-that_WHOSA.pdf?up=1486376756
/books/bunnicula-projects_PROJE.pdf?up=1466611200
/books/bunnicula-character-list_CHARL.pdf?up=1466611200
/books/bunnicula-wordsearch_WORDS.pdf?up=1466611200
/books/bunnicula-bookmarks_PWIXX.pdf?up=1466611200
/books/bunnicula-lit-circles-question-leader_QLEAD.pdf?up=1466611200
/books/bunnicula-lit-circles-summary_SUMMA.pdf?up=1466611200
/books/bunnicula-lit-circles-illustrator_DRAWB.pdf?up=1466611200
/books/bunnicula-lit-circles-word-wizard_WIZAR.pdf?up=1466611200
/books/bunnicula-lit-circles-real-life-connector_CONNE.pdf?up=1466611200
/books/bunnicula-lit-circles-cover_LITCI.pdf?up=1466611200
 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5:
cmd( 1):
processing https://www.downloadtest.com/books/bunnicula-questions-1-2_ONE22.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/books/bunnicula-questions-1-2_ONE22.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/books/bunnicula-summarize-picture-1_BUNNI.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/books/bunnicula-summarize-picture-1_BUNNI.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/books/bunnicula-thinking-question-1-2_UNUSU.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/books/bunnicula-questions-3-4_33344.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/books/bunnicula-summarize-picture-3_PICTU.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/books/bunnicula-thinking-question-3-4_FUNNY.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/books/bunnicula-questions-5-6_FIVES.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/books/bunnicula-summarize-picture-6_STEAK.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/books/bunnicula-thinking-question-5-6_SMART.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/books/bunnicula-questions-7-9_SNINE.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/books/bunnicula-summarize-picture-8_SALAD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/books/bunnicula-thinking-question-7-9_SEVEN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/books/bunnicula-who-said-that_WHOSA.pdf?up=1486376756
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/books/bunnicula-who-said-that_WHOSA.pdf?up=1486376756
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/books/bunnicula-projects_PROJE.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/books/bunnicula-character-list_CHARL.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/books/bunnicula-wordsearch_WORDS.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/books/bunnicula-bookmarks_PWIXX.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/books/bunnicula-lit-circles-question-leader_QLEAD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/books/bunnicula-lit-circles-summary_SUMMA.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/books/bunnicula-lit-circles-illustrator_DRAWB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/books/bunnicula-lit-circles-word-wizard_WIZAR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/books/bunnicula-lit-circles-real-life-connector_CONNE.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/books/bunnicula-lit-circles-cover_LITCI.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/charlottes-web.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/charlottes-web.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/charlottes-web.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):
/books/charlottes-web-1-4_WASDV.pdf?up=1466611200
/books/charlottes-web-1-4_WASDV.pdf?up=1466611200
/books/charlottes-web-1-4-vocabulary_VOCAB.pdf?up=1554268224
/books/charlottes-web-1-4-vocabulary_VOCAB.pdf?up=1554268224
/books/charlottes-web-chapter-1-4-vocabulary-cards_YBFCZ.pdf?up=1554364503
/books/charlottes-web-1-summary-picture_CHARL.pdf?up=1466611200
/books/charlottes-web-5-8_FIVES.pdf?up=1466611200
/books/charlottes-web-5-8-vocab_BNHGJ.pdf?up=1466611200
/books/charlottes-web-chapter-5-8-vocabulary-cards_HGSDA.pdf?up=1554268225
/books/charlottes-web-5-8-draw-scene_DRAWS.pdf?up=1466611200
/books/charlottes-web-9-12_LOKNH.pdf?up=1466611200
/books/charlottes-web-9-12-vocab_GHJKL.pdf?up=1554268322
/books/charlottes-web-chapter-9-12-vocabulary-cards_JOKRE.pdf?up=1554268231
/books/charlottes-web-13-16_CHARL.pdf?up=1466611200
/books/charlottes-web-13-16-vocabulary_THIRT.pdf?up=1466611200
/books/charlottes-web-chapter-13-16-vocabulary-cards_KDXWQ.pdf?up=1554268224
/books/charlottes-web-17-19_TFGBV.pdf?up=1466611200
/books/charlottes-web-17-19-vocabulary_POLKM.pdf?up=1466611200
/books/charlottes-web-chapter-17-19-vocabulary-cards_FHGBX.pdf?up=1554268230
/books/charlottes-web-20-22_LASTS.pdf?up=1466611200
/books/charlottes-web-20-22-vocab_KOLJN.pdf?up=1466611200
/books/charlottes-web-chapter-20-22-vocabulary-cards_FAZUU.pdf?up=1554268224
/books/charlottes-web-whole-book-questions_WHOLE.pdf?up=1466611200
/books/charlottes-web-whole-book-questions_WHOLE.pdf?up=1466611200
/books/charlottes-web-who-said-that_QUOTE.pdf?up=1466611200
/books/charlottes-web-wordsearch_SEARC.pdf?up=1466611200
/books/charlottes-web-fact-opinion_FACTS.pdf?up=1466611200
/books/charlottes-web-bookmarks_FDIXK.pdf?up=1466611200
/books/charlottes-web-lit-circles-question-leader_QUEST.pdf?up=1466611200
/books/charlottes-web-lit-circles-summary_SUMMA.pdf?up=1466611200
/books/charlottes-web-lit-circles-illustrator_ILLUS.pdf?up=1466611200
/books/charlottes-web-lit-circles-word-wizard_WIZAR.pdf?up=1466611200
/books/charlotess-web-lit-circles-real-life-connector_REALL.pdf?up=1466611200
/books/charlottes-web-lit-circles-cover_COVER.pdf?up=1466611200
 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5:
cmd( 1):
processing https://www.downloadtest.com/books/charlottes-web-1-4_WASDV.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/books/charlottes-web-1-4_WASDV.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/books/charlottes-web-1-4-vocabulary_VOCAB.pdf?up=1554268224
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/books/charlottes-web-1-4-vocabulary_VOCAB.pdf?up=1554268224
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/books/charlottes-web-chapter-1-4-vocabulary-cards_YBFCZ.pdf?up=1554364503
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/books/charlottes-web-1-summary-picture_CHARL.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/books/charlottes-web-5-8_FIVES.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/books/charlottes-web-5-8-vocab_BNHGJ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/books/charlottes-web-chapter-5-8-vocabulary-cards_HGSDA.pdf?up=1554268225
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/books/charlottes-web-5-8-draw-scene_DRAWS.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/books/charlottes-web-9-12_LOKNH.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/books/charlottes-web-9-12-vocab_GHJKL.pdf?up=1554268322
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/books/charlottes-web-chapter-9-12-vocabulary-cards_JOKRE.pdf?up=1554268231
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/books/charlottes-web-13-16_CHARL.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/books/charlottes-web-13-16-vocabulary_THIRT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/books/charlottes-web-chapter-13-16-vocabulary-cards_KDXWQ.pdf?up=1554268224
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/books/charlottes-web-17-19_TFGBV.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/books/charlottes-web-17-19-vocabulary_POLKM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/books/charlottes-web-chapter-17-19-vocabulary-cards_FHGBX.pdf?up=1554268230
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/books/charlottes-web-20-22_LASTS.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/books/charlottes-web-20-22-vocab_KOLJN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/books/charlottes-web-chapter-20-22-vocabulary-cards_FAZUU.pdf?up=1554268224
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/books/charlottes-web-whole-book-questions_WHOLE.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/books/charlottes-web-whole-book-questions_WHOLE.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/books/charlottes-web-who-said-that_QUOTE.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/books/charlottes-web-wordsearch_SEARC.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/books/charlottes-web-fact-opinion_FACTS.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/books/charlottes-web-bookmarks_FDIXK.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/books/charlottes-web-lit-circles-question-leader_QUEST.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/books/charlottes-web-lit-circles-summary_SUMMA.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/books/charlottes-web-lit-circles-illustrator_ILLUS.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/books/charlottes-web-lit-circles-word-wizard_WIZAR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/books/charlotess-web-lit-circles-real-life-connector_REALL.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/books/charlottes-web-lit-circles-cover_COVER.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/book-magictreehouse-dinosaursbeforedark.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/book-magictreehouse-dinosaursbeforedark.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/book-magictreehouse-dinosaursbeforedark.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):
/books/magictreehouse-dinosaurs-chapters1-3_TREEH.pdf?up=1466611200
/books/magictreehouse-dinosaurs-chapters1-3_TREEH.pdf?up=1466611200
/books/magictreehouse-dinosaurs-thinking-question-1-2_THINK.pdf?up=1466611200
/books/magictreehouse-dinosaurs-thinking-question-1-2_THINK.pdf?up=1466611200
/books/magictreehouse-dinosaurs-summarize-picture-3_THREE.pdf?up=1466611200
/books/magictreehouse-dinosaurs-chapters4-6_MAGIC.pdf?up=1466611200
/books/magictreehouse-dinosaurs-summarize-picture-6_SIX66.pdf?up=1466611200
/books/magictreehouse-dinosaurs-chapters7-10_DINOS.pdf?up=1466611200
/books/magictreehouse-dinosaurs-summarize-picture-7_SEVEN.pdf?up=1466611200
/books/magictreehouse-dinosaurs-whole-book_WHOLB.pdf?up=1466611200
/books/magictreehouse-dinosaurs-whole-book_WHOLB.pdf?up=1466611200
/books/dinosaurs-before-dark-bookmarks_GWKXW.pdf?up=1466611200
/books/scavenger-hunt-mary-pope-osborne_KFAER.pdf?up=1576488074
/books/magictreehouse-dinosaurs-lit-circles-summary_DINOS.pdf?up=1466611200
/books/magictreehouse-dinosaurs-lit-circles-question-leader_QUESL.pdf?up=1466611200
/books/magictreehouse-dinosaurs-lit-circles-illustrator_PICTU.pdf?up=1466611200
/books/magictreehouse-dinosaurs-lit-circles-real-life-connector_ABCDE.pdf?up=1466611200
/books/magictreehouse-dinosaurs-lit-circles-word-wizard_WWORD.pdf?up=1466611200
/books/magictreehouse-dinosaurs-lit-circles-cover_COVER.pdf?up=1466611200
 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5:
cmd( 1):
processing https://www.downloadtest.com/books/magictreehouse-dinosaurs-chapters1-3_TREEH.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/books/magictreehouse-dinosaurs-chapters1-3_TREEH.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/books/magictreehouse-dinosaurs-thinking-question-1-2_THINK.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/books/magictreehouse-dinosaurs-thinking-question-1-2_THINK.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/books/magictreehouse-dinosaurs-summarize-picture-3_THREE.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/books/magictreehouse-dinosaurs-chapters4-6_MAGIC.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/books/magictreehouse-dinosaurs-summarize-picture-6_SIX66.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/books/magictreehouse-dinosaurs-chapters7-10_DINOS.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/books/magictreehouse-dinosaurs-summarize-picture-7_SEVEN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/books/magictreehouse-dinosaurs-whole-book_WHOLB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/books/magictreehouse-dinosaurs-whole-book_WHOLB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/books/dinosaurs-before-dark-bookmarks_GWKXW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/books/scavenger-hunt-mary-pope-osborne_KFAER.pdf?up=1576488074
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/books/magictreehouse-dinosaurs-lit-circles-summary_DINOS.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/books/magictreehouse-dinosaurs-lit-circles-question-leader_QUESL.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/books/magictreehouse-dinosaurs-lit-circles-illustrator_PICTU.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/books/magictreehouse-dinosaurs-lit-circles-real-life-connector_ABCDE.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/books/magictreehouse-dinosaurs-lit-circles-word-wizard_WWORD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/books/magictreehouse-dinosaurs-lit-circles-cover_COVER.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/book-the-boxcar-children.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/book-the-boxcar-children.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/book-the-boxcar-children.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):
/book-the-boxcar-children/the-boxcar-children-1-3_GJERN.pdf?up=1479194181
/book-the-boxcar-children/the-boxcar-children-1-3_GJERN.pdf?up=1479194181
/book-the-boxcar-children/the-boxcar-children-1-3-vocab_VEBRN.pdf?up=1479197214
/book-the-boxcar-children/the-boxcar-children-1-3-vocab_VEBRN.pdf?up=1479197214
/book-the-boxcar-children/the-boxcar-children-chapter-1-3-vocabulary-cards_BIAGO.pdf?up=1554268029
/book-the-boxcar-children/the-boxcar-children-1-3-writing-prompt_QOELJ.pdf?up=1482131606
/book-the-boxcar-children/the-boxcar-children-4-6_MEROB.pdf?up=1479194191
/book-the-boxcar-children/the-boxcar-children-4-6-vocab_PRKEL.pdf?up=1479194188
/book-the-boxcar-children/the-boxcar-children-chapter-4-6-vocabulary-cards_PIBAN.pdf?up=1554268030
/book-the-boxcar-children/the-boxcar-children-4-6-summarize-picture_TEKRN.pdf?up=1479194186
/book-the-boxcar-children/the-boxcar-children-4-6-writing-prompt_KRNEB.pdf?up=1480934476
/book-the-boxcar-children/the-boxcar-children-7-9_REWNB.pdf?up=1479194195
/book-the-boxcar-children/the-boxcar-children-7-9-vocab_JRNEM.pdf?up=1479194192
/book-the-boxcar-children/the-boxcar-children-chapter-7-9-vocabulary-cards_VUYAS.pdf?up=1554268029
/book-the-boxcar-children/the-boxcar-children-7-9-writing-prompt_ADRGH.pdf?up=1480936934
/book-the-boxcar-children/the-boxcar-children-10-13_PROEN.pdf?up=1479194183
/book-the-boxcar-children/the-boxcar-children-10-13-vocab_EOELM.pdf?up=1479194182
/book-the-boxcar-children/the-boxcar-children-chapter-10-13-vocabulary-cards_KYTDA.pdf?up=1554268029
/book-the-boxcar-children/the-boxcar-children-whole-book_KRENM.pdf?up=1479384503
/book-the-boxcar-children/the-boxcar-children-whole-book_KRENM.pdf?up=1479384503
/book-the-boxcar-children/the-boxcar-children-who-said-it_YRUTB.pdf?up=1479885906
/book-the-boxcar-children/the-boxcar-children-word-scramble_YWBEN.pdf?up=1490850369
/book-the-boxcar-children/box-car-childre-bookmark_GGATS.pdf?up=1479205509
/book-the-boxcar-children/the-boxcar-children-lit-circles-question-leader_KERBN.pdf?up=1479194204
/book-the-boxcar-children/the-boxcar-children-lit-circles-illustrator_QPELR.pdf?up=1479194204
/book-the-boxcar-children/the-boxcar-children-lit-circles-summary_SWERT.pdf?up=1479194205
/book-the-boxcar-children/the-boxcar-children-lit-circles-real-life-connector_MDNEM.pdf?up=1479194204
/book-the-boxcar-children/the-boxcar-children-lit-circles-word-wizard_XMDNE.pdf?up=1479194205
/book-the-boxcar-children/the-boxcar-children-lit-circles-cover_LWNEM.pdf?up=1479194203
 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5:
cmd( 1):
processing https://www.downloadtest.com/book-the-boxcar-children/the-boxcar-children-1-3_GJERN.pdf?up=1479194181
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/book-the-boxcar-children/the-boxcar-children-1-3_GJERN.pdf?up=1479194181
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/book-the-boxcar-children/the-boxcar-children-1-3-vocab_VEBRN.pdf?up=1479197214
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/book-the-boxcar-children/the-boxcar-children-1-3-vocab_VEBRN.pdf?up=1479197214
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/book-the-boxcar-children/the-boxcar-children-chapter-1-3-vocabulary-cards_BIAGO.pdf?up=1554268029
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/book-the-boxcar-children/the-boxcar-children-1-3-writing-prompt_QOELJ.pdf?up=1482131606
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/book-the-boxcar-children/the-boxcar-children-4-6_MEROB.pdf?up=1479194191
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/book-the-boxcar-children/the-boxcar-children-4-6-vocab_PRKEL.pdf?up=1479194188
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/book-the-boxcar-children/the-boxcar-children-chapter-4-6-vocabulary-cards_PIBAN.pdf?up=1554268030
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/book-the-boxcar-children/the-boxcar-children-4-6-summarize-picture_TEKRN.pdf?up=1479194186
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/book-the-boxcar-children/the-boxcar-children-4-6-writing-prompt_KRNEB.pdf?up=1480934476
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/book-the-boxcar-children/the-boxcar-children-7-9_REWNB.pdf?up=1479194195
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/book-the-boxcar-children/the-boxcar-children-7-9-vocab_JRNEM.pdf?up=1479194192
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/book-the-boxcar-children/the-boxcar-children-chapter-7-9-vocabulary-cards_VUYAS.pdf?up=1554268029
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/book-the-boxcar-children/the-boxcar-children-7-9-writing-prompt_ADRGH.pdf?up=1480936934
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/book-the-boxcar-children/the-boxcar-children-10-13_PROEN.pdf?up=1479194183
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/book-the-boxcar-children/the-boxcar-children-10-13-vocab_EOELM.pdf?up=1479194182
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/book-the-boxcar-children/the-boxcar-children-chapter-10-13-vocabulary-cards_KYTDA.pdf?up=1554268029
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/book-the-boxcar-children/the-boxcar-children-whole-book_KRENM.pdf?up=1479384503
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/book-the-boxcar-children/the-boxcar-children-whole-book_KRENM.pdf?up=1479384503
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/book-the-boxcar-children/the-boxcar-children-who-said-it_YRUTB.pdf?up=1479885906
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/book-the-boxcar-children/the-boxcar-children-word-scramble_YWBEN.pdf?up=1490850369
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/book-the-boxcar-children/box-car-childre-bookmark_GGATS.pdf?up=1479205509
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/book-the-boxcar-children/the-boxcar-children-lit-circles-question-leader_KERBN.pdf?up=1479194204
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/book-the-boxcar-children/the-boxcar-children-lit-circles-illustrator_QPELR.pdf?up=1479194204
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/book-the-boxcar-children/the-boxcar-children-lit-circles-summary_SWERT.pdf?up=1479194205
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/book-the-boxcar-children/the-boxcar-children-lit-circles-real-life-connector_MDNEM.pdf?up=1479194204
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/book-the-boxcar-children/the-boxcar-children-lit-circles-word-wizard_XMDNE.pdf?up=1479194205
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/book-the-boxcar-children/the-boxcar-children-lit-circles-cover_LWNEM.pdf?up=1479194203
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/full-books.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/full-books.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/full-books.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):

 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
---Step5:
cmd( 1):
processing https://www.downloadtest.com
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/animals.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/animals.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/animals.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):
/animals/vertebrate-paste_WMMTD.pdf?up=1466611200
/animals/vertebrate-paste_WMMTD.pdf?up=1466611200
/animals/vertebrate-quiz_WMMTF.pdf?up=1466611200
/animals/vertebrate-quiz_WMMTF.pdf?up=1466611200
/animals/vertebrate-groups-table_ANIML.pdf?up=1466611200
/animals/vertebrate-thinking-questions_QUEST.pdf?up=1466611200
/invertebrates/cut-out-invertebrates-vertebrates_HYASU.pdf?up=1492755689
/animals/vertebrate-invertebrate-paste_WMMTQ.pdf?up=1466611200
/animals/reptile-scav-hunt_WMMTN.pdf?up=1526025772
/animals/mammal-scav-hunt_MAMMA.pdf?up=1522993573
/animals/bird-scav-hunt_WMMTR.pdf?up=1526025770
/animals/frog-scav-hunt_WMRRT.pdf?up=1533723948
/animals/fish-scav-hunt_FISHS.pdf?up=1531821789
/wordsearch/mammals-wordsearch_MAMML.pdf?up=1466611200
/wordsearch/reptiles-wordsearch_REPTW.pdf?up=1466611200
/wordsearch/fish-wordsearch_FISHW.pdf?up=1518611504
/wordsearch/birds-wordsearch_BBIRD.pdf?up=1466611200
/animals/cipher-wheel-mammals_CXZAS.pdf?up=1493883439
/animals/cipher-wheel-birds_KFBCX.pdf?up=1494221451
/animals/cipher-wheel-reptiles_KLSVE.pdf?up=1494221471
/animals/cipher-wheel-amphibians_FROGS.pdf?up=1527584457
/animals/cipher-wheel-fish_XMZIY.pdf?up=1494243835
/animals/cootie-catcher-vertebrate_GWTSI.pdf?up=1466611200
/animals/animal-report_GHWZJ.pdf?up=1466611200
/animals/animal-report_GHWZJ.pdf?up=1466611200
/poems/vertebrates-vs-invertebrates_BIRDS.pdf?up=1526458730
/reading-comp/3rd-camouflage_WMMRQ.pdf?up=1466611200
/reading-comp/3rd-hibernation_WBDBQ.pdf?up=1466611200
/reading-comp/3rd-migration_WMMRN.pdf?up=1466611200
/reading-comp/3rd-animal-madness-with-professor-aligator_ANIMA.pdf?up=1466611200
/a-maze-ing/a-maze-ing-animal-classes_VERTE.pdf?up=1466611200
/a-maze-ing/a-maze-ing-lizards_LIZAR.pdf?up=1466611200
/a-maze-ing/a-maze-ing-wildcats_WCATS.pdf?up=1466611200
/a-maze-ing/a-maze-ing-birds_RAPTO.pdf?up=1466611200
/a-maze-ing/a-maze-ing-frogs-toads-salamanders_AMPHI.pdf?up=1466611200
/a-maze-ing/a-maze-ing-sharks_JAWSS.pdf?up=1466611200
 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5:
cmd( 1):
processing https://www.downloadtest.com/animals/vertebrate-paste_WMMTD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/animals/vertebrate-paste_WMMTD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/animals/vertebrate-quiz_WMMTF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/animals/vertebrate-quiz_WMMTF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/animals/vertebrate-groups-table_ANIML.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/animals/vertebrate-thinking-questions_QUEST.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/invertebrates/cut-out-invertebrates-vertebrates_HYASU.pdf?up=1492755689
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/animals/vertebrate-invertebrate-paste_WMMTQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/animals/reptile-scav-hunt_WMMTN.pdf?up=1526025772
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/animals/mammal-scav-hunt_MAMMA.pdf?up=1522993573
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/animals/bird-scav-hunt_WMMTR.pdf?up=1526025770
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/animals/frog-scav-hunt_WMRRT.pdf?up=1533723948
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/animals/fish-scav-hunt_FISHS.pdf?up=1531821789
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/wordsearch/mammals-wordsearch_MAMML.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/wordsearch/reptiles-wordsearch_REPTW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/wordsearch/fish-wordsearch_FISHW.pdf?up=1518611504
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/wordsearch/birds-wordsearch_BBIRD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/animals/cipher-wheel-mammals_CXZAS.pdf?up=1493883439
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/animals/cipher-wheel-birds_KFBCX.pdf?up=1494221451
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/animals/cipher-wheel-reptiles_KLSVE.pdf?up=1494221471
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/animals/cipher-wheel-amphibians_FROGS.pdf?up=1527584457
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/animals/cipher-wheel-fish_XMZIY.pdf?up=1494243835
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/animals/cootie-catcher-vertebrate_GWTSI.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/animals/animal-report_GHWZJ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/animals/animal-report_GHWZJ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/poems/vertebrates-vs-invertebrates_BIRDS.pdf?up=1526458730
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-camouflage_WMMRQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-hibernation_WBDBQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-migration_WMMRN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-animal-madness-with-professor-aligator_ANIMA.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/a-maze-ing/a-maze-ing-animal-classes_VERTE.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/a-maze-ing/a-maze-ing-lizards_LIZAR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/a-maze-ing/a-maze-ing-wildcats_WCATS.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/a-maze-ing/a-maze-ing-birds_RAPTO.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/a-maze-ing/a-maze-ing-frogs-toads-salamanders_AMPHI.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/a-maze-ing/a-maze-ing-sharks_JAWSS.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/animal-articles.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/animal-articles.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/animal-articles.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):
/reading-comp/4th-aardvarks_WDFMQ.pdf?up=1466611200
/reading-comp/4th-aardvarks_WDFMQ.pdf?up=1466611200
/reading-comp/5th-american-bison_ISONB.pdf?up=1489422227
/reading-comp/5th-american-bison_ISONB.pdf?up=1489422227
/reading-comp/5th-the-amazingly-adapted-arctic-fox_FOXES.pdf?up=1516800061
/reading-comp/5th-armadillo_WDFMM.pdf?up=1466611200
/reading-comp/5th-bats_BATTY.pdf?up=1466611200
/reading-comp/6th-beavers_BUSYB.pdf?up=1466611200
/reading-comp/4th-bighorn-sheep_SHEEP.pdf?up=1466611200
/reading-comp/5th-brown-bears_KODIA.pdf?up=1503333640
/reading-comp/3rd-camels_WDFMN.pdf?up=1466611200
/reading-comp/5th-master-grasses_WDFMR.pdf?up=1466611200
/reading-comp/5th-cheetah_CHEET.pdf?up=1466611200
/reading-comp/3rd-chipmunks_WDFMT.pdf?up=1491458701
/reading-comp/4th-dolphins_PORPO.pdf?up=1519642509
/reading-comp/5th-elephant-seals_WDFMW.pdf?up=1466611200
/reading-comp/6th-elephant-seals_SNOUT.pdf?up=1501671913
/reading-comp/4th-fennec-fox_WDFMZ.pdf?up=1466611200
/reading-comp/6th-flying-squirrels_SQUIR.pdf?up=1466611200
/reading-comp/5th-giant-anteaters_GENTL.pdf?up=1531907669
/reading-comp/5th-giant-pandas_CHINA.pdf?up=1473746345
/reading-comp/3rd-giraffe_WBBZT.pdf?up=1549275619
/reading-comp/3rd-groundhog_WDFNB.pdf?up=1466611200
/reading-comp/4th-hedgehogs_WDFND.pdf?up=1466611200
/reading-comp/4th-honey-badger_BADGER.pdf?up=1466611200
/reading-comp/5th-howler-monkeys_HOWLR.pdf?up=1466611200
/reading-comp/5th-hyenas_LAUGH.pdf?up=1466611200
/reading-comp/5th-jaguars_WDFNF.pdf?up=1466611200
/reading-comp/4th-koala_WDFNQ.pdf?up=1466611200
/reading-comp/6th-leopards_SPOTS.pdf?up=1481272161
/reading-comp/4th-a-pride-of-lions_TREES.pdf?up=1466611200
/reading-comp/5th-llama_WDFNM.pdf?up=1466611200
/reading-comp/5th-mandrills_WDFNN.pdf?up=1466611200
/reading-comp/5th-a-mob-of-meerkats_RKATS.pdf?up=1466611200
/reading-comp/5th-the-worlds-largest-deer_MOOSE.pdf?up=1479459211
/reading-comp/4th-muntjac_BARKI.pdf?up=1466611200
/reading-comp/6th-narwhal_UNICO.pdf?up=1466611200
/reading-comp/3rd-okapi_OKAPI.pdf?up=1474881811
/reading-comp/5th-orcas_ORCAS.pdf?up=1466611200
/reading-comp/4th-the-big-ostrich_HATCH.pdf?up=1466611200
/reading-comp/5th-the-worlds-scaliest-mammal_PANGO.pdf?up=1548766065
/reading-comp/3rd-the-pink-fairy-armadillo_PFARM.pdf?up=1466611200
/reading-comp/4th-platypus_WDFNR.pdf?up=1466611200
/reading-comp/4th-polar-bears_WDFNT.pdf?up=1466611200
/reading-comp/4th-polecat_WDFNW.pdf?up=1466611200
/reading-comp/4th-porcupine_WDFNZ.pdf?up=1466611200
/reading-comp/5th-rabbits-and-hares_BUNNY.pdf?up=1476871490
/reading-comp/5th-red-pandas_NANAS.pdf?up=1520917326
/reading-comp/5th-is-that-a-reindeer-or-a-caribou_SNOWY.pdf?up=1509037034
/reading-comp/5th-rockin-rhinos_RHINO.pdf?up=1484640100
/reading-comp/6th-red-kangaroo_KANGA.pdf?up=1466611200
/reading-comp/2nd-skunk-alert_SMELL.pdf?up=1466611200
/reading-comp/3rd-skunks_WDFRB.pdf?up=1466611200
/reading-comp/4th-sloth_SLOTH.pdf?up=1466611200
/reading-comp/4th-spectacled-bears_WDFRD.pdf?up=1466611200
/reading-comp/5th-echidna_SPINY.pdf?up=1466611200
/reading-comp/4th-tamandua_ABCDE.pdf?up=1466611200
/reading-comp/5th-tiger-talk_TIGER.pdf?up=1470933889
/reading-comp/5th-visayan-warty-pigs_WISAY.pdf?up=1466611200
/reading-comp/4th-wolverine_WDFRF.pdf?up=1466611200
/reading-comp/5th-wolves-true-wilderness-animals_HOWLS.pdf?up=1523030962
/reading-comp/5th-what-is-a-wombat_AUSSI.pdf?up=1487054977
/reading-comp/5th-zebras_ZEBRA.pdf?up=1466611200
/reading-comp/4th-alligators-crocodiles_WBDWF.pdf?up=1563430046
/reading-comp/4th-alligator-snapping-turtles_SNAPT.pdf?up=1550657117
/reading-comp/4th-boa-python_WDFRR.pdf?up=1466611200
/reading-comp/4th-chameleon_WDFRN.pdf?up=1496216870
/reading-comp/5th-cobra_COBRA.pdf?up=1466611200
/reading-comp/5th-tortoise_TORTO.pdf?up=1466611200
/reading-comp/5th-monster-of-the-desert_GILAM.pdf?up=1466611200
/reading-comp/5th-green-anaconda_SNAKE.pdf?up=1466611200
/reading-comp/3rd-horned-lizard_WDFRT.pdf?up=1466611200
/reading-comp/4th-iguanas_WDFRW.pdf?up=1466611200
/reading-comp/4th-komodo_WDFTB.pdf?up=1466611200
/reading-comp/5th-rattlesnakes_RATLE.pdf?up=1466611200
/reading-comp/6th-sea-turtles_GREEN.pdf?up=1466611200
/reading-comp/6th-sea-snakes_SEASN.pdf?up=1466611200
/reading-comp/5th-lizard-with-a-blue-tongue_YHWQA.pdf?up=1466611200
/reading-comp/5th-tuatara_WDFRZ.pdf?up=1488874944
/reading-comp/5th-atlantic-puffins_PUFFY.pdf?up=1526288734
/reading-comp/5th-atlantic-puffins_PUFFY.pdf?up=1526288734
/reading-comp/4th-hummingbirds_WDFTF.pdf?up=1466611200
/reading-comp/5th-bald-eagles_EAGLE.pdf?up=1466611200
/reading-comp/4th-flamingos_WDFTQ.pdf?up=1466611200
/reading-comp/4th-flightlessbirds_WDFTM.pdf?up=1466611200
/reading-comp/5th-kiwi_KIWI5.pdf?up=1466611200
/reading-comp/4th-owls_HOOOT.pdf?up=1466611200
/reading-comp/5th-peacocks_PFOWL.pdf?up=1466611200
/reading-comp/5th-penguins_PENGU.pdf?up=1466611200
/reading-comp/5th-a-fast-flying-hunter_FALCO.pdf?up=1541138888
/reading-comp/4th-secretary-bird_SECRE.pdf?up=1466611200
/reading-comp/5th-toucan_TOUCA.pdf?up=1466611200
/reading-comp/6th-valuable-vultures_WORLD.pdf?up=1488532351
/reading-comp/2nd-wild-turkeys_WILDT.pdf?up=1466611200
/reading-comp/4th-electric-eels_SHOCK.pdf?up=1476436969
/reading-comp/4th-flying-fish_WDFTT.pdf?up=1466611200
/reading-comp/5th-greatwhitesharks_WDFTW.pdf?up=1466611200
/reading-comp/4th-hammerheadsharks_SHARK.pdf?up=1466611200
/reading-comp/6th-mysterious-moray-eels_AMORE.pdf?up=1466611200
/reading-comp/5th-eight-legged-creatures_OCTOP.pdf?up=1559208112
/reading-comp/3rd-all-about-squishy-squid_SQUID.pdf?up=1490247729
/reading-comp/5th-seahorse_WDFTZ.pdf?up=1466611200
/reading-comp/4th-sharks_AREAW.pdf?up=1466611200
/reading-comp/4th-fish-with-a-stinging-tail_STING.pdf?up=1528882354
/reading-comp/4th-frogs-toads_WDFWD.pdf?up=1466611200
/reading-comp/4th-amphibians_WDFWF.pdf?up=1466611200
/reading-comp/6th-poison-dart-frogs_DARTF.pdf?up=1508756019
/reading-comp/4th-grasshoppers_WDFWM.pdf?up=1466611200
/reading-comp/6th-leaf-cutter-ants_LEAFC.pdf?up=1466611200
/reading-comp/5th-ladybugs_WDFWN.pdf?up=1466611200
/animals/insects-mealworm-article_WDFWR.pdf?up=1466611200
/reading-comp/3rd-monarch_WDFWT.pdf?up=1493635262
/reading-comp/4th-butterflies-moths_WDFWW.pdf?up=1466611200
/reading-comp/4th-pillbugs_WBDRW.pdf?up=1466611200
/reading-comp/5th-scorpions_STING.pdf?up=1500024374
/reading-comp/5th-scorpions-bathr_WDFZB.pdf?up=1466611200
/reading-comp/6th-rhinoceros-beetle_BEETL.pdf?up=1466611200
/reading-comp/4th-tarantula_WDFZD.pdf?up=1466611200
/reading-comp/4th-going-buggy_WDFZF.pdf?up=1466611200
/reading-comp/4th-stink-bugs_WDFZQ.pdf?up=1466611200
/reading-comp/6th-taxi-crab_TCRAB.pdf?up=1585897014
/reading-comp/5th-banana-slugs_SLUGS.pdf?up=1536573292
/reading-comp/4th-coral-reefs_CORAL.pdf?up=1466611200
/reading-comp/5th-earthworms_WORMY.pdf?up=1466611200
/reading-comp/3rd-octopus_WDFZN.pdf?up=1466611200
/reading-comp/5th-jellyfish_WDFZR.pdf?up=1466611200
/reading-comp/3rd-animal-madness-with-professor-aligator_ANIMA.pdf?up=1466611200
/reading-comp/3rd-camouflage_WDFZW.pdf?up=1466611200
/reading-comp/3rd-hibernation_WBDBQ.pdf?up=1466611200
/reading-comp/3rd-migration_WDQBB.pdf?up=1466611200
/reading-comp/4th-nocturnal_WDQBD.pdf?up=1466611200
/reading-comp/4th-carnivores-herbivores_WDQBF.pdf?up=1466611200
 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5:
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-aardvarks_WDFMQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-aardvarks_WDFMQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-american-bison_ISONB.pdf?up=1489422227
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-american-bison_ISONB.pdf?up=1489422227
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-the-amazingly-adapted-arctic-fox_FOXES.pdf?up=1516800061
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-armadillo_WDFMM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-bats_BATTY.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/6th-beavers_BUSYB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-bighorn-sheep_SHEEP.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-brown-bears_KODIA.pdf?up=1503333640
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-camels_WDFMN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-master-grasses_WDFMR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-cheetah_CHEET.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-chipmunks_WDFMT.pdf?up=1491458701
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-dolphins_PORPO.pdf?up=1519642509
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-elephant-seals_WDFMW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/6th-elephant-seals_SNOUT.pdf?up=1501671913
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-fennec-fox_WDFMZ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/6th-flying-squirrels_SQUIR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-giant-anteaters_GENTL.pdf?up=1531907669
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-giant-pandas_CHINA.pdf?up=1473746345
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-giraffe_WBBZT.pdf?up=1549275619
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-groundhog_WDFNB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-hedgehogs_WDFND.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-honey-badger_BADGER.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-howler-monkeys_HOWLR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-hyenas_LAUGH.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-jaguars_WDFNF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-koala_WDFNQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/6th-leopards_SPOTS.pdf?up=1481272161
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-a-pride-of-lions_TREES.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-llama_WDFNM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-mandrills_WDFNN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-a-mob-of-meerkats_RKATS.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-the-worlds-largest-deer_MOOSE.pdf?up=1479459211
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-muntjac_BARKI.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/6th-narwhal_UNICO.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-okapi_OKAPI.pdf?up=1474881811
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-orcas_ORCAS.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-the-big-ostrich_HATCH.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-the-worlds-scaliest-mammal_PANGO.pdf?up=1548766065
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-the-pink-fairy-armadillo_PFARM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-platypus_WDFNR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-polar-bears_WDFNT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-polecat_WDFNW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-porcupine_WDFNZ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-rabbits-and-hares_BUNNY.pdf?up=1476871490
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-red-pandas_NANAS.pdf?up=1520917326
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-is-that-a-reindeer-or-a-caribou_SNOWY.pdf?up=1509037034
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-rockin-rhinos_RHINO.pdf?up=1484640100
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/6th-red-kangaroo_KANGA.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/2nd-skunk-alert_SMELL.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-skunks_WDFRB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-sloth_SLOTH.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-spectacled-bears_WDFRD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-echidna_SPINY.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-tamandua_ABCDE.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-tiger-talk_TIGER.pdf?up=1470933889
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-visayan-warty-pigs_WISAY.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-wolverine_WDFRF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-wolves-true-wilderness-animals_HOWLS.pdf?up=1523030962
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-what-is-a-wombat_AUSSI.pdf?up=1487054977
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-zebras_ZEBRA.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-alligators-crocodiles_WBDWF.pdf?up=1563430046
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-alligator-snapping-turtles_SNAPT.pdf?up=1550657117
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-boa-python_WDFRR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-chameleon_WDFRN.pdf?up=1496216870
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-cobra_COBRA.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-tortoise_TORTO.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-monster-of-the-desert_GILAM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-green-anaconda_SNAKE.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-horned-lizard_WDFRT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-iguanas_WDFRW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-komodo_WDFTB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-rattlesnakes_RATLE.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/6th-sea-turtles_GREEN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/6th-sea-snakes_SEASN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-lizard-with-a-blue-tongue_YHWQA.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-tuatara_WDFRZ.pdf?up=1488874944
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-atlantic-puffins_PUFFY.pdf?up=1526288734
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-atlantic-puffins_PUFFY.pdf?up=1526288734
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-hummingbirds_WDFTF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-bald-eagles_EAGLE.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-flamingos_WDFTQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-flightlessbirds_WDFTM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-kiwi_KIWI5.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-owls_HOOOT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-peacocks_PFOWL.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-penguins_PENGU.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-a-fast-flying-hunter_FALCO.pdf?up=1541138888
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-secretary-bird_SECRE.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-toucan_TOUCA.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/6th-valuable-vultures_WORLD.pdf?up=1488532351
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/2nd-wild-turkeys_WILDT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-electric-eels_SHOCK.pdf?up=1476436969
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-flying-fish_WDFTT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-greatwhitesharks_WDFTW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-hammerheadsharks_SHARK.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/6th-mysterious-moray-eels_AMORE.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-eight-legged-creatures_OCTOP.pdf?up=1559208112
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-all-about-squishy-squid_SQUID.pdf?up=1490247729
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-seahorse_WDFTZ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-sharks_AREAW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-fish-with-a-stinging-tail_STING.pdf?up=1528882354
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-frogs-toads_WDFWD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-amphibians_WDFWF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/6th-poison-dart-frogs_DARTF.pdf?up=1508756019
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-grasshoppers_WDFWM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/6th-leaf-cutter-ants_LEAFC.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-ladybugs_WDFWN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/animals/insects-mealworm-article_WDFWR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-monarch_WDFWT.pdf?up=1493635262
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-butterflies-moths_WDFWW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-pillbugs_WBDRW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-scorpions_STING.pdf?up=1500024374
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-scorpions-bathr_WDFZB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/6th-rhinoceros-beetle_BEETL.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-tarantula_WDFZD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-going-buggy_WDFZF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-stink-bugs_WDFZQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/6th-taxi-crab_TCRAB.pdf?up=1585897014
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-banana-slugs_SLUGS.pdf?up=1536573292
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-coral-reefs_CORAL.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-earthworms_WORMY.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-octopus_WDFZN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-jellyfish_WDFZR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-animal-madness-with-professor-aligator_ANIMA.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-camouflage_WDFZW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-hibernation_WBDBQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-migration_WDQBB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-nocturnal_WDQBD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-carnivores-herbivores_WDQBF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/butterfly-life-cycle.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/butterfly-life-cycle.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/butterfly-life-cycle.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):
/animals/life-cycle-butterfly-wheel_WMRMW.pdf?up=1466611200
/animals/life-cycle-butterfly-wheel_WMRMW.pdf?up=1466611200
/animals/life-cycle-butterfly_WMRMZ.pdf?up=1524721043
/animals/life-cycle-butterfly_WMRMZ.pdf?up=1524721043
/animals/butterfly-scav-hunt_WMRNB.pdf?up=1531821790
/animals/butterfly-fitb_WMRND.pdf?up=1533188318
/songs/butterfly-ballad_WMRNF.pdf?up=1466611200
/songs/life-cycle-song_WMRNQ.pdf?up=1466611200
/animals/butterfly-crossword_WMRNM.pdf?up=1553168237
/a-maze-ing/a-maze-ing-butterfly-life-cycle_LIFEC.pdf?up=1466611200
/animals/butterfly-life-cycle-mini-book_WMRNN.pdf?up=1466611200
/animals/butterfly-life-cycle-mini-book_WMRNN.pdf?up=1466611200
/animals/butterfly-questions_WMRNR.pdf?up=1466611200
/reading-comp/3rd-monarch_WDFWT.pdf?up=1493635262
/reading-comp/4th-butterflies-moths_WMRNW.pdf?up=1466611200
 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5:
cmd( 1):
processing https://www.downloadtest.com/animals/life-cycle-butterfly-wheel_WMRMW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/animals/life-cycle-butterfly-wheel_WMRMW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/animals/life-cycle-butterfly_WMRMZ.pdf?up=1524721043
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/animals/life-cycle-butterfly_WMRMZ.pdf?up=1524721043
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/animals/butterfly-scav-hunt_WMRNB.pdf?up=1531821790
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/animals/butterfly-fitb_WMRND.pdf?up=1533188318
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/songs/butterfly-ballad_WMRNF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/songs/life-cycle-song_WMRNQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/animals/butterfly-crossword_WMRNM.pdf?up=1553168237
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/a-maze-ing/a-maze-ing-butterfly-life-cycle_LIFEC.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/animals/butterfly-life-cycle-mini-book_WMRNN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/animals/butterfly-life-cycle-mini-book_WMRNN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/animals/butterfly-questions_WMRNR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-monarch_WDFWT.pdf?up=1493635262
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-butterflies-moths_WMRNW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/electricity.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/electricity.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/electricity.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):
/electricity/conductor-insulator_WMNMW.pdf?up=1466611200
/electricity/conductor-insulator_WMNMW.pdf?up=1466611200
/electricity/series-parallel-circuits_WMNMZ.pdf?up=1466611200
/electricity/series-parallel-circuits_WMNMZ.pdf?up=1466611200
/electricity/electrical-circuits-2_WMNNB.pdf?up=1466611200
/electricity/electricity-mini-book_WMNND.pdf?up=1466611200
/electricity/electrical-circuits-1_WMNNF.pdf?up=1466611200
/electricity/current-mc_WMNNQ.pdf?up=1466611200
/electricity/current-fitb_WMNNM.pdf?up=1466611200
/electricity/circuits-broken_WMNNN.pdf?up=1466611200
/electricity/electrical-charges-1_WMNNT.pdf?up=1466611200
/electricity/electrical-charges-1_WMNNT.pdf?up=1466611200
/electricity/static-balloons_WMNNW.pdf?up=1466611200
/electricity/static-fitb_WMNNZ.pdf?up=1466611200
/poems/4th-electric-poetry_ELECT.pdf?up=1528474684
 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5:
cmd( 1):
processing https://www.downloadtest.com/electricity/conductor-insulator_WMNMW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/electricity/conductor-insulator_WMNMW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/electricity/series-parallel-circuits_WMNMZ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/electricity/series-parallel-circuits_WMNMZ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/electricity/electrical-circuits-2_WMNNB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/electricity/electricity-mini-book_WMNND.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/electricity/electrical-circuits-1_WMNNF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/electricity/current-mc_WMNNQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/electricity/current-fitb_WMNNM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/electricity/circuits-broken_WMNNN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/electricity/electrical-charges-1_WMNNT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/electricity/electrical-charges-1_WMNNT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/electricity/static-balloons_WMNNW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/electricity/static-fitb_WMNNZ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/poems/4th-electric-poetry_ELECT.pdf?up=1528474684
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/human-body.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/human-body.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/human-body.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):
/human-body/brain_MVWDT.pdf?up=1466611200
/human-body/brain_MVWDT.pdf?up=1466611200
/human-body/heart_WMNZQ.pdf?up=1466611200
/human-body/heart_WMNZQ.pdf?up=1466611200
/human-body/lungs_WMNZM.pdf?up=1466611200
/human-body/digestive-system_WMNZN.pdf?up=1466611200
/human-body/liver_WMNZR.pdf?up=1466611200
/human-body/kidneys_WMNZT.pdf?up=1466611200
/human-body/muscles_WMNZW.pdf?up=1466611200
/human-body/bones_WMNZZ.pdf?up=1466611200
/reading-comp/4th-skin_DERMI.pdf?up=1466611200
/reading-comp/5th-immune-system_WMRBB.pdf?up=1466611200
/human-body/questions-bodyparts_WMRBQ.pdf?up=1466611200
/human-body/questions-bodyparts_WMRBQ.pdf?up=1466611200
/human-body/organs-label_WMRBM.pdf?up=1466611200
/human-body/organs-picture_WMRBN.pdf?up=1466611200
/science-projects/human-body-scav-hunt_WMTMT.pdf?up=1533730586
/a-maze-ing/a-maze-ing-organs_HBODY.pdf?up=1466611200
/human-body/word-search-human-body_WMRBR.pdf?up=1466611200
 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5:
cmd( 1):
processing https://www.downloadtest.com/human-body/brain_MVWDT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/human-body/brain_MVWDT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/human-body/heart_WMNZQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/human-body/heart_WMNZQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/human-body/lungs_WMNZM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/human-body/digestive-system_WMNZN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/human-body/liver_WMNZR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/human-body/kidneys_WMNZT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/human-body/muscles_WMNZW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/human-body/bones_WMNZZ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-skin_DERMI.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-immune-system_WMRBB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/human-body/questions-bodyparts_WMRBQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/human-body/questions-bodyparts_WMRBQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/human-body/organs-label_WMRBM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/human-body/organs-picture_WMRBN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/science-projects/human-body-scav-hunt_WMTMT.pdf?up=1533730586
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/a-maze-ing/a-maze-ing-organs_HBODY.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/human-body/word-search-human-body_WMRBR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/matter.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/matter.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/matter.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):
/matter/matter-article_WMTBN.pdf?up=1466611200
/matter/matter-article_WMTBN.pdf?up=1466611200
/matter/matter1_WMTBR.pdf?up=1466611200
/matter/matter1_WMTBR.pdf?up=1466611200
/matter/matter-sort_WMTBT.pdf?up=1466611200
/matter/matter-water_WMTBW.pdf?up=1466611200
/matter/matter-project_WMTBZ.pdf?up=1466611200
/matter/matter-crossword_WMTDB.pdf?up=1466611200
/matter/matter-experiment_WMTDD.pdf?up=1466611200
/matter/matter-pictures_WMTDF.pdf?up=1466611200
/matter/cootie-catcher-solid-liquid-gas_PSYKA.pdf?up=1466611200
/matter/matter-deepthoughts_WMTDQ.pdf?up=1487931277
/matter/matter-deepthoughts_WMTDQ.pdf?up=1487931277
/matter/matter-article-cargile_WMTDM.pdf?up=1466611200
/matter/solid-liquid-gas_WMTDN.pdf?up=1466611200
/songs/matter-song_WMTDR.pdf?up=1466611200
/songs/bingo-matter-song1_WMTDT.pdf?up=1466611200
 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5:
cmd( 1):
processing https://www.downloadtest.com/matter/matter-article_WMTBN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/matter/matter-article_WMTBN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/matter/matter1_WMTBR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/matter/matter1_WMTBR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/matter/matter-sort_WMTBT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/matter/matter-water_WMTBW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/matter/matter-project_WMTBZ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/matter/matter-crossword_WMTDB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/matter/matter-experiment_WMTDD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/matter/matter-pictures_WMTDF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/matter/cootie-catcher-solid-liquid-gas_PSYKA.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/matter/matter-deepthoughts_WMTDQ.pdf?up=1487931277
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/matter/matter-deepthoughts_WMTDQ.pdf?up=1487931277
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/matter/matter-article-cargile_WMTDM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/matter/solid-liquid-gas_WMTDN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/songs/matter-song_WMTDR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/songs/bingo-matter-song1_WMTDT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/simple-machines.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/simple-machines.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/simple-machines.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):
/simple-machines/simple-machines-mini-book_WMTNF.pdf?up=1466611200
/simple-machines/simple-machines-mini-book_WMTNF.pdf?up=1466611200
/simple-machines/simple-machines-game_WMTNQ.pdf?up=1466611200
/simple-machines/simple-machines-game_WMTNQ.pdf?up=1466611200
/simple-machines/simple-machines-pictures_WMTNM.pdf?up=1466611200
/simple-machines/simple-machines-article_WMTNN.pdf?up=1466611200
/simple-machines/simple-machines-people_WMTNR.pdf?up=1466611200
/simple-machines/levers-fulcrums-pictures_WMTNT.pdf?up=1466611200
/simple-machines/simple-machines-items_WMTNW.pdf?up=1466611200
/simple-machines/simple-machines-questions_WMTNZ.pdf?up=1466611200
/simple-machines/simple-machines-examples_WMTRB.pdf?up=1466611200
/simple-machines/simple-machines-examples_WMTRB.pdf?up=1466611200
/simple-machines/simple-machines-multiplechoice_WMTRD.pdf?up=1466611200
/simple-machines/simple-machines-crossword_WMTRF.pdf?up=1466611200
 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5:
cmd( 1):
processing https://www.downloadtest.com/simple-machines/simple-machines-mini-book_WMTNF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/simple-machines/simple-machines-mini-book_WMTNF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/simple-machines/simple-machines-game_WMTNQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/simple-machines/simple-machines-game_WMTNQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/simple-machines/simple-machines-pictures_WMTNM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/simple-machines/simple-machines-article_WMTNN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/simple-machines/simple-machines-people_WMTNR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/simple-machines/levers-fulcrums-pictures_WMTNT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/simple-machines/simple-machines-items_WMTNW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/simple-machines/simple-machines-questions_WMTNZ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/simple-machines/simple-machines-examples_WMTRB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/simple-machines/simple-machines-examples_WMTRB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/simple-machines/simple-machines-multiplechoice_WMTRD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/simple-machines/simple-machines-crossword_WMTRF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/solar-system-planets.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/solar-system-planets.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/solar-system-planets.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):
/space/solarsystem-scav-hunt_WMTTD.pdf?up=1531821654
/space/solarsystem-scav-hunt_WMTTD.pdf?up=1531821654
/space/planet-report_WMTTQ.pdf?up=1466611200
/space/planet-report_WMTTQ.pdf?up=1466611200
/space/to-the-moon-space-trivia_WMTTF.pdf?up=1466611200
/space/scoot-solar-system_PLOKM.pdf?up=1466611200
/space/cipher-wheel-solar-system_HOKIE.pdf?up=1493721021
/space/cootie-catcher-solar-system_PISKV.pdf?up=1562927299
/space/mercury_WMTTN.pdf?up=1466611200
/space/venus_WMTTR.pdf?up=1466611200
/space/earth_WMTTT.pdf?up=1466611200
/space/mars_WMTTW.pdf?up=1466611200
/space/jupiter_WMTWB.pdf?up=1466611200
/space/saturn_WMTWD.pdf?up=1562927300
/space/uranus_WMTWF.pdf?up=1466611200
/space/neptune_WMTWQ.pdf?up=1466611200
/space/pluto_WMTWM.pdf?up=1466611200
/space/sun_WMTWN.pdf?up=1520249440
/space/moon_TMOON.pdf?up=1466611200
/reading-comp/5th-dwarf-planets_WMTWR.pdf?up=1466611200
/reading-comp/3rd-moons_WMTWT.pdf?up=1466611200
/reading-comp/4th-constellations_WMTWW.pdf?up=1478782973
/space/solar-system-cut-glue_ABZHU.pdf?up=1500977085
/space/solar-system-cut-glue_ABZHU.pdf?up=1500977085
/space/planet-riddles_WMTZD.pdf?up=1520249756
/space/solar-system-cut-outs_FWJSU.pdf?up=1500482687
/space/weight-on-planets_WMTZF.pdf?up=1466611200
/space/solarsystem-crossword_WMTZQ.pdf?up=1466611200
/space/space-questions_WMTZW.pdf?up=1466611200
/space/seasons-orbit_PERTD.pdf?up=1530179488
/space/moonphases3_WMTZM.pdf?up=1500446844
/space/moonphases4_WMTZN.pdf?up=1500448927
/space/moonphases2_WMTZR.pdf?up=1500444043
/space/moonphases1_WMTZT.pdf?up=1500441439
/science-projects/moon-craters_WMTZZ.pdf?up=1466611200
/poems/harvest-moon_MOONS.pdf?up=1466611200
/space/neil-armstrong-coloring_GGSTD.pdf?up=1501573553
/space/buzz-aldrin-coloring_WESRT.pdf?up=1502087067
/womens-history/sally-ride-coloring_HHYAS.pdf?up=1500443897
/black-history/guion-bluford-coloring_AWWWS.pdf?up=1501238774
/space/John-glenn-coloring_YYYAS.pdf?up=1540553608
 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5:
cmd( 1):
processing https://www.downloadtest.com/space/solarsystem-scav-hunt_WMTTD.pdf?up=1531821654
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/space/solarsystem-scav-hunt_WMTTD.pdf?up=1531821654
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/space/planet-report_WMTTQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/space/planet-report_WMTTQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/space/to-the-moon-space-trivia_WMTTF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/space/scoot-solar-system_PLOKM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/space/cipher-wheel-solar-system_HOKIE.pdf?up=1493721021
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/space/cootie-catcher-solar-system_PISKV.pdf?up=1562927299
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/space/mercury_WMTTN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/space/venus_WMTTR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/space/earth_WMTTT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/space/mars_WMTTW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/space/jupiter_WMTWB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/space/saturn_WMTWD.pdf?up=1562927300
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/space/uranus_WMTWF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/space/neptune_WMTWQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/space/pluto_WMTWM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/space/sun_WMTWN.pdf?up=1520249440
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/space/moon_TMOON.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-dwarf-planets_WMTWR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-moons_WMTWT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-constellations_WMTWW.pdf?up=1478782973
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/space/solar-system-cut-glue_ABZHU.pdf?up=1500977085
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/space/solar-system-cut-glue_ABZHU.pdf?up=1500977085
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/space/planet-riddles_WMTZD.pdf?up=1520249756
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/space/solar-system-cut-outs_FWJSU.pdf?up=1500482687
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/space/weight-on-planets_WMTZF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/space/solarsystem-crossword_WMTZQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/space/space-questions_WMTZW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/space/seasons-orbit_PERTD.pdf?up=1530179488
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/space/moonphases3_WMTZM.pdf?up=1500446844
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/space/moonphases4_WMTZN.pdf?up=1500448927
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/space/moonphases2_WMTZR.pdf?up=1500444043
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/space/moonphases1_WMTZT.pdf?up=1500441439
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/science-projects/moon-craters_WMTZZ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/poems/harvest-moon_MOONS.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/space/neil-armstrong-coloring_GGSTD.pdf?up=1501573553
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/space/buzz-aldrin-coloring_WESRT.pdf?up=1502087067
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/womens-history/sally-ride-coloring_HHYAS.pdf?up=1500443897
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/black-history/guion-bluford-coloring_AWWWS.pdf?up=1501238774
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/space/John-glenn-coloring_YYYAS.pdf?up=1540553608
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/weather.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/weather.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/weather.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):
/weather/cloud-types-article_WMWFQ.pdf?up=1466611200
/weather/cloud-types-article_WMWFQ.pdf?up=1466611200
/weather/cloud-type-project_WMWFM.pdf?up=1466611200
/weather/cloud-type-project_WMWFM.pdf?up=1466611200
/weather/cloud-types_WMWFN.pdf?up=1466611200
/weather/cloud-types-color_WMWFR.pdf?up=1466611200
/reading-comp/5th-hurricanes_WBFFR.pdf?up=1504242395
/weather/waterspouts_WMWFW.pdf?up=1466611200
/reading-comp/6th-terrifying-tsunamis_WATER.pdf?up=1466611200
/weather/weather-instruments_WMWFZ.pdf?up=1466611200
/weather/weather-instruments-2_WMWQB.pdf?up=1466611200
/weather/water-cycle_WMWQD.pdf?up=1466611200
/weather/weather-graph_WMWQF.pdf?up=1466611200
/weather/todays-weather_FHWJL.pdf?up=1466611200
/weather/weather-forecast-sheet_WMWQQ.pdf?up=1466611200
/weather/weather-scav-hunt_WMWDR.pdf?up=1571641729
/weather/weather-scav-hunt_WMWDR.pdf?up=1571641729
/weather/weather-word-problems_WMWDT.pdf?up=1466611200
/weather/weather-chart_WMWDW.pdf?up=1466611200
/weather/weather-chart-color_WMWDZ.pdf?up=1466611200
/songs/cloud-song_WMWFB.pdf?up=1466611200
 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5:
cmd( 1):
processing https://www.downloadtest.com/weather/cloud-types-article_WMWFQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/weather/cloud-types-article_WMWFQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/weather/cloud-type-project_WMWFM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/weather/cloud-type-project_WMWFM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/weather/cloud-types_WMWFN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/weather/cloud-types-color_WMWFR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-hurricanes_WBFFR.pdf?up=1504242395
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/weather/waterspouts_WMWFW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/6th-terrifying-tsunamis_WATER.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/weather/weather-instruments_WMWFZ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/weather/weather-instruments-2_WMWQB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/weather/water-cycle_WMWQD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/weather/weather-graph_WMWQF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/weather/todays-weather_FHWJL.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/weather/weather-forecast-sheet_WMWQQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/weather/weather-scav-hunt_WMWDR.pdf?up=1571641729
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/weather/weather-scav-hunt_WMWDR.pdf?up=1571641729
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/weather/weather-word-problems_WMWDT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/weather/weather-chart_WMWDW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/weather/weather-chart-color_WMWDZ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/songs/cloud-song_WMWFB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/full-science.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/full-science.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/full-science.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):

 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
---Step5:
cmd( 1):
processing https://www.downloadtest.com
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/states-individual.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/states-individual.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/states-individual.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):

 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
---Step5:
cmd( 1):
processing https://www.downloadtest.com
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/explorers.html  
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/explorers.html  
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/explorers.html   |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):
/explorers/explorer-report_WMWMN.pdf?up=1466611200
/explorers/explorer-report_WMWMN.pdf?up=1466611200
/cut-outs/columbus-cutout_WMWMT.pdf?up=1466611200
/cut-outs/columbus-cutout_WMWMT.pdf?up=1466611200
/holiday/columbus-ships_WMDNB.pdf?up=1504163500
/holiday/columbus-mini-book_WMWMW.pdf?up=1466611200
/reading-comp/5th-magellan_WMWNB.pdf?up=1466611200
/explorers/magellan-map_WMWND.pdf?up=1485942704
/explorers/cipher-wheel-magellan_ZYPHR.pdf?up=1493801575
/reading-comp/5th-hudson_WMWQW.pdf?up=1466611200
/reading-comp/5th-hudson_WMWQW.pdf?up=1466611200
/explorers/hudson-map_WMWQZ.pdf?up=1466611200
/explorers/cipher-wheel-hudson_KAILM.pdf?up=1507712008
 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5:
cmd( 1):
processing https://www.downloadtest.com/explorers/explorer-report_WMWMN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/explorers/explorer-report_WMWMN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/cut-outs/columbus-cutout_WMWMT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/cut-outs/columbus-cutout_WMWMT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/holiday/columbus-ships_WMDNB.pdf?up=1504163500
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/holiday/columbus-mini-book_WMWMW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-magellan_WMWNB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/explorers/magellan-map_WMWND.pdf?up=1485942704
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/explorers/cipher-wheel-magellan_ZYPHR.pdf?up=1493801575
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-hudson_WMWQW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-hudson_WMWQW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/explorers/hudson-map_WMWQZ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/explorers/cipher-wheel-hudson_KAILM.pdf?up=1507712008
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/landforms.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/landforms.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/landforms.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):
/landforms/landforms-1_WMWNF.pdf?up=1486029388
/landforms/landforms-1_WMWNF.pdf?up=1486029388
/landforms/bodies-of-water-1_WMWNQ.pdf?up=1486029371
/landforms/bodies-of-water-1_WMWNQ.pdf?up=1486029371
/landforms/landforms-matching-game_WMWNM.pdf?up=1466611200
/landforms/bodies-of-water-matching-game_WMWNN.pdf?up=1466611200
/landforms/landform-cards_WMWNR.pdf?up=1466611200
/landforms/bodies-of-water-cards_WMWNT.pdf?up=1466611200
/landforms/landform-matching_WMWNW.pdf?up=1466611200
/landforms/land-and-water-formations_WMWNZ.pdf?up=1466611200
/landforms/landform-definitions_WMWRF.pdf?up=1466611200
/landforms/landform-definitions_WMWRF.pdf?up=1466611200
/landforms/landform-fitb_WMWRQ.pdf?up=1466611200
/landforms/land-and-water-formations-2_WMWRM.pdf?up=1466611200
/landforms/landforms-2_WMWRN.pdf?up=1466611200
 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5:
cmd( 1):
processing https://www.downloadtest.com/landforms/landforms-1_WMWNF.pdf?up=1486029388
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/landforms/landforms-1_WMWNF.pdf?up=1486029388
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/landforms/bodies-of-water-1_WMWNQ.pdf?up=1486029371
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/landforms/bodies-of-water-1_WMWNQ.pdf?up=1486029371
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/landforms/landforms-matching-game_WMWNM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/landforms/bodies-of-water-matching-game_WMWNN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/landforms/landform-cards_WMWNR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/landforms/bodies-of-water-cards_WMWNT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/landforms/landform-matching_WMWNW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/landforms/land-and-water-formations_WMWNZ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/landforms/landform-definitions_WMWRF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/landforms/landform-definitions_WMWRF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/landforms/landform-fitb_WMWRQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/landforms/land-and-water-formations-2_WMWRM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/landforms/landforms-2_WMWRN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/maps.html  
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/maps.html  
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/maps.html   |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):
/maps/usa-blank_WMWTD.pdf?up=1466611200
/maps/usa-blank_WMWTD.pdf?up=1466611200
/maps/usa-states_WMWTF.pdf?up=1466611200
/maps/usa-states_WMWTF.pdf?up=1466611200
/maps/maps-usa-states-numbered2_ADJXE.pdf?up=1567592638
/maps/usa-capitals_WMWTM.pdf?up=1466611200
/maps/usa-abbreviations_WMWTN.pdf?up=1466611200
/maps/usa-latitude-longitude-map_WMWTR.pdf?up=1466611200
/maps/usa-latitude-longitude-map2_WMWTT.pdf?up=1466611200
/maps/usa-latitude-longitude-map3_WMWTW.pdf?up=1466611200
/maps/us-regions_VDJWL.pdf?up=1466611200
/colonial-america/colonial-map1775-blank_BLANK.pdf?up=1466611200
/colonial-america/colonial-map1775_COLNY.pdf?up=1466611200
/canada/maps-canada-blank_WMWWB.pdf?up=1524724169
/canada/maps-canada-provinces_WMWWD.pdf?up=1524725124
/canada/maps-canada-capitals_WMWWF.pdf?up=1524724494
/canada/maps-canada-provinces-numbered_WMWWQ.pdf?up=1466611200
/canada/maps-canada-capitals-numbered_WMWWM.pdf?up=1524724354
/canada/canada-directions_WMWWN.pdf?up=1466611200
/canada/maps-canada-lat-long01_WMWWR.pdf?up=1524724635
/canada/maps-canada-lat-long02_WMWWT.pdf?up=1525758420
/uk/maps-uk-blank_WMWWZ.pdf?up=1466611200
/uk/maps-uk-labeled_WMWZB.pdf?up=1466611200
/uk/uk-directions_WMWZD.pdf?up=1466611200
/uk/maps-uk-lat-long1_WMWZF.pdf?up=1466611200
/australia/maps-australia-nz-labeled_WMWZM.pdf?up=1524723805
/australia/maps-australia-nz-capitals_WMWZN.pdf?up=1524723792
/australia/maps-australia-nz-numbered_WMWZR.pdf?up=1525770616
/australia/australia-directions_WMWZT.pdf?up=1466611200
/australia/maps-australia-nz-blank_WMWZW.pdf?up=1525164275
/australia/maps-australia-nz-lat-and-long_WMWZZ.pdf?up=1525770315
/australia/maps-australia-nz-lat-and-long2_WMZBB.pdf?up=1525769917
/maps/map-mexico-blank_CKKSM.pdf?up=1466611200
/maps/map-mexico-blank-spanish_FHSIS.pdf?up=1466611200
/maps/map-mexico-labeled_KKZAZ.pdf?up=1466611200
/maps/map-mexico-labeled-spanish_GCUXX.pdf?up=1466611200
/maps/mexico-states_MOSOM.pdf?up=1511866274
/maps/mexico-numbered_MOMSN.pdf?up=1511867560
/maps/map-china-unlabeled_XSUIK.pdf?up=1493112347
/maps/map-china-labeled-simple_ISWIX.pdf?up=1466611200
/maps/maps-china-blank1_LSHJA.pdf?up=1466611200
/maps/maps-china-blank2_SKRET.pdf?up=1466611200
/maps/maps-china-provinces-labeled_DFOYT.pdf?up=1517829798
/maps/maps-china-provinces-labeled-country-only_WJSUI.pdf?up=1466611200
/geography/a-geography-quiz_WMZBF.pdf?up=1506682151
/maps/continents-oceans_WMZBN.pdf?up=1503417198
/maps/continents-oceans_WMZBN.pdf?up=1503417198
/maps/world-numbered_WMZBR.pdf?up=1503418313
/maps/continents-oceans-blank_WMZBT.pdf?up=1466611200
/maps/world-map-4-page_WMZBW.pdf?up=1503419381
/maps/north-america_WMZDB.pdf?up=1466611200
/maps/north-america-numbers_WMZDD.pdf?up=1466611200
/maps/north-america-directions_WMZDF.pdf?up=1466611200
/maps/north-america-blank_WMZDQ.pdf?up=1466611200
/maps/central-america_MOMCA.pdf?up=1466611200
/maps/south-america_WMZDN.pdf?up=1466611200
/maps/south-america-numbers_WMZDR.pdf?up=1466611200
/maps/south-america-directions_WMZDT.pdf?up=1474540130
/maps/south-america-questions_WMZDW.pdf?up=1466611200
/maps/south-america-blank_WMZDZ.pdf?up=1466611200
/maps/europe_WMZFD.pdf?up=1466611200
/maps/europe-numbers_WMZFF.pdf?up=1466611200
/maps/europe-questions_WMZFQ.pdf?up=1466611200
/maps/europe-blank_WMZFM.pdf?up=1466611200
/maps/africa_WMZFN.pdf?up=1579594227
/maps/africa-blank_WMZFR.pdf?up=1466611200
/maps/asia_WMZFW.pdf?up=1509697692
/maps/asia-blank_WMZFZ.pdf?up=1466611200
/maps/australia_WMZQD.pdf?up=1466611200
/maps/australia-blank_WMZQF.pdf?up=1466611200
/maps/australia-blank2_WMZQQ.pdf?up=1466611200
/maps/indian-subcontinent_INDSC.pdf?up=1466611200
/maps/india-states_SUTOI.pdf?up=1509465450
/maps/antarctica_WMZQN.pdf?up=1466611200
/maps/antarctica-blank_WMZQR.pdf?up=1466611200
 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5:
cmd( 1):
processing https://www.downloadtest.com/maps/usa-blank_WMWTD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/maps/usa-blank_WMWTD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/maps/usa-states_WMWTF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/maps/usa-states_WMWTF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/maps/maps-usa-states-numbered2_ADJXE.pdf?up=1567592638
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/maps/usa-capitals_WMWTM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/maps/usa-abbreviations_WMWTN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/maps/usa-latitude-longitude-map_WMWTR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/maps/usa-latitude-longitude-map2_WMWTT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/maps/usa-latitude-longitude-map3_WMWTW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/maps/us-regions_VDJWL.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/colonial-america/colonial-map1775-blank_BLANK.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/colonial-america/colonial-map1775_COLNY.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/canada/maps-canada-blank_WMWWB.pdf?up=1524724169
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/canada/maps-canada-provinces_WMWWD.pdf?up=1524725124
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/canada/maps-canada-capitals_WMWWF.pdf?up=1524724494
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/canada/maps-canada-provinces-numbered_WMWWQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/canada/maps-canada-capitals-numbered_WMWWM.pdf?up=1524724354
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/canada/canada-directions_WMWWN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/canada/maps-canada-lat-long01_WMWWR.pdf?up=1524724635
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/canada/maps-canada-lat-long02_WMWWT.pdf?up=1525758420
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/uk/maps-uk-blank_WMWWZ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/uk/maps-uk-labeled_WMWZB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/uk/uk-directions_WMWZD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/uk/maps-uk-lat-long1_WMWZF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/australia/maps-australia-nz-labeled_WMWZM.pdf?up=1524723805
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/australia/maps-australia-nz-capitals_WMWZN.pdf?up=1524723792
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/australia/maps-australia-nz-numbered_WMWZR.pdf?up=1525770616
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/australia/australia-directions_WMWZT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/australia/maps-australia-nz-blank_WMWZW.pdf?up=1525164275
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/australia/maps-australia-nz-lat-and-long_WMWZZ.pdf?up=1525770315
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/australia/maps-australia-nz-lat-and-long2_WMZBB.pdf?up=1525769917
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/maps/map-mexico-blank_CKKSM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/maps/map-mexico-blank-spanish_FHSIS.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/maps/map-mexico-labeled_KKZAZ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/maps/map-mexico-labeled-spanish_GCUXX.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/maps/mexico-states_MOSOM.pdf?up=1511866274
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/maps/mexico-numbered_MOMSN.pdf?up=1511867560
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/maps/map-china-unlabeled_XSUIK.pdf?up=1493112347
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/maps/map-china-labeled-simple_ISWIX.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/maps/maps-china-blank1_LSHJA.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/maps/maps-china-blank2_SKRET.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/maps/maps-china-provinces-labeled_DFOYT.pdf?up=1517829798
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/maps/maps-china-provinces-labeled-country-only_WJSUI.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/geography/a-geography-quiz_WMZBF.pdf?up=1506682151
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/maps/continents-oceans_WMZBN.pdf?up=1503417198
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/maps/continents-oceans_WMZBN.pdf?up=1503417198
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/maps/world-numbered_WMZBR.pdf?up=1503418313
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/maps/continents-oceans-blank_WMZBT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/maps/world-map-4-page_WMZBW.pdf?up=1503419381
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/maps/north-america_WMZDB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/maps/north-america-numbers_WMZDD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/maps/north-america-directions_WMZDF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/maps/north-america-blank_WMZDQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/maps/central-america_MOMCA.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/maps/south-america_WMZDN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/maps/south-america-numbers_WMZDR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/maps/south-america-directions_WMZDT.pdf?up=1474540130
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/maps/south-america-questions_WMZDW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/maps/south-america-blank_WMZDZ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/maps/europe_WMZFD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/maps/europe-numbers_WMZFF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/maps/europe-questions_WMZFQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/maps/europe-blank_WMZFM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/maps/africa_WMZFN.pdf?up=1579594227
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/maps/africa-blank_WMZFR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/maps/asia_WMZFW.pdf?up=1509697692
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/maps/asia-blank_WMZFZ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/maps/australia_WMZQD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/maps/australia-blank_WMZQF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/maps/australia-blank2_WMZQQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/maps/indian-subcontinent_INDSC.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/maps/india-states_SUTOI.pdf?up=1509465450
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/maps/antarctica_WMZQN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/maps/antarctica-blank_WMZQR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/map-skills.html  
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/map-skills.html  
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/map-skills.html   |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):
/maps/mapskills-castle_WMZNQ.pdf?up=1498717181
/maps/mapskills-castle_WMZNQ.pdf?up=1498717181
/maps/campground-map_WMZNM.pdf?up=1466611200
/maps/campground-map_WMZNM.pdf?up=1466611200
/maps/neighborhood-map_WMZNN.pdf?up=1466611200
/maps/neighborhood-map2_WMZNR.pdf?up=1466611200
/maps/intermediate-directions-map_WMZNT.pdf?up=1466611200
/maps/compass-points_WMZNW.pdf?up=1466611200
/maps/where-i-live_WMZNZ.pdf?up=1470043214
/maps/resource-map_WMZRB.pdf?up=1466611200
/maps/using-scale-basic-map_WMZRD.pdf?up=1466611200
/maps/using-scale-map_WMZRF.pdf?up=1485411249
/maps/usa-latitude-longitude-map_WMWTR.pdf?up=1466611200
/maps/usa-latitude-longitude-map_WMWTR.pdf?up=1466611200
/maps/usa-latitude-longitude-map2_WMZRN.pdf?up=1466611200
/maps/usa-latitude-longitude-map3_WMZRR.pdf?up=1466611200
/australia/maps-australia-nz-lat-and-long_WMZRT.pdf?up=1525770316
/australia/maps-australia-nz-lat-and-long2_WMZRW.pdf?up=1525769918
/canada/maps-canada-lat-long02_WMZRZ.pdf?up=1466611200
/maps/latitude-longitude_WMZTB.pdf?up=1470043034
 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5:
cmd( 1):
processing https://www.downloadtest.com/maps/mapskills-castle_WMZNQ.pdf?up=1498717181
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/maps/mapskills-castle_WMZNQ.pdf?up=1498717181
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/maps/campground-map_WMZNM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/maps/campground-map_WMZNM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/maps/neighborhood-map_WMZNN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/maps/neighborhood-map2_WMZNR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/maps/intermediate-directions-map_WMZNT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/maps/compass-points_WMZNW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/maps/where-i-live_WMZNZ.pdf?up=1470043214
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/maps/resource-map_WMZRB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/maps/using-scale-basic-map_WMZRD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/maps/using-scale-map_WMZRF.pdf?up=1485411249
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/maps/usa-latitude-longitude-map_WMWTR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/maps/usa-latitude-longitude-map_WMWTR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/maps/usa-latitude-longitude-map2_WMZRN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/maps/usa-latitude-longitude-map3_WMZRR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/australia/maps-australia-nz-lat-and-long_WMZRT.pdf?up=1525770316
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/australia/maps-australia-nz-lat-and-long2_WMZRW.pdf?up=1525769918
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/canada/maps-canada-lat-long02_WMZRZ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/maps/latitude-longitude_WMZTB.pdf?up=1470043034
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/full-social-studies.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/full-social-studies.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/full-social-studies.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):

 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
---Step5:
cmd( 1):
processing https://www.downloadtest.com
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/spring.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/spring.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/spring.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):
/spring/word-mix-up-spring-primary_MIXUP.pdf?up=1466611200
/spring/word-mix-up-spring-primary_MIXUP.pdf?up=1466611200
/spring/word-mix-up-spring-intermediate_RAINY.pdf?up=1466611200
/spring/word-mix-up-spring-intermediate_RAINY.pdf?up=1466611200
/addition-sum-game/sum-game-flower_BANHU.pdf?up=1553153573
/spring/spring-hat-bw_SPRBW.pdf?up=1506582995
/spring/spring-fun-wordsearch_SUNNY.pdf?up=1466611200
/wordsearch/baseball-wordsearch_BASES.pdf?up=1466611200
/coloring-pages/coloring-flowers_BLOOM.pdf?up=1583818075
/spring/abc-order-spring-basic_SPRUN.pdf?up=1466611200
/spring/abc-order-spring-advanced_ABCAB.pdf?up=1466611200
/reading-comp/1st-will-and-bud_TZZNT.pdf?up=1581322409
/reading-comp/2nd-rock-collecting_TZZZT.pdf?up=1489313009
/reading-comp/2nd-tulips_WBBDT.pdf?up=1466611200
/reading-comp/3rd-rainy-day-fun_RAINY.pdf?up=1466611200
/reading-comp/3rd-monarch_WDFWT.pdf?up=1493635262
/poems/2nd-sense-sational-day_SENSE.pdf?up=1556082438
/reading-comp/4th-playing-catch_WBDNM.pdf?up=1466611200
/reading-comp/5th-field-day-champion_WBFMZ.pdf?up=1466611200
/reading-comp/5th-ladybugs_WDFWN.pdf?up=1466611200
/addition/mystery-picture-addition-bird_ABZGT.pdf?up=1499753115
/addition/mystery-picture-addition-bird_ABZGT.pdf?up=1499753115
/addition/mystery-picture-addition-ladybug_BVCAR.pdf?up=1499753104
/subtraction/mystery-picture-subtraction-butterfly_MMMAZ.pdf?up=1499671403
/subtraction/mystery-picture-subtraction-ladybug_ABZGT.pdf?up=1499671412
/subtraction/subtraction-minuends-up-to-20-3_OCTUA.pdf?up=1539254988
/multiplication/mystery-picture-multiplication-bird_ONHZJ.pdf?up=1499332649
/multiplication/mystery-picture-multiplication-ladybug_ABZVF.pdf?up=1499332661
/division/mystery-picture-division-ladybug_ABZGT.pdf?up=1499246854
/fractions/mystery-picture-fractions-ladybug_AGHBD.pdf?up=1500892465
/mystery-graph-picture/flower-easy-graph_WQWQF.pdf?up=1466611200
/calendars/march-calendar_WNFNT.pdf?up=1466611200
/calendars/april-calendar_WNFNW.pdf?up=1466611200
/calendars/may-calendar_WNFNZ.pdf?up=1466611200
/spring/make-sentences-sight-words-spring_SPRIN.pdf?up=1466611200
/spring/season-spring-build-a-sentence-flowers-grow-in-the-spring_FLOWE.pdf?up=1466611200
/sentences-basic/build-a-sentence-can-you-fly-the-kite_KITEK.pdf?up=1499934974
/spring/spring-writing-prompts_BLOSS.pdf?up=1551092579
/five-senses/five-senses-spring_DTAHU.pdf?up=1466611200
/writing-storypics/spring-kite-tree-storypic_TREES.pdf?up=1466611200
/writing-storypics/storypic-butterfly-girl_BTFLY.pdf?up=1466611200
/sticker-charts/sticker-chart-april-umbrella_WNQRQ.pdf?up=1466611200
/sticker-charts/sticker-chart-may-flowers_WNQRM.pdf?up=1466611200
/reminder-wristbands/reminder-bracelets-blank-flowers_HWKSI.pdf?up=1466611200
 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5:
cmd( 1):
processing https://www.downloadtest.com/spring/word-mix-up-spring-primary_MIXUP.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/spring/word-mix-up-spring-primary_MIXUP.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/spring/word-mix-up-spring-intermediate_RAINY.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/spring/word-mix-up-spring-intermediate_RAINY.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/addition-sum-game/sum-game-flower_BANHU.pdf?up=1553153573
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/spring/spring-hat-bw_SPRBW.pdf?up=1506582995
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/spring/spring-fun-wordsearch_SUNNY.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/wordsearch/baseball-wordsearch_BASES.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/coloring-pages/coloring-flowers_BLOOM.pdf?up=1583818075
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/spring/abc-order-spring-basic_SPRUN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/spring/abc-order-spring-advanced_ABCAB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/1st-will-and-bud_TZZNT.pdf?up=1581322409
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/2nd-rock-collecting_TZZZT.pdf?up=1489313009
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/2nd-tulips_WBBDT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-rainy-day-fun_RAINY.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-monarch_WDFWT.pdf?up=1493635262
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/poems/2nd-sense-sational-day_SENSE.pdf?up=1556082438
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/4th-playing-catch_WBDNM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-field-day-champion_WBFMZ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/5th-ladybugs_WDFWN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/addition/mystery-picture-addition-bird_ABZGT.pdf?up=1499753115
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/addition/mystery-picture-addition-bird_ABZGT.pdf?up=1499753115
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/addition/mystery-picture-addition-ladybug_BVCAR.pdf?up=1499753104
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/subtraction/mystery-picture-subtraction-butterfly_MMMAZ.pdf?up=1499671403
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/subtraction/mystery-picture-subtraction-ladybug_ABZGT.pdf?up=1499671412
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/subtraction/subtraction-minuends-up-to-20-3_OCTUA.pdf?up=1539254988
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/multiplication/mystery-picture-multiplication-bird_ONHZJ.pdf?up=1499332649
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/multiplication/mystery-picture-multiplication-ladybug_ABZVF.pdf?up=1499332661
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/division/mystery-picture-division-ladybug_ABZGT.pdf?up=1499246854
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/fractions/mystery-picture-fractions-ladybug_AGHBD.pdf?up=1500892465
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/mystery-graph-picture/flower-easy-graph_WQWQF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/calendars/march-calendar_WNFNT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/calendars/april-calendar_WNFNW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/calendars/may-calendar_WNFNZ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/spring/make-sentences-sight-words-spring_SPRIN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/spring/season-spring-build-a-sentence-flowers-grow-in-the-spring_FLOWE.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sentences-basic/build-a-sentence-can-you-fly-the-kite_KITEK.pdf?up=1499934974
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/spring/spring-writing-prompts_BLOSS.pdf?up=1551092579
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/five-senses/five-senses-spring_DTAHU.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing-storypics/spring-kite-tree-storypic_TREES.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing-storypics/storypic-butterfly-girl_BTFLY.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sticker-charts/sticker-chart-april-umbrella_WNQRQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/sticker-charts/sticker-chart-may-flowers_WNQRM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reminder-wristbands/reminder-bracelets-blank-flowers_HWKSI.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/mothers-day.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/mothers-day.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/mothers-day.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):
/mothers-day/mom-letter_LETTE.pdf?up=1466611200
/mothers-day/mom-letter_LETTE.pdf?up=1466611200
/mothers-day/mom-is-special_SPECI.pdf?up=1466611200
/mothers-day/mom-is-special_SPECI.pdf?up=1466611200
/mothers-day/primary-about-mom_ABOUT.pdf?up=1466611200
/mothers-day/award-mother_AWARD.pdf?up=1466611200
/awards/best-mom-trophy_MOOKA.pdf?up=1533622609
/mothers-day/grandma-is-special_GRAMS.pdf?up=1466611200
/mothers-day/award-granny_GRAMS.pdf?up=1466611200
/mothers-day/mothers-day-card-grandma1_GAMMA.pdf?up=1466611200
/mothers-day/mothers-day-card-grandma2_GRNNY.pdf?up=1525756143
/awards/best-grandma-trophy_TPCBE.pdf?up=1533622483
/mothers-day/aunt-is-special_AUNTS.pdf?up=1466611200
/mothers-day/award-aunt_AUNTT.pdf?up=1466611200
/mothers-day/mothers-day-card-aunt1_AUNTY.pdf?up=1466611200
/mothers-day/mothers-day-card-aunt2_AUNTS.pdf?up=1525756083
/mothers-day/stepmother-is-special_VSPEC.pdf?up=1466611200
/mothers-day/award-stepmom_STEPM.pdf?up=1466611200
/mothers-day/mothers-day-card-step-mother1_AIXKS.pdf?up=1525756195
/mothers-day/mothers-day-card-step-mother2_AHXUW.pdf?up=1466611200
/mothers-day/mothers-day-card-step-mother3_CJISL.pdf?up=1466611200
/mothers-day/mothers-day-card4_MOMOM.pdf?up=1466611200
/mothers-day/mothers-day-card4_MOMOM.pdf?up=1466611200
/mothers-day/mothers-day-card6_MOMMA.pdf?up=1466611200
/mothers-day/mothers-day-card2_MOMMM.pdf?up=1525756402
/mothers-day/mothers-day-card5_MOOOM.pdf?up=1525756526
/mothers-day/mothers-day-card3_MOMMY.pdf?up=1525756453
/mothers-day/mothers-day-card1_MAMMA.pdf?up=1525756337
/reading-comp/3rd-mothers-day-gift_MGIFT.pdf?up=1466611200
/poems/mothers-day-cake_MCAKE.pdf?up=1466611200
/poems/mothers-day-letter-poem_LIANA.pdf?up=1466611200
/poems/it-was-mothers-day_MOTHR.pdf?up=1466611200
/poems/mothers-day-gift_MGIFT.pdf?up=1466611200
/writing-storypics/mom-hug-storypic_MOMMA.pdf?up=1466611200
 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5:
cmd( 1):
processing https://www.downloadtest.com/mothers-day/mom-letter_LETTE.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/mothers-day/mom-letter_LETTE.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/mothers-day/mom-is-special_SPECI.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/mothers-day/mom-is-special_SPECI.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/mothers-day/primary-about-mom_ABOUT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/mothers-day/award-mother_AWARD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/awards/best-mom-trophy_MOOKA.pdf?up=1533622609
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/mothers-day/grandma-is-special_GRAMS.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/mothers-day/award-granny_GRAMS.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/mothers-day/mothers-day-card-grandma1_GAMMA.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/mothers-day/mothers-day-card-grandma2_GRNNY.pdf?up=1525756143
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/awards/best-grandma-trophy_TPCBE.pdf?up=1533622483
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/mothers-day/aunt-is-special_AUNTS.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/mothers-day/award-aunt_AUNTT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/mothers-day/mothers-day-card-aunt1_AUNTY.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/mothers-day/mothers-day-card-aunt2_AUNTS.pdf?up=1525756083
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/mothers-day/stepmother-is-special_VSPEC.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/mothers-day/award-stepmom_STEPM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/mothers-day/mothers-day-card-step-mother1_AIXKS.pdf?up=1525756195
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/mothers-day/mothers-day-card-step-mother2_AHXUW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/mothers-day/mothers-day-card-step-mother3_CJISL.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/mothers-day/mothers-day-card4_MOMOM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/mothers-day/mothers-day-card4_MOMOM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/mothers-day/mothers-day-card6_MOMMA.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/mothers-day/mothers-day-card2_MOMMM.pdf?up=1525756402
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/mothers-day/mothers-day-card5_MOOOM.pdf?up=1525756526
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/mothers-day/mothers-day-card3_MOMMY.pdf?up=1525756453
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/mothers-day/mothers-day-card1_MAMMA.pdf?up=1525756337
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/reading-comp/3rd-mothers-day-gift_MGIFT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/poems/mothers-day-cake_MCAKE.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/poems/mothers-day-letter-poem_LIANA.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/poems/it-was-mothers-day_MOTHR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/poems/mothers-day-gift_MGIFT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing-storypics/mom-hug-storypic_MOMMA.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/fathers-day.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/fathers-day.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/fathers-day.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):
/fathers-day/father-letter_DADDD.pdf?up=1466611200
/fathers-day/father-letter_DADDD.pdf?up=1466611200
/fathers-day/primary-about-dad_DADDD.pdf?up=1466611200
/fathers-day/primary-about-dad_DADDD.pdf?up=1466611200
/fathers-day/award-dad_DADDY.pdf?up=1466611200
/fathers-day/award-father_DADDY.pdf?up=1466611200
/awards/best-dad-trophy_DWYAS.pdf?up=1533622587
/fathers-day/primary-about-grandpa_GRAMPS.pdf?up=1466611200
/fathers-day/award-grandpa_GRAMP.pdf?up=1466611200
/fathers-day/fathers-day-card-grandpa1_GRAMPS.pdf?up=1525328663
/fathers-day/fathers-day-card-grandpa2_GRAND.pdf?up=1525328721
/awards/best-grandpa-trophy_EKSAN.pdf?up=1533622502
/fathers-day/primary-about-uncle_UNCLE.pdf?up=1466611200
/fathers-day/award-uncle_UNCLE.pdf?up=1466611200
/fathers-day/fathers-day-card-uncle1_UNCLE.pdf?up=1525328770
/fathers-day/fathers-day-card-uncle2_UNKKY.pdf?up=1525328809
/fathers-day/primary-about-stepdad_STEPD.pdf?up=1466611200
/fathers-day/award-stepdad_STEPD.pdf?up=1466611200
/fathers-day/fathers-day-stepfather-card1_GJDIE.pdf?up=1525329222
/fathers-day/fathers-day-stepfather-card2_CKSUD.pdf?up=1525329289
/fathers-day/fathers-day-stepfather-card3_CGSUI.pdf?up=1466611200
/fathers-day/fathers-day-card3_DDADD.pdf?up=1525328936
/fathers-day/fathers-day-card3_DDADD.pdf?up=1525328936
/fathers-day/fathers-day-card5_DADIO.pdf?up=1525329055
/fathers-day/fathers-day-father-card7_HHHAS.pdf?up=1527699555
/fathers-day/fathers-day-card1_POPPA.pdf?up=1466611200
/fathers-day/fathers-day-card2_DADDY.pdf?up=1525758148
/fathers-day/fathers-day-card7_POPPY.pdf?up=1525329173
/fathers-day/fathers-day-card6_DADDA.pdf?up=1525329103
/fathers-day/fathers-day-card4_POPPS.pdf?up=1525329005
/fathers-day/3rd-fathers-day-fort_FFORT.pdf?up=1466611200
/poems/dads-gift_DGIFT.pdf?up=1466611200
/poems/superstar-dad_SUPER.pdf?up=1466611200
/poems/my-dad_MYDAD.pdf?up=1466611200
/poems/daddy-lets-play_DPLAY.pdf?up=1466611200
/writing-storypics/dad-mug-storypic_DDMUG.pdf?up=1466611200
 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5:
cmd( 1):
processing https://www.downloadtest.com/fathers-day/father-letter_DADDD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/fathers-day/father-letter_DADDD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/fathers-day/primary-about-dad_DADDD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/fathers-day/primary-about-dad_DADDD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/fathers-day/award-dad_DADDY.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/fathers-day/award-father_DADDY.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/awards/best-dad-trophy_DWYAS.pdf?up=1533622587
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/fathers-day/primary-about-grandpa_GRAMPS.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/fathers-day/award-grandpa_GRAMP.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/fathers-day/fathers-day-card-grandpa1_GRAMPS.pdf?up=1525328663
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/fathers-day/fathers-day-card-grandpa2_GRAND.pdf?up=1525328721
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/awards/best-grandpa-trophy_EKSAN.pdf?up=1533622502
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/fathers-day/primary-about-uncle_UNCLE.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/fathers-day/award-uncle_UNCLE.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/fathers-day/fathers-day-card-uncle1_UNCLE.pdf?up=1525328770
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/fathers-day/fathers-day-card-uncle2_UNKKY.pdf?up=1525328809
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/fathers-day/primary-about-stepdad_STEPD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/fathers-day/award-stepdad_STEPD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/fathers-day/fathers-day-stepfather-card1_GJDIE.pdf?up=1525329222
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/fathers-day/fathers-day-stepfather-card2_CKSUD.pdf?up=1525329289
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/fathers-day/fathers-day-stepfather-card3_CGSUI.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/fathers-day/fathers-day-card3_DDADD.pdf?up=1525328936
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/fathers-day/fathers-day-card3_DDADD.pdf?up=1525328936
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/fathers-day/fathers-day-card5_DADIO.pdf?up=1525329055
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/fathers-day/fathers-day-father-card7_HHHAS.pdf?up=1527699555
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/fathers-day/fathers-day-card1_POPPA.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/fathers-day/fathers-day-card2_DADDY.pdf?up=1525758148
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/fathers-day/fathers-day-card7_POPPY.pdf?up=1525329173
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/fathers-day/fathers-day-card6_DADDA.pdf?up=1525329103
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/fathers-day/fathers-day-card4_POPPS.pdf?up=1525329005
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/fathers-day/3rd-fathers-day-fort_FFORT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/poems/dads-gift_DGIFT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/poems/superstar-dad_SUPER.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/poems/my-dad_MYDAD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/poems/daddy-lets-play_DPLAY.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/writing-storypics/dad-mug-storypic_DDMUG.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/calendars.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/calendars.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/calendars.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):
/calendars/january-hockey-calendar_WNFNM.pdf?up=1466611200
/calendars/january-hockey-calendar_WNFNM.pdf?up=1466611200
/calendars/january-calendar_WNFNN.pdf?up=1466611200
/calendars/january-calendar_WNFNN.pdf?up=1466611200
/calendars/february-calendar_WNFNR.pdf?up=1466611200
/calendars/march-calendar_WNFNT.pdf?up=1466611200
/calendars/april-calendar_WNFNW.pdf?up=1466611200
/calendars/may-calendar_WNFNZ.pdf?up=1466611200
/calendars/june-calendar_WNFRB.pdf?up=1466611200
/calendars/july-calendar-beach_WNFRD.pdf?up=1466611200
/calendars/july-calendar_WNFRF.pdf?up=1466611200
/calendars/august-calendar_WNFRQ.pdf?up=1466611200
/calendars/september-calendar_WNFMQ.pdf?up=1466611200
/calendars/september-calendar_WNFMQ.pdf?up=1466611200
/calendars/october-halloween-calendar_WNFMM.pdf?up=1466611200
/calendars/october-scarecrow-calendar_WNFMN.pdf?up=1466611200
/calendars/november-calendar_WNFMR.pdf?up=1466611200
/calendars/november-calendar-squirrel_WNFMT.pdf?up=1466611200
/calendars/december-christmas-calendar_WNFMW.pdf?up=1466611200
/calendars/december-calendar-winter_WNFMZ.pdf?up=1466611200
/calendars/months-poem_WNFNB.pdf?up=1579609973
/calendars/word-wall-week_LIAKO.pdf?up=1466611200
/calendars/word-wall-calender_KAILO.pdf?up=1466611200
 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5:
cmd( 1):
processing https://www.downloadtest.com/calendars/january-hockey-calendar_WNFNM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/calendars/january-hockey-calendar_WNFNM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/calendars/january-calendar_WNFNN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/calendars/january-calendar_WNFNN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/calendars/february-calendar_WNFNR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/calendars/march-calendar_WNFNT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/calendars/april-calendar_WNFNW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/calendars/may-calendar_WNFNZ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/calendars/june-calendar_WNFRB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/calendars/july-calendar-beach_WNFRD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/calendars/july-calendar_WNFRF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/calendars/august-calendar_WNFRQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/calendars/september-calendar_WNFMQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/calendars/september-calendar_WNFMQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/calendars/october-halloween-calendar_WNFMM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/calendars/october-scarecrow-calendar_WNFMN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/calendars/november-calendar_WNFMR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/calendars/november-calendar-squirrel_WNFMT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/calendars/december-christmas-calendar_WNFMW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/calendars/december-calendar-winter_WNFMZ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/calendars/months-poem_WNFNB.pdf?up=1579609973
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/calendars/word-wall-week_LIAKO.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/calendars/word-wall-calender_KAILO.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/full-holiday.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/full-holiday.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/full-holiday.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):

 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
---Step5:
cmd( 1):
processing https://www.downloadtest.com
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/brain-teasers.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/brain-teasers.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/brain-teasers.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):
/brainteasers/soccer-puzzle_WQRTB.pdf?up=1466611200
/brainteasers/soccer-puzzle_WQRTB.pdf?up=1466611200
/brainteasers/whats-the-word_WQRTD.pdf?up=1466611200
/brainteasers/whats-the-word_WQRTD.pdf?up=1466611200
/brainteasers/whats-the-word2_WQRTF.pdf?up=1466611200
/brainteasers/monkey-code_WQRTQ.pdf?up=1466611200
/brainteasers/five-letter-scramble_WQRTM.pdf?up=1466611200
/brainteasers/code-riddles_WQRTN.pdf?up=1466611200
/brainteasers/letter-comes-next_WQRTR.pdf?up=1466611200
/brainteasers/brainteaser1_WQRTT.pdf?up=1466611200
/brainteasers/riddles_WQRTW.pdf?up=1466611200
/brainteasers/weird-words_WQRTZ.pdf?up=1466611200
/brainteasers/rebus1_WQRWB.pdf?up=1466611200
/brainteasers/complete-the-proverbs_PROVE.pdf?up=1466611200
/brainteasers/football-teaser_WQRWD.pdf?up=1549274823
/brainteasers/acrostic-puzzles_WQRWF.pdf?up=1466611200
/brainteasers/acrostic-puzzles2_TWO22.pdf?up=1466611200
/brainteasers/brain-teaser-rebus-a1_GWISS.pdf?up=1466611200
/brainteasers/brain-teaser-rebus-a2_YEJOS.pdf?up=1466611200
/brainteasers/brain-teaser-rebus-a3_GWOXW.pdf?up=1466611200
/brainteasers/brain-teaser-rebus-b1_JIXWS.pdf?up=1466611200
/brainteasers/brain-teaser-rebus-b2_KOZZA.pdf?up=1466611200
/brainteasers/brain-teaser-rebus-b3_JWLSS.pdf?up=1466611200
/brainteasers/hink-pink-a_WQRWN.pdf?up=1466611200
/brainteasers/hink-pink-a_WQRWN.pdf?up=1466611200
/brainteasers/hink-pink-a2_WQRWR.pdf?up=1466611200
/brainteasers/hink-pink-b_WQRWT.pdf?up=1466611200
/brainteasers/hink-pink-c_WQRWW.pdf?up=1466611200
/brainteasers/hink-pink-c2_WQRWZ.pdf?up=1466611200
/brainteasers/hink-pink-c3_HINC3.pdf?up=1466611200
/brainteasers/hink-pink-d1_HIND1.pdf?up=1466611200
/brainteasers/puzzles_WQRZD.pdf?up=1466611200
/brainteasers/stroop-test_ANHUJ.pdf?up=1466611200
/brainteasers/calculator-words_WQRZQ.pdf?up=1466611200
/brainteasers/circle-10s_WQRZM.pdf?up=1466611200
/brainteasers/circle-25s_WQRZN.pdf?up=1466611200
/brainteasers/circle-100s_WQRZR.pdf?up=1466611200
/multiplication/challenge-how-old-am-i_WQRZT.pdf?up=1466611200
 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5:
cmd( 1):
processing https://www.downloadtest.com/brainteasers/soccer-puzzle_WQRTB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/brainteasers/soccer-puzzle_WQRTB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/brainteasers/whats-the-word_WQRTD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/brainteasers/whats-the-word_WQRTD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/brainteasers/whats-the-word2_WQRTF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/brainteasers/monkey-code_WQRTQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/brainteasers/five-letter-scramble_WQRTM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/brainteasers/code-riddles_WQRTN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/brainteasers/letter-comes-next_WQRTR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/brainteasers/brainteaser1_WQRTT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/brainteasers/riddles_WQRTW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/brainteasers/weird-words_WQRTZ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/brainteasers/rebus1_WQRWB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/brainteasers/complete-the-proverbs_PROVE.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/brainteasers/football-teaser_WQRWD.pdf?up=1549274823
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/brainteasers/acrostic-puzzles_WQRWF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/brainteasers/acrostic-puzzles2_TWO22.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/brainteasers/brain-teaser-rebus-a1_GWISS.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/brainteasers/brain-teaser-rebus-a2_YEJOS.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/brainteasers/brain-teaser-rebus-a3_GWOXW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/brainteasers/brain-teaser-rebus-b1_JIXWS.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/brainteasers/brain-teaser-rebus-b2_KOZZA.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/brainteasers/brain-teaser-rebus-b3_JWLSS.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/brainteasers/hink-pink-a_WQRWN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/brainteasers/hink-pink-a_WQRWN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/brainteasers/hink-pink-a2_WQRWR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/brainteasers/hink-pink-b_WQRWT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/brainteasers/hink-pink-c_WQRWW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/brainteasers/hink-pink-c2_WQRWZ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/brainteasers/hink-pink-c3_HINC3.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/brainteasers/hink-pink-d1_HIND1.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/brainteasers/puzzles_WQRZD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/brainteasers/stroop-test_ANHUJ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/brainteasers/calculator-words_WQRZQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/brainteasers/circle-10s_WQRZM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/brainteasers/circle-25s_WQRZN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/brainteasers/circle-100s_WQRZR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/multiplication/challenge-how-old-am-i_WQRZT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/addition-squares.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/addition-squares.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/addition-squares.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):
/addition-squares/2x2-level1-set1_WQRMW.pdf?up=1466611200
/addition-squares/2x2-level1-set1_WQRMW.pdf?up=1466611200
/addition-squares/2x2-level1-set2_WQRMZ.pdf?up=1466611200
/addition-squares/2x2-level1-set2_WQRMZ.pdf?up=1466611200
/addition-squares/2x2-level1-set3_WQRNB.pdf?up=1466611200
/addition-squares/2x2-level2-set1_WQRNF.pdf?up=1466611200
/addition-squares/2x2-level2-set2_WQRNQ.pdf?up=1466611200
/addition-squares/2x2-level2-set3_AAAAA.pdf?up=1466611200
/addition-squares/level3-set1_WQRNZ.pdf?up=1499762118
/addition-squares/level3-set1_WQRNZ.pdf?up=1499762118
/addition-squares/level3-set2_WQRRB.pdf?up=1499761125
/addition-squares/level3-set3_WQRRD.pdf?up=1499762709
 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5:
cmd( 1):
processing https://www.downloadtest.com/addition-squares/2x2-level1-set1_WQRMW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/addition-squares/2x2-level1-set1_WQRMW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/addition-squares/2x2-level1-set2_WQRMZ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/addition-squares/2x2-level1-set2_WQRMZ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/addition-squares/2x2-level1-set3_WQRNB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/addition-squares/2x2-level2-set1_WQRNF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/addition-squares/2x2-level2-set2_WQRNQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/addition-squares/2x2-level2-set3_AAAAA.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/addition-squares/level3-set1_WQRNZ.pdf?up=1499762118
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/addition-squares/level3-set1_WQRNZ.pdf?up=1499762118
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/addition-squares/level3-set2_WQRRB.pdf?up=1499761125
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/addition-squares/level3-set3_WQRRD.pdf?up=1499762709
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/mystery-graph-picture.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/mystery-graph-picture.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/mystery-graph-picture.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):
/mystery-graph-picture/apple-easy-graph_APLZE.pdf?up=1547724076
/mystery-graph-picture/apple-easy-graph_APLZE.pdf?up=1547724076
/mystery-graph-picture/sailboat-easy-graph_WQWQD.pdf?up=1466611200
/mystery-graph-picture/sailboat-easy-graph_WQWQD.pdf?up=1466611200
/mystery-graph-picture/buffalo-easy-graph_HYASW.pdf?up=1545296338
/mystery-graph-picture/castle-intermediate-graph_CLKAS.pdf?up=1548658914
/hanukkah/dreidel-easy-graph_WQWQQ.pdf?up=1466611200
/mystery-graph-picture/flower-easy-graph_WQWQF.pdf?up=1466611200
/mystery-graph-picture/heart-easy-graph_SWEET.pdf?up=1545123832
/mystery-graph-picture/pumpkin-easy-graph_WQWQM.pdf?up=1466611200
/mystery-graph-picture/pumpkin-easy-graph-2_JACKO.pdf?up=1547725151
/mystery-graph-picture/lion-easy-graph_WQWQN.pdf?up=1466611200
/mystery-graph-picture/graph-owl_WQWQR.pdf?up=1480675370
/mystery-graph-picture/graph-polygons_WQWQT.pdf?up=1480675396
/mystery-graph-picture/rabbit-easy-graph_TSIKL.pdf?up=1524037333
/winter/snowman-easy-graph_WQWQW.pdf?up=1466611200
/mystery-graph-picture/graph-xmastree_WQWMB.pdf?up=1480675441
/mystery-graph-picture/dino-easy-graph_XMSKA.pdf?up=1524037332
/mystery-graph-picture/firetruck-easy-graph_HSJKK.pdf?up=1525156746
/mystery-graph-picture/st.pattys-graph-V2_SREAN.pdf?up=1547725546
/mystery-graph-picture/st.pattys-graph_WGYAZ.pdf?up=1548667937
/mystery-graph-picture/graph-lincoln_WQWMD.pdf?up=1480675343
/mystery-graph-picture/graph-santa_WQWMF.pdf?up=1466611200
/mystery-graph-picture/bus-mystery-graph_WQWMQ.pdf?up=1466611200
/mystery-graph-picture/school-mystery-graph_WQWMM.pdf?up=1466611200
/mystery-graph-picture/squirrel-graph_NUTZZ.pdf?up=1548658914
/mystery-graph-picture/graph-washington_WQWMN.pdf?up=1480675458
/mystery-graph-picture/dolphin-mystery-graph_EKDOW.pdf?up=1466611200
/mystery-graph-picture/dolphin-mystery-graph_EKDOW.pdf?up=1466611200
/mystery-graph-picture/graph-robot_WQWMT.pdf?up=1480675493
/mystery-graph-picture/ladybug-hard-graph_SJKSS.pdf?up=1524761075
 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5:
cmd( 1):
processing https://www.downloadtest.com/mystery-graph-picture/apple-easy-graph_APLZE.pdf?up=1547724076
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/mystery-graph-picture/apple-easy-graph_APLZE.pdf?up=1547724076
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/mystery-graph-picture/sailboat-easy-graph_WQWQD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/mystery-graph-picture/sailboat-easy-graph_WQWQD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/mystery-graph-picture/buffalo-easy-graph_HYASW.pdf?up=1545296338
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/mystery-graph-picture/castle-intermediate-graph_CLKAS.pdf?up=1548658914
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/hanukkah/dreidel-easy-graph_WQWQQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/mystery-graph-picture/flower-easy-graph_WQWQF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/mystery-graph-picture/heart-easy-graph_SWEET.pdf?up=1545123832
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/mystery-graph-picture/pumpkin-easy-graph_WQWQM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/mystery-graph-picture/pumpkin-easy-graph-2_JACKO.pdf?up=1547725151
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/mystery-graph-picture/lion-easy-graph_WQWQN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/mystery-graph-picture/graph-owl_WQWQR.pdf?up=1480675370
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/mystery-graph-picture/graph-polygons_WQWQT.pdf?up=1480675396
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/mystery-graph-picture/rabbit-easy-graph_TSIKL.pdf?up=1524037333
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/winter/snowman-easy-graph_WQWQW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/mystery-graph-picture/graph-xmastree_WQWMB.pdf?up=1480675441
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/mystery-graph-picture/dino-easy-graph_XMSKA.pdf?up=1524037332
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/mystery-graph-picture/firetruck-easy-graph_HSJKK.pdf?up=1525156746
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/mystery-graph-picture/st.pattys-graph-V2_SREAN.pdf?up=1547725546
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/mystery-graph-picture/st.pattys-graph_WGYAZ.pdf?up=1548667937
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/mystery-graph-picture/graph-lincoln_WQWMD.pdf?up=1480675343
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/mystery-graph-picture/graph-santa_WQWMF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/mystery-graph-picture/bus-mystery-graph_WQWMQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/mystery-graph-picture/school-mystery-graph_WQWMM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/mystery-graph-picture/squirrel-graph_NUTZZ.pdf?up=1548658914
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/mystery-graph-picture/graph-washington_WQWMN.pdf?up=1480675458
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/mystery-graph-picture/dolphin-mystery-graph_EKDOW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/mystery-graph-picture/dolphin-mystery-graph_EKDOW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/mystery-graph-picture/graph-robot_WQWMT.pdf?up=1480675493
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/mystery-graph-picture/ladybug-hard-graph_SJKSS.pdf?up=1524761075
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/number-detective.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/number-detective.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/number-detective.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):
/number-detective/number-detective-b1_WQZDR.pdf?up=1466611200
/number-detective/number-detective-b1_WQZDR.pdf?up=1466611200
/number-detective/number-detective-b2_WQZDT.pdf?up=1466611200
/number-detective/number-detective-b2_WQZDT.pdf?up=1466611200
/number-detective/number-detective-b3_WQZDW.pdf?up=1466611200
/number-detective/number-detective-b4_WQZDZ.pdf?up=1466611200
/number-detective/number-detective-b5_WQZFB.pdf?up=1466611200
/number-detective/number-detective-b6_WQZFD.pdf?up=1466611200
/number-detective/number-detective-b7_WQZFF.pdf?up=1466611200
/number-detective/number-detective-c1_WQZFM.pdf?up=1466611200
/number-detective/number-detective-c2_WQZFN.pdf?up=1466611200
/number-detective/number-detective-c3_WQZFR.pdf?up=1466611200
/number-detective/number-detective-c4_WQZFT.pdf?up=1466611200
/number-detective/number-detective-c5_WQZFW.pdf?up=1466611200
/number-detective/number-detective-c6_WQZFZ.pdf?up=1466611200
/number-detective/number-detective-c7_WQZQB.pdf?up=1466611200
/number-detective/number-detective-c8_WQZQD.pdf?up=1466611200
/number-detective/number-detective-c9_WQZQF.pdf?up=1466611200
/number-detective/number-detective-c10_WQZQQ.pdf?up=1466611200
/number-detective/number-detective-d1_WQZQN.pdf?up=1466611200
/number-detective/number-detective-d1_WQZQN.pdf?up=1466611200
/number-detective/number-detective-d2_WQZQR.pdf?up=1466611200
/number-detective/number-detective-d3_WQZQT.pdf?up=1466611200
/number-detective/number-detective-d4_WQZQW.pdf?up=1466611200
/number-detective/number-detective-d5_WQZQZ.pdf?up=1466611200
/number-detective/number-detective-d6_WQZMB.pdf?up=1466611200
/number-detective/number-detective-d7_SEVEN.pdf?up=1466611200
/number-detective/number-detective-d8_NUMBD.pdf?up=1466611200
/number-detective/number-detective-d9_NDETE.pdf?up=1466611200
/number-detective/number-detective-d10_NDTEN.pdf?up=1466611200
/number-detective/number-detective-blank_WQZMF.pdf?up=1466611200
 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5:
cmd( 1):
processing https://www.downloadtest.com/number-detective/number-detective-b1_WQZDR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/number-detective/number-detective-b1_WQZDR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/number-detective/number-detective-b2_WQZDT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/number-detective/number-detective-b2_WQZDT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/number-detective/number-detective-b3_WQZDW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/number-detective/number-detective-b4_WQZDZ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/number-detective/number-detective-b5_WQZFB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/number-detective/number-detective-b6_WQZFD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/number-detective/number-detective-b7_WQZFF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/number-detective/number-detective-c1_WQZFM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/number-detective/number-detective-c2_WQZFN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/number-detective/number-detective-c3_WQZFR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/number-detective/number-detective-c4_WQZFT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/number-detective/number-detective-c5_WQZFW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/number-detective/number-detective-c6_WQZFZ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/number-detective/number-detective-c7_WQZQB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/number-detective/number-detective-c8_WQZQD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/number-detective/number-detective-c9_WQZQF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/number-detective/number-detective-c10_WQZQQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/number-detective/number-detective-d1_WQZQN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/number-detective/number-detective-d1_WQZQN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/number-detective/number-detective-d2_WQZQR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/number-detective/number-detective-d3_WQZQT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/number-detective/number-detective-d4_WQZQW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/number-detective/number-detective-d5_WQZQZ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/number-detective/number-detective-d6_WQZMB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/number-detective/number-detective-d7_SEVEN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/number-detective/number-detective-d8_NUMBD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/number-detective/number-detective-d9_NDETE.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/number-detective/number-detective-d10_NDTEN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/number-detective/number-detective-blank_WQZMF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/states-lost.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/states-lost.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/states-lost.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):
/states-lost/states-lost-1_WNDFN.pdf?up=1466611200
/states-lost/states-lost-1_WNDFN.pdf?up=1466611200
/states-lost/states-lost-2_WNDFR.pdf?up=1466611200
/states-lost/states-lost-2_WNDFR.pdf?up=1466611200
/states-lost/states-lost-3_WNDFT.pdf?up=1466611200
/states-lost/states-lost-4_WNDFW.pdf?up=1466611200
/states-lost/states-lost-5_WNDFZ.pdf?up=1466611200
/states-lost/states-lost-6_WNDQB.pdf?up=1466611200
/states-lost/states-lost-7_WNDQD.pdf?up=1466611200
/states-lost/states-lost-8_WNDQF.pdf?up=1466611200
/states-lost/states-lost-9_WNDQQ.pdf?up=1466611200
/states-lost/states-lost-10_WNDQM.pdf?up=1466611200
/states-lost/states-lost-11_WNDQN.pdf?up=1466611200
/states-lost/states-lost-12_WNDQR.pdf?up=1466611200
/states-lost/states-lost-13_WNDQT.pdf?up=1466611200
/states-lost/states-lost-14_WNDQW.pdf?up=1466611200
/states-lost/states-lost-15_WNDQZ.pdf?up=1466611200
/states-lost/states-lost-16_WNDMB.pdf?up=1466611200
/states-lost/states-lost-17_WNDMD.pdf?up=1466611200
/states-lost/states-lost-18_WNDMF.pdf?up=1466611200
/states-lost/states-lost-19_WNDMQ.pdf?up=1466611200
/states-lost/states-lost-20_WNDMM.pdf?up=1466611200
/states-lost/states-lost-21_WNDMN.pdf?up=1466611200
/states-lost/states-lost-22_WNDMR.pdf?up=1466611200
/states-lost/states-lost-23_WNDMT.pdf?up=1466611200
/states-lost/states-lost-24_WNDMW.pdf?up=1466611200
/states-lost/states-lost-25_WNDMZ.pdf?up=1466611200
/states-lost/states-lost-26_WNDNB.pdf?up=1466611200
/states-lost/states-lost-27_WNDND.pdf?up=1466611200
/states-lost/states-lost-28_WNDNF.pdf?up=1466611200
/states-lost/states-lost-29_WNDNN.pdf?up=1466611200
/states-lost/states-lost-29_WNDNN.pdf?up=1466611200
/states-lost/states-lost-30_WNDNR.pdf?up=1466611200
/states-lost/states-lost-31_WNDNT.pdf?up=1466611200
/states-lost/states-lost-32_WNDNW.pdf?up=1466611200
/states-lost/states-lost-33_WNDNZ.pdf?up=1466611200
/states-lost/states-lost-34_WNDRB.pdf?up=1466611200
/states-lost/states-lost-35_WNDRD.pdf?up=1466611200
/states-lost/states-lost-36_WNDRF.pdf?up=1466611200
/states-lost/states-lost-37_WNDRQ.pdf?up=1466611200
/states-lost/states-lost-38_WNDRM.pdf?up=1466611200
/states-lost/states-lost-39_WNDRN.pdf?up=1466611200
/states-lost/states-lost-40_WNDRR.pdf?up=1466611200
/states-lost/states-lost-41_WNDRT.pdf?up=1572692267
/states-lost/states-lost-42_WNDRW.pdf?up=1466611200
/states-lost/states-lost-43_WNDRZ.pdf?up=1466611200
/states-lost/states-lost-44_WNDTB.pdf?up=1466611200
/states-lost/states-lost-45_WNDTD.pdf?up=1466611200
/states-lost/states-lost-46_WNDTF.pdf?up=1466611200
/states-lost/states-lost-47_WNDTQ.pdf?up=1466611200
/states-lost/states-lost-48_WNDTM.pdf?up=1466611200
/states-lost/states-lost-49_WNDTN.pdf?up=1466611200
/states-lost/states-lost-50_WNDTR.pdf?up=1466611200
 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5:
cmd( 1):
processing https://www.downloadtest.com/states-lost/states-lost-1_WNDFN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/states-lost/states-lost-1_WNDFN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/states-lost/states-lost-2_WNDFR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/states-lost/states-lost-2_WNDFR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/states-lost/states-lost-3_WNDFT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/states-lost/states-lost-4_WNDFW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/states-lost/states-lost-5_WNDFZ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/states-lost/states-lost-6_WNDQB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/states-lost/states-lost-7_WNDQD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/states-lost/states-lost-8_WNDQF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/states-lost/states-lost-9_WNDQQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/states-lost/states-lost-10_WNDQM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/states-lost/states-lost-11_WNDQN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/states-lost/states-lost-12_WNDQR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/states-lost/states-lost-13_WNDQT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/states-lost/states-lost-14_WNDQW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/states-lost/states-lost-15_WNDQZ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/states-lost/states-lost-16_WNDMB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/states-lost/states-lost-17_WNDMD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/states-lost/states-lost-18_WNDMF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/states-lost/states-lost-19_WNDMQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/states-lost/states-lost-20_WNDMM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/states-lost/states-lost-21_WNDMN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/states-lost/states-lost-22_WNDMR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/states-lost/states-lost-23_WNDMT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/states-lost/states-lost-24_WNDMW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/states-lost/states-lost-25_WNDMZ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/states-lost/states-lost-26_WNDNB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/states-lost/states-lost-27_WNDND.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/states-lost/states-lost-28_WNDNF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/states-lost/states-lost-29_WNDNN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/states-lost/states-lost-29_WNDNN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/states-lost/states-lost-30_WNDNR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/states-lost/states-lost-31_WNDNT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/states-lost/states-lost-32_WNDNW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/states-lost/states-lost-33_WNDNZ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/states-lost/states-lost-34_WNDRB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/states-lost/states-lost-35_WNDRD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/states-lost/states-lost-36_WNDRF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/states-lost/states-lost-37_WNDRQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/states-lost/states-lost-38_WNDRM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/states-lost/states-lost-39_WNDRN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/states-lost/states-lost-40_WNDRR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/states-lost/states-lost-41_WNDRT.pdf?up=1572692267
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/states-lost/states-lost-42_WNDRW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/states-lost/states-lost-43_WNDRZ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/states-lost/states-lost-44_WNDTB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/states-lost/states-lost-45_WNDTD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/states-lost/states-lost-46_WNDTF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/states-lost/states-lost-47_WNDTQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/states-lost/states-lost-48_WNDTM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/states-lost/states-lost-49_WNDTN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/states-lost/states-lost-50_WNDTR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/full-puzzles.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/full-puzzles.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/full-puzzles.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):

 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
---Step5:
cmd( 1):
processing https://www.downloadtest.com
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/teachingtools.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/teachingtools.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/teachingtools.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):
/teachingtools/subjectlessonplanner_WNQZD.pdf?up=1466611200
/teachingtools/subjectlessonplanner_WNQZD.pdf?up=1466611200
/teachingtools/stgradebook_WNQZF.pdf?up=1466611200
/teachingtools/stgradebook_WNQZF.pdf?up=1466611200
/teachingtools/homework-note_WNQZR.pdf?up=1492062473
/teachingtools/school-supply-note_WNQZT.pdf?up=1466611200
/teachingtools/addressbook_WNQZM.pdf?up=1466611200
/teachingtools/parent-communication_WNQZN.pdf?up=1466611200
/teachingtools/showandtell-note_WNQZW.pdf?up=1466611200
/teachingtools/permission-slip_FIELD.pdf?up=1466611200
/weather/weather-chart_WNMBB.pdf?up=1466611200
/weather/weather-chart-color_WNMBD.pdf?up=1466611200
/timeline/birthday-timeline_CARDS.pdf?up=1497255544
/teachingtools/birthday-calendar-bw_WNMBQ.pdf?up=1497256544
/teachingtools/birthday-calendar-color_WNMBM.pdf?up=1497256784
/teachingtools/birthday-calendar-balloons-bw_WNMBN.pdf?up=1497256231
/teachingtools/birthday-calendar-balloons-color_WNMBR.pdf?up=1497256243
/teachingtools/absent01_WNMBW.pdf?up=1497254147
/teachingtools/absent02_WNMBZ.pdf?up=1497254615
/teachingtools/absent03_WNMDB.pdf?up=1497254815
/alphabet/alphabet-line-picture_YAUJV.pdf?up=1466611200
/alphabet/alphabet-line-coloring_DTAHY.pdf?up=1466611200
/alphabet/alphabet-line-basic_JAYHU.pdf?up=1466611200
/alphabet/alphabet-line-draw_HATYI.pdf?up=1466611200
/alphabet/alphabet-reference_POAIG.pdf?up=1466611200
/fractions/fraction-ordering-bookmarks_TWMDD.pdf?up=1466611200
/measurement/measurement-bookmarks_TWTWM.pdf?up=1466611200
/generator-newsletter.html title=Classroom Newsletter (Generator)>Classroom Newsletter (Generator)</a><p class=ws_also style=margin-bottom:17px>Create your own custom classroom newsletters. Choose a layout, and your favorite theme. Then enter your own content.</p><a data-type=StaticLink class=content homeLink
/teachingtools/agenda_WNMDM.pdf?up=1497365348
/teachingtools/agenda-horizontal_WNMDN.pdf?up=1497365346
/teachingtools/agenda-double_WNMDR.pdf?up=1497365344
/teachingtools/agenda-double2_WNMDT.pdf?up=1497365343
/teachingtools/agenda-border_WNMDW.pdf?up=1466611200
/teachingtools/desk-top-horizontal01_WNMFB.pdf?up=1497369416
/teachingtools/desk-top-horizontal02_WNMFD.pdf?up=1497369419
/teachingtools/desk-tag-vertical-fish_WNMFF.pdf?up=1497369413
/teachingtools/desk-tag-vertical-heroes_WNMFQ.pdf?up=1497369414
/teachingtools/desk-tag-vertical-robots_WNMFM.pdf?up=1497369415
/teachingtools/desk-tag-vertical-dinos_WNMFN.pdf?up=1497369412
/time/clocks-tabs-triangles_HSGSO.pdf?up=1497370559
/time/clocks-tabs-triangles-advanced_JSWOS.pdf?up=1497370558
/time/clocks-tabs-round_HDOSO.pdf?up=1497370558
/time/clocks-tabs-round-advanced_GSJIA.pdf?up=1497370557
 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5:
cmd( 1):
processing https://www.downloadtest.com/teachingtools/subjectlessonplanner_WNQZD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/teachingtools/subjectlessonplanner_WNQZD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/teachingtools/stgradebook_WNQZF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/teachingtools/stgradebook_WNQZF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/teachingtools/homework-note_WNQZR.pdf?up=1492062473
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/teachingtools/school-supply-note_WNQZT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/teachingtools/addressbook_WNQZM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/teachingtools/parent-communication_WNQZN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/teachingtools/showandtell-note_WNQZW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/teachingtools/permission-slip_FIELD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/weather/weather-chart_WNMBB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/weather/weather-chart-color_WNMBD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/timeline/birthday-timeline_CARDS.pdf?up=1497255544
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/teachingtools/birthday-calendar-bw_WNMBQ.pdf?up=1497256544
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/teachingtools/birthday-calendar-color_WNMBM.pdf?up=1497256784
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/teachingtools/birthday-calendar-balloons-bw_WNMBN.pdf?up=1497256231
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/teachingtools/birthday-calendar-balloons-color_WNMBR.pdf?up=1497256243
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/teachingtools/absent01_WNMBW.pdf?up=1497254147
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/teachingtools/absent02_WNMBZ.pdf?up=1497254615
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/teachingtools/absent03_WNMDB.pdf?up=1497254815
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/alphabet/alphabet-line-picture_YAUJV.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/alphabet/alphabet-line-coloring_DTAHY.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/alphabet/alphabet-line-basic_JAYHU.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/alphabet/alphabet-line-draw_HATYI.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/alphabet/alphabet-reference_POAIG.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/fractions/fraction-ordering-bookmarks_TWMDD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/measurement/measurement-bookmarks_TWTWM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/generator-newsletter.html title=Classroom Newsletter (Generator)>Classroom Newsletter (Generator)</a><p class=ws_also style=margin-bottom:17px>Create your own custom classroom newsletters. Choose a layout, and your favorite theme. Then enter your own content.</p><a data-type=StaticLink class=content homeLink
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/teachingtools/agenda_WNMDM.pdf?up=1497365348
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/teachingtools/agenda-horizontal_WNMDN.pdf?up=1497365346
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/teachingtools/agenda-double_WNMDR.pdf?up=1497365344
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/teachingtools/agenda-double2_WNMDT.pdf?up=1497365343
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/teachingtools/agenda-border_WNMDW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/teachingtools/desk-top-horizontal01_WNMFB.pdf?up=1497369416
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/teachingtools/desk-top-horizontal02_WNMFD.pdf?up=1497369419
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/teachingtools/desk-tag-vertical-fish_WNMFF.pdf?up=1497369413
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/teachingtools/desk-tag-vertical-heroes_WNMFQ.pdf?up=1497369414
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/teachingtools/desk-tag-vertical-robots_WNMFM.pdf?up=1497369415
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/teachingtools/desk-tag-vertical-dinos_WNMFN.pdf?up=1497369412
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/time/clocks-tabs-triangles_HSGSO.pdf?up=1497370559
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/time/clocks-tabs-triangles-advanced_JSWOS.pdf?up=1497370558
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/time/clocks-tabs-round_HDOSO.pdf?up=1497370558
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/time/clocks-tabs-round-advanced_GSJIA.pdf?up=1497370557
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/awards.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/awards.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/awards.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):
/awards/writing-author-award-girl-j_WNDWT.pdf?up=1559579751
/awards/writing-author-award-girl-j_WNDWT.pdf?up=1559579751
/awards/reading-progress-girl-j_WNDWW.pdf?up=1557828025
/awards/reading-progress-girl-j_WNDWW.pdf?up=1557828025
/awards/reading-progress-boy-j_WNDWW.pdf?up=1557828022
/awards/reading-remarkable-j_WNDWX.pdf?up=1557817511
/awards/reading-recognition-award-j_AWRRC.pdf?up=1557827376
/awards/reading-bookworm-j_WNDZB.pdf?up=1557819414
/awards/Reading-10-j_WNDZD.pdf?up=1466611200
/awards/Reading-25-j_WNDZF.pdf?up=1466611200
/awards/Reading-40-j_WNDZQ.pdf?up=1466611200
/awards/Reading-50-j_WNDZM.pdf?up=1466611200
/awards/reading-100-j_WDNMZ.pdf?up=1466611200
/awards/phonics-award-j_AWPAC.pdf?up=1557826129
/awards/cursive-award_PENMA.pdf?up=1466611200
/awards/math-award-j_WNDZR.pdf?up=1561049419
/awards/social-studies-j_WNDZT.pdf?up=1557819701
/awards/spelling-award-girl-j_WNDZW.pdf?up=1557939474
/awards/spelling-award-boy-j_WNDZW.pdf?up=1557939477
/awards/science-award-j_WNDZZ.pdf?up=1557819088
/awards/artist-j_WNFBB.pdf?up=1557383124
/awards/music1-award-girl-j_WNFBF.pdf?up=1557898048
/awards/music1-award-boy-j_WNFBF.pdf?up=1557898052
/awards/music2-award-j_WNFBQ.pdf?up=1557818014
/awards/music3-award-j_WNFBM.pdf?up=1466611200
/awards/music4-award-j_WNFBN.pdf?up=1557818204
/awards/music5-award-j_WNFBR.pdf?up=1557818906
/awards/computer-award-j_WNFBW.pdf?up=1557818708
/awards/typing-award-j_WNFBZ.pdf?up=1466611200
/awards/birthday1-j_WNFDD.pdf?up=1466611200
/awards/attendance-award-j_WNFDM.pdf?up=1557820105
/awards/courageous-effort_EFFOR.pdf?up=1466611200
/awards/great-progress_PROGR.pdf?up=1466611200
/awards/show-and-tell-certificate-j_WNFDN.pdf?up=1466611200
/awards/homework-award-j_WNFDR.pdf?up=1466611200
/awards/honor-roll-award_STARR.pdf?up=1552727714
/awards/honor-roll-award-a_SUPER.pdf?up=1552727711
/awards/honor-roll-award-ab_SUPAB.pdf?up=1552727708
/awards/honor-roll-award-b_SUPBB.pdf?up=1552727710
/awards/merit-roll-award_MERIT.pdf?up=1552727722
/awards/kindness-award_LHFAD.pdf?up=1466611200
/awards/kindness-award_LHFAD.pdf?up=1466611200
/awards/responsibility-award-j.pdf?up=1466611200
/awards/cooperative-award-j.pdf?up=1466611200
/awards/behavior-award-j.pdf?up=1466611200
/awards/helper-award-j.pdf?up=1466611200
/awards/people-mother.pdf?up=1466611200
/awards/people-father.pdf?up=1466611200
/awards/people-grandmother.pdf?up=1466611200
/awards/people-grandfather.pdf?up=1466611200
/awards/people-brother.pdf?up=1466611200
/awards/people-sister.pdf?up=1466611200
/awards/people-teacher.pdf?up=1466611200
/awards/people-friend.pdf?up=1466611200
/awards/people-funniest.pdf?up=1466611200
/awards/people-kindest.pdf?up=1466611200
/awards/first-place-writing-trophy_SAQKU.pdf?up=1533622564
/awards/first-place-spelling-bee-trophy_SPELL.pdf?up=1533622444
/awards/first-place-reading-trophy_READQ.pdf?up=1533622673
/awards/first-place-music-trophy_KIZZZ.pdf?up=1533622554
/awards/first-place-math-trophy-silver_MATHH.pdf?up=1533622539
/awards/best-dad-trophy_DWYAS.pdf?up=1533622587
/awards/best-mom-trophy_MOOKA.pdf?up=1533622609
/awards/best-grandpa-trophy_EKSAN.pdf?up=1533622502
/awards/best-grandma-trophy_TPCBE.pdf?up=1533622483
 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5:
cmd( 1):
processing https://www.downloadtest.com/awards/writing-author-award-girl-j_WNDWT.pdf?up=1559579751
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/awards/writing-author-award-girl-j_WNDWT.pdf?up=1559579751
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/awards/reading-progress-girl-j_WNDWW.pdf?up=1557828025
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/awards/reading-progress-girl-j_WNDWW.pdf?up=1557828025
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/awards/reading-progress-boy-j_WNDWW.pdf?up=1557828022
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/awards/reading-remarkable-j_WNDWX.pdf?up=1557817511
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/awards/reading-recognition-award-j_AWRRC.pdf?up=1557827376
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/awards/reading-bookworm-j_WNDZB.pdf?up=1557819414
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/awards/Reading-10-j_WNDZD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/awards/Reading-25-j_WNDZF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/awards/Reading-40-j_WNDZQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/awards/Reading-50-j_WNDZM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/awards/reading-100-j_WDNMZ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/awards/phonics-award-j_AWPAC.pdf?up=1557826129
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/awards/cursive-award_PENMA.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/awards/math-award-j_WNDZR.pdf?up=1561049419
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/awards/social-studies-j_WNDZT.pdf?up=1557819701
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/awards/spelling-award-girl-j_WNDZW.pdf?up=1557939474
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/awards/spelling-award-boy-j_WNDZW.pdf?up=1557939477
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/awards/science-award-j_WNDZZ.pdf?up=1557819088
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/awards/artist-j_WNFBB.pdf?up=1557383124
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/awards/music1-award-girl-j_WNFBF.pdf?up=1557898048
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/awards/music1-award-boy-j_WNFBF.pdf?up=1557898052
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/awards/music2-award-j_WNFBQ.pdf?up=1557818014
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/awards/music3-award-j_WNFBM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/awards/music4-award-j_WNFBN.pdf?up=1557818204
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/awards/music5-award-j_WNFBR.pdf?up=1557818906
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/awards/computer-award-j_WNFBW.pdf?up=1557818708
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/awards/typing-award-j_WNFBZ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/awards/birthday1-j_WNFDD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/awards/attendance-award-j_WNFDM.pdf?up=1557820105
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/awards/courageous-effort_EFFOR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/awards/great-progress_PROGR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/awards/show-and-tell-certificate-j_WNFDN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/awards/homework-award-j_WNFDR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/awards/honor-roll-award_STARR.pdf?up=1552727714
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/awards/honor-roll-award-a_SUPER.pdf?up=1552727711
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/awards/honor-roll-award-ab_SUPAB.pdf?up=1552727708
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/awards/honor-roll-award-b_SUPBB.pdf?up=1552727710
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/awards/merit-roll-award_MERIT.pdf?up=1552727722
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/awards/kindness-award_LHFAD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/awards/kindness-award_LHFAD.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/awards/responsibility-award-j.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/awards/cooperative-award-j.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/awards/behavior-award-j.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/awards/helper-award-j.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/awards/people-mother.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/awards/people-father.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/awards/people-grandmother.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/awards/people-grandfather.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/awards/people-brother.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/awards/people-sister.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/awards/people-teacher.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/awards/people-friend.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/awards/people-funniest.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/awards/people-kindest.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/awards/first-place-writing-trophy_SAQKU.pdf?up=1533622564
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/awards/first-place-spelling-bee-trophy_SPELL.pdf?up=1533622444
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/awards/first-place-reading-trophy_READQ.pdf?up=1533622673
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/awards/first-place-music-trophy_KIZZZ.pdf?up=1533622554
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/awards/first-place-math-trophy-silver_MATHH.pdf?up=1533622539
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/awards/best-dad-trophy_DWYAS.pdf?up=1533622587
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/awards/best-mom-trophy_MOOKA.pdf?up=1533622609
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/awards/best-grandpa-trophy_EKSAN.pdf?up=1533622502
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/awards/best-grandma-trophy_TPCBE.pdf?up=1533622483
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/full-teacher.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/full-teacher.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/full-teacher.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):

 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
---Step5:
cmd( 1):
processing https://www.downloadtest.com
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/full-letters-alphabet.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/full-letters-alphabet.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/full-letters-alphabet.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):

 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
---Step5:
cmd( 1):
processing https://www.downloadtest.com
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/counting.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/counting.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/counting.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):

 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
---Step5:
cmd( 1):
processing https://www.downloadtest.com
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/shapes-basic.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/shapes-basic.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/shapes-basic.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):
/geometry/circles-1_WRBTN.pdf?up=1499925700
/geometry/circles-1_WRBTN.pdf?up=1499925700
/geometry/circles-color-and-trace_WRBTM.pdf?up=1499925490
/geometry/circles-color-and-trace_WRBTM.pdf?up=1499925490
/geometry/hexagons-1_WRBWN.pdf?up=1500267925
/geometry/hexagons-color-and-trace_WRBWM.pdf?up=1500269141
/geometry/octagons-1_WRBWT.pdf?up=1500270919
/geometry/octagons-color-and-trace_WRBWR.pdf?up=1500272133
/geometry/ovals-color-and-trace_WRBWW.pdf?up=1500276199
/geometry/rectangles-1_WRBWD.pdf?up=1500278079
/geometry/rectangles-color-and-trace_WRBWB.pdf?up=1466611200
/geometry/rhombuses-1_HWOZD.pdf?up=1500389909
/geometry/rhombuses-color-and-trace_GWKSI.pdf?up=1500392344
/geometry/squares-1_WRBTZ.pdf?up=1500393335
/geometry/squares-color-and-trace_WRBTW.pdf?up=1500394754
/geometry/stars-1_EUOXL.pdf?up=1500395170
/geometry/stars-color-and-trace_UKKAS.pdf?up=1500395571
/geometry/trapezoids-1_WRBWQ.pdf?up=1500396078
/geometry/trapezoids-color-and-trace_WRBWF.pdf?up=1500396809
/geometry/triangles-1_WRBTT.pdf?up=1500355119
/geometry/triangles-color-and-trace_WRBTR.pdf?up=1500355996
/geometry/basic-shapes-1_WRBZD.pdf?up=1500359957
/geometry/basic-shapes-1_WRBZD.pdf?up=1500359957
/geometry/basic-shapes-mini-book_WRBZF.pdf?up=1466611200
/geometry/basic-shapes-2_WRBZQ.pdf?up=1466611200
/geometry/basic-shapes-matching-game_WRBZM.pdf?up=1466611200
/geometry/coloring-shapes2_WRBZN.pdf?up=1466611200
/geometry/coloring-shapes3_WRBZR.pdf?up=1466611200
/geometry/coloring-shapes_WRBZT.pdf?up=1466611200
/geometry/counting-shapes_WRBZW.pdf?up=1466611200
 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
 WARN: [cmd] - [Not implemented or void for no action!]
---Step5:
cmd( 1):
processing https://www.downloadtest.com/geometry/circles-1_WRBTN.pdf?up=1499925700
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/geometry/circles-1_WRBTN.pdf?up=1499925700
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/geometry/circles-color-and-trace_WRBTM.pdf?up=1499925490
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/geometry/circles-color-and-trace_WRBTM.pdf?up=1499925490
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/geometry/hexagons-1_WRBWN.pdf?up=1500267925
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/geometry/hexagons-color-and-trace_WRBWM.pdf?up=1500269141
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/geometry/octagons-1_WRBWT.pdf?up=1500270919
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/geometry/octagons-color-and-trace_WRBWR.pdf?up=1500272133
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/geometry/ovals-color-and-trace_WRBWW.pdf?up=1500276199
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/geometry/rectangles-1_WRBWD.pdf?up=1500278079
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/geometry/rectangles-color-and-trace_WRBWB.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/geometry/rhombuses-1_HWOZD.pdf?up=1500389909
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/geometry/rhombuses-color-and-trace_GWKSI.pdf?up=1500392344
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/geometry/squares-1_WRBTZ.pdf?up=1500393335
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/geometry/squares-color-and-trace_WRBTW.pdf?up=1500394754
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/geometry/stars-1_EUOXL.pdf?up=1500395170
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/geometry/stars-color-and-trace_UKKAS.pdf?up=1500395571
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/geometry/trapezoids-1_WRBWQ.pdf?up=1500396078
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/geometry/trapezoids-color-and-trace_WRBWF.pdf?up=1500396809
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/geometry/triangles-1_WRBTT.pdf?up=1500355119
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/geometry/triangles-color-and-trace_WRBTR.pdf?up=1500355996
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/geometry/basic-shapes-1_WRBZD.pdf?up=1500359957
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/geometry/basic-shapes-1_WRBZD.pdf?up=1500359957
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/geometry/basic-shapes-mini-book_WRBZF.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/geometry/basic-shapes-2_WRBZQ.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/geometry/basic-shapes-matching-game_WRBZM.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/geometry/coloring-shapes2_WRBZN.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/geometry/coloring-shapes3_WRBZR.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/geometry/coloring-shapes_WRBZT.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
cmd( 1):
processing https://www.downloadtest.com/geometry/counting-shapes_WRBZW.pdf?up=1466611200
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/full-kindergarten.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/full-kindergarten.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/full-kindergarten.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):

 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
---Step5:
cmd( 1):
processing https://www.downloadtest.com
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/generator-word-search.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/generator-word-search.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/generator-word-search.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):

 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
---Step5:
cmd( 1):
processing https://www.downloadtest.com
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/generator-multiple-choice.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/generator-multiple-choice.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/generator-multiple-choice.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):

 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
---Step5:
cmd( 1):
processing https://www.downloadtest.com
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/generator-fill-in-the-blank.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/generator-fill-in-the-blank.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/generator-fill-in-the-blank.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):

 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
---Step5:
cmd( 1):
processing https://www.downloadtest.com
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/full-generators-index.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/full-generators-index.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/full-generators-index.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):

 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
---Step5:
cmd( 1):
processing https://www.downloadtest.com
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/full-index.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/full-index.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/full-index.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):

 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
---Step5:
cmd( 1):
processing https://www.downloadtest.com
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/help.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/help.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/help.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):

 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
---Step5:
cmd( 1):
processing https://www.downloadtest.com
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/terms-of-use.html 
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/terms-of-use.html 
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/terms-of-use.html  |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):

 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
---Step5:
cmd( 1):
processing https://www.downloadtest.com
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:
==Task3: [task/probe_page ==> locate_downloadpage:  ]
---Step1:
~~~SubStep1: [print:  ]
/contact-us.html
~~~SubStep2: [print:  ]
locate pdf from: https://www.downloadtest.com/contact-us.html
~~~SubStep3: [print:  ]
curl -s https://www.downloadtest.com/contact-us.html |grep ".pdf?up=" |awk '{split($0,a,"href="); print a[2]}'|awk '{split($0,a,"alt="); print a[1]}' |tr -d \" |grep -v "# "

---Step2:
cmd( 1):

 .. ok
. ok
---Step3:
~~~SubStep1: [print:  ]
return code: 0
~~~SubStep2: [reg:  ]
---Step4:
---Step5:
cmd( 1):
processing https://www.downloadtest.com
 .. ok
cmd( 2):

 .. ok
. ok
---Step6:
---Step7:

```

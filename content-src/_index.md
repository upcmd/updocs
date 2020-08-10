---
title: "up cmd"
date: 2020-06-25T22:32:46+11:00
draft: false
---

# UPcmd [github source](https://github.com/upcmd/up)

## User Manual

### Chapter 1 [installation and usage](/usage/)

  1. [command line basics](/usage/cli_usage/)
  2. [list tasks](/usage/list_tasks/)
  3. [config yml](/usage/config_yml/)
  4. [command args](/usage/cli_args/)
  5. [public and protected tasks](/usage/c0140/)
  6. [working directory](/usage/c0144/)

### Chapter 2 [quick start](/quick-start/)

  1. [First task - hello world](/quick-start/c0001/)
  2. [Multiple Steps](/quick-start/c0002/)
  3. [Yaml literal](/quick-start/c0003/)
  4. [Use array](/quick-start/c0004/)
  5. [manage dependencies](/quick-start/c0005/)
  6. [call func (power up)](/quick-start/c0104/)
  7. [use env var](/quick-start/c0006/)
  8. [syntax variation](/quick-start/c0007/)
  9. [a little taste of UPcmd](/quick-start/c0151/)

### Chapter 3 [vars](/vars/)

  1. [Use variables](/vars/c0012/)
  2. [vars in func](/vars/c0013/)
  3. [variables in callee](/vars/c0014/)
  4. [use vars](/vars/c0019/)
  5. [use golang template](/vars/c0021/)
  6. [var rendering](/vars/c0022/)
  7. [local vars](/vars/c0035/)
  8. [local vs reg global](/vars/c0076/)
  9. [var scope and accessibility](/vars/c0105/)
  10. [var scope in callee](/vars/c0106/)
  11. [task scope](/vars/c0108/)
  12. [taskScope vars in callee](/vars/c0109/)
  13. [local vars from file](/vars/c0146/)
  14. [use pure local vars](/vars/c0147/)
  15. [taskScope vars in block](/vars/c0148/)
  16. [probe exisitence of path](/vars/c0154/)

### Chapter 4 [call func](/call-func/)

  1. [assemble worlflow](/call-func/c0017/)
  2. [call func as interface](/call-func/c0020/)
  3. [overriden in callee](/call-func/c0111/)
  4. [multiple layers overriding](/call-func/c0112/)
  5. [return values](/call-func/c0113/)
  6. [sequence matters in return 1](/call-func/c0114/)
  7. [sequence matters in return 2](/call-func/c0115/)
  8. [chained calls and return values](/call-func/c0150/)

### Chapter 5 [scope](/scope/)

  1. [vars intro](/scope/c0008/)
  2. [externalize settings](/scope/c0009/)
  3. [leave merge](/scope/c0010/)
  4. [runtime merge](/scope/c0011/)

### Chapter 6 [dvars](/dvars/)

  1. [dvars intro](/dvars/c0023/)
  2. [complext object](/dvars/c0024/)
  3. [string literal](/dvars/c0025/)
  4. [convert dvar](/dvars/c0027/)
  5. [externalize settings](/dvars/c0028/)
  6. [dvars in scopes](/dvars/c0029/)
  7. [complex case](/dvars/c0030/)
  8. [leave level merge](/dvars/c0031/)
  9. [instance level merge](/dvars/c0032/)
  10. [local scope merge](/dvars/c0033/)
  11. [dvars in call](/dvars/c0034/)
  12. [dynamics on dynamics](/dvars/c0050/)
  13. [load file using dvar](/dvars/c0070/)
  14. [dvar to global](/dvars/c0078/)
  15. [datakey as source](/dvars/c0082/)
  16. [void for action](/dvars/c0086/)
  17. [load from refdir](/dvars/c0088/)
  18. [datapath](/dvars/c0098/)
  19. [datatemplate](/dvars/c0099/)

### Chapter 7 [golang template](/template/)

  1. [builtin funcs](/template/c0036/)
  2. [sprig funcs](/template/c0037/)
  3. [gtf funcs](/template/c0038/)
  4. [advanced usage](/template/c0039/)
  5. [use loop](/template/c0040/)
  6. [range and outer value](/template/c0075/)
  7. [UPcmd internal template funcs](/template/upcmd_template_funcs/)
  8. [all template funcs](/template/list_all_template_funcs/)
  9. [splitLines func](/template/c0085/)
  10. [type compare](/template/c0162/)
  11. [yml obj json conversion](/template/c0165/)

### Chapter 8 [shell func](/shell-func/)

  1. [register result](/shell-func/c0026/)
  2. [check result](/shell-func/c0041/)
  3. [ignore error](/shell-func/c0052/)

### Chapter 9 [design patterns](/design-patterns/)

  1. [workflow skeleton](/design-patterns/c0043/)
  2. [basic cli skeleton](/design-patterns/c0047/)
  3. [data structure](/design-patterns/c0079/)
  4. [modular tasks](/design-patterns/c0080/)
  5. [externalise task def](/design-patterns/c0081/)
  6. [func and vars](/design-patterns/func_and_vars/)
  7. [design patterns](/design-patterns/design_patterns/)
  8. [private var scope](/design-patterns/c0094/)

### Chapter 10 [environment vars](/env-vars/)

  1. [basics](/env-vars/c0044/)
  2. [client validation](/env-vars/f0045/)
  3. [environment vars](/env-vars/c0046/)
  4. [declare env var](/env-vars/c0048/)
  5. [env vars in scopes](/env-vars/c0049/)

### Chapter 11 [security](/security/)

  1. [builtin en/decryption](/security/c0051/)

### Chapter 12 [flow controll](/flow-controll/)

  1. [if condition](/flow-controll/c0054/)
  2. [if condition advanced](/flow-controll/c0055/)
  3. [use pause](/flow-controll/c0063/)
  4. [Exit result per step](/flow-controll/c0068/)
  5. [dynamic routing](/flow-controll/c0092/)
  6. [dynamic routing extra](/flow-controll/c0093/)
  7. [loopitem in callee&#39;s dvar](/flow-controll/c0110/)
  8. [use break in call](/flow-controll/c0121/)
  9. [non-exist value if](/flow-controll/c0122/)
  10. [indirect reference](/flow-controll/c0123/)
  11. [else in hard way](/flow-controll/c0126/)
  12. [else branch](/flow-controll/c0127/)
  13. [else with a flow](/flow-controll/c0131/)
  14. [None value](/flow-controll/c0157/)
  15. [final cleanup when shell exceution fails](/flow-controll/f0171/)
  16. [final cleanup without resuce](/flow-controll/f0172/)
  17. [guaranteed final step to ensure clean up/rescue](/flow-controll/c0172/)
  18. [finally steps block](/flow-controll/c0173/)
  19. [conditionally error handling](/flow-controll/c0174/)
  20. [finally/resuce support in task level](/flow-controll/c0175/)

### Chapter 13 [loop](/loop/)

  1. [loopitem in dvar](/loop/c0090/)
  2. [multi-layers loop 1](/loop/c0091/)
  3. [multi-layers loop 2](/loop/c0117/)
  4. [loop with condition](/loop/c0118/)
  5. [condition with until](/loop/c0119/)
  6. [break recursive loop](/loop/f0125/)
  7. [loop usage guide](/loop/c0056/)
  8. [loop with var name](/loop/c0167/)
  9. [loop with range iterator](/loop/c0168/)
  10. [loop and retry example](/loop/c0169/)

### Chapter 14 [config organization](/organization/)

  1. [ref task](/organization/c0059/)
  2. [use case 1](/organization/c0060/)
  3. [use case 2](/organization/c0061/)
  4. [use tasksref](/organization/c0062/)
  5. [load from refdir](/organization/c0089/)
  6. [config file loading](/organization/config_file/)

### Chapter 15 [cmd func](/cmd-func/)

  1. [print message](/cmd-func/c0064/)
  2. [reg and deReg](/cmd-func/c0066/)
  3. [read/write file](/cmd-func/c0071/)
  4. [sleep](/cmd-func/c0087/)
  5. [toObj](/cmd-func/c0095/)
  6. [delete in yml file](/cmd-func/c0102/)
  7. [modify yml content](/cmd-func/c0103/)
  8. [break cmd](/cmd-func/c0120/)
  9. [color print](/cmd-func/c0142/)
  10. [template using data file](/cmd-func/c0145/)

### Chapter 16 [block func](/block-func/)

  1. [use block](/block-func/c0128/)
  2. [embeded block](/block-func/c0129/)
  3. [block in else](/block-func/c0130/)
  4. [call task from block](/block-func/c0134/)
  5. [call block from task](/block-func/c0135/)
  6. [testcase1 for block](/block-func/c0136/)
  7. [test case 1](/block-func/c0159/)
  8. [test case 2](/block-func/c0160/)
  9. [test case 3](/block-func/c0161/)
  10. [complicated test case 4](/block-func/c0163/)
  11. [complicated test case 5](/block-func/c0164/)

### Chapter 17 [templating](/templating/)

  1. [templating using dvar](/templating/c0069/)
  2. [use template cmd](/templating/c0072/)
  3. [datakey and datapath](/templating/c0096/)

### Chapter 18 [test and debug](/test-debug/)

  1. [verbose flag](/test-debug/c0065/)
  2. [pause and inspect](/test-debug/c0101/)
  3. [ues trace](/test-debug/c0107/)
  4. [assert and inspect](/test-debug/c0132/)
  5. [failFast](/test-debug/c0133/)
  6. [deactivated step](/test-debug/c0137/)
  7. [deactivated sub step in cmd](/test-debug/c0138/)
  8. [fail](/test-debug/c0139/)
  9. [error handling](/test-debug/error_handling/)

### Chapter 19 [user interaction](/user-interaction/)

  1. [profile with env vars](/user-interaction/c0153/)
  2. [externise exec profile](/user-interaction/c0158/)
  3. [prompt in taskScope](/user-interaction/prompt_taskscope/)
  4. [user prompt](/user-interaction/prompt_basic/)
  5. [chained pipein from stdin](/user-interaction/pipe_in/)
  6. [profile with env vars logs](/user-interaction/exec_profile/)
  7. [exec profile example](/user-interaction/exec_profile_eg1/)

### Chapter 20 [object oriented](/object-oriented/)

  1. [reg/deReg/void](/object-oriented/c0042/)
  2. [parse yml to object](/object-oriented/c0074/)
  3. [query](/object-oriented/c0100/)
  4. [map structure result](/object-oriented/c0141/)
  5. [reg/set a object](/object-oriented/c0156/)

### Chapter 21 [syntax](/syntax/)

  1. [multiline styles](/syntax/c0073/)

### Chapter 22 [advanced use cases](/advanced-cases/)

  1. [web scraping example](/advanced-cases/web-scraping/)
  2. [upcmd doco auto generation](/advanced-cases/upcmd-doc-gen/)

### Chapter 23 [modules](/module/)

  1. [module commands](/module/module-commands/)
  2. [module config](/module/module-config/)
  3. [HelloWorld example](/module/0001/)
  4. [simple vars](/module/0002/)
  5. [return value](/module/0003/)
  6. [jailed exec context](/module/0004/)
  7. [Complex vars](/module/0005/)
  8. [Complex call layers](/module/0006/)
  9. [use alias](/module/0007/)
  10. [moule lock](/module/0008/)
  11. [module validation](/module/0009/)
  12. [multi module calls](/module/0010/)
  13. [module multi vesioning](/module/0011/)
  14. [max module restriction](/module/f0012/)
  15. [manage indirect dependencies](/module/0013/)

### Chapter 24 [idea and faq](/ideas_and_faq/)

  1. [mutli-threads/concurrency](/ideas_and_faq/concurrency/)
  2. [watch and monitoring](/ideas_and_faq/watch/)
  3. [more funcs support](/ideas_and_faq/modules/)
  4. [http/rest api support](/ideas_and_faq/http/)
  5. [cli tools work with](/ideas_and_faq/cli_tools/)


---
title: "UPcmd internal template funcs"
date: 2020-06-25T22:32:46+11:00
draft: false
weight: 901
---

### List of internal UPcmd template funcs

You can use CLI cmd to check supported golang template funcs
 
```
Ξ ▶ up assist upcmd_template_func
-assist: upcmd_template_func
=List of upcmd template funcs
                      catLines : (func(string) string)(0x174a230)
                         deReg : (func(string) string)(0x174a8b0)
                            OS : (func() string)(0x174a1f0)
                          ARCH : (func() string)(0x174a210)
                     fromSlash : (func(string) string)(0x174a3f0)
                       toSlash : (func(string) string)(0x174a410)
                      printObj : (func(interface {}) string)(0x174a4a0)
                      ymlToObj : (func(string) interface {})(0x174a650)
                           reg : (func(string, interface {}) string)(0x174a710)
                           now : (func() string)(0x174a430)
                   pathExisted : (func(string) bool)(0x174a9a0)
   validateMandatoryFailIfNone : (func(string, string) string)(0x174af20)
                    splitLines : (func(string) []string)(0x174a310)
                      objToYml : (func(interface {}) string)(0x174a570)
                   fileContent : (func(string) string)(0x174aad0)

```

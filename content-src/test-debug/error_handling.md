---
title: "error handling"
date: 2020-06-25T22:32:46+11:00
draft: false
weight: 1710
---

### Exception handling in UP cmd

#### types of exceptions in up cmd

There are three type of errors/exceptions:

1. external integration exception, eg, exception raised in sub step in shell func, it is already handled using Result.Code, Result.Msg

2. internal exception, eg, file loading, templating etc, this should be handled by user, need to find a mechanism

3. panic 

This should be an unhandled bug. The UP cmd program should be reported and fixed to deal with the case.


#### internal exceptions

They can be categorized to:

A. exception not restorable, or should not be restored:

For example, the templating Rendering parsing/syntax issue, as this is a fundamental call for a CMD func step, there could be a few of this type of rendering along the processing chaine to result in final result

B. exception could be optionally handlled by user, user could take control of an alternative approach for better result

1. templating rendering(not a syntax/parsing issue)

2. resource validation: eg file/url/end point does not exist

So question is: should we need to use a rescue element to deal with it? It is debatable. It is better to deal with better pre-check/validation before blindly invoke any cmd/shell func and rely on resue to recover the mess. There are a few benefits doing so:

* It will make the whole workflow clean and precise and less of code of patching and cause headache when peer code reviewing
* It is a good exercise in a modern CI/CD agile process to have the same code tested in CI, gradually tested in staging, then production. It is expected the error scenarios would be identified, failed fast and eliminated gradually and always produce predictable result in production environment. 

#### external exceptions

Currently, the main external exception is the exception from within the call of shell func.

The shell execution result is wrapped into last_result for convenient access. You can access:

```
.last_result.Code 
.last_result.Output 
``` 

You can use ignore_error to ignore the error encountered

##### references

* [Multiple steps](../../shell-func/c0052/)
* [shell func](../../quick-start/c0002/)
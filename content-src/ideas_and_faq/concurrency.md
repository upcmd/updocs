---
title: "mutli-threads/concurrency"
date: 2020-06-25T22:32:46+11:00
draft: false
weight: 2202
---

#### What about multi-threads, sub task, parallel jobs?

Multi-threads is one of killing features in golang, to add certain level of concurrency, such as parallel jobs, wait and join, queuing have all been taken into consideration, however weighing the pros and cons, it is not a good idea to implement it, for the following reasons:

* Lack of real business case convincing that multi-threads job is a must

If we look into multiple different projects, rarely the task/workflow execution in a CLI need multi-threads execution. A task execution in CLI mainly helps with task dependencies management, integrating with sub modules, workflow orchestration. It must be robust and the data must be consistent. In the modern provisioning workflow, it often interacts with different resources in cloud, such as resource in AWS, Google Cloud, or different API end points. The execution speed is not the highest priority but the expected result, hence the sequential execution step by step is the safest choice.

* Multi-thread concurrency increase the complexity

Not only the complexity of the implementation of this software increases, the way how it could be used also increased. Ultimately, incorrect use of the concurrency would become a pain when problem surfaces and often the hardest part is to correctly understand where the problem stem from, normally this would be caused by combination of wrong sequence/timing of the execution and inconsistent data.

So it is choice not to do it but not because it is not possible 

#### So what is best practice when it comes to parallel tasks job execution in CI/CD?

A task management tool like UP cmd should be always used to run a blocked step by step task. When parallel jobs required, it is better to utilize the feature provided by your CI/CD tools, such as GOCD, Jenkins etc to collaborate the time wait and parallel execution. A good example is the fan-in/fan-out usage, refer to: https://docs.gocd.org/current/introduction/concepts_in_go.html#fan_in_out

    
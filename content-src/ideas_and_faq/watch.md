---
title: "watch and monitoring"
date: 2020-06-25T22:32:46+11:00
draft: false
weight: 2203
---

#### Why not add watching and trigger feature

Sometimes, we may think a little extra feature would make this tool shine, such as watcher and trigger.

With this feature, certain files or a directory could be watched, if there is a detected change, than the build task would be trigger to run certain task.

It seems to be nice idea, but here are the reasons that this wouldn't be implemented

1. UP cmd supposes to be a good companion and integrated with CI/CD build. 

In this case, the trigger normally, mostly likely be from CI/CD tool, eg GoCD pipeline. Anything daemonized will not be good for such kind of integration and hard to kill and nicely managed for its life cycle in a pipelien, or it will be just hanging there to take the unnecessary resources. 

2. This feature could be simply achieved by using other CLI cmd, like watch, or modd, refer to: https://github.com/cortesi/modd


   
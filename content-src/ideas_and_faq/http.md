---
title: "http/rest api support"
date: 2020-06-25T22:32:46+11:00
draft: false
weight: 2204
---

#### Why not a func implementation for http GET/POST or rest API call etc

Technically, it is easy to implement this with existing UP cmd, if it is implemented, then user will just need to follow the api specs in UP cmd to supply all parameters and then get the result.

However, after careful investigation, implementing this is purely a re-inventing the wheel.

The core merit of UP cmd would be a coordinator to provide detailed data to the external CLI command for execution and do fast message exchange and pass on to the next execution until the end of the job. So the question is really to ask if there is already a capable tool for such a purpose.

Yes, simple use curl command will be more than enough. This encourages reusing the best tool within the team and every one follows the simple the best consistent pattern, one tool for all.       

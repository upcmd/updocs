---
title: "use upcli docker image"
date: 2020-06-25T22:32:46+11:00
draft: false
weight: 105
---

#### Use the docker image

##### Load the shell function


```

. ./funcs.rc

```

##### This will load below func

```

run_upcli_docker(){
  docker run -it --rm  -v `pwd`:/workspace docker.pkg.github.com/upcmd/up/upcli:latest /bin/sh
}


```

##### Run the docker container

```
run_upcli_docker
```

This will mount your current directory as project directory to /workspace in container, so that you can use the up command

There are some default tools installed just in case needed, checkout the Dockerfile: https://github.com/upcmd/up/blob/master/Dockerfile

You can add your own tools into the Dockerfile for your local work

If you are interested to add this into the official Docker build, please raise an issue: https://github.com/upcmd/up/issues 
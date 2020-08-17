---
title: "installation"
date: 2020-06-25T22:32:46+11:00
draft: false
weight: 101
---

### Installation

#### Install the latest

* Install for Mac OSX:

```
curl -s https://api.github.com/repos/upcmd/up/releases \
    |grep darwin_amd64_latest \
    |grep download \
    |head -n 1 \
    |awk '{print $2}' \
    |xargs -I % curl -L % -o up \
    && chmod +x up
```

* Install for Linux:

```
curl -s https://api.github.com/repos/upcmd/up/releases \
    |grep linux_amd64_latest \
    |grep download \
    |head -n 1 \
    |awk '{print $2}' \
    |xargs -I % curl -L % -o up \
    && chmod +x up
```

* Install for Windows:

```
curl -s https://api.github.com/repos/upcmd/up/releases \
    |grep windows_amd64_latest \
    |grep download \
    |head -n 1 \
    |awk '{print $2}' \
    |xargs -I % curl -L % -o up \
    && chmod +x up
```

* use func to download it

```
install_latest(){
if [ "$1" == "" ];then
    echo "syntax exaple: install_latest darwin | linux | windows"
else
    os=$1
    curl -s https://api.github.com/repos/upcmd/up/releases \
        |grep ${os}_amd64_latest \
        |grep download \
        |head -n 1 \
        |awk '{print $2}' \
        |xargs -I % curl -L % -o up \
        && chmod +x up
fi
}

```

1. install for mac: 

```
install_latest darwin
```

2. install for linux: 

```
install_latest linux
```

1. install for windows: 

```
install_latest windows
```


#### Install the rolling release

```

list_rolling(){
curl -s https://api.github.com/repos/upcmd/up/releases \
    |grep darwin_amd64_rolling \
    |grep download \
    |awk '{print $2}'  |cut -d \-  -f2|cut -d \/ -f1
}    

download_rolling(){
if [ "$1" == "" ];then
    echo "syntax exaple: download_rolling 20200814"
else
    ver=$1
    curl -s https://api.github.com/repos/upcmd/up/releases \
        |grep darwin_amd64_rolling \
        |grep download \
        |grep $ver \
        |awk '{print $2}' \
        |xargs -I % curl -L % -o up \
        && chmod +x up
fi
}    

list_rolling

download_rolling 20200814
```

#### Install the latest tagged release

* Install for Mac OSX:

```
curl -s https://api.github.com/repos/upcmd/up/releases \
    |grep darwin_amd64_v \
    |grep download \
    |head -n 1 \
    |awk '{print $2}' \
    |xargs -I % curl -L % -o up \
    && chmod +x up
```

* Install for Linux:

```
curl -s https://api.github.com/repos/upcmd/up/releases \
    |grep linux_amd64_v \
    |grep download \
    |head -n 1 \
    |awk '{print $2}' \
    |xargs -I % curl -L % -o up \
    && chmod +x up
```

* Install for Windows:

```
curl -s https://api.github.com/repos/upcmd/up/releases \
    |grep windows_amd64_v \
    |grep download \
    |head -n 1 \
    |awk '{print $2}' \
    |xargs -I % curl -L % -o up \
    && chmod +x up
```

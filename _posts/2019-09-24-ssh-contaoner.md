---
title: "如何用 ssh 連線到 Docker Container"
header:
  image: /assets/images/docker.png
---
## 如何用 ssh 連線到 Docker Container

首先在Dockerfile中安裝好ssh
<br>
並且將entrypoint設定好
<br>
entrypoint的用意為 每當container開始運行時重啟ssh服務
<br>
範例:
```Dockerfile
FROM ubuntu:18.04

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        build-essential \
        curl \
        wget \
        git \
        gcc \
		ssh \
	    vim && \
    rm -rf /var/lib/apt/lists/*

ENTRYPOINT service ssh restart && bash
```

當docker run建立container時 記得開啟對外的port
<br>
參數為 -p 外部port:內部port

```shell
docker run -p 1234:22 helloworld:latest
```


運行container之後 要先更改密碼
```shell
passwd
```
再來要更改ssh的設定
<br>
ssh的設定檔在Ubuntu系統的預設位置為:
<br>
```shell
/etc/ssh/sshd_config
```
把PasswordAuthentication的註解拿掉 並更改設定為yes
```vim
...
PasswordAuthentication yes
...
```
最後重啟ssh服務就好囉
```shell
service ssh restart
```
---
layout: single
title: "Raspberry pi5 _ ubuntu"
categories:
  - Raspberry pi5
tags:
  - [Raspberrypi5, pi5, ubuntu]
toc: true
toc_sticky: true

Date: 2024-06-06
published: true
---

# 📌 SSH
SSH (보안 셸, Secure Shell)은 네트워크 프로토콜로, 컴퓨터와 네트워크 장치 사이의 안전한 데이터 통신을 가능하게 한다.

원격 로그인 기능을 통해 Ubuntu 에 접속해보자.

## 📋 설치

기본적으로 접속하고자 하는 기기에 ssh server 가 있어야 작동하는데 ubuntu 에는 깔려있지 않다.

모니터를 연결하여 ssh server 를 설치해주어야 한다.

```Bash
sudo apt update
sudo apt install openssh-server
```

설치 후 정상적으로 실행 중인지 확인한다.

```Bash
sudo systemctl status ssh
```

## 📋 포트
### 기본포트

기본포트는 22번이다.

```Bash
sudo ufw allow ssh
```

### 포트변경

```Bash
sudo vi /etc/ssh/sshd_config

#Port 22
Port 1234
```

변경했으면 재시작한다.

```Bash
sudo service ssh restart
```

변경한 포트에서 돌아가고 있는지 확인하자.
```Bash
netstat -tnlp
```

### net-tools

만약 `netstat` 명령어가 작동하지 않는다면 `net-tools` 를 설치하자.

```Bash
sudo apt install net-tools
```

### 방화벽 열어주기
```Bash
sudo ufw deny 22
sudo ufw allow 1234
```

## 📋 연결
```Bash
# 바꾼 포트를 사용하자.

ssh -p 1234 baecrong@192.168.0.0
```
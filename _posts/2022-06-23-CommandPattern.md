---
layout : single
title : "커맨드 패턴 (Command Pattern)"
categories:
  - CSharpStudy
tags:
  - [C#, CSharp, Study, Interview]

toc: true
toc_sticky: true

Date: 2022-06-23
published: false
---

# 📌 Command Pattern

## ✏️ 커맨드 패턴이란?
실행될 기능을 캡슐화하여 여러 기능을 실행할 수 있는 재사용성이 높은 클래스를 설계하는 패턴이다.

커맨드 패턴은 몇 가지 역할로 구성된다.

> **Command**
> - 실행될 기능에 대한 인터페이스
> - 실행될 기능을 execute()로 
> **ConcreteCommand**
> **Invoker**
> **Receiver**
> **Client**


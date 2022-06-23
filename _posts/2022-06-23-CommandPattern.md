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
> 
> **ConcreteCommand**
> - 실행되는 기능을 구현하는 클래스
> - Receiver가 무엇을 처리해야 하는지 세부구현
> 
> **Invoker**
> - 기능의 실행을 요청하는 호출자 클래스
> - Client의 요청을 받아 Receiver의 기능 호출
> 
> **Receiver**
> - ConcreteCommand의 기능을 실행하기 위해 사용하는 수신자 클래스
> - Client가 요청한 내용에 대해 액션만 취해주면 됨
> 
> **Client**
> - 요청자
> - 누가(Receiver)가 어떻게 처리할 지(ConcreteCommand)만 알고 있으면 됨


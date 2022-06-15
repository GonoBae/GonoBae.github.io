---
layout : single
title : "옵저버 패턴 (Observer Pattern)"
categories:
  - CSharpStudy
tags:
  - [C#, CSharp, Study, Interview]

toc: true
toc_sticky: true

Date: 2022-06-15
published: false
---

# 📌 Observer Pattern

## ✏️ C\#

### 📋 옵저버 패턴이란?
객체의 상태 변화를 관찰하는 관찰자들, 즉 옵저버들의 목록을 객체에 등록하여 상태 변화가 있을 때마다

메소드 등을 통해 객체가 직접 목록의 각 옵저버에게 통지하도록 하는 디자인 패턴.

주로 분산 이벤트 핸들링 시스템을 구현하는 데 사용된다.

발행 / 구독 모델로 알려져 있기도 하다.

예제로 이해해보자.

만약 유튜버가 방송을 켠다.

그럼 구독자들은 방송의 알림을 받게 된다.

당연하게도 구독을 하지 않은 사람들은 메시지를 받지 않는다.

```cs
using System;

interface Youtuber
{
    void subscribe(Subscriber sub);
    void unsubscribe(Subscriber sub);
    void notify(string msg);
}

interface Subscriber
{
    void update(string msg);
}

class Holy : Youtuber
{
    private List<Subscriber> subscribers = new List<Subscriber>();

    public void food()
    {
        Console.WriteLine("먹 방");
        notify("먹방 방송이 시작되었습니다.");
    }

    public void dance()
    {
        Console.WriteLine("춤 방");
        notify("춤방 방송이 시작되었습니다.");
    }

    public void notify(string msg)
    {
        foreach (Subscriber sub in subscribers)
        {
            sub.update(msg);
        }
    }

    public void subscribe(Subscriber sub)
    {
        subscribers.Add(sub);
    }

    public void unsubscribe(Subscriber sub)
    {
        subscribers.Remove(sub);
    }
}

class Danny : Subscriber
{
    public void update(string msg)
    {
        Console.WriteLine("Danny : " + msg);
    }
}

class Lala : Subscriber
{
    public void update(string msg)
    {
        Console.WriteLine("Lala : " + msg);
    }
}

class Land : Subscriber
{
    public void update(string msg)
    {
        Console.WriteLine("Land : " + msg);
    }
}

class ObserverPattern
{
    public static void Main1()
    {
        Holy holy = new Holy();
        Danny danny = new Danny();
        Lala lala = new Lala();
        Land land = new Land();

        holy.subscribe(danny);
        holy.subscribe(lala);
        holy.subscribe(land);

        holy.food();

        holy.unsubscribe(lala);

        holy.dance();
    }
}
```

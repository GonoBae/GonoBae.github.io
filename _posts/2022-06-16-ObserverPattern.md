---
layout : single
title : "옵저버 패턴 (Observer Pattern)"
categories:
  - UnityStudy
tags:
  - [C#, CSharp, Study, Interview, Observer, Unity]

toc: true
toc_sticky: true

Date: 2022-06-16
published: true
---

# 📌 Observer Pattern

## ✏️ 옵저버 패턴이란?
객체의 상태 변화를 관찰하는 관찰자들, 즉 옵저버들의 목록을 객체에 등록하여 상태 변화가 있을 때마다

메소드 등을 통해 객체가 직접 목록의 각 옵저버에게 통지하도록 하는 디자인 패턴.

주로 분산 이벤트 핸들링 시스템을 구현하는 데 사용된다.

발행 / 구독 모델로 알려져 있기도 하다.

예제로 이해해보자.

만약 유튜버가 방송을 켠다.

그럼 구독자들은 방송의 알림을 받게 된다.

당연하게도 구독을 하지 않은 사람들은 메시지를 받지 않는다.

### 📋 코드

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
    public static void Main()
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

우선 Youtuber 와 Subscriber 인터페이스가 존재한다.

Holy 라는 유튜버는 구독자 리스트를 가지고 있고, 방송을 하면 구독자들에게 메시지를 보낸다.

Danny , Lala , Land 는 Holy 를 구독했다.

그래서 Holy 의 먹방 알림을 받는다.

잠시후 Lala 는 구독을 취소했다.

Holy 가 춤 방송을 시작하지만 Lala 에게는 알림이 오지 않는다.

### 💻 실행

![1](https://user-images.githubusercontent.com/87271529/173989335-84554060-c934-457e-82e8-51d7f8efcce7.png)

## Unity 에서 사용해보기
옵저버 패턴은 delegate, Action, Func 등 다양한 방식으로 구현할 수 있는데, Action 으로 구현해보자.

### 📋 코드

```cs
// 클릭할 대상
using UnityEngine;
using System;

public class ObserverTest : MonoBehaviour
{
    public static event Action ClickEvent;

    private void OnMouseDown()
    {
        Debug.Log("Click");
        ClickEvent?.Invoke();
    }
}
```

```cs
// 클릭 시 일어날 이벤트
using UnityEngine;
using UnityEngine.UI;

public class Observer : MonoBehaviour
{
    private AudioSource audioSource;
    private Text text;
    int count = 0;

    private void Awake()
    {
        audioSource = GetComponent<AudioSource>();
        text = FindObjectOfType<Text>();
    }

    private void OnEnable()
    {
        ObserverTest.ClickEvent += PlayAudio;
        ObserverTest.ClickEvent += UpdateText;
    }

    private void OnDisable()
    {
        ObserverTest.ClickEvent -= PlayAudio;
        ObserverTest.ClickEvent -= UpdateText;
    }

    private void PlayAudio() => audioSource.Play();

    private void UpdateText() => text.text = (++count).ToString();
}

```

내가 원하는 대상을 클릭하면 소리와 클릭한 횟수가 UI 로 출력된다.

### 💻 실행

![1](https://user-images.githubusercontent.com/87271529/173992323-97aae658-48cf-47e8-8094-9dac3a264788.gif)

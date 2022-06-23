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
published: true
---

# 📌 Command Pattern

## ✏️ 커맨드 패턴이란?
실행될 기능을 캡슐화하여 여러 기능을 실행할 수 있는 재사용성이 높은 클래스를 설계하는 패턴이다.

또한 기능이 추가될 때 코드 수정없이 기능에 대한 클래스만 정의하면 되어 확장성이 유연해진다.

커맨드 패턴은 몇 가지 역할로 구성된다.

> **Abstract Command**
> - 실행될 기능에 대한 인터페이스
> - 실행될 기능을 execute()로 정의
> 
> **Concrete Command**
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

### 📋 코드

- Abstract Command

```cs
public interface ICommand
{
    // 실행할 기능
    void Execute();
    // 되돌리기
    void Undo();
}
```

- Receiver

```cs
using UnityEngine;

public class Lightbulb : MonoBehaviour
{
    bool isPowerOn = false;
    
    // 전등 껐다 켜기
    public void TogglePower()
    {
        if(isPowerOn == false)
        {
            GetComponent<Renderer>().material.EnableKeyword("_EMISSION");
            transform.GetChild(0).gameObject.SetActive(true);
            isPowerOn = true;
        }
        else
        {
            GetComponent<Renderer>().material.DisableKeyword("_EMISSION");
            transform.GetChild(0).gameObject.SetActive(false);
            isPowerOn = false;
        }
    }
    
    // 전등 원하는 색으로 바꾸기
    public void SetLightColor(Color newColor)
    {
        Material material = GetComponent<Renderer>().material;
        material.color = newColor;
        material.SetColor("_EmissionColor", newColor);
        transform.GetChild(0).gameObject.GetComponent<Light>().color = newColor;
    }

    // 전등 랜덤 색으로 바꾸기
    public void SetRandomColor()
    {
        Color randomColor = Random.ColorHSV(0f, 1f, 1f, 1f, 0.5f, 1f);
        Material material = GetComponent<Renderer>().material;
        material.color = randomColor;
        material.SetColor("_EmissionColor", randomColor);
        transform.GetChild(0).gameObject.GetComponent<Light>().color = randomColor;
    }
}
```

- Invoker

```cs
using System.Collections.Generic;

public class LightApp
{
    // 명령 저장해두기
    Stack<ICommand> _commandList;

    public LightApp()
    {
        _commandList = new Stack<ICommand>();
    }

    public void AddCommand(ICommand newCommand)
    {
        newCommand.Execute();
        _commandList.Push(newCommand);
    }

    public void UndoCommand()
    {
        if( _commandList.Count > 0 )
        {
            ICommand latestCommand = _commandList.Pop();
            latestCommand.Undo();
        }
    }
}
```

- Concrete Command

```cs
public class TogglePowerCommand : ICommand
{
    Lightbulb _lightbulb;
    public TogglePowerCommand(Lightbulb lightbulb)
    {
        _lightbulb = lightbulb;
    }
    public void Execute()
    {
        _lightbulb.TogglePower();
    }

    public void Undo()
    {
        _lightbulb.TogglePower();
    }
}
```

```cs
using UnityEngine;

public class ChangeColorCommand : ICommand
{
    Lightbulb _lightbulb;

    Color _previousColor;

    public ChangeColorCommand(Lightbulb lightbulb)
    {
        _lightbulb = lightbulb;
        _previousColor = _lightbulb.GetComponent<Renderer>().material.color;
    }

    public void Execute()
    {
        if(_lightbulb.isPowerOn)
        {
            _lightbulb.SetRandomColor();
        }
    }

    public void Undo()
    {
        _lightbulb.SetLightColor(_previousColor);
    }
}
```

- Client

```cs
using UnityEngine;

public class UserInput : MonoBehaviour
{
    public Lightbulb _lightbulb;

    LightApp _lightapp;

    private void Start()
    {
        _lightapp = new LightApp();
    }

    private void Update()
    {
        if(Input.GetKeyDown(KeyCode.Space))
        {
            ICommand togglePowerCommand = new TogglePowerCommand(_lightbulb);
            _lightapp.AddCommand(togglePowerCommand);
        }
        else if(Input.GetKeyDown(KeyCode.C))
        {
            ICommand changeColorCommand = new ChangeColorCommand(_lightbulb);
            _lightapp.AddCommand(changeColorCommand);
        }
        else if(Input.GetKeyDown(KeyCode.Z))
        {
            _lightapp.UndoCommand();
        }
    }
}
```

### 💻 실행
![1](https://user-images.githubusercontent.com/87271529/175237222-a7531ce8-f0c8-4e1d-be79-077df1e8e7c6.gif)

굿!!

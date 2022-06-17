---
layout : single
title : "컴포넌트 패턴 (Component Pattern)"
categories:
  - CSharpStudy
tags:
  - [C#, CSharp, Study, Interview]

toc: true
toc_sticky: true

Date: 2022-06-18
published: true
---

# 📌 Component Pattern

## ✏️ 컴포넌트 패턴이란?
Component 는 요소, 부품 이라는 의미를 가진다.

이 뜻대로 우리가 필요한 요소 및 부품을 골라 넣어줄 수 있는 패턴이다.

유니티를 사용해보면 우리가 필요한 컴포넌트를 찾아 오브젝트에 넣어주게 된다.

AudioSource , Rigidbody , Collider 등등.

이들은 각자의 기능을 가지고 독립적으로 동작하는 요소이기 때문에 추가 및 제거가 쉽다.

독립적이라는 말은 오브젝트 내에서 함께 사용되어도 이들끼리 영향을 끼치지 않는다는 의미이고

이 말은 서로서로 **디커플링** 되어있다.라고 한다.

또한 컴포넌트 패턴은 재사용이 가능하다.

내가 플레이하는 Player 외에도 어디서든 Rigidbody 를 사용하듯

Player, Enemy, AI 등에서 물리를 위한 컴포넌트를 사용할 수 있다.

### 📋 코드

```cs
using UnityEngine;

public class Com_PlayerController : MonoBehaviour
{
    private float vertical;
    private float _power = 500f;
    private Rigidbody _rigidbody;

    private void Start()
    {
        _rigidbody = GetComponent<Rigidbody>();
    }
    private void Update()
    {
        vertical = Input.GetAxisRaw("Vertical");
        if (vertical != 0)
        {
            _rigidbody.AddForce(new Vector3(0f, 0f, vertical) * _power * Time.deltaTime);
        }
    }
}
```

```cs
using UnityEngine;

public class Com_Inventory : MonoBehaviour
{
    private KeyCode _inventory = KeyCode.I;
    [SerializeField] private GameObject _UI;

    private void Update()
    {
        if(Input.GetKeyDown(_inventory))
        {
            CheckActive();
        }
    }

    private void CheckActive()
    {
        if(_UI.activeSelf == true)
        {
            _UI.SetActive(false);
        }
        else
        {
            _UI.SetActive(true);
        }
    }
}
```

```cs
using UnityEngine;

public class Com_Click : MonoBehaviour
{
    private Renderer renderer;
    private void Start()
    {
        renderer = GetComponent<Renderer>();
    }

    private void OnMouseDown()
    {
        ChangeColor();
    }

    private void ChangeColor()
    {
        renderer.material.color = new Color(Random.Range(0f, 1f), Random.Range(0f, 1f), Random.Range(0f, 1f));
    }
}
```

내가 만드는 캐릭터를 조작할 수 있었으면 좋겠고, I 키를 누르면 인벤토리도 나왔으면 좋겠고,

클릭했을 때 색도 바뀌었으면 좋겠다.

물론 이들을 한 클래스 내에서 구현할 수 있지만 추가적인 기능을 구현해 나갈 때 어려움이 있다.

그래서 각각의 기능을 클래스로 만들고 한 오브젝트에 3개의 스크립트를 넣어준다.

![1](https://user-images.githubusercontent.com/87271529/174349316-f5c673a2-cd20-4a06-ad7f-657302a16bf9.png)

### 💻 실행
![1](https://user-images.githubusercontent.com/87271529/174349336-7712cf3d-b09c-4f8f-bcd5-aa2a65a4f81d.gif)


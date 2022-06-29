---
layout : single
title : "State Pattern"
categories:
  - UnityStudy
tags:
  - [C#, CSharp, Study, Interview, Unity]

toc: true
toc_sticky: true

Date: 2022-06-29
published: false
---

# 📌 State Pattern

## ✏️ 상태 패턴이란?

상태패턴이란 각 상태별로 스크립트를 분리

즉, 시스템의 각 상태를 클래스로 분리해 표현하는 것이다.

한 스크립트에 모두 넣을 수 있는 내용을 왜 나누는 것일까?

첫쨰로 한 객체의 상태를 여러 스크립트로 나누는 것이기에 코드의 가독성이 좋아진다.

둘째로 각 상태별로 코드를 관리하기에 개발하는 입장에서 관리가 쉽다. (상태 추가가 쉽다.)

하지만 단점은 존재한다.

상태가 너무 많아지게 되면 그만큼 스크립트가 증가하기에 관리가 어렵다.

이 하나가 굉장히 큰 단점이 될 수 있다고 생각한다.

이제 실제 게임에서 어떻게 사용될 수 있는지 한 번 생각해보자.

만약 게임에 존재하는 상자에 상태를 적용한다면 어떻게 될까?

상자가 기본 상태 / 들려진 상태 / 놓아진 상태 / 던저진 상태 를 가진다고 생각해보자.

그리고 상자 오브젝트는 이 모든 스크립트를 가지는 것이 아닌,

이들을 관리하는 매니저 스크립트를 가진다.

그럼 각 상태별로 코드를 짜준다.

(우선 모든 상태는 BaseState 이라는 추상 클래스를 상속받는다.)

## 📋 코드

### 📝 BaseState

```cs
using UnityEngine;

public abstract class BoxBaseState
{
	public abstract void EnterState(BoxStateManager box);
	public abstract void UpdateState(BoxStateManager box);
	public abstract void OnCollisionEnter(BoxStateManager box, Collision collision);
}
```

### 📝 IdleState

```cs
using UnityEngine;

public class BoxIdle : BoxBaseState
{
	public override void EnterState(BoxStateManager box) { }
	
	public override void UpdateState(BoxStateManager box)
	{
		if(box.player != null && box.player._yes)
		{
			box.player._yes = false;
			box.SwitchState(box._pickState);
		}
	}
	
	public override void OnCollisionEnter(BoxStateManager box, Collision collision)
	{
		GameObject other = collision.gameObject;
		if(other.CompareTag("Player"))
		{
			if(box.player == null) box.player = other.GetComponent<PlayerController>();
			box.player.DrawUI();
		}
	}
}
```

### 📝 PickedupState

```cs
using UnityEngine;

public class BoxPickedUp : BoxBaseState
{
	public override void EnterState(BoxStateManager box)
	{
		box.transform.position = box.player.transform.position + new Vector3(0, 2f, 0);
		box.transform.SetParent(box.player.transform);
		box.GetComponent<Rigidbody>().isKinematic = true;
	}
	
	public override void UpdateState(BoxStateManager box)
	{
		if(box.player._putDown)
		{
			box.player._putDown = false;
			box.SwitchState(box._putDownState);
		}
		else if(box.player._throwAway)
		{
			box.player._throwAway = false;
			box.SwitchState(box._throwAwayState);
		}
	}
	
	public override void OnCollisionEnter(BoxStateManager box, Collision collision) { }
}
```

### 📝 PutDownState

```cs
using UnityEngine;

public class BoxPutdown : BoxBaseState
{
	float _countdown = 1.0f;
	
	public override void EnterState(BoxStateManager box)
	{
		box.transform.SetParent(null);
		box.transform.position = box.player.transform.position + new Vector3(2, 0, 0);
		box.GetComponent<Rigidbody>().isKinematic = false;
	}
	
	public override void UpdateState(BoxStateManager box)
	{
		if(_countdown > 0)
			_countdown -= Time.deltaTime;
		else
			box.SwitchState(box._idleState);
	}
	
	public override void OnCollisionEnter(BoxStateManager box, Collision collision) { }
}
```

### 📝 ThrowedAwayState

```cs
using UnityEngine;

public class BoxThrowedAway : BoxBaseState
{
	float _countdown = 2.0f;
	
	public override void EnterState(BoxStateManager box)
	{
		box.transform.SetParent(null);
		box.GetComponent<Rigidbody>().isKinematic = false;
		box.GetComponent<Rigidbody>().AddForce(Vector3.right * 10, ForceMode.Impulse);
	}
	
	public override void UpdateState(BoxStateManager box)
	{
		if(_countdown > 0)
			_countdown -= Time.deltaTime;
		else
			box.SwitchState(box._idleState);
	}
	
	public override void OnCollisionEnter(BoxStateManager box, Collision collision) { }
}
```

### 📝 

```cs
using UnityEngine;

public class BoxStateManager : MonoBehaviour
{
	BoxBaseState _currentState;
	
	public PlayerController player;
	public BoxIdle _idleState = new BoxIdle();
	public BoxPickedUp _pickState = new BoxPickedUp();
	public BoxPutdown _putDownState = new BoxPutdown();
	public BoxThrowedAway _throwAwayState = new BoxThrowedAway();
	
	private void Start()
	{
		_currentState = _idleState;
		_currentState.EnterState(this);
	}
	
	protected void Update()
	{
		_currentState.UpdateState(this);
	}
	
	public void SwitchState(BoxBaseState state)
	{
		_currentState = state;
		_currentState.EnterState(this);
	}
	
	void OnCollisionEnter(Collision collision)
	{
		_currentState.OnCollisionEnter(this, collision);
	}
}
```

## 💻 실행

![1](https://user-images.githubusercontent.com/87271529/176471784-06742866-4fc6-493d-8c24-567087a4a649.gif)

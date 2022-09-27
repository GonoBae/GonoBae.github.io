---
layout : single
title : "캐릭터 유령화"
categories:
  - PoliceAndThief
tags:
  - [Unity, PoliceAndThief]

toc: true
toc_sticky: true

Date: 2022-09-27
published: true
---

# 📌 Ghost

어몽어스 게임에서 임포스터에게 죽게되면 유령이 된다.

내 화면에서는 유령으로 , 상대 화면에서는 시체 혹은 안보이게 된다.

이를 Photon (포톤) 으로 구현해보자.

## 📝 Player

우선 Player 는 기본적인 Material 을 가진다.

![Player](https://user-images.githubusercontent.com/87271529/192085120-281b229b-6a82-4366-bdb9-72ef4bf76f27.png)

유령처럼 보이기 위해 Material 의 Alpha 값을 낮추고 Rendering Mode 를 Transparent 로 바꿔 주어야 한다.

코드로 다 바꿔줄 수 있다.

하지만 Texture Atlas 로 인해 죽은 캐릭터를 유령으로 바꾸면 모든 플레이어가 바뀌는 현상이 생기므로

유령에 쓸 Material 을 새로 만들어준다.

## 📝 원리

죽은 캐릭터는 유령 Material 로 교체해준다.

![2](https://user-images.githubusercontent.com/87271529/192454293-e21f5c6b-5e94-4f2a-afe9-e25a7fc95ec1.png)

기본 캐릭터의 body , head 가 있고 유령의 bodyGhost , headGhost 가 있다.

유령의 Material 을 기본 캐릭터의 Material 에서 유령 Material 교체해주면 된다.

이를 포톤 서버의 RPC 메소드를 통해 내 화면에서는 유령으로, 상대 화면에서는 시체로 만들어주자.

### 📋 Code

```cs
public Renderer _body;
public Renderer _head;
public Material _bodyGhost;
public Material _headGhost;

private void Awake()
{
	_pv = GetComponent<PhotonView>();
	_bodyGhost = Resources.Load<Material>("BodyGhost");
	_headGhost = Resources.Load<Material>("HeadGhost");
}

public void TakeDamage(string attacker)
{
	_pv.RPC("RPC_TakeDamage", RpcTarget.All);
}

// 내 화면 : 고스트 , 다른 화면 : 시체
[PunRPC]
private void RPC_TakeDamage()
{
	_playerLife = PlayerLife.Ghost;
	
	if (_pv.IsMine)
	{
		ChangeMaterial(_bodyGhost, _headGhost);
		RemoveGravityCollider();
	}
	else
	{
		_ani.SetTrigger("Hit");
		GetComponent<PhotonTransformView>().enabled = false;
		GetComponent<CapsuleCollider>().enabled = false;
	}
	GetComponent<PhotonAnimatorView>().enabled = false;
}
```

### 💻 Execute

죽인 입장

![2](https://user-images.githubusercontent.com/87271529/192459131-a7113cc6-a82d-410a-bfcf-b877153a32bb.gif)

죽은 입장

![3](https://user-images.githubusercontent.com/87271529/192459170-5cc48603-44d1-4e7a-ab3a-a2b474e04687.gif)

---
layout : single
title : "Path Finding Algorithm"
categories:
  - UnityStudy
tags:
  - [Unity]

toc: true
toc_sticky: true

Date: 2022-07-28
published: true
---

# 📌 A* Algorithm

## ✏️ A* Algorithm 이란?
LOL 혹은 Starcraft 와 같은 게임을 하다보면 선택한 캐릭터를 마우스로 조작할 수 있다.

우리가 원하는 목표로 캐릭터를 이동시킬 수 있는데,

이 때 캐릭터는 최단경로로 목표지점까지 이동한다.

A* 알고리즘은 최단경로를 찾는 알고리즘 중 하나이다.

시작지점과 목표지점이 명확히 주어진 상태에서 작동하고,

각 노드는 이동에 필요한 경비(Cost)를 3가지 가지게 된다.

G_Cost = 시작점에서 현재 위치까지의 거리

H_Cost = 현재 위치에서 끝점까지의 거리

F_Cost = G_Cost + H_Cost

이렇게 3가지의 비용을 가지며 이들을 비교하여 최단거리를 구하게 된다.

## 📝 전체 크기
모든 게임은 맵이 존재한다.

사용자가 편하게 Grid 설정을 조절하기 쉽게 해보자.

스크립트는 MyGrid.cs 를 만들어서 해보자.

### 📋 Code

```cs
[SerializeField] Vector2 _gridSize;
private void OnDrawGizmos()
{
  Gizmos.DrawWireCube(transform.position, new Vector3(_gridSize.x, 0.5f, _gridSize.y));
}
```

DrawWireCube 를 통해 와이어프레임을 큐브 형태로 그려준다.

현재 스크립트가 달린 위치에 new Vector3 의 x, y, z 사이즈로 그리게 된다.

### 💻 Execute

![1](https://user-images.githubusercontent.com/87271529/181204637-50368688-24de-4ac1-9223-5805df622237.gif)

## 📝 원하는 사이즈로 자르기
전체 틀의 사이즈를 잡았다면 이를 잘게 잘라보자.

이를 위해서 Node.cs 클래스를 만들어보자.

각 노드는 위치, x, y 값을 가진다.

그리고 위치값을 토대로 grid 를 그리며 다음과 같이 그려진다.

제일 왼쪽, 아래가 시작점 이라는 것을 기억하자.

![1](https://user-images.githubusercontent.com/87271529/181211821-e7a53703-d7aa-4e24-a56b-8fc4d021034d.png)

### 📋 Code

Node.cs
```cs
public class Node
{
  public Vector3 _worldPos;
  public int _gridX;
  public int _gridY;
	
  public Node(Vector3 worldPos, int gridX, int gridY)
  {
	  _worldPos = worldPos;
	  _gridX = gridX;
	  _gridY = gridY;
  }
}
```

Grid.cs
```cs
[SerializeField] bool _displayGizmos;
[SerializeField] Vector2 _gridSize;
[SerializeField] float _nodeRadius;
Node[,] _grid;
float _nodeDiameter;
int _gridSizeX , _gridSizeY;

void Awake()
{
	_nodeDiameter = _nodeRadius * 2;
	_gridSizeX = Mathf.RoundToInt(_gridSize.x / _nodeDiameter);
	_gridSizeY = Mathf.RoundToInt(_gridSize.y / _nodeDiameter);
	CreateGrid();
}

void CreateGrid()
{
	_grid = new Node[_gridSizeX, _gridSizeY];
	Vector3 worldBottomLeft = 
		transform.position - Vector3.right * (_gridSize.x / 2) - Vector3.forward * (_gridSize.y / 2);
	for(int x = 0; x < _gridSizeX; x++)
	{
		for(int y = 0; y < _gridSizeY; y++)
		{
			Vector3 worldPos = 
				worldBottomLeft + Vector3.right * (x * _nodeDiameter + _nodeRadius)
				+ Vector3.forward * (y * _nodeDiameter + _nodeRadius);
			_grid[x, y] = new Node(worldPos, x, y);
		}
	}
}

private void OnDrawGizmos()
{
	Gizmos.DrawWireCube(transform.position, new Vector3(_gridSize.x, 0.5f, _gridSize.y));
	if(_grid != null && _displayGizmos)
	{
		foreach(var node in _grid)
		{
			Gizmos.color = Color.white;
			Gizmos.DrawCube(node._worldPos, Vector3.one * (_nodeDiameter - 0.1f));
		}
	}
}
```

### 💻 Execute

![2](https://user-images.githubusercontent.com/87271529/181212155-2c0ce00b-5c9a-424c-9998-59ae3f7a1c43.gif)

## 📝 장애물 체크
이제 하나의 Node 위에 장애물이 존재하는지 체크를 해주어야 한다.

그래야 player 가 갈 수 있는지 없는지를 체크할 수 있기 때문!

약간의 코드만 추가해주면 된다.

### 📋 Code

Node.cs
```cs
public class Node
{
	public Vector3 _worldPos;
	public int _gridX;
	public int _gridY;
	public bool _walkable;
	
  public Node(Vector3 worldPos, int gridX, int gridY, bool walkable)
	{
		_worldPos = worldPos;
		_gridX = gridX;
		_gridY = gridY;
		_walkable = walkable;
	}
}
```

Grid.cs
```cs
public class MyGrid : MonoBehaviour
{
	[SerializeField] bool _displayGizmos;
	[SerializeField] Vector2 _gridSize;
	[SerializeField] float _nodeRadius;
	[SerializeField] LayerMask _unwalkableMask;
	Node[,] _grid;
	float _nodeDiameter;
	int _gridSizeX , _gridSizeY;
	
  void Awake()
	{
		// 그대로
	}
	
  void CreateGrid()
	{
    {
      {
				Vector3 worldPos = 
					worldBottomLeft + Vector3.right * (x * _nodeDiameter + _nodeRadius)
					+ Vector3.forward * (y * _nodeDiameter + _nodeRadius);
        // 아래 변경
				bool walkable = !(Physics.CheckSphere(worldPos, _nodeRadius, _unwalkableMask));
				_grid[x, y] = new Node(worldPos, x, y, walkable);
      }
    }
	}
  
  private void OnDrawGizmos()
	{
    // Gizmos.color = Color.white; 을 지우고 아랫줄 추가
		Gizmos.color = (node._walkable) ? Color.white : Color.red;
	}
}
```

### 💻 Execute

![3](https://user-images.githubusercontent.com/87271529/181285662-59bbd2b5-e286-4ff1-88bd-76ce516484ca.gif)

CheckSphere 의 경우 반지름 0.5 로 체크를 하기 때문에

장애물이 칸을 조금이라도 넘어가면 옆칸도 이동이 불가능하다고 나온다.

```cs
bool walkable = !(Physics.CheckSphere(worldPos, _nodeRadius, _unwalkableMask));
```
부분의 radius 부분을 radius - 0.2f 정도로 검사범위를 좁혀주면 조금 더 여유롭게 검사하게 된다.

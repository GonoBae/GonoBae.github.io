---
layout : single
title : "Path Finding Algorithm 2"
categories:
  - UnityStudy
tags:
  - [Unity]

toc: true
toc_sticky: true

Date: 2022-09-02
published: false
---

# 📌 A* Algorithm
길 찾기를 하기 위해 

## 📝 Player Grid

플레이어의 위치를 Grid 위에 표시해보자.

### 📋 Code

MyGrid.cs 에 추가해 줄 내용
```cs
public Transform _player;

public Node NodeFromWorldPoint(Vector3 worldPosition)
{
  float percentX = (worldPosition.x + _gridSize.x * 0.5f) / _gridSize.x;
  float percentY = (worldPosition.z + _gridSize.y * 0.5f) / _gridSize.y;
		
  int x = Mathf.FloorToInt(_gridSizeX * percentX);
  int y = Mathf.FloorToInt(_gridSizeY * percentY);
		
  return _grid[x, y];
}

private void OnDrawGizmos()
{
  Gizmos.DrawWireCube(transform.position, new Vector3(_gridSize.x, 0.5f, _gridSize.y));
  if(_grid != null && _displayGizmos)
  {
    Node playerNode = NodeFromWorldPoint(_player.position);
    foreach(var node in _grid)
    {
      Gizmos.color = (node._walkable) ? Color.white : Color.red;     
      
      // 추가된 부분
      if(playerNode == node) // 플레이어가 서있는 노드가 현재 그려지고 있는 노드와 같다면
      {
        Gizmos.color = Color.cyan; // 색을 바꿔라
      }
      // 추가 끝

      Gizmos.DrawCube(node._worldPos, Vector3.one * (_nodeDiameter - 0.1f));
    }
  }
}
```

플레이어의 위치가 전체 Grid 의 몇 퍼센트 쯤에 서있는지 확인하고 해당 Node 를 반환해준다.

이 Node 와 OnDrawGizmos() 에서 그려주는 Node 를 비교하여 색을 바꿔준다.

### 💻 Execute

![1](https://user-images.githubusercontent.com/87271529/188099621-912c5eeb-01f6-4dd1-acf4-34d074768ea8.gif)

## 📝 Path Finding

플레이어가 서있는 위치에서 목표 지점까지 길을 찾는 기능을 만들어보자.

### 📋 Code



### 💻 Execute



### 📋 Code



### 💻 Execute



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
published: 
---

# 📌 A* Algorithm
길 찾기를 본격적으로 해보자.

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

Node.cs

```cs
public class Node
{
  public Vector3 _worldPos;
  public int _gridX;
  public int _gridY;
  public bool _walkable;
	
  public int _gCost;
  public int _hCost;
  
  public int _fCost
  {
    get
    {
      return _gCost + _hCost;
    }
  }
	
  public Node _parent;
	
  public Node(Vector3 worldPos, int gridX, int gridY, bool walkable)
  {
    _worldPos = worldPos;
    _gridX = gridX;
    _gridY = gridY;
    _walkable = walkable;
  }
}
```

MyGrid.cs

```cs
public class MyGrid : MonoBehaviour
{
  public Transform _player;
  [SerializeField] bool _displayGizmos;
  [SerializeField] Vector2 _gridSize;
  [SerializeField] float _nodeRadius;
  [SerializeField] LayerMask _unwalkableMask;
  Node[,] _grid;
  float _nodeDiameter;
  int _gridSizeX , _gridSizeY;
	
  public List<Node> _path;
	
  public List<Node> GetNeighbours(Node node)
  {
    List<Node> neighbours = new List<Node>();
		
    for(int x = -1; x <= 1; x++)
    {
	    for(int y = -1; y <= 1; y++)
	    {
	      // 제자리이면 앞으로
	      if(x == 0 && y == 0) continue;
				
	      int checkX = node._gridX + x;
	      int checkY = node._gridY + y;
				
	      if(checkX >= 0 && checkX < _gridSizeX && checkY >= 0 && checkY < _gridSizeY)
	      {
	        neighbours.Add(_grid[checkX, checkY]);
	      }
	    }
	  }
		
	  return neighbours;
	}
	
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
        bool walkable = !(Physics.CheckSphere(worldPos, _nodeRadius - 0.2f, _unwalkableMask));
        _grid[x, y] = new Node(worldPos, x, y, walkable);
      }
    }
  }
	
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
        if(playerNode == node)
        {
          Gizmos.color = Color.cyan;
        }
        if(_path != null)
        {
          if(_path.Contains(node))
          {
            Gizmos.color = Color.black;
          }
        }
        Gizmos.DrawCube(node._worldPos, Vector3.one * (_nodeDiameter - 0.1f));
      }
    }
  }
}
```

PathFinding.cs

```cs
public class PathFinding : MonoBehaviour
{
  public Transform _player , _target;
	
  MyGrid _grid;
	
  private void Awake()
  {
    _grid = GetComponent<MyGrid>();
  }
	
  private void Update()
  {
    FindPath(_player.position, _target.position);
  }
	
  private void FindPath(Vector3 startPos, Vector3 targetPos)
  {
    Node startNode = _grid.NodeFromWorldPoint(startPos);
    Node targetNode = _grid.NodeFromWorldPoint(targetPos);
		
    List<Node> openSet = new List<Node>();
    HashSet<Node> closedSet = new HashSet<Node>();
		
    openSet.Add(startNode);
		
    while(openSet.Count > 0)
    {
      Node currentNode = openSet[0];
      for(int i = 1; i < openSet.Count; i++)
      {
        if(openSet[i]._fCost <= currentNode._fCost && openSet[i]._hCost < currentNode._hCost)
        {
          currentNode = openSet[i];
        }
      }
			
      openSet.Remove(currentNode);
      closedSet.Add(currentNode);
			
      if(currentNode == targetNode)
      {
        RetracePath(startNode, targetNode);
        return;
      }
			
      foreach(Node neighbour in _grid.GetNeighbours(currentNode))
      {
        if(!neighbour._walkable || closedSet.Contains(neighbour)) continue;
				
        int newMovementCostToNeighbour = currentNode._gCost + GetDistance(currentNode, neighbour);
        if(newMovementCostToNeighbour < neighbour._gCost || !openSet.Contains(neighbour))
        {
	        neighbour._gCost = newMovementCostToNeighbour;
	        neighbour._hCost = GetDistance(neighbour, targetNode);
	        neighbour._parent = currentNode;
					
	        if(!openSet.Contains(neighbour)) openSet.Add(neighbour);
	      }
      }
    }
	}
	
  private void RetracePath(Node startNode, Node endNode)
  {
    List<Node> path = new List<Node>();
    Node currentNode = endNode;
		
    while(currentNode != startNode)
    {
      path.Add(currentNode);
      currentNode = currentNode._parent;
    }
		
    path.Reverse();
		
    _grid._path = path;
  }
	
  private int GetDistance(Node nodeA, Node nodeB)
  {
    int dstX = Mathf.Abs(nodeA._gridX - nodeB._gridX);
    int dstY = Mathf.Abs(nodeA._gridY - nodeB._gridY);
		
    if(dstX > dstY) return 14 * dstY + 10 * (dstX - dstY);
    return 14 * dstX + 10 * (dstY - dstX);
  }
}
```

### 💻 Execute

![2](https://user-images.githubusercontent.com/87271529/188159827-ba5109b0-3dc8-4c8c-9955-ee8e7325a572.gif)


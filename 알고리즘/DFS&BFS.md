## 너비 우선 탐색(Breadth First Search, BFS)와 깊이 우선 탐색(Depth First Search, DFS)란?
+ 대표적인 그래프 탐색 알고리즘
  + DFS : 정점의 자식들을 먼저 탐색하는 방식
    + 한 노드의 자식을 타고 끝까지 순회한 후, 다시 돌아와서 다른 형제들의 자식을 타고 내려가며 순회
  + BFS : 정점들과 같은 레벨에 있는 노드들(형제 노드들)을 먼저 탐색하는 방식
    + 한 단계씩 내려가면서 해당 노드와 같은 레벨에 있는 노드들(형제 노드들)을 먼저 순회

<p align="center"><img src="https://user-images.githubusercontent.com/98029695/190102582-75540ada-70f8-4cf7-ad5e-4a16d1ef3350.png" width="600px"></p>

## 그래프 표현 in Python
+ 파이썬에서 제공하는 딕셔너리와 리스트 자료 구조를 활용
```python
graph = dict()

graph['A'] = ['B', 'C']
graph['B'] = ['A', 'D']
graph['C'] = ['A', 'G', 'H', 'I']
graph['D'] = ['B', 'E', 'F']
graph['E'] = ['D']
graph['F'] = ['D']
graph['G'] = ['C']
graph['H'] = ['C']
graph['I'] = ['C', 'J']
graph['J'] = ['I']
```

<p align="center"><img src="https://user-images.githubusercontent.com/98029695/190103585-dc756c5b-741b-4194-b15d-208947d7ec6b.png" width="600px"></p>

## DFS 알고리즘 구현
+ 자료구조 스택과 큐를 활용
  + need_visit 스택과 visited 큐
  + 스택과 큐 구현은 별도 라이브러리를 활용할 수도 있지만 간단하게 파이썬 리스트를 활용할 수도 있음
> BFS 자료구조는 두 개의 큐를 활용
```python
def dfs(graph, start_node):
  visited, need_visit = list(), list()
  need_visit.append(start_node)
  
  while need_visit:
    node = need_visit.pop()
    if node not in visited:
      visited.append(node)
      need_visit.extend(graph[node])
      
  return visited
```
```python
print(dfs(graph, 'A')) # ['A', 'C', 'I', 'J', 'H', 'G', 'B', 'D', 'F', 'E']
```

## BFS 알고리즘 구현
+ 자료구조 큐를 활용
  + need_visit 큐와 visited 큐

<p align="center"><img src="https://user-images.githubusercontent.com/98029695/190108519-d7f2011a-047b-4ac0-abca-dd8231f2c258.png" width="600px"></p>

+ 큐의 구현은 간단히 파이썬 리스트를 활용
```python
data = [1, 2, 3]
data.extend([4, 5])
print(data) # [1, 2, 3, 4, 5]
```
```python
def bfs(graph, start_node):
  visited, need_visit = list(), list()
  
  need_visit.append(start_node)
  
  while need_visit:
    node = need_visit.pop(0)
    if node not in visited:
      visited.append(node)
      need_visit.extend(graph[node])
      
  return visited
```
```python
print(bfs(graph, 'A')) # ['A', 'B', 'C', 'D', 'G', 'H', 'I', 'E', 'F', 'J']
```

## 시간 복잡도
+ 일반적인 DFS 시간 복잡도
  + 노드 수 : V
  + 간선 수 : E
    + 위 코드에서 while need_visit 을 V + E 번 수행
  + 시간 복잡도 : O(V + E)
+ 일반적인 BFS 시간 복잡도
  + 노드 수 : V
  + 간선 수 : E
    + 위 코드에서 while need_visit 을 V + E 번 수행
  + 시간 복잡도 : O(V + E)

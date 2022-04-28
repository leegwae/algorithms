# Tree Traversals

- **트리 순회(tree traversals)**: `이진 트리`에 대하여, 트리의 노드들을 중복 없이 모두 방문하는 것이다.



## 트리 순회의 종류

트리 순회에는 네가지 방법이 있다.

- 전위 순회(preorder traversal)
- 중위 순회(inorder traversal)
- 후위 순회(postorder traveral)
- 레벨 순회(level traversal)



## 준비

`인접 리스트`로 트리를 구현한다.

```python
tree = {}
tree[node] = [left, right]
```



### 전위 순회

- **전위 순회(preorder traversal)**: `루트->왼쪽 자손->오른쪽 자손`

```python
def preorder(node):
    left, right = tree[node]
    
    print(root)
	preorder(left)
	preorder(right)
```



### 중위 순회

- **중위 순회(inorder traversal)**: `왼쪽 자손-> 루트 -> 오른쪽 자손`

```python
def inorder(node):
    left, right = tree[node]
    
   	inorder(left)
    print(root)
	inorder(right)
```



### 후위 순회

- **후위 순회(postorder traversal)**: `왼쪽 자손-> 오른쪽 자손-> 루트`

```python
def postorder(node):
    left, right = tree[node]
    
   	postorder(left)
    print(root)
	postorder(right)
```



### 레벨 순회

- **레벨 순회(level traversal)**: 위에서부터 같은 레벨의 모든 노드를 왼쪽에서 오른쪽으로 방문한다. [BFS](https://github.com/leegwae/algorithms/blob/main/BFS.md)와 같다.

```python
def level_traversal(node):
    queue = deque([node])
    
    while queue:
		cur = queue.popleft()
        
        left, right = tree[cur]
        queue.append(left)
        queue.append(right)
```



[예: 트리 순회](https://github.com/leegwae/problem-solving/blob/main/tree/traversal/트리_순회.py)
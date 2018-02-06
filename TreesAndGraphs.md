# Trees and Graphs
## Trees
### Binary tree
```python
class Node(object):
    def __init__(self, data):
        self.data = data
        self.left = None
        self.right = None

    def add_child(self, child):
        if self.left is None:
            self.left = child
        else:
            self.right = child
```
### N-ary tree
```python
class Node(object):
    def __init__(self, data):
        self.data = data
        self.children = []

    def add_child(self, child):
        self.children.append(child)
```
### Trie-tree
```python
class Trie:
    def __init__(self):
        self.head = Node()
    
    def __getitem__(self, key):
        return self.head.children[key]
    
    def add(self, word):
        current_node = self.head
        word_finished = True
        
        for i in range(len(word)):
            if word[i] in current_node.children:
                current_node = current_node.children[word[i]]
            else:
                word_finished = False
                break
        
        # For ever new letter, create a new child node
        if not word_finished:
            while i < len(word):
                current_node.addChild(word[i])
                current_node = current_node.children[word[i]]
                i += 1
        
        # Let's store the full word at the end node so we don't need to
        # travel back up the tree to reconstruct the word
        current_node.data = word
```
## Graph representation
### Objects and pointers
This approach is commonly used for object oriented implementations, since it is more readable and convenient for object oriented users.
```python
a = Vertex(1)
b = Vertex(2)
edge = Edge(a,b, 30) # init an edge between ab and be with weight 30 
```
### Adjacency matrix
A matrix is just a simple 2 dimensional array. Assuming you have vertex ID's that can be represented as an int array like this:
```python
m, n = 8, 5
Matrix = [[0 for j in range(m)] for i in range(n)]
Matrix[0][1] = 30 # sets the weight of a vertex 0 that is adjacent to vertex 1
```
This is commonly used for **dense graphs** where **index access** is necessary. You can represent a un/directed and weighted structure with this.
- Storage size - `O(|V|^2)`


- Adding a Vertex – `O(|V|^2)`
- Adding an edge – `O(1)`
- Deleting a Vertex – `O(|V|^2)`
- Deleting an edge – `O(1)`
- Answering the question “is there an edge between i and j” – `O(1)`
- Finding the successors of a given vertex – `O(n)`
- Finding (if exists) a path between two vertices – `O(n^2)`

### Adjacency list
This is a list of lists, where each item in the list contains a list of adjacent nodes.
```python
a = [[] for i in range(n)] # n vertices
a[0].append(1) # vertex 1 is adjacent to 0
```
This is used for representing **sparse graphs**. For Google, you should know that the webgraph is sparse. You can deal with them in a more scalable way using a BigTable.
- Storage size - `O(|V|+|E|)`


- Adding a Vertex – `O(1)`
- Adding an edge – `O(1)`
- Deleting a Vertex – `O(|V|+|E|)`
- Deleting an edge – `O(|E|)`
- Answering the question “is there an edge between i and j” – `O(|V|)`
- Finding the successors of a given vertex – `O(k)`, where “k” is the length of the lists containing the successors of i
- Finding (if exists) a path between two vertices – `O(n+m)` – where `m <= n`
## Tree traversal

### Pre order
The root is always the first node visited.
```python
def preorder(tree):
    if tree:
        print(tree.getRootVal())
        preorder(tree.getLeftChild())
        preorder(tree.getRightChild())
```
### In order
When performed on a binary search tree, it visits the nodes in ascending order (hence the name "in-order").
```python
def inorder(tree):
  if tree != None:
      inorder(tree.getLeftChild())
      print(tree.getRootVal())
      inorder(tree.getRightChild())
```
### Post order
The root is always the last node visited.
```python
def postorder(tree):
    if tree != None:
        postorder(tree.getLeftChild())
        postorder(tree.getRightChild())
        print(tree.getRootVal())
```
### BFS
Calculate the shortest distance from the  starting node to all the other nodes in the graph.
```python
import queue # for BFS
from collections import defaultdict # for graph
        
class Graph:
    def __init__(self, n):
        self.n = n
        self.edges = defaultdict(lambda: [])
        
    def connect(self, x, y):
        self.edges[x].append(y)
        self.edges[y].append(x)
        
    def find_all_distances(self, s):
        dist = [-1 for i in range(self.n)]
        unvisited = set([i for i in range(self.n)])
        q = queue.Queue()
        
        dist[s] = 0
        unvisited.remove(s)
        q.put(s)
        
        weight = 6
                
        while not q.empty():
            node = q.get()
            
            for child in self.edges[node]:
                if child in unvisited:
                    dist[child] = dist[node] + weight
                    q.put(child)
                    unvisited.remove(child)
        dist.pop(s)
        
        print(" ".join(map(str,dist)))
```
### DFS
Given an `N x M` matrix, find and print the number of cells in the largest region in the matrix. Note that there may be more than one region in the matrix.
```
1 1 0 0
0 1 1 0
0 0 1 0
1 0 0 0
```
```python
def getBiggestRegion(grid):
    n = len(grid)
    m = len(grid[0])
    
    maxRegion = 0
    
    for i in range(n):
        for j in range(m):
            maxRegion = max(maxRegion, countCells(grid,i,j))
            
    return maxRegion
    
def countCells(grid,i,j):
    count = 0
    if not(i in range(n)) or not(j in range(m)):
        return 0
    if grid[i][j] == 0:
        return 0
    else:
        grid[i][j] = 0
        
        count +=1
        
        for k in range(-1,2):
            for l in range(-1,2):
                count += countCells(grid,i+k,j+l)
        
        return count
```

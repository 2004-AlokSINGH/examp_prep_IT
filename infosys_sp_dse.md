# 🎯 INFOSYS DSE/SP ROUND 2 - LATEST CODING QUESTIONS (2025-2026)

## Assessment Format
- **Duration**: 60-90 minutes
- **Problems**: 2-3 medium difficulty problems
- **Languages**: Python, Java, C++, C
- **Scoring**: 40% Correctness, 30% Efficiency, 20% Code Quality, 10% Edge Cases

## RECENT QUESTIONS (Latest from Glassdoor & GeeksforGeeks)
Here's a **short tricks summary** of the key graph concepts in your notes:

## **Graph Algorithms - Quick Tricks**

### **Cycle Detection**
- **Undirected**: During DFS, if visited node ≠ parent → cycle found
- **Directed**: Track `inRecursion` array. If visited AND in recursion → cycle found

### **Bipartite & Coloring**
- **Trick**: Use 2-coloring (0/1). If adjacent nodes have same color → not bipartite
- **Reason**: Bipartite = no odd-length cycles

### **Topological Sort**
- **Only works on DAG** (no cycles)
- **Kahn's Algo**: Find all nodes with indegree=0, process them, reduce neighbors' indegree
- **Return order**: Process all V nodes before returning

### **Shortest Paths**
- **Dijkstra**: Skip if `cost > ans[node]` (already found shorter path)
- **Bellman-Ford**: Relax edges exactly V-1 times; detects negative cycles
- **Floyd-Warshall**: Check all intermediate nodes k; O(V³)

### **MST (Minimum Spanning Tree)**
- **Prim's**: Use min-heap, grow tree incrementally
- **Kruskal's**: Sort edges, use DSU to detect cycles; add if union succeeds

### **Tree Problems**
- **LCA**: If left≠null AND right≠null → current node is LCA
- **Diameter**: `left + right` (edges) or `left + right + 1` (nodes)
- **Distance Sum**: Use **re-rooting** (DFS twice) - O(n) instead of O(n²)
- **Spread/Infection**: Convert tree to undirected graph, BFS from source

### **DSU (Disjoint Set Union)**
- Always do **path compression** in `find()`
- Check `find(x) == find(y)` before union to detect cycles

**Key Pattern**: Tree problems often need **DFS + post-order** to aggregate results from children.



Codes:-
Graph
Undirected Graph Cycle | Practice | GeeksforGeeks​
Idea - while traversing if any node that is visited and not parent ⇒ cycle and if not visted check the same thing again.

```py
class Solution:
    def isCycle(self, V, edges):
        
        vis = set()
        adj = {}

        # Build adjacency list correctly
        for i, j in edges:
            adj.setdefault(i, []).append(j)
            adj.setdefault(j, []).append(i)

        def dfsCycle(st, parent):
            vis.add(st)
            for ng in adj.get(st, []):
                if ng in vis and ng!=parent:
                    return True
                if ng not in vis and dfsCycle(ng, st):
                    return True
            return False
            
        for i in range(V):
            if i not in vis:
                if dfsCycle(i, -1):
                    return True

        return False
        
        
        
        
        
from collections import deque
class Solution:
    def isCycle(self, V, edges):
        
        vis = set()
        adj = {}

        # Build adjacency list
        for i, j in edges:
            adj.setdefault(i, []).append(j)
            adj.setdefault(j, []).append(i)

        def bfsCycle(st):
            q = deque()
            q.append((st, -1))
            vis.add(st)

            while q:
                node, par = q.popleft()
                
                for ng in adj.get(node, []):
                    if ng not in vis:
                        vis.add(ng)
                        q.append((ng, node))
                    elif ng != par:
                        return True
            return False

        for i in range(V):
            if i not in vis:
                if bfsCycle(i):
                    return True

        return False   
```   
        
👉 Time Complexity:  O(V + E)
👉 Space Complexity: O(V + E)

In an undirected graph, each edge is explored twice, so actual work is V + 2E, 
but constants are ignored in Big-O, so complexity is O(V + E).”

---
​
Deque in python
```py
from collections import deque
q=deque()
​
Action
Left Side (Start)
Right Side (End)
Remove
popleft()
pop()
Add
appendleft()
append()
```
Directed Graph Cycle | Practice | GeeksforGeeks​
Idea - in directed graph while traversing if got any node visited then also check it is in current recursion or not?

```py
class Solution:
    def dfsCycle(self,st,parent):
        self.vis[st]=True
        self.inrec[st]=True
        
        for ng in self.adj.get(st,[]):
            if self.vis[ng]==True:
                if self.inrec[ng]==True:
                    return True
            else:
                if self.dfsCycle(ng,st):
                    return True
        self.inrec[st]=False
            
            
      
        
    def isCyclic(self, V, edges):
        # code here
        self.vis=[False]*V
        self.inrec=[False]*V
        self.adj={}
        for i,j in edges:
            self.adj.setdefault(i,[]).append(j)
            
    
        for i in range(V):
            if self.vis[i]==False:
                if self.dfsCycle(i,-1):
                    return True
        return False

```
​
Topological Sort:
Idea  → in directed Acyclic graph only DAG. If cycle → toplogical sort not possible.
Linear Order: It organizes nodes in a sequence such that all edges point from left to right.
So node with indegree 0 comes first.
Kahn’s Algo

```py
from collections import deque

class Solution:
    
    def topSortKahn(self):
        ans = []
        q = deque()        
        indg = self.indg[:]  
        for i in range(len(indg)):
            if indg[i] == 0:
                q.append(i)
        
        while q:
            curr = q.popleft()
            ans.append(curr)
            
            for ng in self.adj.get(curr, []):
                indg[ng] -= 1
                if indg[ng] == 0:
                    q.append(ng)        
        return ans
        
        
    def topoSort(self, V, edges):
        
        self.adj = {}
        self.indg = [0] * V
        
        for i, j in edges:
            self.adj.setdefault(i, []).append(j)
            self.indg[j] += 1
        
        arr = self.topSortKahn()
        return arr
​



from collections import deque

class Solution:
    
    def topSortdfs(self, st):
        self.vis[st] = True
        
        for ng in self.adj.get(st, []):
            if not self.vis[ng]:
                self.topSortdfs(ng)
        
        self.stk.append(st)
        
        
    def topoSort(self, V, edges):
        
        self.adj = {}
        for i, j in edges:
            self.adj.setdefault(i, []).append(j)
        
        self.vis = [False] * V
        self.stk = []
        
        for i in range(V):
            if not self.vis[i]:
                self.topSortdfs(i)
                
        return self.stk[::-1]

```




count of provinces

Idea - in one dfs flow all completed.
```py
class Solution:
    def findCircleNum(self, isConnected: List[List[int]]) -> int:
        def dfs(st,vis):
            vis.add(st)

            for i in (adj.get(st,[])):
                if i not in vis:
                    dfs(i,vis)
        
        vis=set()
        adj={}
        n=len(isConnected)
        for i in range(n):
            for k in range(n):
                if i==k:
                    continue
                if isConnected[i][k]==1:
                    adj.setdefault(i+1,[]).append(k+1)
        print(adj)
        cnt=0
        for i in range(1,n+1):
            if i not in vis:
                cnt+=1
                dfs(i,vis)
        return cnt


        


```


numcourse:--
https://leetcode.com/problems/course-schedule/description/


```py
class Solution(object):
    def canFinish(self, numCourses, prerequisites):
        """
        :type numCourses: int
        :type prerequisites: List[List[int]]
        :rtype: bool
        """
        adj={}
        indg=[0]*numCourses
        q=deque()
        for i,j in prerequisites:
            adj.setdefault(i,[]).append(j)
            indg[j]+=1
        
        for i in range(numCourses):
            if indg[i]==0:
                q.append(i)
        cnt=0
        while q:
            curr=q.popleft()
            cnt+=1
            for ng in adj.get(curr,[]):
                indg[ng]-=1
                if indg[ng]==0:
                    q.append(ng)    


        print(cnt)
        return cnt==numCourses
```



Is Graph Bipartite ?

Idea - odd length cycle then cannot.
```py
class Solution(object):
    def isBipartite(self, graph):
        """
        :type graph: List[List[int]]
        :rtype: bool
        """
        def dfsbipart(st,adj,currColor,color):
            color[st]=currColor

            for i in adj.get(st,[]):
                if color[i]==color[st]:
                    return False
                elif color[i]==-1:
                    colNow=1-currColor
                    if dfsbipart(i,adj,colNow,color)==False:
                        return False
            return True
        adj={}
        for i in range(len(graph)):
            adj[i]=graph[i]
        color=[-1]*len(graph)
        for i in range(len(graph)):
            if color[i]==-1:
                if dfsbipart(i,adj,0,color)==False:
                    return False
        return True

```


dsu---

```py
class Solution(object):
    def countPairs(self, n, edges):
        """
        :type n: int
        :type edges: List[List[int]]
        :rtype: int
        """
        adj = {i: [] for i in range(n)}
        for u, v in edges:
            adj[u].append(v)
            adj[v].append(u)
        
        visited = set()

        def dfs(st,vis,size):
            vis.add(st)
            size=1

            for i in adj.get(st,[]):
                if i not in vis:
                    size+=dfs(i,vis,size)
            
            return size
            
        ans=0
        totalnode=n
        for i in range(n):
            if i not in visited:
                dfscomp=dfs(i,visited,1)
                ans= ans + ((dfscomp)*(totalnode-dfscomp))
                totalnode -=dfscomp
        return ans

        # soln 2--
        
        rank=[0]*n
        parent=[-1]*n
        for i in range(n):
            parent[i]=i
        def find(x):
            if x==parent[x]:
                return x
            else:
                parent[x]=find(parent[x])
                return parent[x]
               
        def union(x,y):
            parx=find(x)
            pary=find(y)
            if parx==pary:
                return 
            if rank[parx]>rank[pary]:
                parent[pary]=parx
            elif rank[pary]>rank[parx]:
                parent[parx]=pary
            else:
                parent[parx]=pary
                rank[pary]+=1
        
        for i,j in edges:
            union(i,j)
            
        mpcnt={}
        for i in parent:
            root=find(i)
            mpcnt[root]=mpcnt.get(root,0)+1
        print(mpcnt)
        arr=mpcnt.values()
        ans=0
        totalnode=n
        for i in arr:
            ans= ans + ((i)*(totalnode-i))
            totalnode -=i
        return ans
```



---
## 🔥 SHORTEST PATH ALGORITHMS - COMPREHENSIVE GUIDE

### 1️⃣ DIJKSTRA'S ALGORITHM
**Use When**: Non-negative weights, single source shortest path to all nodes

**Idea**: Greedily pick unvisited node with smallest distance, update neighbors

**Key Trick**: `if cost > ans[node]: continue` - Skip if we already found a better path

```py
import heapq
from collections import defaultdict

class Solution:
    def dijkstra(self, V, edges, src):
        """
        Args:
            V: number of vertices
            edges: list of [u, v, weight]
            src: source node
        Returns:
            list of shortest distances from src to all nodes
        """
        # Build adjacency list
        adj = defaultdict(list)
        for u, v, wt in edges:
            adj[u].append((v, wt))
            adj[v].append((u, wt))  # Remove if directed
        
        # Initialize distances
        dist = [float('inf')] * V
        dist[src] = 0
        
        # Min heap: [distance, node]
        pq = [(0, src)]
        
        while pq:
            curr_dist, node = heapq.heappop(pq)
            
            # 🔥 CRITICAL: Skip if we already found better path
            if curr_dist > dist[node]:
                continue
            
            # Update neighbors
            for neighbor, weight in adj.get(node, []):
                new_dist = curr_dist + weight
                if new_dist < dist[neighbor]:
                    dist[neighbor] = new_dist
                    heapq.heappush(pq, (new_dist, neighbor))
        
        return dist

# EXAMPLE:
# edges = [[0,1,4], [0,2,1], [2,1,2], [1,3,1], [2,3,5]]
# src = 0
# Output: [0, 3, 1, 4]  (distances to nodes 0,1,2,3)

# ⏱️ Complexity: O(E log V)
# 🧠 Space: O(V + E)
```

**Tricks for Dijkstra:**
- Use `defaultdict(list)` to avoid KeyError
- Always check `if cost > ans[node]` to optimize
- For dense graphs: O(V²) with array instead of heap
- Track parent nodes to reconstruct path

**Reconstruct Shortest Path:**
```py
def dijkstra_with_path(V, edges, src, dest):
    adj = defaultdict(list)
    for u, v, wt in edges:
        adj[u].append((v, wt))
        adj[v].append((u, wt))
    
    dist = [float('inf')] * V
    parent = [-1] * V
    dist[src] = 0
    pq = [(0, src)]
    
    while pq:
        d, u = heapq.heappop(pq)
        if d > dist[u]:
            continue
        for v, w in adj[u]:
            if dist[u] + w < dist[v]:
                dist[v] = dist[u] + w
                parent[v] = u
                heapq.heappush(pq, (dist[v], v))
    
    # Reconstruct path
    path = []
    node = dest
    while node != -1:
        path.append(node)
        node = parent[node]
    path.reverse()
    return path, dist[dest]
```

---

### 2️⃣ BELLMAN-FORD ALGORITHM
**Use When**: 
- Negative edge weights allowed
- Need to detect negative cycles
- Single source shortest path

**Idea**: Relax all edges exactly V-1 times. If can relax again → negative cycle

```py
class Solution:
    def bellmanFord(self, V, edges, src):
        """
        Args:
            V: number of vertices
            edges: list of [u, v, weight]
            src: source node
        Returns:
            distances array or [-1] if negative cycle exists
        """
        # Initialize distances with large value
        INF = float('inf')
        dist = [INF] * V
        dist[src] = 0
        
        # Relax edges V-1 times
        # Why V-1? Shortest path can have at most V-1 edges
        for _ in range(V - 1):
            for u, v, wt in edges:
                # Only relax if source is reachable
                if dist[u] != INF and dist[u] + wt < dist[v]:
                    dist[v] = dist[u] + wt
        
        # Check for negative cycle
        # If we can still relax, negative cycle exists
        for u, v, wt in edges:
            if dist[u] != INF and dist[u] + wt < dist[v]:
                return [-1]  # Negative cycle detected
        
        return dist

# EXAMPLE:
# V = 5, src = 0
# edges = [[0,1,-1], [0,2,4], [1,2,3], [1,3,2], [1,4,2], [3,2,5], [3,1,1], [4,3,-3]]
# Output: [0, -1, 2, -2, 1]

# ⏱️ Complexity: O(V * E)
# 🧠 Space: O(V)
```

**When to use Bellman-Ford over Dijkstra:**
✅ Graph has negative edge weights
✅ Need to detect negative cycles
❌ Performance critical (use Dijkstra for positive weights)

**Detect Negative Cycle + Get Nodes:**
```py
def findNegativeCycle(V, edges):
    dist = [float('inf')] * V
    dist[0] = 0
    
    # Relax V-1 times
    for _ in range(V - 1):
        for u, v, w in edges:
            if dist[u] != float('inf') and dist[u] + w < dist[v]:
                dist[v] = dist[u] + w
    
    # Find nodes affected by negative cycle
    affected = set()
    for u, v, w in edges:
        if dist[u] != float('inf') and dist[u] + w < dist[v]:
            affected.add(v)
    
    # Propagate affected nodes
    for _ in range(V):
        temp = set(affected)
        for u, v, w in edges:
            if u in affected:
                affected.add(v)
    
    return list(affected)
```

---

### 3️⃣ FLOYD-WARSHALL ALGORITHM
**Use When**: 
- All-pairs shortest path (every node to every other node)
- Graph size is small (V ≤ 500)
- Simple implementation needed

**Idea**: Try all intermediate nodes k. Update dist[i][j] via k if shorter

```py
def floyd_warshall(V, edges):
    """
    Args:
        V: number of vertices
        edges: adjacency matrix (INF for no edge)
    Returns:
        distance matrix with all-pairs shortest paths
    """
    INF = float('inf')
    
    # Initialize distance matrix
    dist = [[INF] * V for _ in range(V)]
    for i in range(V):
        dist[i][i] = 0  # Distance from node to itself
    
    # Fill edges
    for u, v, w in edges:
        dist[u][v] = min(dist[u][v], w)  # Handle multiple edges
        # Remove next line if directed graph
        dist[v][u] = min(dist[v][u], w)
    
    # Try all intermediate nodes k
    for k in range(V):
        for i in range(V):
            for j in range(V):
                # Path via k might be shorter
                if dist[i][k] != INF and dist[k][j] != INF:
                    dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j])
    
    return dist

# EXAMPLE:
# V = 4
# edges = [[0,1,4], [0,2,2], [1,2,1], [1,3,5], [2,3,8], [3,2,10]]
# 
# Initial:     After k=0:  After k=1:  After k=2:  After k=3:
# 0 4 2 ∞      0 4 2 ∞     0 4 2 9     0 4 2 9     0 4 2 9
# ∞ 0 1 5      ∞ 0 1 5     ∞ 0 1 5     ∞ 0 1 5     ∞ 0 1 5
# ∞ ∞ 0 8      ∞ ∞ 0 8     ∞ ∞ 0 8     ∞ ∞ 0 8     ∞ ∞ 0 8
# ∞ ∞ 10 0     ∞ ∞ 10 0    ∞ ∞ 10 0    ∞ ∞ 10 0    ∞ ∞ 10 0

# ⏱️ Complexity: O(V³)
# 🧠 Space: O(V²)
```

**Floyd-Warshall with Path Reconstruction:**
```py
def floyd_warshall_with_path(V, dist_matrix):
    INF = float('inf')
    dist = [row[:] for row in dist_matrix]
    next_node = [[-1] * V for _ in range(V)]
    
    # Initialize next_node
    for i in range(V):
        for j in range(V):
            if i != j and dist[i][j] != INF:
                next_node[i][j] = j
    
    # Floyd-Warshall
    for k in range(V):
        for i in range(V):
            for j in range(V):
                if dist[i][k] + dist[k][j] < dist[i][j]:
                    dist[i][j] = dist[i][k] + dist[k][j]
                    next_node[i][j] = next_node[i][k]
    
    # Reconstruct path
    def reconstruct_path(i, j):
        if next_node[i][j] == -1:
            return []
        path = [i]
        while i != j:
            i = next_node[i][j]
            path.append(i)
        return path
    
    return dist, reconstruct_path
```

---

## 📊 COMPARISON TABLE

| Algorithm | Use Case | Negative Weights | Time | Space | All-Pairs |
|-----------|----------|------------------|------|-------|-----------|
| **Dijkstra** | Single source, non-negative | ❌ | O(E log V) | O(V+E) | ❌ |
| **Bellman-Ford** | Single source, negative edges | ✅ | O(V*E) | O(V) | ❌ |
| **Floyd-Warshall** | All pairs, any weights | ✅ | O(V³) | O(V²) | ✅ |

---

## 🎯 COMMON EDGE CASES & TRICKS

```py
# 1. Disconnected nodes
if dist[node] == float('inf'):
    print(f"Node {node} unreachable from source")

# 2. Self-loops (should be 0)
for i in range(V):
    dist[i][i] = 0

# 3. Multiple edges between same nodes
dist[u][v] = min(dist[u][v], weight)

# 4. Very large weights (avoid overflow)
INF = 10**9  # Instead of float('inf') in some cases

# 5. Negative cycle detection
if any(dist[i][i] < 0 for i in range(V)):
    print("Negative cycle exists")

# 6. Path reconstruction edge case
if dist[dest] == float('inf'):
    return "No path exists"
```

---

## 🔧 PRACTICE PROBLEM PATTERNS

| Problem | Algorithm | Trick |
|---------|-----------|-------|
| Network Delay Time | Dijkstra | Max distance |
| Cheapest Flights K Stops | Dijkstra/BFS | Limit edges |
| Reachable Nodes | Floyd-Warshall | Check if dist < INF |
| Detect Cycle | Bellman-Ford | Negative cycle |
| Minimum Path Sum | Dijkstra | Grid DP combo |



---
MST = all node connected with minimm wieghttt.
```py
class Solution:
    def spanningTree(self, V, edges):
        # code here
        parents={}
        pq=[]
        heapq.heappush(pq,(0,0,-1))
        adj={}
        for i,j,k in edges:
            adj.setdefault(i,[]).append((j,k))
            adj.setdefault(j,[]).append((i,k))
        vis=set()
        sm=0
        
        while pq:
            wt,node,parent=heapq.heappop(pq)
            if node not in vis:
                sm+=wt
                parents[node]=parent
                vis.add(node)
                for ng,wt in adj.get(node,[]):
                    heapq.heappush(pq,(wt,ng,node))
        # print(parents)
        return sm
                    
            
```
---

krushkals

- Below are the steps for finding MST using Kruskal's algorithm:

- Sort all the edges in a non-decreasing order of their weight. 
- Pick the smallest edge. Check if it forms a cycle with the spanning tree formed so far. If the cycle is not formed, include this edge. Else, discard it.  It uses the Disjoint Sets to detect cycles.
- Repeat step 2 until there are (V-1) edges in the spanning tree.

```py
from typing import List

class Solution:
    def kruskalsMST(self, V: int, edges: List[List[int]]) -> int:
        # Initialize parent and rank arrays
        parent = [i for i in range(V)]
        rank = [0] * V

        def find(x):
            if parent[x] != x:
                parent[x] = find(parent[x])  # Path compression
            return parent[x]

        def union(x, y):
            par_x = find(x)
            par_y = find(y)
            if par_x == par_y:
                return True  
            if rank[par_x] > rank[par_y]:
                parent[par_y] = par_x
            elif rank[par_x] < rank[par_y]:
                parent[par_x] = par_y
            else:
                parent[par_y] = par_x
                rank[par_x] += 1
            return False

        # Sort edges by weight
        edges.sort(key=lambda x: x[2])
        mst_weight = 0

        # Kruskal’s algorithm
        for u, v, w in edges:
            if union(u, v)==False:
                mst_weight += w

        return mst_weight
```












---------------------------------------

tree level order

```py
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution(object):
    def zigzagLevelOrder(self, root):
        """
        :type root: Optional[TreeNode]
        :rtype: List[List[int]]
        """
        def levelOrder(root):
            if not root:
                return []
            q=deque()
            q.append(root)
            ans=[]
            flg=True
            while q:
                n=len(q)
                curlvl=deque()
                for i in range(n):
                    curr=q.popleft()
                    if flg:
                        curlvl.append(curr.val)     
                    else:
                        curlvl.appendleft(curr.val)               
                    
                    if curr.left!=None:
                        q.append(curr.left) 
                    if curr.right!=None:
                        q.append(curr.right) 
                if len(curlvl)>0:
                    ans.append(list(curlvl))
                    curlvl=[]
                    flg=not flg
                
            return ans
        
        return levelOrder(root)
```

```py
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution(object):
    def buildTree(self, preorder, inorder):
        """
        :type preorder: List[int]
        :type inorder: List[int]
        :rtype: Optional[TreeNode]
        """
        self.idx=0
        def solve(pre,ino,start,end):
            if start>end:
                return None
            for i in range(start,end):
                if ino[i]==pre[self.idx]:
                    break
            root=TreeNode(pre[self.idx])
            self.idx+=1
            
            root.left=solve(pre,ino,start,i-1)
            root.right=solve(pre,ino,i+1,end)
            return root
        return solve(preorder,inorder,0,len(preorder)-1)
```


build

```py
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution(object):
    def buildTree(self, preorder, inorder):
        """
        :type preorder: List[int]
        :type inorder: List[int]
        :rtype: Optional[TreeNode]
        """
        self.idx = 0

        def solve(pre, ino, start, end):
            if start > end:
                return None
            # find root in inorder
            for i in range(start, end + 1):
                if ino[i] == pre[self.idx]:
                    break
            root = TreeNode(pre[self.idx])
            self.idx += 1

            # use inorder index `i` to split left and right
            root.left = solve(pre, ino, start, i - 1)
            root.right = solve(pre, ino, i + 1, end)
            return root

        return solve(preorder, inorder, 0, len(inorder) - 1)







        # lca

# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def lowestCommonAncestor(self, root, p, q):
        """
        :type root: TreeNode
        :type p: TreeNode
        :type q: TreeNode
        :rtype: TreeNode
        """

        # idea : if any left and right both get then lca = root
        # if left got then left else right
        if not root:
            return None
        if root==p or root==q:
            return root
        leftN=self.lowestCommonAncestor(root.left, p, q)
        rightN=self.lowestCommonAncestor(root.right, p, q)
        if leftN!=None and rightN!=None:
            return root

        if leftN!=None:
            return leftN
        else:
            return rightN
    
        
```


---

Input: root = [1,5,3,null,4,10,6,9,2], start = 3
Output: 4
Explanation: The following nodes are infected during:
- Minute 0: Node 3
- Minute 1: Nodes 1, 10 and 6
- Minute 2: Node 5
- Minute 3: Node 4
- Minute 4: Nodes 9 and 2
It takes 4 minutes for the whole tree to be infected so we return 4.

```py
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution(object):
    def amountOfTime(self, root, start):
        """
        :type root: Optional[TreeNode]
        :type start: int
        :rtype: int
        """
        
        adj = {}

        def build_graph(root):
            if not root:
                return
            
            q = [root]
            
            while q:
                curr = q.pop(0)                
                # ensure key exists
                if curr.val not in adj:
                    adj[curr.val] = []
                # left child
                if curr.left:
                    adj[curr.val].append(curr.left.val)    
                    if curr.left.val not in adj:
                        adj[curr.left.val] = []
                    adj[curr.left.val].append(curr.val)
                    
                    q.append(curr.left)
                
                # right child
                if curr.right:
                    adj[curr.val].append(curr.right.val)
                    
                    if curr.right.val not in adj:
                        adj[curr.right.val] = []
                    adj[curr.right.val].append(curr.val)
                    
                    q.append(curr.right)

        build_graph(root)
        print(adj)

        def get_time(root):
            vis=set()
            vis.add(root)
            q=[root]
            k=0
            while q:
                n=len(q)
                for _ in range(len(q)):
                    curr=q.pop(0)
                    ngbr=adj.get(curr)
                    for ng in ngbr:
                        if ng and ng not in vis:
                            vis.add(ng)
                            q.append(ng)
                k+=1
            return k-1
        return get_time(start)
```

---
string compress
```py
def solve(s, D):
    n = len(s)
    memo = {}

    # encoding length
    def enc_len(x):
        if x == 1:
            return 1
        return 1 + len(str(x))

    def dfs(i, k):
        # base case
        if i == n:
            return 0

        if (i, k) in memo:
            return memo[(i, k)]

        res = float('inf')

        # Option 1: delete current character
        if k > 0:
            res = dfs(i + 1, k - 1)

        # Option 2: form groups
        freq = [0] * 26
        maxf = 0

        for j in range(i, n):
            idx = ord(s[j]) - ord('a')
            freq[idx] += 1
            maxf = max(maxf, freq[idx])

            deletions = (j - i + 1) - maxf

            if deletions > k:
                continue

            res = min(
                res,
                enc_len(maxf) + dfs(j + 1, k - deletions)
            )

        memo[(i, k)] = res
        return res

    # compute max score
    ans = 0
    for k in range(n + 1):
        compressed = dfs(0, k)
        ans = max(ans, n - compressed - k * D)

    return ans

```
---
distancs sum from all node

```py
def dfs(st, vis, depth):
    total = depth
    for ng in d.get(st, []):
        if ng not in vis:
            vis.add(ng)
            total += dfs(ng, vis, depth + 1)
    return total

```
---

depth and count child
```py
 def dfsDepthAndCount(st,vis,dpth,parent):
            cldcnt=1
            total_d=dpth  
            for ng in d.get(st,[]):
                if ng !=parent:
                    vis.add(ng)
                    sub_d,sub_child=dfsDepthAndCount(ng,vis,dpth+1,st)
                    cldcnt  +=  sub_child
                    total_d +=  sub_d
            child[st]=cldcnt
            return total_d,cldcnt

     
```
---

```py
sum of depth of tree.

Input: n = 6, edges = [[0,1],[0,2],[2,3],[2,4],[2,5]]
Output: [8,12,6,10,10,10]
Explanation: The tree is shown above.
We can see that dist(0,1) + dist(0,2) + dist(0,3) + dist(0,4) + dist(0,5)
equals 1 + 1 + 2 + 2 + 2 = 8.
Hence, answer[0] = 8, and so on.

class Solution(object):
    def sumOfDistancesInTree(self, n, edges):
        """
        :type n: int
        :type edges: List[List[int]]
        :rtype: List[int]
        """
        d={}
        for i,j in edges:
            d.setdefault(i,[]).append(j)
            d.setdefault(j,[]).append(i)
        
        def dfs(st,vis,dpth):
            total_d=dpth  
            for ng in d.get(st,[]):
                if ng not in vis:
                    vis.add(ng)
                    total_d+=dfs(ng,vis,dpth+1)
            return total_d
            
        # ans=[]
        # for i in range(n):
        #     vs=set()
        #     vs.add(i)
        #     ans.append(dfs(i,vs,0))
        # print(ans)
        # return ans


        def dfsDepthAndCount(st,vis,dpth,parent):
            cldcnt=1
            total_d=dpth  
            for ng in d.get(st,[]):
                if ng !=parent:
                    vis.add(ng)
                    sub_d,sub_child=dfsDepthAndCount(ng,vis,dpth+1,st)
                    cldcnt  +=  sub_child
                    total_d +=  sub_d
            child[st]=cldcnt
            return total_d,cldcnt
        
        child={}
        k,_=dfsDepthAndCount(0,set([0]),0,-1)
        print(child,k)

        ans=[0]*n
        ans[0]=k
        def fillans(node,par):
            for ng in d.get(node,[]):
                if ng!=par:
                    currans=ans[node]-(child[ng]) + (n-child[ng])
                    ans[ng]=currans
                    fillans(ng,node)
        fillans(0,-1)
        return ans


```
---

count of time to collect all apple
```py
class Solution(object):
    def minTime(self, n, edges, hasApple):
        """
        :type n: int
        :type edges: List[List[int]]
        :type hasApple: List[bool]
        :rtype: int
        """
        adj={}
        for i,j in edges:
            adj.setdefault(i,[]).append(j)
            adj.setdefault(j,[]).append(i)
        def solve(st,parent):
            totaltm=0

            for ng in adj.get(st,[]):
                if ng!=parent:
                    ansfromchild=solve(ng,st)
                    if ansfromchild or hasApple[ng]:
                        totaltm+= (ansfromchild+2)
            return totaltm
               
        return solve(0,-1)
```py

---
diameter of tree
dia = take best or left + right 
but need to return on left or right + 1 (as current edge also need to be counted)

but this diamtr is edge based if need node based the 
but need to return on left or right + 1 (as current edge also need to be counted) and 
update dia= best or left+right+1(as current node also need to be counted)

```py

      root
     /    \
   (3)    (3)
   dia = 6 if edge base
   but nodes cnt= 7

def dfs(node):
    if not node:
        return 0
    
    left = dfs(node.left)
    right = dfs(node.right)
    
    # 🔥 update global answer (same as your res)
    self.res = max(self.res, left + right)
    
    # return longest single path
    return max(left, right) + 1
```


```py
def solve(st, parent):
    first_max = 0
    second_max = 0
    
    for child in adj.get(st, []):
        if child == parent:
            continue
        ans_from_child = solve(child, st)
        
        if s[child] == s[st]:
            continue
        
        if ans_from_child > first_max:
            second_max = first_max
            first_max = ans_from_child
        elif ans_from_child > second_max:
            second_max = ans_from_child

    # update result AFTER loop
    self.res = max(self.res, first_max + second_max + 1)

    # return longest single path
    return first_max + 1
```






---

longest cycle in graph
```py
class Solution(object):
    def longestCycle(self, edges):
        """
        :type edges: List[int]
        :rtype: int
        """
        self.ans=0
        def detectCycle(curr,vis,inrec,cnt):
            if curr!=-1:
                vis[curr]=True
                inrec[curr]=True

                ngbr=edges[curr]
                if ngbr!=-1:
                    if vis[ngbr]==False:
                        cnt[ngbr]=cnt[curr]+1
                        detectCycle(ngbr,vis,inrec,cnt)
                    if inrec[ngbr]:
                        self.ans=max(self.ans,abs(cnt[ngbr]-cnt[curr])+1)
                inrec[curr]=False
        n=len(edges)
        inrec=[False]*n
        vis=[False]*n
        cnt=[1]*n

        for i in range(n):
            if vis[i]==False:
                detectCycle(i,vis,inrec,cnt)
        if self.ans==0:
            return -1
        return self.ans 

        
```

---



### ✅ Q1: Find Pair with Given Difference
**Pattern**: Two Pointer / Hashing
```py
# Find pairs (a,b) from array where a - b = k
def findPairs(arr, k):
    d = {}
    result = set()
    for num in arr:
        if num - k in d or num + k in d:
            result.add((min(num, num-k), max(num, num-k)))
            result.add((min(num, num+k), max(num, num+k)))
        d[num] = True
    return list(result)

# Trick: Use set to avoid duplicates, two pointer for sorted array
# Time: O(n), Space: O(n)
```

### ✅ Q2: Longest Substring Without Repeating Characters
**Pattern**: Sliding Window
```py
def lengthOfLongestSubstring(s):
    charIndex = {}
    maxLen = 0
    left = 0
    
    for right in range(len(s)):
        if s[right] in charIndex and charIndex[s[right]] >= left:
            left = charIndex[s[right]] + 1
        charIndex[s[right]] = right
        maxLen = max(maxLen, right - left + 1)
    
    return maxLen

# Trick: Move left pointer when duplicate found
# Time: O(n), Space: O(min(n, charset))
```

### ✅ Q3: Rotate Array by K Steps
**Pattern**: Array Manipulation
```py
def rotateArray(arr, k):
    n = len(arr)
    k = k % n  # Handle k > n
    arr[:] = arr[-k:] + arr[:-k]  # Pythonic way

# Alternative: Reverse 3 times
def rotateArray2(arr, k):
    k = k % len(arr)
    arr.reverse()
    arr[:k] = reversed(arr[:k])
    arr[k:] = reversed(arr[k:])

# Time: O(n), Space: O(1)
```

### ✅ Q4: Merge Two Sorted Arrays
**Pattern**: Two Pointer
```py
def merge(nums1, m, nums2, n):
    p1, p2, p = m - 1, n - 1, m + n - 1
    
    while p1 >= 0 and p2 >= 0:
        if nums1[p1] > nums2[p2]:
            nums1[p] = nums1[p1]
            p1 -= 1
        else:
            nums1[p] = nums2[p2]
            p2 -= 1
        p -= 1
    
    # Only need to fill remaining nums2
    while p2 >= 0:
        nums1[p] = nums2[p2]
        p2 -= 1
        p -= 1

# Trick: Start from END to avoid overwriting
# Time: O(m+n), Space: O(1)
```

### ✅ Q5: Reverse Words in String
**Pattern**: String Manipulation
```py
def reverseWords(s):
    return ' '.join(s.split()[::-1])

# Alternative for complex cases
def reverseWords2(s):
    words = s.split()
    left, right = 0, len(words) - 1
    while left < right:
        words[left], words[right] = words[right], words[left]
        left += 1
        right -= 1
    return ' '.join(words)

# Time: O(n), Space: O(n)
```

### ✅ Q6: Check if String is Palindrome (ignore spaces/punctuation)
**Pattern**: Two Pointer
```py
def isPalindrome(s):
    s = s.lower()
    left, right = 0, len(s) - 1
    
    while left < right:
        while left < right and not s[left].isalnum():
            left += 1
        while left < right and not s[right].isalnum():
            right -= 1
        if s[left] != s[right]:
            return False
        left += 1
        right -= 1
    
    return True

# Time: O(n), Space: O(1)
```

### ✅ Q7: Remove Duplicates from Sorted Array
**Pattern**: Two Pointer
```py
def removeDuplicates(arr):
    if len(arr) == 0:
        return 0
    
    j = 0  # pointer for unique elements
    for i in range(1, len(arr)):
        if arr[i] != arr[j]:
            j += 1
            arr[j] = arr[i]
    
    return j + 1

# Trick: Maintain j for unique position
# Time: O(n), Space: O(1)
```

### ✅ Q8: Count Occurrences of a Number
**Pattern**: Binary Search
```py
def countOccurrences(arr, x):
    def findFirst(arr, x):
        left, right = 0, len(arr) - 1
        result = -1
        while left <= right:
            mid = (left + right) // 2
            if arr[mid] == x:
                result = mid
                right = mid - 1  # Keep searching left
            elif arr[mid] < x:
                left = mid + 1
            else:
                right = mid - 1
        return result
    
    def findLast(arr, x):
        left, right = 0, len(arr) - 1
        result = -1
        while left <= right:
            mid = (left + right) // 2
            if arr[mid] == x:
                result = mid
                left = mid + 1  # Keep searching right
            elif arr[mid] < x:
                left = mid + 1
            else:
                right = mid - 1
        return result
    
    first = findFirst(arr, x)
    if first == -1:
        return 0
    last = findLast(arr, x)
    return last - first + 1

# Time: O(log n), Space: O(1)
```

### ✅ Q9: Single Number (All others appear twice)
**Pattern**: Bit Manipulation / Hashing
```py
# Method 1: XOR (Most Efficient)
def singleNumber(arr):
    result = 0
    for num in arr:
        result ^= num  # XOR with all elements
    return result

# Method 2: Hashing
def singleNumber2(arr):
    d = {}
    for num in arr:
        d[num] = d.get(num, 0) + 1
    for num, count in d.items():
        if count == 1:
            return num

# XOR Trick: a ^ a = 0, a ^ 0 = a
# Time: O(n), Space: O(1) for XOR, O(n) for hashing
```

### ✅ Q10: Find Equilibrium Point
**Pattern**: Prefix/Suffix Sum
```py
def findEquilibrium(arr):
    total_sum = sum(arr)
    left_sum = 0
    
    for i in range(len(arr)):
        right_sum = total_sum - left_sum - arr[i]
        if left_sum == right_sum:
            return i
        left_sum += arr[i]
    
    return -1

# Trick: Use total_sum to avoid recalculating
# Time: O(n), Space: O(1)
```

---
### sliding window
---
while rt < n:

    # 1. include right element
    add(s[rt])

    # 2. shrink if invalid
    while window_invalid:
        remove(s[lt])
        lt += 1

    # 3. update answer
    ans = ...

    rt+=1

---

```py
class Solution(object):
    def characterReplacement(self, s, k):
        """
        :type s: str
        :type k: int
        :rtype: int
        """
        lt=rt=0
        d={}
        ans=0
        maxf=0
        while  rt<len(s):
            d[s[rt]]=d.get(s[rt],0)+1
            maxf=max(maxf,d[s[rt]])

            while (rt-lt+1)-maxf>k:
                d[s[lt]]-=1
                lt+=1
            ans=max(ans,rt-lt+1)

            rt+=1
        return ans
```

```py
from collections import Counter

class Solution(object):
    def checkInclusion(self, s1, s2):

        need = Counter(s1)
        window = {}

        lt = 0

        for rt in range(len(s2)):

            c = s2[rt]
            window[c] = window.get(c, 0) + 1

            if rt - lt + 1 > len(s1):

                left = s2[lt]
                window[left] -= 1

                if window[left] == 0:
                    del window[left]

                lt += 1

            if window == need:
                return True

        return False

class Solution(object):
    def lengthOfLongestSubstringKDistinct(self, s, k):

        d = {}
        lt = 0
        ans = 0

        for rt in range(len(s)):

            d[s[rt]] = d.get(s[rt], 0) + 1

            while len(d) > k:

                d[s[lt]] -= 1

                if d[s[lt]] == 0:
                    del d[s[lt]]

                lt += 1

            ans = max(ans, rt - lt + 1)

        return ans
```

only for postives
```py
# 🔹 Prefix Sum + Hashmap
# You maintain a running sum (sm) as you iterate once through the array.

# You store the first index where each prefix sum occurs in a dictionary.

# At each step, you check if sm - k has been seen before.

# This works for positive, negative, and mixed numbers.

# Time complexity: O(n), Space complexity: O(n).
class Solution:
    def longestSubarray(self, nums, k):
        lt=rt=0
        sm=0
        d={}
        ans=-10**8
        d[0]=-1
        while rt<len(nums):
            sm+=nums[rt]

            if sm-k in d:
                ans=max(ans,rt-d[sm-k]+1)
            if nums[rt]==sm:
                ans=max(ans,1)
            
            d[sm]=d.get(sm,0)+1

            rt+=1
        return ans
            
```

---
Greedy Algo

```py
class Solution:    
    def minPlatform(self, arr, dep):
        # code here
        arr.sort()
        dep.sort()
        cnt=0
        pd=0
        pc=0
        ans=0
        while pc<len(arr):
            if arr[pc]<=dep[pd]:
                pc+=1
                cnt+=1
            else:
                pd+=1
                cnt-=1
            ans=max(ans,cnt)
        return ans

                
        
        aarr=[]
        for i in range(len(arr)):
            aarr.append((arr[i],"A"))
            aarr.append((dep[i],"D"))
        aarr.sort(key=lambda x:x[0])
        print(aarr)
        
        cnt=0
        ans=0
        for i,j in aarr[:]:
            if j=="A":
                cnt+=1
            if j=="D":
                cnt-=1
        return cnt
            
            
```



# BFS, DFS, Trees & Graphs — Infosys SP/DSE Focus

These are the MOST important templates you should memorize for OA rounds.

In Infosys-style coding rounds, they usually ask:

* direct BFS/DFS
* matrix traversal
* shortest path
* connected components
* tree traversal
* cycle detection
* level order

If you know these templates properly, you can solve MANY questions quickly.

---

# 1. BFS TEMPLATE (GRAPH)

Use when:

* shortest path in unweighted graph
* minimum moves
* nearest distance
* level-by-level traversal

```python
from collections import deque

def bfs(graph, start):

    q = deque([start])
    vis = set([start])

    while q:

        node = q.popleft()

        print(node)

        for nei in graph[node]:

            if nei not in vis:
                vis.add(nei)
                q.append(nei)


graph = {
    0: [1, 2],
    1: [3],
    2: [4],
    3: [],
    4: []
}

bfs(graph, 0)
```

---

# BFS FLOW

```text
Push start node
Mark visited

while queue not empty:
    pop front
    process node

    for neighbors:
        if not visited:
            mark visited
            push
```

---

# 2. DFS TEMPLATE (GRAPH)

Use when:

* connected components
* path existence
* cycle problems
* recursion traversal

```python
def dfs(graph, node, vis):

    vis.add(node)

    print(node)

    for nei in graph[node]:

        if nei not in vis:
            dfs(graph, nei, vis)


graph = {
    0: [1, 2],
    1: [3],
    2: [4],
    3: [],
    4: []
}

dfs(graph, 0, set())
```

---

# DFS FLOW

```text
Visit node

for each neighbor:
    if unvisited:
        dfs(neighbor)
```

---

# 3. NUMBER OF ISLANDS (VERY IMPORTANT)

Classic Infosys-type DFS/BFS.

Topic:

* Matrix DFS

```python
def numIslands(grid):

    rows = len(grid)
    cols = len(grid[0])

    def dfs(r, c):

        if r < 0 or c < 0 or r >= rows or c >= cols:
            return

        if grid[r][c] == "0":
            return

        grid[r][c] = "0"

        dfs(r + 1, c)
        dfs(r - 1, c)
        dfs(r, c + 1)
        dfs(r, c - 1)

    islands = 0

    for r in range(rows):
        for c in range(cols):

            if grid[r][c] == "1":
                islands += 1
                dfs(r, c)

    return islands
```

---

# 4. ROTTING ORANGES (MULTI-SOURCE BFS)

VERY common OA pattern.

Topic:

* BFS
* Queue

```python
from collections import deque

def orangesRotting(grid):

    rows = len(grid)
    cols = len(grid[0])

    q = deque()
    fresh = 0

    for r in range(rows):
        for c in range(cols):

            if grid[r][c] == 2:
                q.append((r, c))

            elif grid[r][c] == 1:
                fresh += 1

    time = 0

    dirs = [[1,0],[-1,0],[0,1],[0,-1]]

    while q and fresh:

        for _ in range(len(q)):

            r, c = q.popleft()

            for dr, dc in dirs:

                nr = r + dr
                nc = c + dc

                if 0 <= nr < rows and 0 <= nc < cols:

                    if grid[nr][nc] == 1:

                        grid[nr][nc] = 2
                        fresh -= 1
                        q.append((nr, nc))

        time += 1

    return time if fresh == 0 else -1
```

---

# 5. TREE DFS (PREORDER)

Binary Tree traversal.

```python
class TreeNode:

    def __init__(self, val=0, left=None, right=None):

        self.val = val
        self.left = left
        self.right = right


def preorder(root):

    if not root:
        return

    print(root.val)

    preorder(root.left)
    preorder(root.right)
```

---

# TREE TRAVERSALS

## Preorder

```text
Root Left Right
```

## Inorder

```text
Left Root Right
```

## Postorder

```text
Left Right Root
```

---

# 6. LEVEL ORDER TRAVERSAL (TREE BFS)

VERY IMPORTANT.

```python
from collections import deque

def levelOrder(root):

    if not root:
        return []

    q = deque([root])

    ans = []

    while q:

        level = []

        for _ in range(len(q)):

            node = q.popleft()

            level.append(node.val)

            if node.left:
                q.append(node.left)

            if node.right:
                q.append(node.right)

        ans.append(level)

    return ans
```

---

# 7. DIAMETER OF BINARY TREE

VERY famous.

Topic:

* Tree DFS

```python
class Solution:

    def diameterOfBinaryTree(self, root):

        ans = 0

        def dfs(node):

            nonlocal ans

            if not node:
                return 0

            left = dfs(node.left)
            right = dfs(node.right)

            ans = max(ans, left + right)

            return 1 + max(left, right)

        dfs(root)

        return ans
```

---

# 8. CYCLE DETECTION IN GRAPH

VERY common.

```python
def hasCycle(graph):

    vis = set()

    def dfs(node, parent):

        vis.add(node)

        for nei in graph[node]:

            if nei not in vis:

                if dfs(nei, node):
                    return True

            elif nei != parent:
                return True

        return False

    for node in graph:

        if node not in vis:

            if dfs(node, -1):
                return True

    return False
```

---

# 9. SHORTEST PATH IN UNWEIGHTED GRAPH

BFS gives shortest path automatically.

```python
from collections import deque

def shortestPath(graph, start):

    q = deque([(start, 0)])
    vis = set([start])

    while q:

        node, dist = q.popleft()

        print(node, dist)

        for nei in graph[node]:

            if nei not in vis:

                vis.add(nei)
                q.append((nei, dist + 1))
```

---

# 10. TOPOLOGICAL SORT (VERY IMPORTANT)

Asked frequently in OA.

Topic:

* DAG
* BFS

```python
from collections import deque

def topoSort(n, edges):

    adj = [[] for _ in range(n)]
    indegree = [0] * n

    for u, v in edges:
        adj[u].append(v)
        indegree[v] += 1

    q = deque()

    for i in range(n):

        if indegree[i] == 0:
            q.append(i)

    ans = []

    while q:

        node = q.popleft()

        ans.append(node)

        for nei in adj[node]:

            indegree[nei] -= 1

            if indegree[nei] == 0:
                q.append(nei)

    return ans
```

---

# MOST IMPORTANT GRAPH PATTERNS

| Problem Type         | Use              |
| -------------------- | ---------------- |
| Shortest path        | BFS              |
| Connected components | DFS              |
| Matrix traversal     | BFS/DFS          |
| Cycle detection      | DFS              |
| Course schedule      | Topological Sort |
| Minimum moves        | BFS              |
| Flood fill           | DFS/BFS          |

---

# MOST IMPORTANT TREE PATTERNS

| Problem        | Pattern   |
| -------------- | --------- |
| Traversals     | DFS       |
| Level order    | BFS       |
| Height         | DFS       |
| Diameter       | DFS       |
| LCA            | Recursion |
| BST validation | Inorder   |

---

# MUST-DO LEETCODE QUESTIONS

## Graphs

* Number of Islands
* Rotting Oranges
* Clone Graph
* Course Schedule
* Pacific Atlantic
* Flood Fill

## Trees

* Maximum Depth
* Diameter of Binary Tree
* Same Tree
* Invert Binary Tree
* Level Order Traversal
* Lowest Common Ancestor

---

# Super Important Interview/OA Tip

For graph problems:

## STEP 1

Ask:

```text
Shortest path?
```

→ BFS

## STEP 2

Ask:

```text
Need exhaustive traversal?
```

→ DFS

## STEP 3

Ask:

```text
Dependency ordering?
```

→ Topological Sort

That alone solves 70% of graph confusion.

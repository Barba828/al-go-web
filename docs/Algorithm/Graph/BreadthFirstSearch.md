# 广度优先遍历

顾名思义，bfs总是先访问完同一层的结点，然后才继续访问下一层结点，它最有用的性质是可以遍历一次就生成中心结点到所遍历结点的最短路径，这一点在求无权图的最短路径时非常有用。广度优先遍历的核心思想非常简单，用python实现起来也就十来行代码。下面就是超精简的实现，用来理解核心思想足够了：

## Python代码实现

```python
import Queue def bfs(adj, start):    
    visited = set()    
    q = Queue.Queue()    
    q.put(start)    
    while not q.empty():       
        u = q.get()        
        print(u)        
        for v in adj.get(u, []):            
            if v not in visited:                
            visited.add(v)                
            q.put(v)  

graph = {1: [4, 2], 2: [3, 4], 3: [4], 4: [5]}bfs(graph, 1)
bfs(graph, 1)
```


## JAVA代码实现

```java
import java.util.LinkedList;
import java.util.Queue;

public class BFSTest {     
    /**     
    * 存储节点信息     
    */    
    private char[] vertices;     
    /**     
     * 存储边信息（邻接矩阵）     
     */    
    private int[][] arcs;     
    /**     
     * 图的节点数     
     */    
    private int vexnum;     
    /**     
     * 记录节点是否已被遍历     
     */    
    private boolean[] visited;     
    /**     
     * 初始化     
     */    
    public BFSTest(int n) {        
        vexnum = n;        
        vertices = new char[n];        
        arcs = new int[n][n];        
        visited = new boolean[n];        
        for (int i = 0; i < vexnum; i++) {            
            for (int j = 0; j < vexnum; j++) {                
                arcs[i][j] = 0;            
            }        
        }    
    }     
    /**     
     * 添加边     
     */    
    public void addEdge(int i, int j) {        
        if (i == j) {            
            return;        
        }        
        arcs[i][j] = 1;        
        arcs[j][i] = 1;    
    }     
    /**     
     * 设置节点集     
     */    
    public void setVertices(char[] vertices) {        
        this.vertices = vertices;    
    }     
    /**     
     * 设置节点访问标记     
     */    
    public void setVisited(boolean[] visited) {        
        this.visited = visited;    
    }     
    /**     
     * 打印遍历节点     
     */    
    public void visit(int i) {        
        System.out.print(vertices[i] + " ");    
    }     
    /**     
     *  输出邻接矩阵     
     */    
    public void pritf(int[][] arcs){        
        for(int i=0;i<arcs.length;i++){            
            for(int j=0;j<arcs[0].length;j++){                
                System.out.print(arcs[i][j]+ "\t");            
            }            
            System.out.println();        
        }    
    }     
    /**     
     * 实现广度优先遍历     
     */    
    public void bfs() {        
        // 初始化所有的节点的访问标志        
        for (int v = 0; v < visited.length; v++) {            
            visited[v] = false;        
        }        
        Queue<Integer> queue = new LinkedList<Integer>();        
            for (int i = 0; i < vexnum; i++) {            
                if (visited[i] == false) {                
                    visited[i] = true;                
                    // 打印当前已经遍历的节点                
                    visit(i);                
                    // 添加到队列里面                
                    queue.add(i);                
                    // 只要队列不为空                
                    while (!queue.isEmpty()) {                    
                    // 出队节点,也就是这一层的节点.                    
                    int k = queue.poll();                    
                    // 遍历所有未被访问的邻接节点,放入队列                    
                    for (int j = 0; j < vexnum; j++) {                        
                        // 也就是访问这一层剩下的未被访问的节点                        
                        if (arcs[k][j] == 1 && visited[j] == false) {                                     
                            visited[j] = true;                            
                            visit(j);                            
                            queue.add(j);                        
                        }                    
                    }                
                }            
            }        
        }        
        System.out.println();        
        pritf(arcs);    
    }     
    public static void main(String[] args) {        
    BFSTest g = new BFSTest(9);        
    char[] vertices = {'A', 'B', 'C', 'D', 'E', 'F', 'G', 'H', 'I'};        
    // 设置顶点集        
    g.setVertices(vertices);        
    // 添加边        
    g.addEdge(0, 1);       
    g.addEdge(0, 5);        
    g.addEdge(1, 0);        
    g.addEdge(1, 2);        
    g.addEdge(1, 6);        
    g.addEdge(1, 8);        
    g.addEdge(2, 1);        
    g.addEdge(2, 3);        
    g.addEdge(2, 8);        
    g.addEdge(3, 2);        
    g.addEdge(3, 4);        
    g.addEdge(3, 6);        
    g.addEdge(3, 7);        
    g.addEdge(3, 8);        
    g.addEdge(4, 3);        
    g.addEdge(4, 5);        
    g.addEdge(4, 7);        
    g.addEdge(5, 0);        
    g.addEdge(5, 4);        
    g.addEdge(5, 6);        
    g.addEdge(6, 1);        
    g.addEdge(6, 3);        
    g.addEdge(6, 5);        
    g.addEdge(6, 7);        
    g.addEdge(7, 3);        
    g.addEdge(7, 4);        
    g.addEdge(7, 6);        
    g.addEdge(8, 1);        
    g.addEdge(8, 2);        
    g.addEdge(8, 3);        
    System.out.print("广度优先遍历(队列)：");        
    g.bfs();    
    }
}
```

## C++代码实现
```c
#include<iostream>
#include<vector>
#include<queue>
#include<memory.h> 
using namespace std; 
vector<vector<int>> tree;
//声明一个二维向量
int flag[10];
//用于搜索到了节点i的第几个节点
queue<int> M;
//声明一个队列
int ar_tree[8] = { 1,1,1,3,5,3,5,7 };
void BFS(int node) {	
    int temp;	
    cout << node << " ";	
    //从队列中取出第一个节点	
    int m_first = M.front();	
    M.pop();	
    while(flag[node] < tree[node].size()) {		
        temp = tree[node][flag[node]];		
        flag[node]++;		
        //把temp加入队列中		
        M.push(temp);	
    }	
    if (!M.empty()) {		
        BFS(M.front());	
    }
}
int main() {	
    ios::sync_with_stdio(false);	
    memset(flag, 0, sizeof(flag));	
    int i,temp;	tree.resize(10);
    //图中的数共有九个节点	
    //生成树	
    for (i = 2; i <=9; i++) {		
        temp = ar_tree[i - 2];		
        tree[temp].push_back(i);
        //表示第i个节点为第temp个节点的子节点	
    }	
    //BFS	
    cout << "BFS过程：" << endl;	
    M.push(1);	
    BFS(1);	
    cout << endl;	
    return 0;
}
```

## JavaScript代码实现
创建 BFS 步骤
1. 创建一个队列Q。
2. 将v标注为被发现的（灰色），并将v入队列Q。
3. 如果Q非空，则运行以下步骤：
>(a) 将u从Q中出队列；</br>
>(b) 将标注u为被发现的（灰色）；</br>
>(c) 将u所有未被访问过的邻点（白色）入队列；</br>
>(d) 将u标注为已被探索的（黑色）。</br>

```js
this.bfs = function (v, callback) {    
    var color = initializeColor(), 
    //初始化所有节点的颜色信息是白色        
    queue = new Queue(); 
    //存储待访问和待探索的顶点    
    queue.enqueue(v); 
    //起始定点  直接入队    
    while (!queue.isEmpty()) { 
        //队列非空        
        var u = queue.dequeue(), 
        //操作队列，从中移除一个顶点            neighbors = adjList.get(u); 
        //取得包含所有邻点的邻接表        
        color[u] = 'grey'; 
        // 表示已经访问但未探索        
        for (var i = 0; i < neighbors.length; i++) { // 对u的每个邻点            
        var w = neighbors[i]; // 取值            if (color[w] === 'white') { 
            // 如果没有进行访问                
            color[w] = 'grey'; 
            // 标记访问               
            queue.enqueue(w); 
            // 将该顶点加入队列中            
            }        
        }        
        color[u] = 'black'; 
        // 已经访问并已经探索完成        
        if (callback) { 
            // 回调函数...            
            callback(u);        
            }    
        }
    };
```
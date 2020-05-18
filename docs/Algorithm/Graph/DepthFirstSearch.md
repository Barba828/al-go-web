# 深度优先遍历

深度优先遍历(Depth First Search)的主要思想是：

1. 首先以一个未被访问过的顶点作为起始顶点，沿当前顶点的边走到未访问过的顶点；
2. 当没有未访问过的顶点时，则回到上一个顶点，继续试探别的顶点，直至所有的顶点都被访问过。

在此我想用一句话来形容 “不到南墙不回头”。

## Python代码实现
```python
#递归实现DFS    
def depth_first_search(self,root=None):        
    order=[]        
    def dfs(node):           
        self.visited[node] = True            
        order.append(node)            
        for n in self.node_neighbors[node]:                   if not n in self.visited:                         dfs(n)         
    if root:            
        dfs(root)         
#对于不连通的结点（即dfs（root）完仍是没有visit过的单独处理，再做一次dfs        
    for node in self.nodes():            
        if not node in self.visited:          
            dfs(node)        
    self.visited = {}       
    print order        
    return order
```

## Java代码实现

1. 递归实现DFS
```java
//DFS递归实现
public void DFSWithRecursion(TreeNode root) {    
    if (root == null)        
    return;     
    //在这里处理遍历到的TreeNode节点            
    if (root.left != null)        
    DFSWithRecursion(root.left);    
    if (root.right != null)        
    DFSWithRecursion(root.right);}
```
2. 使用STACK实现DFS算法

```java
//DFS的迭代实现版本（Stack）
public void DFSWithStack(TreeNode root) {     
    if (root == null)         
    return;     
    Stack<TreeNode> stack = new Stack<>();     stack.push(root);      
    while (!stack.isEmpty()) {         
        TreeNode treeNode = stack.pop();          
        //在这里处理遍历到的TreeNode                
        if (treeNode.right != null)                 
            stack.push(treeNode.right);          
        if (treeNode.left != null)                 
            stack.push(treeNode.left);     
        }
    }
```

## C++代码实现

### 递归实现
```c
void dfs(int v)//以v开始做深度优先搜索
{
    list<int>::iterator it;
    visited[v] = true;
    cout << v << " ";
    for (it = graph[v].begin(); it != graph[v].end(); it++)
        if (!visited[*it])
            dfs(*it);
}
```
### 非递归实现
```c
void dfs_noRecursion(int v)//以v开始做深度优先搜索,非递归实现
{
    list<int>::iterator it;
    visited[v] = true;
    cout << v << " ";
    stack<int>mystack;
    mystack.push(v);
    while (!mystack.empty())
    {
        v = mystack.top();
        mystack.pop();
        if (!visited[v])
        {
            cout << v << " ";
            visited[v] = true;
        }

        for (it = graph[v].begin(); it != graph[v].end(); it++)
        {
            if (!visited[*it])
            {
                mystack.push(*it);

            }
        }
    }
    cout << endl;
}
```

## JavaScript代码实现

```js
let tree={
  'id1':{
    message:'hello'
  },
  'id2':{
    message:'world',
    children:{
      'id2-1':{
        message:'haha',
        children:{
        }
      },
      'id2-2':{
        message:'heihei'
      }
    }
  }
}
function dfs(tree={},messages=[]){
  let i=0;
  if(!messages) messages=[]
  if(tree.message) messages.push(tree.message);
  const keys=Object.keys(tree.children||{});
  while(i<keys.length){
    dfs(tree.children[keys[i]],messages);
    i+=1;
  }
  return messages;
}
tree={
  message:null,
  children:tree
};
dfs(tree);
```
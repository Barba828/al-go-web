# Kruskal最小生成树

克鲁斯卡尔算法的基本思想是以边为主导地位，始终选择当前可用的最小边权的边（可以直接快排或者algorithm的sort）。每次选择边权最小的边链接两个端点是kruskal的规则，并实时判断两个点之间有没有间接联通。

## JAVA代码实现
```java
import java.util.Arrays;
import java.util.Scanner;
class Node implements Comparable<Node>
{
    int x;
    int y;
    int val;
    public Node(int x,int y,int val)
    {
        this.x=x;
        this.y=y;
        this.val=val;
    }
    @Override
    public int compareTo(Node o) {
        return this.val-o.val;
    }
}
public class Main {
    
    public static void init(int a[])//并查集初始化，用来判断是否有环
    {
        for(int i=1;i<a.length;i++)a[i]=i;
        
    }
    public static int find(int a[],int x) //查找节点的父亲,没有优化的方法
    {
        while(a[x]!=x)
        {
            x=a[x];
        }
        
        return x;
    }
    public static boolean union(int a[],int x,int y)//union一条边
    {
        int fx=find(a, x);
        int fy=find(a, y);
        if(fx!=fy)
        {
            a[fx]=fy;
            return true;  //成功加入
            
        }
        return false;//成环 
    }

    public static void main(String[] args) {
        // TODO Auto-generated method stub
        Scanner scn=new Scanner(System.in);
        int len=scn.nextInt();
        while(len-->0)
        {
            int ans=0;//保存最后的答案
            int v=scn.nextInt();
            int e=scn.nextInt();
            Node n[]=new Node[e];
            for(int i=0;i<e;i++)
            {
                n[i]=new Node(scn.nextInt(),scn.nextInt(),scn.nextInt()); 
            } 
            Arrays.sort(n);
            //并查集的初始化
            int father[]=new int[v+1];
            init(father);
            int index=0;
            for(int i=0;i<e;i++)
            {
                if(union(father, n[i].x,n[i].y))
                {
                    index++; //没成环，加入这条边
                    ans+=n[i].val;
                }
                if(index==v-1)
                {
                    break;
                } 
            }
            int min=scn.nextInt();
            
            for(int j=1;j<v;j++)
            {
                int temp=scn.nextInt();
                if(min>temp) min=temp;  
            }
            System.out.println(ans+min);
        }
    }
}
```
## C++代码实现
```C++
#include<iostream>
#include<cstdio>
#include<algorithm>
using namespace std;
//边的结构
typedef struct{
	int a,b; 
	int w; //权重
}Road;
Road road[maxSize];
//并查集的结构
int v[maxSize];
//得到p的根结点
int getRoot(int p) {
	while (p!=v[p]) //当p==v[p]时，即找到了根结点
		p = v[p];
	return p; //只有根结点的父亲是它自己
}
// 克鲁斯卡尔算法
// road[]：边集
// n：顶点数
// e：边的个数
int Kruskal(Road road[], int n, int e) {
	int sum=0;
	int a,b;
	int i;
	for (i=0; i<n; ++i) v[i]=i; //构造并查集的初始状态，各个节点为单独的树（即它的父亲都为它们自己）
	sort(road, e); //将边数组，从小到大排序
	for (i=0; i<e; ++i) { //遍历边数组，边数组已经被排序过了，从小到大-->从头到尾扫描即可
		a = getRoot(road[i].a);
		b = getRoot(road[i].b);
		if (a!=b) { //不会构成环
			v[a] = b; //更新并查集
			sum += road[i].w; //将边加入生成树
		}
	}
}
```
## JavaScript代码实现
```js
/**
 * kruskal算法
 * 遍历所有的边，按权值从小到大排序，每次选取当前权值最小的边，只要不构成回环，则加入生成树
 * 邻接矩阵转换成边集数组
 * 优点：适合点多边少的情况
 * @param matrix 邻接矩阵
 * @return Array 最小生成树的边集数组
 * */
function kruskal(matrix) {
    const edgeArray = changeMatrixToEdgeArray(matrix),
        result = [],
        //使用一个数组保存当前顶点的边的终点,0表示还没有已它为起点的边加入
        savedEdge = new Array(matrix.length).fill(0);
 
    for (let i = 0, len = edgeArray.length; i < len; i++) {
        const edge = edgeArray[i];
        const n = findEnd(savedEdge, edge.getBegin());
        const m = findEnd(savedEdge, edge.getEnd());
        console.log(savedEdge, n, m);
        //不相等表示这条边没有与现有生成树形成环路
        if (n !== m) {
            result.push(edge);
            //将这条边的结尾顶点加入数组中，表示顶点已在生成树中
            savedEdge[n] = m;
        }
    }
    return result;
}
/**
 * 查找连线顶点的尾部下标
 * @param arr 判断边与边是否形成环路的数组
 * @param start 连线开始的顶点
 * @return Number 连线顶点的尾部下标
 * */
function findEnd(arr, start) {
    //就是一直循环，直到找到终点，如果没有连线，就返回0
    while (arr[start] > 0) {
        start = arr[start];
    }
    return start;
}
```
 

# Prim最小生成树
论中的一种算法，可在加权连通图里搜索最小生成树。意即由此算法搜索到的边子集所构成的树中，不但包括了连通图里的所有顶点（英语：Vertex (graph theory)），且其所有边的权值之和亦为最小。

## Prim算法思想
1. 从某个顶点开始（不要把它看成一个单独的顶点，把它看成只有一个结点的子生成树）
2. 在第一步的生成树的相邻边中，选一条最小的边，将最小的边和边的另一个结点并入子生成树中（生成树就长大了一点）
3. 继续，直到所有的顶点都被并入了生成树

## Prim算法实现步骤
1.取图中任意一个顶点 v 作为生成树的根，之后往生成树上添加新的顶点 w
2.在添加的顶点 w 和已经在生成树上的顶点v 之间必定存在一条边，
3.并且该边的权值在所有连通顶点 v 和 w 之间的边中取值最小。
4.之后继续往生成树上添加顶点，直至生成树上含有 n-1 个顶点为止。

## JAVA代码实现
```java
package com.liuzhen.chapter8;

import java.util.ArrayList;

public class Prim {
    /*
     * 参数G：给定的图，其顶点分别为0~G.length-1，相应权值为具体元素的值
     * 函数功能：返回构造生成的最小生成树，以二维数组形式表示，其中元素为0表示最小生成树的边
     */
    public void getMinTree(int[][] G) {
        int[][] result = G;
        int[] vertix = new int[G.length];   //记录顶点是否被访问，如果已被访问，则置相应顶点的元素值为-2
        for(int i = 0;i < G.length;i++)
            vertix[i] = i;
        ArrayList<Integer> listV = new ArrayList<Integer>(); //保存已经遍历过的顶点
        listV.add(0);      //初始随意选择一个顶点作为起始点，此处选择顶点0
        vertix[0] = -2;    //表示顶点0被访问
        while(listV.size() < G.length) {  //当已被遍历的顶点数等于给定顶点数时，退出循环
            int minDistance = Integer.MAX_VALUE;    //用于寻找最小权值，初始化为int最大值，相当于无穷大的意思
            int minV = -1;   //用于存放未被遍历的顶点中与已被遍历顶点有最小权值的顶点
            int minI = -1;   //用于存放已被遍历的顶点与未被遍历顶点有最小权值的顶点  ；即G[minI][minV]在剩余的权值中最小
            for(int i = 0;i < listV.size();i++) {   //i 表示已被访问的顶点
                int v1 = listV.get(i);
                for(int j = 0;j < G.length;j++) {
                    if(vertix[j] != -2) {    //满足此条件的表示，顶点j未被访问
                        if(G[v1][j] != -1 && G[v1][j] < minDistance) {//G[v1][j]值为-1表示v1和j是非相邻顶点
                            minDistance = G[v1][j];
                            minV = j;
                            minI = v1;
                        }
                    }
                }
            }
            vertix[minV] = -2;
            listV.add(minV);
            result[minI][minV] = 0;
            result[minV][minI] = 0;
        }
        System.out.println("使用Prim算法，对于给定图中的顶点访问顺序为：");
        System.out.println(listV);
        System.out.println("使用Prim算法，构造的最小生成树的二维数组表示如下（PS：元素为0表示树的边）：");
        for(int i = 0;i < result.length;i++) {
            for(int j = 0;j < result[0].length;j++)
                System.out.print(result[i][j]+"\t");
            System.out.println();
        }
    }
    
    public static void main(String[] args) {
        Prim test = new Prim();
        int[][] G = {{-1,3,-1,-1,6,5},
                {3,-1,1,-1,-1,4},
                {-1,1,-1,6,-1,4},
                {-1,-1,6,-1,8,5},
                {6,-1,-1,8,-1,2},
                {5,4,4,5,2,-1}};
        test.getMinTree(G);
    }
}
```

## C代码实现
```c
// 最小生成树
#define INF 1000 //最大值
//VRType表示边的类型
VRType GetMSTSumByPrim(MGraph G, int v0) { //从v开始找最小生成树
	int MST[MAX_VERTEX_NUM]; //结点i已在最小生成树里-->MST[i]=1;
	VRType lowCost[MAX_VERTEX_NUM]; //最小代价
	VRType sum; //生成树的代价

	VRType min=INF; //当前最小的边
	int minNode; //最小边的另一个顶点
	
	int n=0; //生成树当前有几个结点
	int i;
	
	// 初始化
	n=0; //生成树当前有几个结点
	for (i=0; i<G.vexnum; ++i) {
		lowCost[i] = G.arcs[v0][i].adj; //最小边
		MST[i]=0; //访问记录
	}
	MST[v0]=++n; //v是生成树的第一个结点
	sum=0;
	// 循环考虑其他结点
	while (n<G.vexnum) { //生成树中的结点数<总数
		// 找lowCost里的最小值 和 对应的顶点
		min = INF;
		for (i=0; i<G.vexnum; i++) {
			// i结点还没有并入生成树
			if (MST[i]==0 && lowCost[i]<min) {
				min=lowCost[i];
				minNode=i;
			}
		}
		// 将minNode加入生成树
		sum += min;
		MST[minNode]=++n;
		// 更新lowCost
		for (i=0; i<G.vexnum; i++) {
			if (MST[i]==0 && G.arcs[minNode][i].adj<lowCost[i]) {
				lowCost[i]=G.arcs[minNode][i].adj;
			}
		}
	}

	return sum;
}
```
## JavaScript代码实现
```js
/**
 * Prim算法
 * 以某顶点为起点，逐步找各顶点上最小权值的边构建最小生成树，同时其邻接点纳入生成树的顶点中,只要保证顶点不重复添加即可
 * 使用邻接矩阵即可
 * 优点：适合点少边多的情况
 * @param matrix 邻接矩阵
 * @return Array 最小生成树的边集数组
 * */
function prim(matrix) {
    const rows = matrix.length,
        cols = rows,
        result = [],
        savedNode = [0];//已选择的节点
    let minVex = -1,
        minWeight = MAX_INTEGER;
    for (let i = 0; i < rows; i++) {
        let row = savedNode[i],
            edgeArr = matrix[row];
        for (let j = 0; j < cols; j++) {
            if (edgeArr[j] < minWeight && edgeArr[j] !== MIN_INTEGER) {
                minWeight = edgeArr[j];
                minVex = j;
            }
        }
 
        //保证所有已保存节点的相邻边都遍历到
        if (savedNode.indexOf(minVex) === -1 && i === savedNode.length - 1) {
            savedNode.push(minVex);
            result.push(new Edge(row, minVex, minWeight));
 
            //重新在已加入的节点集中找权值最小的边的外部边
            i = -1;
            minWeight = MAX_INTEGER;
 
            //已加入的边，去掉，下次就不会选这条边了
            matrix[row][minVex] = MAX_INTEGER;
            matrix[minVex][row] = MAX_INTEGER;
        }
    }
    return result;
}
```
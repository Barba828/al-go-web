# Dijkstra最短路径

```java
public void start(int vs) {
		//初始化类参数
		boolean[] isVisited = new boolean[numNodes ];
		for (int i = 0; i < isVisited.length; i++) {
			dist[i] = matrix[vs][i];
			prev[i] = -1;
			if(dist[i] != INF) {
				prev[i] = vs;
			}
		}
		isVisited[vs] = true;
		//两次循环
		for (int i = 0; i < isVisited.length; i++) {
			int min = INF;
			int k = 0;
			//找到最近的节点
			for (int j = 0; j < isVisited.length; j++) {
				if(!isVisited[j] && dist[j] < min ) {
					min = dist[j];
					k = j;
				}
			}
			
			isVisited[k] = true;
			//更新最近路径和父节点
			for (int j = 0; j < isVisited.length; j++) {
				if(!isVisited[j] && matrix[k][j] != INF) {
					if(dist[j] > matrix[k][j] + dist[k]) {
						dist[j] =  matrix[k][j] + dist[k] ;
						prev[j] = k;
					}
        }
			}
		}
		//打印节点、路径、距离
		for (int i = 0; i < isVisited.length; i++) {
			System.out.print( "节点" + i + "  " );
			int a = i;
			System.out.print("路径：");
			
			while (a != vs) {
				System.out.print(   prev[a] +"  ");
				a = prev[a];
			}
			System.out.println("距离" + dist[i]);
		}
	}
```
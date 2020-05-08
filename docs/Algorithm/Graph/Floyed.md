## Floyed最短路径

```java
Floyed(int e[][]) {
  for(k=0;k<e.length;k++) {
    for(i=0;i<e.length;i++) {
      for(j=0;j<e.length;j++) {
        if(e[i][j]>e[i][k]+e[k][j] )   
            e[i][j]=e[i][k]+e[k][j]; 
      }
    }
  }
}
```

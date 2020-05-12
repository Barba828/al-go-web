
# 数组问题

- 以下输出是什么 ？
```java
class TestIt
{
    public static void main ( String[] args )
    {
        int[] myArray = {1, 2, 3, 4, 5};
        ChangeIt.doIt( myArray );
        for(int j=0; j<myArray.length; j++)
            System.out.print( myArray[j] + " " );
    }
}
class ChangeIt
{
    static void doIt( int[] z ) 
    {
        z = null ;
    }
}
```
> 1 2 3 4 5

- 如果我们声明：
```java
int [] ar = {1,2,3,4,5,6};
```
数组ar的大小是 ：
> 6

- Java 使用按值调用。 以下方法调用传递给程序的值是多少 ？
```java
double[] rats = {1.2, 3.4, 5.6};
routine( rats );
```
> rats 数组的引用
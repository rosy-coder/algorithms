### 堆

```java
class Heap{
    private int[] heap;
    public Heap(int n){heap=new int[n+1];}//1..n
    private void swap(int i,int j){
        int tmp=heap[i];
        heap[i]=heap[j];
        heap[j]=tmp;
    }
    private void sift_up(int i){
        //sift up heap[i] if needed,based on others are right in place.
        while (i!=1){
            if (heap[i]>heap[i/2]){
                swap(i,i/2);
            }else break;
            i=i/2;
        }
    }
    
    private void sift_down(int i){
        //sift down heap[i] if needed,based on others are right in place.
        while (2*i<=n){
            i=2*i;
            if (i+1<=n&&heap[i+1]>heap[i]) i++;
            if (heap[i]>heap[i/2]) swap(i,i/2);
            else break;
        }
    }
    public void insert(int e){}//add in the tail and sift up
    public void delete(int i){}//use one to replace and accodingly sift up or down
    public void makeHeap(int[] heap){
        for (i=heap.length/2;i>=1;i--) sift_down(i);
    }
}
```

### 并查集

```java
class UnionFind{
    private int[] parent;
    public UnionFind(int n){parent=new int[n];}
    public int find(int x){
        if (x!=parent[x]) parent[x]=find(parent[x]);
        return parent[x];
    }
    public void union(int x,int y){parent[find(x)]=find(y);} 
}//optimize using rank.
```

### 全排列

```java
//next sequence: from right to left, find the first decline,swap this value with the smallest value in its right place,and sort values after this place.
void permution(int n){
    int[] s=new int[n+1];
    for (int i=1;i<n;i++) s[i]=i;
    
    void perm(int m){
        if (m==n-1){
            for (int i=1;i<=n;i++) System.out.print(i+" ");
        }else{
            for (int i=m;i<=n;i++){
                swap(m,i);
                perm(m+1);
                swap(m,i);
            }
        }
    }
    
    perm(0);
}
```

### 矩阵+快速幂求斐波那契数

```java
//use |1 1| to calculate fibonacci. |1 1|.| a(n) |==>|a(n+1)|
//	  |1 0|							|1 0|`|a(n-1)|==>| a(n) |
int[][] matrix=new int[2][2];
matrix[0][0]=matrix[0][1]=matrix[1][0]=1;

int[][] pow(int n){
    //calculate matrix^n
    if (n==0){
        int[][] res=new int[2][2];
        res[0][0]=res[1][1]=1;
    }else{
    	int[][] tmp=pow(n/2);
        int[][] res=multi(tmp,tmp);
        if (n&1) res=multi(res,matrix);
    } 
    return res;
}
int[][] multi(int[][] a,int[][] b){
    int[][] res=new int[2][2];
    res[0][0]=a[0][0]*b[0][0]+a[0][1]*b[1][0];
    res[0][1]=a[0][0]*b[0][1]+a[0][1]*b[1][1];
    res[1][0]=a[1][0]*b[0][0]+a[1][1]*b[1][0];
    res[1][1]=a[1][0]*b[0][1]+a[1][1]*b[1][1];
    return res;
} 
int fib(int n){
    int m=pow(n-1);
    return m[0][0]+m[0][1];
}
```

### Prim

```java
//from one point,find lowcost to other point
int[] lowcost=new int[n];
int[] visited=new int[n];
void prim(int[][] graph){
    visited[0]=1;
    for (int i=0;i<n;i++){
        lowcost[i]=graph[0][i];//lowcost means the low dis to founded set. 
    }
    
    for (int i=1;i<n;i++){
        int mini;
        int minDis=0x3f3f3f3f;
        for (int j=1;j<n;j++){//find lowcost.
            if (visited[j]==0){
                if (minDis>lowcost[j]){
                    mini=j;
                    minDis=lowcost[j];
                }
            }
        }
        //add something here.
        
        visited[mini]=1;
        for (int j=1;j<n;j++){//update lowcost.
            if (visited[j]==0){
                if (lowcost[j]>graph[mini][j]) lowcost[j]=graph[mini][j];
            }
        }
    }
}//optimize using heap
```

### Kruskal

```java
class UnionFind;//use before
class Edge{
    public int beg;
    public int end;
    public Edge(int b,int e){this.beg=b;this.end=e;}
}
void kruskal(Edge[] edges){
    PriorityQueue<Edge> pqn=new PriorityQueue<>();
    for (Edge e: edegs){
        pqn.offer(e);
    }//or just sort and traverse
    while (!pqn.isEmpty()){
        Edge e=pqn.poll();
        if (find(e.beg)!=find(e.end)){
            union(e.beg,e.end);
            //add somthing here
        }
    }
}
```

### Dijkstra

```java
//when it comes to negatives, need some changes for instance plus abs(minoflen).
int[] dis=new int[n];
int[] path=new int[n];
int[] visited=new int[n];
Arrays.fill(path,-1);

void dijkstra(int begin,int[][]graph){
    for (int i=0;i<n;i++){
        dis[i]=graph[i][begin];
        if (dis[i]!=INF) path[i]=begin;
    }
    
    visited[begin]=1;
    for (int i=1;i<n;i++){//loop n-1 times
        int minj;
        int minDis=0x3f3f3f3f;
        for (int j=1;j<n;j++){
            if (visited[j]==0){
                if (minDis>dis[j]){
                    minj=j;
                    minDis=dis[j];
                }
            }
        }
        visited[minj]=1;
        
        for (int j=0;j<n;j++){
            if (visited[j]==0){
                if (dis[j]>dis[minj]+graph[j][minj]){
                    dis[j]=dis[minj]+graph[j][minj];
                    path[j]=minj;
                }
            }
        }
    }
}
```

### 树状数组

```java
int[] tree=new int[n];
int lowbit(int x){return x&-x;}
void add(int x,int k){
	for (int i=x;i<n;i+=lowbit(i)){
        tree[i]+=k;
    }
}
int query(int x){
    int res=0;
    for (int i=x;i>0;i-=lowbit(i)){
        res+=tree[i];
    }
    return res;
}//and one use about two bounds for this see NKOI 1317.
```

### Java遍历Map

```java
HashMap<T,U> map=new HashMap<>();
//1.forEach+lambda
map.forEach((k,v)->{System.out.print(k+" "+v);});
//2.Map.Entry
for (Map.Entry<T,U> it :map.entrySet()){
	System.out.print(it.getKey()+" "+it.getValue);
}
//3.keySet and values
for (T t: map.keySet()){
    System.out.print(t);
}
for (U u: map.values()){
	System.out.print(u);
}
//4.iterator,don't iterate and remove simultaneously.
for(Iterator<Map.Entry<T,U>> it = aMap.entrySet().iterator();it.hasNext();){
	Map.Entry<T,U> iter = it.next();
	System.out.println(iter.getKey()+" "+iter.getValue());
}
```

### Java自定义排序PriorityQueue,TreeMap/Set

```java
//1.create class implements Comparator or Comparable
class T implements Comparable<T>{
    @Override
    public int compareTo(T o){
        
    }
}
//or
class T implements Comparator<T>{
    @Override
    public int compare(T t1,T t2){
        
    }
}
//2.use comparator as the parameter
PriorityQueue<T> pqn=new PriorityQueue<>(new Comparator<T>(){
 	@Override
    public int compare(T t1,T t2){
        
    }
});
//3.use lambda as the parameter
PriorityQueue<T> pqn=new PriorityQueue<>((T t1,T t2)->....)//maybe hhh
```

### Java Lambda

```java
(parameters)-> {statements;}
(parameters)-> expression
```

*See [lambda on runoob](https://www.runoob.com/java/java8-lambda-expressions.html).*																																																												2021/04/18 1:18

---


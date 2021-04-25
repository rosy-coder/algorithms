### 第六章 堆排序

#### 堆

```c
//MAX_HEAPLIFY
//assump that right(i) and left(i) are right heaps
void sift_max(int* A,int i,int n){//A[1..n]
   	int maxi=i;
    if (i*2<=n&&A[i]<A[i*2]) maxi=i*2;
    if (i*2+1<=n&&A[maxi]<A[i*2+1]) maxi=i*2+1;
    if (maxi!=i){
        swap(A,i,maxi);
        sift_max(A,maxi,n);
    }
}//O(logn)

//MIN_HEAPLIFY
void sift_min(int* A,int i,int n){
    int mini=i;
    if (i*2<=n&&A[i]>A[i*2]) mini=i*2;
    if (i*2+1<=n&&A[mini]>A[i*2+1]) mini=i*2+1;
    if (mini!=i){
        swap(A,i,mini);
        sift_min(A,mini,n);
    }
}
```

#### 建堆

```c
void make_*_heap(int* A,int n){
    for (int i=n/2;i>0;i--){
        sift_*(A,i);
    }
}//O(n)
```

#### 堆排序

```c
void heap_sort(int* A,int& n){
    make_max_heap(A,n);
    for (int i=n;i>1;i--){
        swap(A,1,i);
        n--;
        sift_max(A,1,n);
    }
}
```

#### 优先队列

```java
class PriorityQueue{
	private int[] A;
    private int n;
    //...
    public int heap_maximum(){return A[1];}
    public int heap_extract_max(){
        int max=A[1];
        A[1]=A[n];
        n--;
        sift_max(A,i,n);
        return max;
    }
    public int insert(int key){
        n++;
        int i=n;
        while (i>1&&A[i/2]<key){
            A[i]=A[i/2];
            i/=2;
        }
        A[i]=key;
    }
}
```

#### 思考题

* 用不同的方法建堆

  ```text
  此题相当于比较sift_up()与sift_down()函数建堆的过程。
  sift_up()是从n/2开始直到1;复杂度O(n)
  sift_down()是从1到n;复杂度O(nlogn)
  ```

* 对d叉堆的分析

  ```
  d叉树
  ```

* Young矩阵

  ```java
  //a.从上到下、从左到右都排好序的mxn矩阵称为young矩阵，未放置数字处为∞
  //b.O(m+n)实现extract_min()
  T extract_min(T[][] t,int i,int j){//应该再添加越界判断.
      int min=t[i][j];
  	if (t[i][j+1]==INF&&t[i+1][j]==INF){
          t[i][j]=INF;
          return min;
      }
      if (t[i][j+1]<t[i+1][j]){
          t[i][j]=t[i][j+1];
          t[i][j+1]=min;
          return extract_min(t,i,j+1);
      }else{
          t[i][j]=t[i+1][j];
          t[i+1][j]=min;
          return extract_min(t,i+1,j);
      }
  }
  //c.O(m+n)插入未满的矩阵。
  T insert(T[][] t,T key){
      int i=m;int j=n;
      t[i][j]=key;
      while (t[i-1][j]>t[i][j]||t[i][j-1]>t[i][j]){
          if (t[i-1][j]<t[i][j-1]){
  			/swap(t[i][j--],t[i][j-1]);
          }else{
              /swap(t[i--][j],t[i-1][j]);
          }
      }
  }
  //d.利用young矩阵O(n^3)排序。
  part c+part b
  //O(m+n)判断给定数字是否存在矩阵中。
  //先找行再找列    
  ```

  

### 快速排序

```java
int partition(int[] A,int p,int r){
    int x=A[r];//this can be produced randomlly
    int i=p;
    for (int j=p;j<r;j++){
        if (A[j]<=x){
            swap(A,i++,j);
        }//another way: use one from left and another from right towards mid position.
    }
    swap(A,i,r);
    return i;
}
void quick_sort(int[] A,int p,int r){
    if (p<r){//
		int q=partition(A,p,r);
        quick_sort(A,p,q-1);
        quick_sort(A,q+1,r);
    }
}
```

`模糊排序`？？？

### 线性时间排序

#### 计数排序

```java
void counting_sort(int[] A,int[] B,int k){
   	int[] C=new int[k];
    for (int i=0;i<A.length;i++){
        C[A[j]]++;
    }
    for (int i=1;i<k;i++){
        C[i]+=C[i-1];//C[i]表示<=i的数目
    }
    for (int i=A.length-1;i>=0;i--){
        B[C[A[j]]]=A[j];
        C[A[j]]--;
    }
}
```

#### 基数排序

```java
void radix_sort(int[] A,int d){//d位数字
	for (int i=1;i<=d;i++){
        //use stable sort to sort array A on digit i;
    }
}
```

#### 桶排序

```java
//桶排序适用于输入数据服从均匀分布，基本步骤：
//划分为k个大小相同的区间
//n*A[i] 落在相应的区间中，对区间内的元素排序
//合并
```

### 中位数和顺序统计量

#### 最小值和最大值

```cpp
//线性扫描可以分别获得最大值和最小值
//也可以同时获得最大与最小值，只需要3[n/2]
pair<int,int> int minmax(int[] A){
    
}
```

#### 线性选择算法

```java
int select(int[] A, int k){
    
}
```


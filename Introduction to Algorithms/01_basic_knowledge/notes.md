### 第二章 算法基础

#### 插入排序

```java
void insert_sort(int[] A,int n){//n shows length of used part of A
    for (j=2;j<n;j++){
        int key=A[j];
        i=j-1;
        while (i>0&&A[i]>key){
            A[i+1]=A[i];
            i--;
        }
        A[i+1]=A[i];
    }
}
```

#### 归并排序

```java
void merge(int[] A,int p,int q,int r){//A p..q q+1..r
	int[] B=new int[r-p+1];
    int i=p,j=q+1;
    int id=0;
    while (i<=q&&j<=r){
        if (A[i]<=A[j]){
            B[id]=A[i++];
        }else{
            B[id]=A[j++];
        }
        id++;
    }
    while (i<=q){
        B[id++]=A[i++];
    }
    while (j<=r){
        B[id++]=A[j++];
    }
    for (int k=p;k<=r;k++){
        A[k]=B[k-p];
    }
}
void merge_sort(int[] A,int p,int r){
    if (p<r){
        int q=p+(r-p)/2;
        merge_sort(A,p,q);
        merge_sort(A,q+1,r);
        merge(A,p,q,r);
    }
}
```

### 第三章 函数的增长



### 第四章 分治策略

#### 最大子数组和问题

```java
//Find Max Subarray
//1.O(n^2)
int find_max_subarray(int[] A){
    for (int i=1;i<A.length;i++){
        A[i]+=A[i-1];
    }
    int res=0x80000000;
    for (int j=0;j<A.length;j++){
        for (int k=j+1;k<A.length;k++){
            res=res>(A[j]-A[i]) ? res : A[j]-A[i];
        }
    }
    return res;
}

//2.O(nlogn)--
int find_max_crossing_subarray(int[] A,int low,int high,int mid){
    int sum=0;
    int left_sum,right_sum;
    left_sum=right_sum=0x80000000;
    for (int i=mid;i>=low;i--){
       	sum+=A[i];
        left_sum=sum>left_sum ? sum : left_sum;
    }
    sum=0;
    for (int i=mid+1;i<=right;i++){
        sum+=A[i];
        right_sum=right_sum>sum ? right_sum : sum;
    }
    return left_sum+right_sum
}
int find_max_subarray(int[] A,int low,int high){
    if (low==high) return A[low];
    int mid=low+(high-low)/2;
    return max(
  		find_max_subarray(A,low,mid),
		find_max_subarray(A,mid+1,high),
    	find_max_crossing_subarray(A,low,high,mid)
    );//function max ..
}

//3.O(n) dp--dp[i]=max(dp[i-1]+A[i],A[i])
int find_max_subarray(int[] A){
    int[] dp=new int[A.length];
    dp[0]=A[0];
    for (int i=1;i<A.length;i++){
		dp[i]=(dp[i-1]<0 ? 0 : dp[i-1])+A[i];
    }
   	return dp[A.length-1];
}
```

#### 矩阵乘法的Strassen算法

```java
//C=AxB,O(n^lg7)
/*1.
    A=|A11 A12| B=|B11 B12| C=|C11 C12|
      |A21 A22|   |B21 B22|   |C21 C22|
  2.
    S1=B12-B22
    S2=A11+A12
    S3=A21+A22
    S4=B21-B11
    S5=A11+A22
    S6=B11+B22
    S7=A12-A22
    S8=B21+B22
    S9=A11-A21
    S10=B11+B12
  3.
    P1=A11xS1
    P2=S2xB22
    P3=S3xB11
    P4=A22xS4
    P5=S5xS6
    P6=S7xS8
    P7=S9xS10
  4.
    C11=P5+P4-P2+P6
    C12=P1+P2
    C21=P3+P4
    C22=P5+P1-P3-P7
```

#### 递归式的求解问题

##### 代入法

* 猜测
* 归纳证明

##### 递归树猜测

##### 主方法求解递归式

**关于这部分结合习题和[答案](https://github.com/cuiHL/Answer-of-algorithm-introduction-English-)阅读为佳。**

 ### 第五章 概率分析和随机算法

#### 利用Random(0,1)产生Random(a,b)

```c
int random(int a,int b);//O(1)
int arrayToInt(int* A,int n);
int Random(int a,int b){//O(log(b-a))
	if (a==0&&b==1) return random(0,1);
    int n=ceil(log(2,b-a+1));
    int* array=(int*)malloc(sizeof(int)*n);
    while (true){
        for (int i=0;i<n;i++){
        	A[i]=random(0,1);
    	}
    	int ran=arrayToInt(A,n);
    	if (ran>=a&&ran<=b) return ran;
    }
}
```

#### 利用biased_random()输出random(0,1)

```c
int biased_random();//p to output 1 and (1-p) to output 0
int random(){
    int a=0,b=0;
    while (a==b){
    	a=biased_random();
    	b=biased_random();
        if (a>b) return 0;
        if (b>a) return 1;
    }
}
```

#### 随机排列数组

```kotlin
//1.
fun permute_by_sort(A:IntArray){
    val P=IntArray(A.length)
    val n=A.length
    val n3=n*n*n
    for (i in 0 until n){
		P[i]=random(1,n3);
    }
    //use P to sort A 
}

//2.
fun permute_by_sort(A:IntArray){
    val n=A.length
    for (i in 0 until n){
        swap(A,i,random(i,n))
    }
}
```

#### 几个有趣的随机问题

##### 生日悖论

> 一个屋子里人数达到多少，才能使其中两个人生日相同的机会达到50%？

##### 球与箱子

> 将相同的球随机投到b个箱子里，每次投球独立，球等可能落在每一个箱子里。
>
> 需要头多少个球才能使每个箱子中至少有一个球？

##### 特征序列

> 假设投一枚标准的硬币n次，最长连续正面的序列的期望长度有多长？

##### 在线雇用问题

> 在面试k个应聘者之后，只要出现比之前的几个人都要合适的就雇用，k与应聘者数量n的关系最好为多少？
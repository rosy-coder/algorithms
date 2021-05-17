### 第十章 基本数据结构

```java
class Stack<T>{
    private T[] st;
    private int top;
    public Stack(int capacity){
		st=new int[capacity];
        top=0;
    }
    public boolean isEmpty(){
        return top==0;
    }
    public void push(T t){
		s[top++]=t;
    }
    public T pop(){
        if (isEmpty()) throw //...
        return s[top--];
    }
}

class Queue<T>{
    private T[] qu;//泛型数组好像会有点问题...此处应用Object[]
    private int tail;
    private int head;
    public Queue(int capacity){
        qu=new int[capacity];
        head=0;
        tail=0;
    }
    public isEmpty(){
        return tail==head;
    }
    public void enqueue(T t){
        if ((tail+1)%qu.length==head){
            throw //..
        }
        qu[tail]=t;
        tail=(tail+1)%qu.length;
    }
    public T dequeue(T t){
        if ;//..
        return qu[head++];//..
    }
}

class List<T>{
    
}

class TreeNode<T>{
   	private T value;
    private TreeNode left;
    private TreeNode right;
    public TreeNode(T v){this.value=v;}
}
```

### 第十一章 散列表

```java
//散列表
//寻址

//重写hashCode()和equals()方法
//因为java泛型是根据链接法解决冲突的，所以hashCode相同时需要看equals寻找
//cpp STL 自定义方法：
/*
template<
    class Key,
    class T,
    class Hash = std::hash<Key>,
    class KeyEqual = std::equal_to<Key>,
    class Allocator = std::allocator< std::pair<const Key, T> >
> class unordered_map;
/*
//因此我们需要自定义hash和keyEqual类
```

[cpp自定义hash](https://blog.csdn.net/y109y/article/details/82669620)

###  第十二章 二叉搜索树

```
//中序按顺序的搜索树来就是二叉搜索树
```

###  第十三章 红黑树

红黑树是指满足下面五个条件的树：

1. 结点非黑即红
2. 根结点为黑色
3. 叶结点为黑色(NIL)
4. 不存在两个连续的父子结点为红色，即红色结点的儿子和父亲一定为黑色。
5. 根节点到叶子结点的路径上，黑色结点的数目相同。(黑平衡)



红黑树与2-3-4树是等价的，具体参照 [码炫课堂](https://www.bilibili.com/video/BV135411h7wJ?p=4) 课程。

#### 旋转

旋转操作分为左旋与右旋，是插入与删除调用的操作。

```java
/**
        Y                     X
	  /	  \       right     /   \
    X      c     ------>   a      Y 
   /   \         <------         / \
  a     b         left          b   c 
*/
void leftRotate(RBNode x){
    
}
void rightRotate(RBNode x){
    
}
```

#### 插入

```java
//插入结点规定为红结点，如果为黑节点那么就回归到原始的二叉搜索树
void insert(RBNode x){
    //find x's parent
    //fixup
}
void insertFixup(RBNode x){
    //while loop when x.parent.color=RED 
    //case 1: x'uncle is red
    //case 2: x'uncle is black and x is a right child
    //case 3: x'uncle is black and x is a left child
}
```

#### 删除

```java
//delete  --- for predecessor and successor

/**
	A. 删除的是叶子节点且该叶子节点是红色的 ---> 无需修复，因为它不会破坏红黑树的5个特性

    B. 删除的是叶子节点且该叶子节点是黑色的 ---> 很明显会破坏特性5，需要修复。

    C. 删除的节点（为了便于叙述我们将其称为P）下面有一个子节点 S，对于这种情况我们通过 将P和S的值交换的方式，巧妙的将删除P变为删除S，S是叶子节点，这样C这种情况就会转 换为A, B这两种情况：

        C1： P为黑色，S为红色 ---> 对应 A 这种情况

        C2: P为黑色或红色，S为黑色 --- > 对应 B 这种情况

    D. 删除的节点有两个子节点，对于这种情况，我们通过将P和它的后继节点N的值交换的方 式，将删除节点P转换为删除后继节点N，而后继节点只可能是以下两种情况：

        D1: N是叶子节点 --- > 对应情况 A 或 B

        D2: N有一个子节点 ---- > 对应情况 C
*/

//因此我们只需要讨论删除结点为叶子结点且该结点为黑色的情况的修复
/**
	只考虑待删除结点 x 在左侧的情况，右侧同理
	case 1: x 的兄弟结点为黑色，有一个右结点 <--推导可得该结点一定为红
		最优的解决方案为: 中间结点跟随父结点的颜色，另外两个结点置黑。
	case 2: x 的兄弟结点为黑色，有一个左结点
		最优的解决方案为: 中间结点跟随父结点的颜色，另外两个结点置黑。
		具体操作为 右旋 兄弟结点 ， =》 case 1
	case 3: x 的兄弟结点为黑色，有两个子结点
		·兄弟结点跟随父结点颜色
		·父结点与右孩子置黑
		·左孩子已经为红
		·对父结点左旋
	case 4: x 的兄弟结点为黑色，无子结点
		此时高度不可避免的减少，将兄弟结点改为红色，递归处理父结点，直到根节点或者遇到红色结点
	case 5: x 的兄弟结点为红色，有两个黑色子结点 <--推导可得这是唯一一种红结点的情况
		·兄弟结点跟随父结点颜色
		·父结点与右孩子置黑
		·左孩子置红
		·对父结点左旋
*/
void delete(RBNode x){
    //find x
    //delete x
    //fix x
}
```




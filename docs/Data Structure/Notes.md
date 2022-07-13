## Ch 1

1. 数据元素是数据的基本单位, 数据项是不可分割的最小单位。 构成数据元素的 最小单位是数据项 。 常作为一个整体来进行处理。 数据元素通常由若干个 数据项 组成，数据项具有原子性，是不可分割的最小单位。

2. 数据的逻辑结构是指数据的数据元素之间的逻辑关系；
   数据元素是数据的基本单位；
   数据项是组成数据元素、有独立含义的、不可分割的最小单位；
   数据对象是性质相同的数据元素的集合，是数据的一个子集
   
3. 数据结构概念包括数据之间的逻辑结构、数据在计算机中的存储方式和数据的运算三个方面。

4. 数据结构从逻辑上可分为两大类： **线性结构和非线性结构** 。

5. 数据结构的逻辑结构独立于其存储结构，数据结构的存储结构不能说它是独立于该数据结构的逻辑结构的
    存储结构: 数据的逻辑结构用计算机语言的实现
    顺序存储，比如数组实现的线性表，则存储结构和逻辑结构是一致的；
    链式存储，同样是实现线性表，其逻辑连续的节点在空间上是独立的；
    而对于树、图等非线性的数据结构，其逻辑和储存一定是独立的。

6. 数据结构相同，逻辑结构要相同，但逻辑结构的实现可以用不同的存储结构；

7. 以下属于逻辑结构的是（ C） A. 顺序表 B. 散列表 C. 有序表 D. 单链表

   顺序表、哈希表和单链表表示几种数据结构，既描述逻辑结构，也描述存储结构和数据运算，而有序表是指关键字有序的线性表，可以链式存储也可以顺序存储，仅描述了元素之间的逻辑关系，故它属于逻辑结构。

   因为A顺序表已经表示了他是按照顺序存储的方式存储，B哈希表使用散列存储，D单链表表明是链式存储。这三个选项都是根据它的物理存储方式命名的，所以都属于存储结构，或是物理结构。
   只有C有序表，既可以用链式存储又可以用顺序存储，所以只是一种逻辑上的有序而不是实际存储的方式。

8. 算法的时间复杂度取决于待处理数据的状态以及问题的规模

## Assignment 2

1. ![image-20211009171215383](https://ohg-typora-image.oss-cn-hangzhou.aliyuncs.com/imgs/imgs/2021/10/202110091712566.png)

2. <img src="https://ohg-typora-image.oss-cn-hangzhou.aliyuncs.com/imgs/imgs/2021/10/202110091723180.png" alt="image-20211009172310121" style="zoom:50%;" />

3. <img src="https://ohg-typora-image.oss-cn-hangzhou.aliyuncs.com/imgs/imgs/2021/10/202110091748282.png" alt="image-20211009174829187" style="zoom:150%;" />

4. ![image-20211009175101466](https://ohg-typora-image.oss-cn-hangzhou.aliyuncs.com/imgs/imgs/2021/10/202110091751534.png)

## Assignment 3

1. <img src="https://ohg-typora-image.oss-cn-hangzhou.aliyuncs.com/imgs/imgs/2021/10/202110141639524.png" alt="image-20211014163909350" style="zoom:80%;" />

2. ![image-20211014164716829](https://ohg-typora-image.oss-cn-hangzhou.aliyuncs.com/imgs/imgs/2021/10/202110141647900.png)

   循环队列会损失一个空间

3. ![image-20211111150656040](https://ohg-typora-image.oss-cn-hangzhou.aliyuncs.com/imgs/imgs/2021/11/202111111507220.png)

## Assignment 4

1. ![image-20211114114623526](https://ohg-typora-image.oss-cn-hangzhou.aliyuncs.com/imgs/imgs/2021/11/202111141146618.png)

2. ![image-20211114144854602](https://ohg-typora-image.oss-cn-hangzhou.aliyuncs.com/imgs/imgs/2021/11/202111141448662.png)

3. ![image-20211114150551469](https://ohg-typora-image.oss-cn-hangzhou.aliyuncs.com/imgs/imgs/2021/11/202111141505550.png)![image-20211114150722630](https://ohg-typora-image.oss-cn-hangzhou.aliyuncs.com/imgs/imgs/2021/11/202111141507680.png)

4. 线索二叉树

   ![image-20211114151822752](https://ohg-typora-image.oss-cn-hangzhou.aliyuncs.com/imgs/imgs/2021/11/202111141518814.png)![image-20211114151900185](https://ohg-typora-image.oss-cn-hangzhou.aliyuncs.com/imgs/imgs/2021/11/202111141519229.png)

## Exp 4.3

下列代码的功能是计算给定二叉树`T`的宽度。二叉树的宽度是指各层结点数的最大值。函数`Queue_rear`和`Queue_front`分别返回当前队列`Q`中队尾和队首元素的位置。

```c++
typedef struct TreeNode *BinTree;
struct TreeNode
{
   int Key;
   BinTree  Left;
   BinTree  Right;
};

int Width( BinTree T )
{
   BinTree  p;
   Queue Q;
   int Last, temp_width, max_width;

   temp_width = max_width = 0;
   Q = CreateQueue(MaxElements);
   Last = Queue_rear(Q);
   if ( T == NULL) return 0;
   else {
      Enqueue(T, Q);
      while (!IsEmpty(Q)) {
         p = Front_Dequeue(Q); 
         ++temp_width; // 3分
         if ( p->Left != NULL )  Enqueue(p->Left, Q);
         if ( p->Right != NULL )  Enqueue(p->Right, Q); // 3分;  
         if ( Queue_front(Q) > Last ) {
            Last = Queue_rear(Q);
            if ( temp_width > max_width ) max_width = temp_width;
            temp_width = 0; // 3分;
         } /* end-if */
      } /* end-while */
      return  max_width;
   } /* end-else */
} 
```

## Exam 链表

```c
#include <stdio.h>
void func(int *p) {
	p = p + 1;
}
int main() {
	int *p = (int *)malloc(20 * sizeof(int));
	for (int i = 0; i < 20; i++)
		p[i] = i + 1;
	printf("%d\n", *p);
	func(p);
	printf("%d\n", *p);
}
```

运行结果为

```
1
1
```

注意！函数里不会改变原有指针！

## Ch 9 查找

Map

- 顺序查找 $O(n)$，若每个记录的查找概率相等则 $ASL = \dfrac{n+1}{2}$
- 二分查找 $O(\log_2(n))$，若每个记录查找概率相等则 $ASL = \dfrac{n + 1}{n}\log_2(n + 1) - 1$
- 分块查找
- 动态查找 - 二叉排序树 BST
  - 新插入结点一定是新添加的叶子结点，是查找不成功时查找路径上访问的最后一个结点的左孩子或右孩子、
  - 中序遍历二叉排序树可得到一个关键字有序的序列
  - **AVL树**是最先发明的自平衡二叉查找树。在AVL树中任何节点的两个子树的高度最大差别为1，所以它也被称为**高度平衡树**。
  - 删除：分情况讨论 - 要删除的节点是叶子/只有左子树或只有右子树/有左子树也有右子树(2种删的方式)
  - ![image-20220103134733595](https://ohg-typora-image.oss-cn-hangzhou.aliyuncs.com/imgs/imgs/2022/01/202201031347708.png)
  - ![image-20220103134753739](https://ohg-typora-image.oss-cn-hangzhou.aliyuncs.com/imgs/imgs/2022/01/202201031347821.png)
  - ![image-20220103134807132](https://ohg-typora-image.oss-cn-hangzhou.aliyuncs.com/imgs/imgs/2022/01/202201031348219.png)
  - ![image-20220103134822089](https://ohg-typora-image.oss-cn-hangzhou.aliyuncs.com/imgs/imgs/2022/01/202201031348185.png)

- Hash Map

## Assignment 6

- 二叉搜索树的查找和折半查找(二分)的时间复杂度相同 F

- ![image-20220103140828206](https://ohg-typora-image.oss-cn-hangzhou.aliyuncs.com/imgs/imgs/2022/01/202201031408270.png)

- **折半查找判定树**：向上取整也可以向下取整，但是整个构造过程中必须选择且只选择一种，如果两种方式同时进行就是错的。
- ![image-20220103143042768](https://ohg-typora-image.oss-cn-hangzhou.aliyuncs.com/imgs/imgs/2022/01/202201031430451.png)

- ![image-20220103143405255](https://ohg-typora-image.oss-cn-hangzhou.aliyuncs.com/imgs/imgs/2022/01/202201031434357.png)

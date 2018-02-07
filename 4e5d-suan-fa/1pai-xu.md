时间复杂度是指执行算法所需要的计算工作量；而空间复杂度是指执行这个算法所需要的内存空间。

复杂度用大O符号表示。

一.冒泡排序\(Bubble Sort\)

效率低，简单

```
#include <stdio.h>

// 分类 -------------- 内部比较排序
// 数据结构 ---------- 数组
// 最差时间复杂度 ---- O(n^2)
// 最优时间复杂度 ---- 如果能在内部循环第一次运行时,使用一个旗标来表示有无需要交换的可能,可以把最优时间复杂度降低到O(n)
// 平均时间复杂度 ---- O(n^2)
// 所需辅助空间 ------ O(1)
// 稳定性 ------------ 稳定

void Swap(int A[], int i, int j)
{
    int temp = A[i];
    A[i] = A[j];
    A[j] = temp;
}

void BubbleSort(int A[], int n)
{
    for (int j = 0; j < n - 1; j++)         // 每次最大元素就像气泡一样"浮"到数组的最后
    {
        for (int i = 0; i < n - 1 - j; i++) // 依次比较相邻的两个元素,使较大的那个向后移
        {
            if (A[i] > A[i + 1])            // 如果条件改成A[i] >= A[i + 1],则变为不稳定的排序算法
            {
                Swap(A, i, i + 1);
            }
        }
    }
}

int main()
{
    int A[] = { 6, 5, 13, 1, 8};    // 从小到大冒泡排序
    int n = sizeof(A) / sizeof(int);
    BubbleSort(A, n);
    printf("冒泡排序结果：");
    for (int i = 0; i < n; i++)
    {
        printf("%d ", A[i]);
    }
    printf("\n");
    return 0;
}
```

二.快速排序\(Quick Sort\)

高效，时间复杂度为nlogn。log以2为底n的对数可参考2分法查找， n = 2^k, 那么k=logn

快速排序。在平均状况下，排序n个元素要O\(nlogn\)次比较。在最坏状况下则需要O\(n^2\)次比较，但这种状况并不常见。事实上，快速排序通常明显比其他O\(nlogn\)算法更快，因为它的内部循环可以在大部分的架构上很有效率地被实现出来。

快速排序使用分治策略\(Divide and Conquer\)来把一个序列分为两个子序列。步骤为：

1.从序列中挑出一个元素，作为"基准"\(pivot\).

2.把所有比基准值小的元素放在基准前面，所有比基准值大的元素放在基准的后面（相同的数可以到任一边），这个称为分区\(partition\)操作。

3.对每个分区递归地进行步骤1~2，递归的结束条件是序列的大小是0或1，这时整体已经被排好序了。

快速排序的代码如下：

```
#include <stdio.h>

// 分类 ------------ 内部比较排序
// 数据结构 --------- 数组
// 最差时间复杂度 ---- 每次选取的基准都是最大（或最小）的元素，导致每次只划分出了一个分区，需要进行n-1次划分才能结束递归，时间复杂度为O(n^2)
// 最优时间复杂度 ---- 每次选取的基准都是中位数，这样每次都均匀的划分出两个分区，只需要logn次划分就能结束递归，时间复杂度为O(nlogn)
// 平均时间复杂度 ---- O(nlogn)
// 所需辅助空间 ------ 主要是递归造成的栈空间的使用(用来保存left和right等局部变量)，取决于递归树的深度，一般为O(logn)，最差为O(n)       
// 稳定性 ---------- 不稳定

void Swap(int A[], int i, int j)
{
    int temp = A[i];
    A[i] = A[j];
    A[j] = temp;
}

int Partition(int A[], int left, int right)  // 划分函数
{
    int pivot = A[right];               // 这里每次都选择最后一个元素作为基准
    int tail = left - 1;                // tail为小于基准的子数组最后一个元素的索引
    for (int i = left; i < right; i++)  // 遍历基准以外的其他元素
    {
        if (A[i] <= pivot)              // 把小于等于基准的元素放到前一个子数组末尾
        {
            Swap(A, ++tail, i);
        }
    }
    Swap(A, tail + 1, right);           // 最后把基准放到前一个子数组的后边，剩下的子数组既是大于基准的子数组
                                        // 该操作很有可能把后面元素的稳定性打乱，所以快速排序是不稳定的排序算法
    return tail + 1;                    // 返回基准的索引
}

void QuickSort(int A[], int left, int right)
{
    if (left >= right)
        return;
    int pivot_index = Partition(A, left, right); // 基准的索引
    QuickSort(A, left, pivot_index - 1);
    QuickSort(A, pivot_index + 1, right);
}

int main()
{
    int A[] = { 5, 2, 9, 4, 7, 6, 1, 3, 8 }; // 从小到大快速排序
    int n = sizeof(A) / sizeof(int);
    QuickSort(A, 0, n - 1);
    printf("快速排序结果：");
    for (int i = 0; i < n; i++)
    {
        printf("%d ", A[i]);
    }
    printf("\n");
    return 0;
}
```



关于时间复杂度

二分查找法 时间复杂度

![](/assets/2.jpg)

十进制转2进制 时间复杂度

n = 2^k  则k = logn![](/assets/1.jpg)


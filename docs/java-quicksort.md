# 快速排序算法的 Java 实现

> 原文：<https://web.archive.org/web/20220930061024/https://www.baeldung.com/java-quicksort>

## 1。概述

在本教程中，我们将详细探讨快速排序算法，重点是它的 Java 实现。

我们也将讨论它的优点和缺点，然后分析它的时间复杂度。

## 2。快速排序算法

**[快速排序](/web/20220806123045/https://www.baeldung.com/cs/the-quicksort-algorithm)是一种排序算法，它利用了[分治原则](/web/20220806123045/https://www.baeldung.com/cs/divide-and-conquer-strategy)。它具有平均的复杂度，是最常用的排序算法之一，尤其是对于大数据量。**

记住快速排序不是一个稳定的算法，这一点很重要。稳定排序算法是指具有相同值的元素在排序后的输出中出现的顺序与它们在输入列表中出现的顺序相同的算法。

输入列表被称为 pivot 的元素分成两个子列表；一个子列表中的元素少于轴心，另一个子列表中的元素多于轴心。对每个子列表重复这个过程。

最后，所有排序后的子列表合并形成最终输出。

### 2.1。算法步骤

1.  我们从列表中选择一个元素，称为轴心。我们将使用它将列表分成两个子列表。
2.  我们将围绕轴心的所有元素重新排序——值较小的元素放在它的前面，比轴心大的元素放在它的后面。在这一步之后，枢轴处于其最终位置。这是重要的分区步骤。
3.  我们递归地将上述步骤应用于中枢左侧和右侧的子列表。

正如我们所见， **quicksort 自然是一种递归算法，就像每一种分而治之的方法一样。**

为了更好地理解这个算法，我们举一个简单的例子。

```
Arr[] = {5, 9, 4, 6, 5, 3}
```

1.  为了简单起见，假设我们选择 5 作为支点
2.  我们首先将所有小于 5 的元素放在数组的第一个位置:{3，4，5，6，5，9}
3.  然后，我们将对左侧子数组{3，4}重复此操作，以 3 为轴心
4.  没有少于 3 的元素
5.  我们对透视右侧的子数组应用快速排序，即{4}
6.  这个子数组只包含一个排序元素
7.  我们继续原始数组的右边部分{6，5，9}，直到得到最终的有序数组

### 2.2。选择最佳支点

快速排序的关键是选择最佳支点。当然，中间的元素是最好的，因为它将列表分成两个相等的子列表。

但是从一个无序列表中找到中间的元素是困难和耗时的，这就是为什么我们把第一个元素、最后一个元素、中间值或任何其他随机元素作为支点。

## 3。用 Java 实现

第一个方法是`quickSort()`，它将待排序的数组、第一个和最后一个索引作为参数。首先，我们检查索引，只有在仍有元素需要排序时才继续。

我们获取排序后的 pivot 的索引，并使用它递归地调用与`quickSort()`方法具有相同参数但具有不同索引的`partition()`方法:

```
public void quickSort(int arr[], int begin, int end) {
    if (begin < end) {
        int partitionIndex = partition(arr, begin, end);

        quickSort(arr, begin, partitionIndex-1);
        quickSort(arr, partitionIndex+1, end);
    }
}
```

让我们继续使用`partition()`方法。为了简单起见，这个函数以最后一个元素作为支点。然后，检查每个元素，如果其值较小，则在透视之前交换它。

在划分结束时，所有小于枢轴的元素都在它的左边，所有大于枢轴的元素都在它的右边。透视位于其最终排序位置，函数返回该位置:

```
private int partition(int arr[], int begin, int end) {
    int pivot = arr[end];
    int i = (begin-1);

    for (int j = begin; j < end; j++) {
        if (arr[j] <= pivot) {
            i++;

            int swapTemp = arr[i];
            arr[i] = arr[j];
            arr[j] = swapTemp;
        }
    }

    int swapTemp = arr[i+1];
    arr[i+1] = arr[end];
    arr[end] = swapTemp;

    return i+1;
}
```

## 4。算法分析

### 4.1。时间复杂度

在最好的情况下，算法会将列表分成两个大小相等的子列表。因此，完整的`n`大小的列表的第一次迭代需要`O(n)`。用`n/2`元素对剩余的两个子列表进行排序各需要`2*O(n/2)` 。因此，快速排序算法具有`O(n log n)`的复杂性。

最坏的情况下，算法每次迭代只会选择一个元素，所以`O(n) + O(n-1) + … + O(1)`，等于 `O(n²)`。

一般来说，QuickSort 具有`O(n log n)`复杂性，这使得它适合大数据量。

### 4.2。快速排序 vs 合并排序

让我们讨论一下在哪些情况下我们应该选择 QuickSort 而不是 MergeSort。

尽管快速排序和合并排序的平均时间复杂度都是`O(n log n)`，但快速排序是首选算法，因为它的空间复杂度是`O(log(n))`。另一方面，Mergesort 需要`O(n)`额外的存储，这使得它对于数组来说相当昂贵。

快速排序需要访问不同的索引来进行操作，但是这种访问在链表中是不能直接实现的，因为没有连续的块；因此，要访问一个元素，我们必须从链表的开始遍历每个节点。此外，合并排序的实现不需要额外的空间给`LinkedLists.`

在这种情况下，增加快速排序和合并排序的开销通常是首选。

## 5。结论

快速排序是一种优雅的排序算法，在大多数情况下非常有用。

**一般是一个“就地”算法，平均时间复杂度为`O(n log n).`**

另一个要提到的有趣的点是，Java 的`Arrays.sort()`方法使用 Quicksort 对原语数组进行排序。该实现使用了两个支点，并且比我们的简单解决方案执行得更好，这就是为什么对于生产代码来说，使用库方法通常更好。

和往常一样，实现这个算法的代码可以在我们的 GitHub 库的[中找到。](https://web.archive.org/web/20220806123045/https://github.com/eugenp/tutorials/tree/master/algorithms-modules/algorithms-sorting)
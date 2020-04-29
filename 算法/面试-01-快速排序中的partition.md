
## 分析

实现快速排序算法的关键在于：partition

即先在数组中选择一个数字，把数组分为两部分，比选择的数字小的数字移到数组左边，比选择的数字大的数字移到数组的右边。

```javascript

function partition(data, length, start, end) {
  
}

```

## 实现快速排序

利用递归和partition实现。

```javascript

function qucikSort(data, length, start, end) {
  if(start === end) {
    return;
  }
  
  let index = Partition(data, length, start, end);
  
  if(index > start) {
    QuickSort(data, length, start, index-1);
  }
  
  if(index < end) {
    QuickSort(data, length, index+1, end);
  }
}

```

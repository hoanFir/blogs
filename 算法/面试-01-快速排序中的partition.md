
## 分析

实现快速排序算法的关键在于：partition

即先在数组中选择一个数字，把数组分为两部分，比选择的数字小的数字移到数组左边，比选择的数字大的数字移到数组的右边。

```javascript

function partition(data, length, start, end) {
  if(data === null || length  <= 0 || start >= length) {
    return;
  }
  
  /* 生成一个在start和end之间对随机数，并放在数组最后 */
  let index = RandomInRange(start, end);
  Swap(data[index], data[end]);
  
  let small = start-1;
  for(index = start; index < end; index++) {
    if(data[index] < data[end]) {
      small++;
      if(small != index) {
        Swap(data[index], data[small]);
      }
    }
  }
  
  small++;
  Swap(data[small], data[end]);
  
  return samll;
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

quickSort(data, 0, data.length-1);

```

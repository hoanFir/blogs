
## 题目

```

判断是否为平衡二叉树。如果某二叉树中任意节点的左、右子树的深度相差不超过1，那么它就是一棵平衡二叉树。

```

## 分析

首先，我们知道求树的深度，有如下解法：

```c

int TreeDeepth(BinaryTreeNode* pTreeRoot) {
  if(!pTreeRoot) {
    return 0;
  }
  
  int nLeft = TreeDeepth(pTreeRoot->pLeft);
  int nLeft = TreeDeepth(pTreeRoot->pLeft);
  
  return (nLeft > nRight) ? (nLeft + 1) : (nRight+1);
}

```

因此，在遍历树的每个节点时，调用函数 `TreeDeepth` 比较左右子树深度，如果相差不超过1，就是一棵平衡二叉树。

## 解法一

```c

int TreeDeepth(BinaryTreeNode* pTreeRoot) {
  if(!pTreeRoot) {
    return 0;
  }
  
  int nLeft = TreeDeepth(pTreeRoot->pLeft);
  int nLeft = TreeDeepth(pTreeRoot->pLeft);
  
  return (nLeft > nRight) ? (nLeft + 1) : (nRight+1);
}

bool IsBalanced(BinaryTreeNode* pTreeRoot) {
    if(!pTreeRoot) {
    return true;
  }
  
  int left = TreeDeepth(pTreeRoot->pLeft);
  int right = TreeDeepth(pTreeRoot->pRight);
  
  int diff = left - right;
  if(diff > 1 || diff < -1) {
    return false;
  }
  
  return IsBalanced(pTreeRoot->pLeft) && IsBalanced(pTreeRoot->pRight)
}

```

上述解法

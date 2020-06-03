
## 题目

> 判断是否为平衡二叉树。如果某二叉树中任意节点的左、右子树的深度相差不超过1，那么它就是一棵平衡二叉树。



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

上述解法虽然简洁，但是时间效率不高。因为，每次往下遍历子树进行判断时，会重复遍历。

## 解法二

使用后序遍历的方式，那么在遍历到每个节点时，就已遍历了它的左、右子树。只要在遍历每个节点时记录其深度，这样就可以一边从下往上遍历一边判断每个节点是不是平衡的。这种思路其实就是斐波那契数列求第n项从下往上解法。

```c

bool IsBalanced(BinaryTreeNode* pTreeRoot, int* pDepth) {
  if(!pTreeRoot) {
    *pDepth = 0;
    return true;
  }
  
  int left, right;
  
  if(IsBalanced(pTreeRoot->pLeft) && IsBalanced(pTreeRoot->pRight)) {
    int diff = left - right;
    if(diff <= 1 && diff >= -1) {
      *pDepth = 1 + (left > right ? left : right);
      return true;
    }
  }
  
  return false;
}

BinaryTreeNode *pTreeRoot;
int depth = 0;
bool isBalanced = IsBalanced(pTreeRoot, &depth);

```


---
pubDatetime: 2025-06-28T10:14:29
title: Recursion
slug: recursion
featured: false
draft: false
tags:
  - algorithm
  - code interview
  - recursion
description:
  Summary of recursion. 
---

# Recursion

## Table of Contents

## Implementation

- what can the function do
  - function sigature: input and output
  - meaning of function
- base case
- subproblem
- recursive rule

## Recursion Tree

Each node in the recursion represents a recursion call.
The times that a recursive function calls itself is the number of branches of this node.

### time and space complexity

TC: O(branch ^ level)
// the total TC of all nodes in the recursion tree
For binary recursion tree, the number of nodes on the bottom level dominates the total TC, i.e. O(n)

SC: O(height)
// the total SC of all nodes on the longest single path

Some problems can have top-half and bottom-half recursion trees (e.g. merge sort)
top-half accounts for partitioning, bottom-half accounts for merging
TC: O(n) + O(nlogn) = O(nlogn)
SC: O(logn) + O(n) = O(n)

## Use Case and Practice

### DFS backtracking

Find all possible

#### n queens

Get all valid ways of putting N Queens on an N * N chessboard so that no two Queens threaten each other.

Assumptions

    N > 0

Return

    A list of ways of putting the N Queens
    Each way is represented by a list of the Queen's y index for x indices of 0 to (N - 1)

```cpp
public class Solution {
  public List<List<Integer>> nqueens(int n) {
    // Write your solution here
    List<List<Integer>> ans = new ArrayList<>();
    dfs(n, 0, new ArrayList<Integer>(), ans);
    return ans;
  }

  private void dfs(int n, int index, List<Integer> board, List<List<Integer>> ans) {
    if (index == n) {
      ans.add(new ArrayList<>(board));
      return;
    }
    for (int i = 0; i < n; i++) {
      if (isValid(board, index, i)) {
        board.add(i);
        dfs(n, index + 1, board, ans);
        board.remove(index);
      }
    }
  }

  private boolean isValid(List<Integer> board, int index, int i) {
    for (int j = 0; j < index; j++) {
      if (i == board.get(j)) {
        return false;
      }
      if (Math.abs(index - j) == Math.abs(i - board.get(j))) {
        return false;
      }
    }
    return true;
  }
}
```

#### reverse linked list

Reverse pairs of elements in a singly-linked list.

```cpp
public class Solution {
  public ListNode reverseInPairs(ListNode head) {
    // Write your solution here
    if (head == null || head.next == null) {
      return head;
    }
    ListNode third = head.next.next;
    ListNode second = head.next;
    second.next = head;
    head.next = reverseInPairs(third);
    return second;
  }
}
```

#### lowest common ancestor I (LCA)

Given two nodes in a binary tree, find their lowest common ancestor.

Assumptions

    There is no parent pointer for the nodes in the binary tree

    The given two nodes are guaranteed to be in the binary tree

```cpp
public class Solution {
  public TreeNode lowestCommonAncestor(TreeNode root, TreeNode one, TreeNode two) {
    // Write your solution here.
    if (root == null) {
      return null;
    }
    if (root == one || root == two) {
      return root;
    }
    TreeNode left = lowestCommonAncestor(root.left, one, two);
    TreeNode right = lowestCommonAncestor(root.right, one, two);
    if (left != null && right == null) {
      return left;
    } else if (left == null && right != null) {
      return right;
    } else if (left != null && right != null) {
      return root;
    } else {
      return null;
    }
  }
}
```

#### maximum path sum binary tree II

Given a binary tree in which each node contains an integer number. Find the maximum possible sum from any node to any node (the start node and the end node can be the same). 

Assumptions

    The root of the given binary tree is not null

```cpp
public class Solution {
  public int maxPathSum(TreeNode root) {
    // Write your solution here
    int[] globalMax = new int[]{Integer.MIN_VALUE};
    maxSinglePathSum(root, globalMax);
    return globalMax[0];
  }

  private int maxSinglePathSum(TreeNode root, int[] globalMax) {
    if (root == null) {
      return 0;
    }
    int leftMax = maxSinglePathSum(root.left, globalMax);
    int rightMax = maxSinglePathSum(root.right, globalMax);
    leftMax = Math.max(leftMax, 0);
    rightMax = Math.max(rightMax, 0);
    globalMax[0] = Math.max(globalMax[0], root.key + leftMax + rightMax);
    return root.key + Math.max(leftMax, rightMax);
  }
}
```

**Notice:** Must use an int array instead of an int here for globalMax. 
Because primitive types (e.g., int) are passed by value, i.e. a copy of the original value; 
but objects (e.g., array) are passed by reference.

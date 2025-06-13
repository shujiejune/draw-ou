---
pubDatetime: 2025-06-12T17:07:23
title: binary Search
slug: binary-search
featured: false
draft: false
tags:
  - algorithm
  - code interview
  - binary search
description:
  Summary of binary search. 
---

# Binary Search

## Table of Contents

## Use Case

Dealing with an **Array**-like structure, **sorted** or partially sorted.

## Method

Core: cut the searching scope.

Steps:
- make clear **left** and **right**
- while loop condition (consider the corner cases)
  - left <= right
  - left < right - 1
- update left and right (do not rule out the target)
  - mid, mid + 1, mid - 1?
- post processing (if any)

Complexity:
- TC = O(logn)
- SC = O(1)

## Practice

### Smallest larger than target

Given a target integer T and an integer array A sorted in ascending order, find the index of the smallest element in A that is larger than T or return -1 if there is no such index.

Assumptions

There can be duplicate elements in the array.

Solutions

```cpp
public int smallestElementLargerThanTarget(int[] array, int target) {
    if (array == null || array.length == 0) {
        return -1;
    }
    int left = 0, right = array.length - 1;
    if (target >= array[right]) {
        return -1;
    }
    while (left < right) {
        int mid = left + (right - left) / 2;
        if (array[mid] <= target) {
            left = mid + 1;
        } else {
            right = mid;
        }
    }
    return left;
}
```

### Closest

Given a target integer T and an integer array A sorted in ascending order, find the index i in A such that A[i] is closest to T.

Assumptions

    There can be duplicate elements in the array, and we can return any of the indices with same value.

Solutions

```cpp
public int closest(int[] array, int target) {
    if (array == null || array.length == 0) {
        return -1;
    }
    int left = 0, right = array.length - 1;
    if (target <= array[left]) {
        return left;
    }
    if (target >= array[right]) {
        return right;
    }
    while (left < right - 1) {
        int mid = left + (right - left) / 2;
        if (array[mid] == target) {
            return mid;
        } else if (array[mid] < target) {
            left = mid;
        } else {
            right = mid;
        }
    }
    if (Math.abs(array[left] - target) < Math.abs(array[right] - target)) {
        return left;
    } else {
        return right;
    }
}
```

### K closest

Given a target integer T, a non-negative integer K and an integer array A sorted in ascending order, find the K closest numbers to T in A. If there is a tie, the smaller elements are always preferred.

Assumptions

    A is not null
    K is guranteed to be >= 0 and K is guranteed to be <= A.length

Return

    A size K integer array containing the K closest numbers(not indices) in A, sorted in ascending order by the difference between the number and T.

Solutions

```cpp
public int[] kClosest(int[] array, int target, int k) {
    int[] ans = new int[k];
    int n = array.length;
    int left = 0, right = n - 1;
    if (target <= array[left]) {
        for (int i = 0; i < k; i++) {
            ans[i] = array[i];
        }
        return ans;
    }
    if (target >= array[right]) {
        for (int i = 0; i < k; i++) {
            ans[i] = array[n - i - 1];
        }
        return ans;
    }
    int curr = 0;
    while (left < right - 1) {
        int mid = left + (right - left) / 2;
        if (array[mid] == target) {
            ans[curr++] = target;
            left = mid - 1;
            right = mid + 1;
            break;
        } else if (array[mid] < target) {
            left = mid;
        } else {
            right = mid;
        }
    }
    while (left >= 0 && right < n && curr < k) {
        if (Math.abs(array[left] - target) <= Math.abs(array[right] - target)) {
            ans[curr++] = array[left--];
        } else {
            ans[curr++] = array[right++];
        }
    }
    if (curr < k) {
        if (left > 0) {
            while (curr < k) {
                ans[curr++] = array[left--];
            }
        } else {
            while (curr < k) {
                ans[curr++] = array[right++];
            }
        }
    }
    return ans;
}
```

**Notice:** Mind the corner case when n == 2. The *if* condition should be (curr < k) rather than (curr < k - 1).

### Search in unknown sized sorted array

Given a integer dictionary A of unknown size, where the numbers in the dictionary are sorted in ascending order, determine if a given target integer T is in the dictionary. Return the index of T in A, return -1 if T is not in A.

Assumptions

    dictionary A is not null
    dictionary.get(i) will return null(Java)/INT_MIN(C++)/None(Python) if index i is out of bounds

Solutions

```cpp
public int search(Dictionary dict, int target) {
    if (target < dict.get(0)) {
        return -1;
    }
    int idx = 1;
    while (dict.get(idx) != null && dict.get(idx) < target) {
        idx *= 2;
    }
    int left = idx / 2, right = idx;
    if (dict.get(right) == null) {
        while (dict.get(right) == null) {
            right--;
        }
    }
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (dict.get(mid) == target) {
            return mid;
        } else if (dict.get(mid) < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    return -1;
}
```

### Search in bitonic array

Search for a target number in a bitonic array, return the index of the target number if found in the array, or return -1.

A bitonic array is a combination of two sequence: the first sequence is a monotonically increasing one and the second sequence is a monotonically decreasing one.

Assumptions:

    The array is not null.

Solutions

```cpp
// clarification: 
// Duplicate number in array? 
// What if find target both in ascending and descending?
public int search(int[] array, int target) {
    int n = array.length;
    if (n == 0) {
        return -1;
    }
    int peek = findPeek(array, n);
    if (array[peek] == target) {
        return peek;
    }
    int left = 0, right = n - 1;
    int leftRes = binarySearch(array, target, left, peek - 1, true);
    int rightRes = binarySearch(array, target, peek + 1, right, false);
    return Math.max(leftRes, rightRes);
}

private int findPeek(int[] array, int n) {
    if (n == 1) {
        return 0;
    }
    int left = 0, right = n - 1;
    while (left < right - 1) {
        int mid = left + (right - left) / 2;
        if (array[mid] < array[mid - 1]) {
            right = mid - 1;
        } else {
            left = mid;
        }
    }
    return array[left] < array[right] ? right : left;
}

private int binarySearch(int[] array, int target, int left, int right, boolean ascend) {
    if (left > right) {
        return -1;
    }
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (array[mid] == target) {
            return mid;
        } else if (array[mid] < target) {
            if (ascend) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        } else {
            if (ascend) {
                right = mid - 1;
            } else {
                left = mid + 1;
            }
        }
    }
    return -1;
}
```

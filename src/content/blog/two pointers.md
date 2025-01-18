---
pubDatetime: 2025-01-18T11:01:54Z
title: Coding Patterns: Two Pointers
slug: two-pointers
featured: false
draft: false
tags:
  - learning
  - coding patterns
description:
  Given an array of sorted numbers and a target sum, find a pair in the array whose sum is equal to the given target. 
---

# Coding Patterns: Two Pointers

## Abstract

**Object**: sorted array / linked-list
**Goal**: find a pair in the array whose sum is equal to the given target
**Method**:
- start with one pointer at the beginning, another pointer at the end.
- at each step, check if the numbers add up to the target sum.
  - if greater than target, decrement the end-ponter;
  - if smaller than target, increment the start-pinter.
**Time Complexity**: O(N)

## Sample Problems

### LeetCode 1: Two Sum (Pair with Target Sum)

Given an array of integers nums and an integer target, return indices of the two numbers such that they add up to target.
**Notice**:
- each input would have exactly one solution
- should not use the same element twice
- if no such pair exists, return[-1, -1]

#### Solution

```cpp
class Solution {
public:
    vector<int> TwoSum(vector<int>& arr, int target) {
        sort(arr.begin(), arr.end());
        int left = 0, right = arr.size() - 1;
        vector<int> ans(2, -1);
        while (left < right) {
            int temp = arr[left] + arr[right];
            if (temp < target) {
                left++;
            } else if (temp > target) {
                right--;
            } else {
                ans[0] = left;
                ans[1] = right;
                return ans;
            }
        }
        return ans;
    }
};
```

#### Analysis

**Time Complexity**: 
if given sorted array, O(N)
otherwise, O(NlogN)
**Space Complexity**:
O(1)

### Find Non-Duplicate Number Instances

Given an array of sorted numbers,
- move all non-duplicate numbers at the beginning of the array **in-place** (the solution should have constant space complexity)
- the non-duplicate numbers should be **sorted**

#### Solution

Keep one pointer for iterating the array, one pointer for placing the next non-duplicate number.

**step-by-step algorithm**:
- initialize a variable *nextNonDuplicate* to 1, to keep track of the position to place the next unique number
- iterate through the array starting from index 1 to the end
- for each number, check if it is different from the one at the position *nextNonDuplicate - 1*
  - if different, copy the current number to position *nextNonDuplicate*
  - increment *nextNonDuplicate* by 1 to the next positionfor a unique number

```cpp
class Solution {
public:
    int moveElements(vector<int>& arr) {
        int nextNonDuplicate = 1;  
        for (int i = 0; i < arr.size(); i++) {
            if (arr[nextNonDuplicate - 1] != arr[i]) {  
                arr[nextNonDuplicate] = arr[i];  
                nextNonDuplicate++;  
            }
        }
        return nextNonDuplicate;  
    }
};
```

#### Analysis

**Time Complexity**:
iterating through the array, O(N)
comparison and assignment at each iteration, O(1)
overall: O(N)
**Space Complexity**:
in-place, O(1)

### Triplet Sum to Zero

Given an array of unsorted numbers, find all unique triplets in it that add up to 0.

#### Solution

```cpp
class Solution {
public:
    vector<vector<int>> searchTriplets(vector<int> &arr) {
        vector<vector<int>> triplets;
        int n = arr.size();
        sort(arr.begin(), arr.end());
        for (int i = 0; i < n - 2; i++) {
            if (i > 0 && arr[i] == arr[i - 1]) {
                continue;
            }
            int target = 0 - arr[i];
            int l = i + 1, r = n - 1;
            while (l < r) {
                int temp = arr[l] + arr[r];
                if (temp < target) {
                    increment(arr, l);
                } else if (temp > target) {
                    decrement(arr, r);
                } else {
                    triplets.push_back(vector<int>{arr[i], arr[l], arr[r]});
                    increment(arr, l);
                    decrement(arr, r);
                }
            }
        }
        return triplets;
    }

    void increment(vector<int> &arr, int &l) {
        int n = arr.size(), temp = arr[l];
        while (l < n && arr[l] == temp) {
            l++;
        }
    }

    void decrement(vector<int> &arr, int &r) {
        int temp = arr[r];
        while (r >= 0 && arr[r] == temp) {
            r--;
        }
    }
}
```
**Notice**:
- static member functions do not have access to the *this* pointer or any non-static members of the class. if defining the *searchTriplets* function as static, then the callee functions *increment* and *decrement* should also be made static.
- there is no need to pass *&arr* when calling functions, because *arr* is already the reference (address) to the vector.
- remember to handle duplicate cases for the main iterating pointer
- there is no need to handle duplicate cases when > or < target
- thus there is no need to handle duplicate cases in helper functions

#### Analysis

**Time Complexity**:
sorting the array: O(NlogN)
main loop through the array: O(N)
pair search for each iteration: O(N)
overall: O(N^2)
**Space Complexity**:
sorting the array: O(N)
triplets storage: O(N^2)
overall: O(N^2)

### Triplet Sum Close to Target

Given an array of unsorted numbers and a target number, find a triplet in the array whose sum is as **close to** the target number as possible, return the sum of the triplet. 
If there are more than one such triplet, return the sum of the triplet with the smallest sum.

#### Solution

```cpp
class Solution {
public:
    int searchTriplet(vector<int>& arr, int targetSum) {
        sort(arr.begin(), arr.end());
        int n = arr.size(), diff = INT_MAX, sum = 0;
        for (int i = 0; i < n - 2; i++) {
            if (i > 0 && arr[i] == arr[i - 1]) { continue; }
            int target = targetSum - arr[i];
            int l = i + 1, r = n - 1;
            while (l < r) {
                int temp = target - arr[l] - arr[r];
                int temp_abs = abs(temp);
                if (temp_abs < diff) {
                    diff = temp_abs;
                    sum = arr[i] + arr[l] + arr[r];
                } else if (temp_abs == diff) {
                    sum = min(sum, arr[i] + arr[l] + arr[r]);
                }
                if (temp > 0) {
                    l++;
                } else if (temp < 0) {
                    r--;
                } else {
                    return targetSum;
                }
            }
        }
        return sum;
    }
}
```
**Notice**:
can use *numeric_limits<int>::max()* instead of *INT_MAX*.

### Triplets with Smaller Sum

Given an array arr of unsorted numbers and a target sum, count all triplets in it such that arr[i] + arr[j] + arr[k] < target where i, j, and k are three different indices. 
Write a function to return the count of such triplets.

#### Solution

```cpp
class Solution{
public:
    int searchTriplets(vector<int> &arr, int target) {
        sort(arr.begin(), arr.end());
        int count = 0, n = arr.size();
        for (int i = 0; i < n - 2; i++) {
            int l = i + 1, r = n - 1;
            while (l < r) {
                int sum = arr[i] + arr[l] + arr[r];
                if (sum < target) {
                    count += r - l;
                    l++;
                } else {
                    r--;
                }
            }
        }
        return count;
    }
}
```

### LeetCode 713: Subarray Product Less Than K

Given an array of integers nums and an integer *k*, return the number of **contiguous subarrays** where the product of all the elements in the subarray is strictly less than *k*.

#### Solution

**step-by-step algorithm**:
- use **two pointers** to represent a **sliding window**
- expand the window with the right pointer and calculate the product
- if the product >= *k*, shrink the window by moving the left pointer
- for every valid window, all subarrays ending at the right pointer are valid

```cpp
class Solution {
public:
    int numSubarrayProductLessThanK(vector<int>& nums, int k) {
        if (k <= 1) {
            return 0;
        }
        int count = 0, n = nums.size();
        int l = 0, prod = 1;
        for (int r = 0; r < n; r++) {
            prod *= nums[r];
            while (prod >= k) {
                prod /= nums[l];
                l++;
            }
            count += r - l + 1;
        }
        return count;
    }
}
```
**Notice**:
if do not handle *k <= 1* corner case, there would be overflow in *prod /= nums[l]*.

#### Analysis

sliding window (using *prod* as a function-wide param, rather than defined inside iteration) controls the time complexity as O(N).

**Time Complexity**:
since *l* is incremented a total number of *n* timesduring the whole array traversal, each element is visited at most twice.
O(N)
**Space Complexity**:
O(1)

### LeetCode 844: Backspace String Compare

Given two strings *s* and *t*, return true if they are equal when both are typed into empty text editors. '#' means a backspace character.
**Notice**:
After backspacing an empty text, the text will continue empty.

#### Solution

**algorithm**:
- iterate through the string **in reverse**
  - if we see a backspace, the next non-backspace char is skipped
  - if a char isn't skipped, it should be compared

```cpp
class Solution {
public:
    bool backspaceCompare(string s, string t) {
        int ns = s.size(), nt = t.size();
        int ps = ns - 1, pt = nt - 1;
        int cts = 0, ctt = 0;
        while (ps >= 0 || pt >= 0) {
            while (ps >= 0) {
                if (s[ps] == '#') {
                    cts++;
                    ps--;
                } else if (cts > 0) {
                    cts--;
                    ps--;
                } else {
                    break;
                }
            }
            while (pt >= 0) {
                if (t[pt] == '#') {
                    ctt++;
                    pt--;
                } else if (ctt > 0) {
                    ctt--;
                    pt--;
                } else {
                    break;
                }
            }
            if (ps >= 0 && pt >= 0 && s[ps] != t[pt]) {
                return false;
            }
            if (ps < 0 != pt < 0) {
                return false;
            }
            ps--;
            pt--;
        }
        return true;
    }
};
```

#### Analysis

**Time Complexity**:
O(M+N)
**Space Complexity**:
O(1)
so it's better than building the result string separately.

### LeetCode 581: Shortest Unsorted Continuous Subarray

Given an integer array *nums*, you need to find one continuous subarray such that if you only sort this subarray in non-decreasing order, then the whole array will be sorted in non-decreasing order.
Return the length of the shortest such subarray.

#### Solution

The idea is to find the correct position of the minimum and maximum elements in the unsorted subarray as the left and right boundary.

*algorithm*:
- traverse from the left end, find the leftmost element when slope falls
- find the minimum element *lb* from this point to the right end
- traverse from the right end, find the rightmost element when slope rises
- find the maximum element *ub* from this point to the left end
- find the left boundary *l*, which is the rightmost element that is no greater than *lb*
- find the right boundary *r*, which is the leftmost element that is no less than *ub*
- elements between *l* and *r* (exclude) is the required subarray

```cpp
class Solution {
public:
    int findUnsortedSubarray(vector<int>& nums) {
        int n = nums.size();
        if (n == 1) {
            return 0;
        }
        int l = 0, r = n - 1;
        while (l < r && nums[l + 1] >= nums[l]) {
            l++;
        }
        if (l == r) {
            return 0;
        }
        int lb = INT_MAX;
        for (int i = l+1; i < n; i++) {
            lb = min(lb, nums[i]);   
        }
        while (r > 0 && nums[r - 1] <= nums[r]) {
            r--;
        }
        int ub = INT_MIN;
        for (int i = r; i >= 0; i--) {
            ub = max(ub, nums[i]);
        }
        l = 0;
        r = n - 1;
        while (l < n && nums[l] <= lb) {
            l++;
        }
        while (r >= 0 && nums[r] >= ub) {
            r--;
        }
        return r - l + 1;
    }
};
```
**Notice**
handle the sorted array case 

#### Analysis
**Time Complexity**:
O(N)
**Space Complexity**
O(1)
the stack method has the same time complexity as using two pointers, but has a space complexity of O(N).

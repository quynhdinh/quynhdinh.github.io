---
layout: post
title: "Sort Indices, Not Values - a Comparator Trick for Preserving Original Indices"
---

This is a quick one. We are now solving two sum problem, where you are given an array of integers and a target integer. You need to find two indices in the array such that their sum is equal to the target integer.

Let's say you want to solve it by sorting the array first, then using two pointers to find the two integers. You sort the array and write the two pointer then you realized that you have to retrieve the original indices but your numbers are now sorted around. You then have to somehow pack the original indices of the integers in the array, so that after sorting you can still retrieve the original indices.
```java
int[] twoSum(int[] nums, int target) {
    int n = nums.length;
    int[][] numberWithIndex = new int[n][2];
    for (int i = 0; i < n; i++) {
        numberWithIndex[i][0] = nums[i];
        numberWithIndex[i][1] = i;
    }
    // sort by num
    Arrays.sort(numberWithIndex, (a, b) -> Integer.compare(a[0], b[0]));
    int left = 0, right = n - 1;
    while (left < right) {
        int sum = numberWithIndex[left][0] + numberWithIndex[right][0];
        if (sum == target) {
            return new int[]{numberWithIndex[left][1], numberWithIndex[right][1]};
        } else if (sum < target) {
            left++;
        } else {
            right--;
        }
    }
    return new int[]{-1, -1};
}
```
There is another way to do it, which is more memory efficient. Instead of creating a 2D array, we can create an array of Integer objects to hold the indices, and then sort that array with a custom comparator that compares the values in the original array.
```java
int[] twoSum(int[] nums, int target) {
    int n = nums.length;
    Integer[] order = new Integer[n];
    for (int i = 0; i < n; i++)
        order[i] = i;
    Arrays.sort(order, (a, b) -> Integer.compare(nums[a], nums[b]));
   // extra check to ensure sorting is correct
   //  for (int i = 0; i + 1 < n; i++) {
   //      assert (nums[order[i]] <= nums[order[i + 1]]);
   //  }
    int left = 0, right = n - 1;
    while (left < right) {
        int sum = nums[order[left]] + nums[order[right]]; // retrieve the value in the sorted order
        if (sum == target) {
            return new int[]{order[left], order[right]};
        } else if (sum < target) {
            left++;
        } else {
            right--;
        }
    }
    return new int[]{-1, -1};
}
```
For example, let's say you want to sort the array `nums = [4, 1, 2, 3, 2]` and its sorted array is `sorted_nums = [1, 2, 2, 3, 4]`. The `order` array will be initialized as `order = [0, 1, 2, 3, 4]`. After sorting with the custom comparator, the `order` array will become `[1, 2, 4, 3, 0]`. Which means number `nums[order[i]]` will be at position `order[i]` in the sorted array `sorted_nums`.

Why this method is better?
- It uses less memory, as we only need an array of integers to hold the indices, instead of a 2D array.
- You don't have to remember to select the correct column after sorting, as the original array is still intact.
- You might save some typing time!

The cons of this technique is that it is less readable for newcomers, to retrieve the value in the "sort form" you have to do `nums[order[i]]` instead of just `numberWithIndex[i][0]`.

In C++, you can initialize the `order` array like this:
```cpp
vector<int> order(n);
iota(order.begin(), order.end(), 0); // fill everything from 0 to n-1
```

This is neat trick which is very applicable when you need to sort with a custom comparator.

That's all for today! See you next time.
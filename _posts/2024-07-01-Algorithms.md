---
layout: page
title: "Algorithms"
date: 2024-06-20
description: My notes of datastructures and algorithms. 
featured: false
published: true
tags: [algorithms, math, code]
category: [Notes]
related_posts: false
thumbnail: assets/img/dsa/chinese-puzzle.jpeg
tabs: true
toc:
  sidebar: left
# class: post-template
# toc:
#     sidebar: left
# description: >
 
---

---



<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/dsa/problem-design.png" title="Problem Solution Framework" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>
<div class="caption">
    Generalized overview of a Problem-Solution Framework
</div>



---

## Data Structures







---

## Algorithms

---

### Searching: Binary Search

This is the fundamental template of the binary search algorithms which will evolve as per the requirements. 

**A trick for analyzing binary search is to avoid else statements and write all cases with else if statements, so that all details can be clearly presented**

Here in the Java code, `...` is the part where implementation can go wrong. Be careful.


{% tabs pattern %}

{% tab pattern python %}

```python
def binarySearch(nums: List[int], target: int) -> int:
    left, right = 0, len(nums) - 1

    while left <= right:
        mid = left + (right - left) // 2
        if nums[mid] == target:
            return mid
        elif nums[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    return -1
```

{% endtab %}

{% tab pattern java %}

```java
int binarySearch(int[] nums, int target) {
    int left = 0, right = ...;

    while(...) {
        int mid = left + (right - left) / 2;
        if (nums[mid] == target) {
            ...
        } else if (nums[mid] < target) {
            left = ...
        } else if (nums[mid] > target) {
            right = ...
        }
    }
    return ...;
}
```

{% endtab %}

{% tab pattern cpp %}

```cpp
int binarySearch(vector<int>& nums, int target) {
    int left = 0, right = nums.size() - 1;

    while(left <= right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] == target) {
            ...
        } else if (nums[mid] < target) {
            left = mid + 1;
        } else if (nums[mid] > target) {
            right = mid - 1;
        }
    }
    return ...;
}
```

{% endtab %}


{% endtabs %}

> `mid = left + (right - left)/2` gives the same value as `(left + right)/2` but we prefer prior to avoid any overflow by addition. 



---











---

# Problem Patterns

---

### Two pointers: one input, opposite ends

{% tabs pattern %}

{% tab pattern python %}

```python

```

{% endtab %}

{% tab pattern java %}

```java
public int fn(int[] arr) {
    int left = 0;
    int right = arr.length - 1;
    int ans = 0;
    while (left < right) {
        // do some logic here with left and right
        if (CONDITION) {
            left++;
        } else {
            right--;
        }
    }
    return ans;
}
```

{% endtab %}

{% endtabs %}





----













---

{% tabs pattern %}

{% tab pattern python %}

```python

```

{% endtab %}

{% tab pattern java %}

```java

```

{% endtab %}

{% tab pattern cpp %}

```cpp

```

{% endtab %}

{% endtabs %}


---
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

{% endtabs %}


---
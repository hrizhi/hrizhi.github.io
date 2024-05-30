---
layout: post
title: DSA templates (Java)
date: 2024-03-15 
description: These are java code templates based on most common datastructures and algorithm problems.
tags: code data-structures, algorithms
categories: Notes Articles
featured: false
toc:
    sidebar: left
---

<!-- ![Flowchart](/img/writings/patterns/flowchart.png)
*Caption: Flowchat for Characterization of Problem* -->

---

### Two pointers: one input, opposite ends

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
----

### Sliding Window

```java
public int fn(int[] arr) {
    int left = 0, ans = 0, curr = 0;

    for (int right = 0; right < arr.length; right++) {
        // do logic here to add arr[right] to curr

        while (WINDOW_CONDITION_BROKEN) {
            // remove arr[left] from curr
            left++;
        }

        // update ans
    }

    return ans;
}
```

---
### Buidling a Prefix Sum 

```java
public int[] fn(int[] arr) {
    int[] prefix = new int[arr.length];
    prefix[0] = arr[0];

    for (int i = 1; i < arr.length; i++) {
        prefix[i] = prefix[i - 1] + arr[i];
    }

    return prefix;
}
```

---
### Efficient String Building

```java
public String fn(char[] arr) {
    StringBuilder sb = new StringBuilder();
    for (char c: arr) {
        sb.append(c);
    }

    return sb.toString();
}
```

---

### *Fast & Slow pointer*

- Similar to the two pointers pattern, the fast and slow pointers pattern uses two pointers to traverse an `iterable data structure` at different speeds. It’s usually used to identify distinguishable features of **directional data structures**, such as a linked list or an array.

- The pointers can be used to traverse the array or list in either direction, however, one moves faster than the other. Generally, the slow pointer moves forward by a factor of one, and the fast pointer moves by a factor of two in each step. However, the speed can be adjusted according to the problem statement.

- Unlike the two pointers approach, which is concerned with data values, the fast and slow pointers approach is used to determine data structure traits using indices in arrays or node pointers in linked lists. The approach is commonly used to detect cycles in the given data structure, so it’s also known as **Floyd’s cycle detection algorithm**.

- The key idea is that the pointers start at the same location, but they move forward at different speeds. If there is a cycle, the two are bound to meet at some point in the traversal. To understand the concept, think of two runners on a track. While they start from the same point, they have different running speeds. If the race track is a circle, the faster runner will overtake the slower one after completing a lap. On the other hand, if the track is straight, the faster runner will end the race before the slower one, hence never meeting on the track again. The fast and slow pointers pattern uses the same intuition.

***Does my problem match this pattern?***
> ***Yes***, if either of these conditions is fulfilled:
>   
> - Either as an intermediate step, or as the final solution, the problem requires identifying:
> 
>   - the first $x \\%$ of the elements in a linked list, or, 
>   - the element at the $k-way$ point in a linked list, for example, the middle element, or the element at the start of the second quartile, etc.
>   - the $k^{th}$ last element in a linked list
> - Solving the problem requires detecting the presence of a cycle in a linked list.
> 
> - Solving the problem requires detecting the presence of a cycle in a sequence of symbols.
>
> 
> ***No***, if either of these conditions is fulfilled:
> 
> - The input data cannot be traversed in a linear fashion, that is, it’s neither in an array, nor in a linked list, nor in a string of characters.
> 
> - The problem can be solved with two pointers traversing an array or a linked list at the same pace.

**Real-world problems**

Many problems in the real world use the fast and slow pointers pattern. Let’s look at some examples.

- ***Symlink verification***: Fast and slow pointers can be used in a symlink verification utility in an operating system. A symbolic link, or symlink, is simply a shortcut to another file. Essentially, it’s a file that points to another file. Symlinks can easily create loops or cycles where shortcuts point to each other. To avoid such occurrences, a symlink verification utility can be used. Similar to a linked list, fast and slow pointers can detect a loop in the symlinks by moving along the connected files or directories at different speeds.

- ***Compiling an object-oriented program***: Usually, programs are not contained in a single file. Particularly, for large applications, modules can be divided into different files for better maintenance. Dependency relationships are then defined to specify the order of compilation for these files. However, sometimes, there might be cyclic dependencies that can lead to an error. Fast and slow pointers can be used to identify and remove these cycles for seamless compilation and execution of the program.

---

### Linked List: Fast and slow pointer 
> a.k.a. ***Floyd’s cycle detection***

```java
public int fn(ListNode head) {
    ListNode slow = head;
    ListNode fast = head;
    int ans = 0;

    //checking this condition so that line 10 
    //i.e. fast.next.next doesn't throw null pointer exception  
    
    while (fast != null && fast.next != null) {
        // do logic
        slow = slow.next;
        fast = fast.next.next;
    }

    return ans;
}
```
---
### Reversing a Linked List

```java
public ListNode fn(ListNode head) {
    ListNode curr = head;
    ListNode prev = null;
    while (curr != null) {
        ListNode nextNode = curr.next;
        curr.next = prev;
        prev = curr;
        curr = nextNode;
    }

    return prev;
}
```

---
### Find number of subarrays that fit an exact criteria

```java
public int fn(int[] arr, int k) {
    Map<Integer, Integer> counts = new HashMap<>();
    counts.put(0, 1);
    int ans = 0, curr = 0;

    for (int num: arr) {
        // do logic to change curr
        ans += counts.getOrDefault(curr - k, 0);
        counts.put(curr, counts.getOrDefault(curr, 0) + 1);
    }

    return ans;
}
```
---
### Monotonic increasing stack

> The same logic can be applied to maintain a monotonic queue.

```java
public int fn(int[] arr) {
    Stack<Integer> stack = new Stack<>();
    int ans = 0;

    for (int num: arr) {
        // for monotonic decreasing, just flip the > to <
        while (!stack.empty() && stack.peek() > num) {
            // do logic
            stack.pop();
        }

        stack.push(num);
    }

    return ans;
}
```
---
**Note:** 
- Iterative and recursive approaches do the job in one pass, but they both need up to $O(h)$ space to keep the stack, where $h$is a tree `height`.
- Morris's approach is a two-pass approach, but it's a constant-space one.

> **DFS: Pre or Inorder Traversal**
> - Iterative: *Best time*
> - Recursive: *Simplest to write*
> - Morris: *Constant Space*

### Binary Tree: DFS(Recursive)

```java
public int dfs(TreeNode root) {
    if (root == null) {
        return 0;
    }

    int ans = 0;
    // do logic
    dfs(root.left);
    dfs(root.right);
    return ans;
}
```

---
### Binary Tree: DFS(Iterative)

```java
public int dfs(TreeNode root) {
    Stack<TreeNode> stack = new Stack<>();
    stack.push(root);
    int ans = 0;

    while (!stack.empty()) {
        TreeNode node = stack.pop();
        // do logic
        if (node.left != null) {
            stack.push(node.left);
        }
        if (node.right != null) {
            stack.push(node.right);
        }
    }

    return ans;
}
```

### Binary Tree: DFS (Pre, In, Post)

```java
public List<Integer> preorderTraversal(TreeNode root) {
    Deque<TreeNode> stack = new ArrayDeque<>();
    LinkedList<Integer> output = new LinkedList<>();
    if (root == null)   return output;
    
    stack.push(root);
    
    while (!stack.isEmpty()) {
        TreeNode node = stack.pop();
        output.add(node.val);
        if (node.right != null) stack.push(node.right);
        if (node.left != null)  stack.push(node.left);
    }
    return output;
}


public List<Integer> inorderTraversal(TreeNode root) {
    List<Integer> res = new ArrayList<>();
    Stack<TreeNode> stack = new Stack<>();
    
    TreeNode curr = root;
    while (curr != null || !stack.isEmpty()) {
        while (curr != null) {
            stack.push(curr);
            curr = curr.left;
        }
        curr = stack.pop();
        res.add(curr.val);
        curr = curr.right;
    }
    return res;
}

public List<Integer> postorderTraversal(TreeNode root) {
    Deque<TreeNode> stack = new ArrayDeque<>();
    List<Integer> order = new ArrayList<Integer>();
    
    if(root != null)    stack.push(root);

    while(!stack.isEmpty()){
        TreeNode current = stack.pop();
        order.add(0, current.val);
        if (current.left != null)   stack.push(current.left);
        if (current.right != null)  stack.push(current.right);
    }
    return ls;
}

// Consider Pre-Order traversal (root --> Left --> Right), 
// while in post-order, it is Left --> Right --> Root. 
// So if you visualize we only have to visit the root in the end while traversing.

// So we will insert the root in the first go and will keep 
// adding the corresponding left and right child before the location of root in the answer (post-order traversal).
//This trick is quite evident in the code. We are making sure all the new additions are updated at the 0th index.

public List<Integer> levelOrderTraversal(TreeNode root) {
    List<Integer> result = new ArrayList<>();
    Deque<TreeNode> queue = new ArrayDeque<>();

    if (root != null) queue.offer(root);

    while (!queue.isEmpty()) {
        TreeNode current = queue.poll();
        result.add(current.val);
        if (current.left != null) queue.offer(current.left);
        if (current.right != null) queue.offer(current.right);
    }
    return result;
}

```



---
### Binary Tree: BFS

```java
public int fn(TreeNode root) {
    Queue<TreeNode> queue = new LinkedList<>();
    queue.add(root);
    int ans = 0;

    while (!queue.isEmpty()) {
        int currentLength = queue.size();
        // do logic for current level

        for (int i = 0; i < currentLength; i++) {
            TreeNode node = queue.remove();
            // do logic
            if (node.left != null) {
                queue.add(node.left);
            }
            if (node.right != null) {
                queue.add(node.right);
            }
        }
    }

    return ans;
}
```

### Graph: DFS (Recursive)

> For the graph templates, assume the nodes are numbered from ```0``` to ```n-1``` and the graph is given as an adjacency list. Depending on the problem, you may need to convert the input into an equivalent adjacency list before using the templates.


```java
Set<Integer> seen = new HashSet<>();

public int fn(int[][] graph) {
    seen.add(START_NODE);
    return dfs(START_NODE, graph);
}

public int dfs(int node, int[][] graph) {
    int ans = 0;
    // do some logic
    for (int neighbor: graph[node]) {
        if (!seen.contains(neighbor)) {
            seen.add(neighbor);
            ans += dfs(neighbor, graph);
        }
    }

    return ans;
}
```

---
### Graph: DFS (Iterative)

```java
public int fn(int[][] graph) {
    Stack<Integer> stack = new Stack<>();
    Set<Integer> seen = new HashSet<>();
    stack.push(START_NODE);
    seen.add(START_NODE);
    int ans = 0;

    while (!stack.empty()) {
        int node = stack.pop();
        // do some logic
        for (int neighbor: graph[node]) {
            if (!seen.contains(neighbor)) {
                seen.add(neighbor);
                stack.push(neighbor);
            }
        }
    }

    return ans;
}
```
---
### Graph: BFS

```java
public int fn(int[][] graph) {
    Queue<Integer> queue = new LinkedList<>();
    Set<Integer> seen = new HashSet<>();
    queue.add(START_NODE);
    seen.add(START_NODE);
    int ans = 0;

    while (!queue.isEmpty()) {
        int node = queue.remove();
        // do some logic
        for (int neighbor: graph[node]) {
            if (!seen.contains(neighbor)) {
                seen.add(neighbor);
                queue.add(neighbor);
            }
        }
    }

    return ans;
}
```

### Union Find

```java
public static class UnionFind<T> {
    private HashMap<T, T> id = new HashMap<>();

    public T find(T x) {
        T y = id.getOrDefault(x, x);
        if (y != x) {
            y = find(y);
            id.put(x, y);
        }
        return y;
    }

    public void union(T x, T y) {
        id.put(find(x), find(y));
    }
}
```



---

### Finding top K elements with heap

```java
public int[] fn(int[] arr, int k) {
    PriorityQueue<Integer> heap = new PriorityQueue<>(CRITERIA);
    for (int num: arr) {
        heap.add(num);
        if (heap.size() > k) {
            heap.remove();
        }
    }

    int[] ans = new int[k];
    for (int i = 0; i < k; i++) {
        ans[i] = heap.remove();
    }

    return ans;
}
```
---

### Binary Search
```java
public int fn(int[] arr, int target) {
    int left = 0;
    int right = arr.length - 1;
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (arr[mid] == target) {
            // do something
            return mid;
        }
        if (arr[mid] > target) {
            right = mid - 1;
        } else {
            left = mid + 1;
        }
    }

    // left is the insertion point
    return left;
}
```

---

### Binary Search: Duplicate elements, left-most insertion point

```java
public int fn(int[] arr, int target) {
    int left = 0;
    int right = arr.length;
    while (left < right) {
        int mid = left + (right - left) / 2;
        if (arr[mid] >= target) {
            right = mid
        } else {
            left = mid + 1;
        }
    }

    return left;
}
```

### Binary search: duplicate elements, right-most insertion point


```java
public int fn(int[] arr, int target) {
    int left = 0;
    int right = arr.length;
    while (left < right) {
        int mid = left + (right - left) / 2;
        if (arr[mid] > target) {
            right = mid;
        } else {
            left = mid + 1;
        }
    }

    return left;
}
```

---

### Binary Search: For greedy problems

> If you are looking for **minimum**

```java
public int fn(int[] arr) {
    int left = MINIMUM_POSSIBLE_ANSWER;
    int right = MAXIMUM_POSSIBLE_ANSWER;
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (check(mid)) {
            right = mid - 1;
        } else {
            left = mid + 1;
        }
    }

    return left;
}

public boolean check(int x) {
    // this function is implemented depending on the problem
    return BOOLEAN;
}
```

> If you are looking for **maximum**

```java
public int fn(int[] arr) {
    int left = MINIMUM_POSSIBLE_ANSWER;
    int right = MAXIMUM_POSSIBLE_ANSWER;
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (check(mid)) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }

    return right;
}

public boolean check(int x) {
    // this function is implemented depending on the problem
    return BOOLEAN;
}
```

---

### Backtracking

```java
public int backtrack(STATE curr, OTHER_ARGUMENTS...) {
    if (BASE_CASE) {
        // modify the answer
        return 0;
    }

    int ans = 0;
    for (ITERATE_OVER_INPUT) {
        // modify the current state
        ans += backtrack(curr, OTHER_ARGUMENTS...)
        // undo the modification of the current state
    }
}
```


### Dynamic Programming: Top-Down Memoization

```java
Map<STATE, Integer> memo = new HashMap<>();

public int fn(int[] arr) {
    return dp(STATE_FOR_WHOLE_INPUT, arr);
}

public int dp(STATE, int[] arr) {
    if (BASE_CASE) {
        return 0;
    }

    if (memo.contains(STATE)) {
        return memo.get(STATE);
    }

    int ans = RECURRENCE_RELATION(STATE);
    memo.put(STATE, ans);
    return ans;
}

```

### Build a Trie

```java
// note: using a class is only necessary if you want to store data at each node.
// otherwise, you can implement a trie using only hash maps.
class TrieNode {
    // you can store data at nodes if you wish
    int data;
    Map<Character, TrieNode> children;
    TrieNode() {
        this.children = new HashMap<>();
    }
}

public TrieNode buildTrie(String[] words) {
    TrieNode root = new TrieNode();
    for (String word: words) {
        TrieNode curr = root;
        for (char c: word.toCharArray()) {
            if (!curr.children.containsKey(c)) {
                curr.children.put(c, new TrieNode());
            }

            curr = curr.children.get(c);
        }

        // at this point, you have a full word at curr
        // you can perform more logic here to give curr an attribute if you want
    }

    return root;
}
```

--- 

### Dijkstra's Algorithm

```java
int[] distances = new int[n];
Arrays.fill(distances, Integer.MAX_VALUE);
distances[source] = 0;

Queue<Pair<Integer, Integer>> heap = new PriorityQueue<Pair<Integer,Integer>>(Comparator.comparing(Pair::getKey));
heap.add(new Pair(0, source));

while (!heap.isEmpty()) {
    Pair<Integer, Integer> curr = heap.remove();
    int currDist = curr.getKey();
    int node = curr.getValue();
    
    if (currDist > distances[node]) {
        continue;
    }
    
    for (Pair<Integer, Integer> edge: graph.get(node)) {
        int nei = edge.getKey();
        int weight = edge.getValue();
        int dist = currDist + weight;
        
        if (dist < distances[nei]) {
            distances[nei] = dist;
            heap.add(new Pair(dist, nei));
        }
    }
}
```




![](/img/DSA_cheatsheet/1.png)
![](/img/DSA_cheatsheet/2.png)
![](/img/DSA_cheatsheet/3.png)
![](/img/DSA_cheatsheet/4.png)


---




## Extra 


### Morris Traversal

- Performed on `threaded binary tree` - has a thread or link that points to some ancestor node
- Takes space complexity of $O(1)$ and do not require stack or queue.
- Time complexity: $O(n)$


```java
//pseudo-code of Morris Inorder
1. Initialize current as the root of the binary tree.
2. While current is not null:
   a. If current has no left child:
      i. Visit the current node.
      ii. Move to the right child (current = current.right).
   b. If current has a left child:
      i. Find the inorder predecessor of current (the rightmost node in the left subtree).
      ii. If the right child of the inorder predecessor is null:
          - Set the right child of the inorder predecessor to current.
          - Move to the left child (current = current.left).
      iii. If the right child of the inorder predecessor is not null:
          - Reset the right child of the inorder predecessor to null.
          - Visit the current node.
          - Move to the right child (current = current.right).

```


```java
public void inorder(Node root) {
    Node current = root;
    while(current != null) {
        //left is null then print the node and go to right
        if (current.left == null) {
            System.out.print(current.data + " ");
            current = current.right;
        }
        else {
            //find the predecessor.
            Node predecessor = current.left;
            //To find predecessor keep going right till right node is not null or right node is not current.
            while(predecessor.right != current && predecessor.right != null){
                predecessor = predecessor.right;
            }
            //if right node is null then go left after establishing link from predecessor to current.
            if(predecessor.right == null){
                predecessor.right = current;
                current = current.left;
            }else{ //left is already visit. Go rigth after visiting current.
                predecessor.right = null;
                System.out.print(current.data + " ");
                current = current.right;
            }
        }
    }
}

public void preorder(Node root) {
    Node current = root;
    while (current != null) {
        if(current.left == null) {
            System.out.print(current.data + " ");
            current = current.right;
        }
        else {
            Node predecessor = current.left;
            while(predecessor.right != current && predecessor.right != null) {
                predecessor = predecessor.right;
            }
            if(predecessor.right == null){
                predecessor.right = current;
                System.out.print(current.data + " ");
                current = current.left;
            }else{
                predecessor.right = null;
                current = current.right;
            }
        }
    }
}

```
--- 

## String Manipulations

```java
String s = "A B C D ";


// trim the trailing spaces in the string 
// "A B C D"
// Returns a copy of string O(N)
s = s.trim();  

//Comparisons

String s1 = new String("QuickRef"); 
String s2 = new String("QuickRef"); 

s1 == s2          // false
s1.equals(s2)     // true

"AB".equalsIgnoreCase("ab")  // true


//Concatenation

String s = 3 + "str" + 3;     // 3str3
String s = 3 + 3 + "str";     // 6str
String s = "3" + 3 + "str";   // 33str
String s = "3" + "3" + "23";  // 3323
String s = "" + 3 + 3 + "23"; // 3323
String s = 3 + 3 + 23;        // 29


// Manipulations 

String str = "Abcd";

str.toUpperCase();     // ABCD
str.toLowerCase();     // abcd
str.concat("#");       // Abcd#
str.replace("b", "-"); // A-cd

"  abc ".trim();       // abc
"ab".toCharArray();    // {'a', 'b'}




// Information

String str = "abcd";

str.charAt(2);       // c
str.indexOf("a")     // 0
str.indexOf("z")     // -1
str.length();        // 4
str.toString();      // abcd
str.substring(2);    // cd
str.substring(2,3);  // c
str.contains("c");   // true
str.endsWith("d");   // true
str.startsWith("a"); // true
str.isEmpty();       // false


```
[More](https://quickref.me/java.html)

--- 

### Java-specific tricks for tackling string-related problems efficiently:

1. **String to Character Array Conversion:**
   - Converting a string to a character array (`char[]`) allows for easy manipulation of individual characters using array indexing.

    ```java
    String s = "leetcode";
    char[] charArray = s.toCharArray();
    ```

2. **StringBuilder for String Concatenation:**
   - Using `StringBuilder` is more efficient than concatenating strings using the `+` operator, especially in a loop where strings are frequently modified.

    ```java
    StringBuilder sb = new StringBuilder();
    sb.append("Hello").append(" ").append("World");
    String result = sb.toString();
    ```

3. **Checking Palindromes:**
   - For palindrome-related problems, consider using two pointers to check characters from the start and end simultaneously.

    ```java
    boolean isPalindrome(String s) {
        int i = 0, j = s.length() - 1;
        while (i < j) {
            if (s.charAt(i++) != s.charAt(j--)) {
                return false;
            }
        }
        return true;
    }
    ```

4. **String Comparison:**
   - When comparing strings, prefer using `equals()` or `compareTo()` instead of `==` to ensure content comparison.

    ```java
    String s1 = "hello";
    String s2 = "world";
    if (s1.equals(s2)) {
        // Strings are equal
    }
    ```

5. **String Trimming and Splitting:**
   - Utilize `trim()` to remove leading and trailing whitespaces and `split()` for breaking a string into an array of substrings.

    ```java
    String input = "   hello, world   ";
    String trimmed = input.trim();
    String[] parts = input.split(",");
    ```

6. **String Reversal:**
   - Reverse a string using either a `StringBuilder` or by converting it to a character array.

    ```java
    String original = "hello";
    String reversed1 = new StringBuilder(original).reverse().toString();
    String reversed2 = new String(new StringBuilder(original).reverse());
    ```

7. **Substring Extraction:**
   - Extract substrings using `substring(startIndex, endIndex)`. Be cautious about the endIndex, as it denotes the exclusive end position.

    ```java
    String s = "leetcode";
    String subString = s.substring(2, 5); // Output: "tco"
    ```

8. **Regular Expressions:**
   - Leverage regular expressions (`java.util.regex`) for complex pattern matching or extraction tasks.

    ```java
    import java.util.regex.*;

    String input = "abc123";
    Pattern pattern = Pattern.compile("[0-9]+");
    Matcher matcher = pattern.matcher(input);

    if (matcher.find()) {
        String digits = matcher.group();
        System.out.println(digits);  // Output: "123"
    }
    ```
---

### HashMap/HashSet Templates

1. **Frequency Counting:**
    > Use `HashMap` to count the frequency of elements in an array or string. This is useful for problems involving finding duplicates, checking anagrams, or identifying the majority element.

   ```java
   Map<Integer, Integer> frequencyMap = new HashMap<>();
   for (int num : nums) {
       frequencyMap.put(num, frequencyMap.getOrDefault(num, 0) + 1);
   }
   ```

2. **Character Frequency:**
   > For problems involving character frequency in strings, use a `HashMap` to count the occurrences of each character.

   ```java
   Map<Character, Integer> charFrequency = new HashMap<>();
   for (char c : s.toCharArray()) {
       charFrequency.put(c, charFrequency.getOrDefault(c, 0) + 1);
   }
   ```

3. **Two-Sum and Three-Sum:**
   > For Two-Sum and Three-Sum problems, use a `HashMap` to store the complement of the current element you are iterating over.

   ```java
   Map<Integer, Integer> complementMap = new HashMap<>();
   for (int i = 0; i < nums.length; i++) {
       int complement = target - nums[i];
       if (complementMap.containsKey(complement)) {
           // Found a pair or triplet
       }
       complementMap.put(nums[i], i);
   }
   ```

4. **Subarray Sum Equals K:**
   > For problems related to subarray sums, use a `HashMap` to store cumulative sums and their frequencies.

   ```java
   Map<Integer, Integer> cumulativeSumMap = new HashMap<>();
   int count = 0, sum = 0;
   cumulativeSumMap.put(0, 1);
   for (int num : nums) {
       sum += num;
       if (cumulativeSumMap.containsKey(sum - k)) {
           count += cumulativeSumMap.get(sum - k);
       }
       cumulativeSumMap.put(sum, cumulativeSumMap.getOrDefault(sum, 0) + 1);
   }
   ```

5. **Linked List Cycle Detection:**
   > When dealing with linked lists, `HashMap` can be used to detect cycles efficiently.

   ```java
   Map<ListNode, Integer> visited = new HashMap<>();
   while (head != null) {
       if (visited.containsKey(head)) {
           // Cycle detected
           break;
       }
       visited.put(head, 1);
       head = head.next;
   }
   ```

6. **Cache Results:**
   > For problems involving repetitive calculations or recursive calls, use a `HashMap` to cache results and avoid redundant computations.

   ```java
   Map<Integer, Integer> memo = new HashMap<>();
   public int fib(int n) {
       if (memo.containsKey(n)) {
           return memo.get(n);
       }
       if (n <= 1) {
           return n;
       }
       int result = fib(n - 1) + fib(n - 2);
       memo.put(n, result);
       return result;
   }
   ```

7. **Mapping Indices:**
   > Use `HashMap` to map values to their indices, enabling quick lookups.

   ```java
   Map<Integer, Integer> indexMap = new HashMap<>();
   for (int i = 0; i < nums.length; i++) {
       indexMap.put(nums[i], i);
   }
   ```

8. **Group Anagrams:**
   > For problems involving anagrams, use `HashMap` to group strings with the same set of characters.

   ```java
   Map<String, List<String>> anagramGroups = new HashMap<>();
   for (String str : strs) {
       char[] charArray = str.toCharArray();
       Arrays.sort(charArray);
       String sortedStr = new String(charArray);
       anagramGroups.computeIfAbsent(sortedStr, k -> new ArrayList<>()).add(str);
   }
   ```

9. **Union-Find with HashMap:**
   > For disjoint-set problems, you can implement union-find using `HashMap` to represent parent relationships.

   ```java
   Map<Integer, Integer> parent = new HashMap<>();
   public int find(int x) {
       if (!parent.containsKey(x)) {
           parent.put(x, x);
       }
       if (parent.get(x) != x) {
           parent.put(x, find(parent.get(x))); // Path compression
       }
       return parent.get(x);
   }
   ```
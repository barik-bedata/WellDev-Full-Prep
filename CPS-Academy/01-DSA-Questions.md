# WellDev Interview Prep — DSA (Data Structures & Algorithms)

> Only **questions + corresponding LeetCode links** - solutions are not provided for self-practice.
> Compiled from real WellDev interview experiences. Most coding round questions are LeetCode Easy/Medium.

---

## Strings

- **Calculate the highest length of a palindrome from a given string.**
  🔗 [5. Longest Palindromic Substring](https://leetcode.com/problems/longest-palindromic-substring/) · [409. Longest Palindrome](https://leetcode.com/problems/longest-palindrome/) · [647. Palindromic Substrings](https://leetcode.com/problems/palindromic-substrings/)

- **Anagram — check whether two strings are anagrams of each other.**
  🔗 [242. Valid Anagram](https://leetcode.com/problems/valid-anagram/) · [49. Group Anagrams](https://leetcode.com/problems/group-anagrams/)

- **Which data structure is best suited for searching strings?**
  🔗 [208. Implement Trie (Prefix Tree)](https://leetcode.com/problems/implement-trie-prefix-tree/)

- **Reverse Words in a String**
  🔗 [151. Reverse Words in a String](https://leetcode.com/problems/reverse-words-in-a-string/)

- **First Non-Repeating Character**
  🔗 [387. First Unique Character in a String](https://leetcode.com/problems/first-unique-character-in-a-string/)

- **String to Integer (atoi)**
  🔗 [8. String to Integer (atoi)](https://leetcode.com/problems/string-to-integer-atoi/)

## Arrays & Two Pointers

- **Two Sum — print all the valid pairs.**
  🔗 [1. Two Sum](https://leetcode.com/problems/two-sum/) · [167. Two Sum II – Input Array Is Sorted](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/)

- **3Sum**
  🔗 [15. 3Sum](https://leetcode.com/problems/3sum/) · [18. 4Sum](https://leetcode.com/problems/4sum/)

- **Sort an array that consists only of odd numbers.**
  🔗 [912. Sort an Array](https://leetcode.com/problems/sort-an-array/) · [905. Sort Array By Parity](https://leetcode.com/problems/sort-array-by-parity/)

- **Zero Mover — move all zeros to one side of the array.**
  🔗 [283. Move Zeroes](https://leetcode.com/problems/move-zeroes/)

- **Print the cumulative (prefix) sum of an array.**
  🔗 [1480. Running Sum of 1d Array](https://leetcode.com/problems/running-sum-of-1d-array/) · [303. Range Sum Query – Immutable](https://leetcode.com/problems/range-sum-query-immutable/)

- **Explain the Two Pointer technique in detail.**
  🔗 [11. Container With Most Water](https://leetcode.com/problems/container-with-most-water/) · [125. Valid Palindrome](https://leetcode.com/problems/valid-palindrome/) · [42. Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/)

- **Given an array, what will be printed if we print only the array name (e.g., `printf("%p", ara)`)? Explain the base address behavior.**
  🔗 No LeetCode equivalent — C/C++ array-to-pointer decay theory question (see Fundamentals file, Lesson 7).

- **Rotate Array**
  🔗 [189. Rotate Array](https://leetcode.com/problems/rotate-array/)

- **Second highest element in an array (one pass)**
  🔗 [414. Third Maximum Number](https://leetcode.com/problems/third-maximum-number/)

## Searching

- **Binary Search — implement it.**
  🔗 [704. Binary Search](https://leetcode.com/problems/binary-search/) · [35. Search Insert Position](https://leetcode.com/problems/search-insert-position/) · [33. Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/)

- **Can we run binary search on a linked list? Why or why not?**
  🔗 Theory - but see [109. Convert Sorted List to BST](https://leetcode.com/problems/convert-sorted-list-to-binary-search-tree/) for the practical workaround.

- **Koko Eating Bananas** (binary search on the answer)
  🔗 [875. Koko Eating Bananas](https://leetcode.com/problems/koko-eating-bananas/)

- **Median of Two Sorted Arrays**
  🔗 [4. Median of Two Sorted Arrays](https://leetcode.com/problems/median-of-two-sorted-arrays/)

## Sorting

- **Merge Sort - implement it.**
  🔗 [912. Sort an Array](https://leetcode.com/problems/sort-an-array/) (using merge sort)

- **Quick Sort - implement it (pivot & partition).**
  🔗 No single LC problem - implement directly, or apply on [912. Sort an Array](https://leetcode.com/problems/sort-an-array/)

- **Sort Colors (Dutch National Flag)**
  🔗 [75. Sort Colors](https://leetcode.com/problems/sort-colors/)

- **Insertion Sort vs Selection Sort — explain difference.**
  🔗 Theory — implement both, compare step by step

## Recursion & Dynamic Programming

- **Write pseudocode to generate Fibonacci numbers using only two variables.**
  🔗 [509. Fibonacci Number](https://leetcode.com/problems/fibonacci-number/) · [70. Climbing Stairs](https://leetcode.com/problems/climbing-stairs/)

- **Write a basic recursive implementation problem.**
  🔗 [344. Reverse String](https://leetcode.com/problems/reverse-string/) · [50. Pow(x, n)](https://leetcode.com/problems/powx-n/)

- **Tower of Hanoi**
  🔗 Classic recursion problem.

- **Given a recursive function that calculates the sum of the first N consecutive numbers, provide an appropriate, readable name for the function.** (Tests naming ability — see Fundamentals file, Lesson 9)
  🔗 No LeetCode equivalent — code-quality / naming discussion.

- **General discussion: Linked List, DP, Binary Search.**
  🔗 [LeetCode Top Interview 150](https://leetcode.com/studyplan/top-interview-150/)

- **House Robber**
  🔗 [198. House Robber](https://leetcode.com/problems/house-robber/)

- **Coin Change**
  🔗 [322. Coin Change](https://leetcode.com/problems/coin-change/)

- **Subsets / Permutations (backtracking)**
  🔗 [78. Subsets](https://leetcode.com/problems/subsets/) · [46. Permutations](https://leetcode.com/problems/permutations/)

## Trees & BST

- **Write the post-order traversal sequence of a given binary tree.**
  🔗 [145. Binary Tree Postorder Traversal](https://leetcode.com/problems/binary-tree-postorder-traversal/)

- **Given the pre-order traversal of a BST, write its post-order traversal.**
  🔗 [1008. Construct Binary Search Tree from Preorder Traversal](https://leetcode.com/problems/construct-binary-search-tree-from-preorder-traversal/) (build the tree, then post-order it)

- **How do you iterate a Binary Search Tree in reverse order?**
  🔗 [230. Kth Smallest Element in a BST](https://leetcode.com/problems/kth-smallest-element-in-a-bst/) · [173. Binary Search Tree Iterator](https://leetcode.com/problems/binary-search-tree-iterator/) (reverse in-order = right → node → left)

- **Solve a binary-tree-based problem.**
  🔗 [104. Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/) · [102. Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/) · [236. Lowest Common Ancestor of a Binary Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/)

## Graphs

- **How would you represent a graph in code?**
  🔗 [133. Clone Graph](https://leetcode.com/problems/clone-graph/) · [207. Course Schedule](https://leetcode.com/problems/course-schedule/)

- **Given a chessboard with some knights already placed, discuss how to place additional knights so that no two knights attack each other.** (Graph — DFS/BFS/backtracking style)
  🔗 [51. N-Queens](https://leetcode.com/problems/n-queens/) (same backtracking shape) · [688. Knight Probability in Chessboard](https://leetcode.com/problems/knight-probability-in-chessboard/) · [1197. Minimum Knight Moves](https://leetcode.com/problems/minimum-knight-moves/)

- **Which data structures are used in DFS and BFS?**
  🔗 Theory — DFS uses a Stack (or recursion), BFS uses a Queue

## Stack & Queue

- **Delete the middle element of a stack without using any additional data structure, preserving the original order.** Only the stack is given; extra variables allowed, but no extra array/list. Input: `[1,2,3,4,5]` → Output: `[1,2,4,5]`
  🔗 No LeetCode equivalent — see GfG: "Delete middle element of a stack". Closest LC idea: [2095. Delete the Middle Node of a Linked List](https://leetcode.com/problems/delete-the-middle-node-of-a-linked-list/)

- **Can we implement a stack using queues (and vice versa)?**
  🔗 [225. Implement Stack using Queues](https://leetcode.com/problems/implement-stack-using-queues/) · [232. Implement Queue using Stacks](https://leetcode.com/problems/implement-queue-using-stacks/)

- **Min Stack — O(1) minimum**
  🔗 [155. Min Stack](https://leetcode.com/problems/min-stack/)

## Priority Queue / Heap

- **Solve one priority queue problem from any online judge.**
  🔗 [703. Kth Largest Element in a Stream](https://leetcode.com/problems/kth-largest-element-in-a-stream/) · [215. Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/) · [347. Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/) · [23. Merge k Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/)

## Linked List

- **How do you reverse a linked list?**
  🔗 [206. Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/) · [92. Reverse Linked List II](https://leetcode.com/problems/reverse-linked-list-ii/)

- **Array vs Linked List — which performs better, when?**
  🔗 Theory (see Fundamentals-adjacent Module 9 discussion)

- **Circular Linked List — insertion/deletion**
  🔗 Theory/Implementation — no direct single LC problem

## OOP in Code

- **Write a function/class that demonstrates polymorphism.**
  🔗 No LeetCode equivalent — design/OOP question. Loosely: [155. Min Stack](https://leetcode.com/problems/min-stack/) or [146. LRU Cache](https://leetcode.com/problems/lru-cache/) for class-design practice.

## Language Fundamentals & Trick Questions

- **Swap two variables.**
  🔗 No LeetCode equivalent — basics (see Fundamentals file, Lesson 1).

- **Swap two values without using a temporary/third variable.**
  🔗 No LeetCode equivalent — use XOR or arithmetic swap. Related idea: [136. Single Number](https://leetcode.com/problems/single-number/) (XOR trick). (See Fundamentals file, Lessons 2–3.)

- **What is the output of the following code?**
  ```cpp
  int i = 5;
  cout << i++ << endl;
  cout << ++i << endl;
  return 0;
  ```
  🔗 No LeetCode equivalent — pre/post increment & sequencing trivia (see Fundamentals file, Lesson 5).

- **What is the time complexity of the following code?**
  ```cpp
  int fun(int n) {
      if (n <= 1) return n;
      int x = fun(n - 1);
      int y = fun(n - 2);
      return x + y;
  }
  ```
  🔗 [509. Fibonacci Number](https://leetcode.com/problems/fibonacci-number/) (naive recursion → exponential O(2ⁿ); memoize to fix)



### A Note
More than half of the coding round questions are LeetCode Easy/Medium - the goal is not to trap you with hard problems, but to test your fundamentals. Mentioning the brute force approach first is a strength, not a weakness - it shows you understand the problem. After that, it is better if you optimize it yourself.
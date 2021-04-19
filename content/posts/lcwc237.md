---
title: Leetcode Contest 237
description: my solutions and thought process for leetcode contest 237
toc: true
authors: 
    - Example Author
tags: 
    - leetcode
    - algorithm
categories: 
    - algorithm
series: []
date: 2021-04-18T21:48:51-07:00
lastmod: 2021-04-18T21:48:51-07:00
featuredVideo:
featuredImage:
draft: false
---

Leetcode 1832 ~ 1825, I was able to finished all problems during the contest : )
<!--more-->
 
- 1832.Check if the Sentence Is Pangram *(easy)*
- 1833.Maximum Ice Cream Bars *(medium)*
- 1834.Single-Threaded CPU *(medium)*
- 1835.Find XOR Sum of All Pairs Bitwise AND *(hard)*


## 1832. Check if the Sentence Is Pangram

A pangram is a sentence where every letter of the English alphabet appears at least once.

Given a string sentence containing only lowercase English letters, return true if sentence is a pangram, or false otherwise.


Constraints:

- 1 <= sentence.length <= 1000
- sentence consists of lowercase English letters.

**Example 1**:
```
Input: sentence = "thequickbrownfoxjumpsoverthelazydog"
Output: true
Explanation: sentence contains at least one of every letter of the English alphabet.
```

**Example 2**:
```
Input: sentence = "leetcode"
Output: false
```

**Solution**:

we loop through the input string, and add all character found in the string to a set, and check if the set has 26 elements in the end
```python
def checkIfPangram(self, sentence: str) -> bool:
    s = set()
    for c in sentence:
        s.add(c)
    return len(s) == 26
```

**Explanation**:

2 things to keep in mind for this problem:

    - every letter of the English alphabet appears at least once
    - sentence consists of lowercase English letters.

this question is asking us to determine wheather the input is a "Panagram" or not
so obviously we need to traverse the whole string, but what are we keeping track of?

we need to keep track of how many **unique** characters has occurred, by default, if a character 
is found in the string, it appeared at least once

talking about checking how many unique character there are, it reminded me of **set**

a set can only have distinct elements, so in the end we just need to check if there are 26 elements 
in the set, since the input can only consists of lowercase English letters


**Optimiazations/Alternatives**:

- replace the set by a fixed size array, the length would be 26, and use it as a set
- use a integer/bitset as a set, just return if there are 26 '1's 


**Complexity**:

- Time: O(n) n is the length of the string
- Space: O(1) there are at most 26 unique characters in the set



## 1833. Maximum Ice Cream Bars

It is a sweltering summer day, and a boy wants to buy some ice cream bars.

At the store, there are n ice cream bars. You are given an array costs of length n, where costs[i] is the price of the ith ice cream bar in coins. The boy initially has coins coins to spend, and he wants to buy as many ice cream bars as possible. 

Return the maximum number of ice cream bars the boy can buy with coins coins.

Note: The boy can buy the ice cream bars in any order.

 

**Example 1**:

```
Input: costs = [1,3,2,4,1], coins = 7
Output: 4
Explanation: The boy can buy ice cream bars at indices 0,1,2,4 for a total price of 1 + 3 + 2 + 1 = 7.
```

**Example 2**:
```
Input: costs = [10,6,8,7,7,8], coins = 5
Output: 0
Explanation: The boy cannot afford any of the ice cream bars.
```

**Example 3**:
```
Input: costs = [1,6,3,1,2,5], coins = 20
Output: 6
Explanation: The boy can buy all the ice cream bars for a total price of 1 + 6 + 3 + 1 + 2 + 5 = 18.
```


**Constraints**:
- costs.length == n
- 1 <= n <= 105
- 1 <= costs[i] <= 105
- 1 <= coins <= 108



**Solution**:

sort + greedy 
buy the cheapist item first, and buy as many as possible 
```python
def maxIceCream(self, costs: List[int], coins: int) -> int:
    costs.sort()
    idx = 0 
    while coins and idx < len(costs):
        if costs[idx] <= coins:
            coins -= costs[idx]
            idx += 1
        else:
            break 
    return idx   

def maxIceCream(self, costs: List[int], coins: int) -> int:
    costs.sort() 
    # cheap ones go first 
    idx = 0 
    while coins and idx < len(costs):
        if costs[idx] <= coins: 
            # purchase the current ice cream 
            coins -= costs[idx]
            idx += 1
        else: 
            # dont have money anymore
            break 
    return idx   
```

**Explanation**:

we need to get the maximum number of ice cream bars the boy can buy with coins coins. And there's no weight between each ice cream bars,
meaning that ice creams are all identical, so just buy the cheapist ones, and maximize the quantity

**Complexity**:
- O(NlogN) N is the length of the input array
- O(1)


## 1834. Single-Threaded CPU

You are given n​​​​​​ tasks labeled from 0 to n - 1 represented by a 2D integer array tasks, where tasks[i] = [enqueueTimei, processingTimei] means that the i​​​​​​th​​​​ task will be available to process at enqueueTimei and will take processingTimei to finish processing.

You have a single-threaded CPU that can process at most one task at a time and will act in the following way:

- If the CPU is idle and there are no available tasks to process, the CPU remains idle.
- If the CPU is idle and there are available tasks, the CPU will choose the one with the shortest processing time. If multiple tasks have the same shortest processing time, it will choose the task with the smallest index.
- Once a task is started, the CPU will process the entire task without stopping.
- The CPU can finish a task then start a new one instantly.

Return the order in which the CPU will process the tasks.

**Example 1**:

```
Input: tasks = [[1,2],[2,4],[3,2],[4,1]]
Output: [0,2,3,1]
Explanation: The events go as follows: 
- At time = 1, task 0 is available to process. Available tasks = {0}.
- Also at time = 1, the idle CPU starts processing task 0. Available tasks = {}.
- At time = 2, task 1 is available to process. Available tasks = {1}.
- At time = 3, task 2 is available to process. Available tasks = {1, 2}.
- Also at time = 3, the CPU finishes task 0 and starts processing task 2 as it is the shortest. Available tasks = {1}.
- At time = 4, task 3 is available to process. Available tasks = {1, 3}.
- At time = 5, the CPU finishes task 2 and starts processing task 3 as it is the shortest. Available tasks = {1}.
- At time = 6, the CPU finishes task 3 and starts processing task 1. Available tasks = {}.
- At time = 10, the CPU finishes task 1 and becomes idle.
```

**Example 2**:
```
Input: tasks = [[7,10],[7,12],[7,5],[7,4],[7,2]]
Output: [4,3,2,0,1]
Explanation: The events go as follows:
- At time = 7, all the tasks become available. Available tasks = {0,1,2,3,4}.
- Also at time = 7, the idle CPU starts processing task 4. Available tasks = {0,1,2,3}.
- At time = 9, the CPU finishes task 4 and starts processing task 3. Available tasks = {0,1,2}.
- At time = 13, the CPU finishes task 3 and starts processing task 2. Available tasks = {0,1}.
- At time = 18, the CPU finishes task 2 and starts processing task 0. Available tasks = {1}.
- At time = 28, the CPU finishes task 0 and starts processing task 1. Available tasks = {}.
- At time = 40, the CPU finishes task 1 and becomes idle.
 ```

**Constraints**:

- tasks.length == n
- 1 <= n <= 105
- 1 <= enqueueTimei, processingTimei <= 109

**Solution**:

Min heap / greedy  / simulation
sort the input array by starting time, push all the available tasks into a min heap,
process the ones that meets the requirement

```python
def getOrder(self, tasks: List[List[int]]) -> List[int]:
    heap, task, res = [], [], []
    time = float('inf')
    
    for i, val in enumerate(tasks):
        time = min(time, val[0]) 
        task.append((val[1], i, val[0])) 
    task.sort(key=lambda x : x[2])
    
    while len(heap) > 0 or len(task) > 0: 
        while len(task) > 0 and task[0][2] <= time: 
            heappush(heap, task[0])
            task.pop(0)
        if len(heap):
            top = heappop(heap)
            res.append(top[1])
            time += top[0]  
        else:               
            time = max(t, task[0][2])
    return res    

def getOrder(self, tasks: List[List[int]]) -> List[int]:
    heap, task, res = [], [], []
    time = float('inf')
    
    for i, val in enumerate(tasks):
        time = min(time, val[0]) 
        # it's also possible to set it as the starting time of the first item after sorting
        task.append((val[1], i, val[0])) 
        # (processing time, index, starting time), the order and value of the tuple is same as the weight of the task 
        # because "if multiple tasks have the same shortest processing time, it will choose the task with the smallest index", if means if tuple[0] == tuple[0], return tuple[1] < tuple[1]
    task.sort(key=lambda x : x[2])
    
    while len(heap) > 0 or len(task) > 0: 
        while len(task) > 0 and task[0][2] <= time: 
            # push all tasks that satisfied the condition (starting <= current time)
            heappush(heap, task[0])
            task.pop(0)
        if len(heap):
            top = heappop(heap)
            res.append(top[1])
            time += top[0]  
            # add the processing time to current time 
        else:               
            # nothing can be executed, move to the earlist possible starting time for the next task, the while loop condition ensure there will be at least one element in the task array, so no need to check before accessing the first item
            time = max(t, task[0][2])
    return res    
```

**Explanation**:

For this type of problem, I look at the strategy / rules and see if I can twist around them, in this case, they are:

- If the CPU is idle and there are no available tasks to process, the CPU remains idle.
- If the CPU is idle and there are available tasks, the CPU will choose the one with the shortest processing time. If multiple tasks have the same shortest processing time, it will choose the task with the smallest index.
- Once a task is started, the CPU will process the entire task without stopping.
- The CPU can finish a task then start a new one instantly.

There aren't anythings that can be optimized here, the problem is simply describing the policy for determining the order of the ouput array, in some other problems, some of these rules can be combined, or simplfied. 

If nothing can be optimized, then simulation is the way to go. We can have a variable called time (current time), and increments it within a loop to simulate the time going forward, then look into all the tasks that statisfy the condition, and perform it.

The input array contains only starting time and processing time, yet the requirements indicates that **"the CPU will choose the one with the shortest processing time. If multiple tasks have the same shortest processing time, it will choose the task with the smallest index."**, transforming the original input to an array of tuple of (processing time, index, starting time) and sort them by starting time is needed for simulation, because after sorting, the original index of each item will be shuffled, and the CPU can't process the tasks with shorter processing time but starting time are in future, this ensures that when looking into the array, the tasks at the front of the array will have starting time closest to the current time, and some of them will be qualify for selection, they will be pushed into the min heap, python by default compares tuble by all values in the tuple, if 2 tiems have the same procfessing time (the first item in the tuple), it will then compare the second item (index), this ensure the top item of the heap will contain the most qualifying task.

When heap is empty and there are not qualifying tasks from the array, it means the current time is too early and should be advanced to the next cycle: the starting time of the earlist possible executable task, this simulated CPU idling, and fast forwarded the waiting time.

**Complexity**:
- time: O(NlogN): for sorting O(NlogN) + for simulation, O(N) * O(logN) 
- space: O(N): heap is already O(N) for worst case



## 1835. Find XOR Sum of All Pairs Bitwise AND

The XOR sum of a list is the bitwise XOR of all its elements. If the list only contains one element, then its XOR sum will be equal to this element.

For example, the XOR sum of [1,2,3,4] is equal to 1 XOR 2 XOR 3 XOR 4 = 4, and the XOR sum of [3] is equal to 3.
You are given two 0-indexed arrays arr1 and arr2 that consist only of non-negative integers.

Consider the list containing the result of arr1[i] AND arr2[j] (bitwise AND) for every (i, j) pair where 0 <= i < arr1.length and 0 <= j < arr2.length.

Return the XOR sum of the aforementioned list.

**Example 1**:

```
Input: arr1 = [1,2,3], arr2 = [6,5]
Output: 0
Explanation: The list = [1 AND 6, 1 AND 5, 2 AND 6, 2 AND 5, 3 AND 6, 3 AND 5] = [0,1,2,0,2,1].
The XOR sum = 0 XOR 1 XOR 2 XOR 0 XOR 2 XOR 1 = 0.
```

**Example 2**:

```
Input: arr1 = [12], arr2 = [4]
Output: 4
Explanation: The list = [12 AND 4] = [4]. The XOR sum = 4.
```

**Constraints**:
- 1 <= arr1.length, arr2.length <= 105
- 0 <= arr1[i], arr2[j] <= 109

**Solution**:

```python
def getXORSum(self, arr1: List[int], arr2: List[int]) -> int:
    n = 0
    b = 0
    for x in arr2:
        b = b ^ x
    for a in arr1:
        n = n ^ (a & b)
    return n

def getXORSum(self, arr1: List[int], arr2: List[int]) -> int:
    a = 0
    b = 0
    for x in arr2:
        b = b ^ x
    for x in arr1:
        a = a ^ x
    return a & b
```

**Explanation**:

the question is asking us to:
given input:

[a, b, c]
[d, f]

return: 
a & d  ^  a & f  ^  b & d  ^  b & f  ^  c & d  ^  c & f

it's basically:

(a & d  ^  a & f)  ^  (b & d  ^  b & f)  ^  (c & d  ^  c & f)

a & (b ^ f)  ^  b & (b ^ f)  ^  c & (b ^ f)

(a ^ b ^ c)  &  (b ^ f)

same as
a * b + a * c > a * (b + c)  

**Complexity**:
- time: O(N)
- space: O(1)

# 数据结构

## 二分查找
1. 比如：总共有n个元素，**每次查找的区间**大小就是n，n/2，n/4，…，n/2^k（接下来操作元素的剩余个数），其中k就是循环的次数。 
由于n/2^k取整后>=1，即令n/2^k=1， 
可得k=log2n,（是以2为底，n的对数），所以时间复杂度可以表示O(logn)

## 树

1. **度**：节点的孩子数量；**树的度:** 所有节点的度中最大的即为树的度。
2. **节点深度**：节点的深度与节点所在的层统一，节点在第几层，那么该节点的深度就是层数值。
3. **树的深度**：节点具有的最大深度即为树的深度，其实就是层数值。



## 二叉堆 优先队列
* 堆通常是一个可以被看做一棵树的数组对象。堆总是满足下列性质：
> **堆中某个节点的值总是不大于或不小于其父节点的值；**
> 
> **堆总是一棵完全二叉树。**


* 二叉堆是一种特殊的堆，二叉堆是完全二元树（二叉树）或者是近似完全二元树（二叉树）。
* 二叉堆有两种：最大堆和最小堆。
> 最大堆：父结点的键值总是大于或等于任何一个子节点的键值；
> 
> 最小堆：父结点的键值总是小于或等于任何一个子节点的键值。
>
> 对于最大堆，删除根节点就是删除最大值；对于最小堆，是删除最小值


## Map 和 Set

1. 当使用 Map 和 Set 时，如果对时间复杂度要求较高的，应该使用 HashMap 或 HashSet。TreeMap 和 TreeSet 是相对有序的，如果需要相对有序的方式储存，应该用 TreeMap 和 TreeSet
2. 计数情况下适合用 Map。


## 二叉搜索树
1. 二叉搜索树指一颗空树或这具有下列性质的二叉树：

* **左子树**上的所有节点**均小于它的根节点**的值
* **右子树**上的所有节点**均大于它的根节点**的值
* Recursively，左、右子树也分别是二叉搜索树。

**二叉搜索树采用中序遍历后，输出的结果是递增的,因此中序遍历可用于判断二叉树是否为二叉搜索树**

## 递归

* 代码模板

```python
def recursion(level, param1, param2):
	
	# recursion terminator 递归终止条件
	if level > MAX_LEVEL:
		print_result
		return
	
	# process logic in current level  业务逻辑
	process_data(level, data...)
	
	# drill down # 递归至下一层
	self.recursion(level + 1, p1, ...)
	
	# reverse the current level status if needed # 某一层返回后进行的操作，非必须
	reverse_state(level)
	
```

## 分治算法

**核心：子问题拆分** 

* 代码模板

```python
def divide_conquer(problem, param1, param2):

    # recursion terminator 利用递归实现分治，递归终止条件
    if problem is None:
        print_result
        return 

    # prepare data 数据准备，将大问题拆分成小问题
    data = prepare_data(problem)
    subproblems = split_problem(problem, data)

    # conquer subproblems  问题处理 
    subresult1 = self.divide_conquer(subproblems[0], p1, ...)
    subresult2 = self.divide_conquer(subproblems[1], p1, ...)
    subresult3 = self.divide_conquer(subproblems[2], p1, ...)
    ...

    # process and generate the final result 合并各个子问题的结果
    result = process_result(subresult1, subresult2, subresult3)


```

## 贪心算法
**贪心法，又称贪心算法、贪婪算法：在对问题求解时，总是做出在当前看来是最好的选择。**

> **适用 Greedy 的场景**
> 
> 简单的说，问题能够分解成子问题来解决，子问题的最优解能递推到最终问题的最优解。这种子问题最优解成为最优子结构。
> 
> 贪心算法与动态规划的不同在于它对每个子问题的解决方案都做出选择，不能回退。动态规划则会保存以前的运算结果，并根据以前的结果对当前进行选择，有回退功能。


## 广度优先搜索（BFS）
```python
# BFS 代码

def BFS(graph, start, end):

	queue = []
	queue.append(start)
	visited.add(start) # 标记已被访问过的

	while queue:
		node = queue.pop()
		visited.add(node)

		process(node)
		nodes = generate_related_nodes(node) # 判断节点是否已经被访问
		queue.append(nodes)
		
```

## 深度优先搜索 (DFS)
```python
# DFS 代码 递归写法

visited = set()
def DFS(node, visited):
	visited.add(node)
	# process current node here.
	...

	for next_node in node.children():
		if not next_node in visited:
			DFS(next_node, visited)
			
```

```python
# DFS 代码 非递归写法

def DFS(self, tree):
	if tree.root is None:
		return []

	visited, stack = [], [tree.root]

	while stack:
		node = stack.pop()
		visited.add(node)

		process(node)
		nodes = generate_related_nodes(node)
		stack.append(nodes)
		...
		
```

### 动态规划

1. 递推！（递归 + 记忆化）
2. 状态的定义：opt[n],dp[n],fib[n]
3. 状态转移方程:opt[n] = best_of(opt[n-1],opt[n-2])，状态转移方程有可能含有 if else 等判断条件。
4. 最优子结构

### DP vs 回溯 vs 贪心
* 回溯（递归）-- 重复计算
* 贪心 -- 永远局部最优
* DP -- 记录局部最优子结构/多种记录值

### 布隆过滤器 Bloom Filter
> 一个很长的二进制向量和一个映射函数
> 布隆过滤器可以用于检索一个元素是否在一个集合中
> 他的优点是空间效率和查询时间都远远超过一般的算法，缺点是有一定的误识别率和删除困难。

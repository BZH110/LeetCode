### 416.Partition-Equal-Subset-Sum

本题是个NP问题。可以采用DFS的方法来暴力枚举，虽然可以利用各种剪枝优化的手段，但根本的时间复杂度仍然是o(2^N)。早期的时候DFS的版本是可以AC的，但是最近被TLE了。

于是我们可以换一个“答案空间”去切入，那就是用背包问题的想法。DFS的解法其实是寻找在一个N维空间上搜索答案(1,0,0,....,0,1,1)，其中0/1表示该数字我们是否选择。显然这个空间的候选数目的order达到了指数级别。“背包问题”就是改变解答空间，思考如果我们构建任意和为s的subset的话，是否能够实现目标。对于s而言，它的范围是从0到nums.size()*nums[i]=2e4. 这个空间是大大缩小的了。举个例子，假如dp[10]=true表示我们可以选择一部分数字加起来是10，那么我们试图思考，我们能否利用这部分数字再加上一些其他数字，使得总和是20呢？也就是说，我们能否通过dp[10]=true来帮助判断dp[20]=true呢？

这就是01背包问题的基本思想。如果dp的空间大小合理，那么我们就可以来解决之前DFS所无法处理的复杂度。基本的模板如下：
```
for (auto x: nums) // 遍历物品
  for (auto s= 1 to sum/2) // 遍历容量
    if dp'[s-x] = true
      dp[s] = true   // 如果考察x之前，已经能够凑出s-x，那么加上x这个数字就一定能凑出和为x的subset。
``` 

此外还有另外一种dp的写法
```
for (auto x: nums) // 遍历物品
  for (auto s= 1 to sum/2) // 遍历容量
    if dp[s] = true
      dp[s+x] = true   // 如果考察x之前，已经能够凑出s，那么加上x这个数字就一定能凑出和为s+x的subset。
``` 

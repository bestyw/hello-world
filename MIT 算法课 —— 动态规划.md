# 公共最长子序列问题

### **【描述】** 求出a[1 ... m]和b[1 .. n]的最长公共子序列

eg:<br>
a: A B C B D A B <br>
b: B D C A B A  <br>
最长公共子序列有：BDAB、BCAB、BCBA <br>

### 1、穷举：
穷举出a[1 ... m]的所有子序列，看b[1 .. n]中是否有。 <br>
a的子序列有 2^m个 <br>
b中是否有，遍历b，n次循环 <br>
O(n2^m) <br>
### 2、优化：
定义LCS(a,b) 为a，b的最长公共子序列 <br>
|s| 为序列s的长度 <br>
优化为两个问题： <br>
1、LCS(a,b)是唯一的，怎么计算？ <br>
2、扩展1，哪些公共子序列达到了长度 <br>

定义c[i, j] = | LCS(a[1 ... i], b[1 ... j) |，即a的前i项和b的前j项的最长公共子序列的长度 <br>
那么c[i, j] = c[i - 1, j - 1] if ai == bj else max(c[i - 1, j], c[i, j -1]) <br>
	
> 动态规划问题的两个特征：<br>
> 1、最优子结构性质，即问题的一个最优解包括了子问题的最优解 <br>
> 2、重叠子问题，即一个递归的过程，包含少数独立的子问题，被重复计算了多次 <br>

#### 最优子结构性质
eg: ** if ** z = LCS(x, y)，如 z = [1 ... c] 共有 c 个 <br>
	** then ** 任何 z = [1 ... x]  1 <= x < c 为某个 a 的prefix和 b 的prefix的 LCS <br>
** recursive of LCS **
'''
LCS(a, b, i, j):
	if a[i] == b[j]
		then c[i, j] = LCS(a, b, i - 1, j - 1)
		else c[i, j] = max(LCS(a, b, i - 1, j), LCS(a, b, i, j - 1))
	return c[i, j)
'''
worst case : 每一个 a[i] 都不等于 b[j]，通过递归树发现，总共有 2^(m+n)，是很慢的，而且有大量重复的计算。<br>
由此导出第二个性质：重叠子问题<br>

#### 重叠子问题
优化——备忘法：（自顶向下）将中间结果进行保存<br>
'''
LCS(a, b, i, j):
	if c[i, j] = None
		then if a[i] == b[j]
			 then c[i, j] = LCS(a, b, i - 1, j - 1)
			 else c[i, j] = max(LCS(a, b, i - 1, j), LCS(a, b, i, j - 1))
	return c[i, j)
'''
	
#### 自底向上：自底向上的计算DP表格
if a[i] = b[j] : 当前表格的值等于左上角表格的值 + 1 <br>
else : 当前表格的值等于 max（上方的值， 左方的值）	<br>

	

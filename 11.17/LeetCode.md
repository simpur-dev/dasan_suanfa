https://leetcode.cn/problems/minimum-changes-to-make-k-semi-palindromes/
2911. 得到 K 个半回文串的最少修改次数
已解答
困难
相关标签
premium lock icon
相关企业
提示
给你一个字符串 s 和一个整数 k，请将 s 划分为 k 个 非空子串 ，使得将每个子串变为 半回文串 所需的字符修改次数之和最小。

返回所需的 最少 字符修改次数。

半回文串 是一类特殊的字符串：它可以按照某种重复模式拆分后，使每一组都成为 回文串 。判断一个字符串是否为半回文串的方法如下：

选择该字符串长度的一个正因数 d。其中，d 的取值范围是从 1 到严格小于字符串长度的所有正因数。对于长度为 1 的字符串，根据这一定义，它不存在合法的因数，因为唯一的因数就是其长度本身，而这是不允许的。
对于给定的因数 d，将字符串按长度为 d 的重复模式分组。具体来说，第 1 组由位置 1、1 + d、1 + 2d、…… 上的字符组成；第 2 组由位置 2、2 + d、2 + 2d、…… 上的字符组成；以此类推。
如果这些分组中的每一组都是回文串，则该字符串被视为半回文串。
以字符串 "abcabc" 为例：

"abcabc" 的长度为 6。合法的因数有 1、2 和 3。
当 d = 1 时：整个字符串 "abcabc" 构成一组。它不是回文串。
当 d = 2 时：
第 1 组（位置 1, 3, 5）："acb"
第 2 组（位置 2, 4, 6）："bac"
这两组都不是回文串。
当 d = 3 时：
第 1 组（位置 1, 4）："aa"
第 2 组（位置 2, 5）："bb"
第 3 组（位置 3, 6）："cc"
所有分组都是回文串。因此，"abcabc" 是一个半回文串。
 

示例 1：

输入： s = "abcac", k = 2

输出： 1

解释： 将 s 划分为 "ab" 和 "cac"。"cac" 本身已经是半回文串。将 "ab" 改为 "aa" 后，它在 d = 1 时成为半回文串。

示例 2：

输入： s = "abcdef", k = 2

输出： 2

解释： 将其划分为子串 "abc" 和 "def"。这两个子串各自都需要修改 1 个字符才能变成半回文串。

示例 3：

输入： s = "aabbaa", k = 3

输出： 0

解释： 将其划分为子串 "aa"、"bb" 和 "aa"。它们都已经是半回文串。

 

提示：

2 <= s.length <= 200
1 <= k <= s.length / 2
s 仅由小写英文字母组成。

```python
from functools import cache

MX = 201
divisors = [[] for _ in range(MX)]
for i in range(1, MX):
    for j in range(2 * i, MX, i):
        divisors[j].append(i)

def get_modify(t: str) -> int:
    m = len(t)
    res = m
    for d in divisors[m]:
        cnt = 0
        for i0 in range(d):
            i, j = i0, m - d + i0
            while i < j:
                cnt += (t[i] != t[j])
                i += d
                j -= d
        res = min(res, cnt)
    return res
class Solution:
    def minimumChanges(self, s: str, k: int) -> int:
        n = len(s)
        
        modify = [[0] * n for _ in range(n - 1)]
        for left in range(n - 1):
            for right in range(left + 1, n):
                modify[left][right] = get_modify(s[left: right + 1])
        @cache
        #dfs[i][j]表示把s[0...j]这段字符串切分成i+1个半回文串所需的最小修改次数
        def dfs(i: int, j: int) -> int:
            if i == 0:
                return modify[0][j]
            return min(
                dfs(i - 1, L - 1) + modify[L][j]
                for L in range(i * 2, j)
            )
        return dfs(k - 1, n - 1)
```
cache的作用：

```python
from functools import cache

@cache
def square(x):
    print(f"正在计算{x}的平方")
    return x * x

print(square(3))
print(square(3))
print(square(3))

#输出结果显示
正在计算 3 的平方
9
9
9
第2、3次调用直接返回缓存的 9
没有@cache的话，会打印3次：正在计算...
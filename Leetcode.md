

1. python ‘//’ 取整，‘%’ 取余
* 比如，2%10=2.通过%10的操作可以得到个位数


2. 判断子序列-7/27/2020
* 利用string.find可以找到某个特定字符在string中的位置
   * 而且可以限定寻找范围
```python
class Solution:
    def isSubsequence(self, s: str, t: str) -> bool:
        if s == "":
            return True
        index = -1
        for i in s:
            index = t.find(i, index+1)
            if index == -1:
                return False
        return True
```

3. 一维数组的动态和-7/27/2020
  * 先初始化sum会加快用时。
```Python
class Solution:
    def runningSum(self, nums: List[int]) -> List[int]:
        res = []
        sum = 0
        for num in nums:
            sum = sum + num
            res.append(sum)
        return res
```

4. 整数翻转-29/7/2020
    * 负数无法取余
    * 这里题目假设只能存储得下32位的有符号整数，则其数值范围为 [−2<sup>31</sup>,  2<sup>31</sup>−1]
    * 思路一： 利用移位进行运算
        * &：按位与操作，只有 1 &1 为1，其他情况为0。可用于进位运算。
        * |：按位或操作，只有 0|0为0，其他情况为1。
        * ~：逐位取反。
        * ^：异或，相同为0，相异为1。可用于加操作（不包括进位项）。
        * \>>：**右移操作**，2的幂相关
            * A right shift by n bits is defined as division by pow(2,n). 
        * \<<：**左移操作**，2的幂相关
            * A left shift by n bits is defined as multiplication with pow(2,n);
            * 整数以二进制补码的方式存储，**正数右移与负数右移得到的效果并不相同**，负数在右移过程中会自动补 1. 在python中，由于动态语言的特性，数据的位数理想上是不受限制的，因此可通过移位的次数进行判断，int型数据移位不能超过32即可，这样程序的循环次数恒定为32.
                * 在32位运算中，1<<31(1左移31位)是一个32位的大负数，而1<<32结果为0
        * Python的 x<<y 相当于直接调用：**int(x*2**y)函数**
```python
class Solution(object):
    def reverse(self, x):
        """
        :type x: int
        :rtype: int
        """
        rev=0
        max=1<<31 if x<0 else (1<<31)-1
        absx=abs(x)
        while absx!=0:
            pop=absx%10
            absx=absx//10
            if rev>(max//10) or (rev==max//10 and pop>max%10):
                return 0
            rev=rev*10+pop
        return (rev if x>0 else -rev)

```
* 也可以利用字符串
    * 思路二：利用str原数字，判断正负，然后根据正负决定 **[::-1]** 还是 **[:0:-1]**
    * 对于"12345",[:1:-1]是"543"
    * 注意题目限定了**32位**整数，正数部分的上限要变成2<sup>31</sup>-1，而负数的下限则是-2<sup>31</sup>
        * -2147483648 < x < 2147483647 
        * 2<sup>n</sup>有n+1位
 ```python
 def reverse_force(self, x: int) -> int:
        if -10 < x < 10:
            return x
        str_x = str(x)
        if str_x[0] != "-":
            str_x = str_x[::-1]
            x = int(str_x)
        else:
            str_x = str_x[:0:-1]
            x = int(str_x)
            x = -x
        return x if -2147483648 < x < 2147483647 else 0
  ···
* 优化解（boywithacoin_cn）
    * 构建反转整数的一位数字。在这样做的时候，我们可以预先检查向原整数附加另一位数字是否会导致溢出。反转整数的方法可以与反转字符串进行类比。

我们想重复“弹出” x 的最后一位数字，并将它“推入”到 res 的后面。最后，res 将与 x 相反。

优化解：
时间复杂度：O(log(x))，x中大约有log10(x) 位数字。
空间复杂度：O(1)

作者：
链接：https://leetcode-cn.com/problems/reverse-integer/solution/pythondan-chu-he-tui-ru-shu-zi-yi-chu-qian-jin-xin/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。


5. 整数拆分-30/7/2020(ref:jyd)
* 本题等价于求解：**max(n<sub>1</sub>xn<sub>2</sub>x...n<sub>a</sub>)**
* 数学推导总体分为两步：① **当所有拆分出的数字相等时，乘积最大**。② 最优拆分数字为 3
* 根据“算术几何均值不等式” ，等号当且仅当n<sub>1</sub>=n<sub>2</sub>=n<sub>a</sub>时成立

![Alt text](https://encrypted-tbn0.gstatic.com/images?q=tbn%3AANd9GcQzHsJCTwM2yRSWOYV7dwwriLwhb0tbdc1Zog&usqp=CAU "Optional title")
    
* 假设数字n等分为a个，即n=a*x，则乘积为x<sup>a</sup>
    * x<sup>a</sup> = x<sup>n/x</sup>，由于n为固定值，x<sup>n/x</sup> = (x<sup>1/x</sup>)<sup>n</sup>, 因此当 (x<sup>1/x</sup>)最大时乘积最大，通过对该函数求极值，发现x=3时乘积最大，因此只需将数字n尽可能以因子3等分即可
    * 余数为1时，将一个1+3转化成2+2
    
    
```python
class Solution:
    def integerBreak(self, n: int) -> int:
        if n <= 3: 
            return n - 1
        a, b = n // 3, n % 3
        if b == 0: 
            return int(math.pow(3, a))
        if b == 1: 
            return int(math.pow(3, a - 1) * 4)
        return int(math.pow(3, a) * 2)
```

* 思路二：将原问题抽象为 f(n)，那么 f(n) 等价于 max(1\*f(n-1), 2\*f(n-2), ..., (n-1)\*f(1))（ref:fe-lucifer)
    * 转化为代码即：
        * 也就是说，对于任意整数n,划分为n=n-i+i,那么乘积只有两种可能，i*(n-i)或是i*f(n-i)
```python
class Solution:
    def integerBreak(self, n: int) -> int:
        if n == 2: 
            return 1
        res = 0
        for i in range(1, n):
            res = max(res, max(i * self.integerBreak(n - i),i * (n - i)))
        return res
```
* 但以上代码有超时的问题，考虑使用**记忆化递归**的方式来解决。只是用一个**hashtable**存储计算过的值即可。
```pyhon
class Solution:
    @lru_cache()
    def integerBreak(self, n: int) -> int:
        if n == 2: 
            return 1
        res = 0
        for i in range(1, n):
            res = max(res, max(i * self.integerBreak(n - i),i * (n - i)))
        return res
```
* 采用自底向上的思考方式——动态规划

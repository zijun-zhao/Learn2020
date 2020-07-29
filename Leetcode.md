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
        * >>：**右移操作**，2的幂相关
            * A right shift by n bits is defined as division by pow(2,n). 
        * <<：**左移操作**，2的幂相关
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
    * 思路二：利用str原数字，判断正负，然后根据正负决定**[::-1]**还是**[:0:-1]**
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

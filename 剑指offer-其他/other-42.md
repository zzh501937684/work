和为S的两个数字
1.题目
输入一个递增排序的数组和一个数字S，在数组中查找两个数，是的他们的和正好是S，如果有多对数字的和等于S，输出两个数的乘积最小的。

输出描述：

对应每个测试案例，输出两个数，小的先输出。

2.思路
对于一个数组，我们可以定义两个指针，一个从左往右遍历（pleft），另一个从右往左遍历（pright）。首先，
我们比较第一个数字和最后一个数字的和curSum与给定数字sum，如果curSum < sum，那么我们就要加大输入值，
所以，pleft向右移动一位，重复之前的计算；如果curSum > sum，那么我们就要减小输入值，所以，pright向左移动一位，重复之前的计算；
如果相等，那么这两个数字就是我们要找的数字，直接输出即可。

这么做的好处是，也保证了乘积最小。


3.c++
```c++
class Solution {
public:
    vector<int> FindNumbersWithSum(vector<int> array,int sum) {
        vector<int> result;
        int length = array.size();
        if(length < 1){
            return result;
        }
        int pright = length - 1;
        int pleft = 0;
        
        while(pright > pleft){
            int curSum = array[pleft] + array[pright];
            if(curSum == sum){
                result.push_back(array[pleft]);
                result.push_back(array[pright]);
                break;
            }
            else if(curSum < sum){
                pleft++;
            }
            else{
                pright--;
            }
        }
        return result;
    }
};
```

4.python
```python
class Solution:
    def FindNumbersWithSum(self, array, tsum):
        # write code here
        if len(array) <= 1:
            return []
        left, right = 0, len(array)-1
        while left < right:
            if array[left] + array[right] == tsum:
                return array[left], array[right]
            elif array[left] + array[right] < tsum:
                left += 1
            else:
                right -= 1
        return []
```

1.题目
在一个长度为n的数组里的所有数字都在0到n-1的范围内。 数组中某些数字是重复的，但不知道有几个数字是重复的。也不知道每个数字重复几次。
请找出数组中任意一个重复的数字。 例如，如果输入长度为7的数组{2,3,1,0,2,5,3}，那么对应的输出是第一个重复的数字2。

2.思路
还可以把当前序列当成是一个下标和下标对应值是相同的数组（时间复杂度为O(n),空间复杂度为O(1)）； 遍历数组，判断当前位的值和下标是否相等：

若相等，则遍历下一位；
若不等，则将当前位置i上的元素和a[i]位置上的元素比较：若它们相等，则找到了第一个相同的元素；若不等，则将它们两交换。
换完之后a[i]位置上的值和它的下标是对应的，但i位置上的元素和下标并不一定对应；重复2的操作，直到当前位置i的值也为i，将i向后移一位，再重复2。

本文采用思路3，如果还是不懂，看下面的实例分析就懂了！
举例说明：{2,3,1,0,2,5,3}

0(索引值)和2(索引值位置的元素)不相等，并且2(索引值位置的元素)和1(以该索引值位置的元素2为索引值的位置的元素)不相等，
则交换位置，数组变为：{1,3,2,0,2,5,3}；
0(索引值)和1(索引值位置的元素)仍然不相等，并且1(索引值位置的元素)和3(以该索引值位置的元素1为索引值的位置的元素)不相等，
则交换位置，数组变为：{3,1,2,0,2,5,3}；
0(索引值)和3(索引值位置的元素)仍然不相等，并且3(索引值位置的元素)和0(以该索引值位置的元素3为索引值的位置的元素)不相等，
则交换位置，数组变为：{0,1,2,3,2,5,3}；
0(索引值)和0(索引值位置的元素)相等，遍历下一个元素；
1(索引值)和1(索引值位置的元素)相等，遍历下一个元素；
2(索引值)和2(索引值位置的元素)相等，遍历下一个元素；
3(索引值)和3(索引值位置的元素)相等，遍历下一个元素；
4(索引值)和2(索引值位置的元素)不相等，但是2(索引值位置的元素)和2(以该索引值位置的元素2为索引值的位置的元素)相等，则找到了第一个重复的元素。



3.c++
```c++
class Solution {
public:
    // Parameters:
    //        numbers:     an array of integers
    //        length:      the length of array numbers
    //        duplication: (Output) the duplicated number in the array number
    // Return value:       true if the input is valid, and there are some duplications in the array number
    //                     otherwise false
    bool duplicate(int numbers[], int length, int* duplication) {
        // 非法输入
        if(numbers == NULL || length <= 0){
            return false;
        }
        // 非法输入
        for(int i = 0; i < length; i++){
            if(numbers[i] < 0 || numbers[i] > length - 1){
                return false;
            }
        }
        // 遍历查找第一个重复的数
        for(int i = 0; i < length; i++){
            while(numbers[i] != i){
                if(numbers[i] == numbers[numbers[i]]){
                    *duplication = numbers[i];
                    return true;
                }
                swap(numbers[i], numbers[numbers[i]]);
            }
        }
        return false;
    }
};
```

4.python
```python
class Solution:
    # 这里要特别注意~找到任意重复的一个值并赋值到duplication[0]
    # 函数返回True/False
    def duplicate(self, numbers, duplication):
        # write code here
        n = len(numbers)
        if n == 0:
            return False
        for i in range(n):
            if numbers[i] < 0 or numbers[i] > n-1:
                return False
        for i in range(n):
            while numbers[i] != i:
                if numbers[i] == numbers[numbers[i]]:
                    duplication[0] = numbers[i]
                    return True
                numbers[numbers[i]], numbers[i] = numbers[i], numbers[numbers[i]]
        return False
```

4.2  python
个人感觉最优的方法：题目里写了数组里数字的范围保证在0 ~ n-1 之间，所以可以利用现有数组设置标志，当一个数字被访问过后，
可以设置对应位上的数 + n，之后再遇到相同的数时，会发现对应位上的数已经大于等于n了，那么直接返回这个数即可。
```python
class Solution:
    # 这里要特别注意~找到任意重复的一个值并赋值到duplication[0]
    # 函数返回True/False
    def duplicate(self, numbers, duplication):
        # write code here
        n = len(numbers)
        if n == 0:
            return False
        for i in range(n):
            index = numbers[i]
            if index >= n:
                index -= n
            if numbers[index] >= n:
                duplication[0] = index
                return True
            numbers[index] += n
        return False
```

### 二分查找

给定一个 `n` 个元素有序的（升序）整型数组 `nums` 和一个目标值 `target ` ，写一个函数搜索 `nums` 中的 `target`，如果目标值存在返回下标，否则返回 -1。

实例1:

```bash
输入: nums = [-1,0,3,5,9,12],target = 9;
输出: 4
解释: 9出现再了nums下标为4的位置
```

思路: 数组为有序的数组,且无重复元素,则可以认为使用二分法(时间复杂度为: K = log~2~n )
test

代码:

```C++
class Soultion {
    public:
    int search(vector<int>& nums,int target)
    {
        intleft = 0;
        int right = nums.size()-1;//左闭右闭的区间
        int mid = (left + roght) >> 1;
        while(left <= right)
        {
            if(target == nums[mid]) return mid;
            if(target < nums[mid])
            {
                right = mid - 1; //target在左区间
            }
            if(target > nums[mid])
            {
                left = mid + 1; //target在右区间                
            }
        }
        return -1;//未找到目标
    }
};
```

### 移除元素

给你一个数组 `nums `和一个值 `val`，你需要 原地 移除所有数值等于 `val `的元素，并返回移除后数组的新长度。

不要使用额外的数组空间，你必须仅使用 O(1) 额外空间并**原地**修改输入数组。

元素的顺序可以改变。你不需要考虑数组中超出新长度后面的元素。

```bash
示例 1: 给定 nums = [3,2,2,3], val = 3, 函数应该返回新的长度 2, 并且 nums 中的前两个元素均为 2。 你不需要考虑数组中超出新长度后面的元素。

示例 2: 给定 nums = [0,1,2,2,3,0,4,2], val = 2, 函数应该返回新的长度 5, 并且 nums 中的前五个元素为 0, 1, 3, 0, 4。
```

思路: 遍历数组,遇到与目标相同的元素,将他的后面的元素向前移一位,并且刷新数组的大小

**暴力解法** : 双层循环,第一层是遍历数组,第二层是更新数组

```c++
class Solution {
    public:
    int removeElement(vector<int>& nums, int val)
    {
        int size = nums.size();
        for(int i = 0; i < size; i++)
        {
            if(val == nums[i])//查找元素
            {
                for(int j = i + 1; j < size; j++)
                {
                    nums[j-1] = nums[j];//移位
                }
                i--;//更新下标
                size--;
            }
        }
        return size;
    }
};
```

**双指针法 **: 使用两个指针,fast和low,在一个热循环中实现两个for循环的工作

快指针: 寻找新数组的元素,新数组就是不含目标元素的数组

慢指针: 指向更新新数组的下标的位置

```C++
class Solution{
    public:
    int removeElement(vector<int>& nums,int val)
    {
        int size = nums.size();
        int low = 0;
        for(int fast = 0; fast < size; fast++)
        {
            if(nums[fast] != val)//如果快指针的不等于val,则快慢指针同时移动,如果不相同,则快指针移动,慢指针不动
            {
                nums[low++] = nums[fast];//快指针的值覆盖慢指针
            }
        }
        return low;//返回慢指针的下标就是新数组的大小
    }
};
```

### 有序数的平方

给你一个按 非递减顺序 排序的整数数组 `nums`，返回 每个数字的平方 组成的新数组，要求也按 非递减顺序 排序。

```bash
示例 1：

输入：nums = [-4,-1,0,3,10]
输出：[0,1,9,16,100]
解释：平方后，数组变为 [16,1,0,9,100]，排序后，数组变为 [0,1,9,16,100]


```

思路:有序的数组,可能是降序或者升序,但是平方的结果一定是正数,且最大数一定分布在数组的两端,且向中间递减的

**暴力解法** :将数组的每个元素的平方求出,再进行排序

**双指针法** :两个指针分别再数组的两端,求出其对应值的平方,与另一端的值进行比较

```C++
if(nums[low] * nums[low] < nums[right] * nums[right])
result[k--] = nums[right] * nums[right];
else
result[k--] = nums[low] * nums[low];
```

 ### 长度最小的子数组

给定一个含有 n 个正整数的数组和一个正整数 s ，找出该数组中满足其和 ≥ s 的长度最小的 连续 子数组，并返回其长度。如果不存在符合条件的子数组，返回 0。

```bash
示例：

- 输入：s = 7, nums = [2,3,1,2,4,3]
- 输出：2
- 解释：子数组 [4,3] 是该条件下的长度最小的子数组。
```

**暴力双循环**

```c++
class Solution {
    public:
    int minSubArrayLen(vector<int>& nums,int target){
        int ans = INT32_MAX;
        int ret = 0;
        int left = 0;
        for(int right = 0; right <= nums.size(); right++)
            for(left; left < right; left++)
            {
                ret = right - left + 1;
                if(target >= nums[left])
                ret = min(ans,ret)
            }
    }
    return ret == INT32_MAX ? 0 : ret;
}
```

**滑动窗口法**

*注意* :要注意如何定义指针的起始和结束位置,定义一个滑动窗口的结束位置,使用while循环来判断当前的起始位置和当前窗口中的值的总和

```c++
class Solution {
public:
    int minSubArrayLen(int target, vector<int>& nums) {
        int right = 0;//右下标
        int ans = INT32_MAX;
        int ret = 0;
        int sum = 0;//窗口总值
        int left = 0;//左下标
        for(right = 0;right < nums.size(); right++)
        {
            sum += nums[right];//计算窗口值 
            while(sum >= target)//循环判断
            {
                ret = right - left + 1;//计算符合和数量
                ans = ret < ans ? ret : ans;//判断下标值
                sum -= nums[left++];//移动窗口
            }
        }
        return ans == INT_MAX  ? 0 : ans;//返回
    }
};

```

***总结*** : 滑动窗口是一个单循环完成的,其中的想法可以理解为==暴力算法的优化,再此算法中,节省了寻找左节点的过程中,使用了循环比较来节省空间的时间==, ~~并且根据不断变化的窗口中的数字的总和,来判断左移~~.

>滑动窗口大概可以想成，是暴力搜索法的简化。在暴力搜索法中，我们依次尝试所有位置作为start的结果，但是滑动是简化了这个过程。比如刚开始，固定某一个start的，恰恰好找到刚刚>=target的end这个时候，现在我把start+1，我完全没必要再重新开始寻找end，因为我们减少了原本start位置的值，现在的[start+1:end]只是刚刚的子集罢了，最好的情况就是不用变化，这个旧的end也满足target，成为新的end，但是不可能end往前移动了，否则这个新的集合就变成[start+1:end-1]，如果这个满足target，那[start,end-1]也一定满足target，我们刚刚就不会找到这个end，而是留在end-1了，因此start+1的时候，只需要考了现在是否满足target，如果不满足，那end只需要往后移动。

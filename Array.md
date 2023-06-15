# Array

List：27,26,80,277,189,41,299,134,118,119,169,229,274,275,243,244,245,217,219,220,55,45,121,122,123,188,309,11,42,334,128,164,287,135,330,321,327,289,57,56,252,253,239,295,53,325,209,238,152,228,163,88,75,283,376,280,324,278,35



## 27

给你一个数组 nums 和一个值 val，你需要 原地 移除所有数值等于 val 的元素，并返回移除后数组的新长度。

不要使用额外的数组空间，你必须仅使用 O(1) 额外空间并 原地 修改输入数组。

元素的顺序可以改变。你不需要考虑数组中超出新长度后面的元素。

```c++
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int slowindex = 0;
        for (int fastindex = 0; fastindex < nums.size(); fastindex++){
            if(nums[fastindex] != val){
                nums[slowindex++] = nums[fastindex];
            }
        }
        return slowindex;
    }
};
```

思路：用快慢指针，快指针进行是否==val的判断，慢指针用于实际更改和存储数组。



## 26

给你一个 **升序排列** 的数组 `nums` ，请你**[ 原地](http://baike.baidu.com/item/原地算法)** 删除重复出现的元素，使每个元素 **只出现一次** ，返回删除后数组的新长度。元素的 **相对顺序** 应该保持 **一致** 。然后返回 `nums` 中唯一元素的个数。

考虑 `nums` 的唯一元素的数量为 `k` ，你需要做以下事情确保你的题解可以被通过：

- 更改数组 `nums` ，使 `nums` 的前 `k` 个元素包含唯一元素，并按照它们最初在 `nums` 中出现的顺序排列。`nums` 的其余元素与 `nums` 的大小不重要。
- 返回 `k` 。

```c++
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        int slowIndex = 1, len = nums.size();
        if(len==0) return 0;
        for(int fastIndex = 1; fastIndex < len; fastIndex++){
            if(nums[fastIndex] != nums[fastIndex-1]){
                nums[slowIndex++] = nums[fastIndex];
            }
        }
        return slowIndex;
    }
};
```

思路：和27类似，用快慢指针，快指针进行是否和前一项相等的判断，慢指针用于实际更改和存储数组。